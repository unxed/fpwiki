# Hint Directives

│ **English (en)** │  **[suomi (fi)](</Hint_Directives/fi> "Hint Directives/fi")** │  **[français (fr)](</Hint_Directives/fr> "Hint Directives/fr")** │    
****

## Using Hint Directives

Whenever an identifier marked with a hint directive is later encountered by the compiler, then a warning will be displayed, corresponding to the specified hint. 

  * **deprecated**
    * The use of this identifier is deprecated, use an alternative instead. The deprecated keyword can be followed by a string constant with a message. The compiler will show this message whenever the identifier is encountered.


  * **experimental**
    * The use of this identifier is experimental: this can be used to flag new features that should be used with caution.


  * **platform**
    * This is a platform-dependent identifier: it may not be defined on all platforms.


  * **unimplemented**
    * This should be used on functions and procedures only. It should be used to signal that a particular feature has not yet been implemented.



The following are examples: 
    
    
    Const
      AConst = 12 deprecated;
    
    var
      p : integer platform;
    
    Function Something : Integer; experimental;
    
    begin
      Something:=P+AConst;
    end;
    
    begin
      Something;
    end.
    

This would result in the following output: 
    
    
    testhd.pp(11,15) Warning: Symbol "p" is not portable
    testhd.pp(11,22) Warning: Symbol "AConst" is deprecated
    testhd.pp(15,3) Warning: Symbol "Something" is experimental
    

Hint directives can follow all kinds of identifiers: units, constants, types, variables, functions, procedures and methods.

---

_Source: [https://wiki.freepascal.org/Hint_Directives](https://web.archive.org/web/20250217212802/https://wiki.freepascal.org/Hint_Directives)_
