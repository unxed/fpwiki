# gdbint

## GDBINT - GNU debugger interface

The gdbint package contains a set of units to interface to the GNU debugger (GDB) library, libgdb. It is used in the Free Pascal text-based IDE. The units are: 

  * **gdbint** The actual interface unit, actually a translation of the library header files. (View interface)
  * **gdbcon** Implements the TGDBController object, which controls a debugging session. It implements methods which map to GDB commands. (View Interface)



There are also some example programs: 

  * **gdbver** Program which detects the version of libgdb present on the system. The exit code is the version number of the gdb library.
  * **symify** Program which translates backtrace addresses to file and line information. This can be used on a program which has debug information compiled in. 
    * **testgdb** small program which offers a simple command-line interface to the GDB debugger.



Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/gdbint](https://web.archive.org/web/20240624103158/https://wiki.freepascal.org/gdbint)_
