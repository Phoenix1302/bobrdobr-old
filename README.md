# BobrDobr VPN — Legacy Python & PySide6 Client (Deprecated)

[![Status](https://img.shields.io/badge/status-archived%20%2F%20deprecated-critical)](https://github.com/Phoenix1302/BobrDobr)
[![Python](https://img.shields.io/badge/python-3.11%20%7C%203.12%20%7C%203.13-blue.svg)](https://www.python.org/)
[![GUI](https://img.shields.io/badge/GUI-PySide6%20Qt6-green.svg)](https://wiki.qt.io/Qt_for_Python)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux-lightgrey.svg)]()

> [!CAUTION]
> **ARCHIVAL NOTICE — ACTIVE DEVELOPMENT HAS MOVED**
>
> This repository contains the historical **Python 3 / PySide6 (Qt6)** codebase of the **BobrDobr VPN** client (versions `v1.0.0` through `v1.1.1`).
>
> Active development has fully transitioned to the next-generation, high-performance **Pure-Go + WebView2** architecture located at:
> 
> 👉 **[https://github.com/Phoenix1302/BobrDobr](https://github.com/Phoenix1302/BobrDobr)**
>
> This repository is maintained as an open-source archive for reference, reproducible builds, and historical continuity.

---

## Overview

BobrDobr VPN is a privacy-first, censorship-resilient VPN client designed to manage dual-protocol tunnels:
- **AmneziaWG (WireGuard with advanced obfuscation parameters)**: custom junk packets (`Jc`, `Jmin`, `Jmax`), custom magic headers (`S1`, `S2`), and custom transport ports (`H1`-`H4`) designed to resist deep packet inspection (DPI).
- **OpenConnect / Cisco AnyConnect / ocserv**: enterprise-grade SSL VPN tunneling support.

This legacy version features a native desktop GUI implemented in **PySide6 (Qt6)**, complete with dark theme ergonomics, real-time adapter health monitoring, subscription sync, and single-binary packaging.

---

## Repository Structure

```
bobrdobr-old/
├── assets/                    # Application icons (beaver.ico), branding logos, and SVGs
├── bin/
│   ├── linux/                 # Bundled Linux helper binaries (amneziawg-go, openconnect, vpnc-script)
│   └── windows/               # Bundled Windows binaries (amneziawg-go.exe, openconnect-worker.exe, wintun.dll)
├── docker/
│   ├── Containerfile.windows  # Wine-based container definition for isolated Windows cross-builds
│   └── scripts/               # Container build helper scripts
├── scripts/
│   ├── generate_ico.py        # Windows multi-resolution icon generator
│   └── generate_release_notes.py # Automated release notes generator
├── src/
│   ├── assets/                # In-app assets bundled into the executable
│   ├── core/                  # Protocol engines (AmneziaWG, OpenConnect, subscription management)
│   ├── gui/                   # PySide6 / Qt6 widgets, tray icon, custom title bars, dialogs
│   ├── utils/                 # Network helpers, adapter detection, DNS guards, logger, version metadata
│   └── main.py                # Main application entry point and lifecycle manager
├── tests/                     # Test suite (adapter protection, reconnect safety, single-instance)
├── BobrDobr-VPN.spec          # PyInstaller multi-target build specification
├── build.ps1                  # PowerShell build script (Podman / native)
├── build.sh                   # POSIX shell build script
├── Containerfile.build         # Linux container definition for isolated container builds
├── Makefile                   # Build automation targets (make build-windows, make build-linux)
└── requirements.txt           # Python dependencies (PySide6, PyInstaller, certifi)
```

---

## Build Instructions

### Method 1: Isolated Containerized Build via Podman (Recommended)

To guarantee zero host contamination and reproducible outputs, builds can be run entirely inside isolated containers.

#### Windows Executable (`.exe`) via Wine:
Using PowerShell:
```powershell
.\build.ps1 -Target windows
```
Or using `make`:
```bash
make build-windows
```
This builds an isolated Wine container (`docker/Containerfile.windows`) with Windows Python 3.11.8, installs PySide6 and PyInstaller, bundles `bin/windows` and `assets`, and outputs standalone binaries to `dist/windows/`.

#### Linux Executable:
Using PowerShell:
```powershell
.\build.ps1 -Target linux
```
Or using `make`:
```bash
make build-linux
```
Outputs the compiled Linux standalone binary to `dist/linux/`.

---

### Method 2: Native Windows Build with Python 3.11

Prerequisites:
- **Python 3.11 (64-bit)** installed on Windows.
- Administrator command prompt or PowerShell.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Phoenix1302/bobrdobr-old.git
   cd bobrdobr-old
   ```

2. **Create and activate a virtual environment:**
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. **Install dependencies:**
   ```powershell
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. **Compile with PyInstaller:**
   ```powershell
   pyinstaller --noconfirm --clean BobrDobr-VPN.spec
   ```

5. **Locate the binary:**
   The output executable will be placed in `dist/` (e.g. `dist/BobrDobr-VPN-v1.1.1.exe`).

---

## Running the Test Suite

The legacy codebase includes a comprehensive unit and integration test suite covering adapter safety, reconnect protection, single-instance locks, and subscription handling:

```powershell
pytest tests/ -v
```

---

## License & Credits

- Original BobrDobr Client: [Phoenix1302](https://github.com/Phoenix1302)
- New pure-Go repository: [https://github.com/Phoenix1302/BobrDobr](https://github.com/Phoenix1302/BobrDobr)
- Bundles Wintun (`wintun.dll`) under the [Wintun License](https://www.wintun.net/).
- OpenConnect is distributed under the GNU Lesser General Public License (LGPL v2.1+).
- AmneziaWG is distributed under the GPL-2.0 / MIT licenses.
