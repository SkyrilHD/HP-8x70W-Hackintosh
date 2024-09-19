# HP 8x70W Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.9.7-green.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/)
[![GitHub release](https://img.shields.io/github/tag/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/HP-8570W-Hackintosh.svg)](https://github.com/SkyrilHD/HP-8570W-Hackintosh/issues/)

### Before you give this EFI a try, make sure you read [this](#Generating-your-own-serial-and-Editing-ROM)!

This repo includes an OpenCore EFI for the 8x70W series with the 1080p TN display. **DreamColor IPS displays are NOT supported!**

### Supported configurations:

NVIDIA: Quadro K1000M-K5000M / K1100M-K5100M

AMD: FirePro M4000

Tested on:

| Specs | 8770W ([@HansHubertHass](https://twitter.com/MacGen2)) (dead) | 8570W ([@HansHubertHass](https://twitter.com/MacGen2)) | 8570W ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- | -- |
| CPU | Intel Core i5-3360M | Intel Core i7-3740QM | Intel Core i7-3840QM |
| GPU | AMD FirePro M4000 | Nvidia Quadro K1000M | Nvidia Quadro K1000M | 
| RAM | 8 GB 1600 MHz DDR3 | 8 GB 1600 MHz DDR3 | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | | |
| WiFi | Intel Centrino Advanced-N 6200 | Azureware AW-CE123H (BCM94352HMB) | Dell DW1530 (BCM94352HMB) |
| Bluetooth | HP BCM20702MD |
| macOS | Monterey | Sonoma | Ventura |

## Working features

- Audio
- Battery readout
- Boot
- Bluetooth
- Brightness Control
- Broadcom WiFi (incl. Handoff + AirDrop)
- Docking Station
- Ethernet
- ExpressCard
- GPU acceleration (for Monterey+, check [here](#Monterey-and-newer))
- Keyboard
- SD-Card (if disabling IEEE 1394)
- Firewire / IEEE 1394 (if disabling Flash media reader)
- Sleep
- Trackpad
- TrackPoint
- Power Management

## Known Issues / not working

- DRM (Apple TV+ + Netflix & Prime, when using Safari)
- Fingerprint sensor

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) page of this repo and download the latest release. Then, copy the EFI folder to your EFI partition... That's it.

## How to Install macOS:

There are two ways for installation:

1. If you have a working macOS install, download the Installer from the App Store and create a bootable Installer with `createinstallmedia` by using the following command in Terminal: 

    - **Catalina**: `sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

    - **Big Sur**: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

    - **Monterey**: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

    - **Sonoma**: `sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After creating a bootable Installer, copy the EFI folder to the EFI partition and proceed with the installation. After completing the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacBookPro10,1.

Use PlistEdit Pro or any decent plist editor to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

Additionally, put your ethernet adapter's MAC address into the ROM section.

## WiFi

Both the stock HP BCM943224HMS or a card based on the BCM94352HMB wifi chip, they will work out of the box, and there is no need to do anything here.

**Note for Monterey and newer:** The HP BCM943224HMS may cause issues preventing the system to boot but works just fine on Big Sur and older. Replace it with the BCM94352HMB in that case.

**Intel users:** Remove AirportBrcmFixup.kext, replace it with [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) and do an OC snapshot with ProperTree to get WiFi working. If you want the full macOS experience with AirDrop, Handoff and all of that, replace the Intel WiFi card with a supported Broadcom one.

Recommended WiFi cards: Azureware AW-CE123H, Dell DW1550

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

## Monterey and newer

For users who wish to run Monterey or a newer version:

- NVIDIA

  After installation, it is necessary to apply the Post-Install Root Patch using [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) to patch the NVIDIA graphics kexts back to Monterey+. Keep in mind that you'll lose System Integrity Protection and the ability to apply Delta OTA updates for doing this.

  The patch needs to be reapplied after every macOS update.

- AMD

  AMD users won't need to follow the earlier mentioned steps, as Monterey natively supports all GCN-based AMD GPUs. However, starting with Ventura, you will need to install the Post-Install Root Patch.

## Faulty or degraded NVRAM

If you encounter installation issues, it is crucial to check if the NVRAM works. You can do this by entering the following command in Terminal:

```bash
sudo nvram myvar="$(date)"
```

After executing the command, reboot your system and verify if the value is stored in NVRAM by entering:

```bash
nvram myvar
```

If you get an error, it indicates that your 8x70W may have a faulty/degraded NVRAM. To finish the installation, you'll need to enable emulated NVRAM.

1. Enable DisableVariableWrite in `config.plist > Booter > Quirks`
2. Enable LegacyOverwrite in `config.plist > NVRAM`
3. Add OpenVariableRuntimeDxe from [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases/)
4. Add OpenVariableRuntimeDxe before OpenRuntime in the config
5. Enable LoadEarly on OpenVariableRuntimeDxe and OpenRuntime

Now, you can proceed with the installation.

Once the installation is complete, copy the LogoutHook folder, found in OpenCorePkg/Utilities, to a safe place and run the following command:

`./Launchd.command install`

## Issues with Power Management

On my tester's Intel Core i7-3740QM 8570W, we got XCPM working fine. However, if you encounter any issues regarding Power Management, try following these steps:

1. Disable SSDT-PLUG from the config
2. Enable the two keys found in config.plist > Delete
3. Disable AppleXcpmCfgLock in config.plist > Kernel > Quirks
4. Enable AppleCpuPmLock in config.plist > Kernel > Quirks

For users on Sonoma, you need to enable the AppleCPUPowerManagement and AppleCPUPowerManagementClient kexts in the config.

## Screenshot

![About this Mac](https://github.com/SkyrilHD/HP-8x70W-Hackintosh/assets/28839925/472d73ac-5202-404c-9bb9-553c44f9e1a6)

## Credits

Thanks to:

- myself (for wasting my time writing this, providing fixes, and creating this EFI)
- acidanthera (for developing an awesome bootloader)
- Rehabman (for providing patches for 8x70)
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for suggesting this idea due to lack of downloads outside of China :P)
- [HansHubertHass](https://github.com/HansHubertHass) and [Bautheile](https://github.com/Bautheile) (for their valuable testing)
- [Krutav](https://forums.macrumors.com/threads/2011-imac-graphics-card-upgrade.1596614/post-30941047) (for Dell vBIOS injection through OpenCore on the ROMless HP FirePro M4000)
- [onejay09](https://www.insanelymac.com/forum/topic/323010-please-help-me-fix-brightness-control-nvidia-graphics-only-laptop/) (for providing fixes for backlight control on NVIDIA cards)
- [ubihazard](https://github.com/ubihazard) (for [porting](https://github.com/ubihazard/OpenCorePkg-ProBook) RehabMan's ProBook 4x30s EFI modules to OpenCore)
- [5T33Z0](https://github.com/5T33Z0) (for enabling [XCPM](https://github.com/5T33Z0/OC-Little-Translated/tree/main/01_Adding_missing_Devices_and_enabling_Features/CPU_Power_Management/Enabling_XCPM_on_Ivy_Bridge_CPUs) on Catalina+)
