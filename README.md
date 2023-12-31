# DELL-T5810-OpenCore (Sonoma Support)
 DELL Precision T5810 OpenCore 0.9.6. 
 Support macOS Big Sur/Monterey/Ventura/Sonoma.

# About this EFI

OpenCore loader (0.9.6) for DELL workstations T5810 (reported to also work on T7810, possibly 7910). Support macOS Big Sur to Sonoma (14.1 tested). Sonoma update must run from the full installer.

**Supported Hardware**

- Precision T5810 (BIOS A33/A32)
- CPUs: E5-1600/2600 V3 & V4 Xeons (Hanswell/Broadwell)
- Required BIOS Settings: SATA Operation -> AHCI, Secure Boot Enable -> Disabled, VT for Direct I/O -> Disabled.
- [TIP]: If you encounter OC/macOS booting issue (like "OCB:StartImage failed..."), e.g. after a CPU change, toogle the BIOS setting [Memory Map I/O ABove 4G], and/or, toogling [Enable Legacy Option ROMs], could fix the problem. Either [YES] or [NO] would work. The trick is to force the BIOS to re-initialize for the new hardware.

**Installation:**

- Generate new serials unique for your system
- Choose correct CPU Emulation. Modify config.plist->Root->Kernel->Emulate->Cpuid1Data
	- For V3 Xeon's: C3060300 00000000 00000000 00000000 (<- current setting)
	- For V4 Xeon's: D4060300 00000000 00000000 00000000

#

**What Works:**

- Everything, excpt Sleep/Wake (should be disabled within the macOS, under System Settings).

#

**EFI Folder**

- OpenCore 0.9.6
- SYMBIOS: MacPro7,1 or iMacPro1,1

- ACPI folder:
	- SSDT-EC.aml - Fix Embedded Controller, via OC Guide
	- SSDT-HPET.aml - Fix HPET/IRQ conflict. Created with SDDTTimes, via OC Guide
	- SSDT-PLUG.aml - Enable CPU power management, via OC Guide
	- SDT-UNC.aml - Disable unused Uncore Bridges, via OC Guide
	- SSDT-RTC0-RANGE.aml - Fix RTC range, via OC Guide
	- SSDT-USBX.aml - Fix USB BUS Power properties, via OC Guide.
	- SSDT-SBUS-MCHC.aml - SBUS/MCHC fix, via OC Guide
	
- Kexts folder:
	- Lilu.kext
	- WhateverGreen.kext
	- VirtualSMC.kext
	- AppleALC.kext - On-board Audio (Layout ID 17. Front headphone, internal Spkr/LineOut-Rear)
	- IntelMausi.kext - Intel LAN port driver
	- USBMap_T5810.kext - Custom USB port maps for T5810	(added MacPro7,1 support)
	- NVMeFix.kext - NvMe SSD on PCI-E adapter
	- X99_InjectorUSB3.kext - USB3 ports injector	(cleaned up)

   	Removed (no longer needed):
	- CpuTscSync.kext - CPU TSC sync (fix TSC out of sync from wake/sleep). Note: Sleep/Wake is disabled.
	- CtlnaAHCIPort.kext - additinal SATA ports support.

- Driver folder:
	- ResetTSCAdjust.efi - Reset TSC sync at OC boot (required for Monterey/Ventura/Sonoma. DELL BIOS failed to do this.)

#

**Credits:**

- This EFI was optimized from two earlier builds by OreyM ([GitHub link](https://github.com/OreyM/Hackintosh-Dell-T5810-Xeon-E5-26xx-V3-OpenCore-PowerMac-G5)) and NOTNICE ([Github link](https://github.com/NOTNlCE/Dell-Precision-T5810-OpenCore)). 
- ResetTSCAdjust.efi tool by denskop ([Github link](https://github.com/denskop/VoodooTSCSync/issues/1#issuecomment-629837192)). This little tool allowed CPU TSC sync to be performed before macOS launch (DELL BIOS failed to do this). Without this, Monterey/Ventura booting would encounter kernel panic due to "non-monotonic time" error.

