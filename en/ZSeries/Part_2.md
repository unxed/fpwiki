# ZSeries/Part 2

_[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 1](<Part_1.md> "ZSeries/Part 1") — [Go Forward to Part 3](<Part_3.md> "ZSeries/Part 3")

Because the number of places I have to look is _lots of them_ , the list of units I go through is broken up into more than one page. I'll try to keep page lengths to a reasonable factor, if a page seems to be too large, I will split it and start a new section. 

I'm also dropping references to files, units and modules that I don't have to change. This will keep the overhead low. 

## Contents

  * 1 To Begin
    * 1.1 How units and include files are listed
  * 2 The Free Pascal Compiler
    * 2.1 pp.pas
      * 2.1.1 Version 2.6.0
      * 2.1.2 Version 2.6.2
    * 2.2 fpcdefs.inc
      * 2.2.1 Version 2.6.0
      * 2.2.2 Version 2.6.2
    * 2.3 Unit globals
      * 2.3.1 Version 2.6.0
      * 2.3.2 Version 2.6.2
    * 2.4 Unit compiler
      * 2.4.1 Version 2.6.0
      * 2.4.2 Version 2.6.2
    * 2.5 Unit fksysutils
    * 2.6 Unit sysutils
      * 2.6.1 Version 2.6.0
      * 2.6.2 Version 2.6.2
    * 2.7 Unit systems
      * 2.7.1 Version 2.6.0
    * 2.8 systems.inc
    * 2.9 Unit globals



# To Begin

