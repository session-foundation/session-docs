# 🏊 Staking Reward Pool

The Staking Reward Pool is one of the most critical elements of Session Token's tokenomics — it is how Session Token's utility is transferred into incentives for Session Node operators and contributors.&#x20;

Session Tokens are added to the Staking Reward Pool when used for activities like **registering** a Session Name or **subscribing** to Session Pro.

Tokens in the pool are time-locked in a smart contract and released at a targeted rate of 14% per year.

The tokens released are awarded to active and registered nodes in the Session Network.&#x20;

These rewards scale based on the amount of tokens in the Staking Reward Pool — so the network's rewards can increase the more Session Token is used (through actions like Session Pro subscriptions and SNS registrations).

### Network reward rate

Although rewards are based on a yearly emission of 14% of the Staking Reward Pool, yearly rewards would be untimely and ineffective.

For this reason, the reward rate is recalculated every 24 hours to determine an applicable network reward for a given day.&#x20;

To ensure that 14% of the Staking Reward Pool is rewarded to the network in a given year, the percentage used to calculate the daily reward is slightly _greater_ than 14%.&#x20;

To calculate the daily network reward:&#x20;

$$
\dfrac{0.151(Staking\:Reward\:Pool)}{365}=Daily\:Network\:Reward
$$

Furthermore, to calculate the reward for an individual node, simply divide the Daily Network Reward by the number of active and registered nodes in the Session Network.&#x20;

$$
\dfrac{Daily\:Network\:Reward}{Nodes\:in\:network}=Node\:Reward
$$

Moreover, if you are a contributor to a node, your individual reward can be calculated by multiplying the node reward by your percentage of the full stake (less operator fees).

Note that the Staking Reward Pool is dynamic, and it is not possible to effectively predict or account for future tokens added to the pool.

For example, if a large amount of Session Tokens were added to the Staking Reward Pool at once, it would make the emission from the previous year period significantly lower than 14%.

### Claiming rewards

To avoid unnecessary transaction fees, rewards are held by the smart contract until they are manually claimed by stakers.&#x20;

Rewards can be claimed once the Session Network validates and authorises the claim and amount.&#x20;

### Genesis provision

The Staking Reward Pool will receive a minimum genesis provision of 40,000,000 Session Tokens. Using the above formula, the network reward for Day 1 can be calculated:

$$
\dfrac{0.151(40,000,000)}{365}=16547.95
$$

This amount will be equally divided by the amount of active and registered nodes on Day 1.

It is possible additional tokens may be committed to the Staking Reward Pool between now and the Token Generation Event (TGE).&#x20;

