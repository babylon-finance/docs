---
description: This page explains the participation mining program.
---

# BABL Mining Program

BABL Mining Program was the first governance proposal.  It was approved unanimously by the token holders. Rewards started on October 16th 2021.

**500,000 tokens** will be allocated to the BABL Mining Program, representing 50% of the total token supply over ten years.

You can read the initial Medium announcement [here](https://medium.com/babylon-finance/babl-mining-program-4829c313268d).

![](<../.gitbook/assets/image (15).png>)

## TLDR

* The Mining Program lasts for aprox. **10 years**.
* Only **active strategies** earn BABL.
* Members can claim the BABL **only after the strategy has completed**.
* The **longer** the strategy lasts, the more BABL it earns.
* The **more capital** the strategy has, the more BABL it earns.
* The **more returns the strategy has**, the more BABL it earns.
* **Staked prophets** in gardens give garden members additional bonuses in the mining program.
* **Strategist receives 10% of the BABL** of a strategy (if it returns profits).
* **Voters receive 10% of the BABL** of a strategy.
* **Garden members receive 80% of the BABL** of the strategy weighted by their ownership of the garden and how long were they in the garden relative to the duration of the strategy.

## Supply Curve

Let’s start with the **supply curve**. The supply curve is designed to optimize the long-term sustainability of the protocol. The rewards are front-loaded, slowly decreasing quarter by quarter.

Participation rewards will run for at least **8 years**. We are targeting a specific number of rewards per quarter but the protocol may under-allocate or over-allocate depending on the market conditions.&#x20;

Here is how we will calculate the rewards target for a given quarter E:

$$
RewardsQuarter(E)=R_q(E) = RP -RP*(1/1.12)^E - RpSpent(E)
$$

$$
RP = BABL for Mining Program = 500,000
$$

$$
RpSpent(E) = Rewards SpentUntilE
$$

This creates the following supply curve for the Participation Rewards Program:

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MU6ZbTQOlfV8oj9cw0O%2Fuploads%2FEnBY2jTAF90Ae1zdpVzh%2Ffile.png?alt=media)

## Rewards per epoch and per block

Given the supply curve, we can calculate how many rewards we can distribute per second of a specific quarter. Rq(E) for a given epoch E. An epoch is three months or a quarter.

Then we calculate the max number of rewards to distribute in a second dividing by the number of seconds in the epoch. Rs (Rewards per second) is then calculated as follows:

$$
Rs(E) = Rq(E) /(90 days/Epoch * 24 h/day * 60 min/h * 60 secs/min)
$$

## Strategy Rewards (baseline)

It is important to make sure that the incentives of all the participants are aligned. To that end, we only **reward participants involved in active Strategies**. Active Strategies are the ones that received enough votes to attract capital from the Garden.

The rewards of a Strategy are based both on the **reserve asset** (ETH) allocated (“Principal”) and the **number of seconds** that the Strategy was active. These rewards will also be based on the amount of principal allocated to other Strategies in the protocol during that same time period.

In short, the max rewards of a Strategy can be modeled as follows:

$$
StrategyPower = StrategyPrincipal * StrategyDuration
$$

$$
ProtocolPower (E)= Σ_i^n (StrategyPrincipal (strategy_i) (ETH) * StrategyDuration (strategy_i) (secs))
$$

$$
StrategyRewards(E)(strategy_i)= StrategyPower(strategy_i) /
ProtocolPower(E)*Rq(E)
$$

### Example

As an example, if a Strategy "i" received 1ETH and lasted 1 hour:

_StrategyPower(strategy\_i) = 1 \* 60\*60 = 3600 weight_

If we also assume that there were other 10 strategies in addition to our strategy "i" with a total of 50 ETH (5 ETH each) during that same time period and that we are in Q1, where rewards are 53,571 BABL:

_ProtocolPower(E) = 10 \*(5 ETH\* 3600 secs) + (1 ETH \* 3600 secs)= 183,6K weight_

_StrategyRewards(Q1)(strategy\_i) = 3600 / 1.836 \* 10^5 = 0.02 \* 53,571 = 1,050.41 BABL_

