---
title: 2316, 2332, and 2364 ROMs
permalink: /docs/prom-device-23
exerpt: "2316, 2332, and 2364 Read Only Memory"
---

## 2316, 2332, and 2364 Mask-Programmable Read-only Memory

The 2316 is a 2Kx8 mask-programmable ROM made by Commodore.  Intel made a version called the 2316E.  There were also 4K and 8K versions produced as the 2332 and 2364.  These chips were used by a wide variety of computers in the late 1970s and early 1980s, including products from Commodore, Atari, Ohio Scientific, Radio Shack, and others.

## Compatibility with UV EPROMs

The 2316, 2332 and 2364 ROMs are somewhat unique because they have Chip Select signals that are configured when the data is mask-programmed.  Manufacturers could order a 2316 configured with any of its three Chip Select signals as either active-high or active-low.

This causes a bit of confusion, because the datasheets state that the chips are compatible with standard EPROMs, but they should really state that the chips __can be__ compatible, depending on the configuration selected.  For example, a 2316 is only compatible with a 2716 EPROM if the 2316 was manufactured with *CS1* and *CS2* configured active-low and *CS3* configured active-high.  Otherwise, some signals will need to be inverted to swap out a 23 series chip with a more standard EPROM.

Some systems contain multiple 23xx chips with the Chip Selects configured the same and others will configure each chip differently.  For example, the OSI SuperBoard contains five 2316s and they are all configured with the CS1, CS2, and CS3 lines active-high.  On the other hand, some 8K Atari cartridges use two 2332 chips and will invert one of the Chip Selects so that either chip can be selected with a single address line to differentiate them.  If the chips are removed from the cartridge, they would need different Chip Select values to read their data.

## Reading 23xx chips with TommyPROM

A TommyPROM programmer can be built with an Arduino to read 2316 ROM chips.  This can be constructed on a breadboard for quick use. The [TommyPROM32 printed circuit board](tommyprom32-pcb) version can also be used for a more permanent solution.

The TommyPROM code will scan the ROM to determine the correct Chip Select settings, so no wiring changes or inverters are needed when switching chips of the same type.

To use TommyPROM with 2316 chips, perform the following steps:

* Connect the Arduino data bus lines (D2..D9) to the ROM data lines.
* Connect the shift register address lines to the ROM address lines.
* Connect Arduino A2 (WE) to the ROM's CS3 on pin 21.
* Connect Arduino A1 (CE) to the ROM's CS2 on pin 18.
* Connect Arduino A0 (OE) to the ROM's CS1 on pin 20.
* Compile the TommyPROM code with PROM_IS_23 enabled

When running the TommyPROM software, first use the Unlock command to scan the chip.  This should detect the correct configuration of the chip select lines.
After the scan us complete, the Dump or Read command can be used to read the data.

The 2332 only has two chip selects, so pin 21 should instead be connected to the shift registers A11 and the Arduino Pin A2 left unconncted.  For the 2364, pin 21 connects to A12 and pin 18 connects to A11.

## 23xx Chips and Configurations

|Model     |Type |CS3  |CS2  |CS1  |Tested|Notes|
|:---      |:--- |:---:|:---:|:---:|:---:|:--- |
|**Ohio Scientific SuperBoard**|||
|Monitor   |2316 | x   | H   | H   | Y   |
|BASIC1    |2316 | x   | H   | H   | Y   |
|BASIC2    |2316 | x   | H   | H   | Y   |
|BASIC3    |2316 | x   | H   | H   | Y   |
|BASIC4    |2316 | x   | H   | H   | Y   |
|**Compukit UK101**|||
|MONUK02   |2316 | x   | H   | H   | Y   |
|BASUK01   |2316 | x   | H   | H   | Y   |
|BASUK02   |2316 | x   | H   | H   | Y   |
|BASUK03   |2316 | x   | H   | H   | Y   |
|BASUK04   |2316 | x   | H   | H   | Y   |
|**Commodore 64**|||
|901225-01 |2332 | -   |     |     | N   | Character ROM
|901226-01 |2364 | -   | -   |     | N   | Basic
|901227-03 |2364 | -   | -   |     | N   | Kernal
|**Commodore VIC 20**|||
|901460-03 |2332 | -   | L   | L   | N   | Character ROM
|901486-01 |2364 | -   | -   |     | N   | Basic
|901486-06 |2364 | -   | -   |     | N   | Kernal
|**Commodore 1451**|||
|325302-01 |2364 | -   | -   |     | N   | DOS
|901229-05 |2364 | -   | -   |     | N   | Kernal
|**TRS-80 Model 1**|||
|ROM A     |2364 | -   | -   |     | N|
|ROM B     |2332 | -   |     |     | N|
|**Apple II**|||
|**Atari 400 and 800**|||
|**Atari 2600 Cartridge**|||
|ROM A     |2332 | -   |     |     | N|
|ROM B     |2332 | -   |     |     | N|

## References

[Commodore 64](https://ist.uwaterloo.ca/~schepers/roms.html)

[VIC 20 Character ROM](https://forum.vcfed.org/index.php?threads/replacing-vic-20-rom-char-901460-03-with-2732-unespected-behavior.80746/)

[2532 vs 2732](https://pinside.com/pinball/forum/topic/are-2532-and-2732-eproms-interchangeable)

[2332 vs 2532](https://wereallgeeks.wordpress.com/2023/04/18/2532_2732_progadapter/)