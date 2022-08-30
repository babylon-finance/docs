# Building on Babylon

## Why build on Babylon?

Babylon provides a new DeFi lego building block for developers: :palm\_tree: Babylon Gardens.

Babylon Gardens are the first **DeFi Investment DAO primitive (ERC-20)**.

See our workshop video in HackMoney 2022 to learn how to get started.

{% embed url="https://www.youtube.com/watch?v=nLKtwckXBKU" %}

Gardens are tokenized investment clubs/DAOs where users can deposit digital assets and receive a tokenized share (ERC-20) representing their ownership.

Developers can build on top of Babylon Gardens and benefit from all the following built-in features.

You can read more in our medium announcement here

{% embed url="https://medium.com/@rrecuero/babylon-on-chain-sdk-438d6f61b7e7" %}

### Built-in Features

* Garden ERC-20 tokens are **fully composable and transferrable**.
* Gardens are **non-custodial and trust minimized**. Only users have access to their funds. Capital can only be allocated to strategies approved by the club.
* Gardens also provide an **NFT Membership token (ERC-721)**. Members of a garden that meet selected requirements can mint their NFT.
* **Built-in light governance system with signature-based voting**. Gas-free.
* No-hassle and **No code UI** to create strategies and deploy capital to [+15 DeFi protocols](../protocol/integrations.md) including Aave, Compound, Uniswap, Curve, and Convex.
* **DeFi execution costs are automatically shared** between all members of a Garden, increasing the capital efficiency.
* Smart capital allocation between strategies based on current gas costs and settings.
* **Garden UI** where users & managers can deposit/withdraw and monitor the performance of their investment club.

You can read more on our [managers page](https://www.babylon.finance/managers).

## What can you build?

The design space is unlimited. You can connect the gardens with the rest of DeFi and web3 in many different ways.&#x20;

For example:

* **Create an investment club to invest with your friends** through a Babylon garden. This was the original usecase that prompted us to start Babylon. You can learn more about our origin story [here](https://www.youtube.com/watch?v=Cn-GKRwB9wc).
* You can use a garden as a way **to generate an index of different DeFi opportunities**. For example, here is a Garden that gives exposure [to many different Pickle Jars at once](https://twitter.com/ramonrecuero/status/1522351546922520578?s=20\&t=MbqSGb9flyUdscEEWX0iSQ).
* You can use a garden to **simplify access to DeFi opportunities across different protocols**. For example, the ⛲ [Fountain of ETH](https://www.babylon.finance/garden/0xB5bD20248cfe9480487CC0de0d72D0e19eE0AcB6) generates yield on ETH by taking advantage of staking opportunities across different protocols.
* You can use a garden **to create a supercharged staking mechanic for any ERC-20 token.** You can see more information [here](https://babylon.finance/daos). We use a garden to create a staking mechanic for BABL itself, [see the Heart](https://medium.com/babylon-finance/the-heart-of-babylon-defi-3-0-6d16e35817f2) 🫀.
* Automatically **create and deploy strategies to react to events and market conditions**. You can execute and trigger strategies directly from your smart contracts.

**Gardens and strategies can be created on-chain directly from your smart contracts** in a response to an event or directly through our user interface. Check the section below to learn more about the API.

## Testnets

Instead of using Ethereum testnets like Ropsten or Kovab, Babylon Finance uses [Hardhat](https://hardhat.org/) chain for testing and local development.&#x20;

Babylon Finance integrates with +15 DeFi protocols. Therefore creating a full-working deployment on testnests like Kovan or Ropsten is really cumbersome and prone to errors.&#x20;

**Hardhat lets you clone Ethereum Mainnet and run the clone on your local machine and then you can deploy contracts on top of your cllone**.&#x20;

That way you can build on top of mainnet with all its functionality and DeFi protocols with Ease.

## Smart Contracts

You can view all the information required in our API docs.

{% content-ref url="smart-contract-api/" %}
[smart-contract-api](smart-contract-api/)
{% endcontent-ref %}

You can see their deployment addresses on the deployments page:

{% content-ref url="deployments.md" %}
[deployments.md](deployments.md)
{% endcontent-ref %}

Do you want to connect Babylon to another DeFi protocol that is not supported? Now you can build a custom integration and potentially receive bounties.

{% content-ref url="custom-integrations.md" %}
[custom-integrations.md](custom-integrations.md)
{% endcontent-ref %}

## How to get started

Here are the steps we recommend to get started.

1. You can create an investment club by calling `createGarden` on the [Babylon Controller](smart-contract-api/babcontroller.md) or using our UI at [https://babylon.finance/explore](https://babylon.finance/explore).
2. Once the garden is created, you have your own fully composable tokenized investment DAO as an ERC-20.
3. You can compose gardens and their strategies with the rest of #DeFi. Check the [Garden](smart-contract-api/garden.md) and [Strategy](smart-contract-api/strategy.md) APIs to see the functions available.
4. If you want to connect Babylon to a different protocol, please follow the [custom integration guide](custom-integrations.md).

If you have any trouble, please head to the [development channel in our Discord](https://discord.gg/KMnrHrVJZh) and we'll do our best to help.

