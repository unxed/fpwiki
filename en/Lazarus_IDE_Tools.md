# Lazarus IDE Tools

│ **English (en)** │  **[русский (ru)](<../ru/Lazarus_IDE_Tools.md>)** │

The **[Lazarus](<Lazarus_Faq.md> "Lazarus Faq") IDE Tools** is a library of [Free Pascal](<Free_Pascal.md> "Free Pascal") source parsing and editing tools, called the "codetools". 

These tools provide features like _Find Declaration_ , _Code Completion_ , _Extraction_ , _Moving Inserting_ and _Beautifying Pascal_ sources. These functions can save a lot of time and duplicated work. They are customizable, and every feature is available via shortcuts (see Editor Options). 

Because they work solely on Pascal sources and understand FPC, [Delphi](<Delphi.md> "Delphi") and [Kylix](<Kylix.md> "Kylix") code, they do not require compiled units or an installed Borland/Embarcadero compiler. Delphi and FPC code can be edited at the same time, with several Delphi and FPC versions. This makes porting Delphi code to FPC/Lazarus much easier. 

## Contents

  * 1 Summary Table of IDE shortcuts
  * 2 Method Jumping
  * 3 Include Files
  * 4 Code Templates
  * 5 Parameter Hints
  * 6 Incremental Search
    * 6.1 Hint: Quick searching an identifier with incremental search
  * 7 Syncro Edit
  * 8 Find next / previous word occurrence
  * 9 Code Completion
    * 9.1 Class Completion
    * 9.2 Forward Procedure Completion
    * 9.3 Event Assignment Completion
    * 9.4 Variable Declaration Completion
    * 9.5 Procedure Call Completion
    * 9.6 Reversed Class Completion
    * 9.7 Comments and Code Completion
    * 9.8 Method update
  * 10 Refactoring
    * 10.1 Invert Assignments
    * 10.2 Enclose Selection
    * 10.3 Rename Identifier
    * 10.4 Find Identifier References
    * 10.5 Show abstract methods
    * 10.6 Extract Procedure
  * 11 Find Declaration
    * 11.1 Hints
  * 12 Identifier Completion
    * 12.1 Matching only the first part of a word
    * 12.2 Keys
    * 12.3 Methods
    * 12.4 Properties
    * 12.5 Uses section / Unit names
    * 12.6 Statements
    * 12.7 Icons in completion window
  * 13 Word Completion
  * 14 Unit / Identifier Dictionary (Cody)
  * 15 Goto Include Directive
  * 16 Publish Project
  * 17 Hints from comments
    * 17.1 Comments shown in the hint
    * 17.2 Comments not shown in the hint
  * 18 Quick Fixes
  * 19 Outline



## Summary Table of IDE shortcuts

