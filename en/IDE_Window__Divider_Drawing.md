# IDE Window: Divider Drawing

│ **English (en)** │

## Navigation

You can reach this dialog from the Lazarus IDE: 

  * [Main Menu](<Main_menu.md> "Main menu") > [Tools](<Main_menu.md> "Main menu") > [Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") > [Editor](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") > Divider drawing
  * Source Editor popup context menu > Options... > Editor > Divider drawing.



## Divider Drawing

The source editor draws horizontal dividers between blocks off code helping the programmer to quickly see where a block starts and ends. What a block is can be defined in detail for each language and block type. 

To see see how the editor look with divider drawing enabled, have a look [here](<New_IDE_features_since.md> "New IDE features since"). 

Divider lines can be configured on a detailed level.  
For example you can choose to have a divider after every top-level procedure/function. (set the level to 1) Or you can include nested functions up to a desired level. (level >= 2) 

You can have dividers after the following logical breaks 

  * Unit Sections: implementation, interface, initialization, ...
  * Uses Clause: In case it gets multi line
  * Var/Type: any var/type/const block on top-level (not in a procedure)
  * Var/Type(local): any var/type/const inside a procedure 

    

    

    The nest level indicates how deeply the *procedure* is nested (as var blocks can not nest themselves)
  * Class/Struct: Class/Record or Object declarations. The nest level is for records defined inside a class.
  * Class/Struct(local): same if used inside a type declaration local to a procedure
  * Procedure/Function:
  * Begin/End: Includes Case/Repeat, but not try/except. The nesting is counted to the containing Procedure
  * Try/Except:



Note: 

  * unfinished blocks (open begin with no end) can fool the highlighter.
  * blocks that finish on the same line, on which the opened are ignored

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Divider_Drawing](https://web.archive.org/web/20230604170555/https://wiki.freepascal.org/IDE_Window%3A_Divider_Drawing)_
