# Usage

Once the SDK is installed and the `AirService` successfully initialized, it can be used to authenticate users. Further, the native provider given by the service instance can be used to let users interact with the blockchain.

The `AirService` instance offers following:

| Method                   | Description                                                                                                                                                                                                                                                                      |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| init(…)                  | The init method should generally be called right after creating the `AirService` instance.                                                                                                                                                                                       |
| login(…)                 | This will login the user with their Global ID.                                                                                                                                                                                                                                   |
| getUserInfo()            | Retrieves general user information.                                                                                                                                                                                                                                              |
| goToPartner(...)         | Share the user's session from your dApp to another AIR Kit-enabled dApp.                                                                                                                                                                                                         |
| getProvider()            | <p>Returns <code>Eip1193Provider</code> compatible with <a href="https://github.com/web3/web3.js">web3</a>, <a href="https://github.com/ethers-io/ethers.js">ethers</a>, <a href="https://viem.sh/">Viem</a>, etc.<br>This call will also load the wallet in the background.</p> |
| preloadWallet()          | Loads the wallet in the background. Needs the user to be logged in.                                                                                                                                                                                                              |
| setupOrUpdateMfa()       | Triggers the MFA enrollment screen where the user can set up or update the Passkey.                                                                                                                                                                                              |
| claimAirId(…)            | After the user is logged in, this will start the mint flow. During this call the wallet will be loaded if not yet happened.                                                                                                                                                      |
| isSmartAccountDeployed() | Check if the AA has been deployed. During this call the wallet will be loaded if not yet happened.                                                                                                                                                                               |
| deploySmartAccount()     | Deploy the AA if not yet deployed. It will also automatically be deployed when minting an Air Id. During this call the wallet will be loaded if not yet happened.                                                                                                                |
| logout()                 | Logs out the user and clears up the session                                                                                                                                                                                                                                      |
| clearInit()              | Clears the initialization of the `AirService`                                                                                                                                                                                                                                    |
| on(…) / off(…)           | To keep in sync with the service's state, you can subscribe to it and receive events such as `initialized`, `logged_in`, `wallet_initialized`\*, `air_id_minting_started`\*, `air_id_minting_failed`\* and `logged_out`.                                                         |

### Login & Mint

```typescript
login(options?: { authToken?: string; })
: Promise<AirLoginResult>
```

Without any parameters, this will trigger the default Air login dialog which provides different login methods for the user to choose from.\
We recommend to always require passing in a signed JWT via the `authToken` parameter with at minimum your `partnerId` inside the payload. This would also trigger the default login but enhances security. If you're having your users already authenticated on your side, you can add the `email` and `partnerUserId` to the payload as well.

{% hint style="warning" %}
If an email is provided, we will verify via a one-time password sent to this email since it is used as identifier on our side.
{% endhint %}

An example JWT could look like following:

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  "partnerId": "YOUR PARTNER ID",
  "partnerUserId": "YOUR USER ID", // optional
  "email": "YOUR USER EMAIL", // optional
  "exp": 1728973684,
  "iat": 1728970084
}
</code></pre>

In order to validate the JWTs on our end, we need to know your JWKS endpoint with following JSON:

```json
{
  "keys": [
    {
      "kty": "EC",
      "use": "sig",
      "alg": "ES256",
      "kid": "{your_kid}",
      "crv": "P-256",
      "x": "{your_x}",
      "y": "{your_y}"
    }
  ]
}
```

{% hint style="danger" %}
The above JWKS should contain the public key only, but the JWT generated on your end needs to be signed with the private key.
{% endhint %}

{% hint style="info" %}
If you don't want to provide a JWKS endpoint, you can also provide us the public key directly.
{% endhint %}

After successful login an `AirLoginResult` will be returned which also contains a property `token` generated by us containing following information:

```typescript
{
  sub: string,
  abstractAccountAddress: string,
  partnerId: string,
  partnerUserId?: string
}
```

This token can be used in the future to query some of our partner endpoints.

