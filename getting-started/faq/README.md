---
description: This page answers common questions about Babylon.
---

# FAQ

1. [General Questions](./#general)
2. [$BABL Tokenomics](./#usdbabl-tokenomics)
3. [Mining Program](./#mining-program)
4. [Gardens](./#gardens)
5. [Deposits & Withdrawals](./#deposits-and-withdrawals)
6. [Strategies](./#strategies)
7. [Voting](./#voting)

## General

### What is Babylon.finance?

Babylon is a community-led asset management protocol that enables users to invest in DeFi together. It's built on the Ethereum network and it's non-custodial, transparent, permissionless, and governed by the community.

### When is Babylon.finance going to launch?

The alpha with team and investors started in May, 2021.

Settlers Beta started in July, 2021 and will continue until Dec 2021.&#x20;

Public launch will happen on January 2022.

### Is there going to be a public sale?

There are no plans for a public sale.&#x20;

69% of $BABL is reserved for community rewards.

### What's the Babylon team's responsibility?

The Babylon team is responsible for developing the protocol and handing governance to the community.&#x20;

With vesting over the next 4 years, the Babylon team's compensation is directly tied to protocol performance, aligning incentives to support users.

### Who is on the team?

The Babylon team includes seasoned entrepreneurs and technologists who have worked at companies like Y Combinator, OpenZeppelin, Google, Zynga, Allen Institute, and Telefonica.

You can read more about the team [here](https://www.linkedin.com/company/babylonfinance/people).

### How can I provide feedback & contact the team?

The best way to contact the team is to send a message in [Discord](https://discord.gg/eGatHr2a5u).

For more information about Babylon, please read our [introductory blog post](https://medium.com/babylon-finance/introducing-babylon-finance-8199fa89f918).

## $BABL Tokenomics

### What's the purpose of $BABL?

$BABL is the governance token for Babylon. $BABL holders will be able to vote on proposals through on-chain and off-chain governance.

### What's the total supply of $BABL?

1 million tokens

### What's the $BABL distribution?

* 69% reserved for community
* 19% investors
* 10% team
* 2% reserve

### How I can get $BABL?

$BABL is now listed on Uniswap V3 since November 28th 2021.

You can [purchase BABL here](https://app.uniswap.org/#/swap?inputCurrency=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2\&outputCurrency=0xf4dc48d260c93ad6a96c5ce563e70ca578987c74).

The community also voted to start the **Mining Rewards program**. It started on October 17th 2021.

For more information, please read our [blog post about $BABL.](https://medium.com/babylon-finance/announcing-babl-8f41b58c52c5)

## Participation Rewards Program

### What's the purpose of the Participation Rewards Program?

The purpose of the Participation Rewards Program is to distribute $BABL to users based on their participation and the value they add to the protocol.

### How much $BABL is allocated to the program?

We proposed that the Rewards Program distribute 50% of the total token supply, 500K of $BABL, over the next 10 years.

### When does the program start?

October 17th 2021.&#x20;

All participants in the protocol that deposit into Gardens that have active strategies will be eligible to earn rewards.

### How long will the program last?

The supply curve is designed to optimize for the long-term sustainability of the protocol. The rewards are front-loaded but they last for more than 10 years, slowly decreasing every quarter.

### How are rewards distributed?

To align the incentives of all the active participants, we only plan to reward participants involved in active Strategies. Active Strategies are Strategies that have received enough votes to acquire capital from the Garden.

| Stakeholder    | Value Provided          | Rewards                                |
| -------------- | ----------------------- | -------------------------------------- |
| Member         | Capital, Garden signal. | 80% $BABL Rewards                      |
| Strategist     | Strategy submission     | 10% Profit Rewards + 10% $BABL Rewards |
| Voter          | Voting on Strategies    | 5% Profit Rewards + 10% $BABL Rewards  |
| Garden Creator | Garden creation         | 1.10x $BABL Rewards Bonus              |

For more information, please visit the Participation Rewards Program section in the [Litepaper](../../babl/mining.md) and read our blog post about the [Participation Rewards Program](https://medium.com/babylon-finance/babl-mining-program-4829c313268d).

## Gardens

### What's a Garden?

A Garden is an investment community in Babylon. Babylonians can create a Garden and invite people to deposit capital, propose investment Strategies, vote, and earn rewards.

### Who can create a Garden?

During Beta, only Babylonians who receive an invite will be able to create a Garden. After Beta, anyone will be able to create a Garden.&#x20;

### What is the Garden Creator responsible for?

The Garden Creator is responsible for creating and growing their Garden. They define the Garden parameters, whitelist addresses that can join, and assign permissions to members.

After creating a Garden, the Creator doesn't have any special privileges except for receiving a 10% $BABL bonus on top of the $BABL rewards that he/she receives as part of the Participation Rewards Program.

### Are Garden publics or Private?

Gardens can be public or private and Garden Creators can adjust permissions for depositing, voting, and submitting investment Strategies.&#x20;

A Garden could be completely private by allowing only whitelisted addresses to deposit or it could be completely public by allowing anyone to deposit, suggest investment Strategies, and vote. Gardens can also adjust individual permissions, for example, by allowing anyone to deposit capital but only allowing certain members to suggest investment Strategies and/or vote.

### **What parameters can a Gardener set when creating a Garden?**

* **Garden Name**
* **Garden Description** - The Garden thesis and anything else members will want to know
* **Reserve Asset** - The asset that is used for deposits and withdrawals. Strategies consolidate into this asset upon completion and it's used as a performance benchmark.
* **Garden NFT** - Gardens receive an NFT that evolves as the Garden grows.
* **Minimum Member Deposit** - The minimum amount of capital a user needs to deposit to become a Garden member.
* **Maximum Garden Deposits** - The Maximum amount that can be deposited into a Garden by all members.
* **Deposit Hardlock** - The number of seconds a member has to wait after depositing to withdraw. Flash loan prevention.
* **Limit who can join Garden** - Makes Garden private or public.
* **Early Withdrawal Penalty** - Penalty for withdrawing capital from active Strategies.
* **Minimum Strategy Duration** - Minimum number of days a Strategy must be active.
* **Maximum Strategy Duration** - Maximum number of days a Strategy must be active.
* **% Quorum to Activate Strategy** - % of total votes needed to activate a Strategy.
* **# Voters to Reach Quorum** - # of people who have to vote to activate a Strategy.
* **Strategy Cooldown Period** - Hours to wait before activating a Strategy after quorum has been reached.&#x20;
* **Only assets above this liquidity** - Minimum liquidity of assets the Garden can invest in.

### **What reserve assets are available?**

Currently we support WETH and DAI. We plan to add more reserve assets in the future.

### Can a multisig create a Garden?

Yes. You can also create a Garden with a single multisig member to use Babylon for treasury management.

### **Can a Garden Creator whitelist members?**

If the Garden is private, the Garden Creator will need to whitelist and invite members to join. The Creator can also set and manage member permissions. Anyone can join public Gardens.

### Can a Garden change parameters after the Garden is created?

Currently, Gardens can't be changed after creation. This is done to protect users and to prevent them from having to constantly keep an eye on changes.

### How will Gardens upgrade as the protocol upgrades?

Gardens will automatically be upgraded as the protocol upgrades. We will announce any major changes ahead of time so that Gardens are aware of potential changes.

### **Can a whale take over a Garden by depositing more than everyone else?**

It is worth noting that only invited members can access private Gardens. For public Gardens, if the Garden Creator wants broad participation he/she can set a maximum deposit per member, quorum, and # Voters needed to activate a strategy to mitigate this risk.

### What happens if a Strategist or Garden Creator leaves a Garden?

A Garden Creator can leave at any time if the Garden is public. If the Garden is private, the Creator will need to transfer the control of the permissions whitelist to another member.

A Strategist with tokens staked on an active Strategy will not be able to withdraw the staked tokens until the Strategy completes. However, because they are the Strategist, they can unilaterally complete the Strategy whenever they like, and then withdraw.

### How are gas costs handled?

In Babylon, Gardens are designed to socialize gas costs across all the members. The more members there are in a Garden, the cheaper the costs for everyone. It is worth noting that we use OpenZeppelin relayers to ensure that the transactions are mined at a low gas price.

It's important that Garden Creators and Strategists have skin in the game. They need to pay these fixed costs in order to earn the right to earn bonuses and rewards. Their rewards are also tied to the overall performance of the Garden.

The team is working to optimize the cost of depositing and withdrawing from Gardens to be as cheap as possible. In some instances these actions may be done via signature to reduce costs.&#x20;

**Voting on Strategies is always done off-chain via signatures to make it free for users**.

## Deposits & Withdrawals

### **What is a signature (gasless) based transaction?**

In some cases Babylon uses signature based transactions to **greatly reduce gas costs for users**.&#x20;

1. The user signs a transaction object using MetaMask that includes properties like "minAmountOut" and "maxFee" that are then included in the on-chain transaction.&#x20;
2. This signature is then used by a "Keeper" to execute the transaction on behalf of the user.&#x20;

In this case. the user will pay the transaction fee from the principal of the transaction; ie: in case of a deposit the fee is subtracted from the reserve asset to be deposited.&#x20;

All of our transaction UI components that support signatures include details around the "Max Fee" to be paid but the final cost will often be less.&#x20;

To review details about a signature transaction we suggest using [Etherscan](https://etherscan.io/) by searching for your wallet address and reviewing transactions under “**Erc20 Token Txns**”.

### **How do we calculate Garden ownership when Members deposit capital?**

When a Member deposits capital into the Garden, they receive ERC-20 Garden Tokens that represent their % ownership. The number of Garden Tokens received depends on the current NAV of the Garden and the amount of capital deposited.

### How do we calculate NAV of a Garden?&#x20;

NAV of a Garden is calculated by assessing the current value of all the active assets through [Uniswap V3 Oracles.](https://docs.uniswap.org/protocol/reference/core/libraries/Oracle)&#x20;

### **How does withdrawing work? Can I withdraw at any time?**

Everyone in Babylon has the power to withdraw at any time, however in some instances withdrawing may require ending a Strategy and/or paying a penalty.

There are several scenarios:

* If you want to withdraw and there is enough liquidity in the Garden to withdraw the amount you request, you can withdraw at no penalty.
* If you want to withdraw and there isn't enough capital to withdraw (i.e. it's all tied up in active Strategies) you have two options:
  * Wait until the Strategy ends. At that point, you will be able to withdraw a pro-rata amount of the capital returned by the Strategy at no penalty.
  * Rage quit (withdraw immediately) and pay a 2.5% penalty. The penalty goes to the Garden (i.e. to remaining members) to compensate for the cost of partially liquidating active Strategies.
* If you are a Strategist and you stake Garden Tokens on an active Strategy, you can't withdraw those Garden Tokens until they are unstaked. As the Strategist, you always have the option to end the Strategy early if you want to withdraw.
* When you withdraw, **you give up all the BABL from active strategies**. Strategies only receive BABL once they finalize, so if you leave before that, the pending BABL will disappear.

### What happens if I lose access to my wallet?

Unfortunately, we cannot assist you if you lose access to your wallet. The protocol and website are non-custodial and we never hold your private keys.

### **How will performance and reputation be measured?**

Every Garden will have a member's table with a leaderboard based on the $BABL rewards obtained and other metrics. We have plans to add global leaderboards for Gardens, Strategists, and Voters in the future.

## Strategies

### **What protocols are you integrating with?**

You can find a list of all the [integrations here](../../protocol/integrations.md).

### **What operations are supported?**

Lending, borrowing, buying ERC-20s, providing liquidity, staking, and yield farming.

### Can a Garden send capital to a private wallet?

A Garden can only send capital to a private wallet when a member withdraws from a Garden. In the same way, a Garden can only receive capital when a member deposits into the Garden in exchange for Garden Tokens. All Strategies are managed by the protocol and integrations.

### Can a Garden invest in a coin that benefits only a few people?

It's up to the Strategist and Stewards to decide which assets to buy. However, the protocol verifies that all assets available in Babylon have minimum liquidity in Uniswap to avoid 'exit' scams. Minimum liquidity is set during Garden creation.

### **How does exiting positions work? What if market conditions change?**

Once a Strategy duration elapses, a keeper will automatically complete the Strategy and consolidate the Strategy assets into the Garden's reserve asset. A Strategist can terminate their Strategy at any time if market conditions change.

### Will investments get front-run?

In order to minimize front-running risks the protocol does two things:

* Investments are executed by keepers in small chunks of capital.
* Fuzziness around the exact timing that a keeper will enter/exit positions.

## Voting

### **How will voting work?**

Any member of a Garden that has permission to vote will be able to vote on any candidate Strategy with the full weight of their Garden Tokens.&#x20;

Members can vote for or against a Strategy. Votes are not locked, meaning that if you use all of your Garden Tokens to vote on Strategy A, you can also use all of your Garden Tokens to vote on Strategy B. Voting is done using signatures, making it free.

You can find a detailed explanation of the [voting mechanics in the litepaper](../../protocol/litepaper/#voting-mechanics).

### Can I change my vote afterward?

At the moment, you can not change your vote, so please vote carefully.

### **What happens if market conditions change during a vote?**

A Strategist can delete their Strategy during voting for any reason.

### **How long will voting last for a Strategy?**

A Strategy can remain in the candidate state for a maximum of 7 days. At that point, if the Strategy has not achieved quorum, it will be deleted automatically.&#x20;

If a Strategy reaches quorum, it will enter the first cooldown period. The length of the cooldown period is different for each Garden and is set during Garden creation. During the first cooldown period, members who haven't voted for the Strategy can still do so.

If quorum is still met at the end of the first cooldown period, the Strategy will enter a second cooldown period of equal length and no members can vote during this period. Following the second cooldown period the Strategy will be executed by the Keeper.

### Can I delegate my votes?

Vote delegation is not currently possible. However, it may be enabled in the future.

### **Can votes be weighted by performance?**

This is one of the potential improvements we are considering in the future.
