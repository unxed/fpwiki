# System unit

│ **English (en)** │  **[français (fr)](</System_unit/fr> "System unit/fr")** │    
****

In Free Pascal the **System** unit is the unit that is included by _every_ program. FPC's [run-time library](<RTL.md> "RTL") comes with a System unit where most, if not all of its functionalities work on every available [platform](<Platform_list.md> "Platform list"). 

Also depending on the platform it's not true that no further unit is added, because for example on Windows the **SysInit** unit is also used, because that contains the entry point for the binary. 

## Contents

  * 1 Compilation of System unit
    * 1.1 Mandatory tasks of System unit
    * 1.2 Initialization and finalization of units
    * 1.3 Other issues with compilation
  * 2 Example of System unit
  * 3 Comparative remarks
  * 4 See also



## Compilation of System unit

For quite some time already FPC automatically applies the -Us switch to the unit named System, so you don't need the -Us switch. The -Us switch tells the compiler that only basic type definitions are being made. Types such as [Integer](<Integer.md> "Integer") are not yet available. 

### Mandatory tasks of System unit

If you attempt to create your own system unit, you'll incrementally notice what the compiler needs or what the generated code expects. On a Linux target, the minimal system unit has to contain: 
    
    
    unit system;
    
    interface
    

The type **hresult** must be defined, for example: 
    
    
    type
      hresult = LongInt;
    

The identifier **operatingsystem_result** has to be declared. The data stored at that position are communicated to the operating system as return value. Whether **hresult** and **operatingsystem_result** are defined in the "interface" section is irrelevant. However, it is very plausible to put those into the "interface" section, so programs using this system unit can manipulate those. 
    
    
    var
      ExitCode: hresult = 0; export name 'operatingsystem_result';
    
    implementation
    

Here comes to light what the system responsibilities are: It has to (or should) initialize units, that means call every unit's [Initialization](<Initialization.md> "Initialization") routines. For that the label **FPC_INITIALIZEUNITS** has to be defined. 
    
    
    procedure InitializeUnits; alias: 'FPC_INITIALIZEUNITS'; 
    begin
    end;
    

Also the label **FPC_DO_EXIT** has to be defined. Here, system unit programmers have the chance to finalize units. 
    
    
    procedure doExit; alias: 'FPC_DO_EXIT';
    begin
    end;
    
    end.
    

Furthermore FPC will possibly complain the unit FPIntRes is missing. So create a file fpintres.pas: 
    
    
    unit FPIntRes;
    interface
    implementation
    end.
    

After doing so, you can compile your system unit (flag -Us is not needed), or directly create an empty program and compile it to examine the impact on the binary's size. On a x86-64 Linux target the executable had a size of 2.7 Kbytes. The big trade-off is no convenience. All functionality that can't be “implemented” by the compiler is gone. For example [Addr](<Addr.md> "Addr") or [Ord](<Ord.md> "Ord") still work, while Sin() or Random() are not available. Confer the procedure **create_intern_symbols** in "compiler/psystem.pas". 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Depending on what your program uses, there might _other_ mandatory tasks, too. 

Especially OOP won't work without a functional ObjPas unit.

### Initialization and finalization of units

For those who are curious, how to implement FPC_INITIALIZEUNITS: FPC puts into every program's data section a INITFINAL table. Compiling a program with the "-al" flag will retain the Assembler file "*.s". Examining this Assembler file, you will encounter something like: 
    
    
    .section .data.n_INITFINAL
            .balign 8
    .globl  INITFINAL
            .type   INITFINAL,@object
    INITFINAL:
            .quad   1,0
            .quad   INIT$_$SOMEUNIT
            .quad   FINALIZE$_$SOMEUNIT
    

For further investigations have a glimpse at an actual implementation: ["rtl/inc/system.inc"](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/rtl/inc/system.inc>). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In general, implementing an “own” system unit is a stupid idea. There is “smart-linking” if [size matters](<Size_Matters.md> "Size Matters"). 

### Other issues with compilation

  * Q: Now FPC wants to compile the ObjPas unit.
  * A: That is because your project's mode is set to ObjFPC (Lazarus will pass a -MObjFPC parameter to the compiler then) which automatically leads to an insertion of the ObjPas unit. Either change your project's mode in Project Settings -> Compiler Settings -> Parsing or add a {$mode fpc} to your main program file.



* * *

  * Q: Now FPC cannot find this:


    
    
    Unknown compilerproc "fpc_initializeunits". Check if you use the correct run time library.
    

  * A: You need to add a


    
    
     procedure fpc_initializeunits; compilerproc;
    

