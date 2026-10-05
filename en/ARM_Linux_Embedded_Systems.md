# ARM Linux Embedded Systems

This page shall offer a hardware selection guide for those seeking an ARM based (ie low power) platform for FPC and want also to benefit of the capabilities of the underlying Linux OS. Development kits are listed below their respective targets (mentioning the additional features in the I/O column). 

Also this will link to a [FPC Embedded Nutshell](<FPC_Embedded_Nutshell.md> "FPC Embedded Nutshell") page, which holds ready made 'hello world' samples for various systems, to be able to tryout FPC on your system before having to undergo a full installation and a possible build/cross-compile etc. 

Products are listed alphabetically. 

Model  | Version  | CPU  | Arch  | Speed  | RAM  | Flash  | ext. Stor  | I/O  | JTAG  | OS  | Kernel  | FPC Notes   
---|---|---|---|---|---|---|---|---|---|---|---|---  
[ARM 9 Boards by Artila](<http://www.artila.com>)  
**M-501** |  | Atmel  
AT91RM9200  
ARM920 | 4T | 180MHz | 64M | 16M | - | 1E,4S,32D,3UH,I2C,SPI,SD | Yes | Linux | 2.6.14.x | \-   
**PAC-5010** | - | - | - | - | - | - | SD | 2E,1S,1*485,16 opto DI, 8 opto DO | - | - | - | \-   
[Xilinx FPGA board by MYIR](<http://www.myirtech.com/list.asp?id=502>)  
**MYD-C7Z015** |  | Xilinx  
XC7Z015 | - | 766MHz | 1GB | 4GB eMMC  
32MB QSPI Flash | SD | USB OTG,I2C,SPI,ADC | Yes | Linux | 3.15.0 | \-   
**MYD-CZU3EG** |  | Xilinx  
Zynq UltraScale | - | 1.2GHz | 4GB DDR4 | 4GB eMMC  
128MB QSPI Flash | SD | PS JTAG interface, USB 2.0 interface, Gigabit Ethernet, 156 user PL I/O pins | Yes | Linux | 4.9.0 | \-   
**Z-turn Board** |  | Xilinx  
XC7Z020 | - | 766MHz | 1GB | 16MB QSPI Flash | SD | USB OTG,I2C,SPI,ADC | Yes | Linux | 3.15.0 | \-   
**Z-turn Lite** |  | Xilinx  
XC7Z010 or XC7Z007S | - | 667MHz | 512MB | 4GB eMMC  
66MB QSPI Flash | SD | USB OTG,I2C,SPI,ADC | Yes | Linux | 3.15.0 | \-   
[BeagleBoard](<http://beagleboard.org/>)  
**BeagleBoard** |  | TI  
OMAP3530 | 7R | 720MHz | 256M | 256M | - | UH,UG,1S,2K,SD,HDMI,S-video,Sound IO | Yes | Linux | 2.6.x.x | \-   
**BeagleBone Black** |  | TI  
AM335x ARM® Cortex-A8 | 7A | 1GHz | 512M | 2G | SD | UH,UG,4S,2K,65D,8PWM,I2C,SPI,SD,Micro-HDMI | Yes | Angström Linux, Android, Ubuntu | 3.0 | Lazarus and FPC support depending on Linux distribution   
[DilNet-PC by SSV](<http://www.dilnetpc.com/>)  
**DNP-9200** | Rev2 | Atmel  
AT91RM9200  
ARM920 | 4T | 180MHz | 32M | 16M | - | 1E,3S,UH,UD,20D,SPI,SD | Yes | Linux | 2.6.x.x | \-   
**DNP/SK23** | - | - | - | - | - | - | SD | LCD(4x16Ch),4K | - | - | - | \-   
**ADNP-9200** | Rev2 | Atmel  
AT91RM9200  
ARM920 | 4T | 180MHz | 64M | 16/32M | - | 2E,3S,UH,UD,20D,CF | Yes | Linux | 2.6.x.x | \-   
**DNP/SK27** | - | - | - | - | - | - | SD | LCD(128x64,T6963C),4K | - | - | - | \-   
[Eddy CPU by System Base](<http://sysbas.en.ec21.com/Embedded_CPU_Module--1904028_1904479.html>)  
**Eddy CPU** | V2.5 | ARM9G20 | _ | 400MHz | 32M | 8M | _ | 1E,4S,UH,UG,56D | Yes | Linux | 2.6 | \-   
[FirendlyARM](<https://www.friendlyarm.net/>)  
**Mini2440** |  | Samsung S3C2440A ARM920T |  | 400MHz | 64M | 1G | SD | 1E,1S,UH,UG,34D,10B,4L | Yes | Linux | 2.6 |   
**Mini6440** |  | Samsung S3C6410A ARM1176JZF-S |  | 533MHz | 128/256M | 1G | SD | 1E,1S,UH,UG,30D,8B,4L | Yes | Linux | 2.6 |   
**Mini210** |  | Samsung S5PV210 |  | 1GHz | 512M | 1G | SD | 1E,1S,UH,UG,60D,20B,4L | Yes | Linux | 2.6 |   
[Gnublin](<http://gnublin.org>)  
**Gnublin** | V1.3 | NXP  
LPC3131  
ARM 9 | ? | 180MHz | Micro-SD | 8M | _ | UC,UG,I2C,SPI,8D | No | Debian Squeeze | 2.6 | \-   
[IGEPv2 Board](<http://www.igep-platform.com/index.php?option=com_content&view=article&id=46&Itemid=55>)  
**IGEPv2 Board** | Rev C | TI OMAP3530 | A8 | 600MHz | 4G | 4G | SD | 1E,1S,UH,UG,WiFi,BT,DVI-D | Yes | Linux | 2.6 | \-   
[Open Moko](<http://wiki.openmoko.org/wiki/Main_Page>)  
**GTA02** | - | Samsung 2442B  
ARM920T | 4T | 400-500MHz | 128M | 256+2MB |  |  |  |  |  |   
[Open Risc by Vision Systems](<http://www.visionsystems.de/1_1_4.html>)  
**ALEKTO** | - | ARM922T | 4T | 166Mhz | 64M | 4M | CF | 2E,2S,8D,2 UH,MiniPCI,TWI | ??? | Debian | 2.4 | 2.2.2 native OK   
[Raspberry Pi](<http://www.raspberrypi.org>)  
**[Model 1A](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")** | Revs 0007 to 0009 | Broadcom  
BCM 2835  
ARM1176JZF-S | v6T | 700MHz | 256M | SD/MMC-Card | via USB | 1S,1UH,17D,I2C,SPI | Yes | Debian Squeeze / [Raspbian Wheezy](<Raspbian_Wheezy.md> "Raspbian Wheezy") | 3.2.27 | FPC and Lazarus to be installed via apt-get   
**[Model 1B](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")** | Beta and Revs 0002 to 0006 | Broadcom  
BCM 2835  
ARM1176JZF-S | v6T | 700MHz | 256M | SD/MMC-Card | via USB | 1E,1S,2UH,17D,I2C,SPI | Yes | 3.2.27   
Revs 000d to 000f | Broadcom  
BCM 2835  
ARM1176JZF-S | v6T | 700MHz | 512M | SD/MMC-Card | via USB | 1E,1S,2UH,17D,I2C,SPI | Yes | 3.2.27   
[[1]](<https://en.wikipedia.org/wiki/Raspberry_Pi#Specifications>) Specifications for very detailed information   
[SheevaPlug](<http://www.globalscaletechnologies.com/p-22-sheevaplug-dev-kit-us.aspx>)  
**US-Version** | - | Marvell  
88F6281  
ARM926EJ-S | 5TE | 800M-1G | 512M | 512M | - | 1EG,1S,UH,SD,2L,TWI | Yes | Linux | 2.6.x.x | \-   
[SheevaPlug at CCC](<https://wiki.koeln.ccc.de/index.php?title=Sheevaplug>) for very detailed information (German)   
  
  


## Shortcuts for Column 'I/O'

  * nS Serial Ports
  * nE Ethernet Ports
  * nEG Ethernet Ports(Gigabit)
  * nD Digital I/O
  * nK Keys
  * nL Leds
  * UH USB Host
  * UC USB Console (Serial to USB converter on board)
  * UD USB Device
  * UG USB OTG
  * CF CF interface
  * I2C I2C interface
  * SPI SPI interface
  * PWM PWM port



Part of the information is taken from the [OpenWrt](<http://openwrt.org/>) project.

---

_Source: [https://wiki.freepascal.org/ARM_Linux_Embedded_Systems](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/ARM_Linux_Embedded_Systems)_
