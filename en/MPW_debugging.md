# MPW debugging

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS Classic](</Category:Mac_OS_Classic> "Category:Mac OS Classic") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

The possibilities to do high level debugging in target Mac OS Classic (note: not macOS) is limited, due to limitations of PPCAsm to accept debuginfo (stabs). The limitation is thus not in FPC. 

Therefore, if possible, we suggest to do the main debugging in a macOS/Darwin version of your program. 

If you really want to try to use a debugger, Power Mac Debugger 2.1 can be downloaded from <https://macgui.com/downloads/?file_id=17426> or <https://staticky.com/mirrors/ftp.apple.com/developer/Tool_Chest/Core_Mac_OS_Tools/MPW_etc./Debuggers/> (previosuly available at ftp.apple.com/developer/Tool_Chest/Core_Mac_OS_Tools/MPW_etc./Debuggers/). 

You can not use it in the classic environment included in the early versions of macOS, only real Mac OS Classic will do, and then you must shut off virtual memory. The reason is probably some bug in the memory mapping of executable files, so when a breakpoint is set in the debugger, the breakpoint will eventually be saved to disk, and thus the executable will be corrupted, next time the app is started. 

Compile the programs and units with -g to enable debugging. xcoff files will then be generated which contains debugging info. 

  * You can browse among procedures by their mangled names,


  * You can view the source code.


  * You can set breakpoints, stepping and jumping works.


  * You cannot see variable names, only registers. However this can be useful, especially at procedure entry and exit point where the parameter list and return value are stored in registers, according to the calling conventions.


  * In the assembler view, sometimes the symbolic name of a branch destination is wrong. When actually executing the branch, it will however branch to the correct procedure.

---

_Source: [https://wiki.freepascal.org/MPW_debugging](https://web.archive.org/web/20250315170720/https://wiki.freepascal.org/MPW_debugging)_
