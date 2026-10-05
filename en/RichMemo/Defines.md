# RichMemo/Defines

The following defines are used within the component. You can use those to fix the compilation on your Lazarus setup. 

Each define is prefixed with "RM" (which stands for Rich Memo). This is used 

## Reference

RMLCL18
    the defines indicates that the RichMemo is built with Lazarus 1.8 (or earlier).
    It was introduced to resolve the compilation of RichMemo for Cocoa widgetset. In Lazarus 2.0 the Cocoa units were rearranged.

RMCARBONSELSTART
    the define changes how SetSelStart works for RichMemo. On each selection a notification OnSelection change needs to be fired.

## Adding a Define

  * Open up RichMemo package
  * select "Options"
  * select "Custom options" under "Compiler Options"
  * in the Custom options enter a define, prefxied with "-d". For example, if you need RMLCL18 define then you should type in:


    
    
    -dRMLCL18
    

if you need multiple defines, separate them with space 
    
    
    -dRMLCL18 -dRMCARBONSELSTART
    

## See Also

  * [RichMemo](<../RichMemo.md> "RichMemo")

---

_Source: [https://wiki.freepascal.org/RichMemo/Defines](https://web.archive.org/web/20230529125110/https://wiki.freepascal.org/RichMemo/Defines)_
