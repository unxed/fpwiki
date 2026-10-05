# IDE Window: Editor Options Completion Hints

│ **English (en)** │

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Completion and Hints". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__Options.md> "IDE Window: Options") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 Completion and Hints
    * 1.1 Auto remove empty methods
    * 1.2 Add close statement for Pascal blocks
    * 1.3 Hint settings
      * 1.3.1 Auto Display Function Prototypes
      * 1.3.2 Show Declaration Hints
      * 1.3.3 Show Value Hints when debugging
        * 1.3.3.1 Automatic typecasts for objects
      * 1.3.4 Delay for completion box
      * 1.3.5 Delay for hints
      * 1.3.6 Delay for long line hints in completion box
      * 1.3.7 Show long line hints



# Completion and Hints

### Auto remove empty methods

Published methods without code are automatically removed on save. For example: When you double click on a form the IDE creates the published method FormCreate. When this is option is enabled and the method is still empty the IDE will remove it when you save the form. 

### Add close statement for Pascal blocks

Complete blocks. For example, when you type _begin_ and press return the IDE will automatically add the _end;_. This feature exists since 0.9.27. 

## Hint settings

### Auto Display Function Prototypes

Show a hint with the parameters that a procedure/function takes. This will activate, if you press "(" after the name of a function. It also activates, if you enter a "," while being inside the parentheses of a function call. 

This can also be activated by pressing Shift-Ctrl-Space. Using the key combo works independent of the setting being enabled. 

### Show Declaration Hints

When you hover the mouse over an identifier and wait a moment the IDE will show a hint. This has details on where and how the identifier was declared/defined. 

If the package TurboPowerIProDsgn is installed the mouse can move into the hint bubble and the location of the declaration acts as a link. 

### Show Value Hints when debugging

When you hover the mouse over an identifier like for "Declaration Hints", but while debugging, then the IDE will evaluate the Identifier as a watch and display the current value. 

#### Automatic typecasts for objects

If the identifier evaluated is an object (i.e. instance of a class), then the IDE will typecast it to its actual class, showing more information. 

That is by default "Sender: TObject" evaluates as TObject and only shows TObject fields. But with this option, it will determine the actual class (e.g. maybe a TButton) and show all the info for that class 

  


### Delay for completion box

How long before the IDE open the identifier completion dropdown. 

The IDE only auto open the drop down, if you for example type a "." after the name of an record or object. 

### Delay for hints

How long you have to hover over an identifier before the IDE triggers "Declaration Hints" or "Value Hints when debugging" 

### Delay for long line hints in completion box

In the completion drop box some lines may not fully fit and are cut off. When you hover over such a line, or select it with up/down, then a hint will show the full line 

### Show long line hints

How to position the "long line hints in completion box" hint.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Completion_Hints](https://web.archive.org/web/20191024044244/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Completion_Hints)_
