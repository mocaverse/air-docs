# Quickstart Guide

The **AIR Credential SDK** provides a convenient way to issue and verify digital credentials directly in your web application using a pre-built widget. The widget handles end-to-end flows, including UI presentation and cryptographic verification, streamlining integration with your decentralized app (dApp).&#x20;



### **Installation**

Install the AIR Credential SDK&#x20;

```shell
# Using npm
npm install @mocanetwork/air-credential-sdk
```

### **Getting Started**

**Import styles**

To ensure the widget displays correctly, import the SDK’s CSS file early in your application:

```typescript
import '@mocanetwork/air-credential-sdk/dist/style.css'
```

## **Prepare Auth Token**

```
API_URL=https://credential.api.sandbox.air3.com
```

### **Get Credential SDK's AuthToken from API Key (Issuer)**

```typescript
const getIssuerAuthToken = async (issuerDid: string, apiKey: string, apiUrl: string): Promise<string | null> => {
  try {
    const response = await fetch(`${apiUrl}/issuer/login`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        accept: "*/*",
        "X-Test": "true",
      },
      body: JSON.stringify({
        issuerDid: issuerDid,
        authToken: apiKey,
      }),
    });
    if (!response.ok) {
      throw new Error(`API call failed with status: ${response.status}`);
    }
    const data = await response.json();
    if (data.code === 80000000 && data.data && data.data.token) {
      return data.data.token;
    } else {
      console.error("Failed to get issuer auth token from API:", data.msg || "Unknown error");
      return null;
    }
  } catch (error) {
    console.error("Error fetching issuer auth token:", error);
    return null;
  }
};
```



### **Get Credential SDK's AuthToken from API Key (Verifier)**

```typescript
const getVerifierAuthToken = async (verifierDid: string, apiKey: string, apiUrl: string): Promise<string | null> => {
  try {
    const response = await fetch(`${apiUrl}/verifier/login`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        accept: "*/*",
        "X-Test": "true",
      },
      body: JSON.stringify({
        verifierDid: verifierDid,
        authToken: apiKey,
      }),
    });
    if (!response.ok) {
      throw new Error(`API call failed with status: ${response.status}`);
    }
    const data = await response.json();
    if (data.code === 80000000 && data.data && data.data.token) {
      return data.data.token;
    } else {
      console.error("Failed to get verifier auth token from API:", data.msg || "Unknown error");
      return null;
    }
  } catch (error) {
    console.error("Error fetching verifier auth token:", error);
    return null;
  }
};
```

### **Get AirKit Cross Partner Token**

```typescript
const goToPartnerResult = await airService?.goToPartner(environmentConfig.widgetUrl).catch((err) => {
  console.error("Error getting URL with token:", err);
});
```

## **Issuing a Credential**

#### **1. Prepare Credential Data & Request**

Define the credential subject according to your dashboard's scheme and construct the claim request.

```typescript
import {
  type JsonDocumentObject,
  AirCredentialWidget,
} from '@mocanetwork/air-credential-sdk';

// Define the credential subject with your custom claims
const credentialSubject: JsonDocumentObject = {
  birthday: 20020101,
  documentType: 99
};

// Construct the claim request for issuing a credential
const claimRequest = {
  process: 'Issue',
  issuerDid: 'Your issuer DID',
  issuerAuth: await getIssuerAuthToken(config.issuerDid, ISSUER_API_KEY, API_URL),
  credentialId: 'The credential id configured in the dashboard',
  credentialSubject
};
```

#### **2. Instantiate and Configure the Issuance Widget**

Create a new instance of the widget and pass any optional configurations (such as theme or locale).

```typescript
const airIssuanceWidget = new AirCredentialWidget(claimRequest, partnerId, {
  endpoint: goToPartnerResult?.urlWithToken, // result from air kit goToPartner func
  airKitBuildEnv: airKitBuildEnv || BUILD_ENV.SANDBOX,
  theme: "light", // currently only have light theme
  locale: LOCALE as Language
});
```

