# Setup dynamic link by banch.io

> react-native-branch ^6.2.2

## Configure [Branch](dashboard.branch.io) dashboard

1. Configure Branch Dashboard

- iOS [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/ios-basic-integration#1-configure-branch-dashboard)
- Android [Branch Dashboard Configuration](https://help.branch.io/developers-hub/docs/android-basic-integration#1-configure-branch-dashboard)

Make sure to [configure your default link settings](https://help.branch.io/using-branch/docs/configure-default-link-behaviors) as part of the setup process.

2. Install Branch

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

3. Configure App

### For Android

1. [Add](https://help.branch.io/developers-hub/docs/android-basic-integration#3-add-dependencies) dependencies.
2. [Configure](https://help.branch.io/developers-hub/docs/android-basic-integration#3-add-dependencies) `AndroidManifest.xml` file.
3. Add a branch.json file to your project, which you will use to access certain Branch configuration settings.
   Create an empty file called `branch.json`.
   Place the file in the `src/main/assets` folder of your app.

## validate SDK Integration

```bash
    IntegrationValidator.validate(this)
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
