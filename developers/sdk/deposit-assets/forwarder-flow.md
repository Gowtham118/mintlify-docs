---
description: >-
  AARC SDK supports the migrations of tokens with gas fees paid in stable
  tokens.
hidden: true
---

# Forwarder Flow

The **`executeMigrationForward`** function of our SDK implements a forwarder flow that enables the migration of tokens between wallets or contracts with a unique feature: the gas fees are paid using stable tokens instead of the native blockchain currency. This method is tailored for users who prefer or need to utilize stable tokens to cover transaction costs, offering a practical solution in volatile market conditions.

## Expected parameters

**`executeMigrationForward`** accepts the following parameters:

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
**NOTE**:&#x20;

* If **`transferTokenDetails`**&#x69;s not passed, then all the assets will be transferred.
{% endhint %}

## Function Call

### Transfer all tokens

{% tabs %}
{% tab title="ethers.js" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>await aarcSDK.executeMigrationForward({
</strong>    senderSigner: signer,
    chainId: number,
    receiverAddress:"0x786E6045eacb96cAe0259cd761e151b68B85bdA7"
  });
</code></pre>
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

await aarcSDK.executeMigrationForward({
    senderSigner: signer,
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
await aarcSDK.executeMigrationForward({
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

await aarcSDK.executeMigrationForward({
    senderSigner: signer,
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

```typescript
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

## Supported Networks and Tokens

{% tabs %}
{% tab title="Ethereum" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| USDC  | 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48 |
| USDT  | 0xdac17f958d2ee523a2206206994597c13d831ec7 |
| BUSD  | 0x4fabb145d64652a948d72533023f6e7a623c7c53 |
| DAI   | 0x6b175474e89094c44da98b954eedeac495271d0f |
| UNI   | 0x1f9840a85d5af5bf1d1762f925bdaddc4201f984 |
{% endtab %}

{% tab title="Polygon" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| USDC  | 0x2791bca1f2de4661ed88a30c99a7a9449aa84174 |
| USDT  | 0xc2132d05d31c914a87c6611c10748aeb04b58e8f |
| BUSD  | 0x9c9e5fd8bbc25984b178fdce6117defa39d2db39 |
| DAI   | 0x8f3cf7ad23cd3cadbd9735aff958023239c6a063 |
| UNI   | 0xb33eaad8d922b1083446dc23f610c2567fb5180f |
{% endtab %}

{% tab title="Arbitrum" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| USDC  | 0xfd086bc7cd5c481dcc9c85ebe478a1c0b69fcbb9 |
| USDT  | 0xaf88d065e77c8cc2239327c5edb3a432268e5831 |
| DAI   | 0xda10009cbd5d07dd0cecc66161fc93d7c9000da1 |
| UNI   | 0xfa7f8980b0f1e64a2062791cc3b0871572f1f7f0 |
{% endtab %}

{% tab title="Optimsim" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| USDC  | 0x7f5c764cbc14f9669b88837ca1490cca17c31607 |
| USDT  | 0x94b008aa00579c1307b0ef2c499ad98a8ce58e58 |
| DAI   | 0xda10009cbd5d07dd0cecc66161fc93d7c9000da1 |
{% endtab %}

{% tab title="Gnosis" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| USDC  | 0xDDAfbb505ad214D7b80b1f830fcCc89B60fb7A83 |
| USDT  | 0x4ECaBa5870353805a9F068101A40E0f32ed605C6 |
{% endtab %}
{% endtabs %}

## Support

If you face any trouble, feel free to reach out to our engineers in the [Telegram support group](https://t.me/aarcxyz).
