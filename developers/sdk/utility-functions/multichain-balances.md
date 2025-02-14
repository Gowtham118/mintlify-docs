# Multichain Balances

The `fetchMultiChainBalances` function in our SDK provides a utility for retrieving the token balances of an EOA wallet across various blockchain networks. This functionality is crucial for users managing diverse portfolios that span multiple chains, ensuring a comprehensive overview of their assets in a consolidated manner.

## Expected parameters

`fetchMultiChainBalances` accepts the following parameters:

| Parameter          | Type                        | Description                                                                      |
| ------------------ | --------------------------- | -------------------------------------------------------------------------------- |
| `eoaAddress`       | string                      |  The address whose balances are required to be fetched                           |
| `destinationToken` | DestinationToken (optional) | If provided, it will return the amount needed to convert to the specified token. |

## Function Call

```typescript
type DestinationToken = {
        tokenAddress: string,
        tokenChainId: number,
        tokenAmount: string,
}

const balancesRes = await aarcCoreSDK.fetchMultiChainBalances(
  address: string,
  destinationToken: DestinationToken
);

const balances = balancesRes.data.balances
```

## Response

The response from the `fetchMultiChainBalances` will be:

```typescript
{
    "1": {
        "balances": [
            {
                "decimals": number,
                "name": string,
                "symbol": string,
                "token_address": string
                "logo": string,
                "native_token": boolean,
                "type": string,
                "is_spam": boolean,
                "balance": string,
                "usd_price": boolean
            }
        ],
        "logo": string
    },
    "56": {
        "balances": [
            {
                "decimals": number,
                "name": string,
                "symbol": string,
                "token_address": string
                "logo": string,
                "native_token": boolean,
                "type": string,
                "is_spam": boolean,
                "balance": string,
                "usd_price": boolean
            }
        ],
        "logo": string
    }
}
```

## Support

If you face any trouble, feel free to reach out to our engineers in the [Telegram support group](https://t.me/aarcxyz).
