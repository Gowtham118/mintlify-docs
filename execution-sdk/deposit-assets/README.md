---
hidden: true
---

# Deposit Assets

Aarc SDK provides different functions and supports various smart contract providers to ease the developers' work and improve **smart transactions** in web3.

## Start with the Core Package

1. Install the package and other dependencies.

{% tabs %}
{% tab title="ethers.js" %}
```bash
npm i @aarc-xyz/core ethers@6.9.2
```
{% endtab %}

{% tab title="viem" %}
```bash
npm i @aarc-xyz/core-viem viem
```
{% endtab %}
{% endtabs %}

2. Import the package.

{% tabs %}
{% tab title="ethers.js" %}
```typescript
const ethers = require("ethers");
import { AarcCore } from "@aarc-xyz/core";
```
{% endtab %}

{% tab title="viem" %}
```typescript
import { AarcCore } from "@aarc-xyz/core-viem";
```
{% endtab %}
{% endtabs %}

3. Get the API KEY

> Learn how to get the Aarc API Key from [here](broken-reference).

4. Initialise Aarc SDK

```typescript
let aarcSDK = new AarcCore(
    "YOU_AARC_API_KEY"
);
```

_Now go ahead and bring better UX in web3!_
