# Security & Privacy Policy

Security and user privacy are foundational pillars of **Latch**. This document details our security model, threat mitigations, and data protection practices.

---

## Security Model & Design Principles

### 1. 100% On-Device Processing
- **Zero Cloud Communication**: Latch does not communicate with external analytics, telemetry, or authentication servers.
- **Local Neural Execution**: Face landmark analysis and ArcFace embedding extraction execute entirely in local memory via Apple's Neural Engine and GPU through Core ML.
- **Ephemeral Frame Lifetime**: Camera frames are processed in-memory buffers and immediately zeroed/released. No video, still frame, or unencrypted image ever touches persistent disk storage.

### 2. Encryption at Rest & Key Management
- Face vector templates are stored in an encrypted format using **AES-256-GCM** (authenticated encryption with associated data).
- The symmetric encryption key is generated locally using `CryptoKit.SymmetricKey(size: .bits256)`.
- The key is saved directly into the macOS Keychain with accessibility attribute `kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly`.
- Only authorized processes running under the app's code signature and team identity can retrieve the encryption key.

### 3. Fail-Closed Application Protection
- Protected applications cannot be accessed if Latch encounters an internal failure, biometric mismatch, or timeout.
- Authorization decisions require concurrent agreement from both the **Face Recognition Engine** (similarity above threshold) and the **Liveness Detector** (absence of spoof cues).
- Short-lived authorization grants prevent replay attacks and ensure unattended Macs automatically re-lock.

---

## Spoofing & Threat Mitigations

| Threat | Risk Level | Mitigation Strategy |
|---|---|---|
| **Printed 2D Photograph** | Low | Defeated by multi-cue planar geometry check and denial cue detection (lack of depth parallax). |
| **Smartphone / Tablet Display Spoof** | Medium | Screen reflection, bezel edge analysis, and pixel grid distortion trigger immediate denial. |
| **Pre-recorded Video** | Medium | Dynamic head orientation requirements and physiological micro-movement analysis. |
| **Binary Tampering / Injection** | High | Hardened Runtime enabled, Library Validation enforced, and strict code signature verification. |
| **Malicious Update Delivery** | High | Sparkle update feed is pinned to the official GitHub repository, and update payloads must match the embedded Ed25519 cryptographic signature. |

---

## Secure Update Pipeline

Latch uses **Sparkle 2** for software updates. The update pipeline enforces strict end-to-end cryptographic verification:

1. The update feed is fetched over HTTPS directly from:
   `https://raw.githubusercontent.com/Sensui-moksha/Latch/main/appcast.xml`
2. The downloaded installer (`Latch.dmg`) is verified against the author's public EdDSA key (`SUPublicEDKey`) before any file operations occur.
3. If an update archive has an invalid signature or does not match the published checksum, Sparkle immediately aborts the installation.

---

## Reporting a Vulnerability

If you discover a security vulnerability or bypass in Latch, please report it privately:

- **Security Contact**: Mokshagna ([@Sensui-moksha](https://github.com/Sensui-moksha))
- **Email**: `mokshyagnay@gmail.com`
- Please provide detailed reproduction steps, the macOS version, and relevant log output.
- Security disclosures will be handled promptly with priority fixes.
