# tonuino-battery-board

An all-in-one PCB for a RF-ID controlled music player, not only for kids.



## Hardware

It is designed using the free online tool easyEDA. Here is the project page
on OSHWlab: https://oshwlab.com/tenbaht/tonuino



## Assembly

This board was designed with the JLC assembly service in mind. All SMD parts
(except for the micro-USB connector) are part of their basic parts catalog
and available for assembly at no extra cost.

I paid just under US$40 for 5 boards - including PCB production, parts
procurement, stencil production, assembly, shipping and even prepaid custom
duties (19% VAT for me). It took 13 days from uploading the files to
receiving a little box with my boards (to Germany). I can highly recommend
that service. And no, I don't get paid for this. I am just a happy customer.



## Firmware

The hardware is Arduino compatible with Reset-on-DTR. After flashing the
bootloader with an ISP tool it can be used like any other Arduino compatible
board via the USB connection.


### Flashing the bootloader

	avrdude -c usbasp -p atmega328p -e -u -U lock:w:0xff:m -U efuse:w:0xFD:m -U hfuse:w:0xDE:m -U lfuse:w:0xFF:m -U flash:w:bootloader/optiboot_atmega328_38k4.hex -U lock:w:0xef:m

The precompiled hex file contains a bootloader with the unusual speed
setting of 38400. The more standard value of 115200 baud is difficult with
a clock rate of 8MHz. It will result in a baud rate error of -3.5% which
might be a problem for the CH340. Alternatively, the file 
`bootloader/optiboot_atmega328_115k2.hex` can be used for a higher speed.


### Compiling the bootloader

The precompiled binaries are configured to flash an LED on PD7 on startup.
Even without an actual LED this might come in handy for debugging:

	git checkout https://github.com/Optiboot/optiboot
	cd optiboot
	# compile for 38400 baud
	make atmega328 AVR_FREQ=8000000L LED=D7 BAUD_RATE=38400
	# compile for 115200 baud
	make atmega328 AVR_FREQ=8000000L LED=D7

Using `make atmega328_isp` to flash the compiled hex file often fails with
an error messages about a read error. This is because the unused lock bits
6+7 always read as 1, but avrdude expects them to be 0. Just use the command
above.



## Hardware Specifications

			| v1.0
---			|---
CPU type		| ATmega328p
I/O voltage		| 3.3V
CPU clock		| 8MHz
USB serial interface	| CH340
CPU reset		| 100nF from DTR (Arduino auto-reset)
Power consumption	| < 50mA active, 0.5mA shutdown
Battery type		| 16850 Li-Ion
Battery protection IC	| DW-01 under/overcharging protector
Battery charging IC	| TP4052
charging connection	| micro-USB
Audio player module	| DFplayer mini
Audio storage media	| micro-SD card, FAT32 filesystem
RF-ID reader		| RFID-RC522 module
RF-ID reader connector	| 8 pin XH
button connectors	| 2 pin XH
analog volume connector	| 3 pin XH
speaker connector	| 2 pin XH
speaker impedance	| 4ohm to 8ohm, mono
board size		| 50x90mm²
mounting holes		| 43x83mm, 3mm diameter, connected to GND



## Purpose of this repository

This is only an archive of the design files exported from easyEDA. Just in
case they might change their licence terms in the future.

