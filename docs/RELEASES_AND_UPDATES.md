# Releases & Update Distribution

This document details how updates are packaged, signed, and distributed to **Latch** users via GitHub Releases and the Sparkle framework.

---

## Update Mechanism Overview

Latch delivers automatic, in-app updates using **Sparkle 2**. Updates are distributed exclusively through the official GitHub Releases tab of the repository (`Sensui-moksha/Latch`).

```mermaid
sequenceDiagram
    participant M as Maintainer
    participant G as GitHub Actions
    participant R as GitHub Releases
    participant S as Sparkle (User App)

    M->>G: git tag v1.x && git push origin v1.x
    G->>G: Build Latch.app & sign with Developer ID
    G->>G: Create Latch.dmg & Latch.pkg
    G->>G: Sign update with Sparkle Ed25519 Private Key
    G->>R: Publish Release Assets (Latch.dmg, Latch.pkg, appcast.xml)
    S->>R: Fetch appcast.xml & check for newer version
    S->>S: Download Latch.dmg & verify EdDSA signature
    S->>S: Install update and relaunch seamlessly
```

---

## Maintainer Workflow: Publishing a New Release

### Step 1: Update Version Numbers
Before tagging a release, increment the version numbers in Xcode:
1. `MARKETING_VERSION` (e.g., `1.1`)
2. `CURRENT_PROJECT_VERSION` (e.g., `2`)

### Step 2: Build & Package the Release
Run the local release build and packaging scripts:
```bash
# 1. Compile production distribution
./scripts/build_distribution.sh

# 2. Package into Latch.dmg and Latch.pkg
./scripts/package_release.sh
```

### Step 3: Sign the Update with Sparkle
Generate the EdDSA signature for the new `Latch.dmg`:
```bash
./scripts/update_appcast.sh
```
This produces the `sparkle:edSignature` and file `length` required by `appcast.xml`.

### Step 4: Add New Item to `appcast.xml`
In `appcast.xml`, add the new `<item>` entry with:
- Release title and notes
- `sparkle:version` (build number)
- `sparkle:shortVersionString` (marketing version)
- Download URL pointing to `https://github.com/Sensui-moksha/Latch/releases/download/v<version>/Latch.dmg`
- The generated `sparkle:edSignature` and length

### Step 5: Tag and Push
Commit `appcast.xml` and push the git tag:
```bash
git add appcast.xml
git commit -m "chore: release v1.1 update feed"
git tag v1.1
git push origin main --tags
```

The GitHub Actions workflow `.github/workflows/release.yml` will automatically build the release artifacts and attach `Latch.dmg`, `Latch.pkg`, and `appcast.xml` to the GitHub release.

---

## User Update Experience

1. **Automatic Background Check**: Latch periodically checks `https://raw.githubusercontent.com/Sensui-moksha/Latch/main/appcast.xml`.
2. **Manual Check**: The user can open **Settings** → **About** and click **Check for Updates**.
3. **Prompt & Changelog**: When an update is detected, Sparkle displays release notes and an **Install and Relaunch** button.
4. **Signature Verification**: Sparkle checks the cryptographic signature using the public key embedded in `Latch.app/Contents/Info.plist`:
   ```xml
   <key>SUPublicEDKey</key>
   <string>9fbFN/c7AYvtyUETXcDTEtsFWLZ5o1EGVkqswULZw/I=</string>
   ```
5. **Seamless Upgrade**: If valid, the update is mounted, extracted, swapped in `/Applications/Latch.app`, and reopened without user friction.
