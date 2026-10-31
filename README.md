# Duck Keys

An Android keyboard with seven squeaks, including a high-pitched rubber dog toy and **Video duck squeak**, plus a random sound option.

[Download Duck Keys 1.4](https://github.com/NamelessHero211/duckkeys/raw/refs/heads/main/DuckKeys-1.4.apk)

Requires Android 8.0 or later. Install the APK, open Duck Keys, then enable and select its keyboard using the two setup buttons.

Version 1.4 improves rapid typing. Keys submit text on touch-down, sound playback runs on a separate worker, and Shift updates existing keys without rebuilding the keyboard. Delete uses Android's selection updates rather than synchronously asking the editor for selected text on every press. Sound settings are cached and at most four sounds overlap.

The audio worker keeps only the latest pending sound request, discards requests over 50 milliseconds old, and limits sound starts to 125 per second. Extremely rapid or overlapping taps can merge squeaks. Text input is never throttled by this audio limit. This avoids delayed bursts of old squeaks after heavy tapping.

The sound scheduler passed host-JVM tests for a million-request burst, concurrent producers while playback is blocked, stale-request expiry, bounded sound starts, and cancellation. These tests verify scheduling logic; phone and editor performance still require device testing.

Version 1.3 adds **Video duck squeak**, the sound from the supplied [Rubber Duck - Sound Effect for editing](https://www.youtube.com/watch?v=v1hzgoEYZQY) clip by Sound Library. The audio was converted to mono PCM and trimmed to its 0.43-second squeak for responsive typing. Its pitch and timing are preserved. The other six sounds are synthesized.

## Updates

Version 1.2 adds automatic update checks and downloads. Open it once after installation to schedule periodic background checks. Checks also run when opening the app if the previous attempt was more than six hours ago. Android controls the timing of background work.

Enable update notifications in the app to be alerted when an update has downloaded. Open Duck Keys and tap **Install downloaded update**, then approve Android's installation prompt. On the first update, Android may ask you to allow Duck Keys to install its updates.

Automatic downloads can be turned off. **Check for updates now** remains available for manual checks inside the app.

The update channel is [update.json](update.json). Downloads must use HTTPS, match the published SHA-256 checksum, have the correct package name and a higher version code, and use the same signing certificate as the installed app. The signing key is not stored in this repository.

## Privacy

No ads, analytics, or typing history. Internet access is used only for update metadata and APK downloads from GitHub. Typed text is never sent to the update server. GitHub receives normal update download requests.

## Publishing a future version

1. Build and sign with the original Duck Keys signing key. Increase the manifest version code.
2. Upload the newly named APK to this repository.
3. Update `update.json` with its version code, version name, HTTPS raw download URL, and lowercase SHA-256 checksum.
4. Publish the manifest only after its APK is available. Existing installations will find it on their next successful check.

Build, package alignment, and APK signatures have been verified. Physical-device and emulator testing have not been performed in the creation workspace.
