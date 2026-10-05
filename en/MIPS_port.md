# MIPS port

│ **English (en)** │  **[español (es)](</MIPS_port/es> "MIPS port/es")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These notes describe the situation as of circa 2.2.4. Since then a branch of 2.0.0 has been posted to Sourceforge, this has been merged back into the mainline sources and as of February 2012 is being worked on. The final result may be a usable compiler and libraries during the lifetime of 2.6.0, although it might only be available from the Subversion repository.

See [Native MIPS Systems](<Native_MIPS_Systems.md> "Native MIPS Systems") for a description of the current status. 

The Free Pascal MIPS port is under development. It generates [mipsel](<TARGET_Embedded_Mipsel.md> "TARGET Embedded Mipsel") 32-bits binaries, which should run in mips 32-bits and 64-bits processors. 

## Contents

  * 1 MIPS Architectures
  * 2 Step 1 - Downloading Cross-Binutils
    * 2.1 From Linux to Linux-MIPS
  * 3 Step 2 - Cross-compiler
    * 3.1 Building from source
  * 4 References



## MIPS Architectures

MIPS can be divided into the following targets for a code generator: 

  * MIPS 32-bits Endian Little - named mipsel
  * MIPS 32-bits Endian Big - named mips
  * MIPS 64-bits Endian Little - named mips64el
  * MIPS 64-bits Endian Big - named mips64



These names are compatible with the ones used in Linux. 

It should be noted that 64-bits MIPS processors can run 32-bits software and that Debian, for example, only offers pre-compiled CDs for 32-bits mips and mipsel, so usually one simply writes 32-bits software for MIPS or MIPS-EL, ignoring the possibility of writing 64-bits software. 

## Step 1 - Downloading Cross-Binutils

These are the basic tools necessary to create executables, such as: Linker (ld), Assembler (as), Archiver (ar) (Creates smartlinking .a files), strip and some others. 

### From Linux to Linux-MIPS

  * Debian Packages: 
    * <http://www.emdebian.org/tools/crosstools.html>
    * <http://www.emdebian.org/debian/pool/main/b/binutils/>
  * Others: 
    * <http://www.linux-mips.org/wiki/Toolchains#Debian>



## Step 2 - Cross-compiler

### Building from source

Step 1 - Install the latest release of FPC, as of June 2013 this is FPC 2.6.2 

Step 2 - Get the latest FPC sources from subversion: <http://www.freepascal.org/develop.html#svn>

Step 3 - The Build process 

**Building**

We will make the following assumptions: 

  * Your release compiler is installed here: 
        
        C:\fpc\2.6.2\
        

  * Your binutils where created here: 
        
        C:\binutils\mips\
        

(the directory which contains _mips-linux-as_ and _mips-linux-ld_
  * Your trunk source code is located here: 
        
        C:\fpc\svn
        




In order to build the cross compiler it is necessary to have a correct PATH environment variable. On Windows it is very easy to get a PATH crowded with information put by installers. To ensure that your path is correct, you should run the following commands (you can put them in a batch file or bash script if you want): 
    
    
    cd C:\fpc\svn
    PATH=C:\fpc\2.6.2\bin\i386-win32;C:\binutils\mips
    make all CPU_TARGET=mipsel OS_TARGET=linux
    

At the end of the compile you should not see any errors. 

You should now have a ppccrossmips.exe in 
    
    
    C:\fpc\svn\compiler
    

and some .o and .ppu files in 
    
    
    C:\fpc\svn\rtl\units\mipsel-linux
    

You can install these files into a normal installation of trunk (in this example C:\fpc\2.7.1) by using 
    
    
    make crossinstall INSTALL_PREFIX=C:\fpc\2.7.1
    

For a normal installation of trunk (in this example on a native Win32 host) the following steps are necessary (as documented [here](<Getting_Lazarus.md> "Getting Lazarus")): 
    
    
    make clean all install INSTALL_PREFIX=C:\fpc\2.7.1
    

## References

  * <http://sourceforge.net/projects/fpc-mips/develop>

---

_Source: [https://wiki.freepascal.org/MIPS_port](https://web.archive.org/web/20241207105257/https://wiki.freepascal.org/MIPS_port)_
