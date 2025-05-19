---
description: >-
  This guide will walk you through the complete process of setting up, staking,
  and running a Session Node.
---

# Running a Session Node

## Running a Session Node

You can run Session Node software on any device running a supported operating system, but for the purposes of this guide, we'll assume you will be setting up a Session Node on a remote Ubuntu or Debian server. If you're new to Linux or running servers in general, this is the most straightforward approach. If you're more experienced and would prefer to run your Session Node on a different operating system, you'll need to modify the syntax of some commands to suit your system of choice.

#### **Requirements**

These are the current basic requirements for running a Session Node.  Note that these are generally much less than required for a mainnet node!

<table><thead><tr><th width="242">Spec</th><th>Requirement</th></tr></thead><tbody><tr><td>Latest Session Node software</td><td>Latest Session Node <code>.deb</code> packages (installed via the steps below) or latest <code>stable</code> branch build from source.</td></tr><tr><td>Server operating system</td><td>Ubuntu 22.04+ (latest LTS recommended) or Debian 11+ (latest stable recommended)</td></tr><tr><td>Storage</td><td>45GB</td></tr><tr><td>RAM</td><td>4-8GB</td></tr><tr><td>Connectivity</td><td>100Mb or faster</td></tr><tr><td>Traffic</td><td>1 TB per month or more</td></tr><tr><td>Power</td><td>Redundant with remote cycling ability, as found in most data centres</td></tr></tbody></table>

> **Note:** It is possible for an experienced system administrator to run a Service Node on a server running an operating system other than Ubuntu or Debian. However, this requires additional work to start up and manage the required services, and is beyond the scope of this guide.

#### Session Nodes in a nutshell

* A Session Node starts as a full node on the Arbitrum One blockchain
* The full node becomes a Session Node when the owner locks the required amount of 25,000 SESH and submits a registration transaction
* Once accepted by the network, the Session Node starts performing node operations and becomes eligible to earn rewards in the form of SESH
* Multiple participants can stake into one Session Node and can have the reward automatically distributed among them

**Session Node functionality**

Session Nodes:

