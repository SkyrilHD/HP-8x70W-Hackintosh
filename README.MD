# HP 8570W Hackintosh

This repo includes an OpenCore EFI for macOS Catalina on 8570W.


## Disclaimer: This is still WIP, so don't use this until you know what you are doing!

### Works only with TN-Panels. (No support for DreamColor display)


Tested on:

- Model: HP Elitebook 8570W
- CPU: Intel Core i7-3740QM
- GPU: NVIDIA Quadro K1000M
- RAM: 16GB DDR3 1600Mhz
- Storage: HP SSD 8700 250GB
- WiFi & Bluetooth: Intel Centrino Advanced-N 6205 (doesn't support)
- Software: macOS 10.15.4 Catalina

# What works?

Booting into macOS Catalina

# Download and Install

Copy the EFI folder to your EFI partition... That's it.

# What to expect

- Works ok...
- no brightness control
- no audio
- trackpad and keyboard support

# How to Install macOS Catalina

First of all, you need an already working Mac with supported Specs (check Apple's website). Download the latest version of Catalina from the App Store and use this command:

sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

After it is done copy the EFI folder to the EFI partition and install as usual. After it is installed mount the EFI of the installed OS and copy the EFI folder to its partition.

If you want an OOTB-WiFi:
You need to change your WiFi card to get your internet working. Buy it on eBay or on your trustworthy website and find a suitable card with the kexts.

# Credits

Thanks to:

- me (for wasting my time to write this, provide fixes and creating this EFI)
- Harvé (for suggesting to inject property in Clover)
- Rehabman (for fixing the graphics, battery information and keyboard issues)
- @FernsehSackDE (for the idea to do this because there were no downloads outside China :P)
- @MacGen2 (for testing this on his 8570W)
