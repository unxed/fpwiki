# Nintendo DS

## Contents

  * 1 Nintendo DS port
    * 1.1 History
    * 1.2 Status
    * 1.3 Port notes
      * 1.3.1 Why .nef and .nlf?
    * 1.4 What I need to start coding for Nintendo DS?
    * 1.5 Documentation
    * 1.6 Tools
    * 1.7 Using Lazarus for Nintendo DS development
    * 1.8 Links



# Nintendo DS port

## History

NDS port began and was created by Francesco Lombardi with the knowledge gained from the making of the GBA port. This endeavor is an extension of the original goals of the FPC 4 GBA project. 

## Status

  * All fpc major features are fully supported
  * ASM THUMB mode is not supported yet.
  * The compiler is built for Win32 and Linux



## Port notes

Nintendo DS can run executables made for ARM9 and/or ARM7 cpu. It is possible to switch between ARM9 and ARM7 by using 
    
    
    {$apptype arm9} 
    and
    {$apptype arm7}

That generates .nef.bin and .nlf.bin binaries. If not specified, fpc assumes arm9 as default apptype and, in this case, calls ndstool.exe (a tool you can find as part of [devkitPro](<http://www.devkitpro.org>)) in order to generate a patched binary with .nds extension. This file should work on your hardware/emulator. In case of an arm7/arm9 mixed program, it is necessary to compile separately arm7 and arm9 code, then convert the .nef.bin and .nlf.bin binaries with ndstool.exe in this way: 
    
    
    ndstool -c myprog.nds -9 myprog.nef.bin -7 myprog.nlf.bin

The current NDS port is tested and reported as working with the latest [devkitPro](<http://www.devkitpro.org>) arm-eabi binutils. 

### Why .nef and .nlf?

.nef means "not executable file" and it is the extension that no$gba debugger uses for loading arm9's synmbolic debug infos from. In the same way it loads arm7's debug infos from .nlf files. 

## What I need to start coding for Nintendo DS?

  * [Free Pascal for Nintendo DS](<http://www.freepascal.org/down/arm/nds.var>)
  * [devkitARM](<http://devkitpro.org/>) toolchain
  * [libnds](<http://devkitpro.org/>)



  


## Documentation

(_All these docs are aimed to C/C++ language_) 

  * [Dev-Scene tutorials](<http://www.dev-scene.com/NDS/Developers>): some good tutorials about Nintendo DS programming.
  * [Patater's manual](<http://patatersoft.info/manual.php>): a manual that covers topics including the legality of homebrew and the politics behind it, displaying backgrounds on both screens, sprites, and a bit of game mechanics.
  * [TONC tutorial](<http://www.coranac.com/tonc/text/toc.htm>): this is a tutorial aimed to Gameboy Advance programming, but it is perfectly adaptable to Nintendo DS. You can find a lot of tricks about optimizing your code too.
  * [Homebrew Nintendo DS Development](<http://www.double.co.nz/nintendo_ds/index.html>)
  * [The DS Wiki](<http://tobw.net/dswiki/index.php?title=Main_Page>) a Wiki aimed to Nintendo DS programming.
  * [GBATEK](<http://nocash.emubase.de/gbatek.htm>): GBA and NDS technical infos. THE Bible!



## Tools

  * [DeSmuME emulator](<http://desmume.org>): a pretty good NDS emulator. The emulator has some debugger functions and it implements a GDB stub mechanism.
  * [No$GBA emulator](<http://nocash.emubase.de/gba.htm>): at this time, it's the best NDS and GBA emulator. The emulator itself is freeware; for 15$ you can get the debugger.
  * [iDeaS emulator](<http://www.ideasemu.org/>): another good emulator. Though its level is not comparable to no$gba, it comes for windows and linux too, and has some debugger funcs.



## Using Lazarus for Nintendo DS development

A fpc 2.4.0 based distribution of Lazarus is needed. 

  * Select _Project- >New project->Program_
  * Modify the code removing the unneded parts:


    
    
    program Project1;
    {$mode objfpc}
    uses
      ctypes, nds9;
    
    begin
    
    end.
    

  * From the menu _Project- >Project options...->Compiler options_ select "NoGUI" as LCL widget type
  * Move on _Compiler options- >Code_ then select "arm" as Target CPU and "nds" as Target OS
  * Move on _Compiler options- >Other_ then check "Use additional Compiler Config file", writing the config file of your nds compiler (maybe something like c:\lazarus\fpc\2.4.0\bin\arm-nds\fpc.cfg). Lazarus will complain about some conflicting names, but just do ok and all will be fine
  * From the menu _Run- >Run parameters...->Local->Host Application_ select your emulator with full path (something like c:\desmume\desmume.exe)
  * Put $ProjPath()\$NameOnly($ProjFile()).nds on "Command line parameters" field



**What works** : Code completion and all code related features (tooltips, refactoring, ...) 

**What DOES NOT work** : the debugger (use the debugger on the emulator); the LCL (you can't make applications in the visual way. The NDS coding is somewhat similar to console application programming) 

## Links

  * [My home page](<http://itaprogaming.free.fr>) with some tools and demos.
  * [FPC4GBA initiative site](<http://fpc4gba.pascalgamedevelopment.com>)
  * [GBADev](<http://www.gbadev.org>): a great GBA developers community.
  * [Pascal Game Development](<http://www.pascalgamedevelopment.com>): the biggest pascal game development community.
  * [devkitPro](<http://www.devkitpro.org>): the home page of the GBA, NDS, Gamecube, GP32 and PSP development toolkit.

---

_Source: [https://wiki.freepascal.org/index.php/Nintendo_DS](https://web.archive.org/web/20220706213819/https://wiki.freepascal.org/index.php/Nintendo_DS)_
