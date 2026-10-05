# Porting Free Pascal

This page is the start of a tutorial about how to port Free Pascal to different architectures and operating systems. 

## Contents

  * 1 Porting to a new Operating System
    * 1.1 Overview
    * 1.2 Adding a new target
    * 1.3 Compiling your target
  * 2 Porting to a new Architecture
    * 2.1 Step 1 - The Binutils
    * 2.2 Step 2 - Code Skeleton
  * 3 See also



## Porting to a new Operating System

### Overview

Porting Free Pascal to a new operating system involves several steps, namely: 

  1. Add a new target to the compiler
  2. Take the closest existing RTL as a start, copy it and start implementing



### Adding a new target

This task requires making small changes on several files on the compiler. You need to add your target to both the Makefiles, to the fpmake tool that creates the makefiles and the compiler itself. 

With your updated fpmake you can then recreate the makefile tree. 

Below is a example of how a new target is added. This Patch was responsible for adding SymbianOS target to the compiler. Be careful that this adds a i386-symbian target. If your target is for another architecture of course you should change the files on the directories for your particular architecture. 
    
    
    Index: compiler/compiler.pas
    ===================================================================
    --- compiler/compiler.pas	(revision 6039)
    +++ compiler/compiler.pas	(working copy)
    @@ -107,6 +107,9 @@
     {$ifdef win32}
       ,i_win
     {$endif win32}
    +{$ifdef symbian}
    +  ,i_symbian
    +{$endif symbian}
       ;
     
     function Compile(const cmd:string):longint;
    Index: compiler/i386/cputarg.pas
    ===================================================================
    --- compiler/i386/cputarg.pas	(revision 6039)
    +++ compiler/i386/cputarg.pas	(working copy)
    @@ -71,6 +71,9 @@
         {$ifndef NOTARGETWATCOM}
           ,t_watcom
         {$endif}
    +    {$ifndef NOTARGETSYMBIAN}
    +      ,t_symbian
    +    {$endif}
     
     {**************************************
                  Assemblers
    Index: compiler/systems.pas
    ===================================================================
    --- compiler/systems.pas	(revision 6039)
    +++ compiler/systems.pas	(working copy)
    @@ -136,7 +136,9 @@
                  system_x86_64_embedded,    { 55 }
                  system_mips_embedded,      { 56 }
                  system_arm_embedded,       { 57 }
    -             system_powerpc64_embedded  { 58 }
    +             system_powerpc64_embedded, { 58 }
    +             system_i386_symbian,       { 59 }
    +             system_arm_symbian         { 60 }
            );
     
            tasm = (as_none
    ===================================================================
    --- compiler/utils/ppudump.pp	(revision 20515)
    +++ compiler/utils/ppudump.pp	(working copy)
    @@ -147,7 +147,8 @@
       { 67 }  'Linux-MIPSel',
       { 68 }  'NativeNT-i386',
    -  { 69 }  'iPhoneSim-i386'
    +  { 69 }  'iPhoneSim-i386',
    +  { 70 }  'Wii-powerpc'
       );
     
     const
    Index: rtl/Makefile.fpc
    ===================================================================
    --- rtl/Makefile.fpc	(revision 6038)
    +++ rtl/Makefile.fpc	(working copy)
    @@ -28,6 +28,7 @@
     dirs_solaris=solaris
     dirs_gba=gba
     dirs_nds=nds
    +dirs_symbian=symbian
     
     [install]
     fpcpackage=y
    Index: utils/fpcm/fpcmake.ini
    ===================================================================
    --- utils/fpcm/fpcmake.ini	(revision 6038)
    +++ utils/fpcm/fpcmake.ini	(working copy)
    @@ -908,6 +908,12 @@
     SHORTSUFFIX=gba
     endif
     
    +# Symbian OS
    +ifeq ($(OS_TARGET),symbian)
    +SHAREDLIBEXT=.dll
    +SHORTSUFFIX=symbian
    +endif
    +
     else
     # long version for 1.0.x - target specific extensions
     
    Index: utils/fpcm/fpcmmain.pp
    ===================================================================
    --- utils/fpcm/fpcmmain.pp	(revision 6038)
    +++ utils/fpcm/fpcmmain.pp	(working copy)
    @@ -70,7 +70,7 @@
             o_linux,o_go32v2,o_win32,o_os2,o_freebsd,o_beos,o_netbsd,
             o_amiga,o_atari, o_solaris, o_qnx, o_netware, o_openbsd,o_wdosx,
             o_palmos,o_macos,o_darwin,o_emx,o_watcom,o_morphos,o_netwlibc,
    -        o_win64,o_wince,o_gba,o_nds,o_embedded
    +        o_win64,o_wince,o_gba,o_nds,o_embedded,o_symbian
           );
     
           TTargetSet=array[tcpu,tos] of boolean;
    @@ -88,14 +88,14 @@
             'linux','go32v2','win32','os2','freebsd','beos','netbsd',
             'amiga','atari','solaris', 'qnx', 'netware','openbsd','wdosx',
             'palmos','macos','darwin','emx','watcom','morphos','netwlibc',
    -        'win64','wince','gba','nds','embedded'
    +        'win64','wince','gba','nds','embedded','symbian'
           );
     
           OSSuffix : array[TOS] of string=(
             '_linux','_go32v2','_win32','_os2','_freebsd','_beos','_netbsd',
             '_amiga','_atari','_solaris', '_qnx', '_netware','_openbsd','_wdosx',
             '_palmos','_macos','_darwin','_emx','_watcom','_morphos','_netwlibc',
    -        '_win64','_wince','_gba','_nds','_embedded'
    +        '_win64','_wince','_gba','_nds','_embedded','_symbian'
           );
     
           { This table is kept OS,Cpu because it is easier to maintain (PFV) }
    @@ -126,7 +126,8 @@
             { wince    }( true,  false, false, false, false, true,  false),
             { gba    }  ( false, false, false, false, false, true,  false),
             { nds    }  ( false, false, false, false, false, true,  false),
    -        { embedded }( true,  true,  true,  true,  true,  true,  true)
    +        { embedded }( true,  true,  true,  true,  true,  true,  true),
    +        { symbian } ( true,  false, false, false, false, true,  false)
           );
     
         type
    

