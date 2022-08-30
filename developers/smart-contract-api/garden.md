# Garden

Interface: [IGarden.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/interfaces/IGarden.sol)

## ERC-20 Functions

Gardens are fully composable (ERC-20). That means all the ERC-20 functions are available.

You can see all of them [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol).

## Functions

The following functions can be called by public roles.

### Deposit

_Anyone can call this function for public gardens. In case of private gardens the user requires specific permissions to deposit._

This function allow a user to deposit the `_amountIn` of reserve asset into the garden.&#x20;

```
  function deposit(
        uint256 _amountIn,
        uint256 _minAmountOut,
        address _to,
        address _referrer
    ) external payable;    
```

| Parameter               | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| `uint256 _amountIn`     | Amount of the reserve asset to deposit by the contributor |
| `uint256 _minAmountOut` | Min amount of Garden shares to receive by contributor     |
| `address _to`           | Address to mint Garden shares to                          |
| `address _referrer`     | The user that referred the deposit                        |

### Withdraw

_Only garden members: Any garden member can call this function._

This function withdraws the reserve asset based on the garden tokens passed as `_amountIn` in the garden and sends it back to the `_to` address.&#x20;

```
 function withdraw(
        uint256 _amountIn,
        uint256 _minAmountOut,
        address payable _to,
        bool _withPenalty,
        address _unwindStrategy
    ) external;   
```

| Parameter                 | Description                                                     |
| ------------------------- | --------------------------------------------------------------- |
| `uint256 _amountIn`       | Quantity of the garden token to withdraw                        |
| `uint256 _minAmountOut`   | Min quantity of reserve asset to receive                        |
| `address payable _to`     | Address to send component assets to                             |
| `bool _withPenalty`       | Whether or not this is a withdrawal that triggers a liquidation |
| `address _unwindStrategy` | Strategy to unwind if any. Zero address otherwise               |

### ClaimReturns

_Only garden members: Any garden member can call this function._

This function allows a user to claim the rewards from the strategies that were active while the user was a member of the garden.&#x20;

```
function claimReturns(address[] calldata _finalizedStrategies) external;    
```

| Parameter                                 | Description                                         |
| ----------------------------------------- | --------------------------------------------------- |
| `address[] calldata _finalizedStrategies` | Array of finalized strategies to claim rewards from |

{% hint style="info" %}
It must include all finalized strategies since the last claim.
{% endhint %}

### ClaimAndStakeReturns

_Only garden members: Any garden member can call this function._

This function allows a user to claim the rewards from the strategies that executed while a member of the garden and stakes these rewards into the Heart Garden.

```
function claimAndStakeReturns(uint256 _minAmountOut, address[] calldata _finalizedStrategies) external;    
```

| Parameter                                 | Description                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| `uint256 _minAmountOut`                   | Min quantity of Heart Garden tokens (hBABL) to receive        |
| `address[] calldata _finalizedStrategies` | Array of finalized strategies to claim and stake rewards from |

### ClaimNFT

_Any garden member can call this function._

This __ function allows any garden member to claim a garden NFT.

```
    function claimNFT() external;
```

### MakeGardenPublic

_Only a garden creator can call this function._

The function makes public a previously private garden.

```
function makeGardenPublic() external;
```

### TransferCreatorRights

_Only a garden creator can call this function._

The function allows a garden creator to transfer his creator rights to another account. The original creator can also renounce its creator role by assigning it to address(0) in public gardens.

```
function transferCreatorRights(address _newCreator, uint8 _index) external;    
```

| Parameter             | Description                                                          |
| --------------------- | -------------------------------------------------------------------- |
| `address _newCreator` | New creator address                                                  |
| `uint8 _index`        | Index of the extra creator in the array of 4 extra-creators (if any) |

### AddExtraCreators

_Only the original garden creator can call this function._

The function adds extra creators. Can only be called if all the addresses in the extra-creators array are zero (0x).   &#x20;

```
function addExtraCreators(address[4] memory _newCreators) external;
```

| Parameter                        | Description                         |
| -------------------------------- | ----------------------------------- |
| address\[4] memory \_newCreators | Addresses of the new extra creators |

### ResetHardlock

_Only a garden creator can call this function._

This function allows the creator to reset the garden hardlock for all users.

