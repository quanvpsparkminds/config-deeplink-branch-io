# Config deeplink using Branch.io

Register your [app](https://dashboard.branch.io/)

# 1. Configure Branch Dashboard

- iOS [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/ios-basic-integration#1-configure-branch-dashboard)
- Android [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/android-basic-integration#1-configure-branch-dashboard)

Make sure to [configure your default link settings](https://help.branch.io/using-branch/docs/configure-default-link-behaviors) as part of the setup process.

# 2. Install Branch

```bash
    yarn add react-native-branch
```
**Note**: The `react-native-branch` module requires your `react-native` version to be greater than or equal to `0.60`

## iOS App With CocoaPods

1. Add the following code to your Podfile in order to install Branch

```ruby
    platform :ios, '11.0'

    target 'APP_NAME' do
      # if swift
      use_frameworks!

      pod 'react-native-branch', path: '../node_modules/react-native-branch'
    end
```

2. Run the `pod install` command to regenerate the `Pods` project with the new dependencies. Please note that the location of `node_modules` relative to your Podfile may vary.

# 3. Configure App

### iOS Configuration

1. Configure bundle identifier.
  - Find the `bundle identifier` for the relevant target associated with your project in Xcode, under the ***Signing & Capabilities*** tab
  ![alt](https://ibb.co/yF8FVJH)
  - Return to the Configuration [page](https://dashboard.branch.io/configuration/general) of the Branch Dashboard, and use the ***Add New Bundle ID*** button to add your Bundle ID
2. Configure associated domains.
  - In your Branch Dashboard, navigate to the ***Link Domain*** section of the Configuration page.
  - Return to the ***Signing & Capabilities*** tab in Xcode, and add the domains from your Branch Dashboard to your project's target
    - Use `applinks:subdomain.app.link` for the format.
    - The `-alternate` flag is required to ensure proper functioning of Universal Links and [Deepviews](https://help.branch.io/using-branch/docs/deepviews) for users that do not have your app installed.
    - The `.test` flag is required if you need to use a test key.
**Please note**: if you use a [custom link domain](https://help.branch.io/using-branch/docs/advanced-settings-configuration#section-change-link-domain), you will need to include your old link domain, your `-alternate` link domain, and your new link domain in your project.
3. Configure `Info.plist` file.

Add the following code to your `info.plist` file

```xml
<plist version="1.0">
	<dict>
		<key>branch_universal_link_domains</key>
		<array>
			<string>example.app.link</string> <!-- REPLACE `example.app.link` with your `app.link` domain -->
			<string>example-alternate.app.link</string> <!-- REPLACE `example-alternate.app.link` with your `-alternate` domain (required for proper functioning of App Links and Deepviews) -->
			<string>example.test-app.link</string> <!-- REPLACE `example.test-app.link` with your `test-app.link` domain -->
		</array>
		<key>CFBundleURLTypes</key>
		<array>
			<dict>
				<key>CFBundleTypeRole</key>
				<string>Editor</string>
				<key>CFBundleURLSchemes</key>
				<array>
					<string>branchsters</string>
				</array>
				<key>CFBundleURLName</key>
				<string>io.Branch.Branchsters</string>
			</dict>
		</array>
		<key>branch_key</key>
		<dict>
			<key>live</key>
			<string>key_live_aaa000AAA</string> <!-- REPLACE `BranchKey` with the value from your Branch Dashboard -->
			<key>test</key>
			<string>key_test_bbb000BBB</string> <!-- REPLACE `BranchKey.test` with the value from your Branch Dashboard -->
		</dict>
	</dict>
</plist>
```
4. Add a `branch.json` file to your project, which you will use to access certain Branch configuration settings:
   - Create an empty file called `branch.json`.
   - Add the file to your project using Xcode. Within your project, navigate to **File → Add Files**.
   - Select the `branch.json` file and make sure every target in your project that uses Branch is selected.
   - Click **Add**.

### Android Configuration

1. [Add](https://help.branch.io/developers-hub/docs/android-basic-integration#3-add-dependencies) dependencies.
2. Configure `AndroidManifest.xml` file.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
	xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.android">
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
	<application
      		android:allowBackup="true"
      		android:name="com.example.android.CustomApplicationClass"
      		android:icon="@mipmap/ic_launcher"
      		android:label="@string/app_name"
      		android:supportsRtl="true"
      		android:theme="@style/AppTheme">
		<!-- Launcher Activity to handle incoming Branch intents -->
		<activity
        		android:name=".LauncherActivity"
        		android:launchMode="singleTask"
        		android:label="@string/app_name"
        		android:theme="@style/AppTheme.NoActionBar"
        		android:exported="true">

			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>

			<!-- Branch URI Scheme -->
			<intent-filter>
				<!-- If utilizing $deeplink_path please explicitly declare your hosts, or utilize a wildcard(*) -->
				<!-- REPLACE `android:scheme` with your Android URI scheme -->
				<data android:scheme="yourapp" android:host="open" />
				<action android:name="android.intent.action.VIEW" />
				<category android:name="android.intent.category.DEFAULT" />
				<category android:name="android.intent.category.BROWSABLE" />
			</intent-filter>

			<!-- Branch App Links - Live App -->
			<intent-filter android:autoVerify="true">
				<action android:name="android.intent.action.VIEW" />
				<category android:name="android.intent.category.DEFAULT" />
				<category android:name="android.intent.category.BROWSABLE" />
				<!-- REPLACE `android:host` with your `app.link` domain -->
				<data android:scheme="https" android:host="example.app.link" />
				<!-- REPLACE `android:host` with your `-alternate` domain (required for proper functioning of App Links and Deepviews) -->
				<data android:scheme="https" android:host="example-alternate.app.link" />
			</intent-filter>

			<!-- Branch App Links - Test App -->
			<intent-filter android:autoVerify="true">
				<action android:name="android.intent.action.VIEW" />
				<category android:name="android.intent.category.DEFAULT" />
				<category android:name="android.intent.category.BROWSABLE" />
				<data android:scheme="https" android:host="example.test-app.link" />
				<!-- REPLACE `android:host` with your `-alternate` domain (required for proper functioning of App Links and Deepviews) -->
				<data android:scheme="https" android:host="example-alternate.test-app.link" />
			</intent-filter>
		</activity>

		<!-- Branch init -->
		<!-- REPLACE `BranchKey` with the value from your Branch Dashboard -->
		<meta-data android:name="io.branch.sdk.BranchKey" android:value="key_live_XXX" />
		<!-- REPLACE `BranchKey.test` with the value from your Branch Dashboard -->
		<meta-data android:name="io.branch.sdk.BranchKey.test" android:value="key_test_XXX" />
		<!-- Set to `true` to use `BranchKey.test` -->
		<meta-data android:name="io.branch.sdk.TestMode" android:value="false" />

	</application>
</manifest>
```

3. Add a `branch.json` file to your project, which you will use to access certain Branch configuration settings.
   - Create an empty file called `branch.json`.
   - Place the file in the `src/main/assets` folder of your app.

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

- Add `openURL()` Method

```c
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  RNBranch.application(app, open:url, options:options)
  return true
}
```

- Add `continueUserActivity()` Method

```c
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
  RNBranch.continue(userActivity)
  return true
}
```

### Branch Initialization on Android

To initialize Branch on Android, you need to:

1. Add Branch to your `MainApplication.kt` file:

- Import from RNBranch

```kotlin
import io.branch.rnbranch.*
```

- Override `onCreate()` Function

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

2. Add Branch to your `MainActivity.kt` file:

- Import from RNBranch

```kotlin
import io.branch.rnbranch.*
```

- Override `onStart()` Function

```kotlin
override fun onStart() {
    super.onStart()
    RNBranchModule.initSession(getIntent().getData(), this)
}
```

- Override `onNewIntent()` Function

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

------------------- Initiating Branch integration verification ---------------------------

> - Checking for URI scheme correctness ... Passed
> - Checking for bundle identifier correctness ... Passed
> - Checking for iOS Team ID correctness ... Passed

**Make sure to comment out or remove** `validateSDKIntegration()` **in your production build.**

For more about the `validateSDKIntegration()` method, visit our [blog](https://www.branch.io/resources/blog/how-to-validate-your-branch-mobile-sdk-integration/?_gl=1*12347gp*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzcyMDcuMzEuMC4w#:~:text=Step%202%3A%20Utilize%20Branch%E2%80%99s%20Integration%20Validator)

### For Android

1. The [Integration Status](https://help.branch.io/developers-hub/docs/ios-testing#integration-status-tab) tab in the [Branch Dashboard](https://dashboard.branch.io/integration?_gl=1*1re0ydv*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzUyMDIuNjAuMC4w).

2. The Branch Android SDK's Integration Validation method.

Another simple way to test the status of your Branch Android SDK integration is using the built-in `IntegrationValidator.validate()` method.

To use this method, add the following code to your MainActivity's `onStart()`:

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

------------------- Initiating Branch integration verification ---------------------------

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

**Make sure to comment out or remove** `IntegrationValidator.validate()` **in your production build.**

For more about the `IntegrationValidator.validate()` method, visit our [blog](https://www.branch.io/resources/blog/how-to-validate-your-branch-mobile-sdk-integration/?_gl=1*12347gp*_ga*MzU4MjkwNDEuMTcyNDA0MTg3OA..*_ga_KSDD8Y11CT*MTcyNTg3MjE4Ni4xMy4xLjE3MjU4NzcyMDcuMzEuMC4w#:~:text=Step%202%3A%20Utilize%20Branch%E2%80%99s%20Integration%20Validator)
