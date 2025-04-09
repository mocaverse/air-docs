# Customization

We offer many ways to customize the experience of the login and wallet. This includes theming, localization and other settings such as control over the login methods or recommended wallets.

Below you can find all currently available settings:

```typescript
{ 
  // Node of the air id, e.g. "moca"
  node: string;
  
  // Gating mechanism for minting an id
  gating: "none" | "jwt";
  
  // Identifier of your theme which specifies the css property name
  // and html tag, e.g. "anime".
  theme?: string;
  
  // The chain id which is used to mint the id, e.g. "0x14a34" for Polygon
  chainId: string;
  
  // A whitelist of origin hostnames, any other domain will abort
  // the service initialization. E.g. ["sdk-demo.web3.com"]
  allowedDomains: string[];
  
  // In case you want to use custom auth and/or jwt gating,
  // we will use either the public key or the jwks endpoint
  // to validate your tokens.
  publicKey?: string;
  jwksUrl?: string;
  
  // This sets rules on new air ids.
  // The default values are min 4 and max 20.
  realmIdValidation: {
    minLength: number;
    maxLength: number;
  };
  
  // Defines the available login methods in specified order
  loginMethods?: ("passwordless", "passwordlessToggle", "google", "wallet", "passkey")[];
	
  // Defines the wallet options for the login and signup screen
  // in specified order. For this to show up, "wallet" needs to be
  // in the list of loginMethods above.
  walletLoginMethods?: ("metamask" | "coinbase" | "okx")[];
  
  // Defines the recommended wallet options in specified order.
  recommendedWallets?: ("metamask" | "coinbase" | "okx")[];
  
  // The default language to fallback to when none is matching, e.g. "en"
  defaultLanguage?: string;
  
  style?: {
    // Select between having the passwordless button inlined inside
    // the email input or standalone. Default is inline.
    passwordlessButtonVariant?: 'inline' | 'standalone';
    // Change the air logo dark and light behaviour to best match the
    // partner theme
    airLogoTheme?: "default" | "reverse" | "dark_only" | "light_only";
    // Show the advanced details section expanded on the transaction screen.
    // By default it is collapsed.
    transactionDetailsExpanded?: boolean;
  }
}
```

### Theming

For custom theming the you can provide us following CSS custom properties below with your partner `name` specified in the config above as `theme`.

