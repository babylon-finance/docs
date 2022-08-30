# Strategy

Interface: [IStrategy.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/interfaces/IStrategy.sol)

## Functions

The following functions can be called by any user or public roles.

### Sweep

_Public: Anyone can call this function._

This function converts any token sent to the strategy into the reserve asset and sends it to the garden.

```
function sweep(address _token, uint256 _newSlippage) external;
```

{% hint style="info" %}
The strategy must be inactive
{% endhint %}

| Parameter              | Description                   |
| ---------------------- | ----------------------------- |
| `address _token`       | Address of the token to sweep |
| `uint256 _newSlippage` | Overrides slippage setting    |

### UpdateParams

_Only the Strategist can call this function_

This function updates specific strategy parameters.

```
function updateParams(uint256[5] calldata _params) external;
```

| Parameter                                           | Description                       |
| --------------------------------------------------- | --------------------------------- |
| <pre><code>uint256[5] calldata _params</code></pre> | Array of 5 parameters. See below. |

Definition of strategy parameters:

```
*   _params[0]  duration                    Strategy duration in seconds
*   _params[1]  maxGasFeePercentage         Max gas fee % capped to 10e16
*   _params[2]  maxTradeSlippagePercentage  Max trading slippage % capped to 20e16
*   _params[3]  maxAllocationPercentage     Max garden capital allocation % (<= 1e18)
*   _params[4]  maxCapitalRequested         Max capital requested in reserveAsset
```

{% hint style="info" %}
New duration value must be equal or less than the original duration.&#x20;
{% endhint %}

### DeleteCandidateStrategy

_Only the Strategist can call this function_

This function deletes a candidate strategy.

```
function deleteCandidateStrategy() external;
```

## Privileged Functions

The following functions can only be executed by protocol privileged roles.

### ResolveVoting

_Only the protocol Keeper can call this function_

This function adds off-chain gas-less voting results on-chain.

```
function resolveVoting(
    address[] calldata _voters,
    int256[] calldata _votes,
    uint256 _fee
) external;
```

| Parameters                   | Description                                        |
| ---------------------------- | -------------------------------------------------- |
| `address[] calldata _voters` | An array of garden member who voted on strategy    |
| `int256[] calldata _votes`   | An array of votes by on strategy by garden members |
| `uint256 _fee`               | The fee paid to keeper to compensate the gas cost  |

### ExecuteStrategy

_Only the protocol Keeper can call this function_

This function executes a strategy that has been activated and gone through the cooldown period.

```
function executeStrategy(uint256 _capital, uint256 _fee) external;
```

| Parameter          | Description                                       |
| ------------------ | ------------------------------------------------- |
| `uint256 _capital` | The capital to allocate to this strategy          |
| `uint256 _fee`     | The fee paid to keeper to compensate the gas cost |

### FinalizeStrategy

_Only the protocol Keeper can call this function_

This function exits from an executed strategy. Returns balance back to the garden and sets the capital aside for withdrawals in ETH. Pays the keeper and updates the reserve asset position accordingly.

```
function finalizeStrategy(
    uint256 _fee,
    string memory _tokenURI,
    uint256 _minReserveOut
) external;
```

| Parameter                 | Description                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| `uint256 _fee`            | The fee paid to keeper to compensate the gas cost                 |
| `string memory _tokenURI` | URL with the JSON for the strategy to grant NFT to the strategist |
| `uint256 _minReserveOut`  | Minimum reserve asset to get during strategy finalization         |

### UnwindStrategy

_Only the protocol Keeper can call this function_

This function partially unwinds a strategy. Triggered from a penalty withdrawal in the Garden.

```
function unwindStrategy(uint256 _amountToUnwind, uint256 _strategyNAV) external;
```

| Parameter                 | Description                     |
| ------------------------- | ------------------------------- |
| `uint256 _amountToUnwind` | The amount of capital to unwind |
| `uint256 _strategyNAV`    | NAV of the strategy to unwind   |

### ExpireStrategy

_Only the protocol Keeper can call this function_

This function expires a candidate that has spent more than `CANDIDATE_PERIOD` seconds without reaching quorum.

```
function expireStrategy(uint256 _fee) external;
```

| Parameter      | Description    |
| -------------- | -------------- |
| `uint256 _fee` | The keeper fee |

### SetData

_Only the garden can call this function_

Sets the data for the operations of this strategy.

```
function setData(
    uint8[] calldata _opTypes,
    address[] calldata _opIntegrations,
    bytes memory _opEncodedData
) external;
```

| Parameter                            | Description                                                          |
| ------------------------------------ | -------------------------------------------------------------------- |
| `uint8[] calldata _opTypes`          | An array with the op types                                           |
| `address[] calldata _opIntegrations` | Addresses with the integration for each op                           |
| `bytes memory _opEncodedData`        | 64 bytes of operations metadata per operation (size of 64 \* numOps) |

