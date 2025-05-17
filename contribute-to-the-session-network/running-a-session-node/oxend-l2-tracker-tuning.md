---
description: Learn how to configure options for your Arbitrum RPC provider.
icon: sliders-up
---

# Oxend L2 tracker tuning

One important function that Session Nodes have is to monitor the Arbitrum contracts that define node registrations and rewards.To allow this monitoring, Session Nodes are required to configure an Arbitrum RPC provider.

Not all Arbitrum RPC providers are the same, however.  Different providers have different tiers often something along the lines of (free, paid, pay more, pay even more), and frequently add arbitrary limitations to the free or cheaper tiers that are designed to get you to move to the paid tiers.

The Session Network has been designed so that node operators can opt the free tier of most providers by being more 'relaxed' in its requirements for L2 data. For example, Session appchain blocks include special "L2 event" transactions in response to events on the Arbitrum smart contracts (such as new registrations or unlock requests), but pulse quorums deliberately delay adding these special event transactions into blocks until at least 70 seconds after they first saw them. That slightly delays how long it takes events to be fully confirmed on the appchain, but also allows RPC checks to be reduced to about once a minute without leaving nodes unable to properly confirm these events.

At the same time, providers limit their free tiers in different ways, and there is no single setting that works for all providers. Thus, defaults have been selected to have a reasonable chance of working for most providers' free tiers, as well as many 'knobs' to turn that can be tailored to the individual requirements of a particular provider.

This guide is designed to explain what these options do, how to configure them, and reasonable values for the different options available.

> **Note:** In saying that the Session Network is designed to accommodate the "free tier", this does _not_ mean relying on public Arbitrum RPC URLs. While these may be acceptable for use with wallets, they will often rate limit or not be reliable enough for a Session Node. Instead, be sure to sign up with a trusted RPC provider and get URLs for your  node to use that are tied to your individual account. Not only is this a more reliable approach, it also provides some useful statistics as to your RPC usage.&#x20;

### RPC Calls Used

As different providers have different costs for different endpoints, it can be useful to know which RPC endpoints are used by `oxend` rate limits to figure out how many requests or "credits" a service node will use.

