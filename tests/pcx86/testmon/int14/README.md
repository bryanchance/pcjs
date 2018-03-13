---
layout: page
title: PCx86 INT 14h TSR
permalink: /tests/pcx86/testmon/int14/
---

PCx86 INT 14h TSR
-----------------

[INT14.ASM](INT14.ASM) is a Terminate-and-Stay-Resident (TSR) utility that scans the ROM BIOS Data Area for a COM port
whose I/O address is 0x2F8 (or 0x3F8 if the /1 option is specified).  If the port is found, then the utility installs
replacement INT 14h services for that port.  Also, unless the /P option ("polled mode") is specified, the utility also
installs a hardware interrupt handler for IRQ3 (or IRQ4 if /1 is specified) and enables interrupt-driven I/O for the
COM port.

Note that a serial adapter with address 0x2F8 is normally named "COM2", but not always.  For example, if it's the only
adapter in the PC, then DOS will name it "COM1" even if it's using the traditional COM2 address.

[INT14.COM](INT14.COM) and [INT14.LST](INT14.TXT) were built with [Microsoft Macro Assembler 4.00](/disks/pcx86/tools/microsoft/masm/4.00/)
using the following commands:

    masm int14,,int14;
    link int14;
    exe2bin int14.exe int14.com

INT14.COM is intended to be run on an actual IBM PC/XT/AT, so that the PCjs [TestMonitor](/modules/pcx86/bin/testmon.js)
command-line utility can be used to control the PC.  Here's the procedure:

- Turn on your PC
- Boot DOS 2.00 or later
- Load the PCjs INT14 TSR: "INT14"
- Run the DOS CTTY command: "CTTY COM2" 
- On your connected machine, run the PCjs TestMonitor utility: "node testmon.js"

You should now be able to control the PC using the TestMonitor utility, in your choice of either "terminal mode" or
"command mode".

If interrupt-driven I/O doesn't seem to be working, then try installing INT14.COM with /P for "polled mode":

    INT14 /P

In "polled mode", no hardware interrupt handler is installed.  Instead, the INT 14h functions attempt to control
the flow of incoming characters by toggling the RTS line.  However, that may not be sufficient for high speeds (e.g.,
9600 baud), so it's recommended that you use the PC's COM port at the default speed of 2400 baud, which you can also
set with the DOS **MODE** command:

    MODE COM2:2400,N,8,1

testmon.js uses the same default speed of 2400 baud, which you can explicitly set or change as needed:

    node testmon.js --baud=2400

There are currently no `parity`, `databits`, or `stopbits` overrides, so you should always use "N,8,1" with the DOS
**MODE** command.