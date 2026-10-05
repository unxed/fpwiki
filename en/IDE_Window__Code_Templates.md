# IDE Window: Code Templates

│ **English (en)** │

## Contents

  * 1 What are Code Templates?
  * 2 Example 1 - ifb
  * 3 Example 2 - Using macros
  * 4 Example 3 - Editing macros parameters
  * 5 Template Autocompletion
  * 6 Macros
    * 6.1 Define your own macros
  * 7 DCI file format



## What are Code Templates?

A Code Template is a section of 'boiler plate' code with an associated abbreviation which invokes its insertion. For example, you can type 'cl' in the Source Editor, and then press the Ctrl+J keyboard shortcut to invoke the code template(s) starting with 'cl'. This feature is a great time saver. 

The Code Templates dialog (accessed via the Tools|Code Templates... menu option) allows you to edit existing templates, or add your own templates. 

Code template text can contain macros, and you can even add your own macros to the list of those already available by creating a design time package and using the macrointf.pas of the IDEIntf package. 

## Example 1 - ifb

type 
    
    
      ifb|
    

Here the pipe symbol (|) represents the blinking screen cursor. Press Ctrl+j. The three characters you typed ('ifb') will be expanded to 
    
    
      if | then begin
    
      end;
    

Again the pipe represents the cursor blinking onscreen. You will see that the cursor has been moved. This template is called 'ifb' and it is defined as follows: 
    
    
    if | then begin
    
    end; 
    

This time the pipe '|' is an actual character you type as part of this code template's definition. It stipulates where the cursor will be positioned after the template has been inserted. 

## Example 2 - Using macros

This second example shows you how to create a code template 'w' which repeats the name of the current procedure somewhere in your code. For long names this can be quite a time saver, and it also types it for you without any spelling mistakes! 

  * In the Code Templates dialog, click on the [Add] button. In the dialog which pops up set Token to 'w', and Comment to 'writeln(ProcName)', then click [OK].
  * Your new code template item has been added to the list box.
  * In the source below fill in


    
    
     writeln(' ',|);
    

  * Here pipe is an actual pipe character defining the new cursor position. Now place the cursor after the first apostrophe (') and click on the 'Insert macro' button. A dialog pops up listing all available code macros. Choose 'ProcedureName' and click 'Insert Macro'. You should now have:


    
    
     writeln('$ProcedureName() ',|);
    

  * Click [OK] to save and close the Code Templates dialog.
  * Move the cursor into the body of a method, and type 'w'. For instance:


    
    
     procedure TForm1.FormDestroy(Sender: TObject);
     begin
       w|
     end;
    

(Here the pipe represents the blinking cursor, not a character). 

  * Press Ctrl+j. You will get:


    
    
     procedure TForm1.FormDestroy(Sender: TObject);
     begin
       writeln('TForm1.FormDestroy ',|);
     end;
    

## Example 3 - Editing macros parameters

  * Go: Tools - Code Templates, in group box Templates press button Add
  * Input Token: w
  * Input Comment: While i<count do
  * Press Ok
  * In field below Copy/Paste next text:


    
    
     $Param(i):=0;
     while($Param(i,Sync=1)<$Param(Count))do
     begin
       |
     inc($Param(i,Sync=1));
     end;     
    

  * Set next check-boxes: Enable Macros, line break, do not add character.
  * Somewhere in editor where you want cycle type letter 'w' (without brackets) and press Enter
  * Now you can type name of counter used instead of 'i' (simultaneously in several places) and press Tab - for edit next parameter. You can press Esc any time to exit from Macros



## Template Autocompletion

Code templates are invoked either by using the Ctrl+j shortcut, or (if you set this option) automatically following particular events. For each template you can specify the event (if any) that automatically invokes it. You can enable none, some or all of the following events: 

  * line break - insert the template whenever the user presses the [Return] or [Enter] key
  * space - insert the template whenever the user presses the [Space bar].
  * word end - whenever the user presses a word-ending key. For example, when typing 'b;' the code template 'b' can be executed.
  * do not add character - this option can be combined with any of the above. When enabled the triggering key itself will not be inserted. For example when the option 'space' is enabled and the user triggers a template insertion by pressing the space bar, the space character itself will not be inserted in the source editor.



## Macros

Macros have the form **$(MacroName)** or **$MacroName(Param1,...)**. Macros can be nested. 

### Define your own macros

You can register your own macros by writing a design time package and installing it. See ide/codetemplatesdlg.pas procedure CreateStandardCodeMacros for examples. 

## DCI file format

The default dci file of the IDE is _pcp/lazarus.dci_ , where _pcp_ is your primary config path (e.g. under Linux $HOME/.lazarus). If this file is not present the IDE will copy the file ide/lazarus_dci_file.dci. 

A dci file is a text file with a list of templates. 

Each template starts with line [name | description]. The template's value are the following lines.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Code_Templates](https://web.archive.org/web/20230603155246/https://wiki.freepascal.org/IDE_Window%3A_Code_Templates)_
