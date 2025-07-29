# Usage

### Initialize Flutter SDK

After installation, the next step to use the AirKit SDK is to create an `AirService` instance and call the `initialize` method.

The `partnerId` you will receive from us and the `redirectUrl` parameter needs to match the schema discussed in the installation section. It is different for Android and iOS.

```dart
Future<void> initialize({
    required String partnerId,
    required GlobalKey<NavigatorState> navigatorKey,
    Environment env = Environment.production,
    bool enableLogging = false,
})
```

The `partnerId` you will receive from us and the `navigatorKey` parameter is needed to overlay dialogs necessary for specific user flows. During development we recommend using `Environment.sandbox` and enabled logging.

### Rehydration

In case a user should be rehydrated after e.g. an app restart, `rehydrate` can be called to automatically login a previously logged in user with an existing valid session. Otherwise login needs to be called.

### Login

Before the `login` method can be used, custom auth details need to be handed over to us first. \
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

Currently, the email address needs to be used as identifier inside the JWT which will be generated on your side.

An example JWT could look like following:

```json
{
  "partnerId": "YOUR PARTNER ID",
  "partnerUserId": "YOUR USER ID", // optional
  "email": "YOUR USER EMAIL",
  "exp": 1728973684,
  "iat": 1728970084
}
```

### State Events

You can listen to `AirService` state events either via exposed `Stream` or  `on` / `off` / `clearEventListeners` methods. A convenient switch block helps with typing.

```dart
void eventListener(AirEvent event) {
    switch (event) {
      case AirInitializedEvent():
        log("Service initialized");
      case AirLoggedInEvent(:final payload):
        log("User logged in as: ${payload.toString()}");
      case AirLoggedOutEvent():
        log("User logged out");
      case AirWalletInitializedEvent():
        log("Wallet initialized");
    }
});

airService.airEvents.listen(eventListener);

airService.on(eventListener);
airService.off(eventListener);
airService.clearEventListeners();
```

### Mint Realm ID <mark style="color:orange;">(⚠️ not ready yet in 1.5.x)</mark>

Calling `claimId` will start the mint flow which mints the Air ID based on your node (e.g. `.moca`) and also creates the AA for the user.\
If you want to have more control over the gating process, please refer to [following section](https://realm-network.gitbook.io/realm-sdk/web-sdk/usage#mint-settings) which explains how to setup a claim token.

