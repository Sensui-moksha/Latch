# Getting Started with Latch

Welcome to **Latch**, the biometric face and Touch ID application lock for macOS.

---

## System Requirements

- **Operating System**: macOS 15.0 Sequoia or later
- **Hardware**: Apple Silicon (M1/M2/M3/M4) or Intel Mac with FaceTime HD or external USB/Continuity camera
- **Authentication**: Touch ID sensor or Apple Watch / Device Owner password

---

## 1. Installation

1. **Download the latest release**:
   Download `Latch.dmg` directly from the [GitHub Releases](https://github.com/Sensui-moksha/Latch/releases/latest).

2. **Mount and Install**:
   Double-click `Latch.dmg`, then drag the **Latch** application icon into your `/Applications` folder.

3. **First Launch**:
   Open **Latch** from `/Applications` or via Spotlight. If macOS warns that it cannot verify or trust the developer on first open:
   - Go to **System Settings** → **Privacy & Security**, scroll down to the **Security** section, and click **Open Anyway**.
   - Alternatively, right-click (or Control-click) `Latch.app` in Finder and select **Open** → **Open**.

---

## 2. Permissions Setup

When Latch starts for the first time, it will guide you through granting the required macOS permissions:

| Permission | Purpose | Privacy Guarantee |
|---|---|---|
| **Camera** | Used on-device by Apple Vision framework to detect face landmarks and generate embeddings. | Video frames are processed in-memory only and discarded immediately. No photos are saved to disk. |
| **Accessibility** | Required to detect frontmost active application windows and coordinate seamless unlocking. | Strictly scoped to application window management. No keystrokes are recorded. |
| **Touch ID** | Authorizes access to the hardware-backed AES-256 master encryption key stored in Keychain. | Handled directly by macOS `LocalAuthentication` and Secure Enclave. |

---

## 3. Face Enrollment

1. Launch Latch and follow the interactive onboarding wizard.
2. Position your face in front of the camera inside the guidance circle.
3. Slowly turn your head in the 9 guided directions (center, left, right, up, down, diagonals).
4. Latch converts the detected facial geometry into a **512-dimensional vector embedding** using an ArcFace Core ML model.
5. The raw camera frames are immediately cleared from memory. Only the mathematical vector is encrypted and stored.

---

## 4. Protecting Applications

1. Click the **Latch** icon in the macOS menu bar or open **Settings**.
2. Navigate to **Protected Apps**.
3. Click **Add App** and select any application installed on your Mac (e.g., Mail, Slack, Messages, Photos, Terminal).
4. When you attempt to launch or switch to a protected app, Latch seamlessly displays the Dynamic Notch scan window.
5. A simple glance at your Mac verifies your face and unlocks the app instantly!

---

## 5. Checking for Updates

Latch includes integrated automatic updates powered by Sparkle:
- Click the Menu Bar icon → **Settings** → **About**.
- Click **Check for Updates** to check for new releases directly from GitHub.
- You can also toggle **Automatically check for updates** to keep Latch seamlessly up to date.
