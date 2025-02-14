# SDK Implementation in Typescript + ethers

Here is a step-by-step guide to integrating the [SDK](../sdk/) into your dApp seamlessly to `checkout`, cross-chain custom `mint` function,  you can find the complete implementation in this [repo](https://github.com/aarc-xyz/Aarc-Core-SDK-Contract-Call-Example)

1. &#x20; Setup Prerequisites

Ensure you have the following:

* Node.js (v16 or higher) installed
* TypeScript and `ts-node` installed
* Environment variables configured

2. Initialize a Node.js project:

```bash
mkdir contract-call-example
cd contract-call-example
npm init -y
```

3. Install required dependencies:

```bash
npm install ethers dotenv @aarc-xyz/core-viem
npm install --save-dev typescript ts-node
```

4. &#x20;Create a `tsconfig.json` file with the following:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true
  }
}
```

5. &#x20;Create a `.env` file in the root directory:

```plaintext
API_KEY=your_aarc_api_key
PRIVATE_KEY=your_private_key
RPC_URL=your_rpc_url
```

* Ensure `dotenv` is loaded in your script to access these variables.

6. &#x20;Create a new file `contract-call-example.ts`.

* **Load environment variables**:

```typescript
import { config } from 'dotenv';
import { ethers } from 'ethers';
import { AarcCore } from '@aarc-xyz/core-viem';

config();
const API_KEY = process.env.API_KEY!;
const PRIVATE_KEY = process.env.PRIVATE_KEY!;
const RPC_URL = process.env.RPC_URL!;
```

* **Initialize Aarc Core SDK and wallet**:

```typescript
const aarcCoreSDK = new AarcCore(API_KEY);
const walletProvider = ethers.getDefaultProvider(RPC_URL);
const wallet = new ethers.Wallet(PRIVATE_KEY, walletProvider);
```

* **Example destination token:**

```typescript
const destinationWalletAddress = "0x45c0470ef627a30efe30c06b13d883669b8fd3a8";
const destinationToken = {
  decimals: 6,
  chainId: 8453,
  address: "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
};
```

* **Fetch wallet balances**:

```typescript
async function getMultichainBalance(address: string) {
// this will fetch balances of user in terms of destination token
  return await aarcCoreSDK.fetchMultiChainBalances(address, {
      tokenAddress: destinationToken.address,
      tokenChainId: destinationToken.chainId,
      tokenAmount: requestedAmount,
    });
}
const balances = await getMultichainBalance(wallet.address);
console.log('Balances:', balances);
```

* **Generate transaction call data**:

```typescript
function generateCheckoutCallData(
  token: string,
  toAddress: string,
  amount: string
): string {
  const simpleDappInterface = new ethers.Interface([
    "function mint(address token, address to, uint256 amount) external",
  ]);

  return simpleDappInterface.encodeFunctionData("mint", [
    token,
    toAddress,
    amount,
  ]);
}

 const contractPayload = generateCheckoutCallData(
      destinationToken.address,
      destinationWalletAddress,
      "10000"
 );
```

* **Fetch deposit address**:

```typescript
async function getDepositAddress(contractPayload, fromToken, fromAmount) {
    const transferType = TransferType.WALLET; // decides the transfer method
    const provider = undefined; // defined in case of onramp or cex

    const payload: any = {
      userOpHash: "",
      transferType,
      destinationChainId: destinationToken.chainId?.toString() ?? "",
      destinationTokenAddress: destinationToken.address ?? "",
      toAmount: baseToAmount,
      destinationRecipient: destinationWalletAddress ?? "",
      targetCalldata: contractPayload,
      // destructure and format the input from fromtoken
      fromAmount,
      fromChainId,
      fromTokenAddress,
      fromAddress
    };
    
  return aarcCoreSDK.getDepositAddress(payload);
}

const depositData = await getDepositAddress({
  contractPayload: callData,
  fromToken: { decimals: 18, chainId: 1, address: 'fromTokenAddress' },
  fromTokenAmount: '0.01',
});

console.log('Deposit Address:', depositData);
```

* **Execute transaction**:

```typescript
const tx = {
  to: depositData.depositAddress,
  value: ethers.utils.parseEther('0.01'),
  data: depositData.txData,
};
const txResponse = await wallet.sendTransaction(tx);
console.log('Transaction Hash:', txResponse.hash);

 // Notify Aarc about the transaction hash (this is optional, but recommended to get the status of the transaction faster)
await aarcCoreSDK.postExecuteToAddress({
  depositData,
  trxHash: txResponse.hash,
});
```

* **Poll transaction status:**

```typescript
async function pollRequestStatus(requestId: string) {
  let status;
  do {
    status = await aarcCoreSDK.getRequestStatus(requestId);
    console.log('Polling status:', status);

    if (status.pollStatus === 'success') {
      break;
    }

    // Wait for 10 seconds before the next iteration
    await new Promise(resolve => setTimeout(resolve, 10000));
  } while (status.pollStatus !== 'success');
}

// Call the function with the requestId
pollRequestStatus(depositData.requestId);
```

for more comprehensive implementation of polling refer [this](https://github.com/aarc-xyz/Aarc-Core-SDK-Contract-Call-Example/blob/main/polling-example.ts)

7. Run the script&#x20;

```bash
npx ts-node contract-call-example.ts
```

8. Output Logs:

* Wallet balances.
* Generated call data.
* Deposit address.
* Transaction hash.
* Polling status updates until completion.

9. Handle Errors

* Use `try...catch` blocks to manage errors like invalid API keys, insufficient balances, or unavailable routes.
* Log descriptive error messages for debugging.
* Do input validation and vanity checks.

**Notes**

*   **Disable transaction execution for testing**: Uncomment the `throw new Error()` line to prevent actual transactions.

    ```typescript
    throw new Error("Transaction execution is disabled for now. Remove this line to proceed.");
    ```
* **Modify configuration**: Adjust the script to your specific use case (e.g., token address, destination chain).

Now you can `checkout` to a cross-chain contract call with different payment methods. :tada:
