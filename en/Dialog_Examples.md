# Dialog Examples

│ **English (en)** │  **[русский (ru)](<../ru/Dialog_Examples.md>)** │

## Contents

  * 1 Some useful dialogs
    * 1.1 Message Dialogs
      * 1.1.1 ShowMessage
      * 1.1.2 MessageBox
      * 1.1.3 MessageDLG
      * 1.1.4 QuestionDlg
    * 1.2 Text input Dialogs
      * 1.2.1 InputBox
      * 1.2.2 InputQuery
      * 1.2.3 PasswordBox
    * 1.3 Constants and Types used in message dialogs



# Some useful dialogs

Here are some useful dialogs not found in the [Component Palette](<Component_Palette.md> "Component Palette"): 

  * `procedure ShowMessage (const Msg: string);`
  * `function MessageBox (Text, Caption : PChar; Flags: Word): Integer;`
  * `function MessageDlg (const Msg: string; AType: TMsgDlgType; AButtons: TMsgDlgButtons; HelpCtx: LongInt): Word;`
  * `function InputBox (const ACaption, APrompt, ADefault: string); string;`
  * `function InputQuery (const ACaption, APrompt: string; var Value: string): Boolean;`
  * `function PasswordBox(const ACaption, APrompt : String) : String;`



Each of these components causes a small popup box to be displayed, which contains some information and requires a user response: either a button press or some text entry or both. The programmer has little control over the format, size or position of these popup boxes, but can influence their textual content.  
The reason why there are often several very similar alternatives, is to allow different methods of calling the component and receiving data back from the [procedure](<Procedure.md> "Procedure") or [function](<Function.md> "Function"). These dialogs might be **platform dependent** i.e. they might be displayed differently. For example, strings which are fully displayed in Windows XP, might be truncated in Windows 7. 

## Message Dialogs

Message dialogs display a message and wait for a key-press or mouse-click response. 

### ShowMessage
    
    
    procedure ShowMessage(const Msg: string);
    

{ Defined in Dialogs.pp } 

The simplest message dialog: takes a simple [string](<String.md> "String") as parameter, displays it in a stereotyped box, and waits for a mouse-click or ↵ Enter-key event before returning to the calling [routine](<Routine.md> "Routine") or [program](<Program.md> "Program").  
This is a modal procedure call, that is the box is displayed, receives focus, and does not relinquish focus until the OK box is clicked or otherwise selected. 

Example: 
    
    
    program LazMessage;
    uses 
      Dialogs;
    begin
      ShowMessage('This is a message from Lazarus');
    end.
    

You have the ability to create multilines message using the following line separators, they will all work: 

  * `sLineBreak`
  * [`LineEnding`](<End_of_Line.md> "End of Line")
  * or the character code: #13#10



**Multiple lines example:**
    
    
    program LazMessage;
    uses 
      Dialogs;
    begin
      ShowMessage('This is a multilines' + sLineBreak + 'message!' );
    end.
    

### MessageBox
    
    
    function Application.MessageBox(Text, Caption: PChar; Flags: LongInt): Integer;
    

{ Defined in Forms.pp as part of TApplication; hence must be called as Application.Messagebox() or using the '[`with`](<With.md> "With") `Application` [`do`](<Do.md> "Do") ...' construct } 

Parameters include 

  * Text: the string that is displayed as a prompt or instruction in the Box;
  * Caption: the string label at the top of the message box;
  * Flags: [longint](<Longint.md> "Longint") \- an [integer](<Integer.md> "Integer") constructed by adding together various [constants](<Constant.md> "Constant") to define the contents and behaviour of the box, for example `MB_ABORTRETRYIGNORE + MR_ICONQUESTION` will cause the application to display a query (?) icon in a box with three buttons: ABORT RETRY IGNORE.



The function returns an integer value corresponding to the button that was pressed; its value can be determined by reference to the constants [IDOK..IDHELP] 

It can be invoked like a procedure call (ie as a 'MessageBox()' [statement](<statement.md> "statement") rather than as a 'Variable := MessageBox()' function call - see example below) 

Example: 
    
    
    uses 
      Forms, Dialogs, LCLType;
     
    procedure DisplayMessageBox;
    var 
      Reply, BoxStyle: Integer;
    begin
      BoxStyle := MB_ICONQUESTION + MB_YESNO;
      Reply := Application.MessageBox('Press either button', 'MessageBoxDemo', BoxStyle);
      if Reply = IDYES then Application.MessageBox('Yes       ', 'Reply',MB_ICONINFORMATION)
        else Application.MessageBox('No         ', 'Reply', MB_ICONHAND);
    end;
    

