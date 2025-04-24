# Reference

### Air Service

```dart
class AirService {

  static Future<void> initialize(
      {required String partnerId,
        required Uri redirectUrl,
        Environment env = Environment.production});

  static bool isLoggedIn();
  
  static Future<UserInfo> getUserInfo();
  
  static Future<LoginResult> rehydrate();

  static Future<LoginResult> login(
      {required String authToken, OnOtpRequest? onOtpRequest});

  static Future<void> logout();

  // below methods aren't ready in version 0.2.x
  static Future<LoginResult> claimId({String? token});

  static Future<WalletResponse> request(
      {required String method, List<dynamic> requestParams = const []});
}
```

### Models

```dart
enum Environment { staging, production }

class LoginResult {
  final bool isLoggedIn;
  final String? id;
  final String? abstractAccountAddress;
  final String? token;
}

enum AirIdStatus {
  minting,
  minted,
}

class AirId {
  final String id;
  final String name;
  final String node;
  final AirIdStatus status;
  final int? chainId;
  final String? imageUrl;
}

class UserInfo {
  final AirId? airId;
  final String? partnerUserId;
  final String? partnerId;
  final User? user;
}

class User {
  final String id;
  final String? abstractAccountAddress;
  final String? email;
}

class WalletResponse {
  final bool success;
  final String? result;
  final String? error;
}
```
