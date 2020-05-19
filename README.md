# Bitrise CICD

- Create app in Bitrise.
- Add SSH git repo to Bitrise.
- Add SSH public key to Git account has authority to clone and write repo.

I. [TEST] Firebase Distribute & TestFlight

Note: 
- Only apply scripts to Flutter project and work in flow for testing in firebase app distribute and testflight. Default Flutter SDK 1.12.13+hotfix-9.
- Auto signing not config in ios project will not work. Only running when ios add distribution provisioning.

Pick up file:
- bitrise_distribute_testflight.yml 
- fastfile_distribute_testflight_script

Prerequisites:
- Private packages/libraries with specifed url path point to Git services must be SSH.
    Ex: git@gitlab.com:xxx/xx.git

1. Replace bitrise.yml in Bitrise.

2.
[iOS]
 - P12, bundle id, appstore connect, distribution profiles have already created.
 - Appropriated account to deploy app to appstore connect.
 - Fastlane init in your ios project.
 - Replace Fastfile with fastfile_distribute_testflight_script.
 - Add distribution provisioning profile into ios project with release type.
 - Add appropriated distribution provisioning profile into 'Code Signing'.
 - Add p12 file pair with distribution provisioning profile into 'Code Signing'
 - Make sure 'Show matching Certificates, Devices & Capabilities' light status is green.
[Android]
 - App is created in firebase project for distribution.
 - key.properties and keystore are added in android folder and build target to 'release' (optional)

3. Add variables in 'Secret Environment Variables'
    6.1. $FB_TOKEN (Account's fb token is added into app/project distribute)
    6.2. $APPLE_ID, $APPLE_PASSWORD, $ FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD
    (username, email which are granted to appstore connect to your app)
    6.3. $TELEGRAM_CHAT_ID (group chat id), $TELEGRAM_BOT_TOKEN
    6.4. $FB_ANDROID_APP_DISTRIBUTE_ID (Android AppID in Firebase)
4. Add variables in 'Environment Variables'
    7.1. $FB_APP_DISTRIBUTE_TESTGROUP (required)

