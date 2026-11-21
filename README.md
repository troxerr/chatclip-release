# chatclip-release

Release runner for ChatClip. This repository is public only to get free macOS GitHub Actions minutes. It contains **no app source**.

Running the `release` workflow (Actions → release → Run workflow, with a tag such as `v3.0.0`):

1. checks out the public source repository (`troxerr/chat-clip`),
2. builds, signs with Developer ID and notarizes the app,
3. uploads the DMG and an updated Sparkle `appcast.xml` to private Cloudflare R2 storage.

There is no GitHub Release and no workflow artifact. There are no push or pull request triggers, so forks cannot reach any secret.

## Required configuration

Secrets: `MACOS_CERT_P12`, `MACOS_CERT_PASSWORD`, `APPLE_API_KEY_P8`, `APPLE_API_KEY_ID`, `APPLE_API_ISSUER_ID`, `SPARKLE_PRIVATE_KEY`, `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET`, and `GOOGLE_CLIENT_SECRET`.

Variables: `R2_PUBLIC_BASE_URL`, `SU_PUBLIC_ED_KEY`, `LEMON_PRODUCT_ID`, `CHECKOUT_URL`, and optionally `TRIAL_DAYS`, `DROPBOX_APP_KEY`, `GOOGLE_CLIENT_ID`.

Keep write access to this repository limited to the owner: anyone who can edit the workflow can read its secrets.

## ChatClip rename

Source of truth: [troxerr/chat-clip `docs/RELEASE.md`](https://github.com/troxerr/chat-clip/blob/main/docs/RELEASE.md). The app is `ChatClip.app` (bundle ID `com.chatclip.app`) and the uploaded DMGs are `releases/ChatClip-<version>-<arch>.dmg`. The workflow signs and notarizes with the App Store Connect API key, so the `chatclip-notary` keychain profile (local releases only) is not used in CI. Secret and variable names are unchanged.

External steps still required (not done by this repository): re-issue or confirm the Sparkle EdDSA key and `appcast.xml` for the new bundle ID, update Lemon Squeezy / Polar product names, and do a dry run with a throwaway tag.

## Windows

Windows releases are not built here. They use `windows-release.yml` in `troxerr/chat-clip` (see its `docs/WINDOWS_RELEASE.md`), which does not call for this repository to host them.
