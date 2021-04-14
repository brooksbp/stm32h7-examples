# stm32h7-examples

## STM32H743ZI2

* [UM2407 - STM32H7 Nucleo-144 boards](https://www.st.com/resource/en/user_manual/dm00499160-stm32h7-nucleo144-boards-mb1364-stmicroelectronics.pdf)
* [DS12110](https://www.st.com/resource/en/datasheet/stm32h743vi.pdf)
* [RM0433](https://www.st.com/resource/en/reference_manual/dm00314099-stm32h742-stm32h743753-and-stm32h750-value-line-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
* [PM0253](https://www.st.com/resource/en/programming_manual/dm00237416-stm32f7-series-and-stm32h7-series-cortexm7-processor-programming-manual-stmicroelectronics.pdf)

```
$ sudo screen /dev/ttyACM0 115200
```

## J-Link EDU

```
[ 6266.997614] usb 1-13: new high-speed USB device number 4 using xhci_hcd
[ 6267.146424] usb 1-13: New USB device found, idVendor=1366, idProduct=0101, bcdDevice= 1.00
[ 6267.146429] usb 1-13: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 6267.146432] usb 1-13: Product: J-Link
[ 6267.146434] usb 1-13: Manufacturer: SEGGER
[ 6267.146437] usb 1-13: SerialNumber: 000261007681
```

## OpenOCD

* [OpenOCD User's Guide](http://openocd.org/doc-release/html/index.html)
* [OpenOCD Developer's Guide](http://openocd.org/doc-release/doxygen/index.html)

```
sudo apt-get install make libtool pkg-config autoconf automake texinfo libusb-1.0.0 libhidapi-dev libcapstone-dev
```

```
git clone http://openocd.zylin.com/openocd
cd openocd
./bootstrap
./configure
make
```

[Accessing Devices without Sudo](https://elinux.org/Accessing_Devices_without_Sudo)

```
groups <username>
sudo useradd -G plugdev <username>   <- iff necessary

sudo cp contrib/60-openocd.rules /etc/udev/rules.d/

sudo udevadm trigger
```

```
~/openocd (master *)$ cat ../my.cfg
adapter driver jlink
adapter speed 1000

transport select swd
source [find target/stm32h7x_dual_bank.cfg]

proc attach () {
     init
     reset run
}
```

```
~/openocd (master *)$ ./src/openocd -s tcl/ -f ../my.cfg
Open On-Chip Debugger 0.11.0+dev-00082-g0f06d9433-dirty (2021-04-13-14:23)
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
attach
Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections
Info : J-Link V11 compiled Jul  3 2020 10:47:34
Info : Hardware version: 11.00
Info : VTarget = 3.303 V
Info : clock speed 1800 kHz
Info : SWD DPIDR 0x6ba02477
Info : stm32h7x.cpu0: hardware has 8 breakpoints, 4 watchpoints
Info : gdb port disabled
Info : starting gdb server for stm32h7x.cpu0 on 3333
Info : Listening on port 3333 for gdb connections
```

```
~/openocd$ telnet 127.0.0.1 4444
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Open On-Chip Debugger
>
> usage
adapter
  adapter assert |deassert [srst|trst [assert|deassert srst|trst]]
  adapter deassert |assert [srst|trst [deassert|assert srst|trst]]
  adapter driver driver_name
  adapter list
  adapter name
  adapter speed [khz]
  adapter srst
    adapter srst delay [milliseconds]
    adapter srst pulse_width [milliseconds]
  adapter transports transport ... 
  adapter usb
    adapter usb location [<bus>-port[.port]...]
add_help_text command_name helptext_string
add_script_search_dir <directory>
add_usage_text command_name usage_string
arm
  arm core_state ['arm'|'thumb']
  arm disassemble address [count ['thumb']]
  arm mcr cpnum op1 CRn CRm op2 value
  arm mrc cpnum op1 CRn CRm op2
  arm reg
  arm semihosting ['enable'|'disable']
  arm semihosting_cmdline arguments
  arm semihosting_fileio ['enable'|'disable']
  arm semihosting_resexit ['enable'|'disable']
array2mem arrayname bitwidth address count
bindto [name]
bp [<address> [<asid>] <length> ['hw'|'hw_ctx']]
capture command
command
  command mode [command_name ...]
cortex_m
  cortex_m maskisr ['auto'|'on'|'off'|'steponly']
  cortex_m reset_config ['sysresetreq'|'vectreset']
  cortex_m vector_catch ['all'|'none'|('bus_err'|'chk_err'|...)*]
cti
  cti create name '-chain-position' name [options ...]
  cti names
dap
  dap create name '-chain-position' name
  dap info [ap_num]
  dap init
  dap names
debug_level number
dump_image filename address size
echo [-n] string
exit
fast_load
fast_load_image filename address ['bin'|'ihex'|'elf'|'s19'] [min_address [max_length]]
find <file>
flash
  flash bank bank_id driver_name base_address size_bytes chip_width_bytes
            bus_width_bytes target [driver_options ...]
  flash banks
  flash erase_address ['pad'] ['unlock'] address length
  flash erase_check bank_id
  flash erase_sector bank_id first_sector_num (last_sector_num|'last')
  flash fillb address value n
  flash filld address value n
  flash fillh address value n
  flash fillw address value n
  flash info bank_id ['sectors']
  flash init
  flash list
  flash mdb address [count]
  flash mdh address [count]
  flash mdw address [count]
  flash padded_value bank_id value
  flash probe bank_id
  flash protect bank_id first_block [last_block|'last'] ('on'|'off')
  flash read_bank bank_id filename [offset [length]]
  flash verify_bank bank_id filename [offset]
  flash verify_image filename [offset [file_type]]
  flash write_bank bank_id filename [offset]
  flash write_image [erase] [unlock] filename [offset [file_type]]
gdb_breakpoint_override ('hard'|'soft'|'disable')
gdb_flash_program ('enable'|'disable')
gdb_memory_map ('enable'|'disable')
gdb_port [port_num]
gdb_report_data_abort ('enable'|'disable')
gdb_report_register_access_error ('enable'|'disable')
gdb_save_tdesc
gdb_sync
gdb_target_description ('enable'|'disable')
halt [milliseconds]
help [command_name]
init
itm
  itm port <port> (0|1|on|off)
  itm ports (0|1|on|off)
jlink
  jlink config [<cmd>]
    jlink config ip [A.B.C.D[/E] [F.G.H.I]]
    jlink config mac [ff:ff:ff:ff:ff:ff]
    jlink config reset
    jlink config targetpower [on|off]
    jlink config usb [0-3]
    jlink config write
  jlink emucom
    jlink emucom read <channel> <length>
    jlink emucom write <channel> <data>
  jlink freemem
  jlink hwstatus
  jlink jtag [2|3]
  jlink serial <serial number>
  jlink targetpower <on|off>
  jlink usb <0-3>
jsp_port [port_num]
load_image filename address ['bin'|'ihex'|'elf'|'s19'] [min_address] [max_length]
log_output [file_name | "default"]
mdb ['phys'] address [count]
mdd ['phys'] address [count]
mdh ['phys'] address [count]
mdw ['phys'] address [count]
measure_clk
mem2array arrayname bitwidth address count
mmw address setbits clearbits
mrb address
mrh address
mrw address
ms
mwb ['phys'] address value [count]
mwd ['phys'] address value [count]
mwh ['phys'] address value [count]
mww ['phys'] address value [count]
nand
  nand device bank_id driver target [driver_options ...]
  nand drivers
  nand init
noinit
ocd_find file
pld
  pld device driver_name [driver_args ... ]
  pld init
poll ['on'|'off']
poll_period
power_restore
profile seconds filename [start end]
program <filename> [address] [pre-verify] [verify] [reset] [exit]
ps  
rbp 'all' | address
reg [(register_number|register_name) [(value|'force')]]
reset [run|halt|init]
reset_config [none|trst_only|srst_only|trst_and_srst]
          [srst_pulls_trst|trst_pulls_srst|combined|separate]
          [srst_gates_jtag|srst_nogate] [trst_push_pull|trst_open_drain]
          [srst_push_pull|srst_open_drain]
          [connect_deassert_srst|connect_assert_srst]
reset_nag ['enable'|'disable']
resume [address]
rtt
  rtt channellist
  rtt channels
  rtt polling_interval [interval]
  rtt server
    rtt server start <port> <channel>
    rtt server stop <port>
  rtt setup <address> <size> <ID>
  rtt start
  rtt stop
rwp address
script <file>
shutdown
sleep milliseconds ['busy']
soft_reset_halt
srst_deasserted
step [address]
stm32h7x
  stm32h7x lock bank_id
  stm32h7x mass_erase bank_id
  stm32h7x option_read bank_id reg_offset
  stm32h7x option_write bank_id reg_offset value [mask]
  stm32h7x unlock bank_id
stm32h7x.ap2
  stm32h7x.ap2 arp_examine ['allow-defer']
  stm32h7x.ap2 arp_halt
  stm32h7x.ap2 arp_halt_gdb
  stm32h7x.ap2 arp_poll
  stm32h7x.ap2 arp_reset
  stm32h7x.ap2 arp_waitstate
  stm32h7x.ap2 array2mem arrayname bitwidth address count
  stm32h7x.ap2 cget target_attribute
  stm32h7x.ap2 configure [target_attribute ...]
  stm32h7x.ap2 curstate
  stm32h7x.ap2 eventlist
  stm32h7x.ap2 examine_deferred
  stm32h7x.ap2 invoke-event event_name
  stm32h7x.ap2 mdb address [count]
  stm32h7x.ap2 mdd address [count]
  stm32h7x.ap2 mdh address [count]
  stm32h7x.ap2 mdw address [count]
  stm32h7x.ap2 mem2array arrayname bitwidth address count
  stm32h7x.ap2 mwb address data [count]
  stm32h7x.ap2 mwd address data [count]
  stm32h7x.ap2 mwh address data [count]
  stm32h7x.ap2 mww address data [count]
  stm32h7x.ap2 was_examined
stm32h7x.cpu0
  stm32h7x.cpu0 arm
    stm32h7x.cpu0 arm core_state ['arm'|'thumb']
    stm32h7x.cpu0 arm disassemble address [count ['thumb']]
    stm32h7x.cpu0 arm mcr cpnum op1 CRn CRm op2 value
    stm32h7x.cpu0 arm mrc cpnum op1 CRn CRm op2
    stm32h7x.cpu0 arm reg
    stm32h7x.cpu0 arm semihosting ['enable'|'disable']
    stm32h7x.cpu0 arm semihosting_cmdline arguments
    stm32h7x.cpu0 arm semihosting_fileio ['enable'|'disable']
    stm32h7x.cpu0 arm semihosting_resexit ['enable'|'disable']
  stm32h7x.cpu0 arp_examine ['allow-defer']
  stm32h7x.cpu0 arp_halt
  stm32h7x.cpu0 arp_halt_gdb
  stm32h7x.cpu0 arp_poll
  stm32h7x.cpu0 arp_reset
  stm32h7x.cpu0 arp_waitstate
  stm32h7x.cpu0 array2mem arrayname bitwidth address count
  stm32h7x.cpu0 cget target_attribute
  stm32h7x.cpu0 configure [target_attribute ...]
  stm32h7x.cpu0 cortex_m
    stm32h7x.cpu0 cortex_m maskisr ['auto'|'on'|'off'|'steponly']
    stm32h7x.cpu0 cortex_m reset_config ['sysresetreq'|'vectreset']
    stm32h7x.cpu0 cortex_m vector_catch ['all'|'none'|('bus_err'|'chk_err'|...)*]
  stm32h7x.cpu0 curstate
  stm32h7x.cpu0 eventlist
  stm32h7x.cpu0 examine_deferred
  stm32h7x.cpu0 invoke-event event_name
  stm32h7x.cpu0 itm
    stm32h7x.cpu0 itm port <port> (0|1|on|off)
    stm32h7x.cpu0 itm ports (0|1|on|off)
  stm32h7x.cpu0 mdb address [count]
  stm32h7x.cpu0 mdd address [count]
  stm32h7x.cpu0 mdh address [count]
  stm32h7x.cpu0 mdw address [count]
  stm32h7x.cpu0 mem2array arrayname bitwidth address count
  stm32h7x.cpu0 mwb address data [count]
  stm32h7x.cpu0 mwd address data [count]
  stm32h7x.cpu0 mwh address data [count]
  stm32h7x.cpu0 mww address data [count]
  stm32h7x.cpu0 rtt
    stm32h7x.cpu0 rtt channellist
    stm32h7x.cpu0 rtt channels
    stm32h7x.cpu0 rtt polling_interval [interval]
    stm32h7x.cpu0 rtt setup <address> <size> <ID>
    stm32h7x.cpu0 rtt start
    stm32h7x.cpu0 rtt stop
  stm32h7x.cpu0 tpiu
    stm32h7x.cpu0 tpiu config (disable | ((external | internal (<filename> | <:port> | -)) (sync <port
              width> | ((manchester | uart) <formatter enable>))
              <TRACECLKIN freq> [<trace freq>]))
  stm32h7x.cpu0 was_examined
stm32h7x.dap
  stm32h7x.dap apcsw [value [mask]]
  stm32h7x.dap apid [ap_num]
  stm32h7x.dap apreg ap_num reg [value]
  stm32h7x.dap apsel [ap_num]
  stm32h7x.dap baseaddr [ap_num]
  stm32h7x.dap dpreg reg [value]
  stm32h7x.dap info [ap_num]
  stm32h7x.dap memaccess [cycles]
  stm32h7x.dap ti_be_32_quirks [enable]
stm32h7x.swo
  stm32h7x.swo cget attribute
  stm32h7x.swo configure [attribute value ...]
  stm32h7x.swo disable
  stm32h7x.swo enable
  stm32h7x.swo eventlist
stm32h7x.tpiu
  stm32h7x.tpiu cget attribute
  stm32h7x.tpiu configure [attribute value ...]
  stm32h7x.tpiu disable
  stm32h7x.tpiu enable
  stm32h7x.tpiu eventlist
swd
  swd newdap
swo
  swo create name [-dap dap] [-ap-num num] [-address baseaddr]
  swo init
  swo names
target
  target create name type '-chain-position' name [options ...]
  target current
  target init
  target names
  target smp targetname1 targetname2 ...
  target types
target_request
  target_request debugmsgs ['enable'|'charmsg'|'disable']
targets [target]
tcl_notifications [on|off]
tcl_port [port_num]
tcl_trace [on|off]
telnet_port [port_num]
test_image filename [offset [type]]
test_mem_access size
tpiu
  tpiu config (disable | ((external | internal (<filename> | <:port> | -)) (sync <port
            width> | ((manchester | uart) <formatter enable>)) <TRACECLKIN
            freq> [<trace freq>]))
  tpiu create name [-dap dap] [-ap-num num] [-address baseaddr]
  tpiu init
  tpiu names
trace
  trace history ['clear'|size]
  trace point ['clear'|address]
transport
  transport init
  transport list
  transport select [transport_name]
usage [command_name]
verify_image filename [offset [type]]
verify_image_checksum filename [offset [type]]
version
virt2phys virtual_address
wait_halt [milliseconds]
wp [address length [('r'|'w'|'a') value [mask]]]
```

## GDB

```
$ sudo apt-get install gcc-arm-none-eabi gdb-multiarch
```

```
i7-8700:~$ gdb-multiarch
GNU gdb (Debian 10.1-1.7) 10.1.90.20210103-git
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) target remote :3333
Remote debugging using :3333
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
0x08016a9c in ?? ()
(gdb) 
```

```
accepting 'gdb' connection on tcp/3333
target halted due to debug-request, current mode: Thread 
xPSR: 0x81000000 pc: 0x08016a9c msp: 0x240090c8
Device: STM32H74x/75x
flash size probed value 2048
STM32H7 flash has dual banks
Bank (0) size is 1024 kb, base address is 0x08000000
Device: STM32H74x/75x
flash size probed value 2048
STM32H7 flash has dual banks
Bank (1) size is 1024 kb, base address is 0x08100000
New GDB Connection: 1, Target stm32h7x.cpu0, state: halted
Prefer GDB command "target extended-remote 3333" instead of "target remote 3333"
> 
```

```
(gdb) continue
Continuing.
^C
Program received signal SIGINT, Interrupt.
0x0801e9a0 in ?? ()
(gdb) info target
Remote serial target in gdb-specific protocol:
Debugging a target over a serial line.
(gdb) info program
Debugging a target over a serial line.
Program stopped at 0x801e9a0.
It stopped with signal SIGINT, Interrupt.
Type "info stack" or "info registers" for more information.
(gdb) info stack
#0  0x0801e9a0 in ?? ()
Backtrace stopped: previous frame identical to this frame (corrupt stack?)
(gdb) info registers
r0             0x1c82089           29892745
r1             0x4000              16384
r2             0x4000              16384
r3             0x4000              16384
r4             0x64                100
r5             0x1c82088           29892744
r6             0x65                101
r7             0x0                 0
r8             0x0                 0
r9             0x0                 0
r10            0x0                 0
r11            0x0                 0
r12            0x84000000          -2080374784
sp             0x240090c8          0x240090c8
lr             0x801e9a1           134343073
pc             0x801e9a0           0x801e9a0
xPSR           0x81000000          -2130706432
fpscr          0x2000000           33554432
msp            0x240090c8          0x240090c8
psp            0x0                 0x0
primask        0x0                 0
basepri        0x0                 0
faultmask      0x0                 0
control        0x4                 4
(gdb) list
No symbol table is loaded.  Use the "file" command.
(gdb) disassemble
No function contains program counter for selected frame.
```
