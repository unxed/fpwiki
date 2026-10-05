# IDE Window: Codetools Options

│ **English (en)** │

[![](https://wiki.freepascal.org/images/9/9f/IDE-options-codetools.JPG)](</File:IDE-options-codetools.JPG>)

[](</File:IDE-options-codetools.JPG> "Enlarge")

Codetool options

## Contents

  * 1 General
    * 1.1 Additional Source search path for all projects/packages
    * 1.2 Jumping (e.g. Method Jumping)
    * 1.3 Indentation
  * 2 Class Completion
    * 2.1 Insert class part
    * 2.2 Insert methods
    * 2.3 Method default section
    * 2.4 Flags
    * 2.5 Header comment for class
    * 2.6 Implementation comment for class
    * 2.7 Property completion
  * 3 Code Creation
    * 3.1 Procedure insert policy
    * 3.2 Keep order of procedures
  * 4 Words
    * 4.1 Keyword policy
    * 4.2 Identifier policy
  * 5 Line Splitting
  * 6 Space
  * 7 Identifier completion
    * 7.1 Add semicolon
    * 7.2 Add assignment operator
    * 7.3 Automatically invoke after point
    * 7.4 Add parameter brackets
    * 7.5 Replace whole identifier



## General

### Additional Source search path for all projects/packages

If you are too lazy to setup packages and you do not want to share your projects/packages, then you can set here a global search path. Please note: "global" refers to codetools, and does not include the settings for the compiler. You have to add the path to the fpc.cfg as well. 

### Jumping (e.g. Method Jumping)

  * **Adjust top line due to comment in front** : When jumping from method declaration to method body, the IDE tries to position the source editor, so that the top line shown in the editor is the first line of the procedure. Normally a comment in front belongs to the procedure as well. Enable this option to scroll so that the comment is shown too.


  * **Center cursor line** : If jumping from the method body to the declaration in the class (or interface) the IDE can center line vertically in the editor.


  * **Cursor beyond EOL** : When the IDE jumps to a new position it is allowed to jump to a nice position, even if this is beyond the end of the line. This must be enabled for the source editor too.


  * **Skip forward declarations** : When doing a _find declaration_ the codetools searches upwards and stops on the first fit. This might be a forward declaration, e.g. _TControl = class;_. If this option is enabled the codetools will jump to the real class declaration instead.


  * **Jump directly to method body** : When _Find declaration_ jumps to a method/procedure declaration it takes an extra jump to the implementation.



### Indentation

Since 0.9.29 the source editor has a smarter auto indenter for pascal. It imitates your indentation. For example when you press return after a _try_ the indenter will search for other _try..finally_ blocks and indents accordingly. When pasting code from the clipboard the indenter will indent it too. 

  * **On break line** : Indent when pressing return and breaking the line. If disabled the default indenter of synedit is used, which indents as the line above.


  * **On paste form clipboard** : Indent when copying text from the clipboard. At the moment it only indents when inserting at column 1.


  * **Context sensitive** : The indenter searches in the surrounding code for similar code and copies the indentation. This means it searches first the code in front, then the code below. If it is a project unit, all project units are searched. If it is a package unit all package units are searched. Finally the example file is searched. If this option is disabled only the example file is searched.


  * **Example file** : This file contains beautiful code examples. You can edit the default file or select another. It can be a unit or program source.



For a detailed explanation see [Fully automatic indentation](<Fully_automatic_indentation.md> "Fully automatic indentation"). 

## Class Completion

### Insert class part

Defines where to insert new variables and method to the class declaration: 

  * Alphabetically
  * Last



### Insert methods

Where to insert new method bodies. Methods of the same class are always grouped together. 

  * Alphabetically
  * Last
  * Class order: Use the same order as in the class declaration



### Method default section

What class visibility should be used for new methods by default (since 1.8). 

  * Private
  * Protected
  * Public
  * Published



### Flags

  * Mix methods and properties: When adding a new method allow to insert between properties.
  * Update all method signatures: 
    * Enabled: Class completion updates all method signature (i.e. case, modifiers) from declaration to implementation.
    * Disabled: Class completion updates only the method signature under the cursor.
  * Header comment for class: Add a _{ TYourClass }_ comment in front of type.
  * Implementation comment for class: Add a _{ TYourClass }_ comment in front of the first method body.



### Header comment for class

Add a header comment in front of the class declaration. For example { TForm } 

### Implementation comment for class

Add a comment in front of the first method body. For example { TForm } 

### Property completion

  * Complete properties: Enable to complete incomplete property declarations.
  * Read Prefix - the 'Get' in 'function GetColor: TColor'
  * Write Prefix - the 'Set' in 'procedure SetColor(aValue: TColor)'
  * Stored Prefix - the 'IsStored' in 'function IsStoredColor';
  * Variable Prefix - The new private variable name is this prefix plus property name, e.g. the 'F' in 'FColor'.
  * Set property Variable - the parameter name for the setter, e.g. the 'aValue' in 'procedure SetColor(aValue: TColor)'. 
    * is prefix - enable to use aValueColor instead of aValue as parameter name. e.g. 'procedure SetColor(aValueColor: TColor)'.
    * use const - enable to use the 'const' accessor for the parameter. e.g. 'procedure SetColor(const aValue: TColor)'.



## Code Creation

In this section you can define, where (and in which order) new procedures and units will be inserted. 

### Procedure insert policy

Where to insert new procedure bodies 

  * Last (at end of source)
  * in front of methods
  * behind methods



### Keep order of procedures

When inserting new procedure bodies, keep order of the interface. 

## Words

### Keyword policy

How to write new keywords. 

### Identifier policy

How to write new identifiers. 

## Line Splitting

## Space

## Identifier completion

See the tutorial [Identifier completion](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools"). 

### Add semicolon

Allow to add missing semicolon. For example: 
    
    
     s:=Caption|  //

will add a semicolon. It does not add a semicolon if the identifier is a L-Value like in the following example: 
    
    
      Button1|

### Add assignment operator

This will add a **:=** if the identifier is a L-Value with no sub identifiers. For example: 
    
    
      Caption|  // Caption is a string, so Caption:=

### Automatically invoke after point

If enabled the identifier completion is automatically shown, when user pressed a point _._ and waited for the time shown in Editor / Automatic Features / Tooltip expression evaluation. For example: 
    
    
      Button1.|  // typing the point and wait will show the completion box

### Add parameter brackets

### Replace whole identifier

  * Enable: replace the whole identifier at cursor
  * Disable: replace only the identifier in front of the cursor

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Codetools_Options](https://web.archive.org/web/20200807155943/https://wiki.freepascal.org/IDE_Window%3A_Codetools_Options)_
