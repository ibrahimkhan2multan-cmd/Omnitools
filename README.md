# OmniTools — All-in-One Utility Suite

A native Android app (Kotlin, Jetpack Compose, Material 3) that bundles everyday utilities. Everything runs on-device: no account, no analytics, no ads, no backend.

> **Status: Part 1.** This is the first stage of a larger roadmap. Only tools that actually work are listed in the app. Nothing has been compiled or run on a device yet by the author of this commit — see [Build status](#build-status).

## What's in Part 1

| Tool | What it does |
|---|---|
| QR & Barcode Scanner | CameraX + on-device ML Kit. Camera permission is requested only when the scanner opens. Shows format and content, never opens anything automatically. Links are analysed (http, IP address, shortener, punycode, `user@` tricks, odd ports) and need confirmation. Local history with per-item delete and clear-all. |
| QR Code Generator | Text, web link, Wi-Fi and contact (vCard) codes using ZXing, fully offline. Save as PNG (system file picker), share, copy. |
| Notepad | Multiple notes, search, autosave (debounced and on leaving the app), copy/share/delete. Stored as private JSON files. |
| Text Tools | Counts, case conversion, whitespace cleanup, duplicate removal, sorting, reverse, line numbers, find & replace, Base64, URL encode/decode, JSON format/minify/validate. |
| Calculator | Standard + scientific (brackets, `%`, `^`, `!`, roots, trig in DEG/RAD, ln/log). |
| Unit Converter | Length, weight, area, volume, speed, data size (decimal vs binary), temperature, number bases (2–36, arbitrary size). |
| Device Information | Model, Android/API, ABIs, screen, RAM/storage snapshot, battery, sensors, cameras, plus clearly separated app-only values. |
| Password Generator | `SecureRandom`, guaranteed character classes, look-alike avoidance, entropy estimate; plus UUID generator. Nothing is stored. |

Dashboard: search (name, description, category, keywords), recents, favorites, suggested tools, category screens, light/dark/system theme, settings, about & privacy.

## Not built yet (planned)

PDF reader and image-to-PDF, image tools, file manager, ZIP manager, code editor and HTML/JS runner, productivity timers, premium/trial framework. Categories with no tools yet are hidden from the dashboard instead of showing dead buttons.

## Build status

Unit tests for the calculator, converters, QR payloads, URL safety, text tools, password generator, recents logic and tool search are included in `app/src/test`.

**These have not been run.** The environment this was written in has no Android SDK and blocks Google/Maven/Gradle hosts, so the project has never been compiled. Expect a few compile errors on first build and report them. The GitHub Actions workflow (`.github/workflows/android.yml`) runs the tests and builds the debug APK on every push, which will be the first real check.

Manual checks still needed on a phone: camera scanning, permission denial flow, save/share of QR images, notes surviving restart, battery/sensor/camera listings.

## Build it

Requirements: Android Studio (Ladybug or newer) or JDK 17 + Android SDK 35.

1. Open the `omnitools` folder in Android Studio and let Gradle sync (it will create the Gradle wrapper).
2. **Run tests:** `gradle testDebugUnitTest` (or `./gradlew …` after the wrapper exists).
3. **Debug APK:** Build ▸ Build APK(s), or `gradle assembleDebug`.
   Output: `app/build/outputs/apk/debug/app-debug.apk`
4. Or push to GitHub and download `omnitools-debug-apk` from the Actions run.

Package id is the placeholder `com.omnitools.app`; change `namespace`/`applicationId` in `app/build.gradle.kts` before any release.

## Privacy

Notes, favorites, recents and scan history stay on the device. The scanner model is bundled with ML Kit and runs locally. The only permission is CAMERA. Copying a password puts it on the system clipboard, which other apps may read on some Android versions.

## Layout

```
app/src/main/java/com/omnitools/app/
  MainActivity.kt, OmniApp.kt        entry point and navigation
  data/                              Prefs (DataStore), ToolRegistry
  ui/                                theme, shared components, home, settings, ToolHost
  tools/{qr,notes,text,calc,device,password}/
  util/                              clipboard/share helpers, formatting
app/src/test/                        JVM unit tests
```

## Roadmap

Part 2 — image tools, PDF reader, image-to-PDF, file info. Part 3 — ZIP manager (Zip Slip-safe), code editor, HTML/JS preview. Part 4 — productivity tools. Part 5 — entitlement framework and clearly-labelled local trial simulation.
