# Signing calldata for execution

{% hint style="info" %}
**NOTE:**\
We are using **`ethers v6.9`**&#x66;or the **`senderSigner`** object.
{% endhint %}

## Forward Transactions

When you receive the response from the [`migrate/forward-calldata`](./#migrate-forward-calldata) it needs to be signed by the user before getting executed by the [`migrate/forward-calldata-trx`](./#migrate-forward-calldata-trx).

To get the data signed and executed, go through the following steps:

1. Generate the forward calldata.

```typescript
const transferTokenDetails = [{
  tokenAddress: "0x0537a0f2e1373886a3d4f0e2b24c026ffa6f16f7";
  amount: "1000000";
}];
const requestBody = {
        chainId: "137",
        owner: await senderSigner.getAddress(),
        receiverAddress,
        tokensList: transferTokenDetails,
};
      
const forwardCallResponse = await sendRequest({
      url: GENERATE_FORWARD_CALL_DATA_ENDPOINT,
      method: HttpMethod.POST,
      headers: {
        "x-api-key": "AARC_API_KEY",
      },
      body: requestBody,
});
```

* `senderSigner` is the signer object of the user.&#x20;

2. Get  `forwardCallDataResponse` from the response in a variable.

```typescript
const forwardTrxList = forwardCallResponse.forwardCallDataResponse;
```

3. Run a loop to sign each item in the `forwardTrxList`

```typescript
for (let index = 0; index < forwardTrxList.length; index++) {
          const element = forwardTrxList[index];
          if (element.txType === "PERMIT") {
            // Sign the EIP-712 message
            const eip712Struct = element.eip712Struct
            const { domain, types, value } = eip712Struct
            const signature = await senderSigner.signTypedData(domain, types, value);
            const dataSet = JSON.parse(element.data);
            dataSet.signature = signature;
            element.data = JSON.stringify(dataSet);
          } else if (element.txType === "PERMIT2_BATCH") {
            const eip712Struct = element.eip712Struct
            const { domain, types, value } = eip712Struct
            const signature = await senderSigner.signTypedData(domain, types, value);
            const dataSet = JSON.parse(element.data);
            dataSet.signature = signature;
            element.data = JSON.stringify(dataSet);
          }
        }
```

* The `if` conditions are used to differentiate among the different Transaction Types involved in the transaction.&#x20;
* the signatures are generated for the `eip712Struct` present in the transaction item by signing it using the `senderSigner`.

At the end of the loop, this will edit and finalize the `forwardTrxList`.

4. Pass the `forwardTrxList` and the `txIndexes` to the execution endpoint.

```typescript
const txResponse: ExecuteCallDataResponse = await sendRequest({
  url: EXECUTE_FORWARD_CALL_DATA_ENDPOINT,
  method: HttpMethod.POST,
  headers: {
    "x-api-key": "AARC_API_KEY",
  },
  body: {
    chainId,
    trxList: forwardTrxList,
    txIndexes: forwardCallResponse.txIndexes,
  },
});
```

> _Voila! Your forward transaction is executed 🥳_

## Gasless Transactions

When you receive the response from the [`migrate/gasless-calldata`](./#migrate-gasless-calldata) it needs to be signed by the user before getting executed by the [`migrate/gasless-calldata-trx`](./#migrate-gasless-calldata-trx).

To get the data signed and executed, go through the following steps:

1. Generate the gasless calldata.

```typescript
const transferTokenDetails = [{
  tokenAddress: "0x0537a0f2e1373886a3d4f0e2b24c026ffa6f16f7";
  amount: "1000000";
}];
const requestBody = {
    chainId: this.chainId.toString(),
    owner: await senderSigner.getAddress(),
    receiverAddress,
    tokensList: transferTokenDetails,
};

const gaslessResponse = await sendRequest({
      url: GENERATE_GASLESS_CALL_DATA_ENDPOINT,
      method: HttpMethod.POST,
      headers: {
        "x-api-key": "AARC_API_KEY",
      },
      body: requestBody,
});
```

* `senderSigner` is the signer object of the user.&#x20;

2. Get  `data` from the response in a variable.

```typescript
const resultSet = gaslessResponse.data;
```

3. Initialize a variable to store the transactions which do not support Permit.

```typescript
const non_permit_transactions = [];
```

3. Run a loop to sign item in the `resultSet`

```typescript
for (let index = 0; index < resultSet.length; index++) {
  const element = resultSet[index];
  if (element.txType === "PERMIT") {
    // Sign the EIP-712 message
    const eip712Struct = element.eip712Struct
    const { domain, types, value } = eip712Struct
    const signature = await senderSigner.signTypedData(domain, types, value);
    const dataSet = JSON.parse(element.data);
    dataSet.signature = signature;
    element.data = JSON.stringify(dataSet);
  }
  if (element.txType === "PERMIT2_SINGLE") {
    const eip712Struct = element.eip712Struct
    const { domain, types, value } = eip712Struct
    const signature = await senderSigner.signTypedData(domain, types, value);
    const dataSet = JSON.parse(element.data);
    dataSet.signature = signature;
    element.data = JSON.stringify(dataSet);
  }
  if (element.txType === "PERMIT2_BATCH") {
    const eip712Struct = element.eip712Struct
    const { domain, types, value } = eip712Struct
    const signature = await senderSigner.signTypedData(domain, types, value);
    const dataSet = JSON.parse(element.data);
    dataSet.signature = signature;
    element.data = JSON.stringify(dataSet);
  }
  if (element.txType === "NATIVE_TRANSFER") {
    non_permit_transactions.push({
      from: owner,
      to: receiverAddress,
      data: '0x',
      tokenAddress: element.tokenAddress[0],
      amount: element.amount,
      type: "dust",
    })
  }
  if (element.txType === "ERC20_TRANSFER") {
    non_permit_transactions.push({
      from: owner,
      data: element.data,
      to: receiverAddress,
      tokenAddress: element.tokenAddress[0],
      amount: element.amount,
      type: "cryptocurrency",
    })
  }
  if (element.txType === "NFT_TRANSFER") {
    non_permit_transactions.push({
      from: owner,
      to: receiverAddress,
      data: element.data,
      tokenAddress: element.tokenAddress[0],
      amount: '1',
      type: "cryptocurrency",
    })
  }
}
```

* The `if` conditions are used to differentiate among the different Transaction Types involved in the transaction.&#x20;
* the signatures are generated for the `eip712Struct` present in the transaction item by signing it using the `senderSigner`.
* `transactions` is populated with the transactions for tokens that don't support Permit.

At the end of the loop, this will edit and finalize the `resultSet`

4. Execute the gasless transactions.

```typescript
const txResponse = await sendRequest({
      url: EXECUTE_GASLESS_CALL_DATA_ENDPOINT,
      method: HttpMethod.POST,
      headers: {
        "x-api-key": "AARC_API_KEY",
      },
      body: { "137", trxList: resultSet},
});
```

5. Execute the transactions for tokens that don't support Permit.

```typescript
for (const tx of non_permit_transactions) {
    const trx = await senderSigner.sendTransaction({
        to: tx.tokenAddress,
        value: tx.type === "dust" ? tx.amount : '0x0',
        data: tx.data,
        gasLimit: BigInt(parseInt(Number(gasEstimated).toString()))
    });
}
```

> _Voila! Your gasless transaction is executed 🥳_