| Param          | Type                                 | Description                                        |
| -------------- | ------------------------------------ | -------------------------------------------------- |
| endpoint       | string?                              | URL to the widget endpoint.                        |
| theme          | Theme?                               | `"auto"`, `"light"` or `"dark"`, default `"auto"`. |
| locale         | Language?                            | `"en"` or `"zh-hk"`, default `"en"`                |
| airKitBuildEnv | the environment you want to work in  | Preferably use SANDBOX                             |

#### **3. Handle Issuance Events & Launch the Widget**

Register an event handler for when the issuance process completes, then launch the widget (for instance, on a button click).

```typescript
// Event listener for successful credential issuance
airIssuanceWidget.on('issueCompleted', () => {
  console.log('Credential issuance completed successfully.');
  // You can add additional success logic here, such as UI notifications.
  setIsSuccess(true);
  setIsLoading(false);
});


// Attach launch action to a button click event
const button = document.getElementById('launchIssueButton');
button.addEventListener('click', () => {
  airIssuanceWidget.launch();
});
```

## Verifying Credentials

The process of verifying a credential is similar. Follow the steps below:

#### **1. Configure the Query Request**

Set up the request parameters required for verification. Generate your API key [here](https://developers.sandbox.air3.com/api-key)

```typescript
import { AirCredentialWidget, type QueryRequest, type VerificationResults, type Language } from "@mocanetwork/air-credential-sdk";

//set your config
const config = {
  verifierAuth: "your-verifier-auth-token", // generate it on the dashboard
  programId: "program-123", // your program name
  partnerId: "parter-123"  // air partner id
  redirectUrlForIssuer: "" //where
};
// Step 1: Fetch the verifier auth token using the API key
const fetchedVerifierAuthToken = await getVerifierAuthToken(config.verifierDid, VERIFIER_API_KEY, API_URL);

const queryRequest: QueryRequest = {
  process: "Verify",
  verifierAuth: fetchedVerifierAuthToken, // obtained from the dashbaord
  programId: config.programId //configured on the dashboard
};
```

#### **2. Instantiate and Configure the Verification Widget**

Create the widget instance for verifying credentials.

```typescript
// Here's how you can set it up
const airVerifierWidget = new AirCredentialWidget(
  queryRequest,
  partnerId,
  options
);
```

| Param          | Type                                 | Description                                        |
| -------------- | ------------------------------------ | -------------------------------------------------- |
| theme          | Theme?                               | `"auto"`, `"light"` or `"dark"`, default `"auto"`. |
| locale         | Language?                            | `"en"` or `"zh-hk"`, default `"en"`                |
| airKitBuildEnv | the environment you want to work in  | Preferably use SANDBOX                             |

```typescript

 // Reference code to use
 const airVerifierWidget = new AirCredentialWidget(queryRequest, partnerId, {
  endpoint: goToPartnerResult?.urlWithToken, // result from air kit goToPartner func
  airKitBuildEnv: airKitBuildEnv || BUILD_ENV.SANDBOX,
  theme: "light", // currently only have light theme
  locale: LOCALE as Language,
  redirectUrlForIssuer: config.redirectUrlForIssuer || undefined
});
```

**3. Handle Verification Events & Launch**

Set up event handling for the verification process and launch the widget when appropriate.

```typescript
// Listen for the verification process completion
airVerifierWidget.on('verifyCompleted', (results: { status: string }) => {
  const { status } = results;
  if (status === 'Compliant') {
    // Add your success logic here, such as:
    // Updating UI to show verification success.
  } else if (status === 'Non-Compliant') {
    // Prompt the user that does not meet your verification conditions.
  } else {
    // See the table below for more details on other statuses.
  }
});

// Attach verification launch to a button click event
const verifyButton = document.getElementById('launchVerifyButton');
verifyButton?.addEventListener('click', () => {
// Start the widget
  airVerifierWidget.launch();
});
```

| Status   | Description                                                      |
| -------- | ---------------------------------------------------------------- |
| Pending  | The target credential is waiting for confirmation on blockchain. |
| Revoking | Target credentials are being revoked.                            |
| Revoked  | The target credentials have been revoked.                        |
| Expired  | The target credentials have expired.                             |
| NotFound | No such credentials were found.                                  |

You can refer to [this GitHub example](https://github.com/MocaNetwork/air-credential-example/) to test the Issuance and Verification of credentials locally.