Notice that in this example the 'Yes' and 'No' strings have been padded out with spaces; otherwise the box would not be wide enough to display the caption properly 

[![MessageBoxDemo.png](https://wiki.freepascal.org/images/c/c2/MessageBoxDemo.png)](</File:MessageBoxDemo.png>) [![ReplyYes.png](https://wiki.freepascal.org/images/0/06/ReplyYes.png)](</File:ReplyYes.png>)

### MessageDLG
    
    
    function MessageDlg(const aMsg: string; DlgType: TMsgDlgType; 
      Buttons: TMsgDlgButtons; HelpCtx: Longint): Integer;
    function MessageDlg(const aCaption, aMsg: string; DlgType: TMsgDlgType; 
      Buttons: TMsgDlgButtons; HelpCtx: Longint): Integer;
    

Two versions of this function, ie first 'Caption' parameter is optional; if omitted, caption is missing from box 

This is the most complete and elaborate of the message dialogs, and allows the programmer considerable control over the appearance of the dialog box. The parameters defining the kind of box and its icon are types rather than integer constants, and the buttons can be specified as a [set](<Set.md> "Set") in [square brackets](<square_brackets.md> "square brackets") eg `[mbRetry, mbIgnore, mbAbort, mbCancel]`. The HelpCtx parameter is not currently implemented and should be set to zero. The return value from the function is the identity of the button pressed, expressed as an integer (see the definitions below, [mrNone..mrAll]). 

Example: 
    
    
    uses 
      Forms, Dialogs, LCLType, Controls;
     
    procedure TryMessageDlg;
    begin
      if MessageDlg('Question', 'Do you wish to Execute?', mtConfirmation, 
       [mbYes, mbNo, mbIgnore],0) = mrYes
      then { Execute rest of Program };
    end;
    

[![Question.png](https://wiki.freepascal.org/images/3/30/Question.png)](</File:Question.png>)

  


### QuestionDlg

Question dialog allows changing button captions and setting default and cancel buttons. The 3rd argument in the `QuestionDlg()` call determines the icon shown in the dialog. In some widgetsets, the icon can be suppressed by selecting message type `mtCustom`. 

Example: 
    
    
        case QuestionDlg('Caption', 'Message', mtInformation, [mrYes, 'Positive', mrNo, 'Negative', 'IsDefault'], '') of
            mrYes: QuestionDlg('Caption', 'So you mean „Yes“', mtCustom, [mrOK, 'That is right'],'');
            mrNo: QuestionDlg('Caption', 'Oh, you mean „No“', mtCustom, [mrOK, 'Exactly'], '');
            mrCancel: QuestionDlg('Caption', 'You cancelled the dialog with ESC or close button.', mtCustom, [mrOK, 'Exactly'], '');
        end;
    

[![QuestionDlg.png](https://wiki.freepascal.org/images/a/ac/QuestionDlg.png)](</File:QuestionDlg.png>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** If the user cancels the dialog and there is no 'IsCancel' button defined, mrCancel is returned! If you want to redirect ESC/close to a defined button use the 'IsCancel' parameter just after the cancel button.

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Do not confuse here the `mrXXXX` button identifiers with the `mbXXXX` identifiers used by MessageDlg, otherwise the return value will not be correct.

Advanced examples for buttons (from promptdialog.inc) 
    
    
        [mrOk, mrCancel, 'Cancel now', mrIgnore, 300, 'Do it', 'IsDefault']
    

This will result in 4 buttons: 

  * 'Ok' returning mrOk
  * 'Cancel now' returning mrCancel
  * 'Ignore' returning mrIgnore
  * 'Do it' returning 300. This will be the default button (focused)



Usually buttons in Lazarus dialogs have icons. In order to prevent icons from being shown non standardize values for message results can be used. Currently highest standardized value is 11. For example: 
    
    
      case QuestionDlg ('Caption','Message',mtCustom,[20,'Positive', 21, 'Negative',22,'I do not know','IsCancel'],'') of
         20: QuestionDlg ('Caption','So you mean „Yes“',mtCustom,[20,'That is right'],'');
         21: QuestionDlg ('Caption','Oh, you mean „No“',mtCustom,[21,'Exactly'],'');
         22: QuestionDlg ('Caption','So, please find out!',mtCustom,[22,'Maybe'],'');
      end;
    

In order to facilitate work, constants can be defined. For example: 
    
    
    const
      mrNoneNI=      20;    // "NI" to indicate: "no icon"
      mrOkNI=        mrNoneNI+1;
      mrCancelNI=    mrNoneNI+2;
      mrAbortNI=     mrNoneNI+3;
      mrRetryNI=     mrNoneNI+4;
      mrIgnoreNI=    mrNoneNI+5;
      mrYesNI=       mrNoneNI+6;
      mrNoNI=        mrNoneNI+7;
      mrAllNI=       mrNoneNI+8;
      mrYesToAllNI=  mrNoneNI+10;
      mrCloseNI=     mrNoneNI+11;
      mrLastNI=      mrCloseNI; 
    begin
      case QuestionDlg ('Caption','Message',mtCustom,[mrYesNI,'Positive', mrNoNI, 'Negative',mrCancelNI,'I do not know','IsCancel'],'') of
        mrYesNI: QuestionDlg ('Caption','So you mean „Yes“',mtCustom,[mrYesNI,'That is right'],'');
        mrNoNI: QuestionDlg ('Caption','Oh, you mean „No“',mtCustom,[mrNoNI,'Exactly'],'');
        mrCancelNI: QuestionDlg ('Caption','So, please find out!',mtCustom,[mrCancelNI,'Maybe'],'');
      end; //case
    end;
    

[![QuestionDlgNoButIcons.png](https://wiki.freepascal.org/images/1/15/QuestionDlgNoButIcons.png)](</File:QuestionDlgNoButIcons.png>)

## Text input Dialogs

### InputBox

Text input Dialogs: display a message and await user text input 
    
    
    function InputBox(const ACaption, APrompt, ADefault: String): String;
    

Displays a box with defined title and prompt, and expects user input in a text box. A default string can optionally be displayed in the text box. The user-entered or default string is returned as the function result. If user clicks Cancel, default string is returned as the function result. 

Example: 
    
    
    uses 
      Forms, LCLType, Dialogs, Controls;
     
    procedure TryInputBox;
    var 
      UserString: string;
    begin
      UserString := InputBox('Get some text input', 
        'Please type in some information', 'Some sample text');
      ShowMessage(UserString)
    end;
    

### InputQuery
    
    
    function InputQuery(const ACaption, APrompt : String;
      MaskInput : Boolean; var Value : String) : Boolean;
    function InputQuery(const ACaption, APrompt : String;
      var Value : String) : Boolean;
    

Two versions of this function which displays a prompt and expects user input of textual data; the first includes a MaskInput [boolean](<Boolean.md> "Boolean") parameter which determines whether the user input is masked out by [asterisks (*)](<_.md> "*") in the text-input box (like during entry of a password), while the second omits this property. The text entered by the user is returned in the variable parameter 'Value'; the function result is a boolean which returns [`true`](<True.md> "True") if the OK button was pressed, or [`false`](<False.md> "False") if the box was closed by any other mechanism (such as clicking the 'Close' icon on the top title bar). Omitting the MaskInput parameter is equivalent to setting it `false`. 

Example: 
    
    
    uses 
      Forms, LCLType, Dialogs, Controls;
     
    procedure TryInputQuery;
    var 
      QueryResult: Boolean;
      UserString: string;
    begin
      if InputQuery('Question', 'Type in some data', TRUE, UserString)
      then ShowMessage(UserString)
      else 
      begin
        InputQuery('Don''t be silly', 'Please try again', UserString);
        ShowMessage(UserString);
      end
    end;
    

[![MessageDlgQuestion.png](https://wiki.freepascal.org/images/d/d0/MessageDlgQuestion.png)](</File:MessageDlgQuestion.png>) [![DontBeSillly.png](https://wiki.freepascal.org/images/c/ce/DontBeSillly.png)](</File:DontBeSillly.png>)

### PasswordBox
    
    
    Function PasswordBox(const ACaption, APrompt : String) : String;
    

Behaves very similarly to the InputQuery function with MaskInput = TRUE; the difference is that the password that was typed in is returned as the result of the function (like InputBox). 

## Constants and Types used in message dialogs

Several constants and types relevant for use with the dialog boxes are pre-defined in the [LCL](<LCL.md> "LCL") library: 

const { Defined in LCLType.pp } 

integer constants for defining the types of buttons and the icon for display in MessageBox 
    
    
    MB_OK = $00000000;
    MB_OKCANCEL = $00000001;
    MB_ABORTRETRYIGNORE = $00000002;
    MB_YESNOCANCEL = $00000003;
    MB_YESNO = $00000004;
    MB_RETRYCANCEL = $00000005;
    
     
    MB_ICONHAND = $00000010;
    MB_ICONQUESTION = $00000020;
    MB_ICONEXCLAMATION = $00000030;
    MB_ICONASTERICK = $00000040;
    MB_ICONWARNING = MB_ICONEXCLAMATION;
    MB_ICONERROR = MB_ICONHAND;
    MB_ICONINFORMATION = MB_ICONASTERICK;
    

integer constants defining the return value from MessageBox according to which button was pressed 
    
    
    IDOK = 1; 	ID_OK = IDOK;
    IDCANCEL = 2;	ID_CANCEL = IDCANCEL;
    IDABORT = 3;	ID_ABORT = IDABORT;
    IDRETRY = 4;	ID_RETRY = IDRETRY;
    IDIGNORE = 5;	ID_IGNORE = IDIGNORE;
    IDYES = 6;	ID_YES = IDYES;
    IDNO = 7;	ID_NO = IDNO;
    IDCLOSE = 8;	ID_CLOSE = IDCLOSE;
    IDHELP = 9;	ID_HELP = IDHELP;
    

define whether first, second or third button is default 
    
    
    MB_DEFBUTTON1 = $00000000;
    MB_DEFBUTTON2 = $00000100;
    MB_DEFBUTTON3 = $00000200;
    MB_DEFBUTTON4 = $00000300;
    

The Flags parameter of MessageBox is constructed by adding a button constant [MB_OK..MB_RETRYCANCEL], an optional icon constant [MB_ICONHAND..MB_ICONINFORMATION] and an optional default button constant [MB_DEFBUTTON1..MB_DEFBUTTON3] 

Types for use in MessageDlg, which needs parameters AType of TMsgDlgType and AButtons of TMSgDlgButtons 

  

    
    
    { Defined in Dialogs.pp }
    type
      TMsgDlgType    = (mtWarning, mtError, mtInformation,  mtConfirmation,
                        mtCustom);
      TMsgDlgBtn     = (mbYes, mbNo, mbOK, mbCancel, mbAbort, mbRetry, mbIgnore,
                       mbAll, mbNoToAll, mbYesToAll, mbHelp, mbClose);
      TMsgDlgButtons = set of TMsgDlgBtn;
      
     
    const
      mbYesNoCancel = [mbYes, mbNo, mbCancel];
      mbOKCancel = [mbOK, mbCancel];
      mbAbortRetryIgnore = [mbAbort, mbRetry, mbIgnore];
      
     
      MsgDlgBtnToBitBtnKind: array[TMsgDlgBtn] of TBitBtnKind = (
       bkYes, bkNo, bkOK, bkCancel, bkAbort, bkRetry, bkIgnore,
        bkAll, bkNoToAll, bkYesToAll, bkHelp, bkClose
       );
     
     
      BitBtnKindToMsgDlgBtn: array[TBitBtnKind] of TMsgDlgBtn = (
        mbOk, mbOK, mbCancel, mbHelp, mbYes, mbNo,
        mbClose, mbAbort, mbRetry, mbIgnore, mbAll, mbNoToALl, mbYesToAll
        );
     
     
    { Defined in Controls.pp }
    const
      mrNone = 0;
      mrOK = mrNone + 1;
      mrCancel = mrNone + 2;
      mrAbort = mrNone + 3;
      mrRetry = mrNone + 4;
      mrIgnore = mrNone + 5;
      mrYes = mrNone + 6;
      mrNo = mrNone + 7;
      mrAll = mrNone + 8;
      mrNoToAll = mrNone + 9;
      mrYesToAll = mrNone + 10;
      mrLast = mrYesToAll;

---

_Source: [https://wiki.freepascal.org/Dialog_Examples](https://web.archive.org/web/20210922022803/https://wiki.freepascal.org/Dialog_Examples)_
