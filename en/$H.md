# $H

│ **[Deutsch (de)](</$H/de> "$H/de")** │  **English (en)** │    
****

  
Back to [local compiler directives](<local_compiler_directives.md> "local compiler directives"). 

  
The **$H** or **$LONGSTRINGS** local compiler directives have the same meaning and determine whether the compiler interprets the reserved word [String](<String.md> "String") as an [AnsiString](<Ansistring.md> "Ansistring"). 

The $LONGSTRINGS directive uses the ON and OFF switches. 

The $H directive uses the + and - switches. 

The default is {$H-}. The reserved word [String](<String.md> "String") is a [ShortString](<Shortstring.md> "Shortstring"). 

The compiler mode {$MODE DELPHI} implies a {$H+} statement, all other modes switch it off. As a result, you should always put {$H+} after a mode directive. 

Example: 
    
    
     // String is an AnsiString
     {$H+}
     
     // String is an AnsiString
     {$LONGSTRINGS ON}
    
     // Default; String is a ShortString
     {$H-}
    
     // Default; String is a ShortString
     {$LongStrings OFF}
    

The {$H} or {$LONGSTRINGS} directive corresponds to the **-Sh** command line option.

---

_Source: [https://wiki.freepascal.org/$H](https://web.archive.org/web/20250425032743/https://wiki.freepascal.org/$H)_