```css
:root {
    --name-air-text-primary-color: #002536;
    --name-air-text-secondary-color: #5C5F5F;
    --name-air-text-placeholder-color: #8E9191;
    --name-air-text-disabled-color: #747878;
    --name-air-text-inverse-color: #FFFFFF;
    --name-air-text-on-color: #002536;

    --name-air-button-primary-color: #CCE8EA;
    --name-air-button-primary-hover-color: #DAF6F9;
    --name-air-button-secondary-color: #C3B6F8;
    --name-air-button-secondary-hover-color: #E2D8FF;
    --name-air-button-disabled-color: #E0E3E3;
    --name-air-button-outlined-color: #6F797A;
    --name-air-button-outlined-hover-color: #FFFFFF;
    --name-air-button-outlined-secondary-color: #3F4849;
    --name-air-button-outlined-secondary-hover-color: #C8D2D4;

    --name-air-border-color: #BFC8CA;
    --name-air-border-focus-color: #FFFFFF;

    --name-air-link-color: #00696F;
    --name-air-text-button-color: #00696F;

    --name-air-icon-primary-color: #002536;
    --name-air-icon-secondary-color: #6F797B;
    --name-air-icon-hover-color: #80D4DB;
    --name-air-icon-disabled-color: #747878;

    --name-air-status-success-color: #00C773;
    --name-air-status-error-color: #FF4949;
    --name-air-status-pending-color: #F99247;

    --name-air-surface-color: #F5FAFC;
    --name-air-surface-dim-color: #343A3B;

    --name-air-overlay-color: #00000050;

    --name-air-container-primary-color: #E9F2F4;
    --name-air-container-secondary-color: #D2E0E3;

    --name-air-decorative-color: #D7E3FF;
    --name-air-secondary-5-color: #21304C;

    --name-air-logo: url(https://static.air3.com/assets/moca-logo.svg);
    --name-air-background-color: var(--name-air-container-primary-color);
}

@media (prefers-color-scheme: dark) {
    :root {
        --name-air-text-primary-color: #ffffff;
        --name-air-text-secondary-color: #b0b0b0;
        --name-air-text-placeholder-color: #767676;
        --name-air-text-disabled-color: #ffffff25;
        --name-air-text-inverse-color: #000000;
        --name-air-text-on-color: #ffffff;

        --name-air-button-primary-color: #00bec9;
        --name-air-button-primary-hover-color: #80d4db;
        --name-air-button-secondary-color: #8E74FF;
        --name-air-button-secondary-hover-color: #A793FF;
        --name-air-button-disabled-color: #535353;
        --name-air-button-outlined-color: #ffffff;
        --name-air-button-outlined-hover-color: #2a3233;
        --name-air-button-outlined-secondary-color: #3F4849;
        --name-air-button-outlined-secondary-hover-color: #2A3233;

        --name-air-border-color: #3f4849;
        --name-air-border-focus-color: #00bec9;

        --name-air-link-color: #00bec9;
        --name-air-text-button-color: #00bec9;

        --name-air-icon-primary-color: #ffffff;
        --name-air-icon-secondary-color: #b0b0b0;
        --name-air-icon-hover-color: #80d4db;
        --name-air-icon-disabled-color: #ffffff25;

        --name-air-status-success-color: #74ffcd;
        --name-air-status-error-color: #ff4949;
        --name-air-status-pending-color: #f99247;

        --name-air-surface-color: #1a2121;
        --name-air-surface-dim-color: #343a3b;

        --name-air-overlay-color: #00000050;

        --name-air-container-primary-color: #252b2c;
        --name-air-container-secondary-color: #161d1d;

        --name-air-decorative-color: #d7e3ff;
        --name-air-secondary-5-color: #21304c;

        --name-air-background-color: var(--name-air-container-primary-color);
    }
}
```

{% hint style="info" %}
If you only want to specify one theme and ignore dark/light, you can define the properties inside the `:root` without the `@media` block.
{% endhint %}

### Localization

Currently we only offer English out of the box, but you can change the English wording or add additional languages. Our SDK will pick the language based on the user's browser language. If it does not match any of our available language files, we will fallback to English or the language you have specified as default language.

Below you can find the two language files which can be adjusted or need to be provided for any additional language.

Login:

