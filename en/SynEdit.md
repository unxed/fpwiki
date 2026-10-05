# SynEdit

│ **[Deutsch (de)](</SynEdit/de> "SynEdit/de")** │  **English (en)** │  **[español (es)](</SynEdit/es> "SynEdit/es")** │  **[français (fr)](</SynEdit/fr> "SynEdit/fr")** │  **[日本語 (ja)](</SynEdit/ja> "SynEdit/ja")** │  **[polski (pl)](</SynEdit/pl> "SynEdit/pl")** │  **[русский (ru)](<../ru/SynEdit.md> "SynEdit/ru")** │  **[中文（中国大陆） (zh_CN)](</SynEdit/zh_CN> "SynEdit/zh CN")** │    
****

**SynEdit** is a [syntax highlighting](<Syntax_highlighting.md> "Syntax highlighting") edit/memo package available on the [SynEdit tab](<SynEdit_tab.md> "SynEdit tab") with support for many languages/syntaxes. 

The SynEdit contained in Lazarus was forked from [SynEdit 1.0.3](<http://synedit.sourceforge.net/>), and was adapted and extended quite a lot. Changes are listed below. 

Lazarus package contains a source editor component named [TSynEdit](<TSynEdit.md> "TSynEdit"), several syntax highlighters and other components used for source editing. 

Licensed under the same terms as the original SynEdit (MPL or GPL). 

## Contents

  * 1 Original vs Lazarus version
  * 2 SynEdit 2.0 port
  * 3 SynEdit in the IDE
  * 4 Using SynEdit
    * 4.1 Highlighters
    * 4.2 Markup
    * 4.3 Edit an existing highlighter
    * 4.4 Completion plugins
    * 4.5 Logical/Physical caret position
    * 4.6 Change text from code
    * 4.7 Fold/Unfold from code
    * 4.8 Bookmarks
    * 4.9 More info
    * 4.10 Example apps
    * 4.11 Adding hotkeys for Cut/Copy/Paste/etc
  * 5 Further development, discussions
  * 6 See also



## Original vs Lazarus version

Lazarus version is maintained mostly by [Martin Friebe](</User:Martin> "User:Martin"). Martin wrote at forum, what has been added to Lazarus version since fork appeared: 

Big things added to the Lazarus version: 

  * folding
  * configurable gutter / gutter-parts
  * shared text between several editors
  * utf-8 support
  * sync-editing plugin
  * basic RTL/LTR support
  * mouse config via MouseActions
  * rewrite of various highlight/markup modules



Codebases of Delphi/Lazarus versions were independently refactored. There is very few overlap left. 

## SynEdit 2.0 port

An alternative port of the 2.0.x version of the original SynEdit exists. It's not actively maintained, last commit (now is Jun 2014) was at 2011. 

  * [SynEdit/port](<SynEdit/port.md> "SynEdit/port")
  * [Github page](<https://github.com/rnapoles/>)



## SynEdit in the IDE

The SynEdit in Lazarus is a built-in package, because the IDE uses it itself. Therefore the package can not be removed from the installation list. To remove the entries from the component palette, the SynEditDsgn package can be removed from installation. 

## Using SynEdit

### Highlighters

  * There are several standard highlighters (see [SynEdit tab](<SynEdit_tab.md> "SynEdit tab") in the [Component Palette](<Component_Palette.md> "Component Palette"))
  * There are scriptable highlighters, which can adapt to many other file formats: 
    * [TSynAnySyn](</index.php?title=TSynAnySyn&action=edit&redlink=1> "TSynAnySyn \(page does not exist\)") (standard, on component palette by default)
    * [TSynPositionHighlighter](</index.php?title=TSynPositionHighlighter&action=edit&redlink=1> "TSynPositionHighlighter \(page does not exist\)") (standard, **not** on component palette)
    * [TSynUniHighlighter](</index.php?title=TSynUniHighlighter&action=edit&redlink=1> "TSynUniHighlighter \(page does not exist\)") (standard, **not** on component palette)
    * SynFacilSyn ([Github](<https://github.com/t-edson/SynFacilSyn>))
  * More 3rd-party highlighters exist: SynCacheSyn, SynGeneralSyn, SynRCSyn, SynRubySyn, SynSDDSyn, SynSMLSyn, SynSTSyn, SynTclTkSyn, SynUnrealSyn, SynURISyn, SynVBScriptSyn, SynVrml97Syn, [see here](<http://bugs.freepascal.org/view.php?id=18248>).
  * You can write new highlighter, see info at [SynEdit Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter").



### Markup

Markup provides additional coloring [SynEdit Markup](<SynEdit_Markup.md> "SynEdit Markup")

### Edit an existing highlighter

Sometimes you might want to edit existing highlighters (just like I wanted a few days ago) that already exist. In this example we're going to edit the highlighter for pascal-like code (classname: [TSynPasSyn](<TSynPasSyn.md> "TSynPasSyn"); package: SynEdit V1.0; unit: SynHighlighterPas.pas). 

Say, what we want to reach is, that our application (lazarus in this case) differs between the three types of comments, which exist in Pascal: 
    
    
      (* ansi *)
      { bor }
      // Slash
    

This may be helpful, if you want to differ between different types of your comments (e.g. "Description", "Note", "Reference", etc.) and want them to be e.g. colored in different ways. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Just in case you break something, I suggest to make some "NEW" and "/NEW"-comments, but you don't have to

  * First, open the unit "SynHighlighterPas" which should be located in your SynEdit-directory.
  * As we don't want to cause incompatibilities, we're creating a new type of enumerator which helps us to identify our comment later:



E.g. under the declaration of "tkTokenKind", write this: 
    
    
      {NEW}
      TtckCommentKind = (tckAnsi, tckBor, tckSlash);
      {/NEW}
    

  * In the declaration of "TSynPasSyn" search for "FTokenID" and add the following between "FTokenID" and the next field


    
    
      {NEW}
      FCommentID: TtckCommentKind;
      {/NEW}
      //This creates a new field, where we can store the information, what kind of comment we have
    

  * In the declaration of "TSynPasSyn" search for "fCommentAttri" and add the following between "fCommentAttri" and the next field


    
    
      {NEW}
      fCommentAttri_Ansi: TSynHighlighterAttributes;
      fCommentAttri_Bor: TSynHighlighterAttributes;
      fCommentAttri_Slash: TSynHighlighterAttributes;
      {/NEW}
      //This allows us, to return different Attributes, per type of comment
    

  * Next, search for the constructor-definition of "TSynPasSyn", which should be "constructor TSynPasSyn.Create(AOwner: TComponent);"
  * We need to Create our new Attributes, thus we add our Attributes somewhere in the constructor (I suggest, after the default "fCommentAttri")


    
    
      (...)
      AddAttribute(fCommentAttri);
      {NEW}
      fCommentAttri_Ansi := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Ansi', SYNS_XML_AttrComment+'_Ansi'); //The last two strings are the Caption and the stored name
      //If you want to have default settings for your attribute, you can e.g. add this:
      //fCommentAttri_Ansi.Background := clBlack; //Would set "Background" to "clBlack" as default
      AddAttribute(fCommentAttri_Ansi);
      fCommentAttri_Bor := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Bor', SYNS_XML_AttrComment+'_Bor');
      AddAttribute(fCommentAttri_Bor);
      fCommentAttri_Slash := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Slash', SYNS_XML_AttrComment+'_Slash');
      AddAttribute(fCommentAttri_Slash);
      {/NEW}
      (...)
    

  * The "complex" part now is, to search for the points, where "FTokenID" is set to "tkComment" and to set our "subtype", equally (of course, I've already searched them:)


    
    
    procedure TSynPasSyn.BorProc;
    (...)
      fTokenID := tkComment;
      {NEW}
      FCommentID:=tckBor;
      {/NEW}
      if rsIDEDirective in fRange then
    (...)
    
    
    
    procedure TSynPasSyn.AnsiProc;
    begin
      fTokenID := tkComment;
      {NEW}
      FCommentID:=tckAnsi;
      {/NEW}
    (...)
    
    
    
    procedure TSynPasSyn.RoundOpenProc;
    (...)
            fTokenID := tkComment;
            {NEW}
            FCommentID:=tckAnsi;
            {/NEW}
            fStringLen := 2; // length of "(*"
    (...)
    
    
    
    procedure TSynPasSyn.SlashProc;
    begin
      if fLine[Run+1] = '/' then begin
        fTokenID := tkComment;
        {NEW}
        FCommentID:=tckSlash;
        {/NEW}
        if FAtLineStart then begin
    (...)
    
    
    
    procedure TSynPasSyn.SlashContinueProc;
    (...)
        fTokenID := tkComment;
        {NEW}
        FCommentID:=tckSlash;
        {/NEW}
        while not(fLine[Run] in [#0, #10, #13]) do
    (...)
    

  * Now, we just have to retreve the information when "GetTokenAttribute" is called and return the right Attribute, therefore we edit "GetTokenAttribute" as follows:


    
    
    function TSynPasSyn.GetTokenAttribute: TSynHighlighterAttributes;
    begin
      case GetTokenID of
        tkAsm: Result := fAsmAttri;
        {OLD
        tkComment: Result := fCommentAttri; //This is commented and just backup, so it'll be ignored
        /OLD}
        {NEW}
        tkComment: begin
          if (FCommentID = tckAnsi) then
            Result := fCommentAttri_Ansi //Type is AnsiComment
          else
            if (FCommentID = tckBor) then
              Result := fCommentAttri_Bor //Type is BorComment
            else
              if (FCommentID = tckSlash) then 
                Result := fCommentAttri_Slash //Type is SlashComment
              else
                Result := fCommentAttri //If our code failed somehow, fallback to default
        end;
        {/NEW}
        tkIDEDirective: begin
    (...)
    

If you do use lazarus, just reinstall the SynEdit-Package, if not, recompile your project/the package/<similar>. 

**DONE ! No seriously, you are now ready to differ between the different types of comments.**

The lazarus-IDE does automatically detect, what attributes exist and shows them in the options, such as saves them, if you change them. If your application/IDE doesn't do this, you will have to set Color/Font/etc. of the new Attributes somewhere manually (e.g. in the constructor of TSynPasSyn) 

### Completion plugins

There are 3 completion plug-ins for SynEdit: 

[TSynCompletion](</index.php?title=TSynCompletion&action=edit&redlink=1> "TSynCompletion \(page does not exist\)")

  * Offers a list of words in a drop-down via a shortcut key combination (default: Ctrl-Space).
  * Used in the IDE for identifier completion.
  * Included in examples.
  * Available on the component palette (since 0.9.3x).



Example code to invoke the completion pop up programmatically (i.e. without pressing the keyboard shortcut): 
    
    
    YourSynEdit.CommandProcessor(YourSynCompletion.ExecCommandID, '', nil)
    

[TSynAutoComplete](</index.php?title=TSynAutoComplete&action=edit&redlink=1> "TSynAutoComplete \(page does not exist\)")

  * Replaces the current token with a piece of text. **Not** interactive. **No** drop-down.
  * Included in examples.
  * Available on the component palette.



[TSynEditAutoComplete](</index.php?title=TSynEditAutoComplete&action=edit&redlink=1> "TSynEditAutoComplete \(page does not exist\)")

  * Basic template module. **No** drop-down.
  * Used by IDE for code-templates. IDE contains additional code extending the feature (drop-down and syncro macros are added by IDE).
  * **Not** included in examples.



Todo: Differences between 2nd and 3rd need to be documented. Maybe they can be merged. 

### Logical/Physical caret position

SynEdit offers position of the caret (text blinking cursor) in 2 different forms: 

  * Physical X/Y: Corresponds to visual (canvas) position,
  * Logical X/Y: Corresponds to byte offset of the text.



Both are 1-based. Currently Y coordinates are always the same. This may change in future. 

The Physical coordinate
    is the position in the display grid (ignoring any scrolling). That is:
    the letter "a" and "â" take both ONE cell on the grid, increasing physical x by 1. Even though in utf8 encoding "a" takes one byte, and "â" takes several bytes.
    however the tab char (#9), besides being just one byte and one char, can take several cells in the grid, increasing the physical x by more than one. There are also some chars in Chinese and eastern languages, that take 2 grid positions (google full-width vs half-width char)

The Logical coordinate
    is the byte offset in the string holding the line.
    the letter "a" has 1 byte and increases by 1
    the letter "â" has 2 (or 3) bytes, and increases by that
    tab has 1 byte and increases by that.

Neither of the 2 give the position in UTF8 chars/code-points (e.g. for Utf8Copy or Utf8Length). 

The physical X is always counted from the left of the text, even if this is scrolled out. To get the grid-x of the currently scrolled control do: 

    grid-X-in-visible-part-of-synedit := PhysicalX - SynEdit.LeftChar + 1
    grid-y-in-visible-part-of-synedit := SynEdit.RowToScreenRow(PhysicalY); // includes folding
    use ScreenRowToRow for reverse

### Change text from code

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Changing text via _SynEdit.Lines_ property does _not work with undo/redo._

Text can be accessed via SynEdit.Lines. This is a TStrings based property offering read/write access to each line. It is 0 based. 
    
    
      SynEdit.Lines[0] := 'Text'; // first line
    

SynEdit.Lines can be used to set the initial version of the text (e.g. loaded from file). Note that SynEdit.Lines.Add/SynEdit.Lines.Append does not support line breaks inside the added strings. You should add lines one by one. 

To modify the content of a SynEdit, and allow the user to undo the action use the following methods: 
    
    
        procedure InsertTextAtCaret(aText: String; aCaretMode: TSynCaretAdjustMode = scamEnd);
        property TextBetweenPoints[aStartPoint, aEndPoint: TPoint]: String // Logical Points
          read GetTextBetweenPoints write SetTextBetweenPointsSimple;
        property TextBetweenPointsEx[aStartPoint, aEndPoint: TPoint; CaretMode: TSynCaretAdjustMode]: String
          write SetTextBetweenPointsEx;
        procedure SetTextBetweenPoints(aStartPoint, aEndPoint: TPoint;
                                       const AValue: String;
                                       aFlags: TSynEditTextFlags = [];
                                       aCaretMode: TSynCaretAdjustMode = scamIgnore;
                                       aMarksMode: TSynMarksAdjustMode = smaMoveUp;
                                       aSelectionMode: TSynSelectionMode = smNormal );
    

Examples: 
    
    
      // Insert text at caret
      SynEdit.InsertTextAtCaret('Text');
      // Replace text from (x=2,y=10) to (x=4,y=20) with Str
      SynEdit.TextBetweenPoints[Point(2,10), Point(4,20)] := Str;
    
    
    
      // Delete/replace single char at caret pos
      var
        p1, p2: TPoint;
      begin
        p1 := SynEdit.LogicalCaretXY;
        p2 := p1;
        // Calculate the byte pos of the next char 
        p2.x := p2.x + UTF8CharacterLength(@SynEdit.LineText[p2.x]);
        // p1 points to the first byte of char to be replaced
        // p2 points to the first byte of the char after the last replaceable char
        // Replace with "Text" (or use empty string to delete)
        SynEdit.TextBetweenPoints[p1, p2] := 'Text';
    

### Fold/Unfold from code

  * This is still under construction.
  * This only works if current highlighter supports folding (details at [SynEdit_Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter")).
  * Also note that some highlighters support several independent fold-trees. E.g. in Pascal you have folding on keywords (begin, end, class, procedure, etc) which is the primary fold, and folding on $ifdef or $region which is secondary.
  * Folding of current selection is also different from folding on keywords.



Methods for folding: 

1) TSynEdit.CodeFoldAction 

Folds at the given Line. If there are more than one, folds the inner most (right most). Note: This does not work with selection, nor with Folds that hide entirely / Need testing for 2ndary folds. 

2) TSynEdit.FindNextUnfoldedLine 

3) TSynEdit.FoldAll / TSynEdit.UnfoldAll 

### Bookmarks

  * [Forum topic about adding gutter-marks and colored lines](<http://forum.lazarus.freepascal.org/index.php/topic,14948.msg79794.html>)



### More info

Discussions on the forum, which contain info about SynEdit: 

  * [Search/replace; caret; position logical/physical](<http://forum.lazarus.freepascal.org/index.php/topic,19520.msg111158.html#msg111158>)
  * [Diff between SynEdit/SynMemo](<http://forum.lazarus.freepascal.org/index.php/topic,19645.msg111962.html#msg111962>)
  * [Mouse actions; disable paste on middle click](<http://forum.lazarus.freepascal.org/index.php/topic,23997.msg144100.html#msg144100>)
  * [TSynEditMarkup info](<http://forum.lazarus.freepascal.org/index.php?topic=24842>)



### Example apps

Example applications can be found in the folder "lazarus/examples/synedit". 

### Adding hotkeys for Cut/Copy/Paste/etc

Hotkeys can be implemented by using SynEdit commands. 
    
    
    uses
      SynEdit, SynEditKeyCmds;
    
    procedure TForm1.SynEdit1KeyDown(Sender: TObject; var Key: Word;
      Shift: TShiftState);
    begin
      if (Shift = [ssCtrl]) then
        case Key of
          VK_C: SynEdit1.CommandProcessor(TSynEditorCommand(ecCopy), ' ', nil);
          VK_V: SynEdit1.CommandProcessor(TSynEditorCommand(ecPaste), ' ', nil);
          VK_X: SynEdit1.CommandProcessor(TSynEditorCommand(ecCut), ' ', nil);
        end;
    end;
    

## Further development, discussions

  * RTL (right-to-left): started by Mazen (partly implemented on Windows)
  * SynEdit only uses UTF8; an ASCII/ANSI version no longer exists. A font is pre-selected depending on the system. The user can choose another font, but must then take care to choose a monospaced font. 
    * automatic monospace font selection: At the moment SynEdit starts with a font 'courier'. At the moment the LCL TFont does not provide a property to filter monospaced fonts.
    * automatic UTF-8 font selection: Same as above monospace, but also with an UTF-8 font, so that for example umlauts are shown correctly.
  * Dead keys. Most keyboards support typing two or more keys to create one special character (like accented or umlaut characters). (This is handled by LCL widgedset)
  * [Redesign of the SynEdit component](<Redesign_of_the_SynEdit_component.md> "Redesign of the SynEdit component"). The primary goal is more reliable display and navigation in the text. A more modular approach also allows for better integration of extensions, and for specialized controls, for use outside of Lazarus.
  * [Word Wrapping](<http://bugs.freepascal.org/view.php?id=30395>). This is an experimental implementation following the idea of the TextTrimmer/TabExpansion classes. The linked bugtraker issue has the class and the explanation of changes required in other files for it to work.
  * Hooks in SynEdit key/command processing. On the forum: <http://forum.lazarus-ide.org/index.php/topic,35592.msg243316.html#msg243316>



## See also

  * [SynEdit Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter")
  * ["Edit an existing highlighter": German forum thread at "lazarusforum.de"](<http://www.lazarusforum.de/viewtopic.php?f=5&t=8723>)
  * [ATSynEdit](<ATSynEdit.md> "ATSynEdit")

---

_Source: [https://wiki.freepascal.org/SynEdit](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/SynEdit)_
