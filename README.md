# AharaFlow Admin — Official Desktop Releases

Public binary distribution and signed OTA auto-update host for **AharaFlow Platform Superadmin Desktop Control Panel**.

> This repository hosts **compiled desktop installers, Ed25519 updater signatures, manifests (`latest.json`), and SHA-256 checksums only**. It contains **zero application source code**.

---

## Permanent Stable Downloads

Always points to the latest verified **Stable** release (`make_latest=true`):

| Platform | Architecture | Format | Permanent Download Link |
| :--- | :--- | :--- | :--- |
| **Windows 10/11** | x64 (`x86_64`) | NSIS Setup (`.exe`) | [AharaFlow-Admin-Windows-x64-setup.exe](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-Windows-x64-setup.exe) |
| **macOS 12.0+** | Apple Silicon (`arm64`) | Disk Image (`.dmg`) | [AharaFlow-Admin-macOS-arm64.dmg](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-macOS-arm64.dmg) |
| **macOS 12.0+** | Intel (`x86_64`) | Disk Image (`.dmg`) | [AharaFlow-Admin-macOS-x64.dmg](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-macOS-x64.dmg) |
| **Linux** (Ubuntu 22.04+, Debian 12+, Fedora 38+) | x64 (`x86_64`) | AppImage | [AharaFlow-Admin-Linux-x64.AppImage](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-Linux-x64.AppImage) |
| **Checksums** | All Platforms | SHA-256 | [SHA256SUMS.txt](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/SHA256SUMS.txt) |

---

## Integrity & Checksum Verification

Every release includes **`SHA256SUMS.txt`** covering all versioned installers, permanent versionless aliases, OTA payloads, `.sig` files, and `latest.json`.

### Windows (PowerShell)

```powershell
# Download SHA256SUMS.txt alongside the installer, then compare:
Get-FileHash .\AharaFlow-Admin-Windows-x64-setup.exe -Algorithm SHA256
Get-Content .\SHA256SUMS.txt | Select-String "AharaFlow-Admin-Windows-x64-setup.exe"
```

### macOS (Terminal)

```bash
# Verify ARM64 or Intel DMG against SHA256SUMS.txt
shasum -a 256 AharaFlow-Admin-macOS-arm64.dmg
grep "AharaFlow-Admin-macOS-arm64.dmg" SHA256SUMS.txt
```

### Linux (Terminal)

```bash
sha256sum -c SHA256SUMS.txt --ignore-missing
```

---

## Installation Instructions & Unsigned Installer Notices

> **Security & Signing Notice (Current Stage):**
> - **In-App Auto-Updates (OTA):** Cryptographically protected on **all 4 platforms** by mandatory **Tauri Ed25519 (Minisign)** signatures verified against the compiled-in public key (`C0108F8FED291761`) before any update is installed.
> - **Initial Manual Installers (Windows & macOS):** Paid third-party OS code-signing certificates (Windows Authenticode / Apple Developer ID & Notarization) are deferred to a future upgrade. Until enabled, Windows SmartScreen and macOS Gatekeeper will display an **unverified / unknown publisher** prompt on first manual install. Always verify `SHA256SUMS.txt` before opening a downloaded installer.

### Windows 10 (1809+) / Windows 11 x64

1. Download [AharaFlow-Admin-Windows-x64-setup.exe](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-Windows-x64-setup.exe) and verify its SHA-256 hash against `SHA256SUMS.txt`.
2. Double-click the installer (requires Microsoft Edge WebView2 Runtime, preinstalled on Windows 11 and modern Windows 10).
3. If **Windows SmartScreen** displays *"Windows protected your PC"* / *"Unknown publisher"*, click **More info** → **Run anyway**.
4. The NSIS installer performs a clean per-user installation without requiring UAC elevation.

### macOS 12.0+ (Apple Silicon ARM64 & Intel x64)

1. Download the `.dmg` matching your Mac's processor (`arm64` for M1/M2/M3/M4, `x64` for Intel) and verify its SHA-256 hash against `SHA256SUMS.txt`.
2. Open the `.dmg` and drag **AharaFlow Admin.app** into `/Applications`.
3. Because the app is currently unsigned/unnotarized, **macOS Gatekeeper** will block a normal double-click on first launch (*"cannot be opened because the developer cannot be verified"*):
   - **Recommended:** Right-click (or Control-click) `AharaFlow Admin.app` in `/Applications` → click **Open** → confirm **Open**, OR go to **System Settings → Privacy & Security** → click **Open Anyway**.
   - **Internal Testing Only:** Clear the quarantine attribute with:
     ```bash
     xattr -cr "/Applications/AharaFlow Admin.app"
     ```

### Linux x64 (AppImage — `glibc 2.35+`)

1. Download [AharaFlow-Admin-Linux-x64.AppImage](https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/AharaFlow-Admin-Linux-x64.AppImage) and verify `sha256sum -c SHA256SUMS.txt --ignore-missing`.
2. Make it executable:
   ```bash
   chmod +x AharaFlow-Admin-Linux-x64.AppImage
   ./AharaFlow-Admin-Linux-x64.AppImage
   ```
3. **FUSE Prerequisite:** AppImages require `libfuse2` (`sudo apt install libfuse2` on Ubuntu 22.04 / Debian 12, or `sudo apt install libfuse2t64` on Ubuntu 24.04). If FUSE is unavailable, run:
   ```bash
   ./AharaFlow-Admin-Linux-x64.AppImage --appimage-extract-and-run
   ```

---

## Signed Auto-Updater Endpoints (`tauri-plugin-updater`)

- **Stable Channel:** `https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/latest/download/latest.json`
- **Beta / RC Channel:** `https://github.com/Kshitij-Vijay-Pawar/pos-admin-releases/releases/download/beta-channel/latest.json`

All OTA update manifests reference immutable versioned payloads (`/releases/download/v<VERSION>/...`) for the 4 standard Tauri 2 platform keys (`windows-x86_64`, `darwin-aarch64`, `darwin-x86_64`, `linux-x86_64`).

---

## Retention, CDN & Firewall Allowlist Policy

- **Release Retention:** At least the **last 10 published Stable releases** are retained permanently for rollback and auditability. Published releases are immutable and never overwritten.
- **CDN Convergence:** GitHub Releases CDN (`/releases/latest/download/...`) may take up to **~5 minutes** to converge after a new publication or rollback demotion.
- **Restaurant / Corporate Firewall Allowlist:** Ensure outbound HTTPS (TCP `443`) is permitted to:
  - `github.com`
  - `objects.githubusercontent.com`
  - `release-assets.githubusercontent.com`
