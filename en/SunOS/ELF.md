# SunOS/ELF

The SunOS/ELF aka Solaris port is in maintenance mode. 

## Status

  * The SPARC/Solaris port works fairly well, and nightly testsuite runs keep it that way. It is tested occasionally (not under active development), see [Lazarus on Solaris](<../Lazarus_on_Solaris.md> "Lazarus on Solaris").



## Useful to know

  * FPC/Solaris currently uses the GNU Tool chain. <http://www.blastwave.org/> provides a prepackaged packages with a usable package manager for Solaris.
  * <http://www.sunfreeware.com> also has the required GNU packages (make, libintl, gdb, binutils). Its assembler and linker are called gas/gld in order not to conflict with Sun's as/ld, but the compiler first search for these names. Remember to also use gmake rather than make when building FPC.
  * The equivalent to strace/ktrace is called truss on Solaris.

---

_Source: [https://wiki.freepascal.org/SunOS/ELF](https://web.archive.org/web/20250422001032/https://wiki.freepascal.org/SunOS/ELF)_
