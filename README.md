# POCT application and firmware releases

This public repository is the distribution channel for IIT Bhilai POCT
artifacts. Source code, signing keys, credentials, patient data, and production
configuration do not belong here.

## JeevDristi Android releases

The private `doc-poct/poct_app_flutter` source repository publishes releases
through GitHub Actions:

- Stable source tag: `vMAJOR.MINOR.PATCH`
- Stable distribution tag: `app-vMAJOR.MINOR.PATCH+VERSION_CODE`
- Beta distribution tag: `app-vMAJOR.MINOR.PATCH-beta.BUILD+VERSION_CODE`
- APK: `JeevDristi-VERSION-release.apk` (or a versioned beta equivalent)
- Integrity: matching `.apk.sha256` asset and GitHub release-asset SHA-256 digest
- Metadata: stable releases include a JSON record with the source repository,
  source commit, source tag, build number, toolchain, platform, architecture,
  and checksum

Release tags and assets are immutable. Do not replace an existing version; bump
the version and publish a new release. The Android app reads this repository's
public Releases API and downloads APKs without a GitHub access token.

Firmware artifacts will use their own namespaced tags and compatibility
metadata when that workflow is defined.