The majority of `oxend` requests are to the `eth_blockNumber` and `eth_getLogs` RPC endpoints: the former is used to query the latest Arbitrum block number, while the latter retrieves any events that have been emitted by the Session contracts since the last successful event fetch. These two calls happen once per minute during normal operation, but several `eth_getLogs` will be issued when oxend restarts to reload any recent events (see the [#l2-max-logs](oxend-l2-tracker-tuning.md#l2-max-logs "mention") section below for details).

There are also some less frequent invocations of `eth_call`: one approximately every 10 minutes to fetch the latest reward pool balance, and another about one an hour used to fetch all contract service nodes to help ensure that the list of nodes on the contract and on the Session appchain are in sync.

If you have configured backup providers then each backup will also have an `eth_blockNumber` call issued approximately every three minutes, used to check that the other nodes are up to date. Backups do not have other RPC requests made _unless_ requests to the Primary provider fails, or the primary provider is too far out of sync with the Arbitrum chain.

Finally, `oxend` issues a `eth_chainId` request at startup to each provider to ensure that all given providers are on the same chain (to prevent accidentally giving a provider URL for the wrong network, such as Ethereum L1, or Arbitrum testnet). These are one-time requests per provider and are not repeated (until `oxend` restarts).

### L2 Options

All of the options discussed below are configured by adding to the `/etc/oxen/oxen.conf` file (or `/etc/oxen/node-NN.conf`, or `/home/USERNAME/.oxen/oxen.conf` depending on how you run oxend), with one option per line, formatted as `option=value`.

> **Note:** If you are running your `oxend` binary directly rather than as a system service then you can also pass any of these on the command line, such as `oxend --option=value`, though this is mainly for testing and developer use and it is generally easier to list them in a persistent config file.

#### `log-level`

This setting does not directly affect the L2 provider functioning, but enabling additional logging around the L2 provider communications can be very useful to see what's happening under the hood while tweaking other options. To enable this logging add a line:

```
log-level=l2_tracker=debug
```

If you want even more verbose information about the individual requests being made, you can also turn on debug logging for the `ethyl` category where the actual HTTP requests are made:

```
log-level=l2_tracker=debug,ethyl=debug
```

Once you are happy with your L2 settings, simply delete (or comment out by adding a `#` at the beginning of the line) to turn disable the debug logging. (Leaving it on won't hurt oxend, but will make it harder to see legitimate errors or warnings from oxend in the future that get lost amid all the debug logging).

#### `l2-provider`

This is the most important option for most service nodes: it specifies the URL for your Arbitrum RPC provider. Generally, you create an account with the provider, get a unique URL for your node, and use that URL with `oxend`.

You can specify this multiple times: the _first_ listed provider will be the "primary" provider that oxend will try to use most of the time to obtain the state of the Arbitrum contracts. Any additional providers you give will be backups: these backups (along with the primary) will be periodically checked to see if the primary provider is falling behind, and if it falls too far behind the backups, one of the backups will be used instead until the primary provider catches up again.

Listing a second URL, ideally from a different provider network, is generally a good idea just in case the first one goes down, becomes unreachable, or runs out of credits. Without a backup your service node would be unable to participate in creating blocks, which will lead to decommissioning or deregistration if it happens for an extended time.

You can list as many backups as you want, but keep in mind that the backups only kick in if the primary fails and so a reliable primary and single reliable backup is probably enough in most cases.

Example:

```
l2-provider=http://10.24.0.1/arb
l2-provider=https://arbitrum-rpc-provider.qwe907v0972351x.example.com/abc?xyz=42
```

#### `l2-refresh`

This setting controls the frequency (in seconds) with which oxend queries the provider for any new contract events since its last successful query.

The default is 60s (unless you are running in "l2-proxy" mode, which we discuss below), and we don't suggest going any higher than this. If you have ample provider request room, you should consider reducing this to 30 or lower: while it uses more requests, in the event of a request failure of both the primary and backup nodes (for instance, due to a momentary internet disconnection) the next request retry will come sooner.

#### `l2-max-logs` <a href="#l2-max-logs" id="l2-max-logs"></a>

`oxend`'s main mechanism of scanning the Arbitrum contracts is to use an `eth_getLogs` call, which queries the provider for all events emitted by the contract in a range of blocks. This option controls how large a range of blocks oxend is allowed to request at once. If `oxend` needs events for a larger range than this value then the request is split up into multiple requests, each requesting logs for only `l2-max-logs` blocks at a time.

The setting defaults to 1000.

For example, if our last successful event retrieval was 9500 blocks ago, and this is set to the default of 1000, we would issue 10 requests for \[9499-8500 ago], \[8499-7500 ago], and so on, until we catch up to the current state.

This limit applies most significantly during startup, when `oxend` fetches contract logs for the last 16800 blocks (about 70 minutes) of the Arbitrum chain to ensure that it always has the last hours worth of events (plus a safety margin). At the default setting of 1000 this means 17 `eth_getLogs` requests are issued when `oxend` restarts, but after that there is generally just one per minute.

If your providers allow larger range then you should increase this to whatever they allow. For example, many entry level paid tiers allow 10000 here, and increasing this to 10000 will allow oxend to make just two requests on restarts instead of 17.

**Lowering this is required for some providers!**

Some providers limit the maximum block range for `eth_getLogs.` For example, Infura and Alchemy free tiers limit this endpoint to requesting 500 blocks at a time, and QuickNode's free tier limits it to just 5 (which is only 1.25s of Arbitrum blocks and makes QuickNode's free tier unusable for Session Nodes).

If your provider does not allow a range of 1000, you _must_ configure this setting to lower it to what your provider supports; otherwise `oxend` will be unable to fetch any logs at all, and will be penalized for missing pulse quorums.

Take note that Arbitrum has a 250ms block time (i.e. 4 blocks per second), and so during the normal default 60s update interval there will typically be 240 new blocks that `oxend` needs to fetch logs for. If your provider requires you to drop the `l2-max-logs` value below about 250, then you will end up having to make multiple requests every minute during normal operation just to keep up, which will likely make you run out of credits sooner. It's best to stick to providers that allow a range of at least 500 blocks for the `eth_getLogs` endpoint.

#### `l2-update-cooldown`

This setting is related to `l2-max-logs`, above: it defines a cooldown period between subsequent `eth_getLogs` request and can be used to avoid request (or credit) per second rate limits imposed by some providers, particularly during the initial burst during startup (see above).

For example:

`l2-update-cooldown=2`

will force `oxend` to delay the next `eth_getLogs` request if it has been less than 2 seconds since the last request.

This value defaults to disabled: that is, if oxend needs to send multiple getLogs requests, it will send the next one as soon as it gets the response to the previous one, one after another until it is finished.

Some providers may need this to be set. For example, one provider used by one of Session's developers assigns a credit value of 255 credits per `eth_getLogs` request, and starts returning errors for all requests if more than 500 credits worth of requests per second are issued. If your provider has such limits, then consider setting this to 1 or 2 to add a small delay between requests to keep yourself below the limit.

#### `l2-timeout`

This controls how long we wait for a response from the L2 provider before giving up. When you have backup providers, a request timeout on the primary provider will trigger the same request to a backup provider.

The default is 5 seconds: if your provider is sometimes slow to respond you might want to increase this, and if you know your provider is very quick and low latency, you might want to lower it to try a backup sooner.

#### `l2-check-interval`

This value is the interval for "all-provider" RPC height checks: when `oxend` initiates a chain update, if it has been at least this many seconds since the last height check, `oxend` will request the current chain height from all providers (primary and all backups). If the backup is too many blocks behind the backups, `oxend` will then switch to the first backup that isn't too far behind and will make all requests to that backup provider until a future all-provider check indicates that the primary node has caught up again.

The default is 170s, but note that this is only triggered on the regular refresh cycle (`l2-refresh`), and so this rounds up to the next refresh cycle (and so, with a default 60s refresh cycle, that means this happen every 3 minutes, not 2min50s).

This option has no effect if there are no configured backup providers.

#### `l2-check-threshold`

When doing all-provider RPC height checks (see the previous section) this setting controls how many blocks behind the other nodes the primary has to be before we switch to a backup. It is normal to have a few blocks variation given Arbitrum's very fast 0.25s block time, and so this allows setting how far behind the primary has to be before oxend starts using a backup.

The default is 120 blocks, which is about 30 seconds worth of Arbitrum blocks, but you can lower this if you want to switch over to a backup sooner if the primary starts lagging.

`oxend` will issue warnings (even without changing the log level) when a provider starts lagging more than this threshold, providing you some indicating of when this setting (and switching between primary and backup) is taking place. If you see such log warnings frequently, then you might want to consider switching to a different primary RPC provider.

#### `l2-skip-chainid`

This setting can be set as `l2-skip-chainid=1` if you want to skip `oxend`'s builtin check during startup to make sure that your providers are on the correct chain. This will make oxend start slightly faster, and save you one request per provider on each restart, but disables the safety check against accidentally specifying a URL to the wrong chain.

### Oxend L2 proxy

For operators running multiple service nodes you can configure one or more of your oxend's as proxies, and then point all the rest of the nodes to the proxies.

Only the proxy nodes make L2 provider requests, and every other node gets its L2 updates from the proxies as soon as each proxy gets new L2 data.

#### `l2-proxy`

#### `l2-oxend`

These are the two main options controlling proxy mode: `l2-proxy` enables a node to operate as a proxy, making requests to the L2 providers, and providing those results to other `oxend`s. `l2-oxend` configures a node to _use_ a proxy rather than an `l2-provider` as its source of L2 information.&#x20;

Some notes about all of the above controls on this page:

* For a proxy-using node, the `l2-provider=...` option must _not_ be listed (or must be commented out with a leading `#`) in the config file. oxend will refuse to start if both `l2-oxend` and `l2-provider` options are used at the same time. All the other `l2-...` options listed here can still be listed in the config file, but will be ignored when using an oxend proxy.
* For the proxy node itself (i.e. the one making the L2 requests), all of the options described on this page apply and control how the proxy gets its information, with one difference: the `l2-refresh` setting on a node configured as proxy defaults to 30s instead of 60s, to make the default proxy a little more redundant in its attempts to fetch updates.
* Proxy-using nodes do not have a refresh interval in the same way as L2 provider using nodes: each proxy they are connected to will push L2 updates to any connected nodes as soon as they are retrieved, and so effectively the proxy's refresh interval becomes the update interval of all nodes using that proxy.

See [setting-up-an-oxend-l2-proxy.md](setting-up-an-oxend-l2-proxy.md "mention") for a full guide!&#x20;
