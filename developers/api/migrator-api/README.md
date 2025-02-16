---
description: API for Migration calls.
hidden: true
---

# Migrator API

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrator/balances" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}

## Forward Transactions (pay with Stables)

### Generate calldata&#x20;

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrate/forward-calldata" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}

### Execute calldata

After receiving the `calldata` from the `/migrate/forward-calldata`, it needs to be signed by the signer for different approvals.&#x20;

To do that, you will need to get each item in the `forwardCallDataReponse` signed by the signer and then passed as `trxList` and the `txnIndexes` with it. Learn how to do this [here](signing-calldata-for-execution)[.](signing-calldata-for-execution#signing-the-forward-calldata)

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrate/forward-calldata-trx" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}

## Gasless Transactions (pay NO Gas)

### Generate calldata

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrate/gasless-calldata" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}

### Execute calldata

After receiving the `calldata` from the `/migrate/gasless-calldata`, it needs to be signed by the signer for different approvals.&#x20;

To do that, you will need to get each item in the `forwardCallDataReponse` signed by the signer and then passed as `trxList` and the `txnIndexes` with it. Learn how to do this [here](signing-calldata-for-execution#gasless-transactions)[.](signing-calldata-for-execution#signing-the-forward-calldata)

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrate/gasless-calldata-trx" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}

## Non-gasless Transactions (pay Gas)

{% swagger src="https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json" path="/migrate/non-gasless-calldata" method="post" %}
[https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json](https://raw.githubusercontent.com/aarc-xyz/swagger-api/main/fdk/migrator/migrator.json)
{% endswagger %}
