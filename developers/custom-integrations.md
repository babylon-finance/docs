---
description: >-
  This page will show you how to create a new integration to connect Babylon to
  a new DeFI protocol.
---

# 🛠 Custom Integrations

**Integrations** are adapters that Babylon uses to **connect with other DeFi & Web3 protocols**.

So far, Babylon has developed all these adapters in-house. You can see the [whole list here.](../protocol/integrations.md)

![](<../.gitbook/assets/image (18).png>)

Starting today, anyone can develop an adapter, test it on a private garden and then submit it to be verified and audited by Babylon governance.

## Requirements

* Only gardens with the flag `customIntegrationsEnabled` can create strategies that use custom integrations.
* The protocol you are connecting with **must return an ERC-20 token** in an amount proportional to the capital provided by the strategy.
* The resulting ERC-20 tokens need to be priced on-chain in a safe and reliable way. Enough liquidity, resistant to manipulation...

## Architecture Recap

Babylon has 1-N gardens or investment clubs.

Every Garden has 0-N active strategies at any single moment.

![](<../.gitbook/assets/image (9).png>)

Every strategy has 1-N DeFi operations it executes to start and exit a strategy.

Every **operation uses one adapter** to connect to an specific DeFi protocol.

## How to build a custom integration?

In order to facilitate the process as much as possible, we have created a template called `CustomIntegrationTemplate.sol` that you can copy and paste and rename to start working on your custom integration.

You can also look at the example `CustomIntegrationYearn` to see how to connect with Yearn using the template.

### Steps to build your integration

To complete the integrations, there are a few functions that need to be filled. You can read more about their signatures and parameters on their [API page](smart-contract-api/custom-integrations.md).

1. Copy `CustomIntegrationTemplate.sol` and rename it.
2. Add any state variables you would need.
3. Modify the constructor to receive any extra parameters you need on deployment. Change the name `test_`_`sample_`_`change_me`

```
constructor(IBabController _controller) CustomIntegration('**test_sample_change_me**', _controller) {
    require(address(_controller) != address(0), 'invalid address');
}
```

4\.  Fill the function that checks whether the **parameters passed on execution to this operation are valid**. These are different than the constructor parameters. These are the configurable parameters. For example, which yearn vault to enter.

```
function _isValid(
    bytes memory /* _data */
) internal pure override returns (bool) {
    /**FILL THIS*/
    return true;
}
```

5\.  Fill `_getSpender`, this function returns the address where the input tokens need to be approved to before execution. This address will be called with ERC-20 approve and the input tokens before execution.

```
function _getSpender(
    bytes calldata, /* _data */
    uint8 /* _opType */
) internal pure override returns (address) {
    /** FILL THIS */
    return address(0);
}
```

6\.  Implement `_getResultToken`. This function must return the result token (ERC-20) to be received after executing the enter operation.

```
function _getResultToken(address _token) internal pure override returns (address) {
    /** FILL THIS */
    return _token;
}
```

7\. Provide the input tokens and the weights of these tokens. The strategy will automatically **buy these tokens with the capital before entering the strategy** according to these weights.

```
function getInputTokensAndWeights(
    bytes calldata /* _data */
) external pure override returns (address[] memory _inputTokens, uint256[] memory _inputWeights) {
    /** FILL THIS */
    return (new address[](1), new uint256[](1));
}
```

8\. Provide the **output tokens and the minimum amounts to receive for each upon exit** based on \_liquidity amount of the result token defined above**.**

```
function getOutputTokensAndMinAmountOut(
    bytes calldata, /* _data */
    uint256 /* _liquidity */
) external pure override returns (address[] memory exitTokens, uint256[] memory _minAmountsOut) {
    /** FILL THIS */
    return (new address[](1), new uint256[](1));
}
```

**9. Provide the enter calldata**. You must return the calldata required for the strategy to allocate capital to this integration.

```
function _getEnterCalldata(
    address, /* _strategy */
    bytes calldata, /* _data */
    uint256, /* _resultTokensOut */
    address[] calldata, /* _tokensIn */
    uint256[] calldata /* _maxAmountsIn */
)
    internal
    pure
    override
    returns (
        address,
        uint256,
        bytes memory
    )
{
    /** FILL THIS */
    return (address(0), 0, bytes(''));
}
```

10\. **Provide the exit calldata**. You must return the calldata required for the strategy to exit and recover the capital allocated through this integration.

```
function _getExitCalldata(
    address, /* _strategy */
    bytes calldata, /* _data */
    uint256, /* _resultTokensIn */
    address[] calldata, /* _tokensOut */
    uint256[] calldata /* _minAmountsOut */
)
    internal
    pure
    override
    returns (
        address,
        uint256,
        bytes memory
    )
{
    /** FILL THIS */
    return (address(0), 0, bytes(''));
}
```

11\. (Optional). Provide the list of reward tokens that will be received upon exit. List of ERC-20 addresses.

```
/**
 * The list of addresses of the IERC-20 tokens mined as rewards during the strategy
 *
 * hparam  _data                      Address provided as param
 * @return address[] memory           List of reward token addresses
 */
function _getRewardTokens(
    address /* _data */
) internal pure override returns (address[] memory) {
    return new address[](1);
}
```

