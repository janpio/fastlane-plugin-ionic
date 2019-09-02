# _fastlane_ Plugin for Ionic CLI

[![fastlane Plugin Badge](https://rawcdn.githack.com/fastlane/fastlane/master/fastlane/assets/plugin-badge.svg)](https://rubygems.org/gems/fastlane-plugin-ionic) [![License](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/ionic-zone/fastlane-plugin-ionic/blob/master/LICENSE)
[![Gem](https://img.shields.io/gem/v/fastlane-plugin-ionic.svg?style=flat)](http://rubygems.org/gems/fastlane-plugin-ionic)

This _fastlane_ plugin helps you build your **Ionic Cordova** project via the [`ionic` CLI](https://ionicframework.com/docs/cli/).

It is based on [fastlane-plugin-cordova](https://github.com/bamlab/fastlane-plugin-cordova) (where it borrows a lot of its code. Thanks!).

## Getting Started

This project is a [fastlane](https://github.com/fastlane/fastlane) plugin. To get started with `fastlane-plugin-ionic`, add it to your project by running:

```bash
fastlane add_plugin ionic
```

## Actions

### `ionic`

Runs `ionic cordova build` (technically: `ionic cordova prepare` first, then `ionic cordova compile` [which is the same as what `build` does internally]) to build your Ionic project.

```ruby
ionic(
  platform: 'ios', # Build your iOS Ionic project
)
ionic(
  platform: 'android', # Build your Android Ionic project
  release: false # Build a "Debug" app
)
```


## Examples

Lanes using these actions could look like this:

```ruby
platform :ios do
  desc "Deploy ios app on the appstore"

  lane :deploy do
    match(type: "appstore")
    ionic(platform: 'ios')
    deliver(ipa: ENV['CORDOVA_IOS_RELEASE_BUILD_PATH'])
  end
end

platform :android do
  desc "Deploy android app on play store"

  lane :deploy do
    ionic(
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

---

The `ENV['CORDOVA_ANDROID_RELEASE_BUILD_PATH']` is only valid for `cordova-android` 7.x and newer (which you should be using anyway!).

If you're using **Crosswalk** (which oyu should not really be doing anymore), replace `supply(apk: ENV['CORDOVA_ANDROID_RELEASE_BUILD_PATH'])` (and equivalents) by:

```ruby
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
fastlane actions ionic
```

Which will produce:

| Key | Description | Env Var | Default |
|-----|-------------|---------|---------|
| **platform**             | Platform to build on. <br>Should be either android or ios | CORDOVA_PLATFORM            |        |
| **release**              | Build for release if true,<br>or for debug if false       | CORDOVA_RELEASE             | *true* |
| **device**               | Build for device                                          | CORDOVA_DEVICE              | *true* |
| **prod**                 | Build for production                                      | IONIC_PROD                  | *false* |
| **type**                 | This will determine what type of build is generated by Xcode. <br>Valid options are development, enterprise, adhoc, and appstore| CORDOVA_IOS_PACKAGE_TYPE    | appstore |
| **team_id**              | The development team (Team ID) to use for code signing  | CORDOVA_IOS_TEAM_ID               | *28323HT* |
| **provisioning_profile** | GUID of the provisioning profile to be used for signing | CORDOVA_IOS_PROVISIONING_PROFILE  |           |
| **keystore_path**        | Path to the Keystore for Android                        | CORDOVA_ANDROID_KEYSTORE_PATH     |           |
| **keystore_password**    | Android Keystore password                               | CORDOVA_ANDROID_KEYSTORE_PASSWORD |           |
| **key_password**         | Android Key password (default is keystore password)     | CORDOVA_ANDROID_KEY_PASSWORD      |           |
| **keystore_alias**       | Android Keystore alias                                  | CORDOVA_ANDROID_KEYSTORE_ALIAS    |           |
| **build_number**         | Sets the build number for iOS and  version code for Android   | CORDOVA_BUILD_NUMBER              |           |
| **browserify**           | Specifies whether to browserify build or not            | CORDOVA_BROWSERIFY                |  *false*  |
| **cordova_prepare**      | Specifies whether to run `ionic cordova prepare` before building  | CORDOVA_PREPARE               |  *true*   |
| **min_sdk_version**      | Overrides the value of minSdkVersion set in `AndroidManifest.xml` | CORDOVA_ANDROID_MIN_SDK_VERSION   | ''       |
| **cordova_no_fetch**      | Specifies whether to run `ionic cordova platform add` with `--nofetch` parameter  | CORDOVA_NO_FETCH               |  *false*   |
| **build_flag**           | An array of Xcode buildFlag. Will be appended on compile command.  | CORDOVA_IOS_BUILD_FLAG               | [] |
| **cordova_build_config_file**      | Call `ionic cordova compile` with `--buildConfig=<ConfigFile>` to specify build config file path  | CORDOVA_BUILD_CONFIG_FILE               |     |
| **use_local_ionic**      | Use locally installed Ionic instead of globally installed one. Useful for shared CI environments. | USE_LOCAL_IONIC                   | *false*    |


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
