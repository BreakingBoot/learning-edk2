# To Run Simics

## Setting up Simics
Download the Simics Package Manager and the Packages from Intels website:
```
https://lemcenter.intel.com/productDownload/productDownload
```
1. Install the Package Manager by following the prompts.
2. Now run the Simics Package Manager and follow the steps:
a. The first time you'll have to choose an install location.
b. If you don't go the screen automatically, go to the Install Repository screen to select the Bundle to use.
c. Select Browse for Bundle, and locate the "simics-6-packages-" file that ends with .ispm that you downloaded from the link above.
d. Finish the install.

You can now launch the demo for getting familiar with the software and testing various components by enable and disabling them. The easiest way to launch the demo is to select Launch Demo that is on the screen under Scripts if you are currently on the Demo Project 1.

## Running Simics with Custom UEFI Firmware

Now to Run you own custom UEFI Firmware with it:

Create a source directory, similar to running QEMU with OVMF:
```
mkdir ~/src
cd ~/src
```
Now pull the necessary code to use it:
```
git clone https://github.com/BreakingBoot/edk2.git
git clone https://github.com/tianocore/edk2-platforms.git
git clone https://github.com/tianocore/edk2-non-osi.git
git clone https://github.com/Intel/FSP.git
```
Note: The `edk2` repo has all of the main code that that is "mostly" platform independent, while `edk2-platforms` holds platform specific code (e.g. specific logos for the different professors or assembly code) and `edk2-non-osi` holds additional features that you have created and want to include (also platform specific).

Make sure you go into each repo and run:
```
cd ~/src/edk2 && git submodule update --init --recursive
cd ~/src/edk2-platforms && git submodule update --init --recursive
cd ~/src/edk2-non-osi && git submodule update --init --recursive
cd ~/src/FSP && git submodule update --init --recursive
```
Now that you have all of the necessary code, check for the the boards that are supported in the `edk2-platforms` repo, we are specifically looking for `BoardX58Ich10`:
```
cd ~/src/edk2-platforms/Platforms/Intel/
python build_bios.py -p BoardX58Ich10 -t GCC5
```
That is the same as the build script that is run in normal compilation, execpt it takes into account platform specifics. That should complete successfully and create a `.fd` file that you will need to move in order to run Simics with your modified firmware.


To do this, copy the file `BOARDX58ICH10.fd` into the proper simics directory, for me, I installed Simics into `~/Simics`, so:
```
cp ~/src/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd ~/Simics/simics-qsp-x86-6.0.57/targets/qsp-x86/images/
```
The numbers may be slightly different for the `simics-qsp-x86-#####` due to the version you are using.

Now modify the Simics include file to use your modified image rather than the default one, using the directory information I've been using:

```
~/Simics/simics-qsp-x86-6.0.57/targets/qsp-x86/qsp-uefi.include
Modify:
SIMICSX58IA32X64_1_0_0_bp_r.fd ---> BOARDX58ICH10.fd
```
Now in Simics Package Manager, click on the `>_` icon on the side and execute:
```
./simics targets/qsp-x86-qsp-modern-core.simics
```
A couple of popup windows will now appear, which will allow you to "start" the simulation.

To mount an external file system you need to do a couple of things, first is create the virtual file system:
```
sudo dd = if=/dev/zero of=VHD.vhd bs=1M count=1200
sudo mkfs -t fat VHD.vhd
```
Next is modify Simics to use the file system. To do so, copy `VHD.vhd` to the `targets/qsp-x86/images` directory in your project and then add the following code to `qsp-modern-core.simics` in the `simics-qsp-cpu-6.0.12/targets/qsp-x86` directory:
```
$disk1_image="%simics%/targets/qsp-x86/images/VHD.vhd"
```
