# HP 8570W Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.7.2-green.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/)
[![GitHub release](https://img.shields.io/github/tag/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/)

### Before you give this EFI a try, make sure you read [this](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10), [this](#BIOS-versions) and [this](#Generating-your-own-serial-and-Editing-ROM)!

Please try to keep the BIOS version as low as possible! Newer BIOS versions block downgrading.

This repo includes an OpenCore EFI for 8570W.

This repo may get updated more slowly as I have little free time to continue this project. 

### We're also looking for 8770W users, so if anyone has one of these, let us know!

### EFI Compatibility list:

| GPU | Display | Supported? | Additional notes |
| :-----: | :-----: | :-----: | :-----: |
| NVIDIA Quadro K1000M/K2000M | TN-Panel | **Yes** | 1 |
| AMD FirePro M4000 | TN-Panel | _Unknown_ | 2 |
| NVIDIA Quadro K1000M/K2000M | DreamColor 2 | _No_ | 3 |
| AMD FirePro M4000 | DreamColor 2 | _No_ | 3 |

Bold: confirmed working

Italic: not confirmed

1: Brightness control does NOT work

2: add `radpg=15` as boot-arg

3: Since users of the DreamColor screen can't use any of the stock GPUs: There's the possibility of using a FirePro W5170M or WX4170M, which uses a proper eDP display signal, thus making the internal screen + backlight control work under macOS. Note: You won't be able to use any of the TN panels as these newer GPUs don't give out any LVDS signals. And on the WX4170M, you won't be able to control the GPU fans. If anyone uses any of those GPUs in their 8570W, let us know, so we can test.


Tested on:

| Specs | Laptop 1 ([@HansHubertHass](https://github.com/HansHubertHass)) | Laptop 2 ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- |
| CPU | Intel Core i7-3720QM | Intel Core i7-3840QM |
| GPU | Nvidia Quadro K1000M  | |
| RAM | 16 GB 1600 MHz DDR3  | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | |
| WiFi | Azureware AW-CE123H (BCM94352HMB) | BCM943224HMS (WiFi-only card) |
| Bluetooth | HP BCM920702MD |
| OS | macOS 10.15.7 Catalina | |
| BIOS | F.31 | F.61 |

If you're on a later BIOS and have any issues trying to boot or whatever, we recommend downgrading to F.61.

## What works?

- Audio
- Battery readout
- Boot
- Docking Station (USB + DVI-D)
- Ethernet
- ExpressCard
- GPU acceleration
- Keyboard + Trackpad (incl. Magic TrackPad 2 emulation)
- Sleep
- Sleep through closing the lid
- WiFi (incl. Handoff + AirDrop)
- (NVIDIA) Internal Display shows as internal [Learn more](#NVIDIA-Patches)

## What doesn't work?

- (For NVIDIA) Since the 8570W does not have an iGPU and this laptop only has a K1000M, there is no brightness control.
- Power Management
- SD-Card
- TrackPoint
- Docking Station Audio

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) page of this repo and download the latest release. Then, copy the EFI folder to your EFI partition... That's it.

## How to Install macOS Catalina

There are two ways you can install Catalina:

1. Have a working install of macOS, download the Installer from the App Store, then make a bootable Installer with createinstallmedia by using this command in Terminal `sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) from the offical OpenCore release package.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual (GUID Partiton Map; APFS). After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacBookPro9,1 or 10,1

use PlistEdit Pro or any decent plist editor to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

You should also put in your ethernet adapter's MAC address into the ROM section.

## Bluetooth

As the stock WiFi cards don't have Bluetooth built-in, HP uses a dedicated broadcom bluetooth module which works fine under macOS.

## WiFi

If you have the stock Intel cards:
You need remove AirportBrcmFixup.kext and replace it with [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) to get WiFi working. But for the full macOS experience with AirDrop, Handoff and all of that, replace the Intel WiFi card with a supported Broadcom one.

Recommended WiFi cards: Azureware AW-CE123H, Dell DW1550, Lenovo Lite-On WCBN606BH

## BIOS settings

***System Configuration Menu***

**Boot Options**

* SecureBoot: Disabled

* Boot Mode: UEFI Hybrid (with CSM)

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

* Flash media reader: Disabled (FOR NOW)

* Smart Card: Disabled

## BIOS versions

I decided to include this topic in the README instead of my GitHub Pages for visibility. Basically due to the fact that HP is preventing users from downgrade the BIOS, I recommend sticking to the oldest version if possible. For now, F.31, F.61 and F.71 (currently latest version) are confirmed working. I would like to complete my table and it can be viewed [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10). Contributing to this project will help a ton :). If you want to know whether or not your BIOS is confirmed working, you can check [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues?q=label%3ABIOS+). If you cannot find your BIOS version, please try the EFI out and provide feedback [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/new/choose).

## NVIDIA Patches
Starting with v4.3.13, I've added patches for NVIDIA GPUs to enable "Internal Display" in "About This Mac". Unfortunately, the backlight control still doesn't work as the K1000M is limited. Therefore it is deactivated in "System Preferences". Information on how to activate these patches can be found at this [link](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/1#issuecomment-819961384).

## Credits

Thanks to:

- me (for wasting my time writing this, providing fixes and creating this EFI)
- acidanthera (for making an awesome bootloader)
- Rehabman (for fixing keyboard issues and providing patches for 8x70)
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for the idea to do this because there were no downloads outside of China :P)
- [HansHubertHass](https://github.com/HansHubertHass) and [Bautheile](https://github.com/Bautheile) (for being our testers)
