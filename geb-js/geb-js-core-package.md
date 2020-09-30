# Core GEB.js Package

The main package used to interact with the core GEB contracts.

## Constructors

+ **new Geb**\(`network`: GebDeployment, `provider`: GebProviderInterface \| Provider\): [_Geb_](geb-js-core-package.md)

_Defined in_ [_packages/geb/src/geb.ts:50_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L50)

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `network` | GebDeployment | Either `'kovan'`, `'mainnet'` or an actual list of contract addresses. |
| `provider` | GebProviderInterface \| Provider | Either a Ethers.js provider or a Geb provider \(Soon support for Web3 will be added\) |

**Returns:** [_Geb_](geb-js-core-package.md)

## Properties

### contracts

• **contracts**: _ContractApis_

_Defined in_ [_packages/geb/src/geb.ts:48_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L48)

Object containing all GEB core contracts instances for low level interactions. All contracts object offer a one-to-one typed API to the underlying smart-contract. Currently has the following contracts:

* SAFEEngine
* AccountingEngine
* TaxCollector
* LiquidationEngine
* OracleRelayer
* GlobalSettlement
* DebtAuctionHouse
* PreSettlementSurplusAuctionHouse
* PostSettlementSurplusAuctionHouse
* SettlementSurplusAuctioneer
* GebSafeManager
* GetSafes
* BasicCollateralJoin
* CoinJoin
* Coin \(RAI ERC20 contract\)
* GebProxyRegistry
* FixedDiscountCollateralAuctionHouse \(For ETH-A\)
* Weth \(ERC20\)

## Methods

### deployProxy

▸ **deployProxy**\(\): _TransactionRequest_

_Defined in_ [_packages/geb/src/geb.ts:111_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L111)

Deploy a new proxy owned by the sender.

**Returns:** _TransactionRequest_

### getErc20Contract

▸ **getErc20Contract**\(`tokenAddress`: string\): _Erc20_

_Defined in_ [_packages/geb/src/geb.ts:206_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L206)

Returns an object that can be used to interact with a ERC20 token. Example:

```typescript
const USDCAddress = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const USDC = geb.getErc20Contract(USDCAddress)

// Get 0xdefiisawesome's balance
const balance = USDC.balanceOf("0xdefiisawesome..")

// Send 1 USDC to 0xdefiisawesome (USDC is 6 decimals)
const tx = USDC.transfer("0xdefiisawesome..", "1000000")
await wallet.sendTransaction(tx)
```

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `tokenAddress` | string | Token contract address |

**Returns:** _Erc20_

Erc20

### getProxyAction

▸ **getProxyAction**\(`ownerAddress`: string\): _Promise‹_[_GebProxyActions_](geb-js-proxy-actions.md)_‹››_

_Defined in_ [_packages/geb/src/geb.ts:81_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L81)

Given an address returns a GebProxyActions object to execute bundled operations. Important: This requires the address to have deployed a GEB proxy through the proxy registry contract. It will throw a `DOES_NOT_OWN_HAVE_PROXY` error if the address specified does not have a proxy. Use the [deployProxy](geb-js-core-package.md#deployproxy) function to get a new proxy.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹_[_GebProxyActions_](geb-js-proxy-actions.md)_‹››_

### getProxyActionGlobalSettlement

▸ **getProxyActionGlobalSettlement**\(`ownerAddress`: string\): _Promise‹_[_GebProxyActionsGlobalSettlement_](geb-js-global-settlement-proxies.md)_‹››_

_Defined in_ [_packages/geb/src/geb.ts:95_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L95)

Given an address returns a GebProxyActionsGlobalSettlement object to execute bundled operations during GlobalSettlement. **IMPORTANT**: Same as for `getProxyAction` you will need to deploy a proxy beforehand using the proxy registry.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `ownerAddress` | string | Externally owned user account, Ethereum address that owns a GEB proxy. |

**Returns:** _Promise‹_[_GebProxyActionsGlobalSettlement_](geb-js-global-settlement-proxies.md)_‹››_

### getSafe

▸ **getSafe**\(`idOrHandler`: string \| number, `collateralType?`: string\): _Promise‹_[_Safe_](geb-js-safe-management.md)_‹››_

_Defined in_ [_packages/geb/src/geb.ts:119_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L119)

Get the SAFE object

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `idOrHandler` | string \| number | Safe Id or SAFE handler |
| `collateralType?` | string | - |

**Returns:** _Promise‹_[_Safe_](geb-js-safe-management.md)_‹››_

### multiCall

▸ **multiCall**‹**O1**, **O2**, **O3**›\(`calls`: \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\]\): _Promise‹\[O1, O2, O3\]›_

_Defined in_ [_packages/geb/src/geb.ts:219_](https://github.com/reflexer-labs/geb.js/blob/198bcb4/packages/geb/src/geb.ts#L219)

Bundles several read only GEB contract call into 1 RPC single request. Useful for front-ends or apps that need to fetch many parameters from the contracts but want to minimize the network request and the load on the underlying Ethereum node. The function takes as input an Array of GEB view contract calls. **IMPORTANT**: You have to set the `multicall` parameter of the contract function to `true`, it is the always the last parameter of the function. Multicall works for all contracts in the `Geb.contracts` and can be use with any contract that inherit the `BaseContractApi`. Note that it does not support non-view calls \(Calls that require to pay gas and change the state of the blockchain\).

Example:

```typescript
import { ethers } from "ethers"
import { Geb } from "geb.js"

const provider = new ethers.providers.JsonRpcProvider("http://kovan.infura.io/...")
const geb = new Geb("kovan", provider);

const [ globalDebt, collateralInfo ] = await geb.multiCall([
    geb.contracts.safeEngine.globalDebt(true), // !! Note the last parameter set to true.
    geb.contracts.safeEngine.collateralTypes(ETH_A, true),
])

console.log(`Current global debt: ${globalDebt.toString()}`)
console.log(`Current ETH_A debt: ${collateralInfo.debtAmount}`)
```

**Type parameters:**

▪ **O1**

▪ **O2**

▪ **O3**

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| `calls` | \[MulticallRequest‹O1›, MulticallRequest‹O2›, MulticallRequest‹O3›\] | Call a read only GEB contract function. The GEB contract object needs to be called with the parameter `multicall` set to `true` as seen in the example above. |

**Returns:** _Promise‹\[O1, O2, O3\]›_

Promise Array with the result from their respective requests.
