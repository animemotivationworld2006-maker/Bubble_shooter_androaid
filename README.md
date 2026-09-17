# Bubble Shooter - Android packaging (Capacitor)

This folder is a complete Capacitor project wrapping the already-finished
Bubble Shooter Phaser 3 game (`www/index.html`) for Android. The game itself
is untouched - gameplay, levels, UI, SFX, haptics, and the inert ad hooks are
byte-for-byte the same file you already tested, except one line (the Phaser
script tag now points at a local `vendor/` copy instead of a CDN, for offline
reliability inside the packaged app).

Because the environment that prepared this project has **no internet
access and no Android SDK/Gradle installed**, the remaining steps
(`npm install`, `npx cap add android`, icon generation, and the actual APK
build) cannot be executed here. They are fully automated instead in
`.github/workflows/android-build.yml`, which runs on GitHub's free cloud
build servers - **you never need a PC or Android Studio.**

## What to do, entirely from your Android phone

1. **Create a GitHub account** if you don't have one (github.com, works fine
   in Chrome on Android).
2. **Create a new repository** (e.g. `bubble-shooter-android`), public or
   private, no need to add a README when creating it (this project already
   has one).
3. **Upload this project's files** into that repo. On mobile Chrome:
   open your new repo → tap "Add file" → "Upload files" → select/upload
   every file and folder from this project (keep the folder structure:
   `www/`, `resources/`, `.github/workflows/`, `capacitor.config.json`,
   `package.json`, `.gitignore`). GitHub's mobile upload page supports
   selecting multiple files and preserves folder paths if you drag a zipped
   folder in through "choose your files," or you can upload folder-by-folder.
4. **Commit the files** (the upload page has a "Commit changes" button).
   Committing to `main` automatically triggers the build workflow.
5. **Watch the build**: tap the "Actions" tab at the top of your repo. You'll
   see a "Build Android APK" run in progress (takes a few minutes).
6. **Download the APK**: once it finishes with a green checkmark, open the
   run, scroll to "Artifacts," and tap `bubble-shooter-debug-apk` to download
   it as a zip. Unzip it (Android's Files app can do this) to get
   `app-debug.apk`.
7. **Install it**: tap the APK file. Android will ask permission to install
   from this source the first time (Settings → allow your browser/Files app
   to install unknown apps) - this is normal for any APK not from the Play
   Store. Confirm, and Bubble Shooter installs like any other app.

If the workflow doesn't start automatically, go to the **Actions** tab →
"Build Android APK" → "Run workflow" to trigger it manually.

## What's already done vs. what the workflow does

| Step | Status |
|---|---|
| Game copied into `www/index.html`, verified byte-identical except vendoring the Phaser script tag | ✅ Done here |
| `capacitor.config.json` (appId `com.bubbleshooter.game`, appName "Bubble Shooter", webDir `www`) | ✅ Done here |
| `package.json` with pinned Capacitor dependencies | ✅ Done here |
| App icon source (`resources/icon.png`, 1024x1024, generated procedurally to match the game's premium bubble look) | ✅ Done here |
| Android/WebView compatibility review (localStorage, touch-action, Scale.FIT, no ES modules, no fetch/XHR/workers) | ✅ Reviewed, no issues found |
| `npm install` (fetch real Capacitor packages) | ⏳ Runs in CI (needs internet - sandbox here has none) |
| Vendoring `phaser.min.js` locally into the app bundle | ⏳ Runs in CI |
| `npx cap add android` (generates the native Android project) | ⏳ Runs in CI |
| Lock portrait orientation in `AndroidManifest.xml` | ⏳ Patched automatically in CI (verified against a real Capacitor manifest sample first) |
| Add `VIBRATE` permission so haptics work once packaged | ⏳ Patched automatically in CI |
| Generate all Android icon densities from `resources/icon.png` | ⏳ Runs in CI |
| Build the installable debug APK | ⏳ Runs in CI |

## Notes

- The build produces a **debug APK**, which installs and runs exactly like
  a release build - it's just not signed for the Play Store. That's the
  right choice for "install this on my own phone." If you later want to
  publish to the Play Store, that needs a release keystore and a signed AAB,
  which is a separate, optional step.
- No background music, no ads, and no changes to gameplay were made -
  only packaging.
