# Conditional compilation

│ **English (en)** │  **[русский (ru)](<../ru/Conditional_compilation.md>)** │

**Conditional compilation** refers to compiling or omitting parts of [source code](<Source_code.md> "Source code") based on an [expression evaluated at compile-time](</index.php?title=compile_time_expressions&action=edit&redlink=1> "compile time expressions \(page does not exist\)"). This allows taking account of, for example, different interfaces or architectures of specific [operating systems](<operating_system.md> "operating system") or platforms, while still being able to program in a generic way. 

## Contents

  * 1 Support
  * 2 Relevant compiler directives
    * 2.1 Turbo Pascal style directives
      * 2.1.1 $define
      * 2.1.2 $undef
      * 2.1.3 $ifdef and $endif
      * 2.1.4 $ifndef
      * 2.1.5 $else and $elseif
      * 2.1.6 $ifopt
  * 3 What not to do
    * 3.1 Cases
    * 3.2 Understanding



## Support

Conditional compilation needs to be supported in some way or other. Some [compilers](<Compiler.md> "Compiler") need an additional tool called _pre-processor_ , [FPC](<FPC.md> "FPC") however has all required functionality built-in. 

For FPC and in de-facto most compiled languages, conditional compilation is implemented by specially crafted comments that are then seen as [compiler directives](<Compiler_directive.md> "Compiler directive"). These surround any arbitrary amount of code that may be ignored or remain included based on an expression provided as evaluated at [compile-time](<Compile_time.md> "Compile time"). 

They can be used for a variety of purposes like: 

  * Platform specific code isolation
  * Natural language selection (where `resourceString`s do not suffice)
  * Licensing opensource and closed source parts
  * Isolating experimental code
  * Compiler version: certain compiler features may have been present only since a certain version
  * Library version: interfaces may have changed with certain versions
  * etc., etc.



## Relevant compiler directives

