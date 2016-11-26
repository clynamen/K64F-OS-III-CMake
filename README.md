# K64F µC/OS-III Cmake Template

This is a template CMake project with µC/OS-III for the [NXP FRDM-K64F](www.nxp.com/frdm-k64) board.

It can be imported in a CMake compatible IDE such as KDevelop, QtCreator or built manually

## Manual build

in the main directory:

```bash
mdkir build
cd build
cmake ..
make
```

## Flash the board

Flashing can be done with JLinkExe. It is also possible to execute the **flash** target for flashing the 
device (with default jtag settings):

```bash
# in the build directory:
make flash
```

JLinkExe output:

```bash
-- Configuring done
-- Generating done
-- Build files have been written to: /home/clynamen/documents/uni/uni/realtime_operating_systems/Git/build
[100%] Built target k64f
SEGGER J-Link Commander V6.12 (Compiled Nov 25 2016 18:03:02)
DLL version V6.12, compiled Nov 25 2016 18:02:55


Script file read successfully.
Processing script file...

J-Link connection not established yet but required for command.
Connecting to J-Link via USB...O.K.
Firmware: J-Link OpenSDA 2 compiled Nov 16 2016 09:43:40
Hardware version: V1.00
S/N: 621000000
VTref = 3.300V
Target connection not established yet but required for command.
Device "MK64FN1M0XXX12" selected.


Selected interface (0) is not supported by connected debug probe.
Found SWD-DP with ID 0x2BA01477
Found SWD-DP with ID 0x2BA01477
AP-IDR: 0x24770011, Type: AHB-AP
Found Cortex-M4 r0p1, Little endian.
FPUnit: 6 code (BP) slots and 2 literal slots
CoreSight components:
ROMTbl 0 @ E00FF000
ROMTbl 0 [0]: FFF0F000, CID: B105E00D, PID: 000BB00C SCS
ROMTbl 0 [1]: FFF02000, CID: B105E00D, PID: 003BB002 DWT
ROMTbl 0 [2]: FFF03000, CID: B105E00D, PID: 002BB003 FPB
ROMTbl 0 [3]: FFF01000, CID: B105E00D, PID: 003BB001 ITM
ROMTbl 0 [4]: FFF41000, CID: B105900D, PID: 000BB9A1 TPIU
ROMTbl 0 [5]: FFF42000, CID: B105900D, PID: 000BB925 ETM
ROMTbl 0 [6]: FFF43000, CID: B105900D, PID: 003BB907 ETB
ROMTbl 0 [7]: FFF44000, CID: B105900D, PID: 001BB908 CSTF
Halting CPU for downloading file.
Downloading file [k64f.bin]...
Comparing flash   [100%] Done.
Verifying flash   [100%] Done.
J-Link: Flash download: Flash download skipped. Flash contents already match
O.K.

Reset delay: 0 ms
Reset type NORMAL: Resets core & peripherals via SYSRESETREQ & VECTRESET bit.


Script processing completed.

[100%] Built target flash
```


The target executes the following command:

```bash
JLinkExe -AutoConnect 1 -Device MK64FN1M0XXX12 -If JTAG -JTAGConf -1,-1 -Speed 4000 -CommanderScript load.jlink
```

the **load.jlink** script simply loads the k64f.bin file:

```bash
loadbin k64f.bin, 0x0
r
exit
```

