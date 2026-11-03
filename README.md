# Duck Keys

An Android keyboard with eight typing sounds, including duck squeaks, a high-pitched dog toy, **Video duck squeak**, **Uwu (video)**, and random selection.

[Download Duck Keys 3.5](https://github.com/NamelessHero211/duckkeys/raw/refs/heads/main/DuckKeys-3.5.apk)

Requires Android 8.0 or later. Install, open Duck Keys, then enable and select its keyboard using the setup buttons. Existing users can use **Check for updates now**, then **Install downloaded update**.

## Version 3.5: responsive autocorrect with lower typing load

The keyboard handles common merged words such as “andwish” → “and wish” and “ibcould” → “i could” immediately. Fuzzy personal-word ranking now stays on the background suggestion worker instead of running during the space-key press; the key path uses bounded correction and learned explicit preferences. Suggestions wait for a short idle pause during fast typing so ranking work does not compete with each key press.

## Version 2.7: independent squeak volume

Typing sounds now use Android's sonification/system-sounds audio stream, separate from media playback, so changing music or video volume does not directly change the squeak level. Adjust the Duck Keys percentage slider to set its own sound level. Android still applies system master volume and mute controls, so those can silence any app.

## Version 2.6: tap microphone and silence cutoff

Tap and release the top microphone to start dictation; no hold is needed. Tap again to stop manually. The microphone now uses Android's normal button-click path, so releasing your finger does not stop speech recognition.

Duck Keys requests stop after 10 seconds without a speech-start event or new recognized words. Background volume readings and repeated identical partial transcripts do not extend the timer. The phone speech service is also asked for a 10-second silence window, but may end recognition sooner. This uses the provider's speech detection; it cannot identify whose voice is speaking.

The microphone indicator turns off when listening ends, while final text is processed. If the provider does not finish within eight more seconds, the session is cancelled and any visible draft is kept. A service that never becomes ready is cancelled after 10 seconds. Existing 60-second session cap and single provider fallback remain.

Validation: eleven targeted Android framework tests passed, including a real tap/release through the keyboard view, live partial/final text, 10-second cutoff, new-word timer reset, duplicate/noise rejection, cancellation and provider fallback. APK signing and alignment verified. Physical-phone microphone behavior still needs feedback.

## Version 2.5: Android-style live dictation and the video uwu sound

**Tap to dictate:** tap the top microphone once, wait for Ready, and speak. Partial words appear directly in your message draft. Tap again to stop, or let the speech service finish at a pause. The final transcript replaces the partial draft without duplicating it and adds a trailing space. Duck Keys never sends the message. This follows the tap-and-dictate flow in the supplied [Android example by Rebecca S](https://www.youtube.com/watch?v=GIVF4_dHw10); it is an independent keyboard using Android's speech service, not Motorola's proprietary keyboard.

**Review mode is optional:** turn on **Review dictated text before inserting** to keep the previous Insert/Discard workflow. It is off by default for the native-style flow. In live mode, stopping/hiding/changing fields cancels recognition and keeps any already-visible partial draft; it does not delete other text. Typing or deleting manually stops dictation so later partials cannot overwrite your edits. Provider silence detection may end an utterance sooner; a session requests stop after 60 seconds and times out after 75 seconds. There is no always-listening or automatic restart loop.

**Personal vocabulary:** with personal learning and **Use learned words for dictation** enabled, up to 32 familiar custom words can be supplied as on-device vocabulary hints on Android 13+, where the recognizer supports them. The word list is never attached to the system/online fallback request. Locally, close transcript alternatives can prefer repeatedly used custom words or explicit correction preferences; existing valid dictionary words are protected from fuzzy replacement. Teach a name by typing it several times. Turning learning off or clearing the personal dictionary removes this influence. This adapts vocabulary choices; Duck Keys cannot retrain the provider's acoustic model or verify a speaker's identity.

Supported Android 13+ providers are asked for quality formatting/punctuation. Providers may ignore optional extras or omit partial results; final dictation still works. On-device preference, the single system-service fallback, specific error codes and phone-settings shortcuts from 2.4 remain. Online recognition may process audio through the configured provider after microphone setup consent. Nearby speakers may still be transcribed; optional review mode helps you check the result. Duck Keys saves no speech recordings or transcripts, and does not learn automatically from uncorrected transcripts.

**Uwu (video)** is the actual voice from the supplied [Uwu sound effect](https://www.youtube.com/watch?v=4-LAJosnOKY) by Jacobwalton10. Leading/trailing silence was removed, leaving a 0.63-second mono PCM clip, with original pitch and timing and modest gain/edge fades. Select it in the app or top sound picker. Surprise me can include it. All previous clips remain byte-for-byte unchanged. As with other sounds, extremely fast typing can merge or shorten overlapping playback; sound requests cannot create a typing backlog.

Validation: host learning/vocabulary-choice and live-composition tests; ten Android framework tests for partial/final replacement in the actual IME, optional review, provider fallback/cancellation/readiness, on-device-only vocabulary extras, formatting extras, existing keyboard layout/error display, original duck playback/recovery and uwu sample selection. Physical-phone acoustic accuracy, provider support for hints/punctuation, audible output and latency still need feedback. The example transcript and audio preparation were inspected locally; neither is bundled as a full video.

## Version 2.4: microphone recovery and useful errors

If the on-device speech service reports an unavailable language/model, a provider failure, a busy recognizer or a client error, Duck Keys tries the configured phone speech service once. The fallback chooses its own supported language and online/offline route; it is no longer forced into offline mode or the phone locale. Existing microphone setup consent and review-before-insert still apply. There is no automatic retry loop, and microphone permission/audio failures are not retried with another provider.

Releasing the mic during startup no longer sends stopListening before the service is ready. The stop request is queued until readiness, with a brief 250ms minimum ready interval. Hold the microphone, wait for **Ready**, then speak and release. A 25-second overall deadline still cancels stalled sessions. Old callbacks from a failed recognizer cannot overwrite a new attempt.

Speech errors now stay above the keys with the provider type, numeric error code and specific guidance for network, microphone/privacy-switch access, missing language models, busy services or permission failures. The app adds **Phone speech settings** and **Duck Keys app permissions** shortcuts. If speech still fails, report the complete message and code; the original generic toast did not identify which provider error occurred.

Six Android framework tests passed for one-time local-to-system fallback, unconstrained fallback intent, stale-result suppression, early release/readiness, non-retried permission failures, existing results/cancellation and IME review insertion. Physical-device recognition is not available in the workspace, so this update addresses identified failure paths without claiming a confirmed diagnosis of the reported phone error.

## Version 2.3: personal autocorrect and keyboard controls

**Smarter, local autocorrect:** the personal dictionary learns word frequency, suggestions you explicitly choose, and corrections you undo. Repeated words gain priority in completions; familiar custom words can be corrected from nearby misspellings. Choosing the same one-edit correction twice establishes a preference. Undoing a correction protects that spelling until you explicitly teach another correction or clear the dictionary. Automatic replacements do not create replacement rules on their own. Corrections apply at a word boundary, including Enter/Send. This is English spelling assistance, not a grammar model.

**Learn my words and corrections** is on by default. Disable it or use **Clear learned words and corrections** in the app. Personalization is disabled in password, email, URL and numeric fields, translation mode, fields requesting no suggestions, and editors setting Android's no-personalized-learning flag. Only bounded word counts and correction/undo preferences are retained privately on the phone: up to 512 words, 128 correction rules and 128 protected spellings. No full messages are stored or sent for learning. Dictation transcripts do not train the personal dictionary.

Base correction results use a fixed 512-slot cache, and common short-prefix completions are indexed at dictionary load. Personal typo candidates are indexed too. Dictionary loading, suggestion ranking and delayed/coalesced atomic saves run outside the typing path. Word-boundary correction uses the cache and bounded indexed lookups. Learning uses bounded in-memory updates; it never waits for a file write to enter a key. Previous multi-finger input and bounded audio scheduling remain.

**Volume at top left:** tap the volume icon to open a 0–100% squeak slider and a Typing squeaks switch. This adjusts Duck Keys independently of other apps; the phone's media volume remains the overall output limit. Release the slider to preview the selected squeak.

**Font pinch:** place two fingers over the letter keys, then pinch inward for smaller letter/number/symbol text or spread outward for larger text. A deliberate span change starts resizing; normal overlapping presses stay immediate. Initial provisional letters are removed when a pinch is recognized. The range is 75–140%, with text fitted to its key. The chosen size is remembered; **Reset key font size** in the app restores the default. The gesture is available on the normal English letter layout, not password/special fields or translation mode. It changes text size, not key size.

**Bottom gap:** the keys are raised with clear padding above the navigation area (28 dp portrait, 12 dp landscape). The microphone is at the **top**, alongside the toolbar, as requested.

**Microphone dictation:** first use opens **Set up microphone dictation** in the app. Read the speech-provider notice and enable microphone permission. Return to your text field, hold the top microphone, speak, and release. The recognized text appears for review; **Insert** puts it into the current draft, and **Discard** removes it. It never sends the message. Accessibility click can start/stop a session without holding. Sessions stop after 15 seconds and time out after 25 seconds; hiding the keyboard or changing fields cancels pending speech. Duck squeaks stop during recording.

Dictation **cannot verify that speech is only yours or exclude all nearby voices**. No voiceprint or speaker-verification model is included. Hold-to-talk and review avoid automatic insertion of another speaker's transcript. Android on-device recognition is used when available; otherwise the phone's configured speech provider may process audio online. On-device recognition is preferred; the system fallback chooses its configured language and available online/offline route. Duck Keys does not save speech recordings or transcripts. Speech availability, language models, connection and accuracy depend on the phone/provider. Dictation stays off in password/special fields, translation mode and no-personalization fields.

Validation passed for personal-word learning, explicit corrections, undo preferences, round-trip/clear/size bounds and word-boundary correction; 90,000 typed characters and 40 synthetic swipe paths; private-field policy and background save/clear guards; overlapping touch, haptics and pinch classification; actual IME-view bottom spacing, microphone placement, pinch rollback/persistence and review-before-insert; simulated speech result/cancellation behavior. These are host/Robolectric checks, not physical-phone latency, acoustic recognition, speaker isolation or tactile tests. The Material volume popup uses native Android widgets; its animated SeekBar could not be rendered in this workspace's legacy framework test setup.

## Version 2.2: optional key vibration

**Key vibration** is on by default and can be disabled under **Typing and editing** in the app. Each new finger press requests Android's keyboard-tap haptic, including overlapping fingers. There is no queued vibration playback or extra release pulse. It follows system touch-feedback settings and depends on your phone's vibration motor. This uses Android's [recommended View haptic feedback](https://developer.android.com/develop/ui/views/haptics/haptic-feedback), without overriding system preferences or adding a vibration permission. The haptic on/off dispatch check and all three overlapping-touch tests pass. Physical vibration feel still needs phone feedback.

## Version 2.1: fast multi-finger typing and sound recovery

The mute, cursor controller, keyboard switch, sound picker and settings controls now sit in a toolbar above the suggestion strip and keys, clearing the bottom edge.

Character keys now accept each new finger immediately, including two fingers landing on the same key. Keys across rows remain independently pressed and release in either order. Touch input calls its action directly; accessibility click actions remain available separately. A static pressed highlight replaces overlapping ripple animations. Suggestion updates wait for a 45 ms typing pause, and ordinary keys skip unnecessary deferred-action allocations. Text is not rate-limited by the sound scheduler.

Squeaks now use **media volume**, plus Duck Keys' own volume slider. The selected sample is loaded first using explicit resource IDs. Failed or stuck sample loads can retry, opening the keyboard resumes paused audio, and a failed native playback can trigger a bounded background rebuild. The original Video duck squeak recording is unchanged. Audio still uses four streams and a one-slot latest-request queue; extremely rapid keys can merge squeaks rather than building delayed audio.

Android framework tests cover three simultaneous keys across rows, two fingers on the same key, cancellation, accessibility click availability and 2,000 rapid overlapping presses without lost/duplicate text. Existing key-symbol regression tests also pass. A SoundPool framework test verifies the video sample selection/playback and recovery after a simulated failed load. The original WAV is non-silent and unchanged. These are simulated Android touch-dispatch tests, not a measurement of your phone's speed or audible output.

Version 2.0 reduces per-key editor batching and suggestion-strip redraws. Dictionary loading and suggestions run on a worker. English autocorrect now uses an indexed dictionary for more misspellings, including nearby-key errors and transpositions, and applies at a word boundary. Backspace immediately after a correction restores the original spelling. Autocorrect remains optional in the app and **ellipsis → Autocorrect**. Valid words and ambiguous cases are left alone; this is a conservative spelling engine, without grammar prediction. Version 2.3 adds personal word preferences.

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

Text enters on touch-down. Audio has a separate bounded worker, at most four overlapping sounds, and no backlog of old squeaks during button spamming. Extremely rapid presses may merge squeaks. The **Video duck squeak** comes from the user-supplied [Sound Library clip](https://www.youtube.com/watch?v=v1hzgoEYZQY), trimmed to its 0.43-second squeak; the six original duck/toy sounds are synthesized.

## Verification and limits

Host tests passed for dictionary correction/undo, Unicode source editing, a 90,000-character typing burst, 40 synthetic swipe paths, and 100,000 source updates with one translation request in flight. Tests check stale-result handling, error/retry and cancellation. Android framework tests passed for Yandex request JSON and compact bar geometry/control callbacks. APK alignment and the original signing certificate are verified. These checks do not establish phone latency, real-finger swipe accuracy, or behavior in every editor. Authenticated Yandex translation and physical-device testing still require feedback with a configured key.

## Updates

Version 1.2 adds automatic update checks and downloads. Open it once after installation to schedule periodic background checks. Checks also run when opening the app if the previous attempt was more than six hours ago. Android controls the timing of background work.

Enable update notifications in the app to be alerted when an update has downloaded. Open Duck Keys and tap **Install downloaded update**, then approve Android's installation prompt. On the first update, Android may ask you to allow Duck Keys to install its updates.

Automatic downloads can be turned off. **Check for updates now** remains available for manual checks inside the app.

The update channel is [update.json](update.json). Downloads must use HTTPS, match the published SHA-256 checksum, have the correct package name and a higher version code, and use the same signing certificate as the installed app. The signing key is not stored in this repository.

## Privacy

No ads, analytics, or full-message typing history. Normal typing, suggestions and autocorrect stay local. With personal learning enabled, a bounded set of words, usage counts and correction/undo preferences is saved in private app storage, excluded from Android backup. Turn it off or clear it in the app. Learned custom words may be offered as on-device speech hints, and transcript alternatives are ranked locally; learned word lists are not attached to online speech requests. Speech requires setup and microphone permission; on-device recognition is preferred, otherwise the configured provider may receive audio. Duck Keys does not save recordings or transcripts. GitHub receives update metadata and APK download requests, not your typing. While the inline translator is active, its source text is automatically sent to Yandex Cloud after a short typing pause. Your API key is encrypted using Android Keystore in private app storage. Source text and translation history are not saved by Duck Keys. Yandex handles translation requests under its own policies. The separate website option opens the text in a Yandex HTTPS URL through your browser, whose history and policies also apply.

## Publishing a future version

1. Build and sign with the original Duck Keys signing key. Increase the manifest version code.
2. Upload the newly named APK to this repository.
3. Update `update.json` with its version code, version name, HTTPS raw download URL, and lowercase SHA-256 checksum.
4. Publish the manifest only after its APK is available. Existing installations will find it on their next successful check.

Build, package alignment, and APK signatures have been verified. Physical-device and emulator testing have not been performed in the creation workspace.

## Dictionary attribution

The [English dictionary adaptation](English-dictionary.txt) is filtered from Hermit Dave's [FrequencyWords 2018 English list](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/en/en_50k.txt), derived from OpenSubtitles. The first 16,000 alphabetic entries and contractions were retained with their frequency counts. Source content and this adaptation are licensed [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The attribution and [full dictionary license](Dictionary-LICENSE.txt) are included in the APK and accessible through **Dictionary credits and license** in the app. No endorsement is implied.
