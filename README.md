# HP 8570W Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.6.6-green.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/)
[![GitHub release](https://img.shields.io/github/release/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/)

### Before you give this EFI a try, make sure you read [this](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10) and [this](#BIOS-versions)!

Please try to keep the BIOS version as low as possible! New BIOS versions block the downgrade.

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
- WiFi (incl. Handoff + AirDrop)
- (NVIDIA) Internal Displays shows as internal [Learn more](#NVIDIA-Patches)

## What doesn't work?

- (For NVIDIA) Since the 8570W does not have an iGPU and this laptop only has a K1000M, there is no brightness control.
- Sleep through closing the lid
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

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Bluetooth

As the stock WiFi cards don't have Bluetooth built-in, HP uses a dedicated broadcom bluetooth module which works fine under macOS. If you have a wifi card with bluetooth built-in and wish to use that instead, unplug the bluetooth module from the laptop and isolate Pin 51 on the WiFi card.

## WiFi

If you have the stock Intel cards:
You need remove AirportBrcmFixup.kext and replace it with [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases/tag/v1.2.0) to get WiFi working. But for the full macOS experience with AirDrop, Handoff and all of that, replace the Intel WiFi card with a supported Broadcom one.

Recommended WiFi cards: Azureware AW-CE123H, DW1550, BCM943224HMS (no Bluetooth), DW1520 (no Bluetooth)

## BIOS settings

// TO-DO

## BIOS versions

I decided to include this topic in the README instead of my GitHub Pages for visibility. Basically due to the fact that HP is preventing users from downgrade the BIOS, I recommend sticking to the oldest version if possible. For now, F.31 and F.61 are confirmed working. I would like to complete my table and it can be viewed [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/10). Contributing to this project will help a ton :). If you want to know whether or not your BIOS is confirmed working, you can check [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues?q=label%3ABIOS+). If you cannot find your BIOS version, please try the EFI out and provide feedback [here](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/new/choose).

## NVIDIA Patches
Starting with v4.3.13, I've added patches for NVIDIA GPUs to enable "Internal Display" in "About This Mac". Unfortunately, the backlight control still doesn't work as the K1000M is limited. Therefore it is deactivated in "System Preferences". Information on how to activate these patches can be found at this [link](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/1#issuecomment-819961384).

## Credits

Thanks to:

- me (for wasting my time writing this, providing fixes and creating this EFI)
- acidanthera (for making an awesome bootloader)
- Rehabman (for fixing keyboard issues and providing patches for 8x70)
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for the idea to do this because there were no downloads outside of China :P)
- [HansHubertHass](https://github.com/HansHubertHass) and [Bautheile](https://github.com/Bautheile) (for being our testers)
