# Staking Reward Pool

The Staking Reward Pool is a vital component of the Session ecosystem’s incentivization layer, transforming token utility into rewards for the Session Network.&#x20;

Session Tokens are added to the Staking Reward Pool when Session Tokens are used for advanced features, such as registering a Session Name or unlocking Session Pro, users burn Session Network Fees. These fees are then re-minted into the Staking Reward Pool, increasing network rewards

Tokens in the pool are time-locked in a smart contract and released at a targeted rate of 14% per year. These tokens are rewarded to active and registered nodes in the Session Network. These rewards scale based on the amount of tokens in the Staking Reward Pool, so the network's rewards can increase the more Session Token is used (through actions like Session Pro unlocks and SNS registrations).

### Network reward rate

Rewards are based on a targeted yearly emission of 14% of the Staking Reward Pool, recalculated every 24 hours (to update for fluctuations in the pool size).

To ensure that 14% of the Staking Reward Pool is rewarded to the network in a given year, the percentage used to calculate the daily reward is slightly greater than 14%.

To calculate the daily network reward:

$$
\dfrac{0.151(Staking\:Reward\:Pool)}{365}=Daily\:Network\:Reward
$$

Furthermore, to calculate the reward for an individual node, simply divide the Daily Network Reward by the number of active and registered nodes in the Session Network.

$$
\dfrac{Daily\:Network\:Reward}{Nodes\:in\:network}=Node\:Reward
$$

Moreover, if you are a contributor to a node, your individual reward can be calculated by multiplying the node reward by your percentage of the full stake (less operator fees).

Note that the Staking Reward Pool is dynamic, and it is not possible to effectively predict or account for future tokens added to the pool.

For example, if a large amount of Session Tokens were added to the Staking Reward Pool at once, it would make the emission from the previous year period significantly lower than 14%, even though actual rewards would be greater than forecast.

### Claiming rewards

To avoid unnecessary transaction fees, rewards are held by the smart contract until they are manually claimed by stakers. Rewards can be claimed once the Session Network validates and authorizes the claim and amount.

### Genesis provision

The Staking Reward Pool will receive a minimum genesis provision of 40,000,000 Session Tokens. Using the above formula, the network reward for Day 1 can be calculated:

$$
\dfrac{0.151(40,000,000)}{365}=16547.95
$$

This amount will be equally divided by the amount of active and registered nodes on Day 1.&#x20;

It is possible additional tokens may be committed to the Staking Reward Pool between now and the Token Generation Event (TGE).

