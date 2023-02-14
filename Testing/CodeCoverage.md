Enable Serial Console Logging in Simics:

```
board.serconsole.con.capture-start -overwrite output.txt
```

Load the UEFI FW Tracker Module:

```
load-module uefi-fw-tracker
load-module uefi_fw_tracker
load-module uefi_fw_tracker_comp
load-module uefi_fw_mapper
```

delete the existing tracker and enable the fw tracker:
```
board.software.delete-tracker
board.software.insert-tracker tracker = uefi_fw_tracker_comp
```

write the current settings to a parameter file:
```
$tracker_params = "uefi.params"
board.software.tracker.detect-parameters -overwrite param-file = $tracker_params
```

load parameters:
```
board.software.tracker.load-parameters -load
```

Enable the FW Tracker:

```
board.software.enable-tracker
```

To collect the coverage:

```
$cc = (collect-coverage context-query = "'UEFI Firmware'")
```

Add mapping to the efi files for the drivers:
```
$cc.add-path-map "C:\\uefi\\uefi-testfiles-build.git\\uefi-edk2-simics.git" "C:\\Users\\Connor Glosner\\Downloads\\Build\\SimicsOpenBoardPkg\\BoardX58Ich10\\DEBUG_GCC5"