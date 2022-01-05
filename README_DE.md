# HP 8570W Hackintosh
 
### WICHTIG: Lest und führt den [Abschnitt](#Erstellung-einer-eigenen-Seriennummer) durch, bevor ihr euch an die Konfiguration wagt!

Diese Repository enthält eine OpenCore EFI-Konfiguration für das HP Elitebook 8570W.

### This EFI ist nur mit Geräten mit NVIDIA-GPU und TN-Display kompatibel. DreamColor-Displays werden nicht unterstützt!

Getestet auf:

| Specs | Laptop 1 ([@HansHubertHass](https://github.com/HansHubertHass)) | Laptop 2 ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- |
| CPU | Intel Core i7-3720QM | Intel Core i7-3840QM |
| GPU | Nvidia Quadro K1000M  | |
| RAM | 16 GB 1600 MHz DDR3  | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | |
| WiFi | Azureware AW-CE123H (BCM94352HMB) | BCM943224HMS (WiFi-only card) |
| Bluetooth | HP BCM20702MD |
| macOS | 10.15.7 (Catalina) | 11.5.2 (Big Sur) |
| BIOS | F.31 | F.61 |

## Was funktioniert?

- Audio
- Akkuanzeige
- Boot
- Broadcom WiFi (incl. Handoff + AirDrop)
- Dockingstation (USB + DVI-D)
- Ethernet
- ExpressCard
- GPU-Beschleunigung
- Tastatur + Trackpad (inkl. Magic TrackPad 2 emulation)
- SD-Kartenleser
- Sleep
- Sleep durchs Zuklappen des Notebooks
- TrackPoint
- (NVIDIA) Internes Display wird als internes angezeigt
- Powermanagement

## Was funktioniert nicht?

- Helligskeitskontrolle (wegen NVIDIA und fehlender iGPU)
- Audio durch die Dockingstation

## BIOS-Einstellungen

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

## Installation

Neuste Konfiguration auf der [Releases-Seite](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) herunterladen und den EFI-Ordner auf die EFI Partition des Speichermediums kopieren, wo sich macOS befindet.

## macOS Big Sur installieren

Es gibt zwei Wege zum Kreieren eines Installationsstick:

1. Bei einer vorhandenen macOS-Installation muss lediglich der Installer aus dem App Store heruntergeladen werden und mit dem Befehl `createinstallmedia` im Terminal auf einen USB Stick mit mind. 16 GB untergebracht werden: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. Windows-Nutzer müssen sich mit [macrecovery.py](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) vom OpenCore-Paket bedienen.

Nach der Erstellung muss der EFI-Ordner auf die EFI-Partition des Installationsmediums kopiert werden und schon kann die Installation von macOS durchgeführt werden. Nach der Installation muss die EFI-Partition der SSD/HDD, worauf macOS installiert wurde, eingehängt und der EFI-Ordner dort eingefügt werden.

## Erstellung einer eigenen Seriennummer

Für's Generieren einer eigenen Seriennummer wird GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) eingesetzt. Als SMBIOS wird MacBookPro9,1 angegeben.

Für das Eintragen der im Video folgenden Daten eignet sich ein Plist-Bearbeitungsprogramm wie PlistEdit Pro oder ProperTree (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

Die MAC-Adresse der internen Netzwerkkarte des Notebooks sollte ebenfalls angegeben werden.

## WLAN

Nutzer von Intel-WLAN-Karten müssen AirportBrcmFixup.kext entfernen, es durch [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) ersetzen und ein Snapshot mit ProperTree durchführen, damit die Änderung in der Konfigurationsdatei (config.plist) übernommen wird und WLAN wieder funktioniert.

## macOS Monterey

Um macOS Monterey einzurichten, muss folgender Wert geändert werden:

| Quirk | geänderter Wert | Zu finden unter |
| -- | -- | -- |
| csr-active-config | 030A0000 | NVRAM/7C436110-AB2A-4BBB-A880-FE41995C9F82 |

Nach der Monterey-Installation muss der Post-Install Volume Patch via [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) ausgeführt werden, um die NVIDIA Grafiktreiber wieder ins System zu bringen.
Warnung: Hierbei entfällt die Möglichkeit, Delta-OTA-Updates (ca. 2 GB groß) auszuführen. Stattdessen wird jedes noch so kleine Update immer 12 GB beanspruchen.
Der GPU-Patch muss nach jedem OTA-Update wiederholt werden.

