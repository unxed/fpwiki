# ARM compiler options

## Contents

  * 1 Overview
  * 2 ABI
  * 3 Instruction sets
  * 4 Floating point support
    * 4.1 Interactions
  * 5 Other relevant options
  * 6 See also



## Overview

The FPC compiler lets you create (cross)compilers for various [ARM](<ARM.md> "ARM") instruction sets, floating point units etc. 

Current (FPC 2.6.x/2.7.x) compilers (and RTL PPUs etc) are built for just one ABI and cannot output other ABIs once built - e.g. adding `-dFPC_ARMHF` when compiling a program will not change anything: <http://lists.freepascal.org/lists/fpc-devel/2013-May/032093.html>

The options below are passed in OPTS= when building FPC. When building a cross compiler, some of them can/should be passed in CROSSOPT as well. 

## ABI

ABIs are selected using the -Ca switch. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Normally, specifying an ABI is not needed. Source: <http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg35496.html>

  * default: ?
  * eabi: ?
  * EABI HF is selected using `-dFPC_ARMHF`; the `-Ca eabihf` option is present but is not necessary (but may not hurt). (Source: <http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg35487.html>)



  


## Instruction sets

Instructions sets are selected using the `-Cp` switch. Possible instruction sets (note many sets are backward compatible): 

  * ARMV3
  * ARMV4
  * ARMV4T
  * ARMV5
  * ARMV5T
  * ARMV5TEJ
  * ARMV6: e.g. in Raspberry Pi; will probably work on most current hardware
  * ARMV6K
  * ARMV6T2
  * ARMV6Z
  * ARMV7: e.g. in Nokia N900, Nokia N9, Nokia N950
  * ARMV7A: e.g. in Odroid U2, Odroid U3
  * ARMV7R
  * ARMV7M
  * ARMV7EM



## Floating point support

Floating point support coprocessor (FPU) options are selected with the `-Cf` switch Possible FPU support options: 

  * soft: softfloat/no FPU support.
  * vfpv2: e.g. on Raspberry Pi??
  * vfpv3: e.g. on Odroid U2, Odroid U3
  * vfpv3_d16



... 

### Interactions

Below adapted from: [[1]](<http://www.mail-archive.com/fpc-devel@lists.freepascal.org/msg28056.html>)

  * armel and armhf indicate how floating point (fp) values are passed. 
    * Armel means that all floating point values are always passed in integer registers.
    * Armhf means that fp values are generally passed in floating point registers. As indicated above, armhf is settable by `-dFPC_ARMHF`; see above)


  * The softfp/vfpv2 distinction for armel indicates how the fp operations are performed. 
    * Softfp means that all fp operations are performed by software
    * vfpv2, vfpv3 means that fp operations are performed by the hardware. Although fp values are still passed in integer registers, they're copied to fp registers before performing the operation.



From an ABI point of view softfp and vfpv2 are compatible. However, vfpv2 executables will crash if run on a processor not having vfpv2 hardware. 

## Other relevant options

  * If you are using FPC 2.7.1+ and want to speed up floating point, you can also specify `-OoFASTMATH` to sacrifice precision for performance.



## See also

  * [Setup Cross Compile For ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")

---

_Source: [https://wiki.freepascal.org/ARM_compiler_options](https://web.archive.org/web/20250324170237/https://wiki.freepascal.org/ARM_compiler_options)_
