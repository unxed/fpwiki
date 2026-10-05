# sGoto

│ **[Deutsch (de)](</sGoto/de> "sGoto/de")** │  **English (en)** │    
****

  
Back to [local compiler directives](<local_compiler_directives.md> "local compiler directives"). 

  


# $GOTO

The **$GOTO** directive: 

  * determines whether jump commands can be used;
  * knows the ON and OFF switches;
  * defaults to {$GOTO OFF}. That is, no jump commands are allowed;
  * can be enabled if the directive is {$GOTO ON}, the compiler supports the [GOTO](<Goto.md> "Goto") and [LABEL](<Label.md> "Label") commands;
  * corresponds to the -Sg command line switch.



Example: 
    
    
      {$GOTO ON}
    
     label TheEnd;
    
     begin
       If ParamCount = 0 then
         GoTo TheEnd;
       Writeln('Parameters were passed on the command line');
     TheEnd:
     end.
    

Note for inline assembler: If labels are used in the assembly code, the directive {$GOTO ON} must be used.

---

_Source: [https://wiki.freepascal.org/sGoto](https://web.archive.org/web/20250420054919/https://wiki.freepascal.org/sGoto)_