You will also need to add a i_target.pas and t_target.pas files to the compiler directory. Just copy an existing file for a similar platform and start working from there. 

If you want your target to use an internal linker (ELF, COFF, etc.) you need to add your target to that one, too. For the target i386-nativent the following change was done in the COFF output generator: 
    
    
    Index: compiler/ogcoff.pas
    ===================================================================
    --- compiler/ogcoff.pas	(Revision 14564)
    +++ compiler/ogcoff.pas	(Revision 14565)
    @@ -2906,7 +2918,7 @@
                 idtxt  : 'PECOFF';
                 asmbin : '';
                 asmcmd : '';
    -            supported_targets : [system_i386_win32];
    +            supported_targets : [system_i386_win32,system_i386_nativent];
                 flags : [af_outputbinary,af_smartlink_sections];
                 labelprefix : '.L';
                 comment : '';
    

You might also need to add your target to one of the assemblers for the target platform. E. g. if you're targeting an i386 system you might add your target like the following (the line breaks annotated with {cr} were added to keep the layout of the article intact): 
    
    
    Index: compiler/x86/agx86att.pas
    ===================================================================
    --- compiler/x86/agx86att.pas	(Revision 15151)
    +++ compiler/x86/agx86att.pas	(Arbeitskopie)
    @@ -380,7 +380,8 @@
                 asmcmd : '--32 -o $OBJ $ASM';
                 supported_targets : [system_i386_GO32V2,system_i386_linux,system_i386_Win32,{cr}
                                     system_i386_freebsd,system_i386_solaris,system_i386_beos,
                                     system_i386_netbsd,system_i386_Netware,system_i386_qnx,
                                     system_i386_wdosx,system_i386_openbsd,{cr}
    -                                system_i386_netwlibc,system_i386_wince,system_i386_embedded,{cr}
                                     system_i386_symbian,system_i386_haiku,system_x86_6432_linux];
    +                                system_i386_netwlibc,system_i386_wince,system_i386_embedded,{cr}
                                     system_i386_symbian,system_i386_haiku,system_x86_6432_linux,
    +                                system_i386_nativent];
                 flags : [af_allowdirect,af_needar,af_smartlink_sections,af_supports_dwarf];
                 labelprefix : '.L';
                 comment : '# ';
    

_Note_ : In this case the change is only needed to get the "-al" option of the compiler, which needs the GNU assembler on Windows, working for i386-nativent. 

### Compiling your target

Now that you have a target you will want to see if it works and spot compilation problems by compiling the compiler. To do this, open a console, navigate to your fpc subversion source code directory and build the compiler: 
    
    
    PATH=C:\Path_latest_fpc_stable
    cd compiler
    make arm
    

And to build your RTL: 
    
    
    cd rtl\my_new_os
    make FPC=c:\fpc_subversion\compiler\ppcarm -XP$FPCTARGET-
    

While trying to fix (or implement) RTL for your new target, information found in [System unit structure](<System_unit_structure.md> "System unit structure") may be useful. 

## Porting to a new Architecture

### Step 1 - The Binutils

The first step in porting to a new architecture is getting and understanding how the binutils of the platform work. 

### Step 2 - Code Skeleton

You can start porting Free Pascal to a new Architecture by creating a new Lazarus project and a new skeleton for it. You can get the Lazarus project for ARM for example, rename it, lets say to ppcmipsel.lpi, modify it in a text editor to refer to mips directory instead of arm and then modify it in Lazarus in the Compiler Options Dialog in the Others tab and set the compiler directive to -dmipsel instead of -darm. 

## See also

  * [Porting Free Pascal Example](<Porting_Free_Pascal_Example.md> "Porting Free Pascal Example")
  * Paul Robinson is doing a step-by-step demonstration of his attempts to create a port for the IBM 370 mainframe (the [ZSeries](<ZSeries.md> "ZSeries") using the OS/VS1 operating system, explaining how he's checking everything, and what's happening along the way. You can read about how he's coming along at [Part 1](<ZSeries/Part_1.md> "ZSeries/Part 1") of his article. (Note that the article changes as he learns more about how the compiler works.)
  * [Structure of the compiler (FPC internals)](<FPC_internals.md>)

---

_Source: [https://wiki.freepascal.org/Porting_Free_Pascal](https://web.archive.org/web/20240630213310/https://wiki.freepascal.org/Porting_Free_Pascal)_
