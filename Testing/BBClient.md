# BreakingBoot Fuzzing Application Called BBClient
This document will layout how to compile, execute, and limitations of the current application. The code was added ontop of the current `edk2` code in our own repo. The current code under development can be found [here](https://github.com/BreakingBoot/edk2/tree/breakingboot).

## About the Application
The application currently has 4 services it can fuzz:
1. `CoreProcessFirmwareVolume`
2. `CoreCloseEvent`
3. `CoreCreateEvent`
4. `CoreLoadImage`
The exact code that was added can be found in the (`ShellPkg/Application/BBClient`)[https://github.com/BreakingBoot/edk2/tree/breakingboot/ShellPkg/Application/BBClient] folder. Note: `CoreCloseEvent` and `CoreCreateEvent` are fuzzed at the same time.

## Compiling
This application was compiled following the instructions in the [Linux-Compilation](https://github.com/BreakingBoot/learning-edk2/blob/main/Compiling/Linux-Compiling.md) document.

## Executing 
Once the code is compiled, follow the [execution guide](https://github.com/BreakingBoot/learning-edk2/blob/main/Executing/QEMU-Emulation.md) to test the code, it can be tested using Simics as well. After you execute the emulator, use the following command to execute it:
```
BBClient.efi <Service to fuzz><Random input data>
```
That is the general form for executing the fuzzer, where `<Service to fuzz>` is the alphanumeric character representing the service that you want to fuzz (see below to see the mapping) and `<Random input data>` is the just random data to use.

An example for fuzzing the `CoreProcessFirmwareVolume` is:
```
BBClient.efi 0123456789012345678901234567890
```

If insufficient input data has given then a debug message is sent to the debug console.

## Service Mapping
0. `CoreProcessFirmwareVolume`
1. `CoreCloseEvent` and `CoreCreateEvent`
2. `CoreLoadImage`

## Limitations
The current limitations for the BBClient application are:
* It only has a limited number of services that can be fuzzed.
