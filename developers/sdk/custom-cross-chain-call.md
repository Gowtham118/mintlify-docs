---
description: Call your contract with custom payload on different destination chain
---

# Custom cross-chain call

The `getDepositAddress` function allows developers to interact with contracts on a different destination chain. For instance, if a user holds funds on \`Arbitrum\` but needs to call a contract on `Polygon`, they can achieve this seamlessly by generating a deposit address with `getDepositAddress` and submitting the payload on-chain.

### Expected parameters

`getDepositAddress` accepts following [parameters](buy-tokens.md#payload).

### Function Call

The `getDepositAddress` function generates an address based on the provided payload, which must then be funded and submitted on-chain.\


{% tabs %}
{% tab title="ethers.js" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>const provider = new ethers.BrowserProvider(window.ethereum);
</strong>const signer = await provider.getSigner();

const address = signer.getAddress();
const contractInterface = new ethers.Interface(ABI); // create interface with your own ABI

const destinationPayload = contractInterface.encodeFunctionData("mint", [
  address,
  1000000000000,
]); // Replace with your own function name and parameters

const payload: any = {
        // ... remaining params
        destinationRecipient : YOUR_CONTRACT_ADDRESS, 
        targetCalldata: destinationPayload
};

const depositData = await aarcCoreSDK.getDepositAddress(payload); 

// make client sign the generated tx
const { to, value, data, gasLimit, chainId, from } = depositData.txData;
const txHash = await signer.sendTransaction({ to, value, data, gasLimit, chainId, from });
</code></pre>
{% endtab %}

{% tab title="viem" %}
<pre class="language-typescript"><code class="lang-typescript">import { createWalletClient, custom } from "viem";

const getWalletClient = async () => {
<strong>  const [account] = await window.ethereum.request({
</strong>    method: "eth_requestAccounts",
  });
  const walletClient = createWalletClient({
    chain: arbitrum,
    account,
    transport: custom(window.ethereum!),
  });
  return walletClient;
};

const wallet = await getWalletClient();

const [address] = await wallet.requestAddresses();

const destinationPayload = encodeFunctionData({
  abi: ABI, // Replace with your ABI
  functionName: "mint",
  args: [address, 1000000000000],
}); // Replace with your own function name and parameters

const payload: any = {
        // ... remaining params
        targetCalldata: destinationPayload
};

const depositData = await aarcCoreSDK.getDepositAddress(payload); 

// make client sign the generated tx
const { to, value, data, gasLimit, chainId, from } = depositData.txData;
const txHash = await signer.sendTransaction({ to, value, data, gasLimit, chainId, from });

console.log('tx', tx);
</code></pre>
{% endtab %}
{% endtabs %}

Using onramp for contract call

```typescript
// Using moonpay for onramp (you can also use kado)
const payload = {
    // rest of the payload,
    transferType: "onramp",  
};

const provider = "moonpay"; 

const url = await aarcCoreSDK.generateMoonpayOnrampUrl({
        walletAddress: depositAddressData.depositAddress,
        defaultCryptoCurrencyCode: depositAddressData.depositTokenSymbol,
        fiatAmount: fromToken.amount_required,
        fiatCurrencyCode: "USD",
        network: "BASE",
        cryptoTokenData: {
          tokenAmount: fromToken.amount_required,
          tokenCode: depositAddressData.depositTokenSymbol ?? "ETH",
        },
      });

const windowRef = window.open(url, "_blank");
```

For more comprehensive implementation, refer [cookbook](../cookbook/sdk-implementation-in-typescript-+-ethers.md) example.  &#x20;
