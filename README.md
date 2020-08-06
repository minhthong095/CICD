# Bitrise CICD

Installation for all the scripts

- Create app in Bitrise.
- Add repo's git ssh to Bitrise (manual section).
- Add Bitrise's ssh public key to Git account has authorized to clone and writed repo.

I. Firebase Distribute & TestFlight Script
Pick up file:
- bitrise_distribute_testflight/bitrise.yml
- bitrise_distribute_testflight/Fastfile
- postman.json (Trigger build in postman)

Note: 
- This script's only appropiated for Flutter project and work well when deploy to Firebase Distribute and Testflight.
- Default Flutter SDK 1.17.4

Prerequisites:
- Auto signing config in ios project will not work. Only running when ios project is added distribution provisioning with release type.
- Private packages/libraries with specifed url path point to Git services must be SSH.
    Ex: 
    xxx_package:
        git:
          url: git@gitlab.com:minhthong095/xxx_package.git
          ref: master

Steps:
Step 1: Replace bitrise.yml in Bitrise with bitrise_distribute_testflight/bitrise.yml .

Step 2:
[iOS]
 2.1 Prepare P12, bundle id, appstore connect, distribution profiles.
 2.2 Prepare an authorized account to deploy app to appstore connect.
 2.3 Fastlane init in your ios project.
 2.4 Replace Fastfile file in ios folder with bitrise_distribute_testflight/Fastfile .
 2.5 Add distribution provisioning profile into ios project with release type.
 2.6 Add distribution provisioning profile into Bitrise Workflow Editer's 'Code Signing'.
 2.7 Add p12 file pair with distribution provisioning profile into Bitrise Workflow Editer's 'Code Signing'.
 2.8 Make sure 'Show matching Certificates, Devices & Capabilities' light status is green.
[Android]
 2.1 App is created in Firebase project for distribution.
 2.2 key.properties and keystore are added in android folder and build target to 'release' (optional)

Step 3: Add variables in 'Secret Environment Variables' ('Secrets' next to 'Code Signing')
    3.1. $FB_TOKEN (Firebase token of email account is added into Firebase project on Firebase Console)
    3.2 Use account on Step 2.2 (iOS) to fill these variables.
        $APPLE_ID (authorized apple account to deploy app via TestFlight)
        $APPLE_PASSWORD (account's password)
        $FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD (a application specific password in appleid.apple.com/account/manage)
    3.3. $FB_ANDROID_APP_DISTRIBUTE_ID (Android AppID in Firebase)
    3.4. (optional)
        $TELEGRAM_CHAT_ID (group chat id)
        $TELEGRAM_BOT_TOKEN
Step 4: Change variables in 'Environment Variables'
    4.1. $FB_APP_DISTRIBUTE_TESTGROUP (Your FirebaseApp Distribution test group)

Postman trigger:
- Installation:
    Replace YOUR-BITRISE-TOKEN, YOUR-APP-SLUG.
    Ex: https://app.bitrise.io/app/2dcxxxxxbd4f5#/builds (YOUR-APP-SLUG is 2dcxxxxxbd4f5)
    Replace YOUR-BUILD-SLUG.
    Ex: https://app.bitrise.io/build/374a3xxxxxx7fcb89#?tab=log (BUILD-SLUG is 374a3xxxxxx7fcb89)
        Build-slug also find on result of trigger build.
- Option: There are 4 variables in 'build' request's body data. The name of these variables is easily understand and remember to uncomment it for using.
    ANDROID_CUSTOM_VERSION_NAME_ENDPOINT
    ANDROID_CUSTOM_BUILD_NUMBER_ENDPOINT
    IOS_CUSTOM_VERSION_NAME_ENDPOINT
    IOS_CUSTOM_BUILD_NUMBER_ENDPOINT
Build number will automatically increment when request doesn't define it.