both in the interface and implementation section. 

* * *

  * Q: I am stuck, FPC says:


    
    
    Internal type "TEXCEPTADDR" was not found
    

  * A: The compiler expects some system defined types to be defined when parsing a subroutine such as the implementation section of System. This means you have to add some type definitions to System. It appears that this check was added to 3.2.0.



## Example of System unit

The example is made by user Okoba [at forum](<https://forum.lazarus.freepascal.org/index.php?topic=49434.msg367677#msg367677>). 

File system.pas: 
    
    
    unit system;
    {$MODE FPC}
    interface
    
    type
      hresult = LongInt;
      DWord = LongWord;
      Cardinal = LongWord;
      Integer = SmallInt;
      UInt64 = QWord;
      TTypeKind = (tkUnknown, tkInteger, tkChar, tkEnumeration, tkFloat, tkSet,
        tkMethod, tkSString, tkLString, tkAString, tkWString, tkVariant, tkArray,
        tkRecord, tkInterface, tkClass, tkObject, tkWChar, tkBool, tkInt64, tkQWord,
        tkDynArray, tkInterfaceRaw, tkProcVar, tkUString, tkUChar, tkHelper, tkFile,
        tkClassRef, tkPointer);
    
    type
      jmp_buf = packed record
        rbx, rbp, r12, r13, r14, r15, rsp, rip: QWord;
        {$IFDEF win64}
        rsi, rdi: QWord;
        xmm6, xmm7, xmm8, xmm9, xmm10, xmm11, xmm12, xmm13, xmm14, xmm15: record 
          m1, m2: QWord;
        end;
    
        mxcsr: LongWord;
        fpucw: word;
        padding: word;
        {$ENDIF win64}
      end;
    
      Pjmp_buf = ^jmp_buf;
      PExceptAddr = ^TExceptAddr;
      TExceptAddr = record 
        buf: Pjmp_buf;
        next: PExceptAddr;
        {$IFDEF CPU16}
        frametype: SmallInt;
        {$ELSE CPU16}
        frametype: LongInt;
        {$ENDIF CPU16}
      end;
    
      PGuid = ^TGuid;
      TGuid = packed record
        case Integer of
        1:
         (Data1: DWord;
          Data2: word;
          Data3: word;
          Data4: array [0 .. 7] of byte;
        );
        2:
         (D1: DWord;
          D2: word;
          D3: word;
          D4: array [0 .. 7] of byte;
        );
        3:
        ( { uuid fields according to RFC4122 }
          time_low: DWord; // The low field of the timestamp
          time_mid: word; // The middle field of the timestamp
          time_hi_and_version: word;
          // The high field of the timestamp multiplexed with the version number
          clock_seq_hi_and_reserved: byte;
          // The high field of the clock sequence multiplexed with the variant
          clock_seq_low: byte; // The low field of the clock sequence
          node: array [0 .. 5] of byte; // The spatially unique node identifier
        );
      end;
    
    var
      ExitCode: hresult = 0; export name 'operatingsystem_result';
    
    procedure fpc_initializeunits; compilerproc;
    
    procedure fpc_do_exit; compilerproc;
    
    implementation
    
    procedure fpc_initializeunits;
    begin
    end;
    
    procedure fpc_do_exit;
    begin
    end;
    
    end.
    

File sysinit.pas: 
    
    
    unit sysinit;
    {$MODE FPC}
    
    interface
    
    procedure _FPC_mainCRTStartup; stdcall; public name '_mainCRTStartup';
    
    implementation
    
    procedure _FPC_mainCRTStartup; stdcall;
    begin
    end;
    
    end.
    

File fpintres.pas: 
    
    
    unit FPIntRes;
    {$MODE FPC}
    
    interface
    implementation
    end.
    

## Comparative remarks

Other compilers such as [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") or [Delphi](<Delphi.md> "Delphi") use similar constructs, but to varying extent, and the file is not necessarily named "system.pas". 

## See also

  * FPC RTL System unit reference
  * [System unit structure](<System_unit_structure.md> "System unit structure"), some explanation about the distributed system unit
  * [Minimal RTL](<Minimal_RTL.md> "Minimal RTL")
  * [RTL development articles](<RTL_development_articles.md> "RTL development articles")

---

_Source: [https://wiki.freepascal.org/System_unit](https://web.archive.org/web/20241125212626/https://wiki.freepascal.org/System_unit)_