12\. (Optional) If the result tokens cannot be priced through Babylon Price Oracle, you need to implement the following function that returns the price of the result token in the `_tokenDenominator`

```
function getPriceResultToken(
    bytes calldata, /* _data */
    address /* _tokenDenominator */
) external pure override returns (uint256) {
    /** FILL THIS */
    return 0;
}

```

13\. (Optional) If you need to execute another custom action before entry or exit, you can define a pre-action hook. `_customOp` will be 0 for enter and 1 for exit. If this action needs to execute an ERC approval you can fill `_preActionNeedsApproval`.

```
/**
 * (OPTIONAL). Return pre action calldata
 *
 * hparam _strategy                  Address of the strategy
 * hparam  _asset                    Address param
 * hparam  _amount                   Amount
 * hparam  _customOp                 Type of Custom op
 *
 * @return address                   Target contract address
 * @return uint256                   Call value
 * @return bytes                     Trade calldata
 */
function _getPreActionCallData(
    address, /* _strategy */
    address, /* _asset */
    uint256, /* _amount */
    uint256 /* _customOp */
)
    internal
    view
    override
    returns (
        address,
        uint256,
        bytes memory
    )
{
    return (address(0), 0, bytes(''));
}
```

14\. (Optional) Similarly, if you need to execute another custom action after entering or exiting, you can define a pre-action hook. `_customOp` will be 0 for enter and 1 for exit.  If this action needs to execute an ERC approval you can fill `_postActionNeedsApproval`.

```
/**
 * (OPTIONAL) Return post action calldata
 *
 * hparam  _strategy                 Address of the strategy
 * hparam  _asset                    Address param
 * hparam  _amount                   Amount
 * hparam  _customOp                 Type of op
 *
 * @return address                   Target contract address
 * @return uint256                   Call value
 * @return bytes                     Trade calldata
 */
function _getPostActionCallData(
    address, /* _strategy */
    address, /* _asset */
    uint256, /* _amount */
    uint256 /* _customOp */
)
    internal
    view
    override
    returns (
        address,
        uint256,
        bytes memory
    )
{
    return (address(0), 0, bytes(''));
}
```

### Utils

You can use the following functions directly from your custom integration:

#### `_getPrice(address _tokenIn, address _tokenOut) view returns (uint256)`

It gets you the price of `_tokenIn` __ denominated in __ `_tokenOut` from our price oracle (1e18). It will return an exception if it doesn't know how to price a specific token.

**`_getDurationStrategy(address _strategy) view returns (uint256)`**

It returns the duration of the strategy in seconds.

#### `_getTokenOrETHBalance(address _strategy, address _token) view returns (uint256)`

Returns the balance of the strategy of the `_token`. If token is 0x or 0xEEEE..EEE, it will return the ETH balance of the strategy.

#### `_getTradeCallData(address _strategy, address _tokenIn, address _tokenOut, uint256 _amountIn, uint256 _minAmountOut) view returns (address, uint, bytes memory)`

Receives the trade call data needed to execute a swap of `_tokenIn` to `_tokenOut.` The number of \_tokenIn supplied is specified by`_amountIn`. The call will revert if you don't receive at least `_minAmountOut`. It returns the address, callvalue, and calldata to pass directly to a preaction hook, postaction hook, enter or exit functions

## Testing the custom integration

In the `contracts` repository there is a test file that executes the yearn custom integration example. In order to test your integration, follow these steps:

1. Change the integration to deploy to the name of your custom integration. Pass the appropriate params depending on your constructor

![](<../.gitbook/assets/Screen Shot 2022-05-17 at 8.47.04 AM.png>)

&#x20;   2\. Run `yarn run test`. That's it, you can start debugging 🎉

The test suite will automatically create a strategy with your integration, approve it, allocate capital to it, execute it and finalize it.

## Testing on mainnet with a live garden

You can also test it directly on mainnet using the [Test WETH garden](https://www.babylon.finance/garden/0x2c4Beb32f0c80309876F028694B4633509e942D4) on the Babylon website or creating your own garden. Make sure you enable custom integrations in the garden parameters.

* Deploy your custom integration to mainnet.
* Deposit the min amount to join the test garden.
* Submit a new strategy and select your custom integration as one of the operations. Set the address of your deployed integration as the integration address. You can also add two optional params that the integration will receive on execution, an address and a number.

![](<../.gitbook/assets/Screen Shot 2022-05-18 at 2.20.01 PM.png>)

It should execute automatically in two hours given that strategies automatically reach quorum in this garden and do not need voters.

## Submitting it for approval

After the tests above pass for your integration, please follow the next steps:

* Clone the repo and **submit a PR** against the `contracts` repo.
* **Head to our Discord and post a message** explaining your integration and adding the link to your PR in the `#development` channel.
* We'll **review your PR** and follow up to get it audited and eventually incorporated into our list of verified integrations.
* We may **award a bounty** if the integration is approved and it is one of the integrations on our wishlist.
