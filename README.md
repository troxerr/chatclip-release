# chatclip-release

Release runner for ClipSpike. This repository is public only to get free macOS GitHub Actions minutes. It contains **no app source**.

Running the `release` workflow (Actions → release → Run workflow, with a tag such as `v3.0.0`):

1. checks out the public source repository (`troxerr/chat-clip`),
2. builds, signs with Developer ID and notarizes the app,
3. uploads `ClipSpike-<version>-arm64.dmg` and `ClipSpike-<version>-x86_64.dmg`, plus an updated Sparkle `appcast.xml`, to Cloudflare R2.

There is no GitHub Release and no workflow artifact. There are no push or pull request triggers, so forks cannot reach any secret.

## Required configuration

Secrets: `MACOS_CERT_P12`, `MACOS_CERT_PASSWORD`, `APPLE_API_KEY_P8`, `APPLE_API_KEY_ID`, `APPLE_API_ISSUER_ID`, `SPARKLE_PRIVATE_KEY`, `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET`, and `GOOGLE_CLIENT_SECRET`.

Variables: `R2_PUBLIC_BASE_URL`, `SU_PUBLIC_ED_KEY`, `DROPBOX_APP_KEY`, and `GOOGLE_CLIENT_ID`. `TRIAL_DAYS` is optional; when it is empty the workflow uses `trialDays` from `win/build-config.json`.

The Polar checkout, product id, API base, and price label are read from `win/build-config.json` in the source checkout. The workflow does not take those from Actions variables.

Keep write access to this repository limited to the owner: anyone who can edit the workflow can read its secrets.

## ClipSpike

Source of truth: [troxerr/chat-clip `docs/RELEASE.md`](https://github.com/troxerr/chat-clip/blob/main/docs/RELEASE.md). The app is `ClipSpike.app` (bundle ID `com.clipspike.app`). Uploaded files are `releases/ClipSpike-<version>-<arch>.dmg`. The workflow signs and notarizes with the App Store Connect API key. A local `clipspike-notary` keychain profile is not used in CI.

External steps still required (not done by this repository): the Developer ID certificate, the App Store Connect API key, the Sparkle EdDSA key, the R2 bucket, and the Dropbox and Google OAuth clients.

## Windows

Windows releases are not built here. They use `windows-release.yml` in `troxerr/chat-clip` (see its `docs/WINDOWS_RELEASE.md`), which does not call for this repository to host them.