When a Strategy duration is longer than a quarter, it will get proportional BABL tokens per quarter. In that case, the strategy "i" will get:

$$
StrategyRewards (strategy_i) = Σ_j^N StrategyRewards(E_j)(strategy_i)
$$

$$
Σ_j^N ((StrategyPower(E_j)(strategy_i)/ProtocolPower(E_j))*Rq(E_j))
$$

The Strategy will get the sum of the corresponding rewards from each epoch (each of them will depend on each protocol power and the available number of tokens per quarter/epoch).

This calculation provides the strategy rewards "baseline". In the following section we will discuss about the new weights introduced by the decentralized governance.

$$
StrategyRewardsBaseline (strategy_i)
$$

### Adjusting based on performance vs. principal

As part of the decentralized governance proposal [bip#1](https://www.withtally.com/governance/babylon/proposal/86895390742944283448920585513992384112055913494715263243154281712731665188411), and later in [bip#7](https://www.withtally.com/governance/eip155:1:0xBEC3de5b14902C660Bd2C7EfD2F259998424cc24/proposal/42309909082201573650128991948203453910830611292212115544850475689408371624584), the community decided to introduce new weights to penalize strategies that are farming BABL without generating wealth.

The performance (returns) must have higher impact than the principal (capital allocated) in the final rewards received by each strategy. These are the weights:

* _Principal Weight_ (5%): represents the principal weight to apply to the baseline.
* _Profit Wei_ght (95%): represents the weight of profits to apply to the baseline.

Therefore, the **profits of a strategy** represent the most important factor to determine BABL rewards earned. We want to encourage strategists, stewards an garden creators to create, vote and execute the best performing strategies as possible.

Note that the profit can be positive or negative, which means that we can get more BABL than provided by the baseline but negative profit strategies will get less than if they were only based on principal.

### Negative, break-even and positive strategies

In order to carry out the goal to reward strategies, BIP-7 divides the strategies into three different buckets based on its profits.\
\
**Bad strategies:** all strategies that are **under -20% returns** annualized belong to this segment. These strategies will get only <mark style="color:red;">15% of the rewards</mark> (85% slashed).

**Break-even strategies**: all strategies which are above -20% but are **still under 4% returns annualized** belong to this segment. These strategies will get only <mark style="color:yellow;">60% of the rewards</mark> (40% slashed).

**Profitable strategies:** all strategies which are equal or **above 4% annualized** returns belong to this segment. These strategies will get an additional <mark style="color:green;">bonus of 15% rewards</mark> i.e they will receive 115%. After BIP-7 there is no max cap for the strategies, so higher returns mean higher BABL without any limit.

Each Strategy will get then its BABL rewards as follows:

$$
StrategyRewards(E)(strategy_i)= StrategyRewardsBaseline(E)(strategy_i)*(PrincipalWeight +  Profit(strategy_i)*ProfitWeight*BenchmarkRatioForSegment)
$$

where "_BenchmarkRatioForSegment_"  is 15% for segment 1, 60% for segment 2 and 115% for segment 3.

### Heart  strategies

In case of the strategy belongs to the Heart of Babylon, it will always get a <mark style="color:blue;">bonus of 50%</mark> (i.e. they will receive 150%).

## Rewards per participant

At this point, we know how much BABL to allocate to a finalized Strategy. However, we still need to **distribute this BABL among all the participants** for that Strategy.&#x20;

In addition to BABL rewards (which are based mostly on participation), the **protocol will also distribute the Profit Rewards** (if any) among the different participants depending on their active role. 5% of the profits are allocated to the treasury of the protocol, the other 95% is allocated to the participants following the garden default or customized distribution.

With regard to BABL rewards, LPs get 80%, the Strategist gets 10% and the Voters get 10% of Strategy BABL rewards. To summarize, we outline the profit-sharing below but we will focus on the BABL rewards. You can read more about each participant and the profit split in the [litepaper](../protocol/litepaper/#profit-and-reward-split).

Now, we are going to cover the reward baseline for each type of participant.

### 🧠 **Strategists**

**10%** in BABL Rewards

The creator of the Strategy will receive 10% of BABL rewards if the Strategy is successful. In order to align incentives, **Strategies that result in losses for the Garden will not result in any BABL rewards** for their Strategists.

_e.g. If 100 BABL rewards are delivered to the Strategy"i" in total, 10 goes to the Strategist in case it is a profitable Strategy._

The Strategist will also receive 10% (or the customized % value per each garden, if any) of the Strategy profits (if any) if the Strategy was successful. On the other hand, **if the Strategy is not successful, the protocol will slash the Strategist's stake**.

_e.g. If 100 ETH are returned as profits, the Strategist will get 10 ETH._

### 📰 **Voters**

**10%** in BABL Rewards

All the Voters that upvoted the strategy will receive their proportional share of the rewards based on the number of votes cast by each member.

_e.g. If 100 BABL tokens are delivered to the Strategy" i" in total; 10 will be split between Voters according to the votes cast by each Voter._

Voters will also receive a total performance fee of 5% (or the customized % defined at the garden level, if any) of all the Strategy Profits in ETH. You can see a table with the specifics in the [litepaper](../protocol/litepaper/#profit-considerations).

_e.g. If 100 ETH are returned as profits, Voters will get in total 5 ETH that will be split according to the votes cast by each Voter._

_Adjustments: as there are multiple cases, we recommend to read the section "_Adjusting based on performance and expected returns_". Anyway, here we provide a summary of the different adjustments:_&#x20;

* _i_f the strategy got no profits at all and all users voted for the strategy, **there will be no rewards for stewards** as a mechanism to protect the protocol from bad strategies.&#x20;
* If some users voted against its execution and it finally had no profits, the 10% will be distributed only between those who tried to protect the protocol voting against it.&#x20;
* On the other hand, stewards voting against a strategy returning profits above its expected return will get no rewards as stewards, as they were voting against the execution of a good strategy. In this case, the 10% will be distributed among the users who voted for the strategy.

### 🧧 **Liquidity Providers**

**80%** in BABL Rewards

All Garden LPs that deposit in gardens with active strategies receive 80% of the BABL rewards split based on the amount deposit by each member. The amount depends also on the time the user has been a member of the garden relative to the duration of the strategy

_e.g. If 100 BABL rewards are delivered to the Strategy" i" in total, 80 goes to LPs, proportional to their ETH deposited during the Strategy execution period._

LPs will also receive a total of 80% (or the garden customized value, if any) of the Strategy profits proportional to their ETH deposited during the Strategy execution period.

_e.g. If 100 ETH are returned as Strategy profits, LPs will get  80 ETH split between them based on the amount they had deposited in the Garden at the moment of strategy execution. Contributor power will consider when contributor deposits took place and the size of each deposit out of all garden deposits._

### 🌱 **Gardeners**

**1.10x Multiplier in BABL Rewards**

The Gardener is also an active member of the Garden. As the Gardener of the community, they gets a special bonus for all the other activities that they perform as a member.

## 🛡️Prophet Bonuses

Prophets that are **staked in a garden** gives the user extra bonuses in the mining rewards of that garden.

Prophets offer **boosts to the participants** based on the bonus type.

* 🧧LP Bonus. The LPs get in aggregate 80% of the BABL of the strategy. If you stake a prophet that has a lp bonus, **you will receive a bonus on your proportional share of the lp BABL rewards**.&#x20;

_e.g. If 100 BABL rewards are delivered to the Strategy" i" in total, 80 goes to LPs, proportional to their assets deposited during the Strategy execution period. If you have 10% of the assets and a 3% prophet staked bonus, you would receive 8.24 BABL instead of 8._

* 📰 Voter Bonus. The voters get in aggregate 10% of the BABL of the strategy. If you stake a prophet that has a voter bonus, **you will receive a bonus on your proportional share of the voting BABL rewards**.&#x20;

_e.g. If 100 BABL tokens are delivered to the Strategy" i" in total; 10 will be split between Voters according to the votes cast by each Voter. If you have 20% of the voting power and a 2% prophet staked bonus, you would receive 2.04 BABL instead of 2._

* 🧠Strategist Bonus. If you stake a prophet that has a strategist bonus, it will be **added to the 10% mentioned above**.&#x20;

_e.g. if your great prophet gives you a 1% bonus, your strategist BABL rewards will be 11% of the strategy instead of 10%._

* 🌱 Gardener Bonus. If you stake a prophet that has a gardener bonus, it will be **added to the 1.10x multiplier above**.&#x20;

_e.g. If your great prophet gives you a 3% bonus, your multiplier will be 1.13x instead of 1.10._

### Summary

The total sum of BABL rewards will then be divided into the different profiles:

$$
StrategyRewards(strategy_i)= StrategyRewardsStrategists(strategy_i) + StrategyRewardsVoters(strategy_i) + StrategyRewardsLPs(strategy_i)
$$

where:

$$
StrategyRewardsStrategist (strategy_i)= StrategyRewards(strategy_i) * StrategistWeight
$$

$$
StrategyRewardsVoters(strategy_i) = StrategyRewards (strategy_i) * VotersWeight
$$

$$
StrategyRewardsLPs(strategy_i) = StrategyRewards(strategy_i) * LPsWeight
$$

**Note**:  the Gardener will get the 1.10x multiplier on all the other roles taken in this Garden. Strategist and Voters will be able to get additional tokens (limited by a 2x cap) depending on the expected return as described below.

## Adjusting based on performance and expected returns

The BABL rewards explained above are refined based on performance to ensure that Babylon rewards Strategies that return capital and respect the opportunity cost associated with executing them.

### Expected return

The "_expected return_" concept is a key element in Babylon. It is set by the Strategist based on market conditions, knowledge, and experience. Voters **review the Strategy details, including the expected return,** set by the Strategist to decide whether or not it is a good use of the Garden's capital. Voters need to stake their Garden Tokens to vote so Voters need to pick the Strategies they want to vote on wisely.&#x20;

The protocol tries to ensure that the dominant tactic for each participant is aligned with the goals of Babylon.

* Strategies are penalized if they inflate their expected returns. Their BABL rewards are tuned based on the accuracy of their prediction.&#x20;
* Voters are disincentivized from upvoting or downvoting without enough information. They are locking their BABL tokens and will only get rewarded when they provide an accurate signal.

### Negative Strategies

Strategies that result in losses will **only provide BABL rewards to Voters that downvoted the strategy**. The Strategist and the Voters that upvoted it do not receive any rewards. The Strategist's stake gets also slashed with a quadratic penalty of **-1.75x the % of losses**.&#x20;

_e.g. If a Strategy loses 5% of capital (capital returned / capital allocated = 95%), the Strategist will get a quadratic penalty of 1.75x 5% = 8.75% of the Strategist stake will then be burned by the protocol._

### Real Return vs Expected Return

Adjustments to the baseline model are done based on the distance between the expected return and the real return.

#### 🧠 **Strategist Bonus**

Strategist receives a bonus (**capped to a max of 2x**) of BABL rewards based on the accuracy of their prediction. The more accurate it is, the more BABL rewards the Strategist receives.

On the other hand, a negative strategy (with negative profits) will not provide BABL rewards to Strategists and the Strategist's stake will be slashed as defined above.

#### 📰 **Voter Bonus**

Voters voting in **favor** of a **profitable Strategy** will get proportional **BABL rewards depending on their stake** (vote) out of the total positive votes. Users voting in favor of bad strategies with negative results will get 0 BABL rewards as they contributed to draining the capital of the Garden.

Voters voting **against** a Strategy will get **BABL rewards only if the returns are below the expected** return. The bonus has a max cap of 2x.  It will also be proportional to the stake used for the vote out of the total negative votes.&#x20;

## Montecarlo Simulations

* You can check the numbers of a Montecarlo simulation with 🔢 [**200 strategies here**](https://docs.google.com/spreadsheets/d/1jsvRCogeuXTcUICUgqqU81m8PP919lVCaw0KxrrUCAU/edit?usp=sharing).

![](<../.gitbook/assets/Captura de pantalla 2022-01-26 a las 18.40.08.png>)
