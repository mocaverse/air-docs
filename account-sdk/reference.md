# Reference

### AirService

```typescript
class AirService {
    constructor({ partnerId }: {
        partnerId: string;
        modalZIndex?: number;
    });
    get buildEnv(): BUILD_ENV_TYPE; // available env: SANDBOX
    get isInitialized(): boolean;
    get isLoggedIn(): boolean;
    get isWalletInitialized(): boolean;
    get provider(): EIP1193Provider;
    init({ buildEnv, enableLogging, skipRehydration, }: {
        buildEnv: BUILD_ENV_TYPE;
        enableLogging: boolean;
        skipRehydration: boolean;
    }): Promise<AirLoginResult | null>;
    login(options?: {
        authToken?: string;
    }): Promise<AirLoginResult>;
    isSmartAccountDeployed(): Promise<boolean>;
    deploySmartAccount(): Promise<{ txHash: string; }>;
    getCredentialSalt(): Promise<{ credentialSalt: string; }>;
    getProvider(): EIP1193Provider;
    preloadWallet(): Promise<void>;
    setupOrUpdateMfa(): Promise<void>;
    getUserInfo(): Promise<AirUserDetails>;
    goToPartner(partnerUrl: string): Promise<{ urlWithToken: string; }>;
    getAccessToken(): Promise<{ token: string; }>;
    logout(): Promise<void>;
    cleanUp(): Promise<void>;
    on(listener: AirEventListener): void;
    off(listener: AirEventListener): void;
    clearEventListeners(): void;
}
```

### Types

```typescript
export type AirIdDetails = {
  id: string;
  name?: string;
  node: string;
  status: "minting" | "minted";
  chainId: number;
  imageUrl?: string;
};

export type AirUserDetails = {
    partnerId: string;
    partnerUserId: string;
    airId?: AirIdDetails;
    user: {
        id: string;
        abstractAccountAddress?: string;
        email?: string;
        isMFASetup: boolean;
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
    isMFASetup: boolean;
};

export type AirWalletInitializedResult = {
    abstractAccountAddress: string | null;
    isMFASetup: boolean;
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
```
