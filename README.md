# ThorWoW

ThorWoW is an independent Android launcher for World of Warcraft: Forever,
built for the AYN Thor. It is based on the DS Forever launcher and companion,
with the dual-screen companion integrated into the launcher. It also retains
the WoW Forever / GameNative launcher and runtime foundations. Upstream
copyright and licence notices are preserved.

Unofficial community software. Not affiliated with or endorsed by Blizzard,
the WoW Forever maintainers, DS Forever, or GameNative. World of Warcraft and
Battle.net are their owners' marks. Supply your own authorised game installation
and account. No Blizzard client files, extracted art, or QuestieDB are bundled.

## Current development build

- Android package: `com.sirbedtime.thorwow`
- App version: `0.1.0` (version code `1`)
- Target hardware: AYN Thor; other devices are unverified
- Android: API 29+; arm64-v8a
- ThorWoW is a separate app and can coexist with DS Forever.
- Debug builds use Android's debug signing key and cannot update a separately
  signed release. Release updates must use the same private ThorWoW signing key.

This is an initial identity-migration branch. A successful build and on-device
test have not yet been recorded for this branch.

## Dual-screen companion

The companion is embedded in the launcher and is intended to use the Thor's
lower display. Its source and add-on integration live in `companion/`; the
`:dsforever` module provides the in-launcher presentation integration. The
companion retains its room-based interface, controller workflow, game status,
maps, and related features from the upstream base.

## Game installation

Choose the folder containing `.build.info` and `Data/`, including hidden
files. The game root is selected by the user; game paths are derived from that
root rather than assuming a fixed install directory.

The source includes support for the live `wowf` product and the
`_forever_` client folder, as well as legacy beta product detection. The
launcher does not grant beta entitlement or bypass Blizzard authentication.

## Diagnostics and privacy

The companion's problem-report flow redacts sensitive account and character
information before saving a report. Reports are saved locally for the user to
review and share; this project does not silently upload them. Diagnostic logs
must never include passwords, authentication tokens, or other credentials.

ThorWoW uses its own Android package ID, app updater identity, and report issue
destination. It migrates existing companion settings/reports from the legacy
`Documents/DS Forever` subfolders to `Documents/ThorWoW` when the new
destination does not already exist.

## Build on Windows

Install JDK 17, Python 3.8+, Git for Windows (Bash/curl), CMake 3.16+, Ninja,
Android platform 36, build-tools 36.1.0, and NDK 27.2.12479018. Put the tools on
PATH and set `JAVA_HOME` and `ANDROID_HOME`.

```powershell
./tools/bootstrap.ps1
./gradlew.bat :app:assembleModernDebug
```

The debug APK is for development and testing. Do not use it to update another
launcher installation.

For a signed ThorWoW release, keep the signing keystore outside the repository
and supply these environment variables:

- `THORWOW_KEYSTORE`
- `THORWOW_KEYSTORE_PASS`
- `THORWOW_KEY_ALIAS`
- `THORWOW_KEY_PASS`
- `THORWOW_CERT_SHA256` (64-character SHA-256 certificate fingerprint)

Then run:

```powershell
python tools/build_release.py --out C:/build-output/ThorWoW-0.1.0
```

The release script checks the runtime component hashes, companion add-on
manifest, packaged native libraries, zero-art gate, JNI callbacks, package ID,
version, and signing certificate. It builds but does not install or publish
the APK.

## Upstream and licensing

This project derives from DS Forever, WoW Forever for Android, and GameNative.
The application fork is GPL-3.0; see `LICENSE`, `THIRD_PARTY_NOTICES`,
`RELEASE_NOTICES.md`, and the `licenses/` directory for notices and
component-specific terms.

The inherited release inventory identifies unresolved source, permission, and
redistribution issues for some bundled runtime components. Those issues must be
resolved before public binary redistribution. Retaining upstream notices is
not, by itself, legal clearance.