FPC supports four different styles of conditional compilation: 

  * [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") and early [Delphi](<Delphi.md> "Delphi") style directives
  * [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") style directives
  * Modern Free Pascal and Delphi style directives
  * Compile time Macros



Note the syntax here is not [case sensitive](<case-sensitive.md> "case-sensitive") as conforms to all [Pascal](<Pascal.md> "Pascal") syntax. We will use both lowercase and uppercase examples. We will show you the difference between the modes and how to efficiently use them. 

### Turbo Pascal style directives

The Turbo Pascal style directives are 

  * `{$DEFINE}`,
  * `{$IFDEF}`,
  * `{$ENDIF}`,
  * `{$IFNDEF}`,
  * `{$IFOPT}`,
  * `{$ELSE}`,
  * `{$ELSEIF}` and
  * `{$UNDEF}`.



We will describe the directives in the context of the style. Some defines have an extended meaning in another style. 

That means later on we may expand the meaning of certain directives like e. g. `{$DEFINE}`in the context of Macros. 

#### `$define`

The `{$DEFINE}` directive simply declares a symbol that we later can use for conditional compilation: 
    
    
    {$DEFINE name} // This defines a symbol called "name"
    

Note you can also define a symbol from the [command line](<Command-line_interface.md> "Command-line interface") or the [IDE](<IDE.md> "IDE"), for example 
    
    
    -dDEBUG
    

is the command line equivalent of 
    
    
    {$DEFINE DEBUG}
    

in the source code. 

#### `$undef`

The `{$UNDEF}` directive undefines a (presumably) previously defined symbol. Here is an example that the author uses in practice: 
    
    
    // Some older source code is polluted with {$IFDEF FPC}
    // that are no longer necessary
    // depending on the Delphi version to which it it should be compatible.
    // I always test this by trying this on top of the program or unit:
    {$IFDEF FPC}
      {$MODE DELPHI}
      {$UNDEF FPC}
      {$DEFINE VER150} 
      // code will now compile as if it was Delphi 7,
      // provided the original Delphi source code
      // was indeed written for Delphi 7 and up.
    {$ENDIF}
    

#### `$ifdef` and `$endif`

The simplest way to define a block of conditional code is like this: 
    
    
    unit cross;
    {$IFDEF FPC}{$MODE DELPHI}{$ENDIF}
    

The above example is quite common for source code that has to compile with both Delphi and FPC. 

If the compiler is Delphi, then nothing is done, but if the compiler is the FPC, it will configure FPC to compile and use Delphi syntax mode. 

This `FPC` conditional symbol is defined by the compiler (cf. [`compiler/options.pas`](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/tags/release_3_2_0/compiler/options.pas?view=markup#l3695>)). The `{$IFDEF}` and `{$ENDIF}` frame syntax is symmetrical: Every `{$IFDEF}` has a matching `{$ENDIF}`. 

To help you recognize the corresponding blocks you can use e. g. indentation, but you can also use the [comment](<Comments.md> "Comments") feature: 
    
    
    {$IFDEF FPC this part is Free Pascal specific}
    // some Free Pascal specific code
    {$ENDIF Free Pascal specific code}
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This comment feature is often not well understood. Some people – as on an older version of this wiki entry – assumed you could nest `{$IFDEF}` because the compiler seems to accept the syntax. But the former is false and the latter is true: Yes, the compiler accepts the syntax below, but it is not a nested `{$IFDEF}` but a single {`$IFDEF}` condition and the rest is a comment! The code below executes the [`writeLn`](<Write.md> "Write") if and only if `red` is defined. In this example `{$ifdef blue}` is a comment! Even if the `{$define blue}` is valid. 
    
    
    // program completely wrong;
    {$define blue}  
    begin
    {$ifdef red or $ifdef blue} // everything after red is a comment
      writeLn ('red or blue');  // this code is never reached
    {$endif red or blue}        // everything after $endif is a comment.
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The comment feature is non-standard and the [GP](<GNU_Pascal.md> "GNU Pascal")C for instance will emit a warning “`garbage at end of `$ifdef' argument`”.

#### `$ifndef`

This is the opposite of `{$IFDEF}` and code will be included of a certain condition is _not_ defined. A simple example is: 
    
    
    {$IFNDEF FPC this part not for Free Pascal}
    // some specific code that Free Pascal should not compile
    {$ENDIF code for other compilers than Free Pascal}
    

#### `$else` and `$elseif`

`{$ELSE}` is used to compile code that does not belong to the code block that is defined by the corresponding `{$IFDEF}`. It is also valid in the context `{$IFOPT}`, `{$IF}` or `{$IFC}` that we will discuss later. 
    
    
    {$IFDEF red}
         writeLn('Red is defined');
    {$ELSE  no red}
      {$IFDEF blue}
        writeLn('Blue is defined, but red is not defined');
      {$ELSE no blue}
        writeLn('Neither red nor blue is defined');
      {$ENDIF blue}
    {$ENDIF red}
    

Such nested conditional written in the above syntax can get very confusing and thus is prone to errors. Luckily we can simplify it a lot by using `{$ELSEIF}`. The code below is an expanded equivalent of the first example: 
    
    
    {$IF defined(red)}
      writeLn('Red is defined');
    {$ELSEIF defined(blue)}
      writeLn('Blue is defined');
    {$ELSEIF defined(green)}
      writeLn('Green is defined');
    {$ELSE}
      writeLn('Neither red, blue or green. Must be black...or something else...');
    {$ENDIF}
    

As you can see this is a lot more readable. 

#### `$ifopt`

With `{$IFOPT}` we can check if a certain compile option is defined. 

From the programmers’ manual: 

> The `{$IFOPT switch}` will compile the text that follows it if the switch switch is currently in the specified state. If it isn’t in the specified state, then compilation continues after the corresponding `{$ELSE}` or `{$ENDIF}` directive. 

As an example: 
    
    
    {$IFOPT M+}
      writeLn('Compiled with type information');
    {$ENDIF}
    

Will compile the `writeLn` statement only if generation of type information is enabled. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The `{$IFOPT}` directive accepts only short options, i. e. `{$IFOPT TYPEINFO}` will not be accepted.

A common use is this example to test if `DEBUG` mode is defined: 
    
    
    {$IFOPT D+}{$NOTE debug mode is active}{$ENDIF}
    

Such defines can also reside in configuration files like `fpc.cfg` which also contains a full explanation on how to use: 
    
    
    # ----------------------
    # Defines (preprocessor)
    # ----------------------
    #
    # nested #IFNDEF, #IFDEF, #ENDIF, #ELSE, #DEFINE, #UNDEF are allowed
    #
    # -d is the same as #DEFINE
    # -u is the same as #UNDEF
    #
    #
    # Some examples (for switches see below, and the -? help pages)
    #
    # Try compiling with the -dRELEASE or -dDEBUG on the command line
    #
    # For a release compile with optimizes and strip debug info
    #IFDEF RELEASE
      -O2
      -Xs
      #WRITE Compiling Release Version
    #ENDIF
    

## What not to do

This is a short tutorial: 

### Cases

What is wrong with this code? Can you spot it? 
    
    
    var
      MyFilesize:
      {$ifdef Win32}
        Cardinal
      {$else}
        int64
      {$endif}
      ;
    

answer  The answer is: 

  * that Free Pascal compiles for more CPU types than [32](<32_bit.md> "32 bit") an [64 bit](<64_bit.md> "64 bit"), also for e. g. 8 and 16 bit.
  * on most 64-bit platforms the maximum file size is a [QWord](<QWord.md> "QWord"), not an [Int64](<Int64.md> "Int64").

That programmer fell into a trap that is common: If you use a define, make sure your logic is solid. Otherwise such code can easily cause accidents. The compiler will not catch your logic errors! It is always good to realize such things especially that such things can easily be fixed. 
    
    
    var
      MyFilesize:
      {$if defined(Win32)} 
        Cardinal 
      {$elseif defined(Win64)}
        Qword;
      {$else}
        {$error this code is written for win32 or win64}
      {$endif}
    

As an aside of course there is a solution for this particular example that does not use conditionals at all: 
    
    
    var
      MyFilesize: NativeUint;
      
  
---  
  
### Understanding

What is wrong with this code? Can you spot it? 
    
    
    {$IFDEF BLUE AND $IFDEF RED} Form1.Color := clYellow; {$ENDIF}
    {$IFNDEF RED AND $IFNDEF BLUE} Form1.Color := clAqua; {$ENDIF}
    

answer  The Answer is: 

  * Well, I have already wrote a _comment_ that warned you.. so look at the warning.... You should be able to spot it...
  * Compiler directives override the compiler... be careful with that ax Eugene.

  
---  
  
  


Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • Conditional compilation • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Conditional_compilation](https://web.archive.org/web/20241213034729/https://wiki.freepascal.org/Conditional_compilation)_
