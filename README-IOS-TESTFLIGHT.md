# Duck Keys iOS TestFlight build

The repository contains the iOS Xcode project in `DuckKeys-iOS-Xcode.zip` and a root-level Codemagic workflow in `codemagic.yaml`. Codemagic unpacks the project on a hosted macOS runner, obtains App Store signing files for the app and keyboard extension, builds a signed IPA, and uploads it to App Store Connect.

## Codemagic configuration

The repository is connected to Codemagic as a public Git URL. The workflow uses the `DuckKeysAppStoreConnect` Developer Portal integration, which must contain an App Store Connect API key with App Manager access. Keep the private `.p8` file in Codemagic; never commit it to GitHub.

The workflow increments the iOS build number using App Store Connect, fetches profiles for `com.namelesshero.duckkeys` and `com.namelesshero.duckkeys.keyboard`, installs signing certificates, and uploads the IPA. Start `duckkeys-testflight` on the `main` branch from Codemagic.

## TestFlight distribution

After the build finishes, wait for Apple to process it. Complete TestFlight beta details and add internal testers. To invite external testers, create a group and submit the build for Beta App Review.

The App Store Connect app record is `6829620182`. The source bundle does not contain Apple signing certificates, provisioning profiles, or API credentials. iOS keyboards cannot use microphone dictation; use Apple's built-in Dictation control instead. Yandex translation requires the user to supply an API key in the app.