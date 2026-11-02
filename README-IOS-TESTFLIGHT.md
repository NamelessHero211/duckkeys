# Duck Keys iOS TestFlight build

This repo contains the iOS Xcode project in `DuckKeys-iOS-Xcode.zip` and a root-level Codemagic workflow in `codemagic.yaml`. The workflow unpacks the project on a hosted macOS runner, signs it, builds an IPA, and uploads the build to App Store Connect.

## One-time Codemagic setup

1. Create/sign in to Codemagic and connect the public GitHub repository `NamelessHero211/duckkeys`.
2. Add an App Store Connect API key integration named `DuckKeysAppStoreConnect` with App Manager access. Keep the `.p8` private key and issuer/key IDs in Codemagic; do not put them in GitHub.
3. In Codemagic, configure iOS code signing for both bundle IDs: `com.namelesshero.duckkeys` and `com.namelesshero.duckkeys.keyboard`. Use App Store distribution and fetch provisioning profiles for both targets.
4. Start workflow `duckkeys-testflight` on the `main` branch. Confirm the build uploads and finishes processing in App Store Connect.
5. Complete TestFlight beta details and add internal testers. For external testers, create a group and submit the build for Beta App Review.

The App Store Connect app record is `6829620182`. The source bundle does not contain Apple signing certificates, provisioning profiles, or API credentials. iOS keyboards cannot use microphone dictation; use Apple's built-in Dictation control instead. Yandex translation requires the user to supply an API key in the app.