# DELL-T5810-OpenCore (Monterey/Ventura Support)
 DELL T5810 OpenCore 0.8.4
 Support macOS Big Sur/Monterey/Ventura.

# About this EFI

OpenCore loader (0.8.4) for DELL workstations T5810. Support macOS Big Sur to Ventura.

Adding ResetTSCAdjust.efi driver to perform TSC Reset, required to solve the kernel panic during Monterey/Ventura booting.

**Supported Hardware**

- T5810 (BIOS A33)
- CPUs: E5-1600/2600 V3 & V4 Xeons (Hanswell/Broadwell)
- Required BIOS Settings: SATA Operation -> AHCI, Secure Boot Enable -> Disabled, VT for Direct I/O -> Disabled.

**Installation:**

- Generate new serials unique for your system
- Choose correct CPU Emulation. Modify config.plist->Root->Kernel->Emulate->Cpuid1Data
	- For V3 Xeon's: C3060300 00000000 00000000 00000000 (<- current setting)
	- For V4 Xeon's: D4060300 00000000 00000000 00000000

#

**What Works:**

- Everything, including Sleep/Wake.

#

**EFI Folder**

- OpenCore 0.8.4
- SYMBIOS: iMacPro1,1

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
	- AppleALC.kext - On-board Audio (Layout ID 11)
	- CpuTscSync.kext - CPU TSC sync (fix TSC out of sync from wake/sleep)
	- IntelMausi.kext - Intel LAN port driver
	- USBMap.kext - Custom USB port maps for T5810
	- NVMeFix.kext - NvMe SSD on PCI-E adapter
	- CtlnaAHCIPort.kext - additinal SATA ports support
	- X99_InjectorUSB3.kext - USB3 ports injector

- Driver folder:
	- ResetTSCAdjust.efi - Reset TSC sync at OC boot (required for Monterey/Ventura. DELL BIOS failed to do this.)

#

**Credits:**

- This EFI was optimized from two earlier builds by OreyM ([GitHub link](https://github.com/OreyM/Hackintosh-Dell-T5810-Xeon-E5-26xx-V3-OpenCore-PowerMac-G5)) and NOTNICE ([Github link](https://github.com/NOTNlCE/Dell-Precision-T5810-OpenCore)). 
- ResetTSCAdjust.efi tool by denskop ([Github link](https://github.com/denskop/VoodooTSCSync/issues/1#issuecomment-629837192)). This little tool allowed CPU TSC sync to be performed before macOS luanch (DELL BIOS failed to do this). Without this, Monterey/Ventura booting would encounter kernel panic due to "non-monotonic time" error.

