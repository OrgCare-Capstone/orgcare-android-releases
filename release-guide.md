# OrgCare Android Release Guide

Use this guide whenever publishing or editing an Android release in the
[`OrgCare-Capstone/orgcare-android-releases`](https://github.com/OrgCare-Capstone/orgcare-android-releases/releases)
repository.

## Why the release format matters

OrgCare checks GitHub Releases for Android updates. The app validates the release tag,
description, and APK before showing an update. If the required metadata is missing or
formatted differently, the app cannot recognize the release.

The release description must contain these two fields exactly once, each on its own line:

```text
Version: X.Y.Z
Build: N
```

Do not replace them with wording such as `Android version code: N`. The labels must remain
exactly `Version:` and `Build:`.

## Before creating the release

1. Open [`app.json`](../app.json).
2. Under `expo.android`, confirm the values of:
   - `version`, such as `1.2.5`
   - `versionCode`, such as `12`
3. Confirm the APK contains the same package, version, and build number.
4. Confirm the APK is release-signed with the established production certificate.
5. Name the APK using one of the supported formats:
   - `OrgCare.apk`
   - `OrgCare-vX.Y.Z-buildN.apk`

Never publish an APK based only on its filename. Verify its embedded metadata and signing
certificate first.

## GitHub release fields

For version `X.Y.Z` and build `N`, use:

- **Tag:** `vX.Y.Z`
- **Release title:** `OrgCare vX.Y.Z`
- **Pre-release:** Off for a production release
- **Latest:** On for the newest production release
- **Asset:** Exactly one supported APK

The tag version, description version, APK version, and Android version in `app.json` must all
match. The description build, APK version code, and `expo.android.versionCode` must also match.

## Copy-paste release description

Replace every placeholder before publishing:

```text
OrgCare Android vX.Y.Z

What's New

- Describe the first user-visible improvement.
- Describe the second user-visible improvement.
- Describe any important fix.

Android download

Download the attached OrgCare APK below.

Version: X.Y.Z
Build: N
Platform: Android
```

Do not add a second `Version:` or `Build:` line elsewhere in the description.

## Final checklist

- [ ] `app.json` has the intended Android `version` and `versionCode`.
- [ ] The APK package is `com.orgcare.app`.
- [ ] The APK's embedded version matches `Version: X.Y.Z`.
- [ ] The APK's embedded version code matches `Build: N`.
- [ ] The APK is not debuggable.
- [ ] The APK signature matches the established production certificate.
- [ ] The Git tag is exactly `vX.Y.Z`.
- [ ] The release description contains exactly one `Version: X.Y.Z` line.
- [ ] The release description contains exactly one `Build: N` line.
- [ ] Exactly one supported APK is attached.
- [ ] The release is not marked as a pre-release unless intentionally testing.
- [ ] The release is marked as the latest production release when appropriate.
- [ ] After publishing, **Settings > Check for Updates** is tested on an Android device.

## Optional APK verification command

The repository includes `scripts/verify-android-release.mjs`. Run it with the APK and the
SHA-256 fingerprint of the established production certificate:

```sh
ANDROID_HOME=/Users/francis/Library/Android/sdk node scripts/verify-android-release.mjs \
  /path/to/OrgCare-vX.Y.Z-buildN.apk \
  --cert-sha256 YOUR_ESTABLISHED_CERTIFICATE_SHA256
```

Only publish when every verification check reports `PASS`.

## If update checking reports no internet

First confirm that the latest GitHub release description contains the exact `Version:` and
`Build:` lines above. The app rejects malformed release metadata, which can otherwise look like
a connectivity failure.