{% hint style="info" %}
You can validate our token by using following JWKS endpoint:  [https://static.air3.com/.well-known/jwks.json](https://static.air3.com/.well-known/jwks.json)
{% endhint %}

### User Session Across AIR Kit dApps

To maintain a user's logged in state across dApps in our ecosystem, we offer a convenient way to share the user's session from your dApp to another AIR Kit-enabled dApp.

From your dApp, call `goToPartner(partnerUrl: string)` and pass in the target URL of the other dApp to receive an updated url which can be used to carry over the user. Once the user lands on the target dApp, the user session will automatically be rehydrated during SDK initialization.

Example:

```typescript
const { urlWithToken } = await airService.goToPartner('https://www.partner-dapp.xyz/stake');
document.window.href = urlWithToken;
```

{% hint style="warning" %}
This method should be called at the time of navigating to the target url and not when displaying the link because the token attached to the url is short-lived. The target url also cannot be `localhost`.

Additionally, the target dApp needs to make sure to allow automatic rehydration by not setting `skipRehydration` to `true` when calling `init(...)`.
{% endhint %}

### Handling MFA Requirements

In order to make use of any wallet functionality, the user needs to have set up MFA which currently requires Passkey. After the user has set up the Passkey, any wallet action which requires signing or confirming a transaction will require users to verify their Passkey. This ensures funds are protected and only accessible by the user.

By default, the MFA enrollment screen will automatically pop up whenever the user has not set up MFA yet and a wallet action is triggered. This also includes getting the accounts and balance checks.

The MFA screen can also be programmatically triggered by calling `setupOrUpdateMfa()` before doing any wallet related actions if more control over the time of the MFA enrollment is needed.

The example below illustrates a potential way of handling the MFA setup:

```typescript
const loggedInUser = await airService.login();

// The wallet initialization can take a few seconds, so we can preload the wallet
// in the background before we need it
airService.preloadWallet();

// Some time later...
// If the user has not set up MFA yet and we want to trigger it now, we can do
if(!loggedInUser.isMFASetup) {
    await airService.setupOrUpdateMfa();
}

// At this point we're ready to use the wallet
```

{% hint style="warning" %}
As long as the user has not set up MFA, the wallet address will not be returned via login, user info or token.
{% endhint %}

### Minting&#x20;

#### Mint Settings

In case a partner wants to control certain mint behaviors (e.g. gated minting) a JWT token can be passed in with following properties:

```json
{
  "partnerId": "YOUR PARTNER ID",
  "partnerUserId": "YOUR USER ID",
  "eligibility": "none" | "normal" | "specific",
  "mintName": "name" | null
}
```

Eligibility cases:

`none`: User is not eligible to mint new names (can login only)\
`normal`: User can mint any name (except our internally reserved names)\
`specific`: User can only mint the specified name in `mintName`

{% hint style="info" %}
If `eligibility` is set to `normal` but the `mintName` is not null, the specified name will be prefilled but not enforced.
{% endhint %}

#### Reserved Names

During the mint flow we are doing a profanity check on the name and also a check against our internal reserved names list. If a partner has its own reserved names list, we can call a specified url on the partner side to additionally check the eligibility of each mint.

The partners needs to provide following url and optionally api key and name:

```
GET partner_url
Header { [api_key_name]: api_key }
```

Query params:

```
partnerId
name
email?
walletAddress?
token?
```

Response:

```
{
 "eligible": true | false;
}
```

{% hint style="warning" %}
We call the provided partner endpoint after we have done the profanity and reserved names check on our side. In case we cannot reach the endpoint, the mint request will be aborted.
{% endhint %}

### Interact with blockchain

{% hint style="info" %}
To interact with the blockchain, users need to log in and mint an Air ID, which you can explore further in [above section](usage.md#login-and-mint).
{% endhint %}

The provider returned from the service is a compatible `Eip1193Provider` and can be used with [web3](https://github.com/web3/web3.js), [ethers](https://github.com/ethers-io/ethers.js), [Viem](https://viem.sh/), etc.&#x20;

To interact with blockchain data, libraries such as **Ethers.js**, **Viem**, and **Web3.js** can be used. Here are some small snippets for signing, querying, and mutating simple functions on the blockchain.&#x20;

#### Sample smart contract

Consider a sample ERC20 smart contract with a minimal setup, including `balanceOf`, `symbol`, and `mint` functions

Make sure to replace <mark style="color:orange;">`{CONTRACT_ADDRESS}`</mark> and <mark style="color:orange;">`{CHAIN_ID}`</mark> with your corresponding values.

```typescript
const MOCK_TOKEN_CONTRACT = {
  address: "{CONTRACT_ADDRESS}",
  abi: [
    "function balanceOf(address account) view returns (uint256)",
    "function symbol() view returns (string)",
    "function mint(address to, uint256 amount)",
  ],
  chainId: {CHAIN_ID},
};
```

{% hint style="info" %}
The SDK manages two types of accounts:

1. **Smart Account (AA)**

The Realm SDK’s smart contract account complies with the ERC-4337 standard, supporting gas sponsorship via paymasters and other AA-related features. User assets are securely stored within this AA.

2. **Signer Account (EOA)**

The signer account is used for signing transactions and messages and controls the Smart Account (AA).

* For users logging in with a wallet (e.g., MetaMask), this account directly controls the AA.
* For users utilizing social login, a multi-party computation (MPC) wallet is constructed to manage the AA.
{% endhint %}

{% tabs %}
{% tab title="Ethers" %}
**Wallet client**

```typescript
const airProvider = await service.getProvider();
const walletClient = createWalletClient({
  transport: custom(airProvider),
  chain: baseSepolia,
});
```

**Sign a Message**

```javascript
const airProvider = await service.getProvider();
const ethProvider = new BrowserProvider(airProvider, "any");
const signer = await ethProvider.getSigner();
const signedMessage = await signer.signMessage(message);
```

**Query the Blockchain**

```javascript
const airProvider = await service.getProvider();
const ethProvider = new BrowserProvider(airProvider, "any");
const signer = await ethProvider.getSigner();
const contract = new ethers.Contract(
  MOCK_TOKEN_CONTRACT.address,
  MOCK_TOKEN_CONTRACT.abi,
  signer
);

const result = await contract.balanceOf(await signer.getAddress());
```

**Mutation (Send a Transaction)**

<pre class="language-javascript"><code class="lang-javascript">const airProvider = await service.getProvider();
<strong>const ethProvider = new BrowserProvider(airProvider, "any");
</strong>
const accounts = await ethProvider.listAccounts(); 
const signer = accounts[0]; // The first account will be the AA account address, and the second will be the EOA/MPC signer
const contractAddress = MOCK_TOKEN_CONTRACT.address;
const abi = MOCK_TOKEN_CONTRACT.abi;
const contract = new ethers.Contract(contractAddress, abi, signer);

const tx = await contract.mint(await signer.getAddress(), ethers.parseUnits(amount, 18)); // Assuming 18 decimals
await tx.wait(); // Wait for the transaction to be mined\
</code></pre>
{% endtab %}

{% tab title="Viem" %}
**Wallet client**

For signing and creating transactions, you will need a <mark style="color:orange;">`walletClient`</mark>.

```typescript
const airProvider = await service.getProvider();
const walletClient = createWalletClient({
  transport: custom(airProvider),
  chain: baseSepolia,
});

const [aaAccount] = await walletClient.getAddresses();
```

**Sign a Message**

```javascript
const signedMessage = await walletClient.signMessage({
  account: aaAccount,
  message: message,
});
```

**Query the Blockchain**

You can use <mark style="color:orange;">`publicClient`</mark> for querying, having a wallet is not required.

```typescript
const airProvider = await service.getProvider();
const publicClient = createPublicClient({
    transport: custom(airProvider),
    chain: baseSepolia,
});

const balance = await publicClient.readContract({
  abi: parseAbi(MOCK_TOKEN_CONTRACT.abi),
  address: MOCK_TOKEN_CONTRACT.address as Address,
  functionName: "balanceOf",
  args: [aaAccount],
});
```

**Mutation (Send a Transaction)**

<pre class="language-javascript"><code class="lang-javascript"><strong>await walletClient.writeContract({
</strong>  abi: parseAbi(MOCK_TOKEN_CONTRACT.abi),
  address: MOCK_TOKEN_CONTRACT.address as Address,
  functionName: "mint",
  args: [aaAccount, parseEther("1")],
  account: aaAccount,
});
</code></pre>
{% endtab %}

{% tab title="Web3" %}
**Web3 instance:**

```typescript
const airProvider = await service.getProvider();
const web3 = new Web3(airProvider);
const aaAddress = embed.walletAddresses?.aa;
```

**Sign a Message**

<pre class="language-javascript"><code class="lang-javascript"><strong>const message = "Hello, world!";
</strong>const passphrase = "";

const signature = await web3.eth.personal.sign(
  message,
  aaAddress,
  passphrase
);
</code></pre>

**Query the Blockchain**

You can use <mark style="color:orange;">`publicClient`</mark> for querying, having a wallet is not required.

```typescript
const abi = parseAbi(MOCK_TOKEN_CONTRACT.abi);

const contract = new web3.eth.Contract(
  abi as unknown as AbiFragment[],
  MOCK_TOKEN_CONTRACT.address
);

const balance = await contract.methods.balanceOf(account).call();
```

**Mutation (Send a Transaction)**

<pre class="language-javascript"><code class="lang-javascript">const abi = parseAbi(MOCK_TOKEN_CONTRACT.abi);

const contract = new web3.eth.Contract(
  abi as unknown as AbiFragment[],
  MOCK_TOKEN_CONTRACT.address
);

<strong>const tx = await contract.methods
</strong>  .mint(account, web3.utils.toWei("1", "ether"))
  .send({
    from: account,
  });
</code></pre>
{% endtab %}
{% endtabs %}
