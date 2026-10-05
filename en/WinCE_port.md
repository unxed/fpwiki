# WinCE port

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[italiano (it)](</WinCE_port/it> "WinCE port/it")** │  **[português (pt)](</WinCE_port/pt> "WinCE port/pt")** │  **[русский (ru)](<../ru/WinCE_port.md> "WinCE port/ru")** │  **[中文（臺灣） (zh_TW)](</WinCE_port/zh_TW> "WinCE port/zh TW")** │    
****

WinCE port is quite complete and usable. The port was started and maintained by Yury Sidorov. Oliver (Oro06) ported WinCE API headers. 

## Contents

  * 1 Status
  * 2 Debugging WinCE applications
  * 3 See Also
  * 4 Links
  * 5 Contacts



## Status

  * FPC 2.2.0 or later supports WinCE target.
  * CPU support for WinCE target: 
    * [ARM](<ARM.md> "ARM") CPU is fully supported. See [arm-wince](<arm-wince.md> "arm-wince") how to setup a crosscompiling enviroment.  
There are readymade crosscompiler package available(See Lazarus download sources)
    * [i386](</index.php?title=i386&action=edit&redlink=1> "i386 \(page does not exist\)") CPU support was not tested too much and may contain bugs. Patches are welcome.  
Normally there is no readymade crosscompiler package for this CPU. See [i386-wince](<i386-wince.md> "i386-wince") howto setup a cross compile enviroment(Win32 Host)
  * The following platforms are supported: 
    * Devices based on WinCE 3.0 or later
    * Pocket PC 2002 – WinCE version: 3.0
    * Pocket PC 2003 – WinCE version: 4.20
    * Pocket PC 2003 Second Edition – WinCE version: 4.21
    * Windows Mobile 5 – WinCE version: 5.0
    * Windows Mobile 6 – WinCE version: 5.2
    * Windows Mobile 6.5 - WinCE version: 5.2.1
  * [RTL](<http://community.freepascal.org:10000/docs-html/rtl>) and [FCL](<http://community.freepascal.org:10000/docs-html/fcl>) units are working.



## Debugging WinCE applications

GDB can be used to debug your WinCE applications remotely via ActiveSync. Download GDB 6.4 for Win32 host and arm-wince target here: <ftp://ftp.freepascal.org/pub/fpc/contrib/cross/gdb-6.4-win32-arm-wince.zip>

**Some hints:**

  * Pass `--tui` parameter to GDB to enable TUI interface which makes debugging more comfortable.
  * Use unix line endings (LF only) in your pascal source files. Otherwise GDB will show sources incorrctly.



**How to use:**

First, make ActiveSync connection to your Pocket PC device. 

Then launch gdb: 

`gdb --tui <your_executable_at_local_pc>`

On gdb prompt type: 

`run` or just `r`

GDB will copy your executable to the device in `\gdb` folder and run it. 

Here is a short list of most needed GDB commands: 

  * `r args` \- run program with args arguments.
  * `s` \- step into.
  * `n` \- step over.
  * `ni` \- step over instrument.step over assembly instruction.
  * `c` \- continue execution.
  * `br <function_name>` \- set a breakpoint at `function_name`. Use `PASCALMAIN` to set a breakpoint at program start.
  * `br <source_file>:<line_number>` \- set a breakpoint at specified source line.
  * `disas` \- show disassembly of current location.
  * `x/fmt address` \- dump memory at address with special format.use "help x" for more informations.
  * `bt` \- back trace.print back trace of the call stack.
  * `where` \- Display the current line and function and the stack of calls that got you there.
  * `q` \- Quit gdb.



To learn more how to use GDB read its documentation here: <http://www.gnu.org/software/gdb/documentation>

  


For example you want to build FCL. Go to `fpc\fcl` folder and execute the command above. You will get FCL compiled units in `fpc\fcl\units\arm-wince`. 

## See Also

  * [WinCE port hints](<WinCE_port_Hints.md> "WinCE port Hints")
  * [Windows CE interface for Lazarus](<http://wiki.lazarus.freepascal.org/Windows_CE_Interface>)
  * [Windows CE Development Notes](<http://wiki.lazarus.freepascal.org/Windows_CE_Development_Notes>)
  * [WinCE port of KOL GUI library](<KOL-CE.md> "KOL-CE")



## Links

  * Useful WinCE info <http://www.rainer-keuchel.de/documents.html>
  * Standalone Pocket PC device emulator from Microsoft. It emulates ARM CPU. Get it [here](<http://www.microsoft.com/downloads/details.aspx?FamilyId=C62D54A5-183A-4A1E-A7E2-CC500ED1F19A&displaylang=en>)
  * Mamaich Pocket PC port of GCC <http://mamaich.uni.cc>
  * [Buildfaq](<http://www.stack.nl/~marcov/buildfaq.pdf>) is a general FAQ about how to build and configure FPC.



Here are some links related to ARM CPU Architecture 

  * [ARM Core Developers Forum](<http://www.armcorepro.com/>) Not that much active though.
  * [GCC ARM Improvement Project](<http://www.inf.u-szeged.hu/gcc-arm/>)
  * [ARM ASSEMBLER](<http://www.heyrick.co.uk/assembler/index.html>) Good information and codes related to arm assembly language.
  * [GNU ARM toolchain for Cygwin, Linux and MacOS](<http://www.gnuarm.com/>)
  * [Microsoft Windows CE .NET 4.2 ARM Guide](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/wcechp40/html/ccrefarmguide.asp>)
  * [ARM Instruction Sets & Programs](<http://soc.csie.ndhu.edu.tw/source/intro_embedded/ch2-arm-2.ppt>) Very good and consice information about arm architecture
  * [The ARM Instruction Set ](<http://web.njit.edu/~baltrush/arm_stuff/ARMInst.ppt>) Another fine power point file about arm



## Contacts

Write any questions regarding WinCE port to [Yury Sidorov](<mailto:yury_sidorov@mail.ru>)

---

_Source: [https://wiki.freepascal.org/WinCE_port](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/WinCE_port)_
