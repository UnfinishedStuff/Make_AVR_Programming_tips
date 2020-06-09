# Make_AVR_Programming_tips
This is a guide which explains how to set up a PC for the Make: AVR Programming book, mostly because I'm an idiot and every time I try to do this I forget a few things and then spend DAYS trying to figure it out again.

1) Download the [AVR Toolchain from here](https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers).  Note that at some point Microchip put this behind a sign-up wall.

2) Download the [Make: AVR Programming companion files](https://github.com/hexagon5un/AVR-Programming).

3) FOR THE LAST TIME, YOU DO NOT NEED GCC!  You think this EVERY TIME and it's half the reason you take so long to get this set up!

4) Unzip the folders from 1) and 2).

5) Set the Windows Path to include `/toolchain/bin` and `AVR-programming/avrdude_utilities`.

6) Install your USBASP programmer.  You'll need to use [Zadig](https://zadig.akeo.ie/) to change the driver to `libusb-win32`.

7) Edit the `makefile` inside each chapter's example code folder.  `MCU` should be `atmega328p`, `PROGRAMMER_TYPE` should be `usbasp`, and things often go more smoothly with `PROGRAMMER_ARGS` set to `-B 10`.

Using these settings the `make flash` example batch script should work.  The long-form manual instruction is `avrdude -c usbasp -p ATMEGA328p -b 9600 -U flash:w:blinkLED.hex -B 10`

Everyy damn time that you do this you screw up the 328p by flashing the code without setting the fuse bits, so the chip expects there to be an external 8MHz crystal.  You need to flash the chip to not want that.  Use the [AVR Fuse Calculator](https://www.engbedded.com/fusecalc/) to check what you need, everything default with the internal crystal gives `-U lfuse:w:0x62:m -U hfuse:w:0xd9:m -U efuse:w:0xff:m`.  Flash this using `avrdude`: `avrdude -c usbasp -p ATMEGA328p -b 9600 -B 10 -U lfuse:w:0x62:m -U hfuse:w:0xd9:m -U efuse:w:0xff:m`.  It will compain about the efuse value being changed to `0x07` and ask if you want it changed back, say no.

That should be it.  You're welcome future me.  Now stop procrastinating!
