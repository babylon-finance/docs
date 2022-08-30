# Custom Integrations

Interface: [ICustomIntegration.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/interfaces/ICustomIntegration.sol)

Base Classes: [CustomIntegration.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/integrations/custom/CustomIntegration.sol) [BaseIntegration.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/integrations/BaseIntegration.sol)

Template: [CustomIntegrationTemplate.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/integrations/custom/CustomIntegrationTemplate.sol)

Example (Yearn): [CustomIntegrationYearn.sol](https://github.com/babylon-finance/core-interfaces/blob/main/contracts/integrations/custom/CustomIntegrationYearn.sol)

## Functions

The following functions can be called by anyone. Strategy operations call it to execute them within its context.

```
function enter(
    address _strategy,
    bytes calldata _data,
    uint256 _resultTokensOut,
    address[] memory _inputTokens,
    uint256[] memory _maxAmountsIn
) external;

function exit(
    address _strategy,
    bytes calldata _data,
    uint256 _resultTokensIn,
    address[] memory _inputTokens,
    uint256[] memory _minAmountsOut
) external;

function isValid(bytes calldata _data) external view returns (bool);

function getInputTokensAndWeights(bytes calldata _data) external view returns (address[] memory, uint256[] memory);

function getResultToken(address _data) external view returns (address);

function getPriceResultToken(bytes calldata _data, address _tokenAddress) external view returns (uint256);

function getOutputTokensAndMinAmountOut(address _strategy, bytes calldata _data, uint256 _resultTokenAmount)
    external
    view
    returns (address[] memory exitTokens, uint256[] memory _minAmountsOut);

function getRewardTokens(bytes calldata _data) external view returns (address[] memory);
```
