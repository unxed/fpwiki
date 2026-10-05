# Cross compiling OSX on Linux

## Contents

  * 1 Cross compiling for macOS on Linux
    * 1.1 Requirements
    * 1.2 STEP 1: copy the SDK from your Mac
    * 1.3 STEP 2: grab odcctools from SVN
    * 1.4 STEP 3: rebuild FPC
    * 1.5 STEP 4: configure fpc.cfg
    * 1.6 STEP 5: build the Carbon LCL
    * 1.7 Gotcha's
  * 2 Source
  * 3 Older instructions
  * 4 See also



## Cross compiling for macOS on Linux

### Requirements

What you'll need: 

  * an Intel Mac running Leopard, Snow Leopard, or Lion
  * Xcode installed on your Mac or the original or retail operating system DVD (for the SDK)
  * a working Linux setup (tested with Debian Squeeze)
  * up to date source for FPC (tested with 2.4.x) and Lazarus (tested with 0.9.30)
  * the Open Darwin cctools (odcctools)



[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These instructions are for i386 Linux; trying this on x64 Linux has failed. Update 2013-12-13: cross compiling from Linux for macOS no longer works at all for 10.8 and above. The compiler works fine but viable linker tools (cctools for linux) are not available for Linux anymore. Please adjust instructions if you find a solution for this. The cctools are available in Arch Linux. 

### STEP 1: copy the SDK from your Mac

You need to install Xcode from your operating system DVD if you've not already done so and copy the SDK to an appropriate location on your Linux box. 

I'd recommend Leopard 10.5.sdk (compatible with Leopard) but it depends on how far back you want to be compatible. The 10.5.sdk is located in /Developer/SDKs on the Mac. I put mine at /opt/Mac/ on my Debian box. Example using ssh from your Linux box to copy over the files: 
    
    
    su - #become root
    cd /opt
    # all users should have read access to /Developer/SDKs...
    ssh appleuser@applehostname "cd /Developer/SDKs && tar cz MacOSX10.5.sdk" | tar xz
    

### STEP 2: grab odcctools from SVN

Odcctools apparantly provide binutils for OSX/Darwin. 

On 64 bit Linux environments, fpc mailing list users have problems with this step. Perhaps this [StackOverflow question and answer](<http://stackoverflow.com/questions/3024255/building-odcctools-in-a-64-bit-os>) can help: in the configure step, you'd apparently need to set CC="gcc -m32" CXX="g++ -m32" ./configure blah blah blah 
    
    
    aptitude install subversion #if it is not installed already
    mkdir ~/svn #store our repositories here, change according to taste
    cd ~/svn
    svn checkout http://svn.macosforge.org/repository/odcctools/trunk/ odcctools
    

and build it as 
    
    
    cd ~/svn/odcctools
    bash extract.sh #this creates ~/svn/odcctools/odcctools. Fun, right?
    #if you do not run it in bash, it might run in a c shell and complain
    #about pushd not being there, not applying patches, and all kinds of nastiness.
    cd ~/svn/odcctools/odcctools
    #configure:
    CC=gcc-4.4 ./configure --target=i386-darwin \
     --prefix=/opt/odcctools --with-sysroot=/opt/MacOSX10.5.sdk
    #compile and install if succesful:
    make && make install
    

This installs the tools in /opt/odcctools. I've specified gcc 4.4 but it should be okay with other versions. 

### STEP 3: rebuild FPC

(my sources are in ~/hg/pascal) 
    
    
    cd ~/hg/pascal 
    #if you use hg/mercurial you can see release version:
    hg id -bint
     730fd5ffbeb1 16666 fixes_2_4 release_2_4_2
    make distclean && FPC=ppc386 make crossall crossinstall \
     CPU_TARGET=i386 OS_TARGET=darwin \
     CROSSBINDIR=/opt/odcctools/bin BINUTILSPREFIX=i386-darwin- \
     INSTALL_PREFIX=/opt/cross \
     OPT="-gl -gw -godwarfsets -XX -CX -Xd -Fl/opt/MacOSX10.5.sdk/usr/lib"
    

Note that the options (OPT) as shown are vital, especially -gw. 

### STEP 4: configure fpc.cfg

Add a darwin (cross-compile) clause to /etc/fpc.cfg: 
    
    
    #IFDEF darwin
    -Fu/opt/cross/lib/fpc/$fpcversion/units/i386-darwin/
    -Fu/opt/cross/lib/fpc/$fpcversion/units/i386-darwin/*
    -Fu/opt/cross/lib/fpc/$fpcversion/units/i386-darwin/rtl
    -FD/opt/odcctools/bin
    #ENDIF
    

### STEP 5: build the Carbon LCL

Be sure to specify the Darwin OS target, i386 CPU target and, most importantly, add the -gw option. Perform a Clean+Build of the LCL and the Package Registration. 

You should now be able to use Lazarus in Linux to build for macOS. 

### Gotcha's

There are two more gotcha's when cross-compiling to macOS: 

  * Be sure to specify the -gw in your projects to avoid problems



reported in (the unfixable) FPC bug #12001. 

  * Be sure to specify the -XR option pointing to your SDK root (e.g. -XR/opt/Mac/Leopard10.5.sdk), or the Darwin linker will try to link to the wrong startup object (/usr/lib/crt1.o).



## Source

Fpc Mailing list 6 August 2011 post by Bruce titled "Re: Cross Compiling from Linux to Leopard 10.5 or Snow Leopard 10.6 target. How? [SOLVED]" Adapted by BigChimp 

## Older instructions

This section was taken from the general [Cross compiling](<Cross_compiling.md> "Cross compiling") page and may still be of interest: 

  * First you need the binutils for the platform you want to compile to. Download odcctools from this site (use the cvs version) and follow their instructions for installing. <http://www.opendarwin.org/projects/odcctools/>
  * you need to create a fake root dir like: $HOME/darwinroot copy at least the /System and /Frameworks and /usr directories (you may have to copy more than this) from your Apple or Darwin computer to $HOME/darwinroot
  * now that you have these files make a folder in $HOME/darwinroot called cross. where ever you installed the odcctools you need to make links for the cross tools to be more fpc friendly. there are a bunch of files from odcc tools called powerpc-apple-darwin-* you need to make links (or rename them) so powerpc-apple-darwin-ld becomes powerpc-darwin-ld, do the same for *-ar and *-as.
  * now you are ready to crosscompile fpc. basically you need to have the fpc source and have a terminal open there.



type: 
    
    
    $PATH=$PATH:$HOME/darwinroot/cross (or whereever you made the symlinks)
    

type (iirc): 
    
    
    make all TARGET_OS=darwin TARGET_CPU=powerpc OPT="-Xd -Fl/$HOME/darwinroot/usr/lib"
    

if that succeded you can install it to whereever you want with: 
    
    
    make install TARGET_OS=darwin TARGET_CPU=powerpc PREFIX=/cross/fpc
    

now copy the file ./compiler/ppccross somewhere you will be able to find it as it's the compiler you'll need to build powerpc programs 

  * configure your /etc/fpc.cfg file.



add a section like this: 
    
    
    #IFDEF powerpc
    -Fu/cross/fpc/lib/fpc/$fpcversion/units/$fpctarget/
    -Fu/cross/fpc/lib/fpc/$fpcversion/units/$fpctarget/rtl
    -Fu/cross/fpc/lib/fpc/$fpcversion/units/$fpctarget/*
    -k-systemroot $HOME/darwin/cross
    #ENDIF
    

whenever you want to crosscompile you have to have ppccross and the symlinks to powerpc-darwin-* in the PATH and you should be able to just do ppccross someprogie.pas and it will create a darwin executable. 

I may have missed some things (or most everything) as it's been a while since I did this. 

## See also

  * [Cross compiling](<Cross_compiling.md> "Cross compiling") for cross compiling from Linux, macOS and Windows along with other useful cross-compilation information.

---

_Source: [https://wiki.freepascal.org/Cross_compiling_OSX_on_Linux](https://web.archive.org/web/20250319154119/https://wiki.freepascal.org/Cross_compiling_OSX_on_Linux)_