Definition of types of operations:

```
// 0 = BuyOperation
// 1 = LiquidityOperation
// 2 = VaultOperation
// 3 = LendOperation
// 4 = BorrowOperation
```

### Trade

_Only a valid operation can call this function_

Function that calculates the price using the oracle and executes a trade. Must call the exchange to get the price and pass minReceiveQuantity accordingly.

```
function trade(
    address _sendToken,
    uint256 _sendQuantity,
    address _receiveToken
) external returns (uint256);

function trade(
    address _sendToken,
    uint256 _sendQuantity,
    address _receiveToken,
    uint256 _overrideSlippage
) external returns (uint256);
```

| Parameter                   | Description                   |
| --------------------------- | ----------------------------- |
| `address _sendToken`        | Token to exchange             |
| `uint256 _sendQuantity`     | Amount of tokens to send      |
| `address _receiveToken`     | Token to receive              |
| `uint256 _overrideSlippage` | Slippage to override (if any) |

### HandleWeth

_Only a valid operation can call this function_

Function that deposits or withdraws WETH from an operation in this context.

```
function handleWeth(bool _isDeposit, uint256 _wethAmount) external;
```

| Parameter             | Description                      |
| --------------------- | -------------------------------- |
| `bool _isDeposit`     | Whether is a deposit or withdraw |
| `uint256 _wethAmount` | Amount to deposit or withdraw    |

### InvokeFromIntegration

_Only a valid integration can call this function_

Helper to invoke a call to an external contract from integrations in the strategy context.

```
function invokeFromIntegration(
    address _target,
    uint256 _value,
    bytes calldata _data
) external returns (bytes memory);
```

| Parameter              | Description                                         |
| ---------------------- | --------------------------------------------------- |
| `address _target`      | Address of the smart contract to call               |
| `uint256 _value`       | Quantity of Ether to provide the call (typically 0) |
| `bytes calldata _data` | Encoded function selector and arguments             |
| `return value`         | Bytes encoded return value                          |

### InvokeApprove

_Only a valid integration can call this function_

Helper to invoke Approve on ERC20 from integrations in the strategy context.

```
function invokeApprove(
    address _spender,
    address _asset,
    uint256 _quantity
) external;
```

| Parameter           | Description                   |
| ------------------- | ----------------------------- |
| `address _spender`  | Spender address to be allowed |
| `address _asset`    | Asset address                 |
| `uint256 _quantity` | Amount to approve             |

## View functions

The following functions can be called by anyone without a transaction to retrieve information from the strategy.

### GetNAV

Function to get the strategy Net Asset Value (NAV) in reserveAsset.

```
function getNAV() external view returns (uint256);
```

### OpEncodedData

Function to get encoded operation data (in bytes) where each consecutive 64 bytes are reserved for each operation metadata. Metadata is usually including operation addresses and values needed for those operations.

```
function opEncodedData() external view returns (bytes memory);
```

### GetOperationsCount

Function to get the number of operations used by the strategy.

```
function getOperationsCount() external view returns (uint256);
```

### GetOperationByIndex

Function to get the operation data by index.&#x20;

```
function getOperationByIndex(uint8 _index)
    external
    view
    returns (
        uint8,
        address,
        bytes memory
);
```

### GetStrategyDetails

Function to get the details of the strategy.

```
function getStrategyDetails()
    external
    view
    returns (
        address garden,
        address strategist,
        uint256 opIntLength,
        uint256 stake,
        uint256 totalPositiveVotes,
        uint256 totalNegativeVotes,
        uint256 capitalAllocated,
        uint256 capitalReturned,
        uint256 duration,
        uint256 expectedReturn,
        uint256 maxCapitalRequested,
        address strategyNFT,
        uint256 enteredAt,
        uint256 nav
    );
```

| Parameter                     | Description                                                         |
| ----------------------------- | ------------------------------------------------------------------- |
| `address garden`              | Garden address                                                      |
| `address strategist`          | Strategist address                                                  |
| `uint256 opIntLength`         | Length of the array of addresses with the integration for each op   |
| `uint256 stake`               | Stake of the strategist in this strategy                            |
| `uint256 totalPositiveVotes`  | Total positive votes for the strategy                               |
| `uint256 totalNegativeVotes`  | Total negative votes for the strategy                               |
| `uint256 capitalAllocated`    | Capital allocated for the strategy in reserveAsset                  |
| `uint256 capitalReturned`     | Capital returned by the strategy after finalization in reserveAsset |
| `uint256 duration`            | Duration of the strategy (in seconds)                               |
| `uint256 expectedReturn`      | Expected positive % profit (i.e. 5%) in 18 decimals precision.      |
| `uint256 maxCapitalRequested` | Amount of max capital to allocate to the strategy                   |
| `address strategyNFT`         | Address of the strategyNFT                                          |
| `uint256 enteredAt`           | Timestamp when the strategy was created                             |
| `uint256 nav`                 | Net Asset Value of the strategy in reserveAsset                     |

