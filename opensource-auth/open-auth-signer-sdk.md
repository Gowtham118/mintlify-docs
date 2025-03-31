---
hidden: true
---

# Open Auth Signer SDK

Open Auth signer is signer class initiated with a users' authenticated session data and wallet address, providing methods to sign messages, transactions and send transactions form a user's social wallet.

Since the Open Auth Signer also implements from ethers Signer class, all methods in the supported ethers v6 Signer are available for use. Users will be able to access all of them without storing or having the risk of compromising their wallet private keys.

### Installation

```bash
npm i @aarc-xyz/ethers-v6-signer
```

### Initialising

```typescript
import { AarcEthersSigner, OpenAuthProvider } from "@aarc-xyz/ethers-v6-signer";

const rpc_url = 'your rpc url here';
const sessionKey = localStorage.getItem('sessionKey');

const signer = new AarcEthersSigner(rpc_url,
                    {
                        apiKeyId: process.env.AARC_API_KEY,
                        wallet_address: '<authenticated address>',
                        sessionKey: sessionKey,
                        chainId: 1
                    }
                );
```

<mark style="color:orange;">`AarcEthersSigner`</mark> takes in two parameter when initialised, <mark style="color:orange;">`rpc_url`</mark> and <mark style="color:orange;">AarcBaseProps</mark>

* <mark style="color:orange;">`rpc_url`</mark>:  RPC endpiont for the corresponding chain id.

<mark style="color:orange;">`AarcBaseProps`</mark> contains following required parameters.

* <mark style="color:orange;">`apiKeyId`</mark>: is your API key for the Open Auth services.&#x20;
* <mark style="color:orange;">`wallet_address`</mark>: Is the authenticated address of the user's wallet, this is address is returned on success when the user first authenticates using our OpenAuth Widget.&#x20;
* <mark style="color:orange;">`sessionKey`</mark>: Is a unique identifier for the user's session which is stored in local browser storage after successful authentication.
* <mark style="color:orange;">`chainId`</mark>: is the ID of the Ethereum network you want to interact with. For the mainnet, this is 1.

### Signing Message

Since <mark style="color:orange;">`AarcEthersSigner`</mark> implements ethers V6 Signer class, signing is as straight forward as it is in ethers.

To sign a message, use the <mark style="color:orange;">`signMessage`</mark> method:

```typescript
const message = "Hello, world!";
const signature = await signer.signMessage(message);
```

### Signing Transaction

To sign a transaction, use <mark style="color:orange;">`signTransaction`</mark> method:

```typescript
const transaction = {
  to: "0x...",
  value: ethers.utils.parseEther("1.0"),
  gasLimit: 21000,
  // other transaction fields...
};

const signedTransaction = await signer.signTransaction(transaction);
```

### Send Transaction

To send a transaction, use <mark style="color:orange;">`sendTransaction`</mark> method:

```typescript
const transaction = {
  to: "0x...",
  value: ethers.utils.parseEther("1.0"),
  gasLimit: 21000,
  // other transaction fields...
};

const transactionResponse = await signer.sendTransaction(transaction);
```

