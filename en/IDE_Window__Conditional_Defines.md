# IDE Window: Conditional Defines

│ **English (en)** │

Utility to assist in inserting conditional defines. For example, to convert 
    
    
      OnCreate := @CreateHandler
    

to: 
    
    
      OnCreate := {$IFDEF FPC} @ {$ENDIF} CreateHandler
    

select **@** and then use **Edit** , insert **$IFDEF** (default shortcut Ctrl+Shift+D), select "FPC,NONE" and hit return. If you select one or more complete lines then the conditional defines are put on sepearate lines as in: 
    
    
      {$IFDEF DEBUG}
      Writeln('State= ', State)
      {$ENDIF}
    

The choices are listed in abbreviated form so: 

  * MSWINDOWS,UNIX => {$IFDEF MSWINDOWS} ... {$ENDIF} {$IFDEF UNIX} ... {$ENDIF}
  * FPC,ELSE => {$IFDEF FPC} ... {$ELSE} ... {$ENDIF}
  * DEBUG,NONE => {$IFDEF DEBUG} ... {$ENDIF}



This tool is most useful when you need to put several identical conditionals in a file, you can add to the possible conditionals by selecting or typing the required symbols in "First test" and /or "Second test" and using the **Add** button. 

Your additons are saved in the condef.xml file in the lazarus configuration directory.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Conditional_Defines](https://web.archive.org/web/20230924194957/https://wiki.freepascal.org/IDE_Window%3A_Conditional_Defines)_
