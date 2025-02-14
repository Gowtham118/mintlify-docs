---
description: You can deploy Smart Wallets using aarcSDK.
---

# Deploy your Smart Wallet

## Parameters

`deployWallet` accepts the following parameters:

* `owner`: The address which will be the owner of the Smart Wallet.
* `walletType`: The provider you are looking to deploy your Smart Wallet with.\
  &#xNAN;_&#x43;heck the supported wallet providers_ [_here_](broken-reference)_._
* `signer`: The **signer object** from ethers.
* `deploymentWalletIndex`\[OPTIONAL]: An EOA can deploy multiple wallets. You can specify the index.

{% hint style="info" %}
**NOTE:**

* If the wallet corresponding to the provided owner address (**`EOA_ADDRESS`**) and index (**`deploymentWalletIndex`**) is already deployed, the deployment process will not occur.
{% endhint %}

## Deploy Smart Wallet

```typescript
import { WALLET_TYPE } from "aarc-sdk/dist/utils/AarcTypes";

await aarcSDK.deployWallet({
  owner: EOA_ADDRESS,
  walletType: WALLET_TYPE.SAFE, // Smart Contract Wallet Provider.SAFE, // Smart Contract Wallet Provider
  signer: signer, // ethers.signer object
  deploymentWalletIndex: 0 // Optional -- Number: Since an EOA, can be used to deploy multiple wallets. you can supply any index and it will deploy wallet for you
})
```

## Output

This will be the following output you will receive after deploying a Smart Wallet:

```bash
{
  smartWalletOwner: string;
  deploymentWalletIndex: number;
  txHash: string;
  chainId: number;
  message?: string;
}
```

{% hint style="info" %}
**NOTE:**

* **`message`** only returns if the smart wallet is already deployed or an error occurred.
{% endhint %}