```
function resetHardlock(uint256 _hardlockStartsAt) external;
```

| Parameter                   | Description                              |
| --------------------------- | ---------------------------------------- |
| `uint256 _hardlockStartsAt` | New global hardlock starts at timestamp. |



### UpdateCreators

_Only Governance or team multi-sig can call this function._

The function can update garden owners (original owner and extra creators) only if the original creator renounced (creator == address(0)). &#x20;

```
function updateCreators(address _newCreator, address[4] memory _newCreators) external;
```

| Parameter                        | Description                         |
| -------------------------------- | ----------------------------------- |
| `address _newCreator`            | New creator address                 |
| `address[4] memory _newCreators` | Addresses of the new extra creators |

### SetPublicRights

_Only a garden creator can call this function._

The function gives the right to create strategies and/or voting power to garden users in a public garden.

```
function setPublicRights(bool _publicStrategist, bool _publicStewards) external;
```

| Parameter                | Description                                                              |
| ------------------------ | ------------------------------------------------------------------------ |
| `bool _publicStrategist` | Whether or not all garden members will get be able to suggest strategies |
| `bool _publicStewards`   | Whether or not all garden members will be able to vote on proposals      |

### DelegateVotes

_Only a garden creator can call this function._

The function allows a garden holding a governance token to delegate its voting power into a delegatee.

```
function delegateVotes(address _token, address _delegatee) external;
```

| Parameter            | Description                                                     |
| -------------------- | --------------------------------------------------------------- |
| `address _token`     | Address of BABL or any other ERC20Comp related governance token |
| `address _delegatee` | Address to delegate token voting power into                     |

### UpdateGardenParams

_Only a garden creator can call this function._

The function updates relevant garden params.

```
function updateGardenParams(uint256[12] memory _newParams) external;
```

| Parameter                       | Description            |
| ------------------------------- | ---------------------- |
| `uint256[12] memory _newParams` | New params. See below. |

Definition of the garden parameters:

```
_newParams[0...11] are described in order:

 * @param _maxDepositLimit             Max deposit limit in reserve asset units
 * @param _minLiquidityAsset           Number that represents min amount of liquidity denominated in reserve asset
 * @param _depositHardlock             Number that represents the time deposits are locked for
 *                                     an user after the deposit. In seconds.
 * @param _minContribution             Min contribution to the garden in reserve asset units
 * @param _strategyCooldownPeriod      How long after the strategy has been activated, will it be ready
 *                                     to be executed. In seconds.
 * @param _minVotesQuorum              Percentage of votes needed to activate an strategy (0.01% = 1e14, 1% = 1e16)
 * @param _minStrategyDuration         Min duration of an strategy. In seconds.
 * @param _maxStrategyDuration         Max duration of an strategy. In seconds.
 * @param _minVoters                   The minimum amount of voters needed for quorum.
 * @param _pricePerShareDecayRate      Decay rate of price per share. 1e18= 100%
 * @param _pricePerShareDelta          Base slippage for price per share. 1e18= 100%
 * @param _canMintNftAfter             Can mint NFT after being a member for X secs.
 
```

### AddStrategy

_Only a strategist of a garden can call this function._

This function creates a new strategy calling the factory and adds it to the array.

```
function addStrategy(
        string memory _name,
        string memory _symbol,
        uint256[] calldata _stratParams,
        uint8[] calldata _opTypes,
        address[] calldata _opIntegrations,
        bytes calldata _opEncodedDatas
) external;
```

| Parameter                            | Description                                                          |
| ------------------------------------ | -------------------------------------------------------------------- |
| `string memory _name`                | Name of the strategy                                                 |
| `string memory _symbol`              | Symbol of the strategy                                               |
| `uint256[] calldata _stratParams`    | Num params for the strategy                                          |
| `uint8[] callData _opTypes`          | Type for every operation in the strategy                             |
| `address[] calldata _opIntegrations` | Integrations to pass for each operation in order. One per operation. |
| `bytes calldata _opEncodedDatas`     | 64 bytes of operations metadata per operation (size of 64 \* numOps) |

Definition of the strategy parameters:

