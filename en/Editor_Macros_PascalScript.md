# Editor Macros PascalScript

│ **English (en)** │  **[русский (ru)](<../ru/Editor_Macros_PascalScript.md>)** │

## Contents

  * 1 General
  * 2 Availability
  * 3 Simple actions
  * 4 Functions
  * 5 Objects provided
    * 5.1 Caller: TSynEdit
    * 5.2 ClipBoard: TClipBoard
  * 6 Available Macros / Downloads
  * 7 See also



## General

The [PascalScript](<PascalScript.md> "PascalScript") macro functionality is available in Lazarus 1.1 and later. To use the feature you have to install the package EditorMacroScript, which includes the [Pascal Script](<Pascal_Script.md> "Pascal Script") package. Pascal Script is provided by REM Objects. A minimum package is provided with the Lazarus 1.1 distribution. 

## Availability

This functionality is only available on selected platforms/architectures: 

  * Windows 
    * 32/64bit Intel/AMD
  * Linux 
    * 32/64bit Intel/AMD
  * Mac 
    * 32(/maybe 64)bit Intel/AMD
    * 32bit PPC



## Simple actions

All simple Keyboard actions are represented as follows: 

ecLeft;
    Move Caret one to the left (in the editor that invoked the macro)
ecChar('a');
    Inserts an 'a'

See the unit SynEditKeyCmds in package [SynEdit](<SynEdit.md> "SynEdit"), and IDECommands in IDEIntf for a full list. Or use the Recorder to get the names of actions. 

## Functions
    
    
        Function MessageDlg( const Msg: string; DlgType: TMsgDlgType; Buttons: TMsgDlgButtons; HelpCtx: Longint ): Integer;
        Function MessageDlgPos( const Msg: string; DlgType: TMsgDlgType; Buttons: TMsgDlgButtons; HelpCtx: Longint; X, Y: Integer): Integer;
        Function MessageDlgPosHelp( const Msg: string; DlgType: TMsgDlgType; Buttons: TMsgDlgButtons; HelpCtx: Longint; X, Y: Integer; const HelpFileName: string): Integer;
        Procedure ShowMessage( const Msg: string );
        Procedure ShowMessagePos( const Msg: string; X, Y  Integer );
        Function InputBox( const ACaption, APrompt, ADefault: string): string;
        Function InputQuery( const ACaption, APrompt: string; var Value: string): Boolean;
    

## Objects provided

Scripts can refer to the invoking SynEdit via the identifier "Caller". 

### Caller: TSynEdit

The following methods and properties are available: 

Caret
    
    
    
        property CaretX: Integer;
        property CaretY: Integer;
        property CaretXY: TPoint;
        property LogicalCaretXY: TPoint;
        property LogicalCaretX: Integer;
        procedure MoveCaretIgnoreEOL(const NewCaret: TPoint);
        procedure MoveLogicalCaretIgnoreEOL(const NewLogCaret: TPoint);
    

Selection
    
    
    
        property BlockBegin: TPoint;
        property BlockEnd: TPoint;
        property SelAvail: Boolean; // read only
        property SelText: string;
        property SelectionMode: TSynSelectionMode;
        procedure ClearSelection;
        procedure SelectAll;
        procedure SelectToBrace;
        procedure SelectWord;
        procedure SelectLine(WithLeadSpaces: Boolean);
        procedure SelectParagraph;
    

Search/Replace
    
    
    
        function SearchReplace(const ASearch, AReplace: string; AOptions: TSynSearchOptions): integer;
        function SearchReplaceEx(const ASearch, AReplace: string; AOptions: TSynSearchOptions; AStart: TPoint): integer;
    
        TSynSearchOptions = set of
        ( ssoMatchCase, ssoWholeWord,
          ssoBackwards,
          ssoEntireScope, ssoSelectedOnly,  // Default is From Caret to End-of-text (or begin-of-text if backward) 
          ssoReplace, ssoReplaceAll,        // Otherwise the function does a search only
          ssoPrompt,                        // Show the prompt before replacing
          ssoSearchInReplacement,           // continue search-replace in replacement (with ssoReplaceAll) // replace recursive
          ssoRegExpr, ssoRegExprMultiLine,
          ssoFindContinue                   // Assume the current selection is the last match, and start search behind selection
                                            // (before if ssoBackward) // Default is to start at caret (Only SearchReplace / SearchReplaceEx has start/end param)
        );
    

Returns the number of replacements done. When Searching, returns 1 if found, 0 if not found. If found the match will be selected (use BlockBegin/End). 
    
    
      if Caller.SearchReplace('FindMe', ' ', []) > 0 then begin
        // Selection is set to the first occurrence of FindMe (searched from position of caret)
      end;
    

Text
    
    
    
        property Lines[Index: Integer]: string; // read only
        property LinesCount: integer; // read only   (new for Lazarus 2.2)
        property LineAtCaret: string;  // read only
        procedure InsertTextAtCaret(aText: String; aCaretMode : TSynCaretAdjustMode);
        property TextBetweenPoints[aStartPoint, aEndPoint: TPoint]: String              // Logical Points
        procedure SetTextBetweenPoints(aStartPoint, aEndPoint: TPoint;
                                       const AValue: String;
                                       aFlags: TSynEditTextFlags = [];
                                       aCaretMode: TSynCaretAdjustMode;
                                       aMarksMode: TSynMarksAdjustMode;
                                       aSelectionMode: TSynSelectionMode
                                      );
    

