# Duck Keys

An Android keyboard with seven squeaks, including a high-pitched dog-toy squeak, **Video duck squeak**, and random selection.

[Download Duck Keys 2.0](https://github.com/NamelessHero211/duckkeys/raw/refs/heads/main/DuckKeys-2.0.apk)

Requires Android 8.0 or later. Install, open Duck Keys, then enable and select its keyboard using the setup buttons. Existing users can use **Check for updates now**, then **Install downloaded update**.

Version 2.0 reduces per-key editor batching and suggestion-strip redraws. Dictionary loading and suggestions run on a worker. English autocorrect now uses an indexed dictionary for more misspellings, including nearby-key errors and transpositions, and applies at a word boundary. Backspace immediately after a correction restores the original spelling. Autocorrect remains optional in the app and **ellipsis → Autocorrect**. Valid words and ambiguous cases are left alone; this is a conservative spelling engine, without grammar or personalized prediction.

## Inline Yandex translation

1. In the app, tap **Set up Yandex live translation** and enter your personal Yandex Cloud service-account API key on your phone. The key needs translation access and an enabled Cloud account. Provider charges may apply. [Official setup guide](https://yandex.cloud/en/docs/translate/quickstart).
2. In a normal text field, tap **Tr**, in the bottom-left key row. Choose source and target languages or swap them.
3. Type in the compact source bar. After a 180 ms typing pause, Yandex translates it on a background worker and updates the actual app's draft text. **Done** finalizes the translated text and closes the bar; it does not send your message. Review the result before sending.

The supplied [GitHub API definitions](https://github.com/yandex-cloud/cloudapi/tree/master/yandex/cloud/ai/translate/v2) describe the service; they do not supply credentials. No shared API key is bundled. The personal key is encrypted with Android Keystore and can be removed in setup. Do not post it in GitHub or chat.

Only one translation request runs at a time. Intermediate source changes are replaced by the latest text, outdated results are ignored, and errors pause requests until Retry or Setup. Closing the bar or changing fields cancels pending work. The source limit is 2,000 characters. Twenty language choices are offered; the keyboard itself remains English QWERTY with long-press accents. English assistance is disabled for other source languages. Translation is unavailable in password and special fields. Network speed and Yandex service availability affect translation delay.

**Yandex website** remains available from the app and the keyboard's ellipsis menu without an API key. That option requires copying the result back manually.

## Keyboard and sounds

The layout follows the requested Windows Phone photo: rectangular keys, staggered middle row, wide Shift/Backspace, and **&123 · Tr · comma · space · period · Enter**. Emoji is under **ellipsis → Emoji**. Android text rendering and readable fallback labels from 1.9 remain for Shift, Caps Lock, Backspace and Return.

English swipe typing, suggestions, cursor control, Caps Lock, sentence capitalization, double-space period and long-press characters remain. Each typing aid has its own setting. Recognition uses a bundled vocabulary rather than Microsoft's Word Flow engine. The dictionary attribution is below.

Text enters on touch-down. Audio has a separate bounded worker, at most four overlapping sounds, and no backlog of old squeaks during button spamming. Extremely rapid presses may merge squeaks. The **Video duck squeak** comes from the user-supplied [Sound Library clip](https://www.youtube.com/watch?v=v1hzgoEYZQY), trimmed to its 0.43-second squeak; the other six sounds are synthesized.

## Verification and limits

Host tests passed for dictionary correction/undo, Unicode source editing, a 90,000-character typing burst, 40 synthetic swipe paths, and 100,000 source updates with one translation request in flight. Tests check stale-result handling, error/retry and cancellation. Android framework tests passed for Yandex request JSON and compact bar geometry/control callbacks. APK alignment and the original signing certificate are verified. These checks do not establish phone latency, real-finger swipe accuracy, or behavior in every editor. Authenticated Yandex translation and physical-device testing still require feedback with a configured key.

## Updates

Version 1.2 adds automatic update checks and downloads. Open it once after installation to schedule periodic background checks. Checks also run when opening the app if the previous attempt was more than six hours ago. Android controls the timing of background work.

Enable update notifications in the app to be alerted when an update has downloaded. Open Duck Keys and tap **Install downloaded update**, then approve Android's installation prompt. On the first update, Android may ask you to allow Duck Keys to install its updates.

Automatic downloads can be turned off. **Check for updates now** remains available for manual checks inside the app.

The update channel is [update.json](update.json). Downloads must use HTTPS, match the published SHA-256 checksum, have the correct package name and a higher version code, and use the same signing certificate as the installed app. The signing key is not stored in this repository.

## Privacy

No ads, analytics, or saved typing history. Normal typing, suggestions and autocorrect stay local. GitHub receives update metadata and APK download requests, not your typing. While the inline translator is active, its source text is automatically sent to Yandex Cloud after a short typing pause. Your API key is encrypted using Android Keystore in private app storage. Source text and translation history are not saved by Duck Keys. Yandex handles translation requests under its own policies. The separate website option opens the text in a Yandex HTTPS URL through your browser, whose history and policies also apply.

## Publishing a future version

1. Build and sign with the original Duck Keys signing key. Increase the manifest version code.
2. Upload the newly named APK to this repository.
3. Update `update.json` with its version code, version name, HTTPS raw download URL, and lowercase SHA-256 checksum.
4. Publish the manifest only after its APK is available. Existing installations will find it on their next successful check.

Build, package alignment, and APK signatures have been verified. Physical-device and emulator testing have not been performed in the creation workspace.

## Dictionary attribution

The [English dictionary adaptation](English-dictionary.txt) is filtered from Hermit Dave's [FrequencyWords 2018 English list](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/en/en_50k.txt), derived from OpenSubtitles. The first 16,000 alphabetic entries and contractions were retained with their frequency counts. Source content and this adaptation are licensed [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The attribution and [full dictionary license](Dictionary-LICENSE.txt) are included in the APK and accessible through **Dictionary credits and license** in the app. No endorsement is implied.
