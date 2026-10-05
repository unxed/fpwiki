# PPUMove

│ **English (en)** │

From [FreePascal User's Manual](<ftp://ftp.freepascal.org/pub/fpc/docs-pdf/user.pdf>): 

ppumove is a program to make shared or static libraries from multiple units. It can be compared with the tpumove program that comes with Turbo Pascal. It should be distributed in binary form along with the compiler. 

Its usage is very simple: 

ppumove [options] unit1.ppu unit2.ppu ... unitn.ppu 

Where options is a combination of 

-b: If specified, ppumve will generate a batch file that will contain the external linking and archiving commands that must be executed. The name of this batch file is pmove.sh on linux, and pmove.bat otherwise. 

-d xxx: If specified, the output files will put in the directory xxx 

-e xxx: Sets the extension of the moved unit files to xxx. By default, this is .ppl. You don't have to specify the dot. 

-o xxx: sets the name of the output file, i.e. the name of the file containing all the units. This parameter is mandatory when you use multiple files. On linux, ppumove will prepend this name with lib if it isn't already there, and will add an extension appropriate to the type of library. 

-q: Causes ppumove to operate silently. 

-s: Tells ppumove to make a static library instead of a dynamic one; By default, a dynamic library is made on linux. 

-w: Tells ppumove that it is working under Windows NT. This will change the names of te linker and archiving program to ldw and arw, respectively. 

-h or -?: will display a short help.

---

_Source: [https://wiki.freepascal.org/PPUMove](https://web.archive.org/web/20250122043453/https://wiki.freepascal.org/PPUMove)_