```
_stratParams[0]: maxCapitalRequested
_stratParams[1]: stake, 
_stratParams[2]: duration,
_stratParams[3]: expectedReturn,
_stratParams[4]: maxPercentAllocation,
_stratParams[5]: maxGasFeePercentage,
_stratParams[6]: maxSlippagePercentage,    
```

Definition of operation types (opTypes):

```
// 0 = BuyOperation
// 1 = LiquidityOperation
// 2 = VaultOperation
// 3 = LendOperation
// 4 = BorrowOperation
// 5 = CustomOperation
```

Integration addresses can be found in the [deployments](../deployments.md) section.

### ExpireCandidateStrategy

_Only the strategist can call this function._

The function removes an expired candidate strategy from the strategy array.

```
function expireCandidateStrategy() external;
```

## Privileged Functions

The following functions can only be executed by protocol privileged roles.

### FinalizeStrategy

_Only a strategy can call this function._

When the strategy ends, this function saves returns and rewards, and marks the strategy as finalized.

```
function finalizeStrategy(
    uint256 _rewards,
    int256 _returns,
    uint256 _burningAmount
) external;
```

| Parameter                | Description                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------- |
| `uint256 _rewards`       | Amount of Reserve Asset profits to set aside for strategist and stewards roles (if any) |
| `int256 _returns`        | Strategy returns. It can include profits or losses.                                     |
| `uint256 _burningAmount` | The amount of strategist stake to burn in case of strategy losses.                      |

### AllocateCapitalToStrategy

_Only a strategy can call this function._

This function allocates garden capital to a strategy.

```
function allocateCapitalToStrategy(uint256 _capital) external;
```

| Parameter          | Description                                    |
| ------------------ | ---------------------------------------------- |
| `uint256 _capital` | Amount of capital to allocate to the strategy. |

### PayKeeper

_Only the garden or a strategy can call this function._

_The function p_ays gas costs back to the keeper from executing transactions including any past debt.

```
function payKeeper(address payable _keeper, uint256 _fee) external;
```

| Parameter                 | Description                                       |
| ------------------------- | ------------------------------------------------- |
| `address payable _keeper` | Keeper that executed the transaction              |
| `uint256 _fee`            | The fee paid to keeper to compensate the gas cost |

### VerifyGarden

_Only governance or team multi-sig can call this function._

This function can mark a garden as verified.

```
function verifyGarden(uint256 _verifiedCategory) external;
```

| Parameter                   | Description                                                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------ |
| `uint256 _verifiedCategory` | 1 for verified gardens, 0 otherwise. Additional categories will use different numbering in the future. |

### DepositBySig

_Only the protocol Keeper can call this function._

