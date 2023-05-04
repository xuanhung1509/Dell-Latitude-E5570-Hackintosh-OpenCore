# Dell Latitude E5570 Hackintosh Opencore

### Table of contents

- [Hardware Specs](#hardware-specs)
- [What's working](#whats-working)
- [What's not working](#whats-not-working)
- [EFI Folder Structure](#efi-folder-structure)
- [BIOS Settings](#bios-settings)
- [Additional Notes](#additional-notes)
- [Install](#install)
- [Post-Install](#post-install)
- [References](#references)

## Hardware Specs

| Model              | Dell Latitude E5570                                                                                                                                                                                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU                | Intel Core i7-6820HQ                                                                                                                                                                                 |
| GPU                | Intel HD Graphics 530/ AMD Radeon R7 M370                                                                                                                                                            |
| RAM                | 2 x 8GB DDR4 2133MHz                                                                                                                                                                                 |
| Hard Disk          | LITEON CV5-8Q256 256GB                                                                                                                                                                               |
| Display            | 15.6" IPS FHD                                                                                                                                                                                        |
| Audio              | Realtek ALC293 (ALC3235)                                                                                                                                                                             |
| Ethernet           | Intel I219-LM Gigabit Ethernet                                                                                                                                                                       |
| Wifi and Bluetooth | Intel Wireless-AC 8260 Dual Band                                                                                                                                                                     |
| Touchpad           | AlpsPS/2 ALPS DualPoint                                                                                                                                                                              |
| Ports              | <ul><li>1 x SD Card Reader (Realtek RTS525A)</li><li>1 x HDMI 1.4</li><li>1 x VGA</li><li>2 x USB 3.0 and 1 x USB 3.0 with PowerShare</li><li>1 x RJ-45</li><li>1 x headset/mic combo jack</li></ul> |

[Dell Latitude E5570 Owner's Manual (PDF)](https://dl.dell.com/topicspdf/latitude-e5570-laptop_owners-manual_en-us.pdf)

## What's working

- Full Intel HD 530 Graphic Acceleration
- Internal speaker/ Headphone/ Autoswitch
- Trackpoint/ Keyboard/ Touchpad with up to 4 gestures
- Wifi/Bluetooth
- Brightness control with Fn + F11 / F12
- HDMI (You might need to put your laptop to sleep before using it)
- Internal USB Ports
- HD Webcam
- Sleep/Wake with Fn + Insert, Power button and Lid close/open
- SD Card Reader

## What's not working

- AMD R7 M370 (disabled)
- Combo headphone jack's microphone
- VGA port might not work. See [here](https://osxlatitude.com/forums/topic/18160-dell-latitude-e5570-how-to-get-vga-and-bluetooth-working-under-monterey/?do=findComment&comment=118362).

## Known Issues

- Sometimes, the trackpad cursor arbitrarily jumps to a corner of the screen. This seems to relate to the trackpad driver.
- On some rare occasions, the trackpad and internal keyboard not working after waking up from sleep, while external mouse and keyboard work just fine. After unlocking, I put the cursor into a text field and I see that the character `a` is printed forever. If this is your case, the temporary solution is to force shutdown the machine by pressing and holding the power button for about 5 secs.

## EFI Folder Structure

<details>
<summary><strong>Click to reveal</strong></summary>

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-BRT6.aml
    │   ├── SSDT-dGPU-Off.aml
    │   ├── SSDT-EC-USBX.aml
    │   ├── SSDT-GPRW.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-PNLF.aml
    │   ├── SSDT-Swap-CommandOption.aml
    │   └── SSDT-XOSI.aml
    ├── config.plist
    ├── Drivers
    │   ├── HfsPlus.efi
    │   ├── OpenRuntime.efi
    │   └── ResetNvramEntry.efi
    ├── Kexts
    │   ├── AirportItlwm.kext
    │   ├── AppleALC.kext
    │   ├── BlueToolFixup.kext
    │   ├── BrightnessKeys.kext
    │   ├── ECEnabler.kext
    │   ├── IntelBluetoothFirmware.kext
    │   ├── IntelBTPatcher.kext
    │   ├── IntelMausi.kext
    │   ├── Lilu.kext
    │   ├── Sinetek-rtsx.kext
    │   ├── SMCBatteryManager.kext
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── USBToolBox.kext
    │   ├── UTBMap.kext
    │   ├── VirtualSMC.kext
    │   ├── VoodooPS2Controller.kext
    │   └── WhateverGreen.kext
    ├── OpenCore.efi
    └── Tools
        └── OpenShell.efi
```

</details>

## BIOS Settings

<details>
<summary><strong>Click to reveal</strong></summary>

BIOS version 1.34.3

- System Configuration

  - Parallel Port: `disabled`
  - Serial Port: `disabled`
  - SATA Operationi: `AHCI`

- Security

  - TPM 1.2 Security: `TPM On: disabled`

- Secure Boot

  - Secure Boot Enable: `disabled`

- Intel Software Guard Extensions

  - Intel SGX Enable: `disabled`

- Performance

  - Multi Core Support: `all`
  - Intel SpeedStep: `enabled`
  - C-State Control: `enabled`
  - Intel Turbo Boost: `enabled`
  - HyperThread Control: `enabled`

- POST Behavior

  - Fastboot: `Minimal`

- Virtualization Support

  - Virtualization: `Intel Virtualization Technology: enabled`
  - VT for Direct I/O: `enabled` (optional)

</details>

## Additional Notes

- Command and Option keys are remapped by default.
- Before proceeding to installation, you need to [generate SMBIOS](https://github.com/corpnewt/GenSMBIOS) for `MacbookPro13,2`
- CFG-Lock must be disabled. Follow [this guide](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html) to disable CFG-Lock in the BIOS, or enable `AppleXcpmCfgLock` under Kernel > Quirks. Otherwise, your hack won't boot.

> For BIOS version [1.34.3](https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=x2r2c&oscode=w764&productcode=latitude-e5570-laptop), the `offset` is _0x109_ and the `Name` is _Setup_

```bash
setup_var_cv Setup 0x109 0x01 0x00
```

- You might need to [map your USB](https://github.com/USBToolBox/tool) if the provided one doesn't work for you.

This EFI is meant specifically for `Monterey`. If you want to install other versions, you might need to adjust a few things:

- Update `MinDate` and `MinVersion` under `UEFI` > `APFS` if you are booting macOS Catalina or earlier. See [here](https://github.com/5T33Z0/OC-Little-Translated/tree/main/A_Config_Tips_and_Tricks#opencore-troubleshooting-quick-tips).
- Update `SecureBootModel` under `Misc` > `Security`. See [here](https://dortania.github.io/OpenCore-Install-Guide/config.plist/security.html).
- Replace `AirportItlwm` and [`IntelBluetoothFirmware`](https://openintelwireless.github.io/IntelBluetoothFirmware/Installation.html) with the ones for your macOS version.
- Also see [here](https://dortania.github.io/OpenCore-Post-Install/universal/update.html#updating-macos) what's changed in macOS versions.

## Install

Follow the Dortania Guide for [USB creation](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) and [installation process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html#double-checking-your-work).

> You might want to perform a **NVRAM Reset** if you're updating the EFI folder.

## Post-install

- [Boot without USB](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html)
- If you encouter sleep issues, paste these commands into `Terminal`

```bash
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

## References

- The main guide: [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- GPU patching: [WhateverGreen - Intel® HD Graphics FAQs](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md)
- ACPI patch for brightness keys: [OSX Latitude](https://osxlatitude.com/forums/topic/15661-acpi-patch-for-brightness-keys-on-dell-laptops/)
- Supplementary resources: [OC Little Translated](https://github.com/5T33Z0/OC-Little-Translated)
- Various hackintoshes from different models, to name a few:
  - [Lenovo-T530-Hackintosh-OpenCore](https://github.com/5T33Z0/Lenovo-T530-Hackintosh-OpenCore) by `5T33Z0`
  - [dell-inspiron-5370-hackintosh](https://github.com/dreamwhite/dell-inspiron-5370-hackintosh) by `dreamwhite`
  - [Dell-Exx50-Hackintosh](https://github.com/SkyrilHD/Dell-Exx50-Hackintosh) by `SkyrilHD`
