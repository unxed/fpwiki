# GameBoy Advance

GBA port is still in an initial stage, though it should be pretty usable. Initial compiler port was by Jason McMillen. New compiler and RTL port was started and mantained by Francesco Lombardi. 

## Contents

  * 1 GameBoy Advance port
    * 1.1 Status
    * 1.2 Port notes
    * 1.3 Building compiler
    * 1.4 Binaries
    * 1.5 Documentation
    * 1.6 Links



# GameBoy Advance port

## Status

  * The 2.1.x compiler has compiler support for Gameboy Advance.
  * ARM CPU is supported.
  * ASM THUMB mode is not supported.
  * Base RTL units *should* be complete.



  


## Port notes

FPC does not have some features you will find in gcc-aimed source code for gba. 

  * At this time, you can't use THUMB asm instruction set
  * It is not possible to relocate variables in pascal source directly. In gcc you can write:


    
    
    __attribute__ ((section (".mySection"))

A workaround is declaring the variable in an external asm file: 
    
    
      @ external file ExtVars.s
       .section .mySection
       .global myVar
    ...
    

then declare it in pascal source code as external: 
    
    
    ...
    {$L ExtVars.o}  // You need to assemble ExtVars.s and link it
    var 
      myVar: integer; cvar; external;
    ...
    

  * FPC does not allow `volatile` declarations. Just ignore it, because the fpc compiler does not handle such an optimization.
  * The GCC preprocessor is a bit more complex than the fpc one. All #define that require a parameter can be safely replaced by an inline procedure, declaring the parameter as an integer:


    
    
    // GCC preprocessor define...
    #define SCREEN_BASE(m) ((m) << 8)
    
    // ...in fpc becomes:
    function ScreenBase(m: integer): integer; inline;
    begin
      ScreenBase := m shl 8;
    end;
    

## Building compiler

_In this document I will refer to a windows system. It is possible to download a[batch file](<http://itaprogaming.free.fr/download/build_fpc4gba.bat>) that could help in the building and installation process._

In order to build fpc for gba you need a working copy of fpc binaries. Be sure that fpc path (eg. `c:\fpc\bin\i386-win32`) is at the first place in your search path (because compatibility problems with eg. Delphi `make.exe`) 

  
**Step 1** \- Getting Cross Binutils 

You need cross binutils for arm-gba. You can get it (for win32) from <http://itaprogaming.free.fr/download/arm-gba-binutils.zip>. Extract them to some dir in the path on your machine. We will suppose that you extracted the cross binutils to: `%FreePascal%\bin\arm-gba` where %FreePascal% is the path of your fpc binary installation. 

  
**Step 2** \- Making Cross compiler 

Now you need to build fpc cross compiler for ARM processor. To do that you need the latest fpc 2.1.x sources and a working installation of fpc 2.0.x. 

Get the latest 2.1.x source from Subversion repository: <http://www.freepascal.org/develop.html#svn>

We will suppose that your Free Pascal 2.1.x source code is located here: `%FreePascal%\sources`

  
**Step 3** \- The Build process 

In order to build the cross compiler it is necessary to have a correct `PATH` environment variable pointing to cross binutils. In Windows You should add `%FreePascal%\bin\arm-gba` to the PATH. Now open a dos prompt, go to `%FreePascal%\source\compiler` and run 
    
    
      make PPC_TARGET=arm
    

At the end of building process, you should have a `ppcarm.exe` file in `%FreePascal%\source\compiler` directory. 

Now copy these files to `%FreePascal%\bin\arm-gba`. 

In the dos prompt go to `%FreePascal%\source\rtl\gba` and do 
    
    
      make CPU_TARGET=arm OS_TARGET=gba PP=ppcarm OPT="-Tgba"
    

At the end of the compiling process, you can find a new directory called `%FreePascal%\source\rtl\units\arm-gba` Now copy the directory 'arm-gba' and all files inside in `%FreePascal%\units`

  
**Step 4** \- Configuration file 

Now you need to create `fpc.cfg` configuration file in `%FreePascal%\bin\arm-gba` folder in order to use `ppcarm.exe` easily. 

Create an empty `fpc.cfg` file in `%FreePascal%\bin\arm-gba` folder and add the following lines to it: 
    
    
    -Tgba
    -FuD:\freepascal\units\arm-gba
    -XParm-gba-
    -FDD:\freepascal\bin\arm-gba
    

Of course you should replace `D:\freepascal` with your fpc installation directory. 

  
**Done!**

Our job is done! Now you should have a freepascal compiler that can build apps for gba target. Though it is not just about calling "ppcarm yourfile.pp"... First of all, you need to copy `lnkscript` and `prt0.s` (that you have found in `%FreePascal%\source\rtl\gba`) in your project directory. In order to make things a bit simpler, copy following text in a batch file: 
    
    
     @echo off
     REM "placeholder" should be changed according with your file name
     
     ppcarm -gw -s -XX -Xs -Si -Ooregvar placeholder.pp
     arm-gba-as -o prt0.o prt0.s
     arm-gba-as -o placeholder.o placeholder.s
     arm-gba-ld -g --gc-sections -T lnkscript -L. link.res -o placeholder.elf
     arm-gba-objcopy -v -O binary placeholder.elf placeholder.gba
     
     pause
    

  
**Next steps?**

In the SVN repository there is gbaunits, a package that incapsulates a library I have translated from c, but it needs some changes and some testing too. About prt0.s, lnkscript and binutils: these files come from [devkitPro](<http://www.devkitpro.org>). 

## Binaries

A zip package of arm-gba binaries is available [here](<ftp://ftp.freepascal.org/pub/fpc/snapshot/v21/arm-gba/fpc4gba.zip>) or mirrored on my [web site](<http://itaprogaming.free.fr/download/fpc4gba.zip>)

  


## Documentation

  * [FPC 4 GBA Game Programming Tutorial](<http://itaprogaming.free.fr/tutorial.html>): a work-in-progress tutorial I'm writing about gba programming with fpc
  * [Programming The Nintendo Game Boy Advance](<http://www.jharbour.com/gameboy/default.aspx>): a free book for beginners about GBA game programming.
  * [TONC](<http://user.chem.tue.nl/jakvijn/tonc/>): if you want to program a game for gba, you MUST read this tutorial.
  * [FPC4GBA initiative site](<http://fpc4gba.pascalgamedevelopment.com>): Here you can find more infos on FPC 4 GBA.
  * [GBA FAQ](<http://www.devrs.com/gba/files/gbadevfaqs.php>): A lot of obscure GBA internal infos.



## Links

  * [My home page](<http://itaprogaming.free.fr>) with some tools and demos.
  * [GBADev](<http://www.gbadev.org>): a great GBA developers community.
  * [GBA Devr's](<http://www.devrs.com/gba>): Jeff Frohwein's GBA related stuff.
  * [Pascal Game Development](<http://www.pascalgamedevelopment.com>): the biggest pascal game development community.
  * [devkitPro](<http://www.devkitpro.org>): the home page of the GBA development toolkit.

---

_Source: [https://wiki.freepascal.org/GameBoy_Advance](https://web.archive.org/web/20241205154511/https://wiki.freepascal.org/GameBoy_Advance)_
