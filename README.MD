# HP 8570W Hackintosh

This repo includes an OpenCore EFI for macOS Catalina on 8570W.


### Works only with TN-Panels. (No support for DreamColor display)


Tested on:

- Model: HP Elitebook 8570W
- CPU: Intel Core i7-3740QM
- GPU: NVIDIA Quadro K1000M
- RAM: 16GB DDR3 1600Mhz
- Storage: HP SSD 8700 250GB
- WiFi & Bluetooth: Azureware AW-CE123H (BCM94352HMB)
- Software: macOS 10.15.5 Catalina

## What works?

- Boot
- GPU acceleration
- Ethernet
- TrackPad (partially)
- WiFi (incl. Handoff + AirDrop)
- Audio

## What doesn't work?

- battery readout
- Since the 8570W does not have an iGPU and this laptop only has a K1000M, there is no brightness control.
- TrackPad isn´t being recognised as a TrackPad + no Magic TrackPad 2 Emulation (scrolling works)

## Download and Install

Copy the EFI folder to your EFI partition... That's it.

## How to Install macOS Catalina

First of all, you need an already working Mac with supported Specs (check Apple's website). Download the latest version of Catalina from the App Store and use this command:

sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

After it is done copy the EFI folder to the EFI partition and install as usual. After it is installed mount the EFI of the installed OS and copy the EFI folder to its partition.

If you have the original Intel Centrino Wireless-N 6205 card:
You need remove the broadcom kexts and replace them with Airportitlwm (use 1.2.0 debug) and IntelBluetoothFirmware to get both WiFi and Bluetooth working.

## Credits

Thanks to:

- me (for wasting my time to write this, provide fixes and creating this EFI)
- acidanthera (for making an awesome bootloader)
- Rehabman (for fixing keyboard issues and providing patches for 8x70)
- [sha106b](https://github.com/sha106b) (for the idea to do this because there were no downloads outside China :P)
- [HansHubertHass](https://github.com/HansHubertHass) (for testing this on his 8570W)
