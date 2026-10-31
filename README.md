# Duck Keys

An Android keyboard with seven squeaks, including a high-pitched rubber dog toy and **Video duck squeak**, plus a random sound option.

[Download Duck Keys 1.9](https://github.com/NamelessHero211/duckkeys/raw/refs/heads/main/DuckKeys-1.9.apk)

Requires Android 8.0 or later. Install the APK, open Duck Keys, then enable and select its keyboard using the two setup buttons.

Version 1.9 addresses the confirmed blank key symbols in the phone screenshot. Shift, Caps Lock, Backspace, smiley and Return now use Android's normal text renderer. The app checks whether the font supports each symbol and uses a readable Shift/CAPS/Del/:)/Enter label when necessary. Remaining icon drawing compensates for TextView's internal scroll offset, and icon keys disable horizontal text scrolling. The bottom-left smiley slot is now **Tr**, as requested. Open Emoji from the **ellipsis → Emoji** menu, and use **ABC** to return to letters. The separate Tr toolbar button has been removed. The remaining photo-inspired layout, typing behavior, translator and sounds remain.

The Android widget regression test reproduced the old 524,228-pixel horizontal text scroll. The replacement passed checks for nonblank fallback text, zero icon-text scroll, in-button label bounds and native text-draw calls at narrow key sizes, including Caps Lock. Remaining drawable icons passed recorded viewport-coordinate checks with zero, positive and negative content scroll. These are Robolectric Android 9 / legacy-graphics checks, not pixel screenshots or a physical-device test. Native pixel rendering was unavailable because its dependency download was rate-limited.

Version 1.8 attempted a custom canvas fix; the user's phone screenshot showed that it did not resolve the missing symbols. Version 1.9 replaces that path for the affected keys.

**Autocorrect is optional:** turn it on/off near the top of the app, or use the keyboard's **ellipsis → Autocorrect** menu. The same menu offers a separate word-suggestions switch. This is the existing conservative English common-typo correction, with immediate Backspace undo; it does not send text to a spelling service.

**Yandex Translate (no key):** tap **Tr** in the keyboard's bottom-left key row, or **Yandex Translate** in the app. Choose source/target languages, enter up to 2,000 characters, and tap **Open Yandex Translate**. Source-language detection and 20 target languages are offered. The text and language choices open on Yandex's HTTPS website in your browser. Copy the translated result there, return to your app, and paste it. This is a browser handoff, not an embedded translation API or automatic insertion. No API key, cloud account setup, or bundled credentials are used. A **Copy text** fallback is provided if browser text prefill fails. Translation availability depends on your browser, connection, and Yandex.

If ordinary text is selected when you explicitly tap **Tr**, it is locally prefilled into the translation screen for review. Nothing is sent until **Open Yandex Translate** is tapped. Password/special fields are not read for prefill. Only language choices are remembered; Duck Keys does not save the translation text. Browser/Yandex history and data handling follow their own settings and policies. Normal typing stays local.

Version 1.7 matches the requested Windows Phone photo layout: flat rectangular keys, narrow gaps, uppercase letter legends, a staggered middle row, wide Shift and Backspace, and the bottom row **&123 · smiley · comma · space · period · Enter**. Light mode uses pale keys on a blue background; a dark variant follows system night mode. The swipe trail is amber.

The smiley opens a compact 20-emoji panel; **ABC** returns to letters. Sound/mute, the cursor dot, keyboard switching, and the settings ellipsis sit in the slim command row below the keys. Email/URL punctuation and editor-specific Enter actions remain available. Uppercase legends do not force uppercase typing: Shift, Caps Lock, and sentence capitalization still determine the text entered. All typing and sound features from version 1.6 remain.

Version 1.6 adds Windows 10 Mobile-inspired typing behavior while keeping the Android appearance and duck sounds:

- **Swipe to type (English):** slide through letters and lift. Starting another swipe automatically separates the words. The suggestion strip offers alternative matches. Ordinary letter taps still enter text on touch-down.
- **Offline suggestions:** a horizontally scrolling strip offers completions and possible spelling corrections from a bundled 16,000-entry English dictionary. Tap a word to use it and add a space.
- **Conservative autocorrect:** common known typos are corrected at a word boundary. Press Backspace immediately afterward to restore your original spelling. Unknown names are not automatically replaced by fuzzy dictionary matches.
- **Cursor controller:** drag the colored dot left/right or up/down to move through text. Tap it for arrow controls, Select all, Copy, and Paste. Editor support determines how these actions behave.
- **Typing shortcuts:** double-tap Shift for Caps Lock, double-space within half a second to insert a period, capitalize new sentences, and hold letters/punctuation for accents, top-row numbers, and alternate symbols.
- **Settings:** swipe typing, suggestions, common-typo correction, sentence capitalization, and the double-space shortcut have independent switches in the app.

Assistance stays off in password, email, URL, and numeric fields, and in editors requesting no suggestions or their own completions. All recognition and suggestions run locally. There is no learned typing history or network prediction service. This is an independent English implementation inspired by the [Lumia user guide](https://www.bhphotovideo.com/lit_files/251649.pdf), not Microsoft's Word Flow engine. Recognition is limited to the bundled vocabulary; it does not include multilingual models, personalized next-word prediction, or Windows Phone's one-handed positioning.

Dictionary loading, lookup, and swipe matching use a background worker. Suggestion requests replace stale work; ordinary taps do not wait for dictionary lookup. After a swipe lifts, subsequent edits preserve their order while recognition completes, with a bounded 64-event buffer and a 180 ms fallback to the first letter if decoding is unavailable or too slow. Audio retains the bounded worker scheduling introduced in version 1.4.

The new typing rules passed host tests for composition, correction undo, shortcuts, accented characters, Unicode deletion, consecutive words, and a 90,000-character burst. Swipe recognition returned the expected first result for 40 synthetic paths, including slight coordinate jitter. These checks do not establish real finger accuracy or Android latency. Physical-device and emulator tests remain unavailable; please report recognition or editor issues.

Version 1.5 introduced the Android Material-style appearance: staggered QWERTY rows, a pill-shaped Space bar, ripple feedback, vector icons, and accent-colored action keys. System light/dark mode and Android 12+ palette colors remain. The toolbar provides mute, sound selection, and settings. Numeric fields have a four-column keypad, phone fields include `+`, `*`, and `#`, and email/URL fields have convenient punctuation.

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

No ads, analytics, or typing history. Duck Keys uses Internet access for update metadata and APK downloads from GitHub. Ordinary typed text is never sent to the update server. GitHub receives normal update download requests. When you explicitly choose translation, the text you submit is included in a Yandex HTTPS URL opened by your browser; Yandex and the browser handle that text according to their own policies. No background translation is performed.

## Publishing a future version

1. Build and sign with the original Duck Keys signing key. Increase the manifest version code.
2. Upload the newly named APK to this repository.
3. Update `update.json` with its version code, version name, HTTPS raw download URL, and lowercase SHA-256 checksum.
4. Publish the manifest only after its APK is available. Existing installations will find it on their next successful check.

Build, package alignment, and APK signatures have been verified. Physical-device and emulator testing have not been performed in the creation workspace.

## Dictionary attribution

The [English dictionary adaptation](English-dictionary.txt) is filtered from Hermit Dave's [FrequencyWords 2018 English list](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/en/en_50k.txt), derived from OpenSubtitles. The first 16,000 alphabetic entries and contractions were retained with their frequency counts. Source content and this adaptation are licensed [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The attribution and [full dictionary license](Dictionary-LICENSE.txt) are included in the APK and accessible through **Dictionary credits and license** in the app. No endorsement is implied.
