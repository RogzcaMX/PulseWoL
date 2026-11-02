<div align="center">

# Pulse WoL
### By: Rogzca

[![Leer en Español](https://img.shields.io/badge/Leer%20en-Español-red?style=for-the-badge)](../README.md)

**Lightweight, secure, and straightforward power management for your computers over local area network (LAN).**

[Download Latest Release](https://github.com/RogzcaMX/PulseWoL/releases/latest) • [Installation](#installation--usage) • [Wake-on-LAN Setup](#wake-on-lan-wol-configuration) • [Features](#key-features)

</div>

---

## Overview

**Pulse WoL** is an ecosystem designed to handle remote power-on (**Wake-on-LAN**) and **remote shutdown** for Windows PCs directly from your Android device, free from external cloud services or complex networking.

The solution consists of two independent components communicating strictly within your local network:

1. **Pulse WoL Android (`.apk`)**: Native Jetpack Compose app featuring multi-device management, decoupled priority ordering, and interactive home screen widgets.
2. **Pulse WoL Server (`.exe`)**: Minimal background service for Windows operating at 0% idle CPU, quietly tucked into the System Tray.

---

## Key Features

### Android App
* **LAN Auto-Discovery**: Detects and links PCs in seconds via UDP broadcast without manual entry of MAC or IP addresses.
* **Decoupled Multi-Device Management**: Maintain independent list orders and priorities for the Wake and Shutdown tabs.
* **Home Screen Widgets powered by Jetpack Glance**:
  * **Extended Mode**: Displays target machine names directly above action triggers.
  * **Compact Mode (Vertical)**: Minimalist switch-style layout (1×2) with dedicated vector icons.
* **Flexible Widget Target Modes**: Set widgets to trigger the top-ordered machine automatically, stick to a single global device, or handle custom targets per action.
* **Theming**: Complete support for Light Mode, Dark Mode, and dynamic system colors (Material You).

### Windows Server
* **Silent Operation**: Runs in the background and minimizes to the Windows System Tray to keep your taskbar clean.
* **Surgical Shutdown Filter**: Listens exclusively for shutdown commands matching the physical network interface (MAC) of the host machine.
* **Optional Auto-Start**: Built-in toggle to boot with Windows silently.
* **Guided Installer**: Streamlined Inno Setup installer for easy setup and clean uninstallation.

---

## Installation & Usage

### 1. Server (Windows)
1. Head to the **[Releases](https://github.com/RogzcaMX/PulseWoL/releases/latest)** section and download the server installer (`PulseWoLServer_Setup.exe` or equivalent).
2. Run the setup wizard and follow the steps.
3. Upon launch, the Pulse WoL icon will appear in your system tray next to the taskbar clock. If Windows Defender Firewall prompts for network permissions, grant access for Private Networks to allow inbound UDP discovery and commands.

### 2. Mobile App (Android)
1. Grab the latest `.apk` from the **[Releases](https://github.com/RogzcaMX/PulseWoL/releases/latest)** tab.
2. Install it on your device (allow installation from external sources if prompted by Android).
3. Ensure your phone is connected to the **same Wi-Fi network** as the PC.
4. Tap **Search PC Automatically** in either tab to pair your desktop with one tap.

---

## Wake-on-LAN (WoL) Configuration

To wake up your computer over the local network, the feature must be enabled in both the hardware (BIOS/UEFI) and within the operating system (Windows).

---

### 1. Motherboard BIOS / UEFI

> **Important Note:** Every manufacturer organizes and names BIOS settings differently. If the options below differ slightly from your screen, refer to the official manual for your specific motherboard model.

#### Generic Procedure:
1. Power down your PC, then turn it on while repeatedly pressing the BIOS key (typically `Del`, `Delete`, `F2`, or `F12` depending on your brand).
2. Switch to **Advanced Mode** (often mapped to the `F7` key).
3. Navigate to the Power or Onboard Peripherals tab. Common locations include:
   * **Advanced** > **APM Configuration** (ASUS / ASRock).
   * **Settings** > **Advanced** > **Wake Up Event Setup** (MSI).
   * **Power** or **Power Management Setup** (Gigabyte).
4. Locate and **Enable** the WoL-related power option, usually titled:
   * *Power On By PCIE/PCI* or *Power On By PCI-E Devices*.
   * *Resume By Onboard Intel LAN* / *Realtek LAN*.
   * *Wake on LAN* / *PME Event Wake Up*.
5. If you find an **ErP Ready** or **Deep Sleep** state option, set it to **Disabled**. Complete power cut-offs prevent the Ethernet controller from listening for Magic Packets while turned off.
6. Save and exit (typically by pressing `F10`).

---

### 2. Windows 10 & Windows 11

Once enabled in BIOS, Windows and your network adapter must be allowed to wake the system:

#### A. Network Adapter Properties:
1. Right-click the Windows Start menu button and select **Device Manager**.
2. Expand the **Network adapters** section.
3. Double-click your primary Ethernet adapter (e.g., *Realtek PCIe GbE Family Controller* or *Intel Ethernet Connection*).
4. Switch to the **Power Management** tab and ensure all three boxes are checked:
   * *Allow this device to wake the computer*.
   * *Only allow a magic packet to wake the computer*.
5. Switch to the **Advanced** tab, scroll through the list, and configure:
   * **Wake on Magic Packet**: `Enabled`.
   * **Shutdown Wake-On-LAN**: `Enabled`.
   * **Energy Efficient Ethernet / Green Ethernet**: `Disabled` *(recommended to avoid deep sleep link drops)*.
6. Click **OK**.

#### B. Disable Windows Fast Startup *(Recommended)*:
Windows Fast Startup can put the system into a hybrid hibernation state (S4/S5) that prevents network cards from listening for wake triggers:
1. Open the classic **Control Panel** and navigate to **Power Options**.
2. From the left sidebar, click **Choose what the power buttons do**.
3. Click **Change settings that are currently unavailable** near the top.
4. Uncheck **Turn on fast startup (recommended)**.
5. Click **Save changes**.

---

## System Requirements

* **Android**: Android 6.0 (API 23) or higher.
* **Windows**: Windows 10 / Windows 11.
* **Network Hardware**: Motherboard connected via Ethernet cable (LAN) with Wake-on-LAN hardware support.

---

## License

Distributed under the repository's specified license. See the accompanying license file for further information.
