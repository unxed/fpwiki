# FreeBSD

## Contents

  * 1 FreeBSD bleeding edge notes
    * 1.1 Supported FreeBSD versions (12.x..15-CURRENT)
      * 1.1.1 Required kernel options for non GENERIC kernels
      * 1.1.2 FPC 3.3.1 (main)
  * 2 Ports tree
    * 2.1 Official FPC Installer
    * 2.2 Building FPC Main
    * 2.3 Issues
  * 3 Older versions of FPC and FreeBSD
    * 3.1 Support for older versions
    * 3.2 COMPAT_ requirement of the port
    * 3.3 FPC_USE_LIBC
    * 3.4 Unix RTL, 1.0.x/1.9.x RTL compability
  * 4 See also



## FreeBSD bleeding edge notes

This page is mainly a scratchpad where I will try to keep FreeBSD specific issues noted. Some of them may apply to NetBSD/OpenBSD, too. Darwin (macOS) is also sharing a lot of the generic BSD code. 

FreeBSD was the most mature of the BSD ports, though Darwin (macOS) has now caught up. 

### Supported FreeBSD versions (12.x..15-CURRENT)

FreeBSD is currently targeted at versions 12.x, 13.x, 14.x and 15-CURRENT. 

#### Required kernel options for non GENERIC kernels

The FreeBSD pkg installation and compilation from /usr/ports both include FPC compiler binaries which core dump on FreeBSD 12.4, 13.2, 14.0 and 15.0-CURRENT as follows: 
    
    
    $ truss ./work/ppcx64-3.0.4-freebsd
    sigaction(SIGFPE,{ 0x4224f0 SA_SIGINFO ss_t },{ SIG_DFL 0x0 ss_t }) = 0 (0x0)
    sigaction(SIGSEGV,{ 0x4224f0 SA_SIGINFO ss_t },{ SIG_DFL 0x0 ss_t }) = 0 (0x0)
    sigaction(SIGBUS,{ 0x4224f0 SA_SIGINFO ss_t },{ SIG_DFL 0x0 ss_t }) = 0 (0x0)
    sigaction(SIGILL,{ 0x4224f0 SA_SIGINFO ss_t },{ SIG_DFL 0x0 ss_t }) = 0 (0x0)
    ioctl(1,TIOCGETA,0x7fffffffe4a0)         = 0 (0x0)
    ioctl(2,TIOCGETA,0x7fffffffe4a0)         = 0 (0x0)
    ioctl(1,TIOCGETA,0x7fffffffe4a0)         = 0 (0x0)
    ioctl(2,TIOCGETA,0x7fffffffe4a0)         = 0 (0x0)
    compat6.mmap()                           ERR#78 'Function not implemented'
    SIGNAL 12 (SIGSYS) code=SI_KERNEL
    process killed, signal = 12 (core dumped)
    

**Solution**

Use a GENERIC kernel, or for FreeBSD 11.x ensure the following kernel options are uncommented in your KERNCONF file: 
    
    
    options       COMPAT_FREEBSD6         # Compatible with FreeBSD6
    options       COMPAT_FREEBSD7         # Compatible with FreeBSD7
    options       COMPAT_FREEBSD10        # Compatible with FreeBSD10
    
    
    
    _Note 1: COMPAT_FREEBSD7 was necessary otherwise the kernel would fail to build once COMPAT_FREEBSD6 was included._
    _Note 2: COMPAT_FREEBSD11 is also be required for FreeBSD 12.0._
    _**Note 3: Nowadays, lang/fpc port doesn't need COMPAT <= 11 dependencies. It should fix problems when GENERIC kernel is not used (unless compiling trunk from source).**_
    

It seems the FPC binaries are calling some very old FreeBSD system calls dating back some 14 years. 

#### FPC 3.3.1 (main)

Currently, FreeBSD is enable to compile the source for FPC 3.3.1 from ports tree. Take a look at: <https://www.freshports.org/lang/fpc-devel/>

## Ports tree

FPC is currently available as 3.2.2 and 3.3.1 in the ports tree, maintained by Alonso Cárdenas Márquez (acm_at_FreeBSD.org). 

### Official FPC Installer

