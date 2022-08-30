# BabController

Deployment Address: [0xd4a5b5fcb561daf3adf86f8477555b92fba43b5f](https://etherscan.io/address/0xd4a5b5fcb561daf3adf86f8477555b92fba43b5f)

Interface: [IBabController.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/interfaces/IBabController.sol)

## Functions

The following functions can be called by any user or public roles.

### CreateGarden

_Public: Anyone can call this function._

This function creates a new garden and adds it to the Babylon controller.

```
function createGarden(
    address _reserveAsset,
    string memory _name,
    string memory _symbol,
    string memory _tokenURI,
    uint256 _seed,
    uint256[] calldata _gardenParams,
    uint256 _initialContribution,
    bool[] memory _publicGardenStrategistsStewards,
    uint256[] memory _profitSharing
) external payable returns (address);
```

{% hint style="info" %}
Creator must have called ERC-20 approve with the reserve asset and initial contribution on the controller address.
{% endhint %}

| Parameter                                        | Description                                                                                                                                                                                                                          |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `address  _reserveAsset`                         | Address of the reserve asset. WETH, DAI, AAVE, USDC & WBTC are supported.                                                                                                                                                            |
| `string memory _name`                            | Name of the Garden                                                                                                                                                                                                                   |
| `string memory _symbol`                          | ERC-20 symbol of the Garden token                                                                                                                                                                                                    |
| `string memory _tokenURI`                        | Token URI of the membership NFT                                                                                                                                                                                                      |
| `uint256 _seed`                                  | Seed to retrieve the garden NFT                                                                                                                                                                                                      |
| `uint256[] calldata _gardenParams`               | Array of 12 garden parameters. See  below.                                                                                                                                                                                           |
| `uint256 _initialContribution`                   | Amount of reserve asset to be deposited by the creator                                                                                                                                                                               |
| `bool[] memory _publicGardenStrategistsStewards` | Three booleans. First one specifies whether the garden is public or private. Second one specifies whether or not anyone can create strategies. Third one specifies whether or not anyone can vote to approve strategies.             |
| `uint256[] memory _profitSharing`                | Three numbers, 18 decimals each. 1e18 means 100%. The three numbers must add up to 95%. They represent the strategist share of profits, steward share of profits and lp share of profits. You can pass three 0s to use the defaults. |

Definition of the garden parameters:

```

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

#### RemoveGarden

_Only Owner: Only the garden owner can call this function._

This function removes a garden from the controller. The Garden won't be able to execute any more strategies. Garden can only be removed if there are no active strategies.

```
function removeGarden(address _garden) external;
```

| Parameter         | Description                     |
| ----------------- | ------------------------------- |
| `address _garden` | Address of the garden to remove |

### ClaimRewards

_Public: Anyone can call this function_

This function adds a referral amount earned by a depositor and referrer.

```
function claimRewards() external;
```

## Privileged Functions

### AddReserveAsset

_Only Governance or team multisig can call this function_

This function adds a new reserve asset to the controller

```
function addReserveAsset(address _reserveAsset) external;
```

| Parameter               | Description                         |
| ----------------------- | ----------------------------------- |
| `address _reserveAsset` | Address of the reserve asset to add |

### RemoveReserveAsset

_Only Governance or team multisig can call this function_

This function removes a reserve asset from the controller

```
function removeReserveAsset(address _reserveAsset) external;
```

| Parameter               | Description                            |
| ----------------------- | -------------------------------------- |
| `address _reserveAsset` | Address of the reserve asset to remove |

### UpdateProtocolWantedAsset

_Only Governance or team multisig can call this function_

This function indicates the heart to purchase an asset to keep it under PCV.

```
function updateProtocolWantedAsset(address _wantedAsset, bool _wanted) external;
```

| Parameter              | Description                                                           |
| ---------------------- | --------------------------------------------------------------------- |
| `address _wantedAsset` | Address of the asset to purchase                                      |
| `bool _wanted`         | Boolean indicating whether the asset should be purchased by the heart |

### UpdateGardenAffiliateRate

_Only Governance or team multisig can call this function_

This function sets a referral program for a specific garden

```
function updateGardenAffiliateRate(address _garden, uint256 _affiliateRate) external;
```

| Parameter                | Description                                                            |
| ------------------------ | ---------------------------------------------------------------------- |
| `address _garden`        | Address of the garden                                                  |
| `uint256 _affiliateRate` | Amount of BABL to award for the referral for unit of the reserve asset |

### AddAffiliateReward

_Only Garden contracts can call this function_

This function adds a referral amount earned by a depositor and referrer.

```
function addAffiliateReward(
    address _depositor,
    address _referrer,
    uint256 _reserveAmount
) external;
```

| Parameter                | Description                                    |
| ------------------------ | ---------------------------------------------- |
| `address _depositor`     | Address of the depositor                       |
| `address _referrer`      | Address of the referrer                        |
| `uint256 _reserveAmount` | Amount of the reserve asset that was deposited |

### EditPriceOracle

_Only governance or team multisig can call this function._

This function edits the price oracle of the protocol.

```
function editPriceOracle(address _priceOracle) external;
```

| Parameter              | Description                     |
| ---------------------- | ------------------------------- |
| `address _priceOracle` | Address of the new price oracle |

### EditMardukGate

_Only governance can call this function._

This function edits the marduk gate (whitelist checker).

```
function editMardukGate(address _mardukGate) external;
```

| Parameter             | Description                    |
| --------------------- | ------------------------------ |
| `address _mardukGate` | Address of the new marduk gate |

### EditGardenValuer

_Only governance or team multisig can call this function._

This function edits the contract that calculates the price of a garden token.

```
function editGardenValuer(address _gardenValuer) external;
```

| Parameter               | Description                      |
| ----------------------- | -------------------------------- |
| `address _gardenValuer` | Address of the new garden valuer |

### EditTreasury

_Only governance can call this function._

This function edits the address of the treasury.

```
function editTreasury(address _newTreasury) external;
```

| Parameter              | Description                 |
| ---------------------- | --------------------------- |
| `address _newTreasury` | Address of the new treasury |

### EditHeart

_Only governance or team multisig can call this function._

This function edits the address of the heart.

```
function editHeart(address _newHeart) external;
```

| Parameter           | Description              |
| ------------------- | ------------------------ |
| `address _newHeart` | Address of the new heart |

### EditRewardsDistributor

_Only governance can call this function._

This function edits the address of the reward distributor.

```
function editRewardsDistributor(address _rewardsDistributor) external;
```

| Parameter                      | Description                           |
| ------------------------------ | ------------------------------------- |
| `address` \_rewardsDistributor | Address of the new reward distributor |

### EditGardenFactory

_Only governance or multisig can call this function._

This function edits the address of the garden factory contract.

```
function editGardenFactory(address _newGardenFactory) external;
```

| Parameter                   | Description                       |
| --------------------------- | --------------------------------- |
| `address _newGardenFactory` | Address of the new garden factory |

### EditGardenNFT

_Only governance or multisig can call this function._

This function edits the address of the garden NFT template contract.

```
function editGardenNFT(address _newGardenNFT) external;
```

| Parameter               | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `address _newGardenNFT` | Address of the new garden NFT template contract |

### EditStrategyNFT

_Only governance or multisig can call this function._

This function edits the address of the strategy NFT template contract.

```
function editStrategyNFT(address _newStrategyNFT) external;
```

| Parameter                 | Description                                       |
| ------------------------- | ------------------------------------------------- |
| `address _newStrategyNFT` | Address of the new strategy NFT template contract |

### EditStrategyFactory

_Only governance or multisig can call this function._

This function edits the address of the strategy factory template contract.

```
function editStrategyFactory(address _newStrategyFactory) external;
```

| Parameter                     | Description                                       |
| ----------------------------- | ------------------------------------------------- |
| `address _newStrategyFactory` | Address of the new strategy NFT template contract |

### SetOperation

_Only governance or multi-sig can call this function._

This function edits an existing operation in the registry.&#x20;

```
function setOperation(uint8 _kind, address _operation) external;
```

| Parameter            | Description                               |
| -------------------- | ----------------------------------------- |
| `uint8 _kind`        | Operation kind.                           |
| `address _operation` | Address of the operation contract to set. |

### SetMasterSwapper

_Only governance or multi-sig can call this function._

This function allows governance to edit the protocol default trade integration

```
function setMasterSwapper(address _newMasterSwapper) external;
```

### AddKeeper

_Only governance or multi-sig can call this function._

This function adds a new valid keeper to the list.

```
function addKeeper(address _keeper) external;
```

### AddKeepers

_Only governance or multi-sig can call this function._

This function adds an array of new valid keepers to the list.

```
function addKeepers(address[] memory _keepers) external;
```

### RemoveKeeper

_Only governance or multi-sig can call this function._

This function remove a keeper from the keeper's list.

```
function removeKeeper(address _keeper) external;
```

### EnableGardenTokensTransfers

_Only governance can call this function._

This function allows transfers of ERC20 gardenTokens.

```
function enableGardenTokensTransfers() external;
```

### EditLiquidityReserve

_Only governance can call this function._

This function edits the minimum liquidity an asset must have on Uniswap.

```
function editLiquidityReserve(address _reserve, uint256 _minRiskyPairLiquidityEth) external;
```

### PatchIntegration

_Only governance or multi-sig can call this function._

This function replaces old integration with a new one for all the strategies using this integration.

```
function patchIntegration(address _old, address _new) external;
```

### SetPauseGuardian

_Only governance or current pause guardian can call this function._

This function

Other privileged internal functions are:

```








function setPauseGuardian(address _guardian) external;

function setGlobalPause(bool _state) external returns (bool);

function setSomePause(address[] memory _address, bool _state) external returns (bool);
```

## View Functions

The following functions can be called by anyone without a transaction to retrieve information from the controller.

```
function gardenCreationIsOpen() external view returns (bool);

function owner() external view returns (address);

function EMERGENCY_OWNER() external view returns (address);

function guardianGlobalPaused() external view returns (bool);

function guardianPaused(address _address) external view returns (bool);

function isPaused(address _contract) external view returns (bool);

function priceOracle() external view returns (address);

function gardenValuer() external view returns (address);

function heart() external view returns (address);

function gardenNFT() external view returns (address);

function strategyNFT() external view returns (address);

function rewardsDistributor() external view returns (address);

function gardenFactory() external view returns (address);

function treasury() external view returns (address);

function ishtarGate() external view returns (address);

function mardukGate() external view returns (address);

function strategyFactory() external view returns (address);

function masterSwapper() external view returns (address);

function gardenTokensTransfersEnabled() external view returns (bool);

function bablMiningProgramEnabled() external view returns (bool);

function allowPublicGardens() external view returns (bool);

function enabledOperations(uint256 _kind) external view returns (address);

function getGardens() external view returns (address[] memory);

function getReserveAssets() external view returns (address[] memory);

function getOperations() external view returns (address[20] memory);

function isGarden(address _garden) external view returns (bool);

function protocolWantedAssets(address _wantedAsset) external view returns (bool);

function gardenAffiliateRates(address _wantedAsset) external view returns (uint256);

function affiliateRewards(address _user) external view returns (uint256);

function patchedIntegrations(address _integration) external view returns (address);

function isValidReserveAsset(address _reserveAsset) external view returns (bool);

function isValidKeeper(address _keeper) external view returns (bool);

function isSystemContract(address _contractAddress) external view returns (bool);

function protocolPerformanceFee() external view returns (uint256);

function protocolManagementFee() external view returns (uint256);

function minLiquidityPerReserve(address _reserve) external view returns (uint256);
```
