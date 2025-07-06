# DELL-T5810-OpenCore (Sequoia Support)
**Update** (7/4/2025) - Updated to OC 1.0.4. Tested with Sequoia 15.5.

# About this EFI

OpenCore loader (1.0.4) for DELL workstations T5810 (reported to also work on T7810, possibly 7910). Support macOS Big Sur to Sequoia (15.5 tested). Sonoma update must run from the full installer.

**Supported Hardware**

- Precision T5810 (BIOS A34/A33/A32)
- CPUs: E5-1600/2600 V3 & V4 Xeons (Hanswell/Broadwell)
- Required BIOS Settings: SATA Operation -> AHCI, Secure Boot Enable -> Disabled, VT for Direct I/O -> Disabled.
- **[TIP]**: If you encounter OC/macOS booting issue (like "OCB:StartImage failed..."), e.g. after a CPU change, toggle the BIOS setting [Memory Map I/O ABove 4G], and/or, toggling [Enable Legacy Option ROMs], could fix the problem. Either [YES] or [NO] would work. The trick is to force the BIOS to re-initialize for the new hardware.

**Installation:**

- Generate new serials unique for your system
- Choose correct CPU Emulation. Modify config.plist->Root->Kernel->Emulate->Cpuid1Data
	- For V3 Xeon's: C3060300 00000000 00000000 00000000 (<- current setting)
	- For V4 Xeon's: D4060300 00000000 00000000 00000000
- Sonoma/Sequoia install/update: 
	- Modify config.plist->Misc->Security->SecureBootModel-> Disabled
	- After installation, set the value back to "default".

#

**What Works:**

- Everything, excpt Sleep/Wake (should be disabled within the macOS, under System Settings).

#

**Other (optional):**

- Unlock CFG LOCK in BIOS (Tested on A32-A34 BIOS only):
  	- Add modGRUBShell.efi [link](https://github.com/datasone/grub-mod-setup_var) to OC/Tools folder, and add an entry to config.plist->Root->Misc->Tools
  	- At the OC picker, select "modGRUBShell" and hit Return. At the promt grub> , enturn the following command (WARNING: you MUST type in the EXACT words, or you could brick your BIOS!):
  		- **setup_var_cv IntelSetup 0x72 0x1 0x0**
  	- If successful (use VerifyMsrE2.efi to check), set config.plist->Root->Kernel->Quirks->AppleXcpmCfgLock-> False.
#

**EFI Folder**

- OpenCore 1.0.4
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

