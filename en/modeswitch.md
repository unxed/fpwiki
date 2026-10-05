# modeswitch

│ **English (en)** │  **[中文（中国大陆） (zh_CN)](</modeswitch/zh_CN> "modeswitch/zh CN")** │    
****

As of FPC 2.3.1, the **{$MODESWITCH}** directive can be used to select some of the features that a **{$MODE }** directive would select. 

It can be used to enable language features that would otherwise not be available in the current mode. For example, you might be programming in TP mode, but want to use the Out parameters, which are only available in Delphi mode. 

The **{$MODESWITCH }** directive enables or disables individual mode features without changing the current compiler mode. 

This switch is a global switch, and can be used wherever the **{$MODE}** switch can be used. 

The syntax is as follows: 
    
    
    {$MODESWITCH XXX}  
    {$MODESWITCH XXX+}  
    {$MODESWITCH XXX-}
    

The first two will switch on feature XXX, the last one will switch it off. 

The feature XXX can be one of the following: 

Feature | Description   
---|---  
ADVANCEDRECORDS | allow the use of advanced records (i.e. records with methods)   
ALLOWINLINE | Allow inline procedures.   
ANSISTRINGS | Allow use of ansistrings.   
AUTODEREF | Automatic (silent) dereferencing of typed pointers.   
CBLOCKS | C style blocks.   
CLASSICPROCVARS | Use classical procedural variables.   
CLASS | Use object pascal classes.   
CVAR | Allow the use of the CVAR keyword.   
DEFAULTPARAMETERS | Allow use of default parameter values.   
DUPLICATELOCALS | Allow local variables in class methods to have the same names as properties of the class.   
EXCEPTIONS | Allow the use of exceptions.   
HINTDIRECTIVE | Support the hint directives (deprecated, platform etc.)   
INITFINAL | Allow use of Initialization and Finalization   
ISOIO | input/output as required by ISO pascal.   
ISOMOD | mod operation as required by ISO pascal.   
ISOPROGRAMPARAS | Program parameters as required by ISO pascal.   
ISOUNARYMINUS | Unary minus as required by ISO pascal.   
MACPROCVARS | Use mac-style procedural variables.   
NESTEDCOMMENTS | Allow use of nested comments.   
NESTEDPROCVARS | Allow assigning local procedures to procedural variables.   
NONLOCALGOTO | Allow "goto" to a label outside the current scope, e.g. a label located in the outer global scope.   
OBJECTIVEC1 | Allow interfacing with Objective C version 1.   
OBJECTIVEC2 | Allow interfacing with Objective C version 21.   
OBJPAS | Automatically include the ObjPas unit.   
OUT | Allow use of the out parameter type.   
PCHARTOSTRING | Allow automatic conversion of null-terminated strings to strings,   
POINTERTOPROCVAR | Allow silent conversion of pointers to procedural variables.   
PROPERTIES | Allow use of properties.   
REPEATFORWARD | Implementation and Forward declaration must match completely.   
RESULT | Enable the Result identifier for function results.   
TYPEHELPERS | Allow the use of type helpers.   
UNICODESTRINGS | string is by default unicode string.   
  
## Official documentation

  * [$modeswitch](<https://www.freepascal.org/docs-html/3.2.0/prog/progsu106.html#x114-1150001.3.22>) (extern)



## See also

  * [Compiler Mode](<Compiler_Mode.md> "Compiler Mode")
  * [Mode switches](<modeswitches.md> "modeswitches")

---

_Source: [https://wiki.freepascal.org/modeswitch](https://web.archive.org/web/20241212102212/https://wiki.freepascal.org/modeswitch)_
