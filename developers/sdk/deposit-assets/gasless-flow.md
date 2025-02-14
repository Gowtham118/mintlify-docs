---
description: Transfer tokens from EOA to any receiver wallet address without gas fees.
hidden: true
---

# Gasless Flow

The **`executeMigrationGasless`**&#x6D;ethod in our SDK allows users to send tokens from an Externally Owned Account (EOA) to any designated receiver's wallet address without incurring gas fees.

## Expected parameters

**`executeMigrationGasless`** accepts the following parameters:

{% tabs %}
{% tab title="ethers.js" %}
<table><thead><tr><th>Parameter</th><th width="253">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>senderSigner</code></td><td>ethers.Signer</td><td>Signer object obtained from ethers</td></tr><tr><td><code>chainId</code></td><td>number</td><td>The chain id of the current network.</td></tr><tr><td><code>receiverAddress</code></td><td>string</td><td>The address of the recipient.</td></tr><tr><td><code>transferTokenDetails</code>[OPTIONAL]</td><td><code>TransferTokenDetails[]</code></td><td><p>The object specifies tokens with respective <strong>amounts</strong> to transfer or, in the case of NFTs <strong>tokenIds</strong> need to be </p><p>mentioned</p></td></tr></tbody></table>
{% endtab %}

{% tab title="viem" %}
<table><thead><tr><th>Parameter</th><th width="253">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>senderSigner</code></td><td>WalletClient</td><td>Wallet client object obtained from viem</td></tr><tr><td><code>chainId</code></td><td>number</td><td>The chain id of the current network.</td></tr><tr><td><code>account</code></td><td>JSON RPC Account</td><td>A JSON RPC account generated using viem and window.ethereum</td></tr><tr><td><code>receiverAddress</code></td><td>string</td><td>The address of the recipient.</td></tr><tr><td><code>transferTokenDetails</code>[OPTIONAL]</td><td><code>TransferTokenDetails[]</code></td><td><p>The object specifies tokens with respective <strong>amounts</strong> to transfer or, in the case of NFTs <strong>tokenIds</strong> need to be </p><p>mentioned</p></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

```
TransferTokenDetails {
    tokenAddress: string,
    amount?: string, // ERC20
    tokenIds?: string[] // ERC721
}
```

{% hint style="info" %}
**NOTE**: \
If **`transferTokenDetails`**&#x69;s not passed, then all the assets will be transferred.
{% endhint %}

## Function Call

### Transfer all tokens

{% tabs %}
{% tab title="ethers.js" %}
```typescript
await aarcSDK.executeMigrationGasless({
    senderSigner: signer,
    chainId: number,
    receiverAddress:"0x786E6045eacb96cAe0259cd761e151b68B85bdA7"
  });
```
{% endtab %}

{% tab title="viem" %}
```typescript
import { createWalletClient, custom } from 'viem'
import { mainnet } from 'viem/chains'

const walletClient = createWalletClient({
  chain: mainnet,
  transport: custom(window.ethereum!),
})
 
const [account] = await walletClient.getAddresses();

await aarcSDK.executeMigrationGasless({
    senderSigner: walletClient,
    chainId: number,
    receiverAddress:"0x786E6045eacb96cAe0259cd761e151b68B85bdA7",
    account: account
  });
```
{% endtab %}
{% endtabs %}

### Transfer-specific tokens

{% tabs %}
{% tab title="ethers.js" %}
```typescript
await aarcSDK.executeMigrationGasless({
    senderSigner: signer,
    receiverAddress:"0x786E6045eacb96cAe0259cd761e151b68B85bdA7",
    chainId: 137,
    transferTokenDetails:[
    {
        tokenAddress:"0x2271e3Fef9e15046d09E1d78a8FF038c691E9Cf9",
        // .toString(16) is to convert TOKEN_AMOUNT to hex in string format
        amount:TOKEN_AMOUNT.toString(16)   
    },
    {
      tokenAddress:"0x932ca55b9ef0b3094e8fa82435b3b4c50d713043",
      tokenIds:["28507"] 
     },
    ],
  });
```
{% endtab %}

{% tab title="viem" %}
```typescript
import { createWalletClient, custom } from 'viem'
import { mainnet } from 'viem/chains'

const walletClient = createWalletClient({
  chain: mainnet,
  transport: custom(window.ethereum!),
})
 
const [account] = await walletClient.getAddresses();

await aarcSDK.executeMigrationGasless({
    senderSigner: walletClient,
    receiverAddress:"0x786E6045eacb96cAe0259cd761e151b68B85bdA7",
    chainId: 137,
    account: account,
    transferTokenDetails:[
    {
        tokenAddress:"0x2271e3Fef9e15046d09E1d78a8FF038c691E9Cf9",
        // .toString(16) is to convert TOKEN_AMOUNT to hex in string format
        amount:TOKEN_AMOUNT.toString(16)   
    },
    {
      tokenAddress:"0x932ca55b9ef0b3094e8fa82435b3b4c50d713043",
      tokenIds:["28507"] 
     },
    ],
  });
```
{% endtab %}
{% endtabs %}

## Response

```json
[
  {
  tokenInfo: [
    {
      tokenAddress: string,
      amount: BigInt
    }
  ],
  type: string,
  taskId: string,
  txHash: string,
  status: string | boolean,
  }
]
```

## Support

If you face any trouble, feel free to reach out to our engineers in the [Telegram support group](https://t.me/aarcxyz).
