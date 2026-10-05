# Solaris Port

Below are some basic things how you can compile FPC, applying approximately to FPC 2.0 and Solaris 7 (SunOS 2.7). For more recent information see [Lazarus on Solaris](<Lazarus_on_Solaris.md> "Lazarus on Solaris"), which applies to 2.4 and Lazarus 0.9.30 (or later) on Solaris 10. 

Alternatively, if you have Solaris 10 Sparc or later, you might choose to use the FPC 2.6.4 binaries contributed by a user, at <http://www.bolthole.com/solaris/sparc-hints.html>

Note that cross compiling is not easy, especially not since you need to link to libc. 

Also the assembler and linker calls are not correct for assembling and linking on the target platform and may need some manual tweaking in ppas.sh and finally in t_sunos.pas. 

## Contents

  * 1 Build binutils
  * 2 Cross Compile the RTL
  * 3 Compile the famous hello world #1
  * 4 Compile the famous hello world #2
  * 5 See also



## Build binutils

Download the latest GNU binutils and build a cross-assembler and linker 
    
    
    ./configure --target=i386-pc-solaris2.7
    

## Cross Compile the RTL

In the fpc/rtl/solaris/ compile the rtl with: 
    
    
    make BINUTILSPREFIX=i386-pc-solaris2.7- OPT=-g
    

## Compile the famous hello world #1

Copy the solaris libc.* files to a dir on Linux 
    
    
    ppc386 -g -Xd -XPi386-pc-solaris2.7- -Fl<pathtosolarislibc> -Tsolaris helloworld
    

Run the executable on solaris (in gdb) 

## Compile the famous hello world #2

Another option is to compile on i386 and assemble and link on Solaris: 
    
    
    ppc386 -a -st -Tsolaris helloworld
    

This creates a ppas.sh script that can be run on Solaris. 

## See also

  * [Lazarus on Solaris](<Lazarus_on_Solaris.md> "Lazarus on Solaris")

---

_Source: [https://wiki.freepascal.org/Solaris_Port](https://web.archive.org/web/20241207105721/https://wiki.freepascal.org/Solaris_Port)_
