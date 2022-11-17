# Testing Platforms

There are two main UEFI virtual platforms that are used: Simics and QEMU. You may have heard of Boch as well, but it is outdated as Simics and QEMU have much better capabilities.

## QEMU
It is a general open-source machine emulator and virtualizer that runs on OVMF, which is a project to enable UEFI support on virtual machines. The [QEMU](https://www.qemu.org/docs/master/) website documentation page goes into all of the details about the capabilities of the code. QEMU is an open-source firmware image that is currently in the `edk2` repository and the OVMF software is an external library that would need to be included on you computer.

## Simics
Simics was developed by Intel to test their UEFI code and has the capability to run early hardware initialization simulations during the Pre-EFI Initialization (PEI) phase. Simics is very similar to QEMU, but it allows the functionality to only run particular phases or to simulate hardware interactions (e.g. plugging in a USB device or disable different buses). It is possible to modify Simics code to add or disable different hardware features to simulate different execution environments.