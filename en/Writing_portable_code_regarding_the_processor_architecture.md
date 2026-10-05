# Writing portable code regarding the processor architecture

│ **English (en)** │  **[Bahasa Indonesia (id)](</Writing_portable_code_regarding_the_processor_architecture/id> "Writing portable code regarding the processor architecture/id")** │  **[русский (ru)](<../ru/Writing_portable_code_regarding_the_processor_architecture.md> "Writing portable code regarding the processor architecture/ru")** │    
****

There are several main issues when writing code which is portable regarding the processor architecture: endianness and [32](<32_bit.md> "32 bit") vs. [64 bit](<64_bit.md> "64 bit") processors. 

## Contents

  * 1 Endianness
    * 1.1 Changing Endianness
  * 2 Alignment
  * 3 32 Bit vs. 64 Bit
  * 4 Calling conventions
    * 4.1 Mainframes
    * 4.2 x86
    * 4.3 ARM
    * 4.4 PPC
    * 4.5 68K
      * 4.5.1 References
  * 5 See also



## Endianness

Endianness refers to the way that values larger than a [byte](<Byte.md> "Byte") (e.g. 16/32/64-bit [integers](<Integer.md> "Integer")) are stored by the processor. 

Free Pascal supports processors with two types of endianness: 

  1. Store the lowest value on the lowest address; longint(4) encoded as 04 00 00 00 (_[little endian](<http://en.wikipedia.org/wiki/Little_endian>)_)
  2. Store the highest value on the lowest address; longint(4) encoded as 00 00 00 04 (_[big endian](<http://en.wikipedia.org/wiki/Big_endian>)_)



Side note: 

Middle endian, also called mixed endian, processors are rare nowadays. The best known, but now historic, middle endian processor is the [PDP-11](<http://en.wikipedia.org/wiki/PDP-11>) from DEC. 

Endianness is generally a given choice per processor family, but some families of processors can be either big endian or little endian depending on the mainboard they are attached to (ARM, PPC). 

The best known little endian processor family is x86, the processor family used in PCs, and its brethren x86-64. Typical big endian processors are PPC (usually, see above note), and [m68k](<http://en.wikipedia.org/wiki/68k>), many older systems such as the [HP3000](<http://en.wikipedia.org/wiki/HP3000>) minicomputers, and mainframes such as the [IBM 370](<http://en.wikipedia.org/wiki/IBM_370>) (Z series). 

Since TCP/IP specifies that all protocol header structures that go over the wire should be big endian, this notation is sometimes also referred to as _network order_. 

Endianness is important 

  1. when exchanging data between different architectures
  2. when accessing data sometimes as (an array of) a larger type, like integer, and sometimes as (an array of) a byte.



An example of the latter: 
    
    
    type
      Q = record
        case Boolean of
          True: (i: Integer);
          False: (p: array[1..4] of Byte);
      end;
     
    var 
      x:^Q;
     
    begin
      // First indicate what the compiler thinks we (should) be using:
      {$IFDEF ENDIAN_LITTLE}
      Writeln('The compiler has compiled this program for Little Endian machines (such as Intel x86, ARMEL)');
      {$ENDIF}
      {$IFDEF ENDIAN_BIG}
      Writeln('The compiler has compiled this program for Big Endian machines (such as PowerPC, ARMEB)');
      {$ENDIF}  
      New(x);
      x^.i := 5;
      if x^.p[1] = 5 then
        WriteLn(x^.p[1],' Your machine is Little Endian')
      else
      if x^.p[4] = 5 then
        WriteLn(x^.p[1],' Your machine is Big Endian')
      else
        WriteLn(x^.p[1],' ',x^.p[2],' ',x^.p[3],' ',x^.p[4],' Your machine''s endianness is indeterminate; please report the results to the compiler development team');  
      WriteLn;
     
      { Make it wait so we can see the results }
      Write('Press Enter when you finish reading this');
      ReadLn;
    end.
    

On little endian machines (PCs), the above code will write '5' (since longint(5) is stored as 05 00 00 00 in memory), while on big endian machines (e.g. Powermacs) it will write '0' (since longint(5) is stored as 00 00 00 05 in memory). (If you get the report your machine is indeterminate, please report on this wiki page what processor does this and what you get!) 

To determine the endianness of the processor, use the ENDIAN_BIG or ENDIAN_LITTLE (or FPC_LITTLE_ENDIAN and FPC_BIG_ENDIAN starting from version 1.9) defines that are defined by Free Pascal automatically depending on the processor. 

### Changing Endianness

Unit system has routines to convert between Big Endian and native (CPU) order if necessary ([BEtoN](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/beton.html> "doc:rtl/system/beton.html"), [NtoBE](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/ntobe.html> "doc:rtl/system/ntobe.html")), similar for Little Endian ([LEtoN](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/leton.html> "doc:rtl/system/leton.html"), [NtoLE](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/ntole.html> "doc:rtl/system/ntole.html")), as well as a [SwapEndian](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/swapendian.html> "doc:rtl/system/swapendian.html") procedure. 

Many network libraries such as Synapse have their own implementations to convert between network to host order. 

## Alignment

Some processors will allow improperly aligned data but with reduced efficiency (IBM 370/zSeries). Some processors generate hardware processor exceptions when data is badly aligned (e.g. Alpha or ARM). Sometimes the hardware exceptions are caught and fixed using emulation by the OS, but this is very slow, and should be avoided. This can also cause records to have different sizes, so always use sizeof(recordtype); as size of a record. If you define a packed record, try to ensure that data is naturally aligned, if possible. Some processors only have alignment requirements for certain types of data, like floating point (e.g. older PowerPCs). 

To check if the CPU requires proper alignment, check the FPC_REQUIRES_PROPER_ALIGNMENT (version 1.9 and higher) define. On 32 bit CPUs this usually means that data up to a size of 4 must be naturally aligned. If you want to access unaligned data, use the [move](<move.md> "move") procedure to move it to an aligned location before processing it. The move procedure takes care of unaligned data and handles it properly. 

There are multiple strategies for aligning: 

  * Align every field on a multiple of a certain value (typically a power of two, 1,2,4,8. 1 is equivalent to "packed")
  * Pad before every field such that it is aligned on a multiple of its size (so a longint on 4, an int64 on 8 bytes etc). This is typically done by the C compiler, which is why FPC calls it {$packrecords C}.



(For [arrays](<Array.md> "Array") or [nested records](</index.php?title=nested_record&action=edit&redlink=1> "nested record \(page does not exist\)"), the size of their largest sub unit is used) 

macOS {$packrecords C} seems to pad the entire record at the end to make it a certain size. This is still being investigated, and probably will be fixed in compiler. 

## 32 Bit vs. 64 Bit

To achieve maximum compatibility with older code, FPC doesn't change the size of predefined data types like `[integer](<Integer.md> "Integer")`, `[longint](<longint.md> "longint")` or `[word](<Word.md> "Word")` when changing from 32 to 64 Bit. However, the size of a pointer is 8 bytes on a 64 bit architecture so constructs like `longint([pointer](<Pointer.md> "Pointer")(p))` are doomed to crash on 64 bit architectures. However, to allow you to write portable code, the FPC system unit introduces the types `[PtrInt](</index.php?title=Ptrint&action=edit&redlink=1> "Ptrint \(page does not exist\)")` and `[PtrUInt](</index.php?title=Ptruint&action=edit&redlink=1> "Ptruint \(page does not exist\)")` which are signed and unsigned integer data types with the same size as a pointer. 

Keep in mind that the size change of the "pointer" type also affects record sizes. If you allocate records with fixed sizes, and not with [new](</index.php?title=new&action=edit&redlink=1> "new \(page does not exist\)") or with [getmem](</index.php?title=getmem&action=edit&redlink=1> "getmem \(page does not exist\)") (<x>,[sizeof](</index.php?title=sizeof&action=edit&redlink=1> "sizeof \(page does not exist\)")(<x>)), this will have to be fixed. 

This is in line with most open Unix platforms. In the commercial world, there are some exceptions like Tru64, with ILP64. 

## Calling conventions

In general: avoid relying on internal knowledge, like if a pass by const is on the stack or by value. 

Modifier | Sequence | Stack cleared by | Alignment | Registers saved?   
---|---|---|---|---  
none | left to right | function | default | none   
[Register](<register.md> "register") | left to right | function | default | none   
[CDecl](</index.php?title=cdecl&action=edit&redlink=1> "cdecl \(page does not exist\)") | right to left | caller | GCC alignment | GCC registers   
[Interrupt](</index.php?title=interrupt&action=edit&redlink=1> "interrupt \(page does not exist\)") | right to left | function | default | all registers   
[Pascal](<pascal.md> "pascal") | left to right | function | default | none   
[SafeCall](</index.php?title=safecall&action=edit&redlink=1> "safecall \(page does not exist\)") | right to left | function | default | GCC registers   
[StdCall](</index.php?title=stdcall&action=edit&redlink=1> "stdcall \(page does not exist\)") | right to left | function | GCC alignment | GCC registers   
[OldFPCCall](<oldfpccall.md> "oldfpccall") | right to left | caller | default | none   
  
See reference info for the [$CALLING](<http://www.freepascal.org/docs-html/prog/progsu87.html>) for details and additional conventions. 

Size limits of different processor architectures: 

Processor Architecture | Parameters | Local variables   
---|---|---  
i386 | 64 KiB | no limit   
AMD64/x86-64 | 64 KiB | no limit   
Motorola 68000 | 32 KiB | 32 kiB   
Motorola 68020 | 32 KiB | no limit   
PPC | no limit | no limit   
ARM | no limit | no limit   
SPARC | no limit | no limit   
  
### Mainframes

For IBM 370 and zSeries, see the [ZSeries](<ZSeries.md> "ZSeries") page for further discussion. 

### x86

On x86 processors usually all parameters are passed via the stack. In Free Pascal, however, the FastCall or Register convention is preferred (with the exception of [Darwin](<Target_Darwin.md> "Target Darwin") and [macOS](<Portal_Mac.md> "Portal:Mac")). The register convention, which is compatible with [Delphi](<Delphi.md> "Delphi"), determines that the first three ordinal or single-address parameters are passed in the registers EAX, EDX and ECX. 

### ARM

### PPC

Since the PowerPC architecture has a large number of registers most functions can pass all arguments in registers for single level calls. Additional arguments are passed on the stack, if required. For the case that multi-level calls are used and the registers must be saved additional space for register-based arguments is always allocated on the stack. 

PPC processors use the standard AIX or SysV calling conventions. See [PPC Calling conventions](<PPC_Calling_conventions.md> "PPC Calling conventions") for details. 

### 68K

68K processors support the CDecl and the Pascal calling conventions. The 68K stack frame model is a conventional grown-down stack with a frame pointer (FP) and stack pointer (SP). 68K hardware enforces 16-bit alignment for parameters in the stack. See [68K vs. PowerPC](<http://physinfo-mac0.ulb.ac.be/divers_html/powerpc_programming_info/intro_to_ppc/ppc4_runtime2.html>) for details. 

Most of the Macintosh Toolbox routines (high-level functions of classic Mac OS) expect to be called as if from Pascal, although some newer managers follow C conventions. Calls to Macintosh Operating System routines (low level functions of classic Mac OS) put results in registers and take results from registers. 

Just after the call to the Pascal routine, the return address is on the top of the stack. Subsequently, the routine will create a stack frame with the LINK instruction, and it will save any sonscratch registers, so that the stack looks like this after the routine begins: 

| return value   
---|---  
| first argument   
| ...   
| last argument   
| static link (opt)   
| return address   
A6 | previous A6   
| local variables   
SP | saved registers   
  
#### References

  * [THINK Pascal](<THINK_Pascal.md> "THINK Pascal") User Manual, Symantec Corporation, Cupertino, 1988
  * Physinfo, Université libre de Bruxelles: [PowerPC Calling Conventions, 68K vs. PowerPC](<http://physinfo-mac0.ulb.ac.be/divers_html/powerpc_programming_info/intro_to_ppc/ppc4_runtime2.html>) (adapted from Apple Developer University)



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

  


## See also

  * [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

---

_Source: [https://wiki.freepascal.org/Writing_portable_code_regarding_the_processor_architecture](https://web.archive.org/web/20250323150322/https://wiki.freepascal.org/Writing_portable_code_regarding_the_processor_architecture)_
