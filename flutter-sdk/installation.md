# Installation

### Installation

Before you can install the AirKit Flutter package, you need to install the OnePub CLI in order to access our private package repository. We will provide you with the login credentials for OnePub.

Open your terminal and use following commands:

```
dart pub global activate onepub
onepub login
```

{% hint style="warning" %}
In case the `onepub` command is not recognized, adding following to your PATH should help:\
`export PATH=$PATH:$HOME/.pub-cache/bin`
{% endhint %}

To install the AirKit Flutter package, you have two options. You can either manually add the package in the `pubspec.yaml` file, or you can use the `onepub pub add` command.

Add `airkit` as a dependency to your `pubspec.yaml`.

```yaml
dependencies:
  airkit:
    hosted: https://onepub.dev/api/sxhddavuhn/
    version: ^0.2.0
```

Add `airkit` using `onepub pub add` command.

```
onepub pub add airkit
```

### Android Configuration <a href="#android-configuration" id="android-configuration"></a>

#### Update compileSdkVersion <a href="#update-compilesdkversion" id="update-compilesdkversion"></a>

For Android build `compileSdkVersion` needs to be at least `34` and `minSdk` needs to be at least `26`. Check your app module gradle file in your project to change it.

```gradle
android {
    namespace "com.example.app_name"
    compileSdkVersion 34
    // ..

    defaultConfig {
        minSdk = 26
        // ..
    }
}
```

#### Update Permissions <a href="#update-permissions" id="update-permissions"></a>

Open your app's `AndroidManifest.xml` file and add the following permission. Please make sure the `<uses-permission>` element should be a direct child of the `<manifest>` root element.

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

#### Configure Redirection and Deep Linking <a href="#configure-redirection" id="configure-redirection"></a>

Open your app's `AndroidManifest.xml` file and add the following deep link intent filter to your Main activity based on `{scheme}://{your_app_package}/auth`.

```xml
<intent-filter>
  <action android:name="android.intent.action.VIEW" />

  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />

  <data android:scheme="{scheme}" android:host="{your_app_package}" android:path="/auth" />
  <!-- E.g.: air://com.example.airkit/auth -->
</intent-filter>
```

{% hint style="warning" %}
We need to know the final deep link in order to whitelist on our end.
{% endhint %}

### iOS Configuration <a href="#ios-configuration" id="ios-configuration"></a>

#### Update global iOS platform <a href="#update-global-ios-platform" id="update-global-ios-platform"></a>

For iOS build global platform needs to be 14.0. Check `Podfile` in your project to change the global platform.

```
platform :ios, '14.0'
```

#### Configure Redirection and Deep Linking <a href="#configure-redirection" id="configure-redirection"></a>

For iOS we need to know your final `bundleId` in order to whitelist on our end. The final URI will look like following: `{bundleId}://auth`.
