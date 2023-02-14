# Code Coverage with Simics

Once Simics is launched, enable Serial Console Logging in Simics:
```
board.serconsole.con.capture-start -overwrite output.txt
```
That will output the console window to a text file to parse later. Next, load the UEFI FW Tracker Module:

```
load-module uefi-fw-tracker
```
There may be an exisiting tracker module already loaded, so to double check you can run:
```
simics> board.software.info
Information about board.software [class os_awareness]
=====================================================

Software:
          Tracker : board.software.tracker
    Tracker class : uefi_fw_tracker_comp
             CPUs : board.mb.cpu0.core[0][0]
                    board.mb.cpu0.core[1][1]
                    board.mb.cpu0.core[1][0]
                    board.mb.cpu0.core[0][1]
```
The output will show if there is an existing tracker. In the example, there is the correct UEFI tracker, but by default it will be a linux tracker. To replace the linux tracker with the UEFI one, delete the existing tracker and enable the fw tracker:
```
board.software.delete-tracker
board.software.insert-tracker tracker = uefi_fw_tracker_comp
```

Load the tracker parameters:
```
board.software.tracker.load-parameters -load
```

Enable the FW Tracker:

```
board.software.enable-tracker
```

Now, start the collecting code coverage: 

```
$cc = (collect-coverage context-query = "'UEFI Firmware'")
```

To see all of the options for the code coverage in a convenient structure, run:
```
simics> board.software.node-tree
[name='UEFI System']─┬*[name='UEFI Firmware']─┬*[name='board.mb.cpu0.core[0][0]']
                     │                        ├*[name='board.mb.cpu0.core[1][1]']
                     │                        ├*[name='board.mb.cpu0.core[1][0]']
                     │                        └*[name='board.mb.cpu0.core[0][1]']
                     └─[OS]─┬─[name='board.mb.cpu0.core[0][0]']
                            ├─[name='board.mb.cpu0.core[1][1]']
                            ├─[name='board.mb.cpu0.core[1][0]']
                            └─[name='board.mb.cpu0.core[0][1]']
```
Since the code was build on a linux system and then executed on a Windows system, some of the dependencies will need to be modified, for example where all of the build files are. To add the mappting to the efi files to get the correct coverage information:
```
$cc.add.path-map <FROM> <TO>

$cc.add-path-map "/home/user/src/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5" "C:\\Users\\Connor Glosner\\Downloads\\Build\\SimicsOpenBoardPkg\\BoardX58Ich10\\DEBUG_GCC5"
```
I used the example above to show what I did for my case. Finally, we can stop the code coverage and create a viewable HTML report:
```
$cc.stop
$cc.html-report uefi-report
```