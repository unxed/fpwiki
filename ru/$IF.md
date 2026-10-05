# $IF

│ **[English (en)](<../en/$IF.md>)** │  **русский (ru)** │

Директива компилятора `$IF` в [условной компиляции](<Conditional_compilation.md> "Conditional compilation/ru"). 
    
    
    // директива pointermath отсутствовала в версиях FPC до 3.0.0
    {$if FPC_VERSION > 2}
    	// арифметика указателей это плохо. очень плохо
    	{$pointermath off}
    {$endif}
    

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<../en/global_compiler_directives.md> "global compiler directives") • [local compiler directives](<../en/local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<../en/Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<../en/Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<../en/Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<../en/Platform_defines.md> "Platform defines")  
[$IF](<../en/$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/$IF/ru](https://web.archive.org/web/20250418105211/https://wiki.freepascal.org/$IF/ru)_
