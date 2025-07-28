---
description: 'To get started following the steps below:'
---

# Quickstart

### Step 1: Install the SDK

```
npm install @mocanetwork/airkit
```

### Step 2: Get your partner ID

**To use the SDK, you can sign up through this form** [**here**](https://mocanetwork.typeform.com/airkitform) **to get the access code. We’ll be sharing a partner ID with you, using which you can start using the AIR Kit**

If you’re looking to customise the login flow/ the theme, and features you want to support, please submit your request through this email: `support@moca.network`&#x20;

You can customise the following parameters for the login functionality:

* Chain ID
* Custom Styling Config
  * CSS File
* Localisation support (if applicable)
  * language file

### Step 3: Import, Initialize & Login

```typescript
import { AirService, BUILD_ENV } from "@mocanetwork/airkit";

const service = new AirService({
  partnerId: YOUR_PARTNER_ID, // Replace with your actual Partner ID
  env: BUILD_ENV.SANDBOX
});

// Trigger the login flow
await service.init();
const loggedInResult = await service.login();
```

☑️ This will:

* Initialize the `AirService`  with the Sandbox environment that loads the login flow.
* Create a secure iframe for the login UI
* Handle authentication and session setup
* Enable safe communication between your app's JavaScript context and the iframe.

### What You Get After Login

Once the login is complete, you’ll have access to:

* **User UUID**
* **Session token/access metadata**
* **Verified identity context** (for credentialing and permissions)

### What’s Next?

* Add **credential issuing** and **proof verification** using the [Credential SDK](../air-credentials/core-technologies-and-key-components/issuer-module.md)