Although the ports collection is brilliant, there are alternative methods of installing FPC too - installing from the official *.tar release packages. 

  1. Downloading the official *.tar release from [SourceForge.net](<http://sourceforge.net/projects/freepascal/files/FreeBSD/>)
  2. Unpack the .tar package into a temporary directory 

    
         
         tar xvf fpc-3.0.0.x86_64-freebsd10.tar

  3. Run the setup program 

    
         
         ./install.sh

  4. Follow the on-screen prompts



### Building FPC Main

  1. Get the main (development) source code from GitLab (official repository) <https://gitlab.com/freepascal.org/fpc/source> or GitHub SubVersion mirror <https://github.com/fpc/FPCSource/>.
  2. Make sure you have a fully working **latest released FPC version** installed. It is this compiler that will initially compile the main/development FPC.
  3. gmake build OPT="-Fl/usr/local/lib"

  4. gmake install INSTALL_PREFIX=/data/devel/fpc-3.3.1/x86_64-freebsd FPC=/data/devel/fpc-3.3.1/src/compiler/ppcx64




**Note:** In the last command you reference the newly built compiler via the FPC= parameter. 

### Issues

  * gdb. After installing FreeBSD 10/11/12 and FPC, I found `gdb` version was 6.1.1. This will be found by Lazarus on first run as `/usr/libexec/gdb` and it will make debugging impossible (at least with Lazarus 1.2.x through 2.0.10). The fix is to install the latest `gdb` version by doing `pkg install devel/gdb`, and then modify the used debugger in Lazarus menu Tools->Options->Debugger, select from the list `/usr/local/bin/gdb`
  * make. On the first run Lazarus will find `/usr/bin/make` or simply make as the 'make' command. It will not work, install gmake (pkg install gmake) and change it in Lazarus menu Tools->Options->Environment->Files->Make path.
  * Remember that under FreeBSD, by default `/home` is a link to `/usr/home` which is relevant when installing Lazarus using `pkg install lazarus`, by default it will use a primary config path in `/home/username/.lazarus` but then at some point it starts mixing it as `/usr/home/user/.lazarus` which appears to cause problems only on installing packages from OPM as a normal user. Try change opm directories from /home/user to /usr/home/user using Options section.



## Older versions of FPC and FreeBSD

### Support for older versions

4.x is formally no longer supported, but the old code pretty much stayed put, the defaults just changed. The differences are pretty much startup code, threading, and some code under ifdef FREEBSD5: (note, the best chance for this to work is with the code of 2.2.2) 

  * 4.x has other startup code than 7.x, get the old .as files from 2.0.0 or 2.0.2 (or from similar versions svn). (I don't really remember what was different about them. It could only be the ABI number in the ELF ident), assemble them, and copy them over the existing ones.
  * Now bootstrap the compiler using OPT="-dFREEBSD4 -Xf"
  * copy sources + bootstrapped (static) compiler to 4.x
  * Bootstrap system (again with patched startup code and OPT="-Xf -dFREEBSD4"



Write your experiences here under this paragraph since this is all just theory till now. I do not have a fbsd4 to actually try anymore. 

### COMPAT_ requirement of the port

I haven't really researched the issue, but somehow the port maintainers added a dependancy to COMPAT_5, probably because the source default puts the .note in cprt0.as to 504000 or so. This can be easily remedied by patching the cprt0.as file before building, and put the output of 

elfdump -n `which elfdump` |awk '/FreeBSD/{print $2}' 

in the .long line AFTER a .string "FreeBSD" line in cprt0.as. 

A script has been added to -trunk (fpc/rtl/freebsd/i386/identpatch.sh) that does this. If you have improvements, please communicate them back (_to where?_). 

Note: The COMPAT_ dependency was removed with version 2.2.4 of freepascal, and the value of .long line into cprt0.as file is changed to ${OSVERSION} automatically. 

See about ${OSVERSION} at: <http://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook/dads-after-port-mk.html>

### FPC_USE_LIBC

Since jan 2004, most Unix ports of FPC can be recompiled with FPC_USE_LIBC, and in those cases the RTL will use libc functions as much as possible. This was mainly introduced for the Darwin port, and may also be used for e.g. Lazarus distributions. (Lazarus links its apps to libc anyway, because of gtk) 

  


### Unix RTL, 1.0.x/1.9.x RTL compability

The 1.0.x rtl was essentially a linux only hackish rtl. The rtl was rewritten in version 1.9.x/2.x (and this still continues), and unfortunately, compatibility had to be broken. 

For reasons and discussion, see [Unix Rtl Doc](<http://www.stack.nl/~marcov/unixrtl.pdf>)

## See also

  * [FreeBSD Portal](<Portal_FreeBSD.md> "Portal:FreeBSD")
  * [FreeBSD specific Release Engineering](<FreeBSD_specific_Release_Engineering.md> "FreeBSD specific Release Engineering")

---

_Source: [https://wiki.freepascal.org/FreeBSD](https://web.archive.org/web/20240920204230/https://wiki.freepascal.org/FreeBSD)_
