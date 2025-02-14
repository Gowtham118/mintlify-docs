---
hidden: true
---

# Utility Types

Here's a list of all utility types that are used in Fund Kit Widget

### DepositWidgetSDK

| Property/Method                   | Type                                                                                                                             | Description                          |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| config                            | FKConfig                                                                                                                         | Configuration object for the widget  |
| navigation                        | FKNavigation                                                                                                                     | Navigation control for the widget    |
| reset                             | () => void                                                                                                                       | Resets the widget state              |
| getMeshIntegrationPlaforms        | () => Promise                                                                                                                    | Retrieves mesh integration platforms |
| openMeshLink                      | (integrationId?: string) => Promise                                                                                              | Opens a mesh link                    |
| openOnRampFlowViaUrl              | () => Promise                                                                                                                    | Opens the on-ramp flow via URL       |
| searchSupportedTokens             | (searchString?: string, address?: string, chainId?: string) => Promise\<SupportedToken\[]>                                       | Searches for supported tokens        |
| updateDestinationTokenWithAddress | (tokenAddress: string, chainId: string, tokenAmount?: string) => Promise                                                         | Updates the destination token        |
| getSupportedTokens                | () => Promise\<SupportedToken\[]>                                                                                                | Retrieves all supported tokens       |
| getSupportedChains                | () => Promise\<ChainInfo\[]>                                                                                                     | Retrieves all supported chains       |
| getMultichainBalance              | (address: string, extendedBalances?: boolean) => Promise                                                                         | Retrieves multichain balance         |
| getTokenPrice                     | (tokenSymbol?: string) => Promise                                                                                                | Retrieves token price                |
| getCheckoutRoute                  | () => Promise                                                                                                                    | Retrieves checkout route             |
| getDepositRoute                   | () => Promise\<RouteResponse                                                                                                     | undefined>                           |
| handleApprove                     | ({ approvalTxs }: { approvalTxs: ApprovalTransaction\[] }) => Promise\<RelayedTxListResponse\[]>                                 | Handles approval transactions        |
| handleExecute                     | ({ executionTxs, provider }: { executionTxs: ExecutionTransaction\[], provider?: string }) => Promise\<RelayedTxListResponse\[]> | Handles execution transactions       |
| getTransactionStatus              | (txHash: string) => Promise                                                                                                      | Retrieves transaction status         |
| addTxMetaDetails                  | (data: TxMetaDetails) => void                                                                                                    | Adds transaction meta details        |
| updateChainId                     | (chainInfo: ChainInfo) => void                                                                                                   | Updates the chain ID                 |
| goToPreviousStep                  | () => void                                                                                                                       | Navigates to the previous step       |
| updateDestinationContract         | (contract: FKDestination\["contract"]) => void                                                                                   | Updates the destination contract     |
| updateDestinationToken            | (token: SupportedToken, chainInfo?: ChainInfo) => void                                                                           | Updates the destination token        |

### SupportedToken

| Property | Type       | Description                           |
| -------- | ---------- | ------------------------------------- |
| address  | string     | Token address                         |
| chainId  | number     | Chain ID where the token is supported |
| name     | string     | Token name                            |
| symbol   | string     | Token symbol                          |
| decimals | number     | Number of decimal places              |
| logoURI  | string     | URI of the token logo                 |
| priority | number?    | Priority of the token                 |
| source   | string\[]? | Sources supporting the token          |

### ChainInfo

| Property | Type    | Description           |
| -------- | ------- | --------------------- |
| chainId  | string  | Chain ID              |
| name     | string  | Chain name            |
| logoURI  | string  | URI of the chain logo |
| priority | number? | Priority of the chain |

### FormattedToken

<table><thead><tr><th width="218">Property</th><th width="209">Type</th><th>Description</th></tr></thead><tbody><tr><td>formattedBalance</td><td>string</td><td>Formatted token balance</td></tr><tr><td>formattedUSDValue</td><td>string</td><td>Formatted USD value of the token</td></tr><tr><td>contract</td><td>string?</td><td>Contract address</td></tr><tr><td>name</td><td>string</td><td>Token name</td></tr><tr><td>symbol</td><td>string</td><td>Token symbol</td></tr><tr><td>address</td><td>string</td><td>Token address</td></tr><tr><td>token_address</td><td>string</td><td>Token address (duplicate?)</td></tr><tr><td>decimals</td><td>number</td><td>Number of decimal places</td></tr><tr><td>logo</td><td>string</td><td>URI of the token logo</td></tr><tr><td>native_token</td><td>boolean</td><td>Whether it's a native token</td></tr><tr><td>type</td><td>string</td><td>Token type</td></tr><tr><td>is_spam</td><td>boolean</td><td>Whether the token is considered spam</td></tr><tr><td>balance</td><td>string</td><td>Raw balance</td></tr><tr><td>usd_price</td><td>number</td><td>USD price of the token</td></tr><tr><td>sourceAmount</td><td>string?</td><td>Source amount</td></tr><tr><td>chainId</td><td>string</td><td>Chain ID</td></tr><tr><td>chain</td><td>{ id: string, logo: string }</td><td>Chain information</td></tr></tbody></table>

