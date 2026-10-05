# ZSeries/Part 3

_[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 2](<Part_2.md> "ZSeries/Part 2") — [Go Forward to Part 4](<Part_4.md> "ZSeries/Part 4")

This continues the list from part 2 of an examination of every unit in the compiler and where changes/additions need to be made for the addition of a new CPU and/or operating system environment. 

## Contents

  * 1 Units I will have to create
    * 1.1 Unit i_vse
    * 1.2 t_s370.pas
  * 2 Other files



# Units I will have to create

## Unit i_vse

    _Called from:**compiler.pas** , includes **fpcdefs.inc** , no called units noted_

This will be related to use of the compiler under one of the standard IBM operating systems, including VS/1, VM, CMS, or possibly as a program running on the CICS transaction monitor. More on this later. 

## t_s370.pas

(The cross-compiler instructions say I'll need this. It will be in the **systems** subdirectory. Will figure out later.) It appears to be used for running the support programs such as the assembler and the linker. 

# Other files

From the description for porting the compiler, a few other files have to be edited. 

  * utils/fpcm/fpcmmain.pp - add a new entry at the end of TCpu and TOs, near line 85:



` `

`
    
    
         TCpu=(
           c_i386,c_m68k,c_powerpc,c_sparc,c_x86_64,c_arm,c_powerpc64,c_avr,c_armeb,
           c_armel,c_mips,c_mipsel,c_mips64,c_mips64el,c_s370
         );
    

``
    
    
         TOS=(
           o_linux,o_go32v2,o_win32,o_os2,o_freebsd,o_beos,o_haiku,o_netbsd,
           o_amiga,o_atari, o_solaris, o_qnx, o_netware, o_openbsd,o_wdosx,
           o_palmos,o_macos,o_darwin,o_emx,o_watcom,o_morphos,o_netwlibc,
           o_win64,o_wince,o_gba,o_nds,o_embedded,o_symbian,o_nativent,o_iphonesim,
           o_wii,o_s370
         );
    

```

`` About line 81, add the new item to these entries: ` `

`
    
    
         CpuStr : array[TCpu] of string=(
           'i386','m68k','powerpc','sparc','x86_64','arm','powerpc64','avr','armeb',
           'armel', 'mips', 'mipsel', 'mips64', 'mips64el','s370'
         );
    

``
    
    
         CpuSuffix : array[TCpu] of string=(
           '_i386','_m68k','_powerpc','_sparc','_x86_64','_arm','_powerpc64','avr',
           '_armeb', '_armel', '_mips', '_mipsel', '_mips64', '_mips64el','_s370'
         );
    

``
    
    
         ppcSuffix : array[TCpu] of string=(
           '386','m68k','ppc','sparc','x86_64','arm','ppc64','avr','armeb', 'armel',
           'mips', 'mipsel', 'mips64', 'mips64el','s370'
         );
    

``
    
    
         OSStr : array[TOS] of string=(
           'linux','go32v2','win32','os2','freebsd','beos','haiku','netbsd',
           'amiga','atari','solaris', 'qnx', 'netware','openbsd','wdosx',
           'palmos','macos','darwin','emx','watcom','morphos','netwlibc',
           'win64','wince','gba','nds','embedded','symbian','nativent',
           'iphonesim', 'wii','vse'
         );
    

``
    
    
         OSSuffix : array[TOS] of string=(
           '_linux','_go32v2','_win32','_os2','_freebsd','_beos','_haiku','_netbsd',
           '_amiga','_atari','_solaris', '_qnx', '_netware','_openbsd','_wdosx',
           '_palmos','_macos','_darwin','_emx','_watcom','_morphos','_netwlibc',
           '_win64','_wince','_gba','_nds','_embedded','_symbian','_nativent',
           '_iphonesim','_wii','_vse'
         );
    

```

`` And then this huge table is changed to the following: ` `

`
    
    
         { This table is kept OS,Cpu because it is easier to maintain (PFV) }
         OSCpuPossible : array[TOS,TCpu] of boolean = (
           { os          i386    m68k  ppc    sparc  x86_64 arm    ppc64  avr    armeb  armel  mips   mipsel mips64 misp64el, s370}
           { linux }   ( true,  true,  true,  true,  true,  true,  true,  false, true,  false, false, true,  false,  false,  true),
           { go32v2 }  ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { win32 }   ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { os2 }     ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { freebsd } ( true,  true,  false, false, true,  false, false, false, false, false, false, false, false,  false,  false),
           { beos }    ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { haiku }   ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { netbsd }  ( true,  true,  true,  true,  false, false, false, false, false, false, false, false, false,  false,  false),
           { amiga }   ( false, true,  true,  false, false, false, false, false, false, false, false, false, false,  false,  false),
           { atari }   ( false, true,  false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { solaris } ( true,  false, false, true,  true,  false, false, false, false, false, false, false, false,  false,  false),
           { qnx }     ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { netware } ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { openbsd } ( true,  true,  false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { wdosx }   ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { palmos }  ( false, true,  false, false, false, true,  false, false, false, false, false, false, false,  false,  false),
           { macos }   ( false, false, true,  false, false, false, false, false, false, false, false, false, false,  false,  false),
           { darwin }  ( true,  false, true,  false, true,  true,  true,  false, false, false, false, false, false,  false,  false),
           { emx }     ( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { watcom }  ( true,  false, false, false ,false, false, false, false, false, false, false, false, false,  false,  false),
           { morphos } ( false, false, true,  false ,false, false, false, false, false, false, false, false, false,  false,  false),
           { netwlibc }( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { win64   } ( false, false, false, false, true,  false, false, false, false, false, false, false, false,  false,  false),
           { wince    }( true,  false, false, false, false, true,  false, false, false, false, false, false, false,  false,  false),
           { gba    }  ( false, false, false, false, false, true,  false, false, false, false, false, false, false,  false,  false),
           { nds    }  ( false, false, false, false, false, true,  false, false, false, false, false, false, false,  false,  false),
           { embedded }( true,  true,  true,  true,  true,  true,  true,  true,  true , false, false, false, false,  false,  false),
           { symbian } ( true,  false, false, false, false, true,  false, false, false, false, false, false, false,  false,  false),
           { nativent }( true,  false, false, false, false, false, false, false, false, false, false, false, false,  false,  false),
           { iphonesim }( true,  false, false, false, false, false, false, false, false, false, false, false, false, false,  false),
           { wii }     ( false, false, true,  false, false, false, false, false, false, false, false, false, false,  false,  false),
           { vse }     ( false, false, false, false, false, false, false, false, false, false, false, false, false,  false,  true)
         );
    

```

``

I compiled PP under Lazarus using parameters **-dwin32 -di386** and it compiles without error, so the changes have not damaged the original compiler. The next thing to do is to see what happens when trying it with **-ds370 -dlinux** or **-ds370 -dvse** and see if it fails, crashes or just does nothing. 

It does nothing. It compiles okay with both of these. Of course, the question now is where it would go to start code generation. That will take a while. 

    _[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 2](<Part_2.md> "ZSeries/Part 2") — [Go Forward to Part 4](<Part_4.md> "ZSeries/Part 4")

An editor has declared this article to be a stub, meaning that it needs more information. Can you help out and [add some](<Part_3.md>)? If you have some useful information, you can [help](</Help:Editing> "Help:Editing") the Free Pascal Wiki by clicking on the edit box on the left and expanding this page.

---

_Source: [https://wiki.freepascal.org/ZSeries/Part_3](https://web.archive.org/web/20250207042725/https://wiki.freepascal.org/ZSeries/Part_3)_
