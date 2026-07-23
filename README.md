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

## POCT box runtime releases

The box runtime source repository publishes immutable `runtime-vMAJOR.MINOR.PATCH`
releases through GitHub Actions. A runtime release contains:

- `poct-runtime-VERSION-aarch64.tar.zst` — signed, self-contained runtime
  archive for Raspberry Pi Zero 2W and Pi 5 ARM64;
- a matching `.sha256` file; and
- an in-archive signed manifest containing hardware, architecture, minimum app
  version, and a digest for every payload file.

The Flutter app provisions short-lived Wi-Fi credentials only to request an
update. The box downloads the immutable asset itself, verifies its checksum and
signature, activates its inactive slot, and removes those credentials on every
terminal path. Release assets must never contain device configuration, OTA
private keys, source credentials, or patient data.

## POCT public firmware images

The box source repository publishes generic DietPi flash-and-connect images from
`firmware-vMAJOR.MINOR.PATCH` source tags. Each immutable `firmware-vVERSION`
release includes a Pi Zero 2W image and a Pi 5 compatibility/backup image, plus
their checksums, build manifests, and package inventories.

Public images contain no shared box identity, product secret, OTA key, or
maintenance SSH key. On first boot each box generates those values locally and
starts its runtime and BLE Device Link services. Multiple nearby phones can
connect, but the box permits only one active test. This temporary development
onboarding mode is unauthenticated and must not be used for clinical deployment
until encrypted multi-phone pairing is implemented. An image is OTA eligible
only when its manifest declares `boot_layout: poct-rpi-ab-v1`: the box verifies
the hardware-matched immutable image, writes the inactive root, then uses a
one-shot trial boot with health-gated commit and automatic rollback. Legacy
single-root boxes remain manual-reflash-only.
