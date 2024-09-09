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

## validate SDK Integration

```javascript
IntegrationValidator.validate(this);
```

------------------- Initiating Branch integration verification --------------------------- ...

> - Verifying Branch instance creation ... Passed
> - Checking Branch keys ... Passed
> - Verifying application package name ... Passed
> - Checking Android Manifest for URI based deep link config ... Passed
> - Verifying URI based deep link config with Branch dash board. ... Passed
> - Verifying intent for receiving URI scheme. ... Passed
> - Checking AndroidManifest for AppLink config. ... Passed
> - Verifying any supported custom link domains. ... Passed
> - Verifying default link domains integrations. ... Passed
> - Verifying alternate link domains integrations. ... Passed Passed

### For iOS
