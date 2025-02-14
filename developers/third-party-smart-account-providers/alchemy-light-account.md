---
description: A tutorial on integrating Alchemy Light Account for an EOA with Aarc SDK ❇️
---

# Alchemy Light Account

Setup Light Account in **two steps using Aarc SDK:**

1. Fetch the Light Account address for the EOA.
2. Deploy and transfer native funds to the Light Account. &#x20;

So, let's get started with this step-by-step guide. :rocket:

***

1. Create a new directory, initialize `npm` in it and install **Aarc Migrator Package** and **ethers**

{% code overflow="wrap" %}
```bash
mkdir aarc_alchemySW && cd aarc_alchemySW  && npm init -y && npm i @aarc-xyz/migrator ethers@5.7.2
```
{% endcode %}

2. Create a new file `index.js` in `src` folder.
3. Import the dependencies.&#x20;

```javascript
const { ethers, BigNumber } = require("ethers");
const {Migrator, WALLET_TYPE}= require("@aarc-xyz/migrator");
```

4. Get the Aarc API Key (you can learn how to get it from [here](broken-reference)), your EOA private key and the RPC URL.
5. Initialize the provider and Aarc SDK

```javascript
let provider = new ethers.providers.JsonRpcProvider("YOUR_RPC_URL");
let signer = new ethers.Wallet("YOUR_PRIVATE_KEY", provider);
let eoaAddress = signer.address;

let aarcSDK = new Migrator({
    rpcUrl: "YOUR_RPC_URL",
    chainId: 80001, // Mumbai Testnet
    apiKey: "YOUR_AARC_API_KEY"
});
```

6. Fetch the Light Account addresses of the EOA.

```javascript
async function main() {
    try {
        const smartWalletAddresses = await aarcSDK.getSmartWalletAddresses(
            WALLET_TYPE.ALCHEMY_SIMPLE_ACCOUNT, // WALLET_TYPE imported from @aarc-xyz/migrator
            eoaAddress
        );
        console.log(' smartWalletAddresses ', smartWalletAddresses);
    } catch (error) {
        console.error(' error ', error);
    }
}
```

Here, we provide the `WALLET_TYPE.ALCHEMY_SIMPLE_ACCOUNT` to fetch the Light Account addresses associated with the EOA.&#x20;

In the response, we will see the addresses associated with the EOA for Alchemy's Light Account. No Light Account may be deployed, so it will only generate and return the Light Account address, mentioning its deployment status and the `walletIndex`.

<details>

<summary>Example response when no Smart Account is deployed!</summary>

```bash
 smartWalletAddresses  [
  {
    address: '0x5d3848F6CDB26F2b158c19D47a0c325d30734367',
    isDeployed: false,
    walletIndex: 0
  }
]
```

</details>

7. Deploy the Light Account.&#x20;

To deploy the Light Account, use `aarcSDK.transferNativeAndDeploy` this will take a few parameters, as shown below.

{% hint style="info" %}
NOTE:&#x20;

* **`deploymentWalletIndex`** and **`receiver`** can be fetched from the above response.
* **`amount`**&#x73;hould be in hex string, which can be easily done by using`BigNumbers` from ethers.
{% endhint %}

```javascript
async function main() {
    // ... Previous Code ...
    try {
        const response = await aarcSDK.transferNativeAndDeploy({
            walletType: WALLET_TYPE.ALCHEMY_SIMPLE_ACCOUNT,
            owner: eoaAddress,
            receiver: "0x5d3848F6CDB26F2b158c19D47a0c325d30734367",
            signer: signer,
            deploymentWalletIndex: 0,
            amount: BigNumber.from("10000")._hex
        });
        console.log("Transfer and Deploy Response: ", response);
    }catch(error){
        console.error(' error ', error);
    }
}
```

8. Run the script.

<details>

<summary>Complete script!</summary>

{% code overflow="wrap" %}
```javascript
const { ethers, BigNumber } = require("ethers");
const {Migrator, WALLET_TYPE}= require("@aarc-xyz/migrator");

let provider = new ethers.providers.JsonRpcProvider("YOUR_RPC_URL");
let signer = new ethers.Wallet("YOUR_PRIVATE_KEY", provider);
let eoaAddress = signer.address;

let aarcSDK = new Migrator({
    rpcUrl: "YOUR_RPC_URL",
    chainId: 80001, // Mumbai Testnet
    apiKey: "YOUR_AARC_API_KEY"
});

async function main() {
    try {
        const smartWalletAddresses = await aarcSDK.getSmartWalletAddresses(
            WALLET_TYPE.ALCHEMY_SIMPLE_ACCOUNT,
            eoaAddress
        );
        console.log(' smartWalletAddresses ', smartWalletAddresses);
    } catch (error) {
        console.error(' error ', error);
    }

    try {
        const response = await aarcSDK.transferNativeAndDeploy({
            walletType: WALLET_TYPE.ALCHEMY_SIMPLE_ACCOUNT,
            owner: eoaAddress,
            receiver: "0x5d3848F6CDB26F2b158c19D47a0c325d30734367", //Change the receiver address to the SA address
            signer: signer,
            deploymentWalletIndex: 0, // Change the index to the index of the SA
            amount: BigNumber.from("10000")._hex // Change the string to the amount you want to transfer
        });
        console.log("Transfer and Deploy Response: ", response);
    }catch(error){
        console.error(' error ', error);
    }
}

main();
```
{% endcode %}

</details>

```bash
node src/index.js
```

You will see the following response after deploying the Alchemy Smart Wallet.

<details>

<summary>Response after deploying the Smart Account</summary>

```bash
Transfer and Deploy Response:  [
  {
    tokenAddress: '',
    amount: '0x00',
    message: 'Deployment tx sent',
    txHash: '0x67de98b6b158639dd91958da4e996b629fdcfb0551a241a931e2b21fdf3ec0c4'
  },
  {
    tokenAddress: '0x0000000000000000000000000000000000001010',
    amount: '0x2710',
    message: 'Token transfer tx sent',
    txHash: '0xb1fe512fd431199960dfdadb9e9aed2ac406f47d8e1493112be17cd023339e63'
  }
]
```

</details>

***

> _Voila! You have successfully deployed and funded your Alchemy's_ Light Account _in **only two steps** using Aarc SDK❇️_

If you have any doubts, you can contact us on [Telegram](https://t.me/aarcxyz).
