# HP 8570W Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.7.7-green.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/)
[![GitHub release](https://img.shields.io/github/tag/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/)

### Before you give this EFI a try, make sure you read [this](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10), [this](#BIOS-versions) and [this](#Generating-your-own-serial-and-Editing-ROM)!

This repo includes an OpenCore EFI for 8570W. If anyone has an 8770W, we would like to hear a feedback [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/14).

### This EFI only works on NVIDIA GPUs and TN-Panel. DreamColor screens will not be supported!

AMD FirePro M4000 users can check [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/discussions/20).

Tested on:

| Specs | Laptop 1 ([@HansHubertHass](https://github.com/HansHubertHass)) | Laptop 2 ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- |
| CPU | Intel Core i7-3720QM | Intel Core i7-3840QM |
| GPU | Nvidia Quadro K1000M  | |
| RAM | 16 GB 1600 MHz DDR3  | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | |
| WiFi | Azureware AW-CE123H (BCM94352HMB) | BCM943224HMS |
| Bluetooth | HP BCM20702MD |
| macOS | 10.15.7 (Catalina) | 11.5.2 (Big Sur) |
| BIOS | F.31 | F.61 |

## What works?

- Audio
- Battery readout
- Boot
- Bluetooth
- Broadcom WiFi (incl. Handoff + AirDrop)
- Docking Station (USB + DVI-D)
- Ethernet
- ExpressCard
- GPU acceleration
- Keyboard + Trackpad (incl. Magic TrackPad 2 emulation)
- SD-Card
- Sleep
- Sleep through closing the lid
- TrackPoint
- Power Management
- (NVIDIA) Internal Display shows as internal [Learn more](#NVIDIA-Patches)

## What doesn't work?

- (NVIDIA) Since the 8570W does not have an iGPU and this laptop only has a K1000M, there is no brightness control.
- Docking Station Audio

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) page of this repo and download the latest release. Then, copy the EFI folder to your EFI partition... That's it.

## How to Install macOS Big Sur

There are two ways you can install Big Sur:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacBookPro9,1

use PlistEdit Pro or any decent plist editor to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

You should also put in your ethernet adapter's MAC address into the ROM section.

## WiFi

For stock HP BCM943224HMS users, WiFi will work out of the box, no need to do anything here.

Intel WiFi users will need remove AirportBrcmFixup.kext, replace it with [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) and do an OC snapshot to get WiFi working. If you want the full macOS experience with AirDrop, Handoff and all of that, replace the Intel WiFi card with a supported Broadcom one.

Recommended WiFi cards: Azureware AW-CE123H, Dell DW1550

## BIOS settings

***System Configuration Menu***

**Boot Options**

* SecureBoot: Disabled

* Boot Mode: UEFI / UEFI Hybrid (with CSM)

**Device Configurations**
    
* SATA Device Mode: AHCI

* Wake on USB: Disabled

* Virtualization Technology: Enabled

* Virtualization Technology for Directed I/O: Disabled

* TXT Technology: Disabled

* Intel HT Technology: Enabled

**Built-in Device Options**
    
* LAN/WLAN Switching: Disabled

* Fingerprint Device: Disabled

**Port Options**
    
* Serial Port: Disabled

* Parallel Port: Disabled

* Flash media reader: Enabled

* 1394 Port: Disabled

* Smart Card: Disabled

## BIOS versions

I decided to include this topic in the README instead of my GitHub Pages for visibility. Basically due to the fact that HP is preventing users from downgrade the BIOS, I recommend sticking to the oldest version if possible. For now, F.31, F.61 and F.71 (currently latest version) are confirmed working. I would like to complete my table which can be viewed [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10). Contributing to this project will help a ton :). If you want to know whether or not your BIOS is confirmed working, you can check [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues?q=label%3ABIOS+). If you cannot find your BIOS version, please try the EFI out and provide feedback [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/new/choose).

## macOS Monterey

If you want to run Monterey, you have to set the following:

| Quirk | Value to set | Where To Find |
| -- | -- | -- |
| csr-active-config | 030A0000 | Under NVRAM/7C436110-AB2A-4BBB-A880-FE41995C9F82 |

After installing Monterey, you need to install the Post-Install Volume Patch using [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) to patch the NVIDIA graphics kexts back to Monterey. Keep in mind that you'll lose System Integrity Protection and the ability to apply Delta OTA updates after doing this.
The patch needs to be reapplied after every macOS update.

## Credits

Thanks to:

- me (for wasting my time writing this, providing fixes and creating this EFI)
- acidanthera (for making an awesome bootloader)
- Rehabman (for fixing keyboard issues and providing patches for 8x70)
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for the idea to do this because there were no downloads outside of China :P)
- [HansHubertHass](https://github.com/HansHubertHass) and [Bautheile](https://github.com/Bautheile) (for being our testers)
