# Reference

### Air Service

```dart
class AirService {

  Stream<AirEvent> get airEvents;

  void on(AirEventListener listener);
  void off(AirEventListener listener);
  void clearEventListeners();
  
  bool get isInitialized;

  Future<void> initialize({
    required String partnerId,
    Environment env = Environment.production,
    required GlobalKey<NavigatorState> navigatorKey,
    bool enableLogging = false,
  });

  Future<LoginResult> login({
    required String authToken,
    OnOtpRequest? onOtpRequest,
  });

  Future<LoginResult> rehydrate();

  Future<UserInfo> getUserInfo();

  Future<void> preloadWallet();

  Future<String?> getAbstractAccountAddress();

  Future<List<String>> getAccounts();

  Future<void> setupOrUpdateMfa();

  Future<BigInt> getBalance(String address);

  Future<EthereumRpcSuccessResponse> call(
    String address,
    String function,
    List<dynamic> params,
    String abi,
  );

  Future<String> signMessage(String message);

  Future<String> sendTransaction(Transaction transaction);

  Future<EthereumRpcSuccessResponse> sendEthereumRpcRequest(
    EthereumRpcRequest request
  );

  Future<String> deploySmartAccount();

  Future<bool> isSmartAccountDeployed();

  Future<void> showSwapUi();

  Future<void> showOnRampUi({
    required String displayCurrencyCode,
    String? targetCurrencyCode,
  });

  Future<void> logout();

  void cleanup();
```

### Models

```dart

enum Environment { staging, uat, sandbox, production }
​
class LoginResult {
  final bool isLoggedIn;
  final String? id;
  final String? abstractAccountAddress;
  final String? token;
  final bool? isMFASetup;
}
​
enum AirIdStatus {
  minting,
  minted,
}
​
class AirId {
  final String id;
  final String name;
  final String node;
  final AirIdStatus status;
  final int? chainId;
  final String? imageUrl;
}
​
class UserInfo {
  final AirId? airId;
  final String? partnerUserId;
  final String? partnerId;
  final User? user;
}
​
class User {
  final String id;
  final String? abstractAccountAddress;
  final String? email;
  final bool isMFASetup;
}
​
class EthereumRpcRequest {
  final String method;
  final List<dynamic> params;
  final String? requestId;
}

class EthereumRpcSuccessResponse {
  final dynamic response;
}

class AirEvent { }

class AirInitializedEvent extends AirEvent { 

class AirLoggedInEvent extends AirEvent {
  final LoginResult payload;
}

class AirLoggedOutEvent extends AirEvent { }

class AirWalletInitializedEvent extends AirEvent { }

typedef AirEventListener = void Function(AirEvent event);

enum ExceptionType {
  client,
  sdk,
  server,
  unknown,
}

class AirKitException implements Exception {
  final String message;
  final ExceptionType type;
}
```
