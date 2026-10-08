# Frequently Asked Questions & Troubleshooting

Answers to common questions and troubleshooting tips for **Latch**.

---

## Frequently Asked Questions

### Is my face data or camera stream uploaded anywhere?
**No, never.** All computer vision, feature landmark detection, and ArcFace vector extractions execute 100% on-device via Apple's Vision and Core ML frameworks. No camera frames are saved to disk, and no network requests are made.

### Can someone unlock my apps with a photograph or video?
Latch features multi-stage **Liveness Detection** that evaluates 3D face landmark parallax, surface reflections, device bezel outlines, and blink cues over a rolling temporal window. Printed photos and screen-based photos are rejected.

### What happens if the camera cannot see me (e.g. clamshell mode or dark room)?
If face recognition fails or times out, you can unlock immediately using your standard **Touch ID** sensor or Mac login password.

### Which Macs are supported?
Latch supports Apple Silicon Macs (M1, M2, M3, M4) and Intel Macs running **macOS 15.0 Sequoia** or newer.

---

## Troubleshooting

### "macOS cannot verify the developer" / Gatekeeper Warning
Because Latch is distributed independently via GitHub:
1. In Finder, locate `Latch.app` in `/Applications`.
2. Right-click (or Control-click) `Latch.app` and choose **Open**.
3. In the security dialog that appears, click **Open**.
4. Alternatively, open **System Settings** → **Privacy & Security**, scroll down to the Security section, and click **Open Anyway**.

### Camera Access Not Prompted or Denied
1. Open **System Settings** → **Privacy & Security** → **Camera**.
2. Ensure the toggle next to **Latch** is switched **ON**.
3. If Latch is not in the list, restart Latch and grant camera permissions when prompted.

### Accessibility Permission Required
1. Open **System Settings** → **Privacy & Security** → **Accessibility**.
2. Ensure **Latch** is enabled. This permission is necessary for frontmost window and application lock orchestration.

### How to Reset Enrolled Face Data
1. Open Latch **Settings** from the menu bar.
2. Under **Identities**, select your enrolled profile.
3. Click **Delete Identity**. The AES-encrypted embedding file is securely unlinked from disk immediately.