[Declaration Jumping](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+Click or `Alt`+`↑` (jump to declaration of type or variable)   
---|---  
[Method Jumping](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`⇧ Shift`+`↑` (toggle between definition and body)   
[Code Templates](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`J`  
[Syncro Edit](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`J` (while text is selected)   
[Code Completion](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") (Class Completion) | `Ctrl`+`⇧ Shift`+`C`, `Ctrl`+`⇧ Shift`+`X` for creating class fields instead of local variables   
[Identifier Completion](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`space`  
[Word Completion](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`W`  
[Parameter Hints](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`⇧ Shift`+`space`  
[Incremental Search](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`E`  
[Rename Identifier](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") | `Ctrl`+`⇧ Shift`+`E`  
  
## Method Jumping

To jump between a procedure body (begin..end) and the procedure definition (procedure Name;) use `Ctrl`+`⇧ Shift`+`↑`. 

For example: 
    
    
    interface
     
    procedure DoSomething; // procedure definition
      
    implementation
      
    procedure DoSomething; // procedure body 
    begin
    end;
    

If the cursor is on the procedure body and `Ctrl`+`⇧ Shift`+`↑` is pressed, the cursor will jump to the definition. Pressing `Ctrl`+`⇧ Shift`+`↑` again will jump to the body, after 'begin'. 

This works between methods (procedures in classes) as well. 

Hints: 'Method Jumping' jumps to the same procedure with the same name and parameter list. If there is no exact procedure, it jumps to the best candidate and positions the cursor on the first difference. (For Delphians: Delphi can not do this). 

For example a procedure with different parameter types: 
    
    
    interface
     
    procedure DoSomething(p: char); // procedure definition
     
    implementation
       
    procedure DoSomething(p: string); // procedure body
    begin
    end;
    

Jumping from the definition to the body will position the cursor at the 'string' keyword. This can be used for renaming methods and/or changing the parameters. 

For example: 

You renamed 'DoSomething' to 'MakeIt': 
    
    
    interface
     
    procedure MakeIt; // procedure definition
     
    implementation
     
    procedure DoSomething; // procedure body
    begin
    end;
    

Then you jump from MakeIt to the body. The IDE searches for a fitting body, does not find one, and hence searches for a candidate. Since you renamed only one procedure there is exactly one body without definition (DoSomething) and so it will jump to DoSomething and position the cursor right on 'DoSomething'. Then you can simply rename it there too. This works for parameters as well. 

## Include Files

Include files are files inserted into sources with the {$I filename} or {$INCLUDE filename} compiler directive. Lazarus and FPC often uses include files to reduce redundancy and avoid unreadable {$IFDEF} constructs, needed to support different platforms. 

Contrary to Delphi, the Lazarus IDE has full support for include files. You can for example jump from the method in the .pas file to the method body in the include file. All codetools like code completion consider include files as special bounds. 

For instance: When code completion adds a new method body behind another method body, it keeps them both in the same file. This way you can put whole class implementations in include files, as the LCL does for nearly all controls. 

But there is a trap for newbies: If you open an include file for the first time and try method jumping or find declaration you will get an error. The IDE does not yet know to which unit the include file belongs. You must open the unit first. 

As soon as the IDE parses the unit, it will parse the include directives there and the IDE will remember this relationship. It saves this information on exit and on project save to ~/.lazarus/includelinks.xml. The next time you open this include file and jump or do a find declaration, the IDE will internally open the unit and the jump will work. You can also hint the IDE by putting `{%mainunit yourunit.pas}` at the top of yourinclude.inc. 

This mechanism has limits. Some include files are included twice or more. For example: lcl/include/winapih.inc. 

How you will jump from the procedure/method definitions in this include file to their bodies will depend on your last action. If you worked on lcl/lclintf.pp the IDE will jump to winapi.inc. If you worked on lcl/interfacebase.pp, then it will jump to lcl/include/interfacebase.inc (or one of the other include files). If you are working on both, then you can get confused. ;) 

## Code Templates

Code Templates converts an identifier into a text or code fragment. 

Code Templates default short cut is `Ctrl`+`J`. You can type an identifier, press `Ctrl`+`J` and the identifier is replaced by the text defined for the identifier. Code Templates can be defined in Tools -> Options -> CodeTools. 

Example: Write the identifier 'classf', leave the cursor right behind the 'f' and press `Ctrl`+`J`. The 'classf' will be replaced by 
    
    
    T = class(T)
    private
     
    public
      constructor Create;
      destructor Destroy; override;
    end;
    

and the cursor is behind the 'T'. 

You can get the list of templates by positioning the cursor on space (not on an identifier) and pressing `Ctrl`+`J`. The list of code templates will pop up. Use the cursor keys or type some chars to choose one. Return creates the selected template and Escape closes the pop up. 

The biggest time savers are templates 'b'+`Ctrl`+`J` for begin..end. 

## Parameter Hints

Parameter Hints shows a hint box with the parameter declarations for the current parameter list. 

For example: `Canvas.FillRect(|);`

Place the cursor in the brackets and press `Ctrl`+`⇧ Shift`+`space`. A hint box will show up showing the parameters of FillRect. 

[![Parameterhints1.png](https://wiki.freepascal.org/images/c/c8/Parameterhints1.png)](</File:Parameterhints1.png>)

Since 0.9.31 there is a button to the right of each declaration to insert the missing parameters.This will copy the parameter names from the chosen declaration to the cursor position. 

[![Parameterhints2.png](https://wiki.freepascal.org/images/4/48/Parameterhints2.png)](</File:Parameterhints2.png>)

Hint: Use the Variable Declaration Completion to declare the variables. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The short cut's name is "Show code context".

## Incremental Search

Incremental Search changes the statusbar of the source editor. Type some characters and the editor will search and highlight immediately all occurrences in the text. Shortcut is `Ctrl`+`e`. 

  * For example pressing `e` will search and highlight all occurrences of 'e'.
  * Then pressing `t` will search and highlight all occurrences of 'et' and so forth.
  * You can jump to the next with `F3` (or `Ctrl`+`e` while in search) and the previous with `⇧ Shift`+`F3`.
  * Backspace deletes the last character
  * return stops the search without adding a new line in the editor.
  * You can resume the last search by pressing `Ctrl`+`e` a second time, immediately after you started incr-search with `Ctrl`+`e`. that is while the search term is still empty.
  * Paste `Ctrl`+`V` will append the text from the clipboard to the current search text (since lazarus 0.9.27 r19824).



### Hint: Quick searching an identifier with incremental search

  * Place text cursor on identifier (do not select anything)
  * Press `Ctrl`+`C`. The source editor will select the identifier and copy it to the clipboard
  * Press `Ctrl`+`E` to start incremental search
  * Press `Ctrl`+`V` to search for the identifier (since 0.9.27)
  * Use `F3` and `⇧ Shift`+`F3` to quickly jump to next/previous.
  * Use any key (for example cursor left or right) to end the search



## Syncro Edit

Syncro Edit allows you to edit all occurrences of a word at the same time (synchronized). You simple edit the word in one place, and as you type, all other occurrences of the word are updated too. 

Syncro Edit works on all words in a selected area: 

  * Select a block of text
  * press `Ctrl`+`J` or click the icon in the gutter. (This only works, if there are any words that occur more than once in the selection.
  * use the `Tab ⇆` key to select the word you want to edit (if several different words occurred more than once)
  * Edit the word
  * Press `Esc` to finish



See an animated example [here](<New_IDE_features_since.md> "New IDE features since")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** `Ctrl`+`J` is also used for template edit. It switches its meaning if you select some text. 

## Find next / previous word occurrence

The two functions can be found in the popup menu of the source editor 

  * Source editor / popup menu / Find / Find next word occurrence
  * Source editor / popup menu / Find / Find previous word occurrence



And you can assign them shortcuts in the editor options. 

## Code Completion

Code Completion can be found in the IDE menu Edit -> Complete Code and has as standard short cut `Ctrl`+`⇧ Shift`+`C`. 

For Delphians: Delphi calls "code completion" the function showing the list of identifiers at the current source position (`Ctrl`+`Space`). Under Lazarus this is called "Identifier completion". 

Code Completion combines several powerful functions. Examples: 

  * Class Completion: completes properties, adds/updates method bodies, add private variables and private access methods
  * Forward Procedure Completion: adds procedure bodies
  * Event Assignment Completion: completes event assignments and adds method definition and body
  * Variable Declaration Completion: adds local variable definitions
  * Procedure Call Completion: adds a new procedure
  * Reversed procedure completion: adds procedure declarations for procedure/function bodies
  * Reversed Class Completion: adds method declarations for method bodies



Which function is used, depends on the cursor position in the editor and will be explained below. 

Code Completion can be found in the IDE menu Edit -> Complete Code and has as standard short cut `Ctrl`+`⇧ Shift`+`C`. 

### Class Completion

The most powerful code completion feature is "Class Completion". You write a class, add the methods and properties and Code Completion will add the method bodies, the property access methods/variables and the private variables. 

For example: Create a class (see Code Templates to save you some type work): 
    
    
    TExample = class(TObject)
    public
      constructor Create;
      destructor Destroy; override;
    end;
    

Position the cursor somewhere in the class and press `Ctrl`+`⇧ Shift`+`C`. This will create the method missing bodies and move the cursor to the first created method body, so you can just start writing the class code: 
    
    
    { TExample }
     
    constructor TExample.Create;
    begin
      |
    end;
     
    destructor TExample.Destroy;
    begin
      inherited Destroy;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The '|' is the cursor and is not added.

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** You can jump between a method and its body with `Ctrl`+`⇧ Shift`+`↑`. 

You can see, that the IDE added the 'inherited Destroy' call too. This is done, if there is an 'override' keyword in the class definition. 

Now add a method DoSomething: 
    
    
    TExample = class(TObject)
    public
      constructor Create;
      procedure DoSomething(i: integer);
      destructor Destroy; override;
    end;
    

Then press `Ctrl`+`⇧ Shift`+`C` and the IDE will add 
    
    
    procedure TExample.DoSomething(i: integer);
    begin
      |
    end;
    

You can see, that the new method body is inserted between Create and Destroy, exactly as in the class definition. This way the bodies keep the same logical ordering as you define. You can define the insertion policy in Tools > Options -> Codetools -> Code Creation. 

**Complete Properties**

Add a property AnInteger: 
    
    
    TExample = class(TObject)
    public
      constructor Create;
      procedure DoSomething(i: integer);
      destructor Destroy; override;
      property AnInteger: Integer;
    end;
    

Press `Ctrl`+`⇧ Shift`+`C` and you will get: 
    
    
    procedure TExample.SetAnInteger(const AValue: integer);
    begin
      |if FAnInteger=AValue then exit;
      FAnInteger:=AValue;
    end;
    

The code completion has added a Write access modifier and added some common code. Jump to the class with `Ctrl`+`⇧ Shift`+`↑` to see the new class: 
    
    
    TExample = class(TObject)
    private
      FAnInteger: integer;
      procedure SetAnInteger(const AValue: integer);
    public
      constructor Create;
      procedure DoSomething(i: integer);
      destructor Destroy; override;
      property AnInteger: integer read FAnInteger write SetAnInteger;
    end;
    

The property was extended by a Read and Write access modifier. The class got the new section 'private' with a Variable 'FAnInteger' and the method 'SetAnInteger'. It is a common Delphi style rule to prepend private variables with an 'F' and the write method with a 'Set'. If you don't like that, you can change this in Tools -> Options > Codetools -> Code Creation. 

Creating a read only property: 
    
    
    property PropName: PropType read;
    

Will be expanded to 
    
    
    property PropName: PropType read FPropName;
    

Creating a write only property: 
    
    
    property PropName: PropType write;
    

Will be expanded to 
    
    
    property PropName: PropType write SetPropName;
    

Creating a read only property with a Read method: 
    
    
    property PropName: PropType read GetPropName;
    

Will be kept and a GetPropName function will be added: 
    
    
    function GetpropName: PropType;
    

Creating a property with a stored modifier: 
    
    
    property PropName: PropType stored;
    

Will be expanded to 
    
    
    property PropName: PropType read FPropName write SetPropName stored PropNameIsStored;
    

Because stored is used for streaming read and write modifiers are automatically added as well. 

Hint: Identifier completion also recognizes incomplete properties and will suggest the default names. For example: 
    
    
    property PropName: PropType read |;
    

Place the cursor one space behind the 'read' keyword and press `Ctrl`+`Space` for the identifier completion. It will present you the variable 'FPropName' and the procedure 'SetPropName'. 

### Forward Procedure Completion

"Forward Procedure Completion" is part of the Code Completion and adds missing procedure bodies. It is invoked, when the cursor is on a forward defined procedure. 

For example: Add a new procedure to the interface section: 
    
    
    procedure DoSomething;
    

Place the cursor on it and press `Ctrl`+`⇧ Shift`+`C` for code completion. It will create in the implementation section: 
    
    
    procedure DoSomething;
    begin
      |
    end;
    

Hint: You can jump between a procedure definition and its body with `Ctrl`+`⇧ Shift`+`↑`. 

The new procedure body will be added in front of the class methods. If there are already some procedures in the interface the IDE tries to keep the ordering. For example: 
    
    
    procedure Proc1;
    procedure Proc2; // new proc
    procedure Proc3;
    

If the bodies of Proc1 and Proc3 already exists, then the Proc2 body will be inserted between the bodies of Proc1 and Proc3. This behaviour can be setup in Tools -> Options -> Codetools -> Code Creation. 

Multiple procedures: 
    
    
    procedure Proc1_Old; // body exists
    procedure Proc2_New; // body does not exist
    procedure Proc3_New; //  "
    procedure Proc4_New; //  "
    procedure Proc5_Old; // body exists
    

Code Completion will add all 3 procedure bodies (Proc2_New, Proc3_New, Proc4_New). 

Why is it called "Forward Procedure Completion"? 

Because it does not only work for procedures defined in the interface, but for procedures with the "forward" modifier as well. And because the codetools treats procedures in the interface as having an implicit 'forward' modifier. 

### Event Assignment Completion

"Event Assignment Completion" is part of the Code Completion and completes a single Event:=| statement. It is invoked, when the cursor is behind an assignment to an event. 

For example: In a method, say the FormCreate event, add a line 'OnPaint:=': 
    
    
    procedure TForm1.Form1Create(Sender: TObject);
    begin
      OnPaint:=|
    end;
    

The '|' is the cursor and should not be typed. Then press `Ctrl`+`⇧ Shift`+`C` for code completion. The statement will be completed to 
    
    
    OnPaint:=@Form1Paint;
    

A new method Form1Paint will be added to the TForm1 class. Then class completion is started and you get: 
    
    
    procedure TForm1.Form1Paint(Sender: TObject);
    begin
      |
    end;
    

This works just like adding methods in the object inspector. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You must place the cursor just after the ':=' assignment operator. If you place the cursor on the identifier (e.g. OnPaint) code completion will invoke "Local Variable Completion", which fails, because OnPaint is already defined.

Hints: 

  * You can choose the default visibility of the new method in Tools / Options / Codetools / Class Completion / Default section of methods (since 1.8)
  * You can define the new method name by yourself. For example: `OnPaint:=@ThePaintMethod;`.



Since 0.9.31 Lazarus completes procedure parameters. For example 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      List: TList;
    begin
      List:=TList.Create;
      List.Sort(@MySortFunction|);
    end;
    

Place the cursor on 'MySortFunction' and press `Ctrl`+`⇧ Shift`+`C` for code completion. You get a new procedure: 
    
    
    function MySortFunction(Item1, Item2: Pointer): Integer;
    begin
      |
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      List: TList;
    begin
      List:=TList.Create;
      List.Sort(@MySortFunction);
    end;
    

### Variable Declaration Completion

"Variable Declaration Completion" is part of the Code Completion and adds a local variable definition for a Identifier:=Term; statement. It is invoked, when the cursor is on the identifier of an assignment or a parameter. 

For example: 
    
    
    procedure TForm1.Form1Create(Sender: TObject);
    begin
      i:=3;
    end;
    

Place the cursor on the 'i' or just behind it. Then press `Ctrl`+`⇧ Shift`+`C` for code completion and you will get: 
    
    
    procedure TForm1.Form1Create(Sender: TObject);
    var
      i: Integer;
    begin
      i:=3;
    end;
    

The codetools first checks, if the identifier 'i' is already defined and if not it will add the declaration 'var i: integer;'. The type of the identifier is guessed from the term right to the assignment ':=' operator. Numbers like the 3 defaults to Integer. 

Another example: 
    
    
    type
      TWhere = (Behind, Middle, InFront);
     
      procedure TForm1.Form1Create(Sender: TObject);
      var
        a: array[TWhere] of char;
      begin
        for Where:=Low(a) to High(a) do writeln(a[Where]);
      end;
    

Place the cursor on 'Where' and press `Ctrl`+`⇧ Shift`+`C` for code completion. You get: 
    
    
      procedure TForm1.Form1Create(Sender: TObject);
      var
        a: array[TWhere] of char;
        Where: TWhere;
      begin
        for Where:=Low(a) to High(a) do writeln(a[Where]);
      end;
    

Since 0.9.11 Lazarus also completes parameters. For example 
    
    
      procedure TForm1.FormPaint(Sender: TObject);
      begin
        with Canvas do begin
          Line(x1,y1,x2,y2);
        end;
      end;
    

Place the cursor on 'x1' and press `Ctrl`+`⇧ Shift`+`C` for code completion. You get: 
    
    
      procedure TForm1.FormPaint(Sender: TObject);
      var
        x1: integer;
      begin
        with Canvas do begin
          Line(x1,y1,x2,y2);
        end;
      end;
    

Since 0.9.31 Lazarus completes pointer parameters. For example 
    
    
      procedure TForm1.FormCreate(Sender: TObject);
      begin
        CreateIconIndirect(@IconInfo);
      end;
    

Place the cursor on 'IconInfo' and press `Ctrl`+`⇧ Shift`+`C` for code completion. You get: 
    
    
      procedure TForm1.FormCreate(Sender: TObject);
      var
        IconInfo: TIconInfo;
      begin
        CreateIconIndirect(@IconInfo);
      end;
    

In all above examples you can use `Ctrl`+`⇧ Shift`+`X` to show a Code Creation dialog where you can set more options. 

### Procedure Call Completion

Code completion can create a new procedure from a call statement itself. 

Assume you just wrote the statement "DoSomething(Width);" 
    
    
    procedure SomeProcedure;
    var
      Width: integer;
    begin
      Width:=3;
      DoSomething(Width);
    end;
    

Position the cursor over the identifier "DoSomething" and press `Ctrl`+`⇧ Shift`+`C` to get: 
    
    
    procedure DoSomething(aWidth: LongInt);
    begin
    
    end;
    
    procedure SomeProcedure;
    var
      Width: integer;
    begin
      Width:=3;
      DoSomething(Width);
    end;
    

It does not yet create functions nor methods. 

### Reversed Class Completion

"Reversed Class Completion" is part of the **Code Completion** and adds a private method declaration for the current method body. It is invoked, when the cursor is in a method body, not yet defined in the class. This feature is available since Lazarus 0.9.21. 

For example: 
    
    
      procedure TForm1.DoSomething(Sender: TObject);
      begin
      end;
    

The method DoSomething is not yet declared in TForm1. Press `Ctrl`+`⇧ Shift`+`C` and the IDE will add "procedure DoSomething(Sender: TObject);" to the private methods of TForm1. 

For Delphians: Class completion works under Lazarus always in one way: From class interface to implementation or backwards/reversed from class implementation to interface. Delphi always invokes both directions. The Delphi way has the disadvantage, that if a typo will easily create a new method stub without noticing. 

### Comments and Code Completion

Code completion tries to keep comments where they belong. For example: 
    
    
      FList: TList; // list of TComponent
      FInt: integer;
    

When inserting a new variable between FList and FInt, the comment is kept in the FList line. Same is true for 
    
    
      FList: TList; { list of TComponent
        This is a comment over several lines, starting
        in the FList line, so codetools assumes it belongs 
        to the FLIst line and will not break this 
        relationship. Code is inserted behind the comment. }
      FInt: integer;
    

If the comment starts in the next line, then it will be treated as if it belongs to the code below. For example: 
    
    
      FList: TList; // list of TComponent
        { This comment belongs to the statement below. 
          New code is inserted above this comment and 
          behind the comment of the FList line. }
      FInt: integer;
    

### Method update

Normally class completion will add all missing method bodies. (Since 0.9.27) But if exactly one method differ between class and bodies then the method body is updated. For example: You have a method _DoSomething_. 
    
    
      public
        procedure DoSomething;
      end;
    
    procedure TForm.DoSomething;
    begin
    end;
    

Now add a parameter: 
    
    
      public
        procedure DoSomething(i: integer);
      end;
    

and invoke Code Completion (`Ctrl`+`⇧ Shift`+`C`). The method body will be updated and the new parameter will be copied: 
    
    
    procedure TForm.DoSomething(i: integer);
    begin
    end;
    

## Refactoring

### Invert Assignments

Abstract
    : "Invert Assignments" takes some selected pascal statements and inverts all assignments from this code. This tool is usefull for transforming a "save" code to a "load" one and inverse operation.

Example: 
    
    
    procedure DoSomething;
    begin
      AValueStudio:= BValueStudio;
      AValueAppartment :=BValueAppartment;
      AValueHouse:=BValueHouse;
    end;
    

Select the lines with assignments (between begin and end) and do Invert Assignments. All assignments will be inverted and identation will be add automatically. For example: 

Result: 
    
    
    procedure DoSomething;
    begin
      BValueStudio     := AValueStudio;
      BValueAppartment := AValueAppartment;
      BValueHouse      := AValueHouse;
    end;
    

### Enclose Selection

Select some text and invoke it. A dialog will popup where you can select if the selection should be enclosed into **try..finally** or many other common blocks. 

### Rename Identifier

Place the cursor on an identifier and invoke it. A dialog will appear, where you can setup the search scope and the new name. 

  * It will rename all occurences and only those that actually use this declaration. That means it does not rename declarations with the same name.
  * And it will first check for name conflicts.
  * Limits: It only works on pascal sources, does not yet rename files nor adapt lfm/lrs files nor lazdoc files.



### Find Identifier References

Place the cursor on an identifier and invoke it. A dialog will appear, where you can setup the search scope. The IDE will then search for all occurences and only those that actually use this declaration. That means it does not show other declarations with the same name. 

### Show abstract methods

This feature lists and auto completes virtual, abstracts methods that need to be implemented. Place the cursor on a class declaration and invoke it. If there are missing abstract methods a dialog will appear listing them. Select the methods to implement and the IDE creates the method stubs. Since Lazarus 1.3 it adds missing _class interface_ methods too. 

### Extract Procedure

See [Extract Procedure](<IDE_Window__Extract_Procedure.md> "IDE Window: Extract Procedure"). 

## Find Declaration

Position the cursor on an identifier and do 'Find Declaration'. Then it will search the declaration of this identifier, open the file and jump to it. If the cursor is already at a declaration it will jump to the previous declaration with the same name. This allows to find redefinitions and overrides. 

Every find declaration sets a Jump Point. That means you jump with find declaration to the declaration and easily jump back with Search -> Jump back. 

There are some differences to Delphi: the codetools work on sources following the normal pascal rules, instead of using the compiler output. The compiler returns the final type. The codetools see the sources and all steps in between. For example: 

The _Visible_ property is first defined in _TControl_ (controls.pp), then redefined in TCustomForm and finally redefined in TForm. Invoking find declaration on Visible will you first bring to Visible in TForm. Then you can invoke Find Declaration again to jump to Visible in TCustomForm and again to jump to Visible in TControl. 

Same is true for types like _TColor_. For the compiler it is simply a 'longint'. But in the sources it is defined as 
    
    
    TGraphicsColor = -$7FFFFFFF-1..$7FFFFFFF;
    TColor = TGraphicsColor;
    

And the same for **forward defined classes** : for instance in _TControl_ , there is a private variable 
    
    
    FHostDockSite: TWinControl;
    

Find declaration on TWinControl jumps to the forward definition 
    
    
    TWinControl = class;
    

And invoking it again jumps to the real implementation 
    
    
    TWinControl = class(TControl)
    

This way you can track down every identifier and find every overload. 

### Hints

  * ump back with `Ctrl`+`H`.
  * view/navigate all visited locations via Menu: View -> "jump history"
  * With a 5 button Mouse the 2 extra buttons to go forward/backward between the visited points



    using [advanced mouse options](<IDE_Window__EditorMouseOptionsAdvanced.md> "IDE Window: EditorMouseOptionsAdvanced") the buttons can be remapped.

## Identifier Completion

"Identifier Completion" is invoked by `Ctrl`+`space`. It shows all identifiers in scope. For example: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      |
    end;
    

Place the cursor between _begin_ and _end_ and press `Ctrl`+`space`. The IDE/CodeTools will now parse all reachable code and present you a list of all found identifiers. The CodeTools cache the results, so invoking it a second time will be much faster. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** for Delphians: Delphi calls it _Code completion_.

Some identifiers like 'Write', 'ReadLn', 'Low', 'SetLength', 'Self', 'Result', 'Copy' are built into the compiler and are not defined anywhere in source. The identifier completion has a lot of these things built in as well. If you find one missing, just create a feature request in the bug tracker. 

_Identifier completion_ does not complete all **keywords**. So you can not use it to complete 'repe' to 'repeat'. For these things use `Ctrl`+`W` Word Completion or `Ctrl`+`J` Code Templates. Since 0.9.27 _identifier completion_ completes some keywords. 

Identifier completion shows even those identifiers, that are not compatible. 

### Matching only the first part of a word

You can invoke identifier completion only on the first few characters in a word. Position the cursor within a word. _Only characters to the left of the cursor_ will be used to look up identifiers. For example: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Ca|ption
    end;
    

The box will show you only identifiers beginning with 'Ca' ( | indicates the cursor position). 

### Keys

  * Letter or number: add the character to the source editor and the current prefix. This will update the list.
  * Backspace: remove the last character from source editor and prefix. Updates the list.
  * Return: replace the whole word at cursor with the selected identifier and close the popup window.
  * Shift+Return: as _Return_ , but replaces only the prefix (left part) of the word at the cursor.
  * Up/Down: move selection
  * Escape: close popup without change
  * Tab: completes the prefix to next choice. For example: The current prefix is 'But' and the identifier completion only shows 'Button1' and 'Button1Click'. Then pressing _Tab_ will complete the prefix to 'Button1'.
  * Else: as _Return_ and add the character to the source editor



### Methods

When cursor is in a class definition and you identifier complete a method defined in an ancestor class the parameters and the override keyword will be added automatically. For example: 
    
    
    TMainForm = class(TForm)
    protected
      mous|
    end;
    

Completing **MouseDown** gives: 
    
    
    TMainForm = class(TForm)
    protected
      procedure MouseDown(Button: TMouseButton; Shift: TShiftState; X,
             Y: Integer); override;
    end;
    

### Properties
    
    
    property MyInt: integer read |;
    

Identifier completion will show **FMyInt** and **GetMyInt**. 
    
    
    property MyInt: integer write |;
    

Identifier completion will show **FMyInt** and **SetMyInt**. 

### Uses section / Unit names

In uses sections the identifier completion will show the filenames of all units in the search path. These will show all lowercase (e.g. _avl_tree_), because most units have lowercase filenames. On completion it will insert the case of the unit (e.g. _AVL_Tree_). 

### Statements
    
    
    procedure TMainForm.Button1Click(Sender: TObject);
    begin
      ModalRe|;
    end;
    

becomes: 
    
    
    procedure TMainForm.Button1Click(Sender: TObject);
    begin
      ModalResult:=|;
    end;
    

### Icons in completion window

In Lazarus 1.9+, option exists to show icons instead of "types", for lines in the completion window. Picture shows these icons: 

[![ide completion icons.png](https://wiki.freepascal.org/images/2/21/ide_completion_icons.png)](</File:ide_completion_icons.png>)

## Word Completion

_Word Completion_ is invoked by `Ctrl`+`W`. It shows all words of all currently open editors and can therefore be used in non pascal sources, in comments and for keywords. 

Otherwise it works the same as identifier completion. 

## Unit / Identifier Dictionary (Cody)

This dialog lets you search for identifiers in other units. The units do not yet have to be used by your code. 

This feature is part of the package "Cody". To activate the feature install the package. 

## Goto Include Directive

"Goto Include Directive" in the search menu of the IDE jumps to {$I filename} statement where the current include file is used. 

## Publish Project

Creates a copy of the whole project. If you want to send someone just the sources and compiler settings of your code, this function is your friend. 

A normal project directory contains a lot of information. Most of it is not needed to be published: the .lpi file can contain session information (like caret position and bookmarks of closed units) and the project directory contains a lot of .ppu, .o files and the executable. To create a lpi file with only the base information and only the sources, along with all sub directories use "Publish Project". 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Since version 0.9.13 there is a new _Project Option_ that allows you to store session information in a separate file from the normal .lpi file. This new file ends with the .lps extension and only contains session information, which will leave your .lpi file much cleaner.

In the dialog you can setup a filter to include and exclude certain files; with the command after you can compress the output into one archive. 

## Hints from comments

At several places the IDE shows hints for an identifier. For example when moving the mouse over an identifier in the source editor and waiting a few seconds. When the IDE shows a hint for an identifier it searches the declaration and all its ancestors and looks for comments and fpdoc files. There are many coding styles and many commenting styles. In order to support many of the common comment styles the IDE uses the following heuristics: 

### Comments shown in the hint

Comments in front of a declaration, without empty line and not starting with the _<_ sign: 
    
    
    var
      {Comment}
      Identifier: integer;
    

Comments with the _<_ sign belong to the prior identifier. 

Comments behind an identifier on the same line: 
    
    
    var 
      identifier, // Comment
      other,
    

Comments behind the definition on the same line: 
    
    
    var
      identifier: 
        char; // Comment
    

An example for **<** sign: 
    
    
    const
      a = 1;
      //< comment for a
      b = 2;
      // comment for c
      c = 3;
    

All three comment types are supported: 
    
    
      {Comment}(*Comment*)//Comment
      c = 1;
    

### Comments not shown in the hint

Comments starting with **$** or **%** are ignored. For example _//% Hiddden_ , _//$ Hidden_ , _(*$ Hidden*)_. 

Comments in front separated with an empty line are treated as not specific to the following identifier. For example the following class header comment is not shown in the hint: 
    
    
    type
      { TMyClass }
      
      TMyClass = class
    

The class header comments are created on class completion. You can turn this off in the _Options / Codetools / Class completion / Header comment for class_. If you want to show the header comment in the hint, just remove the empty line. 

The following comment will be shown for GL_TRUE, but not for GL_FALSE: 
    
    
      // Boolean
      GL_TRUE                           = 1;
      GL_FALSE                          = 0;
    

## Quick Fixes

Quick Fixes are menu items for specific compiler messages. They help you to quickly fix the problem. Select a message in the _Messages_ window and right click, or right click in the source editor on the icon to the left. 

  * Unit not found: remove from uses section
  * Unit not found: find unit in loaded packages and allow to auto add package dependency
  * Constructing a class "$1" with abstract method "$2": show dialog to override all abstract methods
  * Local variable "$1" not used: remove definition
  * Circular unit reference between $1 and $2: show Unit Dependencies dialog with full path between the two units
  * Identifier not found: search via Code Browser
  * Identifier not found: search via Cody Dictionary (needs package [Cody](<Cody.md> "Cody"))
  * Identifier not found: add local variable
  * Recompiling $1, checksum changed for $2: show a dialog with search paths and other information
  * IDE warning: other sources path of package %s contains directory...: open package
  * any hint, note, warning: add IDE directive {%H-}
  * any hint, note, warning: add compiler directive {$warn id off} (since 1.7)
  * any hint, note, warning: add compiler option -vm<messageid>
  * Local variable "i" does not seem to be initialized: insert assignment (since 1.5)
  * Inherited method is hidden: add modifier override, overload, or reintroduce (since 1.9)



## Outline

This option is located in the IDE Options dialog, Editor - Display - Markup and Matches - Outline (global). 

It makes highlighting of Pascal keywords together with their begin-end brackets. This allows to see nesting of big blocks. For example, for such procedure body: 
    
    
      with ADockObject do
      begin
        if DropAlign = alNone then
        begin
          if DropOnControl <> nil then
            DropAlign := DropOnControl.GetDockEdge(DropOnControl.ScreenToClient(DragPos))
          else
            DropAlign := Control.GetDockEdge(DragTargetPos);
        end;
        PositionDockRect(Control, DropOnControl, DropAlign, FDockRect);
      end;
    

it highlights: 

  * outer with-do-begin-end in orange
  * next if-then-begin-end in green
  * inner if-then-else in cyan



[![ide outline.png](https://wiki.freepascal.org/images/f/fb/ide_outline.png)](</File:ide_outline.png>)

---

_Source: [https://wiki.freepascal.org/Lazarus_IDE_Tools](https://web.archive.org/web/20240301084250/https://wiki.freepascal.org/Lazarus_IDE_Tools)_
