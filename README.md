# Setup dynamic link by banch.io

> react-native-branch ^6.2.2

## Configure [Branch](dashboard.branch.io) dashboard

# 1. Configure Branch Dashboard

- iOS [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/ios-basic-integration#1-configure-branch-dashboard)
- Android [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/android-basic-integration#1-configure-branch-dashboard)

Make sure to [configure your default link settings](https://help.branch.io/using-branch/docs/configure-default-link-behaviors) as part of the setup process.

# 2. Install Branch

```bash
    yarn add react-native-branch
```

## iOS App With CocoaPods

```bash
    platform :ios, '11.0'

    target 'APP_NAME' do
      # if swift
      use_frameworks!

      pod 'react-native-branch', path: '../node_modules/react-native-branch'
    end
```

# 3. Configure App

### iOS Configuration

1. [Add](https://help.branch.io/developers-hub/docs/android-basic-integration#3-add-dependencies) dependencies.
2. [Configure](https://help.branch.io/developers-hub/docs/android-basic-integration#3-add-dependencies) `AndroidManifest.xml` file.
3. Add a branch.json file to your project, which you will use to access certain Branch configuration settings.
   - Create an empty file called `branch.json`.
   - Place the file in the `src/main/assets` folder of your app.

### Android Configuration

1. [Configure](https://help.branch.io/developers-hub/docs/ios-basic-integration#2-configure-bundle-identifier) bundle identifier.
2. [Configure](https://help.branch.io/developers-hub/docs/ios-basic-integration#3-configure-associated-domains) associated domains.
3. [Configure](https://help.branch.io/developers-hub/docs/ios-basic-integration#4-configure-infoplist) `Info.plist` file.
4. Add a `branch.json` file to your project, which you will use to access certain Branch configuration settings:
   - Create an empty file called `branch.json`.
   - Add the file to your project using Xcode. Within your project, navigate to **File → Add Files**.
   - Select the `branch.json` file and make sure every target in your project that uses Branch is selected.
   - Click **Add**.

# 4. Initialize Branch

### Branch Initialization on iOS

To initialize Branch on iOS, add the following to your app's AppDelegate file:

- Import RNBranch

```c
#import <RNBranch/RNBranch.h>
```

- Initialize the Branch Session

```c
func application(_ application: UIApplication, didFinishLaunchingWithOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
  // RNBranch.useTestInstance()
  RNBranch.initSession(launchOptions: launchOptions)

  return true
}
```

- Add openURL() Method

```c
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  RNBranch.application(app, open:url, options:options)
  return true
}
```

- Add continueUserActivity() Method

```c
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
  RNBranch.continue(userActivity)
  return true
}
```

### Branch Initialization on Android

To initialize Branch on Android, you need to:

1. Add Branch to your `MainApplication.kt` file (or `MainApplication.java` for older apps):

- Import from RNBranch

```kotlin
import io.branch.rnbranch.*
```

- Override onCreate() Function

```kotlin
override fun onCreate() {
    super.onCreate()
    RNBranchModule.getAutoInstance(this)

    SoLoader.init(this, false)
    if (BuildConfig.IS_NEW_ARCHITECTURE_ENABLED) {
        // If you opted-in for the New Architecture, we load the native entry point for this app
        load()
    }
}
```

- Enable Logging (Enable Branch logging for debugging)

```kotlin
override fun onCreate() {
    super.onCreate()
    RNBranchModule.getAutoInstance(this)

    SoLoader.init(this, false)
    if (BuildConfig.IS_NEW_ARCHITECTURE_ENABLED) {
        // If you opted-in for the New Architecture, we load the native entry point for this app
        load()
    }

    RNBranchModule.enableLogging(); <--this code-->

}
```

2. Add Branch to your `MainActivity.kt` file (or `MainActivity.java` for older apps):

- Import from RNBranch

```kotlin
import io.branch.rnbranch.*
```

- Override onStart() Function

```kotlin
override fun onStart() {
    super.onStart()
    RNBranchModule.initSession(getIntent().getData(), this)
}
```

- Override onNewIntent() Function

```kotlin
override fun onNewIntent(intent: Intent?) {
    super.onNewIntent(intent)
    setIntent(intent)
    RNBranchModule.reInitSession(this)
}
```

# 5. Validate Integration

### For iOS

1. The [Integration Status](https://help.branch.io/developers-hub/docs/ios-testing#integration-status-tab) tab in the [Branch Dashboard](https://dashboard.branch.io/integration?_gl=1*1re0ydv*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzUyMDIuNjAuMC4w).

2. The Branch iOS SDK's Integration Validation method.

Another simple way to test the status of your Branch iOS SDK integration is using the built-in `validateSDKIntegration()` method.

To use this method, add the following code to your AppDelegate file:

```c
[[Branch getInstance] validateSDKIntegration];
```

This method will check to ensure that the Branch keys, package name, URI schemes, Universal Links, and link domain settings from the Branch Dashboard match those in the build.

Check your Xcode logs to make sure all the SDK integration tests pass.

Make sure to comment out or remove `validateSDKIntegration()` in your production build.

For more about the `validateSDKIntegration()` method, visit our [blog](https://www.branch.io/resources/blog/how-to-validate-your-branch-mobile-sdk-integration/?_gl=1*12347gp*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzcyMDcuMzEuMC4w#:~:text=Step%202%3A%20Utilize%20Branch%E2%80%99s%20Integration%20Validator)

### For Android

1. The [Integration Status](https://help.branch.io/developers-hub/docs/ios-testing#integration-status-tab) tab in the [Branch Dashboard](https://dashboard.branch.io/integration?_gl=1*1re0ydv*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzUyMDIuNjAuMC4w).

2. The Branch Android SDK's Integration Validation method.

Another simple way to test the status of your Branch Android SDK integration is using the built-in `IntegrationValidator.validate()` method.

To use this method, add the following code to your MainActivity's onStart():

```kotlin
import io.branch.referral.validators.IntegrationValidator

...

override fun onStart() {
    super.onStart()
    RNBranchModule.initSession(getIntent().getData(), this)
    IntegrationValidator.validate(this) <--- this code --->
  }
```

This method will check to ensure that the Branch keys, package name, URI schemes, and link domain settings from the Branch Dashboard match those in the build.

Check your ADB Logcat to make sure all the SDK integration tests pass.

Make sure to comment out or remove `IntegrationValidator.validate()` in your production build.

For more about the `IntegrationValidator.validate()` method, visit our [blog](https://www.branch.io/resources/blog/how-to-validate-your-branch-mobile-sdk-integration/?_gl=1*12347gp*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzcyMDcuMzEuMC4w#:~:text=Step%202%3A%20Utilize%20Branch%E2%80%99s%20Integration%20Validator)
