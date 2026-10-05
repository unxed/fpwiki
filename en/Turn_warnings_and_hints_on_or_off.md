# Turn warnings and hints on or off

There are several ways to control the behavior of warnings in both the Lazarus IDE and in the source code itself. Some warnings and hints are not very useful in a particular case so you want to hide them from view. On the other hand, if you would hide warnings or hints globally, you may want to turn a particular warning or hint explicitly on for a particular piece of your code. 
    
    
    procedure TForm.Button1Click(sender:Tobject);
    begin
      // Do something that does not use sender
      Form1.Button1.Caption := 'Test';  
    end;
    

This will generate a hint that parameter sender is not used. Doh. I can see that! Problem is that this hint turns up all over the place and I know what it means. How do I hide them? 

## Contents

  * 1 Lazarus IDE options
    * 1.1 Global IDE option
    * 1.2 Local IDE option
  * 2 Source code and compiler options
    * 2.1 Control all warnings per unit:
    * 2.2 Control all hints per unit:
    * 2.3 Control specific warnings:
    * 2.4 Control Specific Warnings or hints per piece of code:
  * 3 Obtaining the warnings and hints numbers to suppress



# Lazarus IDE options

The Lazarus IDE can control hints and warnings in two ways: 

  * Globally through the Project Options menu
  * Using an IDE macro



## Global IDE option

Go to the Project Options->Compiler Options->Messages and uncheck 'Hint:Paramater "$1" not used'. 

This will turn of this particular hint for your whole project.  
If you would like to turn off this hint for all your projects, you can check the left bottom checkbox 'Set compiler options as default'. 

[![lazarusglobal.png](https://wiki.freepascal.org/images/d/da/lazarusglobal.png)](</File:lazarusglobal.png>)

## Local IDE option

In the Lazarus IDE you can make use of the {%H-} IDE macro to turn hints off for a particular parameter.  
Of course {%H+} will turn hints on. 
    
    
        procedure FormMouseDown(Sender: TObject; {%H-}Button: TMouseButton;
          {%H-}Shift: TShiftState; {%H-}X, {%H-}Y: Integer);
    

Note that this turns off ALL hints for that parameter, not just the 'is not used' hint, so be careful. 

# Source code and compiler options

The options as described above will only work within the Lazarus IDE. 

It won't work in any other IDE, not even de FPC textmode IDE. The compiler output will still emit warnings and hints. 

Luckily there are several other options to control hints and warnings in the source code itself. 

## Control all warnings per unit:

Add 
    
    
    {$warnings off}
    

or resp. 
    
    
    {$warnings on}
    

just after the unit name. 

## Control all hints per unit:

Add 
    
    
    {$hints off}
    

or resp. 
    
    
    {$hints on}
    

just after the unit name. 

## Control specific warnings:

Add 
    
    
    {$warn <number> off}
    

just after the unit name. See the example below. You can also use this construct if you want all warnings off but some specific warning(s) on: 
    
    
    {$warnings off}{$warn <number> on}
    

## Control Specific Warnings or hints per piece of code:

You can control this very finely grained by using {$push} and {$pop} around a block of code. This does not have to be a method body as long it is a block, e.g. begin/end,repeat/until,/while do, try/finally etc. 
    
    
    procedure withunusedparameter(const testme:integer);
    {$push}{$warn 5024 off}
    begin
    //
    end;
    {$pop}
    begin
      withunusedparameter(100);
    end.
    

# Obtaining the warnings and hints numbers to suppress

You can obtain the needed numbers by compiling your code once with line info on and with -vwhilq 

This will show both the numbers and the messages and the line numbers where they are found. 

The other way to obtain hint, notice or warning number is to search inside FP repository for source of your current FPC version. 

All of the messages with its own description is stored in <FPC source dir>/msg/error<language>.msg 

Note: the language codes are not all the same as the standard [ISO 639-1 2-letter language codes](<Language_Codes.md> "Language Codes"). The current files are: 

| File Name  | Language   
---|---  
errorct.msg | Catalan / català   
errord.msg | German / Deutsch (alternative, US-DOS)   
errorda.msg | Danish / dansk   
errordu.msg | German / Deutsch (UTF-8)   
errore.msg | English (default)   
errores.msg | Spanish / Español (CP 28605)   
errorf.msg | French / français (CP 850)   
| File Name  | Language   
---|---  
errorfi.msg | French / français (ISO 8859-1)   
errorhe.msg | Hebrew / עִברִית (CP 1255)   
errorheu.msg | Hebrew / עִברִית (UTF-8)   
errorid.msg | Indondesian / Indonesia   
erroriu.msg | Italian / italiana   
errorn.msg | Dutch / Nederlands   
errorpl.msg | Polish / Polski (CP 852)   
| File Name  | Language   
---|---  
errorpli.msg | Polish / Polski   
errorpt.msg | Portugese / portuguesa (CP 850)   
errorptu.msg | Portugese / portuguesa (UTF-8)   
errorr.msg | Russian / russa (CP 866)   
errorru.msg | Russian / russa (UTF-8)   
errorues.msg | Spanish / Español (CP 65001)   
  
This file also has a header, which provides explanation how error/notice/hint/warning constants are constructed. 

For the English language it contains the following: 
    
    
    # CodePage 20127
    
    #
    # The constants are build in the following order:
    # <part>_<type>_<txtidentifier>
    #
    # <part> is the part of the compiler the message is used
    #   asmr_     assembler parsing
    #   asmw_     assembler writing/binary writers
    #   unit_     unit handling
    #   option_   command line parameter parsing
    #   scan_     scanner
    #   parser_   parser
    #   type_     type checking
    #   general_  general info
    #   exec_     calls to assembler, external linker, binder
    #   link_     internal linker
    #   package_  package handling
    #
    # <type> the type of the message it should normally used for
    #   f_   fatal error
    #   e_   error
    #   w_   warning
    #   n_   note
    #   h_   hint
    #   i_   info
    #   l_   add linenumber
    #   u_   used
    #   t_   tried
    #   c_   conditional
    #   d_   debug message
    #   x_   executable informations
    #   o_   normal (e.g., "press enter to continue")
    #
    # <type> can contain a minus sign at the beginning to mark that
    # the message is off by default. Look at type_w_explicit_string_cast
    # for example.
    

So, to find your warning/notice/etc, you just need to search the message body. For the example, it can be <unit "Pas2Js.pas" not used in main.pas>, to find this warning, you need to replace all of the local data in message body with $< data number >, 
    
    
    for this situation it will be: <Unit "$1" not used in $2>;
    

After that, you can find the required message just by finding its body in errors<language>.msg file, you will find the following: 
    
    
    sym_n_unit_not_used=05023_H_Unit "$1" not used in $2
    % The unit referenced in the \var{uses} clause is not used.
    

So, the actual notice number is 05023. 

Based on that number you can decide to suppress the warning or hint globally, per unit or per routine, depending on what you need. 

For example, if you want to suppress unused parameters you can subsequently compile with -vm5024. 

With the -vm compiler option you can use multiple warning and hint numbers separated by a comma. Or you can write: 
    
    
    {$warn 5024 off}
    

This needs to be done for each number separately. 

See also: 

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<global_compiler_directives.md> "global compiler directives") • [local compiler directives](<local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<Platform_defines.md> "Platform defines")  
[$IF](<$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Turn_warnings_and_hints_on_or_off](https://web.archive.org/web/20250211192018/https://wiki.freepascal.org/Turn_warnings_and_hints_on_or_off)_
