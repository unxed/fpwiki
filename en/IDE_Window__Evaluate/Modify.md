# IDE Window: Evaluate/Modify

│ **[Deutsch (de)](</IDE_Window:_Evaluate/Modify/de> "IDE Window: Evaluate/Modify/de")** │  **English (en)** │  **[français (fr)](</IDE_Window:_Evaluate/Modify/fr> "IDE Window: Evaluate/Modify/fr")** │    
****  
****

## Contents

  * 1 Important
  * 2 Evaluate/Modify
    * 2.1 Interface
    * 2.2 Data



# Important

You must [setup the debugger](<../Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then will this window be useful. 

# Evaluate/Modify

Error creating thumbnail: Unable to save thumbnail to destination

### Interface

    _Evaluate/Modify_ window can be displayed via Main Menu→Run→Evaluate/Modify...

It is **not** grayed only when the executed application is **paused**. 

Evaluate
    Evaluates the given expression

Modify
    When the new values is set, it assigns the new value to the expression. The expression can only be a property or a variable. (Read notes on "New Value")

Watch
    Add the expression to the [watch list](<../IDE_Window__Watch_list.md> "IDE Window: Watch list")

Inspect
    Show the [variable inspector](<../IDE_Window_Variable_Inspector.md> "IDE Window:Variable Inspector") for the expression. The expression can only be a property or a variable.

Use Instance class type
    Objects are normally shown according to the declaration of the watched expression. Evaluating "Sender: TObject" will only show you data, that is declared in TObject. However object variables can contain objects of inherited classes. Sender may be a TForm. Using this the debugger will find the actual class of the object and display all data.

### Data

Expression
    Enter the expression to be evaluated/modified/watched/inspected here

Result
    The result of the evaluation. If the evaluation of the expression failed, an error message is shown.

New Value
    The new value you want to use for a variable or property.

Important note on "New Value": 

    You should not attempt to modify "managed" types (strings, dynamic array). This will lead to memory corruption.
    There is currently no safety check in place, so you must ensure this yourself.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Evaluate/Modify](https://web.archive.org/web/20240526191609/https://wiki.freepascal.org/IDE_Window%3A_Evaluate/Modify)_
