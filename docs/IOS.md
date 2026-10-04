# Tatakai on iOS

Tatakai is packaged for iOS with Capacitor. The iOS shell lives in `ios/App` and loads the production web build from `ios/App/App/public`.

## Requirements

- macOS with Xcode installed
- An Apple Developer account for device installation and App Store distribution
- Node.js and npm
- CocoaPods only if Xcode requests it for a plugin dependency

## Local development

```bash
npm install
npm run mobile:ios:sync
npm run mobile:ios:open
```

`mobile:ios:sync` builds the Vite application and copies it into the native iOS project. `mobile:ios:open` then opens `ios/App/App.xcodeproj` in Xcode.

In Xcode:

1. Select the **App** target.
2. Set the **Team** under **Signing & Capabilities**.
3. Keep the bundle identifier as `app.tatakai.me`, or replace it with an identifier owned by your Apple Developer account.
4. Select an iPhone simulator or a connected iPhone.
5. Press **Run**.

## Release archive

1. Run `npm run mobile:ios:sync`.
2. Open the project with `npm run mobile:ios:open`.
3. In Xcode choose **Any iOS Device (arm64)**.
4. Use **Product → Archive**, then distribute through Organizer/TestFlight/App Store Connect.

## GitHub Actions

The repository includes `.github/workflows/build.yml`. It runs on pushes to `main`/`master`, pull requests, and manual dispatch:

1. Installs dependencies.
2. Builds the Vite web application.
3. Syncs the committed Capacitor iOS project.
4. Builds an unsigned iOS archive on `macos-15`.
5. Uploads an unsigned IPA as a GitHub Actions artifact for 7 days.

The default artifact is **unsigned**. It is useful for validating the native build, but it cannot be installed on a real iPhone or uploaded to App Store Connect. Device/TestFlight distribution requires adding Apple signing certificates, provisioning profiles, and App Store Connect credentials as GitHub repository secrets, then extending the workflow with a signing/export step.

## Notes

- The Linux build environment can generate and sync the iOS project, but it cannot run Xcode or produce a signed `.ipa`; GitHub Actions provides the macOS runner for repeatable unsigned CI builds.
- The app uses the existing Capacitor configuration and plugins, including filesystem, haptics, keyboard, local notifications, splash screen, status bar, and screen orientation support.
- Backend secrets should remain in the deployment environment. Do not commit `.env` files or private API keys into the iOS project.
- Some streaming providers and embedded players may have iOS/WebKit restrictions. Test playback, downloads, subtitles, notifications, and authentication on a real iPhone before release.
