# QEMU
You would build the code with:
* 32-bit emulator in Windows:
```
build -p EmulatorPkg\EmulatorPkg.dsc -t VS2017 -a IA32
```
* 64-bit emulator in Windows:
```
build -p EmulatorPkg\EmulatorPkg.dsc -t VS2017 -a X64
```
* 32-bit emulator in Linux:
```
build -p EmulatorPkg\EmulatorPkg.dsc -t GCC5 -a IA32
```
* 64-bit emulator in Linux:
```
build -p EmulatorPkg\EmulatorPkg.dsc -t GCC5 -a X64
```

Then you have to get into the correct folder and run it:

* 32-bit emulator in Windows:
```
cd Build\EmulatorIA32\DEBUG_VS2017\IA32\ && WinHost.exe
```
* 64-bit emulator in Windows:
```
cd Build\EmulatorX64\DEBUG_VS2017\X64\ && WinHost.exe
```
* 32-bit emulator in Linux:
```
cd Build/EmulatorIA32/DEBUG_GCC5/IA32/ && ./Host
```
* 64-bit emulator in Linux:
```
cd Build/EmulatorX64/DEBUG_GCC5/X64/ && ./Host
```