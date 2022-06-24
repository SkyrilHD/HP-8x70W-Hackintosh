# HP 8570W Hackintosh
 
### WICHTIG: Lest und führt den [Abschnitt](#Erstellung-einer-eigenen-Seriennummer) durch, bevor ihr euch an die Konfiguration wagt!

Diese Repository enthält eine OpenCore EFI-Konfiguration für das HP Elitebook 8570W und 8770W.

### Diese EFI ist nur mit Geräten mit dem TN-Display kompatibel. DreamColor-Displays werden nicht unterstützt!

### Unterstützte Konfigurationen:

NVIDIA: Quadro K1000M-K5000M / K1100M-K5100M ([Klickt hier](#monterey-nvidia-gpus))

AMD: FirePro M4000

Getestet auf:

| Specs | 8770W ([@HansHubertHass](https://twitter.com/MacGen2)) | 8570W ([@HansHubertHass](https://twitter.com/MacGen2)) | 8570W ([@Bautheile](https://github.com/Bautheile)) |
| -- | -- | -- | -- |
| CPU | Intel Core i5-3360M | Intel Core i7-3740QM | Intel Core i7-3840QM |
| GPU | AMD FirePro M4000 | Nvidia Quadro K1000M | Nvidia Quadro K1000M | 
| RAM | 8 GB 1600 MHz DDR3 | 8 GB 1600 MHz DDR3 | 32 GB 1600 MHz DDR3 |
| Screen | 1080p TN-Panel  | | |
| WiFi | Intel Centrino Advanced-N 6200 | Azureware AW-CE123H (BCM94352HMB) | BCM943224HMS |
| Bluetooth | HP BCM20702MD |
| macOS | Monterey | Catalina | Big Sur |

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
- SD-Kartenleser (sofern Firewire ausgeschaltet ist)
- FireWire (sofern SD-Kartenleser ausgeschaltet ist)
- Sleep
- TrackPoint
- Powermanagement

## Was funktioniert nicht?

- (NVIDIA) Helligskeitskontrolle
- (AMD) Fehlerhafte Helligkeitskontrolle
- DRM (Apple TV+ + Netflix & Prime unter Safari)
- unteren TouchPad-Tasten
- Fingerabdrucksensor
- Audio-Ausgang durch die Dockingstation

## BIOS-Einstellungen

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

## Installation

Neuste Konfiguration auf der [Releases-Seite](https://github.com/SkyrilHD/HP-8570W-Hackintosh/releases/) herunterladen und den EFI-Ordner auf die EFI Partition des Speichermediums kopieren, wo sich macOS befindet.

## macOS installieren

Es gibt zwei Wege zum Kreieren eines Installationsstick:

1. Bei einer vorhandenen macOS-Installation muss lediglich der Installer aus dem App Store heruntergeladen werden und mit dem Befehl `createinstallmedia` im Terminal auf einen USB Stick mit mind. 16 GB untergebracht werden:

    **Big Sur**: `sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

    **Monterey**: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. Windows-Nutzer müssen sich mit [macrecovery.py](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) vom OpenCore-Paket bedienen.

Nach der Erstellung muss der EFI-Ordner auf die EFI-Partition des Installationsmediums kopiert werden und schon kann die Installation von macOS durchgeführt werden. Nach der Installation muss die EFI-Partition der SSD/HDD, worauf macOS installiert wurde, eingehängt und der EFI-Ordner dort eingefügt werden.

## Generierung einer eigenen Seriennummer

Für's Generieren einer eigenen Seriennummer wird GenSMBIOS (https://github.com/corpnewt/GenSMBIOS) eingesetzt. Als SMBIOS wird MacBookPro9,1 oder 10,1 angegeben.

Für das Eintragen der im Video folgenden Daten eignet sich ein Plist-Bearbeitungsprogramm wie PlistEdit Pro oder ProperTree (SystemSerialNumber, MLB, and UUID)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

Die MAC-Adresse der internen Netzwerkkarte des Notebooks sollte ebenfalls angegeben werden.

## WLAN

Nutzer von Karten wie die HP BCM943224HMS oder eine basierend auf dem BCM94352HMB müssen nichts unternehmen, da sie von Haus aus funktionieren.

Nutzer von Intel-WLAN-Karten müssen AirportBrcmFixup.kext entfernen, es durch [Airportitlwm](https://github.com/OpenIntelWireless/itlwm/releases) ersetzen und ein Snapshot mit ProperTree durchführen, damit die Änderung in der Konfigurationsdatei (config.plist) übernommen wird und WLAN wieder funktioniert. Für mehr Funktionsumfang ist ein Austausch der WLAN-Karte jedoch anzuraten.

## Monterey (NVIDIA GPUs)

Um macOS Monterey mit einer NVIDIA GPU zum Laufen zu bringen, müssen folgendende Werte geändert werden:

1. Navigiert zu `Kernel > Misc > Security` and ändert `SecureBootModel` von `Enabled` zu `Disabled`.

2. Navigiert zu `NVRAM > 7C436110-AB2A-4BBB-A880-FE41995C9F82` and ändert bei `csr-active-config` den Wert von `00000000` zu `02080000`.

Nach der Monterey-Installation muss der Post-Install Volume Patch via [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) ausgeführt werden, um die NVIDIA Grafiktreiber wieder ins System zu bringen.
Warnung: Hierbei entfällt die Möglichkeit, Delta-OTA-Updates (ca. 2 GB groß) auszuführen und System Integrity Protection zu nutzen.
Der GPU-Patch muss nach jedem OTA-Update wiederholt werden.
