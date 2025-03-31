# Using OpenAuth

## Accessing OpenAuth on GitHub

***

OpenAuth is available as an open-source project on GitHub. To access and contribute to the project:

1. Visit the [OpenAuth GitHub](https://github.com/aarc-xyz/open-auth-monorepo) repository and explore the codebase, documentation, and contribution guidelines in the [repository](https://github.com/aarc-xyz/open-auth-monorepo).
2. Join the OpenAuth community:
   * Star the repository to show your support
   * Watch the repository for updates
   * Participate in discussions in the Issues and Pull Requests sections

OpenAuth invites developers, communities, and organizations to build, adapt, and innovate on a foundation of trust and transparency. Together, we can shape the future of decentralized authentication in the Web3 ecosystem.

For more detailed information on using and contributing to OpenAuth, please refer to the documentation in the GitHub repository.

> Github Repository - [https://github.com/aarc-xyz/open-auth-monorepo](https://github.com/aarc-xyz/open-auth-monorepo)



## OpenAuth Stack

***

1. **Open Auth supports multiple OAuth providers for user authentication and account creation, like**
   1. Email, via a one-time password (OTP), sent to their email address
   2. Phone number, via a one-time password (OTP) sent to their phone number&#x20;
   3. Wallet, via the Sign In With Ethereum (SIWE) standard&#x20;
   4. Web2 social accounts (Google, Apple, Twitter, Discord)&#x20;
   5. Farcaster accounts are available via the Sign In With Farcaster (SIWF) standard.
2. **Wallet Management with Lit Protocol**
   * Decentralized Key Storage: Keys are securely stored across Lit's node network.
   * Lit Actions: Immutable programs for secure access to signing and encryption functionalities.
3. **Comprehensive Signing Functionality**
   * API Endpoints:
     * Sign Message: Secure message signing using Public Key Pair (PKP)
     * Sign Transaction: Transaction signing with PKP for specified chains
     * Send Transaction: Send signed transactions to supported chains
   * Ethers Signer:
     * JS/TS package mimicking Ethers Wallet or Signer functionality
     * Ideal for creating smart wallets without exposing private keys
