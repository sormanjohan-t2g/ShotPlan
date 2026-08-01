# TeeToGreen — App Store packaging

This folder is a minimal Capacitor project. `www/index.html` is your app,
copied in as-is — nothing about T2G itself needs to change for this to work.

You do NOT need a Mac for any of this. The path below uses GitHub Codespaces
(a full dev environment in your browser, works fine on iPad Safari) plus a
cloud build service that has its own Mac/Xcode in the cloud.

## 1. Get this project onto GitHub
- Create a new (private is fine) GitHub repo, e.g. `t2g-app`.
- Upload this whole folder into it (GitHub's web UI lets you drag-and-drop
  files/folders directly — no terminal needed for this step).

## 2. Open it in GitHub Codespaces (browser-based, works on iPad)
- On the repo page: **Code → Codespaces → Create codespace on main**.
- This opens a full VS Code + Linux terminal in your browser, with Node.js
  already available.

## 3. Run these commands in the Codespaces terminal
```
npm install
npx cap add ios
npx cap sync
```
This generates an `ios/` folder containing a real Xcode project that loads
your `index.html` inside a native app shell.

## 4. Add the location-permission text (required — T2G uses GPS heavily)
Open `ios/App/App/Info.plist` (right in the Codespaces file browser) and add,
just before the closing `</dict>`:
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>TeeToGreen uses your location to track shots and give distances on the course.</string>
```

## 5. Commit and push
```
git add -A
git commit -m "Add iOS project"
git push
```

## 6. Build & submit via a cloud Mac build service (no Mac needed)
Pick one — all support Capacitor/iOS builds without you owning a Mac:
- **Codemagic** (codemagic.io) — has a documented Capacitor iOS workflow,
  free tier available. Connect your GitHub repo, it builds, signs, and can
  submit straight to App Store Connect.
- **Ionic Appflow** (ionic.io/appflow) — made by the same team as Capacitor.
- **Codemagic/Capgo "Build Without Mac"** style services — search for
  current offerings, this space moves fast.

You'll need to give whichever service your Apple Developer account details
(next step) so it can sign the build.

## 7. Apple Developer Program
- Enroll at developer.apple.com/programs (check current fee there — it
  changes over time, was ~$99/year historically).
- In App Store Connect, create a new app listing: name, screenshots,
  description, privacy details (flag that you collect location data and
  explain why), age rating, etc.

## 8. Things specific to T2G to double check once it's running as a wrapped app
- **Storage is separate from Safari.** Anything saved via the PWA/Safari
  version (your courses, club stats) will NOT appear automatically inside
  the wrapped native app — it starts with empty storage. If you want
  continuity, you'd want an export/import flow, or just accept starting
  fresh in the App Store version.
- **Test GPS actually prompts correctly** the first time you open the app —
  this is the main thing that can go wrong in a WebView wrapper.
- **Test Mapbox rendering** performs the same as it does in Safari — WebGL
  inside a WKWebView is normally fine, but worth confirming on a real device
  build before submitting.
