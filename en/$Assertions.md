# $Assertions

│ **[Deutsch (de)](</$Assertions/de> "$Assertions/de")** │  **English (en)** │    
****

  
Back to [local compiler directives](<local_compiler_directives.md> "local compiler directives"). 

  
The local compiler directive **$C** or **$ASSERTIONS** : 

  * is used for error detection;
  * determines whether an expression is compiled or not. If the directive is active, then the Assert expression is compiled.



Examples: 
    
    
    ...
    {$ASSERTIONS ON} 
    ...
    Assert(BooleanExpression, AssertMessage);
    ...
    
    
    
    ...
    {$C ON} 
    ...
    Assert(BooleanExpression);
    ...
    

If the _BooleanExpression_ is false, an optional error message (AssertMessage) is output with the file name, the line number and the address and the program aborts with Runtime error 227. If _BooleanExpression_ is true, program execution continues normally. If assertions are not enabled at compile time, the Assert routine does nothing, and no code is generated for the Assert call. 

## see also

  * [`$C` or `$ASSERTIONS`: Assertion support](<https://freepascal.org/docs-html/current/prog/progsu5.html>) in the Programmer’s Guide

---

_Source: [https://wiki.freepascal.org/$Assertions](https://web.archive.org/web/20250324163108/https://wiki.freepascal.org/$Assertions)_
