# Language related articles

Back to [Compiler development articles](<Compiler_development_articles.md> "Compiler development articles")

For a better updated language support list, see also [Delphi language features which FPC does not have](<Delphi_language_features_missing_from_the_Free_Pascal_Compiler.md> "Delphi language features missing from the Free Pascal Compiler"). 

## Contents

  * 1 Things FPC 2.x.x currently doesn't support
  * 2 Things FPC 1.9.x partially support
  * 3 New in 1.9.x (not yet in the docs)
    * 3.1 SSE Usage
  * 4 New in 2.0.x
    * 4.1 UTF-8 support
  * 5 New in 2.1.x
    * 5.1 Unaligned Keyword
  * 6 Ideas for new stuff



## Things FPC 2.x.x currently doesn't support

  * The "implements" style delegation. see ( <https://web.archive.org/web/20070806104804/http://info.borland.com/techpubs/delphi/delphi5/oplg/objintrf.html> )
  * ~~dispinterface~~
  * packages
  * ~~slice (dynarray)<http://www.freepascal.org/bugs/showrec.php3?ID=2856>~~



## Things FPC 1.9.x partially support

  * The more advanced variants support is missing or buggy (detail bugs, custom variants support missing, vararray stuff stil in development)
  * currency support needs testing (there was some work done recently afaik)
  * ~~Dynarrays, inclding Slice function seems to have problems~~
  * Reintroduce; is only skipped not processed (at least the compiler says so) 
    * im pretty sure that reintroduce just suppresses a warning in delphi [Plugwash](</User:Plugwash> "User:Plugwash")
      * According to the Delphi 2010 documentation reintroduce affects the behavior of a method when it's called from a object casted as his ancestor, if the method has overloaded the overloaded version is called else (when you use reintroduce) the original version of that method is called. [Zazula](</User:Zazula> "User:Zazula") 23:15, 8 September 2012 (UTC)
  * [widestrings](<Widestrings.md> "Widestrings"). The default conversion routines for ansistring<>widestring only convert chars in the ascii (code points 0-127) range. The routines can be replaced through procvars but they are designed in a way that makes them useless for any local charset that doesn't have a 1:1 mapping to unicode.



02:56, 1 Jan 2005 (CET) 

The heavily variants using package DECAL has been ported recently and worked fine. So I think the variants problems are mostly over, and only atypical conversions miss. (that can be usually fixed easily if you submit a bug with a simple example that works in Delphi and not in FPC). However some RTL support (the wrapper classes) are largely unimplemented 

## New in 1.9.x (not yet in the docs)

  * {$fputype xxx) to select a certain fpu style or -Cfxxx on command line/cfg file 
    * all: soft (not yet implemented)
    * i386: x87, sse, sse2 
      * code compiled with sse uses the sse to do calculations with the single data type. This code runs only on Pentium III and above and AthlonXP and above
      * code compiled with sse2 uses the sse unit to do calculations with the single and double data type. This code runs only on PentiumIV and above and Athlon64 and above
    * x86-64: sse64
    * powerpc: standard
    * arm: libgcc, fpa, fpa10, fpa11, vfp


  * procedural property support (property outside a class with normal procedures as getters/setters.
  * A -Fa parameter that lets you USES an unit as first unit in each unit. Great for e.g. redefining base types etc.
  * reuse keywords imports all units the reused unit imports.
  * reintroduce omits warning about hidden methods
  * Some mac pascal extensions, see [Mode MacPas](<Mode_MacPas.md> "Mode MacPas").
  * `system.prefetch(const mem);` depending on the target processor the compiler generates a prefetch hint on the given memory location. On iA32 this is the sse instruction prefetchnta which is supported by Pentium3/AthlonXP and above. If the intruction set, selected with -Cp (on iA-32, prefetch instructions are generated for -Oppentium3 and above) doesn't support this instruction, no code is generated for prefetch.
  * Interface type selection: The commandline switch -SI and the directive $interfaces (possible values: corba and com) allow you to choose the type of interfaces which has no parent. Interfaces with parent inherit the style from their parent regardless of the currently selected interface type. 
    * COM style interfaces work like Delphi interfaces: they inherit implicitly from IUnknown and they are reference counted.
    * CORBA style interfaces are neither ref. counted nor do they inherit from IUnknown.



For new rtl related stuff have a look at [New in the 1.9.x rtl (not yet in the docs)](<New_in_the_1.9.md> "New in the 1.9.x rtl \(not yet in the docs\)"). 

### SSE Usage

To determine the supported instruction set, use the is_sse_cpu and is_sse2_cpu of the mmx unit. Because the sse code is usually faster than x87-fpu code, it's recommended to use these switches for heavy floating point calculations. If you want an application which runs on all fpu architectures, a possible solution is to put the code into an include file and include this file twice into your program: once compiled with e.g. -Cfsse2 and once without. Use the e.g. is_sse2_cpu variable to select the appropriate code: 
    
    
    {$fputype x87}
    procedure calc_x87;
    {$i mymathcode.inc}
    
    
    
    {$fputype sse2}
    procedure calc_sse2;
    {$i mymathcode.inc}
    
    
    
    begin 
      if is_sse2_cpu then
        calc_sse2
      else
        calc_x87;
    end;
    

## New in 2.0.x

### UTF-8 support

Support for utf-8 strings inside '...' has been added. The encoding can be either selected by passing -Fcutf8 on the command line, using the directive $codepage utf8 or adding the utf-8 signature $ef $bb $bf at the beginning of a file. Strings with chars>=$80 are converted to widestrings and handled internally as widestrings. Unicode chars > 65535 can't be represented this way, this will cause an error message. 

## New in 2.1.x

### Unaligned Keyword

When typecasting a variable as "unaligned" you tell compiler that this variable should be read or write to one byte at a time.This is usefull on those systems that require aligned data accesses. This is currently only implemented on arm cpus. 

## Ideas for new stuff

  * [Generics proposals](<Generics_proposals.md> "Generics proposals")
  * [Modernised Pascal](<Modernised_Pascal.md> "Modernised Pascal")
  * [Mac Local Procedure Parameters](<Mac_Local_Procedure_Parameters.md> "Mac Local Procedure Parameters")
  * [OpenMP support](<OpenMP_support.md> "OpenMP support")
  * [ThreadEvent](<ThreadEvent.md> "ThreadEvent")

---

_Source: [https://wiki.freepascal.org/Language_related_articles](https://web.archive.org/web/20231204163200/https://wiki.freepascal.org/Language_related_articles)_
