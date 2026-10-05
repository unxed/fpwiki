# Runtime Type Information (RTTI)

│ **English (en)** │  **[русский (ru)](<../ru/Runtime_Type_Information_(RTTI).md>)** │

**Runtime Type Information RTTI** can be utilized to obtain meta information in a Pascal application. 

  


## Contents

  * 1 Converting a enumerated type to a string
  * 2 See Also



  


## Converting a enumerated type to a string

One can use RTTI to obtain a string from a enumerated type. 
    
    
    uses TypInfo; 
    
    type
      TProgrammerType = (tpDelphi, tpVisualC, tpVB, tpJava) ;
    
    var 
      s: string;
    begin
      s := GetEnumName(TypeInfo(TProgrammerType), integer(tpDelphi));
      // Here s = 'tpDelphi'
      WriteLn(s)
    end.
    

But you can also do it without RTTI: 
    
    
    program noRTTI;
    type
      TProgrammerType = (tpDelphi, tpVisualC, tpVB, tpJava) ; 
    var 
      s: string;
    begin
      writestr(s, tpDelphi);
      WriteLn(s);
    end.
    

## See Also

  * [RTTI tab](<RTTI_tab.md> "RTTI tab")
  * [RTTI controls](<RTTI_controls.md> "RTTI controls")
  * [Run-Time Type Information In Delphi - Can It Do Anything For You?](<http://www.blong.com/Conferences/BorConUK98/DelphiRTTI/CB140.htm>)

---

_Source: [https://wiki.freepascal.org/Runtime_Type_Information_(RTTI)](https://web.archive.org/web/20250217104215/https://wiki.freepascal.org/Runtime_Type_Information_(RTTI))_
