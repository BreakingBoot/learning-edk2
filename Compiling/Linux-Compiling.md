The first thing to do is make sure you have some of the necessary tools to compile it:
```
sudo apt install acpica-tools gcc-aarch64-linux-gnu python3-distutils uuid-dev nasm
```

Now, create the source directory where you want to store it:
```
mkdir ~/src
cd ~/src
```
Then clone the repository:
```
git clone https://github.com/BreakingBoot/edk2.git
cd edk2
git submodule update --init --recursive
```
Now compile the `BaseTools`, which are responsible for building the UEFI image that would end up on your SPI Flash:
```
make -C BaseTools
```
Now run the script they have to setup all of the environment variables that will be used:
```
. edksetup.sh
```
Note: You can run that part manually if you wanted it to.

Next, you will need to choose the platform you will running this on. There are two ways to do that:
1. By specifying all of that information on the build line.
```
build -a X64 -t GCC5 -p MdeModulePkg/MdeModulePkg.dsc 
```
where `-a` specifies the architecture you are building for, `-t` is the compiler to use, and `-p` is the active platform to build.
2. Inside the `Conf/target.txt` file
```
vi Conf/target.txt
```
and then find and change the lines to the following:
```
ACTIVE_PLATFORM       = MdeModulePkg/MdeModulePkg.dsc

TOOL_CHAIN_TAG        = GCC5

TARGET_ARCH           = X64
```
Now all you have to do is build:
```
build
```

If you want to test with SMM you need to add a special compiling flag, `-D SMM_REQUIRE`, for example:
```
build -a X64 -t GCC5 -D SMM_REQUIRE
```

If you get the error:
```
No such file or directory
   27 | #include <bits/libc-header-start.h>
```
Run:
```
sudo apt-get install gcc-multilib
sudo apt-get install libx11-dev:i386 libx11-dev libxext-dev libxxf86vm-dev
```

