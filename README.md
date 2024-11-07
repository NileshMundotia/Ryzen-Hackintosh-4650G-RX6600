# Ryzen-Hackintosh-4650G-RX6600

## EFI for Ryzen 5 4650G & RX 6600 Hackintosh (macOS 15.1)

### Overview

This repository contains the EFI folder used to successfully install and run macOS 15.1 on a Ryzen 5 4650G system with an RX 6600 GPU. The configuration is designed for use with OpenCore, and it includes necessary kernel patches, kexts, and settings optimized for this specific hardware configuration.

### System Specifications

- **CPU:** AMD Ryzen 5 4650G (Renoir)
- **GPU:** AMD RX 6600
- **Motherboard:** Gigabyte GA-A320M-S2H (rev. 1.x)
- **macOS Version:** 15.1

### What Works

- **GPU Acceleration:** Fully functional with RX 6600.
- **USB Ports:** Proper USB mapping applied.
- **Ethernet:** Working.
- **Monitor Audio Out:** Audio output via the monitor works well as a workaround for onboard audio issues.

### What Doesn't Work

- **Onboard Audio:** Onboard audio is not functional due to limitations with macOS and the motherboard's audio codec. A USB sound card can be used as a workaround for audio (not tested yet).

### EFI Folder Structure

- **ACPI:** ACPI: Includes SSDT-EC-USBX-DESKTOP and SSDT-CPUR for B550 and A520 motherboards.
- **Drivers:** Essential drivers for OpenCore to boot macOS on this hardware.
- **Kexts:** Includes the following key kexts:
  - `AMDRyzenCPUPowerManagement.kext` - Power management for AMD Ryzen CPUs.
  - `AppleALC.kext` - Enables macOS native audio for supported codecs (onboard audio not working in this setup).
  - `AppleMCEReporterDisabler.kext` - Disables MCE (Machine Check Exception) reporting to prevent kernel panics on AMD systems.
  - `BlueToolFixup.kext` - Fixes Bluetooth functionality issues for newer macOS versions.
  - `Lilu.kext` - Required dependency for other kexts like AppleALC, WhateverGreen, etc.
  - `RealtekRTL8111.kext` - Ethernet support for Realtek RTL8111 controller.
  - `RestrictEvents.kext` - Fixes macOS firmware issues and prevents checks.
  - `SMCAMDProcessor.kext` - Allows monitoring of AMD processor temperatures in macOS.
  - `SMCRadeonSensors.kext` - Enables macOS to read GPU temperature sensors for AMD Radeon cards.
  - `USBToolBox.kext` - For proper USB mapping and functionality.
  - `UTBMap.kext` - Custom USB mapping specific to your system configuration.
  - `VirtualSMC.kext` - Simulates Apple's System Management Controller (SMC) for Hackintosh compatibility.
  - `WhateverGreen.kext` - Ensures proper GPU support, including graphics fixes and acceleration for AMD GPUs.
- **config.plist:** Pre-configured with kernel patches, SMBIOS settings, and all necessary tweaks for this build. It uses MacPro7,1 SMBIOS for compatibility.

### Instructions

1. **BIOS Setup:**
   - Disable **CSM** (Compatibility Support Module).
   - Set **XHCI Hand-off** to Enabled.
   - Disable **Secure Boot** and **Fast Boot**.
   - Enable Above 4G Decoding (also known as 4G Decoding): If this setting is not present in BIOS, set `npci=0x2000` or `npci=0x3000` as boot arguments in your `config.plist` to bypass PCI configuration issues.

2. **Create macOS Bootable USB:**
   - Use macOS or a virtual machine to create a bootable macOS installer.
   - Download OpenCore and copy the contents of this EFI folder to the EFI partition of your USB drive.
   - Use tools like `GenSMBIOS` to generate a unique serial number and UUID for your system.

3. **Post-Installation:**
   - Copy the EFI folder from your bootable USB to the EFI partition of your macOS system disk after installation.

4. **Audio Workaround:**
   - Since onboard audio isn't working, connect your speakers/headphones to the audio-out port of your monitor or use a USB sound card as a workaround.

---

### AMD Kernel Patching Instructions for Ryzen CPUs

If you're using an AMD Ryzen CPU (such as the Ryzen 5 4650G), you'll need to apply specific kernel patches for macOS to recognize and function properly with your hardware. Follow these steps:

1. **Locate Kernel Patches:**
   - Open your `config.plist` and navigate to the `Kernel -> Patch` section.

2. **Remove Old Patches:**
   - Delete the existing `Kernel -> Patch` section in your `config.plist`.

3. **Add New Patches:**
   - Open the `patches.plist` file included in this (https://github.com/AMD-OSX/AMD_Vanilla.git) repository.
   - Copy the entire `Kernel -> Patch` section from `patches.plist` .
   - Paste this into the `config.plist` where the old patches were located.

4. **Modify Core Count:**
   - You will need to modify four patches, all named `algery - Force cpuid_cores_per_package`. 
   - Update the `Replace` value in each of these patches with your CPU’s core count in hexadecimal format.
     - For example, for an 8-Core CPU like the Ryzen 5 5800X, change the `Replace` values to:
       - `B8000000 0000` -> `B8 08 0000 0000`
       - `BA000000 0000` -> `BA 08 0000 0000`
       - `BA000000 0090` -> `BA 08 0000 0090`
       - `BA000000 00` -> `BA 08 0000 00`

5. **Finalize the Patch:**
   - Ensure all other settings are correctly configured for your system and that the patches are applied properly.

---

### Important Notes

- This EFI is specifically tailored for the AMD Ryzen 5 4650G and RX 6600. While it may work for other similar hardware configurations, it is recommended to make adjustments according to your system.
- Ensure you generate your own SMBIOS using `GenSMBIOS` and update the `config.plist` with your unique serial numbers before booting.
- Always back up your current EFI folder before making any modifications.

### Troubleshooting

- **Black Screen on Boot:** Ensure `WhateverGreen.kext` is loaded properly and your `config.plist` has the correct GPU settings.
- **No Audio Output:** If onboard audio doesn't work, use your monitor's audio out port as a workaround.
- **Stuck in Bootloop:** If stuck in a bootloop, try disabling Apple Secure Boot in `config.plist` during installation.
- **USB Issues:** If you experience problems with certain USB ports, try remapping them or modifying the SSDTs in the ACPI folder.

---

### Credits

- OpenCore Team for the bootloader.
- Kext developers for the necessary drivers.
- Hackintosh communities for guides and troubleshooting tips.
- [AMD-Vanilla GitHub Repository](https://github.com/AMD-OSX/AMD_Vanilla) for the latest AMD kernel patches and related resources.

---

Let me know if you'd like any more adjustments or further explanations!


