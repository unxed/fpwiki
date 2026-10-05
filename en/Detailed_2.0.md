# Detailed 2.0.4 Todo

* Go32v2 specific issues 
    * fix debugging in go32v2 ide (does it work now???)
    * fix GO32v2 mouse cursor (FVision - does it work now???)
    * ~~fix DiskSize/DiskFree for large drives ( > 2GB) for GO32v2 running under WinXX~~ (Tomas)
  * Linux/PPC32 target 
    * fix/remove usage of dcbz in rtl/powerp/powerpc.inc so the Linux/ppc32 version no longer crashes on G5's running linux.
    * the IDE crashes at startup with an "Internal SIGSEGV caught" message
  * OS/2 target RTL todos 
    * Isolate and fix the mysterious SIGSEGV during build (Tomas)
    * ~~64-bit file functions under new OS/2 versions~~ (Tomas)
    * EA functions - GetLongName/GetShortName, SetDefaultOS2FileType/SetDefaultOS2Creator (Tomas)
    * port access without EMX (Tomas/Yuri)
    * exception handler (Tomas)
    * OS/2 API functions for widestring support - charset conversion (Tomas)
    * finish threading (Tomas)
  * fix EMX target (Tomas)
  * Review docs according to mac specific things (Olle)
  * IDE 
    * fix ide powerpc/macosx (Jonas/Olle) ?

---

_Source: [https://wiki.freepascal.org/Detailed_2.0.4_Todo](https://web.archive.org/web/20230603081743/https://wiki.freepascal.org/Detailed_2.0.4_Todo)_
