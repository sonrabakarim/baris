---
title: FileVault and BitLocker on a Mac with Boot Camp
---

In the past, setting up both FileVault and BitLocker encryption on a Mac with Boot Camp required configuring the disk partitions in a specific way to work around limitations in the MBR (Master Boot Record) partition scheme. This now works by default with Boot Camp Assistant provided you are using the following:
- a Mac that supports booting Windows in EFI mode (all [Mac computers that support Windows 10](https://support.apple.com/en-au/HT204990#models))
- Boot Camp Assistant 6 or later (included in OS X El Capitan or later and OS X Yosemite via update)
- Windows 8 or later

This is due to Boot Camp Assistant 6 using a different method to create the Boot Camp partition to support EFI booting for Windows 8 or later. The best explanation I have found is in the following article:

- [How El Capitan Boot Camp is Affected by Apple’s New System Integrity Protection (SIP)](https://twocanoes.com/how-el-capitan-boot-camp-is-affected-by-apples/)

> Modern Macs always boot via EFI, but Windows hardware has only recently started natively booting EFI. While there was some support for EFI booting Windows 7, Apple didn’t support EFI booting Windows until Windows 8. With the newest Apple hardware, Windows 8 or later is required, and EFI booting is the only way that Windows will boot on the Mac.

> Usually you don’t have to worry about any of this, since Boot Camp Assistant and the Windows installer will set everything up correctly.

> If you use Boot Camp Assistant to create the Boot Camp partition, you’ll get a standard EFI “guard” MBR

> The hybrid MBR has an entry for each of the first 4 partitions. The guard MBR has only a single entry that covers the entire disk

The key that allows having both FileVault and BitLocker is Boot Camp Assistant creating a "guard" MBR with only a single entry.

## Why this didn't work previously

Older versions of Boot Camp Assistant create a [hybrid MBR](https://en.wikipedia.org/wiki/GUID_Partition_Table#Hybrid_MBR_.28LBA_0_.2B_GPT.29) to support running Windows 7 and earlier in legacy BIOS mode. However, the MBR partition scheme has a limit of four primary partitions, and the hybrid MBR set up by Boot Camp Assistant uses all four.

BitLocker requires a second partition, so a hybrid MBR set up by Boot Camp Assistant has no spare partitions available for BitLocker.

> Two partitions are required to run BitLocker because pre-startup authentication and system integrity verification must occur on a separate partition from the encrypted operating system drive. This configuration helps protect the operating system and the information in the encrypted drive.
> <cite><a href="https://docs.microsoft.com/en-us/windows/device-security/bitlocker/bitlocker-frequently-asked-questions">BitLocker frequently asked questions (FAQ)</a></cite>

Previous solutions worked around this by setting up the MBR manually.

**Note:** The [Use Windows 10 on your Mac with Boot Camp](https://support.apple.com/en-au/HT204990) guide at Apple Support still states:

> Microsoft BitLocker is not compatible with Boot Camp volumes.

This is no longer correct.

## Macs that support booting Windows in EFI mode

The Boot Camp Assistant configuration file (`/Applications/Utilities/Boot Camp Assistant.app/Contents/Info.plist`) provides an indication as to which Mac models are supported:

```xml
<key>PreUEFIModels</key>
<array>
        <string>MacBook7</string>
        <string>MacBookAir5</string>
        <string>MacBookPro10</string>
        <string>MacPro5</string>
        <string>Macmini6</string>
        <string>iMac13</string>
</array>
```

According to this, Macs with a [model identifier](https://support.apple.com/en-au/HT201300) higher than those listed above will be set up to boot Windows in EFI mode.

This matches Apple's official list of [Mac computers that support Windows 10](https://support.apple.com/en-au/HT204990#models).

**Note:** Apple's list of [Mac models you can use with Windows 8.1](https://support.apple.com/en-au/HT201457#models) includes older models. Presumably these will be set up in legacy mode.

## Configuring BitLocker on a Mac

BitLocker encryption normally requires a computer with a Trusted Platform Module (TPM). Macs don't have a TPM, so the only other requirement is to allow BitLocker to work without one:

- [How to Use BitLocker Without a Trusted Platform Module (TPM)](https://www.howtogeek.com/howto/6229/how-to-use-bitlocker-on-drives-without-tpm/)

## Summary

On recent Macs, the combination of Boot Camp Assistant and allowing BitLocker to work without a TPM is all that is required to have both FileVault encrypted macOS/OS X and BitLocker encrypted Windows.