* Monitor the Arbitrum One network for new registrations and departures.
* Provide signatures required to withdraw rewards via Arbitrum
* Monitor other Session Nodes and vote on their performance
* Produce new blocks for the network via [Pulse PoS](https://docs.oxen.io/oxen-docs/about-the-oxen-blockchain/pulse-pos-on-oxen)
* Receive, store, and forward encrypted Session messages
* Route [Lokinet](https://lokinet.org/) traffic

## Session Node setup for new users

### **Step 1: Obtaining a server**

Choosing where to set up Session Node is the first and most critical decision you will face in setting up and running your node. There are a number of factors to consider. Because you will be locking up SESH as part of operating your Session Node, you will want to ensure, at a minimum, that your server meets the technical requirements given above.

Your aim is to provide a stable, reliable server with good network connectivity, so that data can be efficiently routed to and from your node. An underpowered or poorly connected node will have a poor response time and add latency to the network for all users whose traffic passes through it, resulting in a less than optimal experience.

If your server goes down while staked, your Session Node could be [deregistered from the network](../../session-network/session-nodes/deregistration.md) and your SESH locked for 30 days (without receiving rewards).

The simplest and cheapest way to host a server such as a Session Node is to lease a Virtual Private Server (VPS). There are literally hundreds of options when it comes to VPS providers, but some of the more commonly chosen companies and products are listed below.

| Hosting Provider | Product Name      | Cost Per Month ($USD) |
| ---------------- | ----------------- | --------------------- |
| Netcup           | VPS 1000 G8       | 10.50                 |
| Evolution Host   | STARTER           | 5.50                  |
| Online.net       | Start-2-S-SSD     | 13.99                 |
| Scaleway         | START1-M          | 9.33                  |
| OVH              | VPS SSD 2         | 7.61                  |
| Leaseweb         | Virtual Server XL | 34.45                 |
| Digital Ocean    | 4 GB, 2 vCPUs     | 24                    |
| Linode           | 4 GB, 2 vCPUs     | 20                    |
| Feral Hosting    | Neon Capability   | 19.68                 |
| Trabia           | VDS-8G            | 38.54                 |
| Hetzner          | EX41-SSD (30 TB)  | 39.71                 |

> **Note:** Session does not endorse any of these providers. This list is merely a selection of some of the popular options at the time of writing. Of course, this popularity comes at the expense of decentralisation. A useful resource in choosing a less common VPS provider is [ExoticVM](https://www.exoticvm.com/). Another good one is [Server Hunter](https://www.serverhunter.com/).

In any case, do not just settle on the first provider you encounter. No two are alike. Do your own research and choose a provider that seems professional, reputable and fits your budget.

The better ones will utilise [KVM](https://www.linux-kvm.org/page/Main_Page) [virtualisation technology](https://www.tradingfxvps.com/kvm-vs-vmware-vs-openvz-vs-xen/). In particular, you should steer clear of any VPS which uses OpenVZ. This is an incomplete form of virtualisation that allows VPS capacity to be oversold, and is usually incompatible with the full Session Node software. Virtual machines created with it often lack a `/dev/tun` device, which effectively prevent it from providing the Lokinet service required for a Session Node.

A good VPS provider will also allow you to monitor your machine's resource consumption, seamlessly upgrade to a more powerful server at a later date, remotely reboot the host if it becomes unresponsive, and even recover or rebuild the system using out-of-band access if, for example, a bad configuration change results in lost network access.

When selecting your VPS’ operating system, please choose the latest Ubuntu LTS release or latest Debian stable release (currently 24.04 and 12, respectively) if you want to be able to follow the steps below verbatim.&#x20;

### Step 2: Preparing your server

Every provider has a slightly different way of issuing you access to your new VPS. Most will send an email with the IP address, root username, and a root password to the VPS.

To access your server, you will need an SSH client for your operating system. Because this guide will use Windows to illustrate the setup process, we’ll download [PuTTY](https://www.putty.org/). macOS and Linux users can connect by opening a terminal and typing:

```
ssh root@[your VPS IP address]
```

To connect to your VPS, you'll need to paste the provided IP address into the SSH client’s “Host Name (or IP address)” input box and click the “Open” button. The Port number can usually just be left as `22`.

A terminal window will now appear, prompting you for your log-in details, username (`root`) and password, as provided by your VPS provider. When entering your password, characters will not appear in the terminal. This is normal. Hit enter after typing or pasting your password, and you should be logged in to your VPS.

_Note: After logging in for the first time, the VPS may prompt you for a new password for the root account. The terminal will require you to enter the new password twice before you can start running commands. If you aren't prompted for a new root password but want to change it anyway, type sudo passwd. Choose something very secure!_

#### 2.1: Hot tips for using the console on Windows

Consoles don't quite work like the rest of your computer. Here are some basic tips for navigating your way around the command line!

* Don't try copying something by using the usual `Ctrl + C` hotkey! If you want to copy something, do so by highlighting text and then right clicking it and selecting Copy. Pasting works by right clicking a blank area in the console and selecting Paste.
* If you want to kill a process or stop something from running, press `Ctrl + C`. (This is why you shouldn't try copying something with this hotkey!)
* You can always check the directory you are in by typing `pwd`, and you can list its contents by typing `ls`.
* You can always return to your home directory by typing `cd` and pressing Enter.
* You can move into a given directory by typing `cd <name>` or move back up one level by typing `cd ..`.
* PuTTY allows you to easily duplicate or restart a session by right clicking the top of the window. Handy if you’re trying to do a few things at once.

#### 2.2: Server preparation continued

Next, update your package lists (the lists that tell your server which software is available for install or upgrade). The following command downloads package lists from their respective package repositories and "updates" them to get information on the newest versions of packages and their dependencies. It will do this for all repositories and PPAs.

```
sudo apt update
```

You'll notice a bunch of package lists were downloaded. Once this is complete run the below command to fetch new versions of any packages that came preinstalled on the system.

```
sudo apt upgrade
```

You'll be prompted to authorise the use of disk space. Type `y` and Enter to authorise.

If you are prompted during the upgrade that a new version of any file is available then click the up and down arrows until you are hovering over `install the package maintainer’s version` and click Enter.

Alright, good to go. Our server is now set up, up to date, and is not running as root. On to the fun part!

#### 2.3: Firewall configuration

If you are using a firewall then ensure that the following ports are open/reachable

* Port 11022 (blockchain syncing)
* Port 11025 (Session Node to Session Node)

### Step 3: Initial repository setup

You only need to do this step the first time you want to set up the Oxen repository; when you've done it once, the repository will automatically update whenever you fetch new system updates.

To add the `apt` repository, run the following commands.

This first command installs the public key used to sign the Session Node packages:

```
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
```

The second command tells `apt` where to find the packages.&#x20;

{% code overflow="wrap" %}
```
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
```
{% endcode %}

If your distribution does not have `lsb_release` available, you may substitute `<DISTRO>` in the following command with the appropriate value to match your operating system. If your VPS is running Ubuntu 24.04 as recommended in this guide, replace `<DISTRO>` with `noble`.

{% code overflow="wrap" %}
```
echo "deb https://deb.oxen.io <DISTRO> main" | sudo tee /etc/apt/sources.list.d/oxen.list
```
{% endcode %}

Other supported distributions include:

* bookworm (Debian 12)
* bullseye (Debian 11)
* noble (Ubuntu 24.04)
* jammy (Ubuntu 22.04)

There are also repositories for Debian testing (`trixie` or `testing`) and unstable (`sid` or `unstable`), and the latest or upcoming Ubuntu non-LTS release is typically supported. Note, however, that none of these distribution versions are recommended for production Session Nodes.

Then resync your package repositories with:

```
sudo apt update
```

### Step 4: Getting an Arbitrum One RPC provider account

You will need to set up an Arbitrum One RPC provider for your `oxend` to interact with the Arbitrum network. This setup will allow your Session node to communicate with the Arbitrum One blockchain and to witness and facilitate transactions.&#x20;

You can use public RPC providers like Infura and Alchemy or set up your own Arbitrum full node and connect to that node locally.&#x20;

While Arbitrum has several RPC providers available, getting familiar with these providers will be useful for mainnet where a reliable RPC connection is required for the stability of your node.&#x20;

* Arbitrum’s recommendations can be found [here](https://docs.arbitrum.io/build-decentralized-apps/reference/node-providers#arbitrum-public-rpc-endpoints)
* [Infura](https://www.infura.io/): Learn how to get setup with a Arbitrum One node through Infura [here](https://docs.infura.io/api/getting-started).
* [Alchemy](https://www.alchemy.com/): Learn how to get setup with an Arbitrum One node through Alchemy [here](https://docs.alchemy.com/docs/alchemy-quickstart-guide).

If you plan to run more than two nodes, consider either setting up a paid account with an RPC provider, or set up one node as an L2 proxy to serve your other node/s.\
\
For example, if you were using Alchemy as an RPC provider your URL would look something like this:

```
https://arb-mainnet.g.alchemy.com/v2/32bfi3gb298fbb32byfb32bf
```

If you were using [dRPC](https://drpc.org/chainlist/arbitrum) as your provider, your URL would look something like this:&#x20;

```
https://lb.drpc.org/ogrpc?network=arbitrum&dkey=32bfi3gb298fbb3-2byfb32bf
```

_Note: The RPC URLs  here use mock API keys_

Find your RPC URL and copy it for use in the next step.&#x20;

### Step 5: Session Node installation and operation

To install the software needed to run a Session Node, simply install the `session-service-node` package:

```
sudo apt install session-service-node
```

This will detect your public IP (or allow you to enter it yourself), ask for your Arbitrum One L2 provider URL, and create the `/etc/oxen/oxen.conf` configuration file with the necessary additional settings to run a Session Node.

#### 5.1: Interacting with the running `oxend`

If you run the `oxend` command with an appended command (note that `sudo` is not required!), the `oxend` command forwards this instruction to the running `oxend`. So, for example, to get the current `oxend` status you can run you would run:

```
oxend status

oxend print_sn_status
```

To see the output log of your node you can run the following command:

```
journalctl -u oxen-node -af
```

This is useful to see if your node is syncing with the blockchain and to see other diagnostic messages that may come up from time to time. (Press `Ctrl-C` to stop watching the log).

For a full list of supported commands run:

```
oxend help
```

You can also get basic statistics (such as uptime proof and ping times) on the running daemon from the `systemctl status` commands:

```
systemctl status oxen-node
```

### Step 6: Session Node Registration

#### 6.1: Retrieving your wallet address

You'll need your Ethereum wallet address to register your Session Node. Navigate to your Ethereum wallet and copy your wallet address.&#x20;

#### 6.2a: Individual Staking

To run a Session Node as the sole contributor, you'll need:

* A fully synchronized, up-to-date Oxen daemon running on your Session Node
* An Ethereum wallet with at least 25,000 SESH in it (to meet the staking requirement to register your Session Node), and sufficient ETH on the Arbitrum network for gas.&#x20;

#### 6.2b: Multicontributor Staking

To run a multicontributor Session Node as the operator, you'll need:

* A fully synchronized, up-to-date Oxen daemon running on your Session Node
* An Ethereum wallet with at least 6,250 SESH in it (to meet the operator staking requirement to register your Session Node), and sufficient ETH on the Arbitrum One Network for gas.&#x20;

#### 6.3: Preparing your node for registration

Log in (if not already logged in) to the VPS running the Session Node, then run the following command:

```
oxend register [your ETH address]
```

The daemon will output something which looks similar to:

{% code overflow="wrap" %}
```
Submitting operator-only information to stake.getsession.org, please wait.
 
Submitted operator-only information to the staking website successfully!

View your registration at: https://stake.getsession.org/register/[Session Node ID]
```
{% endcode %}

_NOTE: This information will be automatically submitted to the Staking Portal to help with creating the transaction on the Arbitrum One blockchain._

#### 6.4a: Registering your single contributor Session Node

To register and stake your Session Node, ensure your Etherem wallet has a balance of at least 25,000 test SESH as well as sufficient ETH for gas.

Navigate to the [Staking Portal](https://stake.getsession.org/) and connect your wallet. On the **Register** page, the node you have prepared registration for will appear in the 'Your Prepared Registrations' list.\
\
View the prepared node’s details and confirm your registration and stake of 25,000 test SESH.

#### 6.4a: Registering your multicontributor Session Node

To register and stake your Session Node, ensure your Ethereum wallet has a balance of at least 6,250 test SESH as well as sufficient ETH on the Arbitrum network for gas.

Navigate to the [Staking Portal](https://stake.getsession.org/) and connect your wallet. On the **Register** page, the node you have prepared registration for will appear in the 'Your Prepared Registrations' list.

When you view the prepared node's details you can customise your stake amount and operator fee. Change these to whatever you wish, keeping in mind that the minimum Stake Amount for the operator is 6,250 SESH. Once you have confirmed these values, you can proceed to register and stake your node.

You node will be listed on the [Staking Portal](https://stake.getsession.org/) as an Available Node, and anyone can stake to it from there. Once your node has reached full 25,000 SESH stake amount, it will automatically be registered on the network.

### Step 7: Session Node status check

After you've staked to your Session Node, you can check that Session Node is running, recognised, and eligible to earn SESH rewards on the [My Stakes](https://stake.getsession.org/mystakes) page.

## Operating your node

### Keeping your binaries up to date

When a new release is available, upgrading is as simple as syncing with the repository:

```
sudo apt update
```

Then installing updates using:

```
sudo apt upgrade
```

_Note that this will install both updated_ `oxend` _packages and any available system updates (this is generally a good thing!)_

During the upgrade, all instances of `oxend` will be restarted if they are currently running in order to switch to the updated `oxend`.&#x20;

If for some reason you want to install only Oxen package upgrades but not other system package updates, then instead of the `sudo apt upgrade` you can use:

```
sudo apt install session-node
```

### Monitoring

Use the [My Stakes](https://stake.getsession.org/mystakes) page to monitor the status of your staked node.&#x20;

### Back-ups

You should immediately make a backup of your Session Node's secret keys. This will allow you to migrate your node to a different hardware provider if necessary in the future.

<mark style="color:red;">**IMPORTANT: These keys should always remain secret and should never be shared with anyone. Sharing these keys can result in the loss of funds or deregistration of your node.**</mark>

The command to reveal the ed25519 secret keys is:

```
oxen-sn-keys show /var/lib/oxen/key_ed25519
```

The command to reveal your BLS secret keys is:

```
oxen-sn-keys show /var/lib/oxen/key_bls
```

Alternatively, you can use a tool like _scp_ to copy these files off-host for safekeeping.

### Restoration

If you backed up your keys and want to restore an unregistered node to use those backed up keys you can use the following commands.

The command to restore an ed25519 key into a file is:

```
oxen-sn-keys restore /var/lib/oxen/key_ed25519
```

The command to restore BLS key into a file is:&#x20;

```
oxen-sn-keys restore-bls /var/lib/oxen/key_bls
```

Those commands will create a new key file with the correct formatting called “key\_ed25519” and “key\_bls” respectively, if you want to overwrite an existing key file you can pass  the “-- overwrite” flag as such:&#x20;

```
oxen-sn-keys restore --overwrite /var/lib/oxen/key_ed25519
```

For BLS keys:&#x20;

```
oxen-sn-keys restore-bls --overwrite /var/lib/oxen/key_bls
```

You can choose either to overwrite your existing key files in the /var/lib/oxen directory using this command or create new key files and swap them out with the existing files, once keys are overwritten or swapped your node can be restarted with the following command:&#x20;

```
systemctl restart oxen-session-node
```

<mark style="color:red;">IMPORTANT: Never remove or replace keys on an active, registered Session Node!</mark>

### Updating L2 Providers and additional node configuration

You can reconfigure your Session by modifying the file at `/etc/oxen.conf` where settings are kept for the current running instance.&#x20;

In `/etc/oxen.conf` each line denotes a configurable option. \
\
For example, in the following, the Session node is configured to use `http://example.com` as the primary L2 provider and `http://backup.example.com` as a backup if the first provider falls behind.

```
data-dir=/var/lib/oxen
log-file=/var/log/oxen.log
service-node=1
stagenet=1
service-node-public-ip=<your node's IP address>
l2-provider=http://example.com
l2-provider=http://backup.example.com
```

Some additional options are available for advanced users to configure how the Session node talks to the L2 provider:

* `l2-refresh` Specify the time (in seconds) between refreshes of the Ethereum L2 provider current state (default is 60)
* `l2-timeout` Specify the timeout (in seconds) for requests to the L2 provider current state; if multiple providers are configured then after a timeout the next provider will be tried (default is 5)
* `l2-max-logs` Specify the maximum number of logs we will request at once in a single request to the L2 provider. If more logs are needed than this at once then multiple requests will be used (default is 1000).
* `l2-check-interval` When multiple L2 providers are specified, this specifies how often (in seconds) all of them should be checked to see if they are synced and, if not, switch to a backup provider. Earlier L2 providers will be preferred when all providers are reasonably close (default is 170)
* `l2-check-threshold` When multiple L2 providers are specified, this is the threshold (in number of blocks) behind the best provider height before a given provider is considered out of sync (default is 120).

An exhaustive list of available options can be found by running `oxend --help`.

After making your changes, you must restart your node for the new settings to apply. Use the following command:

```
systemctl restart oxen-session-node
```

### Unlocking your stake

Session Nodes will continually earn test SESH rewards indefinitely until an exit is requested or the node becomes deregistered. To request an exit to reclaim your test SESH stake, simply open the [Staking Portal](https://stake.getsession.org/) and navigate to the [My Stakes](https://stake.getsession.org/mystakes) page. You can then click Request Exit for any stake you wish to initiate an unlock for.

Your Session Node will become eligible to exit 15 days after the initial request.

When a Session Node has become eligible to exit after 15 days, the node must formally exit the network within 7 days of becoming eligible to exit. Simply click the Exit button on the node in the Staking Portal. After exiting, you can claim your stake by clicking the Claim button on your My Stakes page.

If the node is not removed within 7 days becoming eligible to exit (22 days after the initial exit request), the node becomes eligible for liquidation by other users. When a node gets liquidated, a 0.2% penalty is taken from the operator's stake: 0.1% of the operator’s stake is transferred to the liquidator, and 0.1% of the operator’s stake is returned to the Staking Reward Pool.\


### Deregistrations

Deregistrations can be issued at any point during the active lifecycle of a Session Node, including during the period after requesting an exit.

Deregistration removes your Session Node from the network, and your stake(s) become locked and unspendable for 30 days on mainnet from the block in which the Session Node was deregistered. After this period, operator and contributors can retrieve their stakes by clicking the Claim button in the Staking Portal.

Receiving a deregistration **after** the node's participant(s) have already submitted an exit request overrides the 15 day stake unlock time, and sets the unlock time to 30 days on mainnet.

To avoid losing 0.2% of their stake to the liquidation penalty, operators can manually exit their node by clicking the Exit button in the Staking Portal. The stake will still remain locked for 30 days. If the node has not been manually exited within 7 days following deregistration, it is eligible for liquidation.

### Conclusion

Well done! Your Session Node is configured, operational, and will now begin receiving ESH rewards.

Having trouble? Head to the [Session Token Discord](https://discord.gg/sessiontoken) to access support.&#x20;