Clipboard
    
    
    
        procedure CopyToClipboard;
        procedure CutToClipboard;
        procedure PasteFromClipboard;
        property CanPaste: Boolean     // read only
    

Logical / Physical
    
    
    
        function LogicalToPhysicalPos(const p: TPoint): TPoint;
        function LogicalToPhysicalCol(const Line: String; Index, LogicalPos
                                  : integer): integer;
        function PhysicalToLogicalPos(const p: TPoint): TPoint;
        function PhysicalToLogicalCol(const Line: string;
                                      Index, PhysicalPos: integer): integer;
        function PhysicalLineLength(Line: String; Index: integer): integer;
    

### ClipBoard: TClipBoard
    
    
        property AsText: String;
    

## Available Macros / Downloads

Editor macros  **Description** | **Link**  
---|---  
**Column Align Source code** A macro that aligns selected code to a specific token. It looks for a specific token in each selected line, and aligns each occurrence of it. Select the 3 lines. If the selection starts right before the ":" then the ":" will be detected. A prompt will ask you to confirm the ":". Then all ":" will be aligned. (If you use a word for alignment, please note, that it will be matched regardless of word boundaries) You can vertically align things like := or // or (  | 
    
    
      foo := 1;
      something := 2;
      x := -3;
    

| 
    
    
      foo       := 1;
      something := 2;
      x         := -3;
      
  
---|---  
[Editor Macro Column Align Source](<Editor_Macro_Column_Align_Source.md> "Editor Macro Column Align Source")   
  
**Count array elements** A macro to count the boundaries in an array constant.   
| 
    
    
      const foo: array [0..] of integer = (
        1, 2, 3,
        4
      );
    

| 
    
    
      const foo: array [0..3] of integer = (
        1, 2, 3,
        4
      );
      
  
---|---  
See <http://forum.lazarus.freepascal.org/index.php/topic,27186.msg167883.html#msg167883> for counting array elements.   
  
**Add PasDoc for function declaration** Comments a procedure/function in [PasDoc](<https://github.com/pasdoc/pasdoc/wiki>) format Example: Executing the macro with the caret in the line of function declaration will insert the comment before.  | 
    
    
    function AddRadioButton(const aCaption: string; aChecked: boolean = False): TCheckBox;
    

| 
    
    
    { Description
      @param(aCaption description )
      @param(aChecked description )
      @returns( description )
      @raises( none )
    }
    function AddRadioButton(const aCaption: string; aChecked: boolean = False): TRadioButton;
      
  
---|---  
  
<https://github.com/DomingoGP/LazarusMacroPasDoc>

[MacroPasDocLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroPasDocLazarus.txt>)  
**Sort selected lines** There are two macros, one with the normal alphabetical order and another that orders by type  | 
    
    
      c:integer;
      b:integer;
      d:boolean;
      a:boolean;
    

| 
    
    
      //normal sorting
      a:boolean;
      b:integer;
      c:integer;
      d:boolean;
    

| 
    
    
      //sorting by type
      d:boolean;
      a:boolean;
      c:integer;
      b:integer;
      
  
---|---|---  
[MacroSortLinesLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroSortLinesLazarus.txt>) [MacroSortByTypeLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroSortByTypeLazarus.txt>)  
**Column Align by procedure/function name the selected lines** | 
    
    
      constructor Create;
      destructor Destroy; override;
      function Load(const aFileName:TFileName):boolean;
      procedure Save(const aFileName:TFileName); 
      function Size:integer;
    

| 
    
    
      constructor Create;
      destructor  Destroy; override
      function  Load(const aFileName:TFileName):boolean;
      procedure Save(const aFileName:TFileName);   
      function  Size:integer;
      
  
---|---  
[MacroAlignFunctionProcedureNamesLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroAlignFunctionProcedureNamesLazarus.txt>)  
**Sort selected function/procedures declarations by Name, keeps the comments before the function/procedure** | 
    
    
      destructor  Destroy; override;
      constructor Create;
      procedure Save(const aFileName:TFileName);
      { Load the aFileName }
      function  Load(const aFileName:TFileName):boolean;
      function  Size:integer;
    

| 
    
    
      constructor Create;
      destructor  Destroy; override;
      { Load the aFileName }
      function  Load(const aFileName:TFileName):boolean;
      procedure Save(const aFileName:TFileName);
      function  Size:integer;
      
  
---|---  
[MacroSortFuncProcLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroSortFuncProcLazarus.txt>)  
**Sort selected units in the uses clause** | 
    
    
      uses
        Classes, SysUtils, Forms, Controls, Graphics, Dialogs;
    

| 
    
    
      uses
        Classes, Controls, Dialogs, Forms, Graphics, SysUtils;
      
  
---|---  
[MacroSortUsesLazarus.txt](<https://github.com/DomingoGP/LazarusMacroPasDoc/blob/master/MacroSortUsesLazarus.txt>)  
  
## See also

  * [IDE Window: Editor Macros](<IDE_Window__Editor_Macros.md> "IDE Window: Editor Macros")
  * [Editor Macro Column Align Source](<Editor_Macro_Column_Align_Source.md> "Editor Macro Column Align Source") \- macro to align selected code to a specific token.

---

_Source: [https://wiki.freepascal.org/Editor_Macros_PascalScript](https://web.archive.org/web/20230530034912/https://wiki.freepascal.org/Editor_Macros_PascalScript)_