### GetStrategyState

Function to get the state of the strategy.

```
function getStrategyState()
    external
    view
    returns (
        address strategy,
        bool active,
        bool dataSet,
        bool finalized,
        uint256 executedAt,
        uint256 exitedAt,
        uint256 updatedAt
    );
```

| Parameter            | Description                                                   |
| -------------------- | ------------------------------------------------------------- |
| `address strategy`   | Address of the strategy                                       |
| `bool active`        | Whether or not the strategy is active                         |
| `bool dataSet`       | Whether or not the strategy data is set                       |
| `bool finalized`     | Whether or not the strategy has finalized                     |
| `uint256 executedAt` | Timestamp of initial execution (0 if still not executed)      |
| `uint256 exitedAt`   | Timestamp of strategy finalization (0 if still not finalized) |
| `uint256 updatedAt`  | Timestamp of last strategy update                             |

### GetStrategyRewardsContext

Function to get all relevant context strategy data required for BABL mining program calculations.

```
function getStrategyRewardsContext()
    external
    view
    returns (
        address strategist,
        uint256[] memory data, 
        bool[] memory boolData
    );
```

| Parameter                | Description                      |
| ------------------------ | -------------------------------- |
| `address strategist`     | Address of the strategist        |
| `uin256[] memory data`   | Context data. See below.         |
| `bool[] memory boolData` | Boolean context data. See below. |

Definition of strategy context data:

```
data[0] Timestamp of when the strategy was executed (executedAt)
data[1] Timestamp of when the strategy was finalized (exitedAt)
data[2] Timestamp of when the strategy was updated (updatedAt)
data[3] Timestamp of when the strategy was created (enteredAt)
data[4] Amount of total positive votes for the strategy
data[5] Amount of total positive votes against the strategy
data[6] Capital allocated to the strategy
data[7] Capital returned by the strategy
data[8] Expected capital returned (considers expected return)
data[9] Strategy Rewards
data[10] Profit amount (if any)
data[11] Amount difference between real return and expected capital returned
data[12] Garden token supply when the strategy was created
data[13] Garden token supply when the strategy was finalized
data[14] Proportional slippage factor vs. total duration
boolData[0] Profits (true) losses (false)
boolData[1] If profits where above expectations (true) or below (false)
```

### IsStrategyActive

Function to check whether or not the strategy is active.

```
function isStrategyActive() external view returns (bool);
```

### GetUserVotes

Function to get specific user votes for a user.

```
function getUserVotes(address _address) external view returns (int256);
```

### Strategist

Function to get the strategist address.

```
function strategist() external view returns (address);
```

### EnteredAt

Function to get the timestamp when the strategy was created.

```
function enteredAt() external view returns (uint256);
```

### EnteredCooldownAt

Function to get the timestamp  when the strategy reached quorum.

Other view functions are the following:

### Stake

Function to get the stake of the strategist in the strategy.

```
function stake() external view returns (uint256);
```

### StrategyRewards

Function to get the assigned strategyRewards from BABL Mining program for the strategy.

```
function strategyRewards() external view returns (uint256);
```

### MaxCapitalRequested

Function to get the max capital requested for the strategy in reserveAsset.

```
function maxCapitalRequested() external view returns (uint256);
```

### MaxAllocationPercentage

Function to get the maximum capital allocation percentage (%).

```
function maxAllocationPercentage() external view returns (uint256);
```

### MaxTradeSlippagePercentage

Function to get the maximum trading slippage in % with 18 decimals precision.

```
function maxTradeSlippagePercentage() external view returns (uint256);
```

### MaxGasFeePercentage

Function to get the maximum gas fee percentage to limit the execution cost for the strategy-keeper.

```
function maxGasFeePercentage() external view returns (uint256);
```

### ExpectedReturn

Function to get the expected return or expected profits in % with 18 decimals precision (i.e. 5% 5e16)

```
function expectedReturn() external view returns (uint256);
```

### Duration

Function to get the duration of the strategy (in seconds).

```
function duration() external view returns (uint256);
```

### TotalPositiveVotes

Function to get the total positive votes of the strategy.

```
function totalPositiveVotes() external view returns (uint256);
```

### TotalNegativeVotes

Function to get the total negative votes of the strategy.

```
function totalNegativeVotes() external view returns (uint256);
```

### CapitalReturned

Function to get the capital returned in reserveAsset.

```
function capitalReturned() external view returns (uint256);
```

### CapitalAllocated

Function to get the capital allocated in reserveAsset.

```
function capitalAllocated() external view returns (uint256);
```

### Garden

Function to get the garden address the strategy belongs to.

```
function garden() external view returns (IGarden);
```

