# Unix RTL to do list

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This page is only very occasionally updated, to keep track of very long term todos

  * ~~check IOPerm and IOPL for all platforms~~ remember doing a bit, but without portio, there is not much need on other targets.
  * [ Unix RTL Dev random notes](<Dev_random.md> "Dev random")
  * ~~Mac OS X compability IPC unit (when lag on Cider lessens, mostly done)~~ Jonas worked on it in 2.2 (2.4?) times, I did some common cleanups.
  * (libc) Directly redirect pipe functions in unit Unix to libc, instead of implement them on top of pipe()
  * ~~64-bit filesystem support.~~ default on unix. Afaik 2.6 got a lot of unix/sysutils etc files adapted.
  * ~~FPC_USE_LIBCify netdb~~ cnetdb in 2.4
  * ~~KQueue~~
  * most 1.0.x deprecations now done, 3.2 gets rid of linux stat union.

---

_Source: [https://wiki.freepascal.org/Unix_RTL_to_do_list](https://web.archive.org/web/20250122071545/https://wiki.freepascal.org/Unix_RTL_to_do_list)_
