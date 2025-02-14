---
description: Aarc widget to use the OpenAuth Kit
hidden: true
---

# OpenAuth Widget

The OpenAuth Widget by Aarc provides a seamless authentication solution for web applications, enabling secure and straightforward integration with various social and traditional auth methods. This widget is designed to support developers by simplifying the user authentication process using both Web3 and conventional authentication methods.

## Installation

To install the OpenAuth widget, open your terminal and run the following command:

```bash
npm i @aarc-xyz/auth-widget
```

## Configuring the widget

The configuration object allows you to customize the behavior and appearance of the authentication widget according to your needs.

### Config Object

<pre class="language-typescript"><code class="lang-typescript">interface AuthConfig {
    Wallet?: Function; // JSX element imported from @aarc-xyz/wallet-auth
    callbacks: { 
        onSuccess: Function; 
        onError: Function;
        onClose: Function;
        onOpen: Function;
    };
    appearance: {
        logoUrl: string; 
        themeColor: string;
        darkMode: boolean;
    };
    authMethods: AuthMethod[]; // select authMethods from AuthMethod
    socialAuth: OAuthProvider[]; // select providers from OAuthProvider 
<strong>    aarcApiKey: string; // get teh aarcApiKey from dashboard
</strong>}

enum OAuthProvider {
    GOOGLE = 'google',
    TWITTER = 'twitter',
    APPLE = 'apple',
    DISCORD = 'discord',
    FARCASTER = 'farcaster',
}

enum AuthMethod {
    EMAIL = 'email',
    WALLET = 'wallet',
    SMS = 'sms',
}
</code></pre>

This object defines the visual aspects of the widget, the authentication methods available, and the callbacks for various events. Adjusting these settings lets developers tailor the authentication experience to the specific needs of their application.

### Example Configuration

```typescript
import Wallets from '@aarc-xyz/wallet-auth'

const config: AuthConfig = {
  Wallet: Wallets, //import if you'd like to use browser wallets for authentication
  appearance: {
    logoUrl: 'https://dashboard.aarc.xyz/assets/AuthScreenLogo-CNfNjJ82.svg',
    themeColor: '#2D2D2D',
    darkMode: true
  },
 callbacks: {
      onSuccess: (data: any) => {
        console.log(data)
      },
      onError: (data: any) => {
        console.log("onError", data)
      },
      onClose: (data: any) => {
        console.log("onClose", data)
      },
      onOpen: (data: any) => {
        console.log("onOpen", data)
      }
    },
  authMethods: ['email', 'wallet'],
  socialAuth: ['google'],
  aarc_api_key: "sample_aarc_api_key"; //Get the API Key from the Aarc dashboard
};

```

This snippet sets up the widget with a custom logo, theme color, and specifies the methods through which users can authenticate. Callback functions are provided to handle various events such as successful login or errors during the authentication process.

## Integrating the Provider

Here’s how you integrate the widget into your application using the 'Provider' component:

### Provider Setup

```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App'; // Ensure this import is correct based on your file structure
import './index.css';
//import the provider
import { Provider } from '@aarc-xyz/auth-widget';
//import the widget styles 
import '@aarc-xyz/auth-widget/dist/style.css'
//import your aarcConfig
import { config } from './config';

const root = ReactDOM.createRoot(document.getElementById('root')!);

root.render(
  // <React.StrictMode> // Uncomment if you want to use React's strict mode features
    <Provider config={config}>
      <App />
    </Provider>
  // </React.StrictMode>,
);
```

{% hint style="info" %}
Make sure you import the style.css for the widget UI to render properly
{% endhint %}

{% hint style="info" %}
For NextJS: Since the Aarc auth widget is meant to be used on the client side please make sure the code is running on the client side, by specifying 'use client' at the top of the file
{% endhint %}

The 'Provider' component wraps around your main app component, making the authentication functionality accessible throughout your application. This setup is essential for maintaining a consistent authentication state.

* **\<Provider config={config}>** : This component is used to pass down the authentication configuration to all components within your application. It uses React context internally to provide the configured auth methods and handlers.&#x20;
* **config**: This should be an object that contains all the necessary configuration for the authentication widget to function. This includes visual appearance settings, callback functions, authentication methods, etc., as described in your earlier documentation section.

## Using the widget

The following component must be wrapped by the provider mentioned above

<pre class="language-typescript"><code class="lang-typescript">//app.tsx

<strong>function Auth() {
</strong><strong>
</strong>  const { openAuthWidget } = useAuthWidget()
  
  useEffect(() => {
    console.log('Auth widget mounted')
    openAuthWidget()
    return () => {
      console.log('Auth widget unmounted')
    }
  }
    , [])
  return (
    &#x3C;div>
    &#x3C;/div>
  )
}
</code></pre>

## Send-Transaction Functionality

Implement the '**sendTransaction**' function using the '**useWallet**' hook to enable blockchain transactions:

```typescript
import { useWallet } from '@aarc-xyz/auth-widget'

const sendTransaction = useWallet();

sendTransaction(
{
  to: '0x0d1d4e623D10F9FBA5Db95830F7d3839406C6AF2',
  value: '0x29a2241af62c0000'
}, 
11155111, 
"Your-API-Key");
```

To get your API Key, visit Aarc dashboard and follow the steps mentioned [here](https://docs.aarc.xyz/developer-docs/getting-started/dashboard-setup-get-api-key).

## Troubleshooting

* Widget not loading: Ensure all script dependencies are loaded and the DOM is fully mounted.
* Callback not firing: Check the correctness of the callback definitions in your config object.

## Support

If you face any trouble, feel free to reach out to our engineers in the [Telegram support group](https://t.me/aarcxyz).
