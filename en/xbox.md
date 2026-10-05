# xbox

Talked to sb about doing a XBox(-1) native port of FreeBasic, and thought I store it in the wiki in case sb is still interested doing something similar for xbox-1 with FPC: 

The basic concept of the xbox port is very simple: You replace the mingw stuff with openxdk stuff. 

OpenXDK works by replacing the standard libc with a special embedded version created for the xbox. After a compiler (gcc ideally, but in this case FreeBASIC) creates a win32 executable with the special library, a utility which comes with OpenXDK then converts a standard win32 PE executable into a standard XBOX PE executable. Very simple process. 

Here's how it works: 

  1. Create the xbox compilation environment by downloading mingw and msys.
  2. Use cvs to download the xbox repository.
  3. In the xbox directory, create a new copy of the rtlib sources(It'll be obvious where from the existing directory structure, and replace any reference to things not implemented in openxdk (really easy -- just compile it and read the linker error messages)
  4. The xbox version must NOT have a main() as the main loop. It must instead have a XBoxStartup or something like that. The documentation for OpenXDK will explain the exact parameters. The xbox version used to do this automatically when you compiled it, I'm not sure how badly wanked the new stuff is.
  5. The xbox version must pass a certain set of parameters to the linker. In particular, it must use a certain function call type, and it can only use libraries from openXDK. The xbox version used to do this automatically when you compiled it, but I'm not sure how badly wanked the new stuff is.
  6. After the executable is created, the xbox version must run the utility to convert the executable into an xbox executable. The xbox version used to do this automatically when you compiled it, but I'm not sure how badly wanked the new stuff is.



After compiling, you should now have the xbox version. 

This version will have the following limitation: You won't be able to use gfxlib or the console in any capacity. The console won't complain, but gfxlib will flat-out fail, probably with a huge linker error. You will need to use SDL exclusively for input and output to the screen and from user I/O devices. Once a working port is completed, I've been led to believe that gfxlib is modular enough that an SDL/newlib port could be created. 

Thanks to SJ Zero at freebasic.net.

---

_Source: [https://wiki.freepascal.org/xbox](https://web.archive.org/web/20241207122427/https://wiki.freepascal.org/xbox)_
