# Cordova Plugin

[![fastlane Plugin Badge](https://rawcdn.githack.com/fastlane/fastlane/master/fastlane/assets/plugin-badge.svg)](https://rubygems.org/gems/fastlane-plugin-cordova)

## Features

- Build your Cordova project inside a lane
- Automatically handle code signing on iOS, even for XCode 8

## Getting Started

This project is a [fastlane](https://github.com/fastlane/fastlane) plugin. To get started with `fastlane-plugin-cordova`, add it to your project by running:

```bash
fastlane add_plugin cordova
```

Then you can integrate it into your Fastlane setup:

```ruby
platform :ios do
  desc "Deploy ios app on the appstore"

  lane :deploy do
    match(type: "appstore")
    cordova(platform: 'ios')
    appstore(ipa: ENV['CORDOVA_IOS_RELEASE_BUILD_PATH'])
  end
end

platform :android do
  desc "Deploy android app on play store"

  lane :deploy do
    cordova(
      platform: 'android',
      keystore_path: './prod.keystore',
      keystore_alias: 'prod',
      keystore_password: 'password'
    )
    supply(apk: ENV['CORDOVA_ANDROID_RELEASE_BUILD_PATH'])
  end
end
```

with an `Appfile` such as

```ruby
app_identifier "com.awesome.app"
apple_id "apple@id.com"
team_id "28323HT"
```

If using **Crosswalk**, replace `supply(apk: ENV['CORDOVA_ANDROID_RELEASE_BUILD_PATH'])` by:
```
supply(
  apk_paths: [
   'platforms/android/build/outputs/apk/android-armv7-release.apk',
   'platforms/android/build/outputs/apk/android-x86-release.apk'
  ],
)
```

## Plugin API

To check what's available in the plugin, install it in a project and run at the root of the project:
```
fastlane actions cordova
```

Which will produce:

| Key | Description | Env Var | Default |
|-----|-------------|---------|---------|
| **platform**             | Platform to build on. <br>Should be either android or ios | CORDOVA_PLATFORM            |        |
| **release**              | Build for release if true,<br>or for debug if false       | CORDOVA_RELEASE             | *true* |
| **device**               | Build for device                                          | CORDOVA_DEVICE              | *true* |
| **type**                 | This will determine what type of build is generated by Xcode. <br>Valid options are development, enterprise, adhoc, and appstore| CORDOVA_IOS_PACKAGE_TYPE    | appstore |
| **team_id**              | The development team (Team ID) to use for code signing  | CORDOVA_IOS_TEAM_ID               | *28323HT* |
| **provisioning_profile** | GUID of the provisioning profile to be used for signing | CORDOVA_IOS_PROVISIONING_PROFILE  |           |
| **keystore_path**        | Path to the Keystore for Android                        | CORDOVA_ANDROID_KEYSTORE_PATH     |           |
| **keystore_password**    | Android Keystore password                               | CORDOVA_ANDROID_KEYSTORE_PASSWORD |           |
| **key_password**         | Android Key password (default is keystore password)     | CORDOVA_ANDROID_KEY_PASSWORD      |           |
| **keystore_alias**       | Android Keystore alias                                  | CORDOVA_ANDROID_KEYSTORE_ALIAS    |           |
| **min_sdk_version**      | Overrides the value of minSdkVersion                    | CORDOVA_ANDROID_MIN_SDK_VERSION   |           |
| **build_number**         | Build Number for iOS and Android                        | CORDOVA_BUILD_NUMBER              |           |
| **browserify**           | Specifies whether to browserify build or not            | CORDOVA_BROWSERIFY                |  *false*  |
| **cordova_prepare**      | Specifies whether to run `cordova prepare` before building  | CORDOVA_PREPARE               |  *true*   |
| **cordova_no_fetch**      | Specifies whether to run `cordova platform add` with `--nofetch` parameter  | CORDOVA_NO_FETCH               |  *false*   |

## Run tests for this plugin

To run both the tests, and code style validation, run

```
rake
```

To automatically fix many of the styling issues, use
```
rubocop -a
```

## Issues and Feedback

For any other issues and feedback about this plugin, please submit it to this repository.

## Troubleshooting

If you have trouble using plugins, check out the [Plugins Troubleshooting](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/PluginsTroubleshooting.md) doc in the main `fastlane` repo.

## Using `fastlane` Plugins

For more information about how the `fastlane` plugin system works, check out the [Plugins documentation](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Plugins.md).

## About `fastlane`

`fastlane` is the easiest way to automate beta deployments and releases for your iOS and Android apps. To learn more, check out [fastlane.tools](https://fastlane.tools).
