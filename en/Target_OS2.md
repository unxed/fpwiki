# Target OS2

OS/2 defines three types of native applications with different behaviour: 

1) PM (Presentation Manager) - typical GUI applications (they need GUI subsystem running, i.e. no access to these from special "recovery" mode (text mode) and special setups for low HW requirements etc. Selected by $APPTYPE GUI 

2) "VIO" applications - in old days of OS/2 (1.0 - 1.2) these were the first native OS/2 applications. They were running in full screen (like in DOS, although with full multitasking and running in protected mode) and accessing the video subsystem using APIs similar to BIOS calls (roughly). This means that these could use all video BIOS capabilities without any restrictions, including any graphics modes etc. (although not accessing video directly but through the driver layer). Applications marked as "full-screen" or "FS VIO" are always started in full-screen mode by OS/2 (even if started from GUI). Applications running in full screen mode use the complete input/output layer compatible to old OS/2 1.x days, which means that they're not using the usual GUI event queues and handling. This provides maximum compatibility to old applications, although it's really hardly ever needed nowadays. Selected by APPTYPE FS (i.e. forced full-screen). 

3) "Windowable VIO" applications - text mode applications which only use the basic part of the video subsystem that can be simulated in a window. They can be still started in full-screen mode if requested by the user, of course, but it isn't required by the system. 99.5% of text-mode applications belong to this category (including all FPC applications using standard RTL libraries like Crt, Free Vision, etc.). Selected by $APPTYPE CONSOLE, users can specify starting in full screen (if they prefer to do that) either in the Program object on the Desktop, using the START command of CMD.EXE, or programmatically as a parameter of an API call DosStartSession if starting new programs from code.

---

_Source: [https://wiki.freepascal.org/Target_OS2](https://web.archive.org/web/20250114054121/https://wiki.freepascal.org/Target_OS2)_
