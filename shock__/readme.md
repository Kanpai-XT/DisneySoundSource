This archive contains information on how to build a "Disney Sound Source" (DSS for short) compatible circuit.
It's intended to go as a "dongle" between a PC's LPT port and a Covox Speech Thing (or compatible circuit), which is pretty much the minimal approach. Using the informations and schematics included it should be possible to derive a standalone DSS compatible device with it's own audio output, a circuit that can switch between DSS and Covox modes or replace the 2 FIFO chips with a cheaper MCU. I'll leave that up to you, as this circuit was developed as a test to become integrated into a mono/stereo-on-1/stereo-on-2 covox compatible board I'm designing.



Files included are the following:

The folders "Variant 74ALS232" and "Variant CD40105" both contain the same subfiles, each adapted for the corresponding main IC type.

- bom.txt		Bill of materials, required to build the example PCB
- dss_beta.jpg		Photo of the circuit built on breadboard to test the schematic
- dss_emu.brd		Eagle PCB layout example (one sided design, "top layer" can be replaced with jumper wires)
- dss_emu.jpg		Eagle 3D rendering of the example layout for better visualisation
- dss_emu.sch		Eagle PCB schematic
- dss_emu_sch.png	Eagle PCB schematic as a png file for display on machines without the software installed
- placement.jpg		Placement of components & jumper wires on the example PCB

readme.txt		This file
fifo.lbr		Eagle PCB library including the 74ALS232 and CD40105 FIFOs (the CD40105 isn't included in the standard libraries and was made by me)



Notes on the schematic:

- The 555 timer circuit allows adjusting of the output rate between 6.72kHz and 7.42kHz via a variable resistor, this is used to compensate component tolerances. The target frequency is 7kHz. Changing the values of R4, R5, R6 and C4 one can "overclock" the DSS, but might give one trouble with various autodetect routines. R6 can in theory be omited as it serves as an offset to downscale the variable resistor, so hitting the intended value becomes easier (ever tried to hit a 100Ohm exact value on a 10K resistor? It's not that easy) and keep the frequency within a sane limit.

- Connecting just 1 !FULL-signal to a transistor inverter and feeding it back to Pin10 on the input LPT port should be sufficient instead of connecting both !FULL-signals to a NAND-gate and feeding to back to Pin10. Since the FIFOs should never go out of sync under normal circumstances, it's more of a textbook approach.

- On the original DSS adapter every pin was passed through except for Pin17 (which was connected to GND on the output side). To achieve a simpler and smaller PCB layout this feature was omited. Instead the out connector just has the buffered data lines and GND connected, which basically restricts it to accepting mono covox plugs (or stereo ones with mono fallback) as output stages.

- VCC should be 5 volts, using a higher or lower voltage might cause issues with the clock signal (it gets scaled up/down along) as it should be TTL level.

- All ICs should be decoupled using 100nF/0.1uF capacitors. Those are not represented in the schematic or layout. You can either use sockets with decoupling caps, solder caps to the underside of the PCB or directly onto the chips.

- Seeing how I ended up with multiple transistor-resistor gates replacing those with Schottky-TTL might be an option (74HC00?). It might seem a bit chaotic, but in the end allows a smaller PCB footprint and is cheaper.

- As the 74ALS232 chip is "closer" to the operation of the custom IC used in the original Disney Sound Source, but discontinued and quite expensive for a 74-logic IC these days ($6-$10) I do strongly recommend building the CD40105 variant. While the status pins operate slightly different on the CD40105, it didn't pose an issue and the circuit operated as intended with all software tested (see below). 



How this was done:
Basically peeking at the "Programmer's guide" which had an incomplete schematic and a brief description of the internals of the device. Addtionally an original DSS was retraced and the parts measured. From those 2 resources a circuit using off the shelf parts was derived.



Tested and confirmed working with:
- Wolfenstein 3D 
- Spear of destiny
- Rise of the Triad
- Duke Nukem 3D
- NiteRaid
- ModPlay Pro 2.19
- sndsrc.drv (used with Windows 98SE, which BSODs after every sound played back - which also seems to happen with original hardware see: https://www.youtube.com/watch?v=1OKNKwPQ3Gg)



Credits:
Original circuit reversed, schematics drawn and layouts designed by shock__ (mos6502c@gmail.com). You can also find me on the VOGONS forum under that nickname.
Thanks to Jepael from VOGONS forum, for his input and checking over my schematics as they were made.
Thanks to keropi from VOGONS forum, for supplying photos and measuring parts on his original DSS.
Thanks to x1541 from Verein zum Erhalt klassischer Computer e.V. (club for the preservation of classic computers) for poking me towards using the cheaper and non-discontinued CD40105 FIFOs.
Started and finished in February 2015.



License:
CC-BY-NC-SA - basically do with it what you want, as long as you credit me, don't offer it commercially and make your extensions/derivates available under the same license. Only exception, you're allowed to sell PCBs created/derived from this package at a price that covers your expenses but doesn't earn you any money (I guess a $ for handling is only fair, but selling a $3 PCB for $30 would be a scumbag move and in violation of the license). 
