# $IF

│ **[Deutsch (de)](</$IF/de> "$IF/de")** │  **English (en)** │  **[français (fr)](</$IF/fr> "$IF/fr")** │  **[русский (ru)](<../ru/$IF.md> "$IF/ru")** │    
****

The `{$if …}` directive can be used in [conditional compilation](<Conditional_compilation.md> "Conditional compilation"). 
    
    
    // pointermath directive did not exist prior FPC 3.0.0
    {$if FPC_VERSION > 2}
    	// pointer arithmetics is bad. very bad
    	{$pointermath off}
    {$endif}
    

Also, enable you to write complex conditions that you cannot do with the `{$ifdef …}`, 

And there are two bool functions[defined/undefined] that can be mixedup with [and/or] logic operators, For example: 
    
    
    //Befor you need write these conditions to check some conditions:
    {$define SOMETHING}
    {$define SOMETHINGELSE}
    {$ifdef SOMETHING}//Union $IfDef to check multiple conditions
    	{$ifdef SOMETHINGELSE}
       {$ModeSwitch advancedrecords}
      {$endif}
    {$endif}
    
    //But with the {$IF} you can check conditions together:
    {$if defined(SOMETHING) and defined(SOMETHINGELSE)}//simple and readabl instead of union {$IFDef}`s
      {$ModeSwitch advancedrecords}
    {$endif} 
    
    {$if defined(somthing) or defined(somethingelse)}
      //Whatever you need!
    {$endif}
    
    {$if undefined(what) and defined(somethingelse)}
      //Just for note, Another usage!
    {$endif}
    

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
$IF  
  
  
****

---

_Source: [https://wiki.freepascal.org/$IF](https://web.archive.org/web/20240906203532/https://wiki.freepascal.org/$IF)_
