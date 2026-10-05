# Porting from Mac Pascal

Although FPC today supports many [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") constructs, there are a few of issues one has to be aware of. Tips to make the code work with GPC are also welcome. 

## Contents

  * 1 Setting the compilation mode
  * 2 Unit Names
  * 3 Uses clause
  * 4 Ordinal sizes
  * 5 Set sizes (Style in particular)
  * 6 Missing CW Functions
  * 7 UPP differences
  * 8 Four Character Codes (FCCs)
  * 9 _Result_ function result alias
  * 10 See also



## Setting the compilation mode

First of all, ensure that the compiler is in [Mac Pascal](<Mode_MacPas.md> "Mode MacPas") mode. You can do this by either using the -Mmacpas command line switch, or by adding {$mode macpas} at the top of all of your source files. 

## Unit Names

Unit names in FPC must match the source file name. Unit names must also differ from any object/class names you use. In Metrowerks Pascal it was possible to have the unit name and an object that was subsequently defined in that unit be the same name. 

## Uses clause

Depending on the compiler you're moving from, you may have a simple or complex uses clause to include all of the toolbox calls. Either way, it can all be replaced with: 
    
    
      uses
        MacOSAll;
    

  


## Ordinal sizes

In Traditional Mac Pascal, the types Byte and Char occupies 2 bytes, except in packed records and in arrays. If you need data to be sharable (e g through files), you need to be aware of this. One remedy is to make such records packed. If possible use SignedByte (which actually occupy only one byte) or Integer (or better SInt16 et. al.). 

## Set sizes (Style in particular)

Sets (eg: mySet = (opt1, opt2, opt3);) may be different sizes if you stored them to disk using CodeWarrior. This is evident with the MacOS type Style: 

sizeof(Style) = 1 in CodeWarrior sizeof(Style) = 4 in FPC 

If you want to convert on the fly, in your structures, replace Style with Byte, then do a conversion in code. 
    
    
      function ByteToStyle(byteStyle: byte): Style;
      
      var
        returnValue: Style;
        count: StyleItem;
      
      begin
        returnValue:= [];
        for count:= bold to extend do
          if ((byteStyle shr Ord(count)) and 1) <> 0
            then returnValue:= returnValue + [count];
        ByteToStyle:= returnValue;
      end;  { ByteToStyle }
    

FPC 2.2.2 and later support packing sets to smaller sizes, by using the {$packset 1} directive. Note however that the set format is considered opaque (i.e., they're not guaranteed to have any particular internal layout), and that the internal format differs between little and big endian systems. 

## Missing CW Functions

CodeWarrior provided built in functions for stripping the Hi and Lo words from a longint. Although most uses of this have been deprecated over the years (like menu commands,) you may still need the functions for old code. Here are substitutions: 
    
    
      function HiWrd(aLong: cardinal): word;
      
      begin
        HiWrd:= hi(aLong);
      end;  { HiWrd }
    
      function LoWrd(aLong: cardinal): word;
      
      begin
        LoWrd:= lo(aLong);
      end;  { LoWrd }
    

## UPP differences

In Codewarrior Pascal, one declared a UPP generically with an "@functionName" parameter. With Carbon, Apple introduced specific UPP creator functions, and with the change to FPC, you simply pass the functionName without the "@" symbol. 

Thus, 
    
    
      theRoutine:= NewAEEventHandlerUPP(@HandleODOC); // CW Apple Event Handler
    

becomes: 
    
    
      theRoutine:= NewAEEventHandlerUPP(HandleODOC);  // FPC Apple Event Handler
    

## Four Character Codes (FCCs)

While FPC supports implicitly converting FCCs to integer constants to some extent, it does not do so in all cases. You can however always add an explicit typecast to OSType/Cardinal/LongWord around an FCC to ensure it is interpreted as an integer constant. This construct is endian-safe. For example: Cardinal('APPL'). 

## _Result_ function result alias

FPC adds an implicit alias for the function result called _result_. This means that you cannot use local variables with the same name, as they will conflict with this identifier. The main reason is that using the function name itself is ambiguous in some situations (e.g., when passing it as a parameter to another routine it usually refers to the function result, but it can also mean the function itself as procedural variable). 

## See also

  * [Converting legacy Pascal code](<macOS_Programming_Tips.md> "macOS Programming Tips").
  * [Mode MacPas](<Mode_MacPas.md> "Mode MacPas")

---

_Source: [https://wiki.freepascal.org/Porting_from_Mac_Pascal](https://web.archive.org/web/20250117072440/https://wiki.freepascal.org/Porting_from_Mac_Pascal)_
