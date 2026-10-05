# Creating IDE Help

│ **[Deutsch (de)](</Creating_IDE_Help/de> "Creating IDE Help/de")** │  **English (en)** │  **[日本語 (ja)](</Creating_IDE_Help/ja> "Creating IDE Help/ja")** │  **[русский (ru)](<../ru/Creating_IDE_Help.md> "Creating IDE Help/ru")** │ 

## Contents

  * 1 Overview
  * 2 Demos and explore
    * 2.1 Help for LCL units
    * 2.2 Help for IDE windows
      * 2.2.1 Example
      * 2.2.2 Redirect
    * 2.3 Help for FPC keywords
    * 2.4 Help for Messages
    * 2.5 Adding Kylix help



# Overview

The IDE can search help for various things and there are various sources in various help formats. Here is a short list to understand the complexity: 

  * **FPC messages** : compiler message like _Error: Illegal enum minimum-size specifier_. The IDE has a default built-in to search for the _FPCSrcDir/compiler/msg/errore.msg file_ , search the message there and show the corresponding comment. The comments contain Latex tags which are stripped. At the moment a simple MessageDlg is shown. Some notorious messages need more explanation and are manually connected to URLs.
  * **IDE windows, dialogs and controls** :
  * **Pascal sources** :
  * **FPC keywords** :
  * **FPC directives** :
  * **IDE directives** :



# Demos and explore

You can see the currently installed help under **View / IDE Internals / About IDE / Help**. 

There is a test and demonstration package under **examples/idehelp/demoidehelp.lpk**. It contains example code how you can add help to the IDE. 

To show your help in the user's browser see examples/helphtml/htmlhelp1.lpi. 

To show HTML help in a LCL control see examples/htmlhelp_ipro/htmlhelpsample.lpi. 

## Help for LCL units

Whenever the user presses F1 for an Object Inspector property or on an identifier in the source editor the IDE searches the declaration and invokes the HTML viewer for the fpdoc item. The items can be edited easily via the [FPDoc Editor](<FPDoc_Editor.md> "FPDoc Editor") tool. 

You can see the state of the LCL documentation here: [LCL Documentation Roadmap](<LCL_Documentation_Roadmap.md> "LCL Documentation Roadmap"). 

## Help for IDE windows

This feature exists since 0.9.15. 

The help for the IDE windows (i.e. 'Object Inspector' or the 'Compiler Options') are documented in this wiki. The IDE has a mapping file _docs/IDEWindowHelpTree.xml_ , which contains the paths from the various IDE forms/controls to wiki pages. This mapping file is edited via an editor that is shown at any place in the IDE via `Ctrl`+`⇧ Shift`+`F1`, except for controls that catch all keys, like synedit. You can set the shortcut in the editor options -> key mapping. 

The root page of the IDE window docs is [Lazarus IDE](<Lazarus_IDE.md> "Lazarus IDE"). 

### Example

  * Open the project options: Project / Project Options
  * Open the help editor: `⇧ Shift`+`Ctrl`+`F1`



