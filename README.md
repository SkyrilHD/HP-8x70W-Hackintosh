# HP 8x70W Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.7.9-green.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/)
[![GitHub release](https://img.shields.io/github/tag/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/)

### Before you give this EFI a try, make sure you read [this](#monterey-nvidia-gpus) and [this](#Generating-your-own-serial-and-Editing-ROM)!

This repo includes an OpenCore EFI for the 8570W and 8770W with the 1080p TN display. **DreamColor IPS displays are NOT supported!**

### Supported configurations:

NVIDIA: Quadro K1000M-K4000M / K1100M-K4100M

AMD: FirePro M4000

Tested on:

| Specs | 8770W ([@HansHubertHass](https://twitter.com/MacGen2)) | 8570W ([@HansHubertHass](https://twitter.com/MacGen2)) | 8570W ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- | -- |
| CPU | Intel Core i5-3360M | Intel Core i7-3740QM | Intel Core i7-3840QM |
| GPU | AMD FirePro M4000 | Nvidia Quadro K1000M | Nvidia Quadro K1000M | 
| RAM | 8 GB 1600 MHz DDR3 | 8 GB 1600 MHz DDR3 | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | | |
| WiFi | Azureware AW-CE123H (BCM94352HMB) | Intel Centrino Advanced-N 6200 | BCM943224HMS |
| Bluetooth | HP BCM20702MD |
| macOS | Monterey | Catalina | Big Sur |

## Working features

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
- SD-Card (if disabling IEEE 1394)
- Firewire / IEEE 1394 (if disabling Flash media reader)
- Sleep
- TrackPoint
- Power Management
- (NVIDIA) Internal Display shows as internal

## Known Issues / not working

- (NVIDIA) Brightness Control
- Docking Station Audio
- Fingerprint sensor
- One of the left USB 2.0 ports is broken due to a stupid implementation from HP (why did they map the internal bluetooth controller to the same port that an external device plugs into)

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) page of this repo and download the latest release. Then, copy the EFI folder to your EFI partition... That's it.

## How to Install macOS:

There are two ways for installation:

1. If you have a working macOS install, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: 

    **Big Sur**: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

    **Monterey**: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacBookPro9,1 or 10,1

use PlistEdit Pro or any decent plist editor to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

You should also put in your ethernet adapter's MAC address into the ROM section.

## WiFi

If you have the stock HP BCM943224HMS or a card based on the BCM94352HMB wifi chip, they will work out of the box, no need to do anything here.

Intel WiFi users will need remove AirportBrcmFixup.kext, replace it with [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) and do an OC snapshot with ProperTree to get WiFi working. If you want the full macOS experience with AirDrop, Handoff and all of that, replace the Intel WiFi card with a supported Broadcom one.

Recommended WiFi cards: Azureware AW-CE123H, Dell DW1550

#### Country Code for Broadcom Wireless Cards

Some countries have different 5GHz bands and may not be supported for some. 
You can specify other country codes like: US, DE, #a, etc by going into:

- `EFI/OC/config.plist > DeviceProperties > Add > PciRoot(0x0)/Pci(0x1C,0x3)/Pci(0x0,0x0)` and rename/uncomment:
- `#brcmfx-country` to `brcmfx-country` and set the desired value (**#a** is the preset value, replace with the country code that you need)

Some cards however have their country code hardcoded to the module in which the setting causes macOS to either no longer boot or your wifi to stop working and you're pretty much out of luck at that point.

## BIOS settings

***System Configuration Menu***

**Boot Options**

* SecureBoot: Disabled

* Boot Mode: UEFI

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

## Monterey (NVIDIA GPUs)

For those using an NVIDIA GPU wanting to run Monterey:

3. Navigate to `Misc > Security` and change `SecureBootModel` to `Disabled`

4. Nagivate to `NVRAM > 7C436110-AB2A-4BBB-A880-FE41995C9F82` and change `csr-active-config` from `00000000` to `00080000`

After installing Monterey, you need to install the Post-Install Volume Patch using [GeForce Kepler Patcher](https://github.com/chris1111/Geforce-Kepler-patcher) to patch the NVIDIA graphics kexts back to Monterey. Keep in mind that you'll lose System Integrity Protection and the ability to apply Delta OTA updates for doing this.
The patch needs to be reapplied after every macOS update.

AMD users won't need to do any of the steps above, as macOS currently still natively supports all GCN based AMD GPUs.

## Credits

Thanks to:

- me (for wasting my time writing this, providing fixes and creating this EFI)
- acidanthera (for making an awesome bootloader)
- Rehabman (for fixing keyboard issues and providing patches for 8x70)
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for the idea to do this because there were no downloads outside of China :P)
- [HansHubertHass](https://github.com/HansHubertHass) and [Bautheile](https://github.com/Bautheile) (for being our testers)
- [Krutav](https://forums.macrumors.com/threads/2011-imac-graphics-card-upgrade.1596614/post-30941047) (Dell vBIOS injection through OpenCore on the ROMless HP FirePro M4000)
