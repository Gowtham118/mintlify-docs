# Widget Implementation in Next.js + wagmi

Here is a step-by-step guide to integrate the [widget ](../widget/)into your dApp seamlessly. You can find the complete implementation in this [repo](https://github.com/aarc-xyz/FDK-Widget-Next-JS).

1. If you are creating a new repository, start by initializing a new Next.js app. If a Next.js project is already set up, you can skip this step.

```bash
npx create-next-app@latest <your-app-name>
```

2. Navigate to your app directory.

```bash
cd <your-app-name>
```

3. After setting up your preferences, install the required packages. You can use any package manager of your choice, such as yarn or pnpm.

```bash
npm i @aarc-xyz/fundkit-web-sdk @aarc-xyz/eth-connector
```

4. Next, define your environment variables in the `.env` file.

<pre class="language-tsconfig"><code class="lang-tsconfig"><strong>NEXT_PUBLIC_API_KEY=your-aarc-api-key
</strong></code></pre>

5. Setup [config ](../widget/config.md)for the widget.

`AarcConfig.ts`

```typescript
"use client";

import {
  FKConfig,
  ThemeName,
  TransactionErrorData,
  TransactionSuccessData,
} from "@aarc-xyz/fundkit-web-sdk";

export const aarcConfig: FKConfig = {
  appName: "Dapp Name",
  module: {
    exchange: {
      enabled: true,
    },
    onRamp: {
      enabled: true,
      onRampConfig: {
        customerId: "323232323",
      },
    },
    bridgeAndSwap: {
      enabled: true,
      fetchOnlyDestinationBalance: false,
      routeType: "Value",
      connectors: [SourceConnectorName.ETHEREUM],
    },
  },
  destination: {
    walletAddress: "0xeDa8Dec60B6C2055B61939dDA41E9173Bab372b2",
  },
  appearance: {
    themeColor: "#A5E547",
    textColor: "#2D2D2D",
    backgroundColor: "#FAFAFA",
    dark: {
      themeColor: "#A5E547",
      textColor: "#FFF",
      backgroundColor: "#2D2D2D",
      highlightColor: "#08091B",
      borderColor: "#424242",
    },
    theme: ThemeName.DARK,
    roundness: 8,
  },
  apiKeys: {
    aarcSDK: process.env.NEXT_PUBLIC_API_KEY ?? "",
  },
  events: {
    onTransactionSuccess: (data: TransactionSuccessData) => {
      console.log("client onTransactionSuccess", data);
    },
    onTransactionError: (data: TransactionErrorData) => {
      console.log("client onTransactionError", data);
    },
    onWidgetClose: () => {
      console.log("client onWidgetClose");
    },
    onWidgetOpen: () => {
      console.log("client onWidgetOpen");
    },
  },
  origin: typeof window !== "undefined" ? window.location.origin : "",
};
```

6. Setup your wagmi & RainbowKit.

```typescript
import { arbitrum, base, mainnet, optimism, polygon, linea, avalanche, bsc } from "wagmi/chains";

export const wagmiConfig = getDefaultConfig({
  appName: "Your App Rainbowkit",
  projectId: "YOUR_RAINBOW_KIT_PROJECT_ID",
  chains: [mainnet, polygon, optimism, arbitrum, base, linea, avalanche, bsc],
  transports: {
    [mainnet.id]: http(),
    [polygon.id]: http(),
    [optimism.id]: http(),
    [arbitrum.id]: http(),
    [base.id]: http(),
    [linea.id]: http(),
    [avalanche.id]: http(),
    [bsc.id]: http(),
  },
});
```

6. Create a custom provider to manage the widget's state consistently throughout the application.

`contexts/AarcProvider.tsx`

```typescript
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import "@aarc-xyz/eth-connector/styles.css";
import {
  AarcFundKitModal,
  SourceConnectorName
} from "@aarc-xyz/fundkit-web-sdk";
import { createContext, useContext, useRef } from "react";
import { useEffect } from "react";
import dynamic from "next/dynamic";
import { http, WagmiProvider } from "wagmi";
import { getDefaultConfig, RainbowKitProvider } from "@rainbow-me/rainbowkit";
import { AarcEthWalletConnector } from "@aarc-xyz/eth-connector";
import { wagmiConfig } from "your-wagmi-config";
import { aarcConfig } from "your-aarc-config"

interface AarcProviderProps {
  children: React.ReactNode;
}

interface AarcContextType {
  aarcModal: AarcFundKitModal | null;
}

const AarcContext = createContext<AarcContextType | undefined>(undefined);

const queryClient = new QueryClient();

const AarcProvider = ({ children }: AarcProviderProps) => {
  const aarcModalRef = useRef<AarcFundKitModal | null>(
    new AarcFundKitModal(aarcConfig)
  );

  const aarcModal = aarcModalRef.current;

  if (!aarcModal) {
    return null; // Or a fallback UI while initializing
  }

  return (
    <WagmiProvider config={wagmiConfig}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>
          <AarcEthWalletConnector aarcWebClient={aarcModal}>
            <AarcContext.Provider value={{ aarcModal }}>
              {children}
            </AarcContext.Provider>
          </AarcEthWalletConnector>
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
};

export const useAarcContext = () => {
  const context = useContext(AarcContext);
  if (!context) {
    throw new Error("useAarcContext must be used within AarcProvider");
  }
  return context;
};

export default AarcProvider;
```

7. Wrap the application with the necessary provider(s).

In root component:

```typescript
import AarcProvider from "@/contexts/AarcProvider"

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
     <AarcProvider>
        {children}
     </AarcProvider>
  );
}
```

8. Opening widget.

```typescript
"use client";

import { useAarcContext } from "@/contexts/AarcProvider";

export default function YourComponent() {
  const { aarcModal } = useAarcContext();

  return (
        <button
          onClick={() => {
            aarcModal?.openModal();
          }}
        >
          Open Aarc Widget
        </button>
  );
}
```

&#x20;That's it ! :tada: \
&#x20;You’ve successfully integrated the widget into your dApp, enabling seamless user onboarding to purchase tokens via CEX, wallets, or fiat. Additionally, the widget supports checkout functionality, allowing you to call custom contracts on the destination chain with the specified payload.