This compiler is _huge_. (I've (re-)compiled it, and the rebuild of the Windows version is 262,000 lines of code.) It's hundreds of source files, and is going to be an enormous task. Where do you start? Well, you start with the main program of the command-line compiler, and you look at it. That file is **pp.pas**. Then you collect all of the units it uses, and you look at each one's source file, then you repeat this recursively, finding every place where there is a reference to a particular machine type or operating system and you add the particular cpu or operating system (or both) to it. 

Note that line numbers indicated in any source file are from the version 2.6.0 compiler sources (I'll also look at the 2.6.2. sources which have just recently been released) and as such, as lines are added, other line numbers where things were found and changed will increase. So line numbers will be referenced in a file from top to bottom so the references should match. Also, so as not to brand this as "windows centric" since the hope is to build a cross-compiler for s370 that could run on either Windows or Linux, when file names are specified, directory separators will use /. 

Note that from this point on, all editing occurs in our "sandbox" directory separate from the original compiler. 

## How units and include files are listed

Since this series of articles is intended to be a tutorial on how to add a new architecture to the compiler, files that call other files or units are listed at the top of the section dealing with that file. Where a unit is called by another, that caller is listed. Same for files that are included by other files. Any units the unit or file **uses** , are also listed. As new units which are referenced are found, they are added to the list of all the units that need to be checked and inspected, and potentially edited, as well as new units that have to be created. 

This also helps since the list of modules is broken over several pages, so you can see where a module cross-references another one even if the references for both are not on the same page. 

The format of the first line is generally like this (items not used will not appear on the line): 

    _Called from:**[file names that call this file as a unit, printed in bold]**_ (or) _**main program** , included by **[files that call this file using**_**include _]_**_, includes**[files that this file calls as include files]** , requires external procedures **[non-Pascal procedures this unit uses]** , calls units **[units that this unit or file uses]** or "no called units noted" if it does not use any units_

If the file is not normally in the Compiler directory that is noted, or if it is in a subdirectory. As items are noted in the source code of that file or unit, they will be mentioned. 

# The Free Pascal Compiler

## pp.pas

### Version 2.6.0

    _**main program** , includes **fpcdefs.inc** , calls units **cmem, profile, catch, globals, compiler**_

**pp.pas** is the main program of the compiler. We're going to edit this file to create an IBM-370 cross-compiler. First, we'll decide what is the switch for this, as well as the names of the components. For the compiler switch or target environment, we'll use S370. Within the code, the cpu name is **cpu_S370** , floating-point processor is **fpu_s370** , and the operating system target is **vse** or **linux** because I have access to both environments. 

So we'll start by adding "S370" to the source comments and wherever else needed to indicate that, by inserting the middle line in the comment block (line 34): 
    
    
     VIS                 generate a compiler for the VIS
     S370                generate a compiler for the IBM 370/390/zSeries mainframes
     DEBUG               version with debug code is generated 
         
    

This program includes **fpcdefs.inc** so we'll check that later. We have to add the indication to only select one target compiler, so we'll select for I370 (about line 139, before the check that no CPU was defined:`  
`

``
    
    
    {$endif AVR}
    {$ifdef S370}
      {$ifdef CPUDEFINED}
        {$fatal ONLY one of the switches for the CPU type must be defined}
      {$endif CPUDEFINED}
      {$define CPUDEFINED}
    {$endif S370}
    {$ifndef CPUDEFINED}
      {$fatal A CPU type switch must be defined}
    {$endif CPUDEFINED}  
    
    

The rest of the main program seems to be okay, but we will have to go through and look at all the units that this program uses, which, depending on which options have been set, are or can be: **`cmem, profile, catch, globals,` and `compiler`**. Then follow those and see where they lead, and "lather, rinse, repeat." The rest of this file seems okay, so we'll save it. (If we don't change a file, we don't re-save it so as not to change the date and time of the last edit.) 

### Version 2.6.2

Line 34:  
` `

``
    
    
      VIS                 generate a compile for the VIS
    

Becomes:  
` `

``
    
    
      VIS                 generate a compiler for the VIS
      S370                generate a compiler for the IBM 370/390/zSeries mainframes
    

Lines 138 and 139:  
` `

``
    
    
    {$endif AVR}
    {$ifndef CPUDEFINED}
    

become:  
` `

`
    
    
    {$endif AVR}
    {$ifdef S370}
       {$ifdef CPUDEFINED}
         {$fatal ONLY one of the switches for the CPU type must be defined}
       {$endif CPUDEFINED}
       {$define CPUDEFINED}
    {$endif S370}
    {$ifndef CPUDEFINED}
    

```

``  


## fpcdefs.inc

    _Called as include file from**pp.pas** , called as include file from units **catch, compiler, verbose** (and virtually all other units), no called units noted_

**fpcdefs.inc** provides various definitions regarding what processor we're compiling for to most units and many other files. We need to define the processor, so we'll borrow the generic one, and add or remove items as we need them. So we'll start with the block beginning with the line `{$ifdef generic_cpu}` through the line `{$endif generic_cpu}`. From later work, I discover I'll have to mark the target machine as big endian, so I'll include that. (Note that the definitions will include things as I discover them, so this may include things I haven't explained.) 

### Version 2.6.0

So on line 146, replace 
    
    
    {$endif mips}
    
    {$IFDEF MACOS}    
     
    

With: 
    
    
    {$endif mips}
    
    {$ifdef S370}
      {$define cpu32bit}
      {$define cpu32bitaddr}
      {$define cpu32bitalu}
      {$define cpuflags}
      {$define cpuextended}
      {$define ENDIAN_BIG}
    {$endif S370}
    
    {$IFDEF MACOS}      
    

Otherwise, now, **fpcdefs.inc** looks okay, so I'll save it. 

### Version 2.6.2

Line 153, replace: 
    
    
    {$endif mips}
    
    {$IFDEF MACOS}    
     
    

With: 
    
    
    {$endif mips}
    
    {$ifdef S370}
      {$define cpu32bit}
      {$define cpu32bitaddr}
      {$define cpu32bitalu}
      {$define cpuflags}
      {$define cpuextended}
      {$define ENDIAN_BIG}
    {$endif S370}
    
    {$IFDEF MACOS}      
    

## Unit globals

### Version 2.6.0

    _Called from:**pp.pas; options.pas** , includes **fpcdefs.inc** , calls unit **comphook**_

**globals.pas** , starting at about line 372, has some definitions for specific target processors. So, again, I'll borrow from `{$ifdef GENERIC_CPU}` and change as necessary. At line 424, we have 
    
    
     {$endif mips}
    

so we'll add the 370 after this, and put in the following: 
    
    
     {$endif mips}
     {$ifdef S370}
           cputype : cpu_s370;
           optimizecputype : cpu_s370;
           fputype : fpu_s370;
     {$endif s370}
    

Note, these values will be defined elsewhere. Note that this unit references a few other units depending on the OS and machine, but the only one relevant to us will be **`comphook`** so we'll put that on the "stack" of units that have to be inspected and possibly edited. 

### Version 2.6.2

There were no changes, the above edits are the same. 

## Unit compiler

### Version 2.6.0

    _Called from:**pp.pas** , includes **fpcdefs.inc** , calls units **fksysutils, sysutils, math, verbose, comphook, systems, cutils, cfileutl, cclasses, globals, options, fmodule, parser, symtable, assemble, link, dbgbase, import, export, tokens, pass_1, wpobase, wpo, cpupara, cpupi, cgcpu, cpunode, cputarg, i_i370, globtype**_

Starting at about line 24, this unit lists the units it uses. Some units depend on other flags, but the units it does always require are: **`verbose, comphook, systems, cutils, cfileutl, cclasses, globals, options, fmodule, parser, symtable, assemble, link, dbgbase, import, export, tokens, pass_1, wpobase, wpo, cpupara, cpupi, cgcpu, cpunode,`** and **`cputarg`**. These are also added to the "stack" for checking on references. 

This unit also defines the operaring that is the target for this compiler, so we have to add a unit which will define our target environment. There are units for every one being targeted, so we have to insert one. At line 116, change: 
    
    
    {$ifdef nativent}
      ,i_nativent
    {$endif nativent}
      ,globtype;    
    

Replace with: 
    
    
    {$ifdef nativent}
      ,i_nativent
    {$endif nativent}
    {$ifdef vs1}
      ,i_vs1
    {$endif vs1}
      ,globtype;    
    

This means we've now added a new source file which will eventually have to be created, **i_vs1.pas** for the new unit **`i_vs1`** , which almost certainly will be in the I370 subdirectory. 

### Version 2.6.2

This file has not been changed, the above changes are the same. 

## Unit fksysutils

This is only used if the particular architecture sets the flag that says it does not use the **`sysutils`** and **`math`** units. Given that all of them do use them, I will presume for now I do not need to create this file. Presume every unit that includes sysutils includes this file if necessary. 

## Unit sysutils

### Version 2.6.0

    _Called from:**compiler.pas; verbose.pas; cclasses.pas; parser.pas; assemble.pas** ; located in **rtl/s370** , includes **fpcdefs.inc** , no called units noted_

This file is a local file for each architecture, so it will be in the **s370** subdirectory of the **rtl** subdirectory. Need to make an **s370** subdirectory in **rtl** as this is the first file included in it. I'll take a look at a couple of architectures to see what's expected to be included. It is used for platform dependent calls, is used by the listed units, and if there are no platform dependent calls, it will simply be a stub unit. For the moment, I'll just create a stub file and come back later ~~if~~ _when_ I need to. 
    
    
    {
    
        This file is part of the Free Pascal run time library.
        Copyright (c) 2012 by Viridian Development Corporation
    
        Sysutils unit for IBM 370/390/zSystem
    
        See the file COPYING.FPC, included in this distribution,
        for details about the copyright.
    
        This program is distributed in the hope that it will be useful,
        but WITHOUT ANY WARRANTY; without even the implied warranty of
        MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
     **********************************************************************}
    unit sysutils;
    
    interface
    implementation
    
    end.
    

Note that I'm using copyright in my company's name; this is to show that the Free Pascal team is not responsible for this code, if they want to include it as part of the regular distribution then they can have their name on it. 

### Version 2.6.2

No difference; use above. 

## Unit systems

### Version 2.6.0

    _Called from:**compiler.pas; cfileutl.pas; options.pas; parser.pas; assemble.pas** , includes **fpcdefs.inc; systems.inc** , calls unit **cutils**_

Lines 243-245: 
    
    
          systems_allow_section = systems_embedded;
    
          systems_allow_section_no_semicolon = systems_allow_section
    

replaced with: 
    
    
          systems_allow_section = systems_embedded;
    	   
          systems_s370 = [system_s370_vs1,system_s370_linux];	   
    
          systems_allow_section_no_semicolon = systems_allow_section
    

Lines 305-307 need to be altered to add the 370 (**TsystemCpu** will be adjusted to compensate): 
    
    
          cpu2str : array[TSystemCpu] of string[10] =
               (_,'i386','m68k','alpha','powerpc','sparc','vm','ia64','x86_64',_
                'mips','arm', 'powerpc64', 'avr', 'mipsel');
    

becomes: 
    
    
          cpu2str : array[TSystemCpu] of string[10] =
               (_,'i386','m68k','alpha','powerpc','sparc','vm','ia64','x86_64',_
                'mips','arm', 'powerpc64', 'avr', 'mipsel','s370');
    

At around line 851 we have to indicate the target operating system, which can be VS1 or Linux. We replace 
    
    
    {$endif mips}
    

with 
    
    
    {$endif mips}
    {$ifdef s370}
     {$ifndef default_target_set}
      default_target(system_s370_linux);
     {$define default_target_set}
     {$endif}
    {$endif s370}
    

## systems.inc

    _Called as include file from:**systems.pas** , no called units noted_

This adds the cpu types available. About line 50, change 
    
    
                cpu_mipsel                    { 13 }
    

to 
    
    
                cpu_mipsel,                   { 13 }
                cpu_s370                      { 14 }    
    

About line 67, change to add use of the gas assembler on linux, and the hlasm assembler under VS/1: 
    
    
               ,asmmode_avr_gas
    

to 
    
    
               ,asmmode_avr_gas
               ,asmmode_s370_gas
               ,asmmode_s370_hlasm
    

About line 152, change 
    
    
                system_powerpc_wii         { 70 }
    

to 
    
    
                system_powerpc_wii,        { 70 }
                system_s370_linux,         { 71 }
                system_s370_vs1,           { 72 }
    

About line 185, change 
    
    
                ,as_i386_nlmcoff
    

to 
    
    
                ,as_i386_nlmcoff
                ,as_s370_as
                ,as_s370_hlasm
    

Otherwise seems okay for now. 

## Unit globals

    _Called from:**compiler.pas; parser.pas; assemble.pas** , includes **fpcdefs.inc** , no called units noted_

**globals.pas** definitely provides direct processor-dependent information. We'll copy the default `{$ifdef GENERIC_CPU}` and change it for the i370. At about line 424 substitute 
    
    
      {$endif mips}
    

with 
    
    
      {$endif mips}
      {$ifdef s370}
            cputype : cpu_s370;
            optimizecputype : cpu_s370;
            fputype : fpu_s370;
      {$endif s370} 
    

At some point I probably should set the switches for a compiler for the i386 under windows and recompile, to be sure that my changes have not left the compiler in an unrebuildable state even if I don't implement anything for the S370 for a while. 

To make this page a reasonable size, this list continues in [part 3](<Part_3.md> "ZSeries/Part 3"). 

  


    _[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Go Back to Part 1](<Part_1.md> "ZSeries/Part 1") — [Go Forward to Part 3](<Part_3.md> "ZSeries/Part 3")

---

_Source: [https://wiki.freepascal.org/ZSeries/Part_2](https://web.archive.org/web/20250207041944/https://wiki.freepascal.org/ZSeries/Part_2)_