This function allows the user to deposit into a garden by using [signature-based gas-less transactions (meta-tx)](https://medium.com/babylon-finance/introducing-gasless-deposits-51e2d31ccd3f).

```
  function depositBySig(
        uint256 _amountIn,
        uint256 _minAmountOut,
        uint256 _nonce,
        uint256 _maxFee,
        address _to,
        uint256 _pricePerShare,
        uint256 _fee,
        address _signer,
        address _referrer,
        bytes memory signature
   ) external;
```

| Parameter                 | Description                                                                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uint256 _amountIn`       | Amount of the reserve asset that is received from contributor.                                                                                                                                            |
| `uint256 _minAmountOut`   | Min amount of Garden shares to receive by contributor.                                                                                                                                                    |
| `uint256 _nonce`          | Current nonce to prevent replay attacks.                                                                                                                                                                  |
| `uint256 _maxFee`         | Max fee user is willing to pay keeper. Fee is substracted from the user wallet approved amount unless it is subsidized totally or partially by the garden or treasury. Fee is expressed in reserve asset. |
| `uint256 _to`             | Address to mint shares to.                                                                                                                                                                                |
| `uint256 _pricePerShare`  | Price per share of the garden calculated off-chain by Keeper.                                                                                                                                             |
| `uint256 _fee`            | Actual fee keeper demands. Have to be less than \_maxFee.                                                                                                                                                 |
| `address _signer`         | The user to who signed the signature.                                                                                                                                                                     |
| `address _referrer`       | The user that referred the deposit.                                                                                                                                                                       |
| `bytes memory _signature` | Signature by the user to verify deposit params                                                                                                                                                            |

### WithdrawBySig

_Only the protocol Keeper can call this function._

This function allows the user to withdraw from a garden by using [signature based gas-less transaction (meta-tx)](https://medium.com/babylon-finance/introducing-gasless-deposits-51e2d31ccd3f).

```
function withdrawBySig(
        uint256 _amountIn,
        uint256 _minAmountOut,
        uint256 _nonce,
        uint256 _maxFee,
        bool _withPenalty,
        address _unwindStrategy,
        uint256 _pricePerShare,
        uint256 _strategyNAV,
        uint256 _fee,
        address _signer,
        bytes memory signature
 ) external;
```

| Parameter                 | Description                                                                                                             |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `uint256 _amountIn`       | Quantity of the garden tokens to withdraw.                                                                              |
| `uint256 _minAmountOut`   | Min quantity of reserve asset to receive.                                                                               |
| `uint256 _nonce`          | Current nonce to prevent replay attacks.                                                                                |
| `uint256 _maxFee`         | Max fee user is willing to pay keeper. Fee is substracted from the withdrawn amount. Fee is expressed in reserve asset. |
| `bool _withPenalty`       | Whether or not this is an immediate withdrawal.                                                                         |
| `address _unwindStrategy` | Strategy to unwind (if any)                                                                                             |
| `uint256 _pricePerShare`  | Price per share of the garden calculated off-chain by Keeper.                                                           |
| `uint256 _strategyNAV`    | NAV of the strategy to unwind                                                                                           |
| `uint256 _fee`            | Actual fee keeper demands. Have to be less than \_maxFee.                                                               |
| `address _signer`         | The user to who signed the signature.                                                                                   |
| `bytes memory _signature` | Signature by the user to verify deposit params                                                                          |

### ClaimRewardsBySig

_Only the protocol Keeper can call this function._

This function allows the user to claim their rewards of a garden by using [signature based gas-less transaction (meta-tx)](https://medium.com/babylon-finance/introducing-gasless-deposits-51e2d31ccd3f).

```
function claimRewardsBySig(
        uint256 _babl,
        uint256 _profits,
        uint256 _nonce,
        uint256 _maxFee,
        uint256 _fee,
        address _signer,
        bytes memory _signature
) external;
```

| Parameter                 | Description                                                                                                                        |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `uint256 _babl`           | BABL rewards from mining program.                                                                                                  |
| `uint256 _profits`        | Profit rewards in reserve asset.                                                                                                   |
| `uint256 _nonce`          | Current nonce to prevent replay attacks.                                                                                           |
| `uint256 _maxFee`         | Max fee user is willing to pay keeper. Fee is substracted from the user wallet approved amount. Fee is expressed in reserve asset. |
| `uint256 _fee`            | Actual fee keeper demands. Have to be less than \_maxFee.                                                                          |
| `address _signer`         | The user who signed the signature.                                                                                                 |
| `bytes memory _signature` | Signature by the user to verify claim params.                                                                                      |

### ClaimAndStakeRewardsBySig

_Only the protocol Keeper can call this function._

This function allows the user to claim their rewards of a garden and stake all BABL rewards into the Heart Garden by using [signature based gas-less transaction (meta-tx)](https://medium.com/babylon-finance/introducing-gasless-deposits-51e2d31ccd3f).

```
function claimAndStakeRewardsBySig(
        uint256 _babl,
        uint256 _profits,
        uint256 _minAmountOut,
        uint256 _nonce,
        uint256 _nonceHeart,
        uint256 _maxFee,
        uint256 _pricePerShare,
        uint256 _fee,
        address _signer,
        bytes memory _signature
) external;
```

| Parameter                 | Description                                                                                                                        |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `uint256 _babl`           | BABL rewards from mining program.                                                                                                  |
| `uint256 _profits`        | Profit rewards in reserve asset.                                                                                                   |
| `uint256 _minAmountOut`   | Minimum hBABL as part of the Heart Garden BABL staking.                                                                            |
| `uint256 _nonce`          | Current nonce of user in the claiming garden at to prevent replay attacks.                                                         |
| `uint256 _nonceHeart`     | Current nonce of user in Heart Garden to prevent replay attacks.                                                                   |
| `uint256 _maxFee`         | Max fee user is willing to pay keeper. Fee is substracted from the user wallet approved amount. Fee is expressed in reserve asset. |
| `uint256 _fee`            | Actual fee keeper demands. Have to be less than \_maxFee.                                                                          |
| `uint256 _pricePerShare`  | Price per share of Heart Garden.                                                                                                   |
| `address _signer`         | The user who signed the signature.                                                                                                 |
| `bytes memory _signature` | Signature by the user to verify claim and stake params.                                                                            |

### StakeBySig

_Only a garden can call this function._

This function allows a garden to stake user BABL rewards on behalf of the user into the Heart Garden by using [signature based gas-less transaction (meta-tx)](https://medium.com/babylon-finance/introducing-gasless-deposits-51e2d31ccd3f) as well as [EIP-1271.](https://eips.ethereum.org/EIPS/eip-1271)

```
function stakeBySig(
        uint256 _amountIn,
        uint256 _profits,
        uint256 _minAmountOut,
        uint256 _nonce,
        uint256 _nonceHeart,
        uint256 _maxFee,
        address _to,
        uint256 _pricePerShare,
        address _signer,
        bytes memory _signature
) external;
```

| Parameter                 | Description                                                                                                                        |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `uint256 _amountIn`       | Amount of BABL that is going to be staked by contributor                                                                           |
| `uint256 _profits`        | Profit rewards in reserve asset.                                                                                                   |
| `uint256 _minAmountOut`   | Minimum hBABL as part of the Heart Garden BABL staking.                                                                            |
| `uint256 _nonce`          | Current nonce of user in the claiming garden at to prevent replay attacks.                                                         |
| `uint256 _nonceHeart`     | Current nonce in heart garden to prevent replay attacks.                                                                           |
| `uint256 _maxFee`         | Max fee user is willing to pay keeper. Fee is substracted from the user wallet approved amount. Fee is expressed in reserve asset. |
| `address _to`             | Address to mint shares to.                                                                                                         |
| `uint256 _pricePerShare`  | Price per share of the garden calculated off-chain by Keeper.                                                                      |
| `address _signer`         | The garden who signed the stake operation using EIP-1271.                                                                          |
| `bytes memory _signature` | Signature by the user to verify claim and stake params.                                                                            |

## View Functions

The following functions can be called by anyone without a transaction to retrieve information from the garden.

### PrivateGarden

Function to check if the garden is private (true) or public (false).

```
function privateGarden() external view returns (bool);
```

### PublicStrategist

Function to check if all garden members have strategist role (true means that all garden members have strategist role).

```
function publicStrategists() external view returns (bool);
```

### PublicStewards

Function to check if all garden members have voting privileges (true means all garden members can vote).

```
function publicStewards() external view returns (bool);
```

### Controller

Function to retrieve the controller smart-contract address.

```
function controller() external view returns (IBabController);
```

### Creator

Function to retrieve the garden original creator address.

```
function creator() external view returns (address);
```

### IsGardenStrategy

Function to check whether or not the strategy belongs to the garden (true or false).

```
function isGardenStrategy(address _strategy) external view returns (bool);
```

### GetContributor

Function to retrieve contributor information.

```
function getContributor(address _contributor)
        external
        view
        returns (
            uint256 lastDepositAt,
            uint256 initialDepositAt,
            uint256 claimedAt,
            uint256 claimedBABL,
            uint256 claimedRewards,
            uint256 withdrawnSince,
            uint256 totalDeposits,
            uint256 nonce,
            uint256 lockedBalance
);
```

| Parameter                  | Description                               |
| -------------------------- | ----------------------------------------- |
| `address _contributor`     | Garden member address                     |
| `uint256 lastDepositAt`    | Timestamp of the last deposit             |
| `uint256 initialDepositAt` | Timestamp of the initial deposit          |
| `uint256 claimedAt`        | Timestamp of the last claim               |
| `uint256 claimedBABL`      | Total amount of claimed BABL              |
| `uint256 claimedRewards`   | Total amount of claimed rewards           |
| `uint256 withdrawnSince`   | Total amount of withdrawals               |
| `uint256 totalDeposits`    | Total amount of deposits                  |
| `uint256 nonce`            | Contributor nonce to avoid replay attacks |
| `uint256 lockedBalance`    | Locked balance of the contributor         |

### ReserveAsset

Function to get the address of the garden reserveAsset.

```
function reserveAsset() external view returns (address);
```

### VerifiedCategory

Function to get whether or not governance has verified the garden and the category.

```
function verifiedCategory() external view returns (uint256);
```

### HardlockStartsAt

Function to get the timestamp that overrides the `depositLock` with a global hard lock.

```
function hardlockStartsAt() external view returns (uint256);
```

### CanMintNftAfter

Function to get the delay required (in seconds) before a garden member can mint a garden NFT since its initial deposit.&#x20;

```
function canMintNftAfter() external view returns (uint256);
```

### TotalContributors

Function to get the total number of contributors in the garden.

```
function totalContributors() external view returns (uint256);
```

### GardenInitializedAt

Function to get the timestamp of the garden creation.

```
function gardenInitializedAt() external view returns (uint256);
```

### MinContribution

Function to get the minimum contribution amount required by the garden.

```
function minContribution() external view returns (uint256);
```

### DepositHardlock

Function to get the minimum required delay (in seconds) between a deposit and a withdrawal (i.e. 1 second to avoid flashloans).

```
function depositHardlock() external view returns (uint256);
```

### MinLiquidityAsset

Function to get the minimum liquidity required for an asset to be tradable by this garden.

```
function minLiquidityAsset() external view returns (uint256);
```

### MinStrategyDuration

Function to get the minimum strategy duration (in seconds).

```
function minStrategyDuration() external view returns (uint256);
```

### MaxStrategyDuration

Function to get the maximum strategy duration (in seconds).

```
function maxStrategyDuration() external view returns (uint256);
```

### ReserveAssetRewardsSetAside

Function to get the number of profits denominated in the reserve asset that are set aside (reserved) for strategies and voters. LP profit rewards are auto-compounded.&#x20;

```
function reserveAssetRewardsSetAside() external view returns (uint256);
```

### AbsoluteReturns

Function to get the garden absolute returns value.

```
function absoluteReturns() external view returns (int256);
```

### TotalStake

Function to get the total stake from strategists in all the strategies.

```
function totalStake() external view returns (uint256);
```

### MinVotesQuorum

Function to get the minimum quorum (1e18 = 100%) needed to approve and execute a strategy.

```
function minVotesQuorum() external view returns (uint256);
```

### MinVoters

Function to get the minimum voters to approve and execute a strategy.

```
function minVoters() external view returns (uint256);
```

### MaxDepositLimit

function to get the maximum deposit limit of a garden.

```
function maxDepositLimit() external view returns (uint256);
```

### StrategyCooldownPeriod

Function to get how long the strategy will wait in seconds to execute after being approved.

```
function strategyCooldownPeriod() external view returns (uint256);
```

### GetStrategies

Function to get an array of current strategies.

```
function getStrategies() external view returns (address[] memory);
```

### ExtraCreators

Function to get the address of an extra creator within the array of extraCreators. As param, it needs the index from 0 to 3 as there is a maximum of 4 extra creators.

```
function extraCreators(uint256 index) external view returns (address);
```

### GetFinalizedStrategies

Function to get an array of all finalized strategies.

```
function getFinalizedStrategies() external view returns (address[] memory);
```

### StrategyMapping

Function to check all active strategies in the garden.

```
function strategyMapping(address _strategy) external view returns (bool);
```

### KeeperDebt

Function to get current debt to be paid to the Keeper.

```
function keeperDebt() external view returns (uint256);
```

### TotalKeeperFees

Function to get the total fees paid to Keeper.

```
function totalKeeperFees() external view returns (uint256);
```

### LastPricePerShare

Function to get the last recorded price per share of the garden during deposit or withdrawal operation.

```
function lastPricePerShare() external view returns (uint256);
```

### LastPricePerShareTS

Function to get the last recorded time of the deposit or withdraw in seconds.

```
function lastPricePerShareTS() external view returns (uint256);
```

### PricePerShareDecayRate

Function to get the decay rate of the slippage for pricePerShare over time.

```
function pricePerShareDecayRate() external view returns (uint256);
```

### PricePerShareDelta

Function to get the base slippage for pricePerShare of the garden.

```
function pricePerShareDelta() external view returns (uint256);
```

### Name

Function to get the name of the garden token.

```
function name() external view returns (string memory);
```
