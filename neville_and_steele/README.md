# Neville & Steele version of LisaDAC

I have been interested in computer-generated sound at least since my freshman year in college. I began thinking about how to add sound output to my Lisa-XL computer. The Macintosh contained simple but effective four-channel sound generation. The Lisa-XL ran the same operating system, so I surmised that it might be possible to build hardware to work with the operating system software.

My friend Darryl Burrows had schematics and other technical documentation for the Lisa. After studying them I found some important features that assured my eventual success.

1. The Lisa provided three expansion slots.

2. It also supported DMA (Direct-Memory Access) from these slots to the system memory

3. The Lisa's built-in speaker was connected to each of the three slots

I designed and built a prototype circuit. It was hand-wired point-to-point. It worked well enough that I moved forward with the project. I outsourced the printed circuit board layout to a company down in Garland, Texas across Jupiter Road from Raytheon. The board layout had an unusual shape due to the sloping back of the Lisa, so it was nice to have a professional board shop do the layout and routing.

I worked with a small surplus electronics company on Bowser Road in Richardson, Texas.

Once we received the bare boards and the components, my wife Marianne and I set to work building the boards. We ordered 50 boards and enough parts to populate them all. I stuffed boards and Marianne did most of the soldering. She also debugged and repaired boards. (She can do just about anything!)

We sold about 30 of the boards. I tried to arrange to sell them through Sun Remarketing in Utah, but they were not interested. I finally contacted a company named DAFAX. They purchased the rights to the entire product. I don't know how many of them they sold, or if they made anymore boards. 
 
As you inspect the board you will notice two long connectors on the bottom half of the bottom half of the board. This is for lower-cost expansion (because making the specially shaped Lisa boards was more expensive). I designed and built a SCSI disk interface board. It leveraged the SCSI interface software that Sun Remarketing had put into the MacWorks Plus software. This may have been one of the reasons why Sun did not buy the board. They were already manufacturing a Lisa SCSI board and did not want a competitor. 

## Design

The board operated as follows. A 6116 static RAM was connected directly to the main Lisa 68000 CPU bus through the expansion connector. It served as a dual port RAM. The back half was also connected to an on-board Z80 CPU. The 68000 CPU was responsible for generating the sound values in a buffer on the RAM chip. The Z80 CPU ran a very small program that simply copied one byte at a time from the RAM and wrote it to a Digital-to-Analog Converter (DAC, hence LisaDAC). The analog output from the DAC was amplified by an LM324 operation amplifier (OPAMP) which drove the Lisa Speaker. The remainder of the TTL chips were used to control access to the 6116 static RAM between the 68000 and Z80 CPUs, as well as address decoding, etc. An on-board EPROM was also connected to the 68000 bus. It was read at power-up by the MacWorks Plus operating system to let the system know that the card was present.

A device driver was also required to be loaded in the MacWorks Plus operating system. It was responsible for setting up the Lisa's Memory Map Unit (MMU) to point to the 6116 static RAM in the proper slot where the LisaDAC was installed. The static RAM was mapped to the address of the main sound buffer, starting at address $7FD00 (refer to Inside Macintosh, Volume III, page 21). The driver also included the Z80 CPU's program. Upon power-up the Z80 CPU was held in reset, which prevented it from accessing the 6116. The 68000 loaded the Z80 program into a reserved section of the 6116 RAM, then wrote to an address that released the Z80 from reset, which allowed the Z80 to run its program from the static RAM.

The Z80 CPU used the Direct Memory Access (DMA) lines to gain access to the 68000 bus in order to read bytes from the SRAM. 