```json
{
  "login": {
    "agree": "By logging in, you agree to our {termsOfUse} & {privacyPolicy}",
    "termsOfUse": "Terms of Use",
    "privacyPolicy": "Privacy Policy",
    "termsOfUseUrl": "#/terms-of-use",
    "privacyPolicyUrl": "#/privacy-policy",
    "showSecondaryLogin": {
      "text": "Text Placeholder",
      "button": "Button Placeholder"
    },
    "loading": {
      "google": "Verify your Google account"
    },
    "emailVerification": {
      "loginToAirId": "You can also log into your Air ID with your email address provided below",
      "provideEmail": "Provide your email address"
    },
    "footer": "Powered by",
    "google": {
      "button": "Continue with Google",
      "error": "Google login failed. Please try again or use another login method."
    },
    "or": "or",
    "passkey": {
      "add": "Add Passkey",
      "authenticating": "Authenticating...",
      "button": "Continue with Passkey",
      "error": "Failed to login with passkey. Please try again or use another login method.",
      "notSupportedError": "Passkey login failed. Please try again or use another login method.",
      "register": "Register passkey",
      "skip": "Skip for now"
    },
    "passkeySetting": {
      "title": "Passkey Setting",
      "register": "Register New Passkey",
      "registering": "Registering...",
      "notSupportedError": "Your browser does not support passkeys.",
      "notRegistered": " No passkeys registered yet.",
      "totalPasskeys": "Total passkeys: {0}",
      "next": "Next",
      "go": "Go",
      "ofPages": "of {0}",
      "prev": "Prev",
      "delete": "Delete",
      "deleting": "Deleting...",
      "lastUsed": "Last used: {0}",
      "created": "Created: {0}",
      "failedToRegister": "Failed to register passkey. Please try again.",
      "failedToDelete": "Failed to delete passkey. Please try again.",
      "failedToLoad": "Failed to load passkeys. Please try again.",
      "cancelRegister": "Passkey registration was cancelled."
    },
    "passwordless": {
      "button": "Continue",
      "invalidError": "Invalid email address",
      "placeholder": "Enter email address",
      "requiredEmailError": "*Empty email"
    },
    "passwordlessOtp": {
      "enterEmail": "Please enter your email to continue",
      "title": "Enter email verification code",
      "description": "Enter the code we sent to {0}",
      "resendCountdown": "Resend code ( {0}s )",
      "resend": "Resend code",
      "invalidCode": "Invalid code, please try again",
      "expiredCode": "Code expired",
      "invalidFormat": "Code should be numeric only",
      "unknownError": "Unknown error",
      "tooManyRequests": "Oops! You made too many requests, please retry later.",
      "maxAttemptsExceeded": "Oops! Too many attempts, please retry later.",
      "checkSpam": "Check your spam folder if you can't find it in your inbox."
    },
    "title": "Log in / Sign up",
    "wallet": {
      "button": "Continue with a wallet",
      "checkWallet": "Check your {0}",
      "connectedWallet": "Connected with {0}",
      "confirmConnection": "Confirm connection in your wallet",
      "failedToConnect": "Failed to connect with {0}",
      "signMessage": "Sign to verify you own this wallet",
      "tryAgain": "Try again",
      "selectWallet": "Select a Wallet",
      "installed": "Installed",
      "recommended": "Recommended"
    },
    "error":{
      "rateLimitError":"Too many requests have been sent to us. Please try again later."
    }
  }
}
```

Wallet Services:

```json
{
  "signMessage": {
    "loading": "Loading...",
    "title": "Signature Request",
    "description": "Only sign this message if you fully understand the content and trust the requesting site.",
    "messageTitle": "Message to sign:",
    "noFeeRequired": "This action does not cost any fee.",
    "button": {
      "confirm": "Confirm",
      "cancel": "Cancel"
    }
  },
  "claim": {
    "claimId": {
      "chooseUsername": "Choose a username for your {{node}} ID",
      "chooseUsernameDesc": "This username will be used across the ecosystem and cannot be changed later on.",
      "claimIdButton": "Claim {{node}} ID",
      "createId": "Creating {{node}} ID",
      "usernameInputPlaceholder": "Username",
      "usernameRequirement1": "Only made of letters or numbers",
      "usernameRequirement2": "Between {{minLength}} to {{maxLength}} characters",
      "usernameRequirements": "Your username must be:"
    },
    "claimIdResult": {
      "claimIdSuccess": "Successfully created your ID",
      "done": "Done",
      "airAccountAddress": "Your Air Account Address",
      "viewResult": "View On {{blockExplorerName}}"
    },
    "claimIdRetry": {
      "almostThere": "Almost there",
      "claimIdRetryReason": "There might have been a network interruption while minting your {{node}} ID, click below to try again.",
      "retry": "Retry"
    }
  },
  "signTransaction": {
    "loading": "Loading...",
    "warning": " Once confirmed, the transaction cannot be canceled.",
    "cancel": "Cancel",
    "confirm": "Confirm",
    "confirmTransaction": " Confirm Transaction",
    "preview": {
      "title": "Preview transaction details:",
      "hexData": "Hex Data",
      "sendingAmount": "Sending amount",
      "gasFee": "Gas fee",
      "total": "Total"
    }
  },
  "error": {
    "back": "Back",
    "oops": "Oops",
    "pleaseTryAgain": "Please try again",
    "rateLimitError":"Too many requests have been sent to us. Please try again later."
  }
}

```