[![HelpEditor1.png](https://wiki.freepascal.org/images/d/d3/HelpEditor1.png)](</File:HelpEditor1.png>)

  * To the left, you can see all controls of the project options dialog. You can see there are also the compiler options.
  * Select the **ProjectApplicationOptionsFrame**
  * Click **Create Help node**. This will create a new item on the right.
  * Check **Has Help**
  * Check **Is a root control**. All controls on the frame should use the help on our new help page.
  * Change the path to **Project_Options**



[![HelpEditorProjectOptions.png](https://wiki.freepascal.org/images/1/11/HelpEditorProjectOptions.png)](</File:HelpEditorProjectOptions.png>)

  * You can click the **Test** button and should see [IDE_Window:_Project_Options](<IDE_Window__Project_Options.md> "IDE Window: Project Options") in your browser.



### Redirect

The TreeView in the IDE options dialog redirects the help to the help of the selected frame: 

ide/ideoptionsdlg.pas 
    
    
    procedure TIDEOptionsDialog.CategoryTreeKeyDown(Sender: TObject; var Key: Word;
      Shift: TShiftState);
    var
      Command: Word;
    begin
      Command := EditorOpts.KeyMap.TranslateKey(Key,Shift,nil);
      if (Command=ecContextHelp) and (PrevEditor <> nil) then begin
        Key:=VK_UNKNOWN;
        ShowContextHelpForIDE(PrevEditor);
      end;
    end;
    

## Help for FPC keywords

You can register a help for fpc keywords. Note: One database can handle multiple contexts, for example help for fpc keywords, help for sources and help for messages. 
    
    
    uses
      IDEHelpIntfs, HelpIntfs, LazHelpIntf, LazHelpHTML...
    
      { TSimpleFPCKeywordHelpDatabase }
    
      TSimpleFPCKeywordHelpDatabase = class(THTMLHelpDatabase) // you can use any type of database you like
      private
        FKeywordPrefixNode: THelpNode;
      public
        function GetNodesForKeyword(const HelpKeyword: string;
                            var ListOfNodes: THelpNodeQueryList; var ErrMsg: string
                            ): TShowHelpResult; override;
        function ShowHelp(Query: THelpQuery; BaseNode, NewNode: THelpNode;
                          QueryItem: THelpQueryItem;
                          var ErrMsg: string): TShowHelpResult; override;
      end;
    
    function TSimpleFPCKeywordHelpDatabase.GetNodesForKeyword(
      const HelpKeyword: string; var ListOfNodes: THelpNodeQueryList;
      var ErrMsg: string): TShowHelpResult;
    var
      KeyWord: String;
    begin
      Result:=shrHelpNotFound;
      if (csDesigning in ComponentState) then exit;
      if (FPCKeyWordHelpPrefix<>'')
      and (LeftStr(HelpKeyword,length(FPCKeyWordHelpPrefix))=FPCKeyWordHelpPrefix) then begin
        // HelpKeyword starts with KeywordPrefix
        KeyWord:=copy(HelpKeyword,length(FPCKeyWordHelpPrefix)+1,length(HelpKeyword));
        // test: testfcpkeyword
        if KeyWord='testfcpkeyword' then begin
          // this help database knows this keyword
          // => add a node, so that if there are several possibilities the IDE can
          //    show the user a dialog to choose
          // Note: In your own database you can create one dummy node or
          // one node per keyword or one node per group of keywords
          // or whatever you like.
          if FKeywordPrefixNode=nil then
            FKeywordPrefixNode:=THelpNode.CreateURL(Self,'','');
          FKeywordPrefixNode.Title:='Pascal keyword '+KeyWord;
          CreateNodeQueryListAndAdd(FKeywordPrefixNode,nil,ListOfNodes,true);
          Result:=shrSuccess;
        end;
      end;
    end;
    
    function TSimpleFPCKeywordHelpDatabase.ShowHelp(Query: THelpQuery; BaseNode,
      NewNode: THelpNode; QueryItem: THelpQueryItem; var ErrMsg: string
      ): TShowHelpResult;
    var
      KeywordQuery: THelpQueryKeyword;
      KeyWord: String;
    begin
      Result:=shrHelpNotFound;
      if not (Query is THelpQueryKeyword) then exit;
      KeywordQuery:=THelpQueryKeyword(Query);
      KeyWord:=copy(KeywordQuery.Keyword,length(FPCKeyWordHelpPrefix)+1,length(KeywordQuery.Keyword));
      debugln(['TSimpleFPCKeywordHelpDatabase.ShowHelp Keyword=',Keyword]);
      // ShowURL ...
    end;
    
    procedure Register;
    begin
     FPCKeywordsHelpDB:=HelpDatabases.CreateHelpDatabase('NameYourHelpDB',
                                                TSimpleFPCKeywordHelpDatabase,true);
    end;
    

## Help for Messages

This feature exists since 0.9.15 and requires the FPC sources installed locally. 

This is invoked when the user presses F1 or uses the Help menu item of the message window. 

Normally the IDE will search the errore.msg file in the FPC sources and show the comment for the message. But some messages like _unit not found_ need extra help. The following describes how to do that: 

To add a wiki help page for a message, do the following: 

  * Create a wiki page under [Build messages](<Build_messages.md> "Build messages")
  * Right click on messages window / Edit help
  * This shows the editor for message additions.



[![AdditionalHelpForMessages.png](https://wiki.freepascal.org/images/6/69/AdditionalHelpForMessages.png)](</File:AdditionalHelpForMessages.png>)

The items are stored in **< lazarusdir>/docs/additionalmsghelp.xml**. 

## Adding Kylix help

For wow to use the Borland Help files within the IDE editor, see [Adding Kylix Help](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE").

---

_Source: [https://wiki.freepascal.org/Creating_IDE_Help](https://web.archive.org/web/20241210000452/https://wiki.freepascal.org/Creating_IDE_Help)_
