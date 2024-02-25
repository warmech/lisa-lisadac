# LisaDAC
 
This project aims to reproduce Neville & Steele's DAC expansion card for the Apple Lisa (via MacWorks) which provided audio functionality similar to the Macintosh sound hardware. The LisaDAC does not, however, produce 100% compatibility with Mac sound hardware and, as such, has a reduced/degraded audio output when compared to a Macintosh. In some cases, calls made in MacWorks to what would be the Mac's sound hardware when using a LisaDAC may result in MacOS/MacWorks crashing.

## ROM Version

Both version 1.0 (v1_0.BIN) from the Neville & Steele card and 1.1 (v1_1.BIN) from the newer LDAC reproduction are included with this repository. These ROM dumps should be programmed on a 2716 EPROM or other compatible device.

## Jumper Settings

- J3 - Speaker Selection: To use the internal speaker, place a jumper on pins 3 and 4; to use an external speaker, connect the leads to pins 1 and 4.
- J4 - DMA Bus Grant Pass-Through: Whether or not this should have a jumper is debatable; no other Lisa expansion card uses DMA, so there aren't really any other DMA processes to interfere with. The stock configuration is to place a jumper across J4.
- J5 - Unknown: Likely has to do with card slot selection. Configuration "A" jumpers pins 1 and 2 and ties the /SHn (where n is 0, 1, or 2) line on the expansion bus to pin 9 of the 74LS00 at U7; configuration "B" jumpers pins 2 and 3 and ties the +5V rail to pin 9 of the 74LS00 at U7.

## Stability and Crashing

The LisaDAC is generally stable and does not cause stability issues in and of itself when used with a Lisa. Rather, the issue comes with Mac software making unsupported calls to the Mac's (well, MacWorks') sound hardware. StuntCopter, for instance, will run with no issues (albeit not sounding the best) until completing a successful round of five jumps. Upon completion of the fifth successful jump the victory chime is triggered; the call made to play the chime causes MacOS/MacWorks to immediately hang up, requiring either the reset button to be pressed or the Lisa's power to be cycled.

The following is a list of software with known incompatibilities with the LisaDAC:

- StuntCopter
	- Victory ("backflip") chime causes MacOS/MacWorks to crash
