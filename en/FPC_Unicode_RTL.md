# FPC Unicode RTL

## Contents

  * 1 The issue
  * 2 The solution
    * 2.1 Using unicode strings
    * 2.2 Using namespaced units
    * 2.3 What about the compiler itself?
  * 3 Compiling the Unicode RTL
  * 4 Compiling a namespaced RTL
  * 5 Compiling the namespaced unicode RTL



## The issue

The Unicode RTL is part of an effort to create a more recent Delphi compatible RTL in Free Pascal. 

Currently, 2 points make creating code that will compile in Delphi and in Free Pascal difficult. 

  * In the Delphi RTL/VCL, the string keyword means WideString (or UnicodeString): an array consisting of 2-byte characters.



    

    In Free Pascal, String is still a single-byte character as it was before Delphi 2009..

  * In Delphi, units have a namespace: _System.SysUtils_ , _VCL.Forms_ etc.



    

    In Free Pascal, the unit names still conform to the Delphi 7 era.

Free Pascal has backwards compatibility high in the list of priorities, so a complete switch from one to the other is not possible. 

## The solution

2 RTLs and packages will be created from the same codebase: 

  * The backwards compatible RTL (no namespaces, single-byte string)
  * The _Unicode RTL_ , which will be Delphi 2009 and upwards compatible as much as feasible.



The default RTL will still be the Free Pascal, backwards compatible, RTL. 

### Using unicode strings

The Free Pascal codebase for the RTL, packages and utilities is cleaned up: 

  * The compiler now defines 2 basic types: _AnsiChar_ (1 byte) and _UnicodeChar_ (2 bytes)
  * The system unit defines _Char_ as an alias for one of the 2 types, using a define: **UNICODERTL**
  * The code of RTL and packages has been revised so the "string" identifier has been removed where needed.
  * In places where the code assumes an _AnsiString_ , the **AnsiString** string type is used.
  * In places where the code is agnostic of the actual string type the **String** keyword was left in place.
  * Similarly, _Char_ has been replaced by _AnsiChar_ , _PChar_ has been replaced by _PAnsiChar_



If you have code that must compile in both RTLs, you can use the following conditional compilation mechanism 
    
    
    {IF SIZEOF(CHAR)=2}
    // Unicode string code
    {$ELSE}
    // Ansistring string code
    {$ENDIF}
    

As of end of July 2023, this work is completed. 

Note that the compiler itself needs a single-byte rtl, compiling the compiler with the 2-byte RTL is not supported at this time. 

### Using namespaced units

The Free Pascal codebase is enhanced with namespaced units: 

  * Using the system of include files, all available units will be made so that they exist in 2 versions: one namespaced, one not namespaced.



Basically, the namespaced unit will look like 
    
    
    unit Api.Mysql57dyn;
    {$DEFINE FPC_DOTTEDUNITS}
    {$i mysql57dyn.pp}
    

as you can see, the not-namespaced unit **mysql57dyn.pp** is included, and it starts with: 
    
    
    {$IFNDEF FPC_DOTTEDUNITS}
    unit mysql57dyn;
    {$ENDIF FPC_DOTTEDUNITS}
    

The occasion has been used to make the unit names more consistent. While for many units, the name has been prefixed with the namespace, for equally many units, the name has been made more consistent. 

Depending on a define _fpmake_ and the _Makefiles_ will compile either the one-byte RTL or the Unicode RTL, never both together. 

### What about the compiler itself?

The compiler itself does not use unicode strings, and it does not use namespaced units. Therefore, if you wish to compile the compiler, you must still use the traditional Free Pascal RTL. 

This is also the reason why the unicode and/or namespaced RTL and packages must be compiled separately. 

## Compiling the Unicode RTL

If you wish to use the 1-byte RTL, you don't need to do anything special: the makefiles will still by default create the backwards compatible RTL. 

The compilation of the unicode RTL rests on the use of [Subtarget support](<FPC_Subtarget_Support.md> "FPC Subtarget Support"). This means you can only create it using a version 3.3.1 or higher of FPC. 

The first step is to create a configuration file next to the standard free pascal config file, called **fpc-unicodertl.cfg** : 
    
    
    -dUNICODERTL
    -Municodestrings
    

When used, this configuration file does 2 things: 

  1. It defines UNICODERTL. This can be used to make the difference (where needed) between the 2 rtls.
  2. It sets the "string" type to UnicodeString.



To compile the unicode RTL is is now sufficient to execute the following commands (replace ppcx64 with the compiler of your choice): 
    
    
    make -C rtl clean all SUB_TARGET=unicodertl PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages clean all SUB_TARGET=unicodertl OPT=-dUSEWIDESTRING PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils clean all SUB_TARGET=unicodertl PP=/path/to/fpc/3.3.1/ppcx64
    

(the **-dUSEWIDESTRING** is needed for the compilation of the regular expressions unit) 

To install, subsequently execute 
    
    
    make -C rtl install SUB_TARGET=unicodertl PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages install SUB_TARGET=unicodertl PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils install SUB_TARGET=unicodertl PP=/path/to/fpc/3.3.1/ppcx64
    

At this point, this will create a non-namespaced unicode rtl. 

To compile your own code with the unicode RTL, you must then of course specify the unicodertl subtarget: 
    
    
    fpc -tunicodertl yourproject.pas
    

## Compiling a namespaced RTL

To compile a namespaced URL, it is sufficient to specify the ""FPC_DOTTEDUNITS=1"" define to the makefiles: 
    
    
    make -C rtl clean all FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages clean all FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils clean all FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    

To install, subsequently execute 
    
    
    make -C rtl install FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages install FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils install FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    

## Compiling the namespaced unicode RTL

To compile a namespaced URL, it is sufficient to combine the techniques of the above 2 sections: specify the **SUB_TARGET=unicodertl** and the **FPC_DOTTEDUNITS=1** defines when calling make: 
    
    
    make -C rtl clean all SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages clean all SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils clean all SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    

To install, subsequently execute 
    
    
    make -C rtl install SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C packages install SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64
    make -C utils install SUB_TARGET=unicodertl FPC_DOTTEDUNITS=1 PP=/path/to/fpc/3.3.1/ppcx64

---

_Source: [https://wiki.freepascal.org/FPC_Unicode_RTL](https://web.archive.org/web/20240809002509/https://wiki.freepascal.org/FPC_Unicode_RTL)_
