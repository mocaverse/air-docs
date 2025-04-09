# Reference

### AirService

```typescript
class AirService {
    constructor({ partnerId }: {
        partnerId: string;
        modalZIndex?: number;
    });
    get buildEnv(): BUILD_ENV_TYPE;
    get isInitialized(): boolean;
    get isLoggedIn(): boolean;
    get isWalletInitialized(): boolean;
    init({ buildEnv, enableLogging, skipRehydration, }: {
        buildEnv: BUILD_ENV_TYPE;
        enableLogging: boolean;
        skipRehydration: boolean;
    }): Promise<AirLoginResult | null>;
    cleanUp(): Promise<void>;
    login(options?: {
        authToken?: string;
    }): Promise<AirLoginResult>;
    deploySmartAccount(): Promise<{ txHash: string; }>;
    isSmartAccountDeployed(): Promise<boolean>;
    logout(): Promise<void>;
    getProvider(): Promise<AirWalletProvider>;
    preloadWallet(): Promise<AirWalletInitializedResult>;
    claimAirId(options?: ClaimAirIdOptions): Promise<ClaimAirIdResult>;
    getUserInfo(): Promise<AirUserDetails>;
    on(listener: AirEventListener): void;
    off(listener: AirEventListener): void;
    clearEventListeners(): void;
}
```

### Types

<pre class="language-typescript"><code class="lang-typescript">export type AirIdDetails = {
  id: string;
  name?: string;
  node: string;
  status: "minting" | "minted";
  chainId: number;
  imageUrl?: string;
};
<strong>
</strong><strong>export type AirUserDetails = {
</strong>    partnerId: string;
    partnerUserId: string;
    airId?: AirIdDetails;
    user: {
        id: string;
        abstractAccountAddress?: string;
        email?: string;
    };
};

export type AirInitializationResult = {
    rehydrated: boolean;
};

export type AirLoginResult = {
    isLoggedIn: boolean;
    id: string;
    abstractAccountAddress?: string;
    token: string;
};

export type AirWalletInitializedResult = {
    abstractAccountAddress: string;
};

export type ClaimAirIdResult = {
    airId: AirIdDetails;
};

export type AirEventOnInitialized = {
    event: "initialized";
    result: AirInitializationResult;
};

export type AirEventOnLoggedIn = {
    event: "logged_in";
    result: AirLoginResult;
};

export type AirEventOnAirIdMintingStarted = {
    event: "air_id_minting_started";
};

export type AirEventOnAirIdMintingFailed = {
    event: "air_id_minting_failed";
    errorMessage?: string;
};

export type AirEventOnLoggedOut = {
    event: "logged_out";
};

export type AirEventOnWalletInitialized = {
    event: "wallet_initialized";
    result: AirWalletInitializedResult;
};

export type AirEventData = AirEventOnInitialized | AirEventOnLoggedIn | AirEventOnWalletInitialized | AirEventOnAirIdMintingStarted | AirEventOnAirIdMintingFailed | AirEventOnLoggedOut;

export type AirEventListener = (data: AirEventData) => void;

export type ClaimAirIdOptions = {
    token?: string;
    background?: false;
} | {
    token: string;
    background: true;
};
</code></pre>
