# KControls/KMemo notes

## Contents

  * 1 Introduction
  * 2 Underlying structure
  * 3 Inserting Text
  * 4 Selections
  * 5 Miscellaneous
  * 6 Playing With Blocks
  * 7 Hyperlinks
  * 8 Search
  * 9 Bullets
  * 10 Font names, styles, and sizes
  * 11 Copy on Selection and Middle Button Paste
  * 12 Lines
  * 13 Locking
  * 14 Bugs
    * 14.1 Copy and Paste Bug
    * 14.2 The Copy (or WriteRTF) Problem
    * 14.3 Paste (or LoadRTF) Issue
  * 15 Executing commands
  * 16 Installation
  * 17 See Also



# Introduction

This page is about the KMemo Component, a part of [KControls](<../KControls.md> "KControls"). KMemo provides a Cross Platform (Linux, Windows, macOS) memo capable of a range of text font styles, colours and similar. Its also capable of showing images and a range of other things but that may need to be dealt with at a later stage. And, importantly, it really does work pretty much the same way across Linux, Windows and Mac. Possibly other platforms as well but I cannot speak authoritatively there. 

The content presented here is in addition to the manual distributed with the KControls package. Its written by a KMemo user, not the author and could contain errors, omissions and possibly outright lies ! Someone else will undoubtedly use a different set of methods so that person is urged to document what they find too. And correct any errors they find here. 

# Underlying structure

Its important, when using KMemo, to understand how the memo content and its meta data is stored. Everything is in a Block, and KMemo itself has a Blocks property. A Block has a Text property that contains the text being displayed, a Font property and a number of others controlling how the text is displayed. Every change to font style, colour, size and so on requires a new block. Block classes discussed here include TKMemoTextBlock, TKMemoParagraph, TKMemoHyperlink. A TKMemoParagraph appears between two paragraphs. 

This **line of _text is an_** _example._

It will contain 5 blocks. 

  * Block 0 "This " - normal font.
  * Block 1 "line of " - bold
  * Block 2 "text is an " - bold and italic
  * Block 3 "example." - italic
  * Block 4 - a paragraph marker.



The KMemo, Block and Blocks classes have a large number of other Properties and Methods, far too many to possibly document here. And there are also many associated classes. Lazarus will prompt you with them and the names are reasonably intuitive. 

# Inserting Text

Easy. KMemo1.Blocks.AddTextBlock(AString). That will append the block at the end of any existing blocks. You can add a number after the string parameter and it will be inserted at that BlockIndex. Its a function, returning the TKMemoBlock so, once created you can alter how that text is presented. 
    
    
    procedure Form1.AddText();
    var
        TB: TKMemoTextBlock;
    begin
      TB := KMemo1.Blocks.AddTextBlock(InStr);
      TB.TextStyle.Font.Size := 16;
      TB.TextStyle.Font.Style := TB.TextStyle.Font.Style + [fsBold];
    end;
    

Now, presumably, AddTextBlock() has allocated some memory for that TextBlock, we assume it will Free it when appropriate. 

If you wish to insert some text at a particular spot that will take on the font characteristics of the insertion point, its even easier. Several functions will return a SelIndex (its just a count of characters starting at zero) - 
    
    
    KMemo1.Blocks.InsertPlainText(SelIndex, 'some bit of text to insert');
    

# Selections

... On our selection ... (sorry, Aussie joke) 

Kmemo offers a couple of functions, KMemo1.Blocks.RealSelStart, KMemo1.Blocks.RealSelEnd that return (read only) SelectionIndexes. ~End is always greater (or equal to) than ~Start unlike KMemo1.SelStart, Kmemo1.SelEnd which are sensitive to the direction the user wiped the mouse. KMemo1.Blocks.RealSelStart tracks the cursor position while a user is typing. 

The function KMemo1..Blocks.SelText returns the selected text (if any) irrespective of intervening block structure. Note that it might include newline characters. 

# Miscellaneous

NOTE: By default upon creation of a table, a text block containing a new line is created within it. Usually it has to be removed by: 
    
    
    procedure Form1.AddText();
       KMemo1.Blocks.Clear;end;
    

**WARNING:** TKmemo does **not** support RightToLeft languages. 

# Playing With Blocks

You will get used to working with the two indexes, one (utf8) character based and the other Block based. Both start at zero. 

The Blocks class has a property, Items that can let you address an individual block, so KMemo1.Blocks.Items[BlockNo] is a particular block. And it has a whole bunch of properties and methods. 

There are KMemo1.Blocks.Count blocks in a KMemo. And there is length(KMemo1.Blocks.Text) characters but only in a Unix based system such as Linux or macOS. All systems allow one character for a Paragraph marker in the KMemo itself but Windows, being Windows puts a CR/LF, (#13#10) at the end of each paragraph in KMemo1.Blocks.Text. So you need allow for that, for an example, see below about Searching. 

**How to convert between the two ? Like this**
    
    
    var 
        BlockNo, CharNo, LocalIndex : longint;
    begin
        CharNo := SomeValue;
        BlockNo := Kmemo1.Blocks.IndexToBlockIndex(CharNo, LocalIndex);
    

BlockNo now has what block number CharNo points to and LocalIndex tells us how far in the block we are. Useful information, if LocalIndex is 0, we are at the start of a block, if its length(KMemo1.Blocks.Items[BlockNo].Text)-1 we must be at the end of a block. Dont forget that the Text component of a Block is, effectively, a Pascal string and it starts at 1, not zero. So, the first character on the first line of a KMemo is 

  * KMemo.Blocks.Items[0].Text[1]
  * TKMemoSelectionIndex = 0
  * and Kmemo1.Blocks.IndexToBlockIndex(0, LocalIndex); // will return (block) 0 and set LocalIndex to 0



**The reverse** \- 
    
    
    CharNo := KMemo1.Blocks.BlockToIndex(KMemo1.Blocks.Items[BlockNo]);
    

will convert a blocknumber to SelectionIndex type of number. A SelectionIndex is a 0 based count of UTF8 characters, not bytes. 

When iterating over blocks, important to know what each block is - 
    
    
    if KMemo1.Blocks.Items[BlockNo].ClassNameIs('TKMemoHyperlink') then
    	URL := TKMemoHyperlink(KMemo1.Blocks.Items[BlockNo].URL);
    

Here we have found out that the Block at BlockNo is a Hyperlink block, we can therefore cast it to TKMemoHyperlink and get the URL stored there previously. 
    
    
    Kmemo1.Blocks.Delete(BlockNo);
    

To delete a block. The source says parameter is an integer, not a TKMemoBlockIndex so you may miss it when looking for something to delete a block. But that appears to be the one to use ! 
    
    
    KMemo1.Blocks.DeleteChar(Index);
    

Can also be a bit confusing. You would expect it to delete the character pointed to by Index, and yes, it might. But if there is another area of Text selected when you call it, instead, it will delete that other area of text. And that can be quite a surprise ! So, something like: 
    
    
    KMemo1.Select(Index, 0);
    while Cnt < Len do begin			
      	KMemo1.Blocks.DeleteChar(Index);    
      	inc(Cnt);
    end;
    

And, you may need to restore the selection points afterwards. 

As a paragraph may consist of many blocks and one para block, its useful to know where that para block is. KMemo1.NearestParagraphIndex returns the index of the FOLLOWING para block (obviously, not necessarily the 'nearest'). 

# Hyperlinks

TKMemo supports text-only hyperlinks. As of October 2017 Hyperlinks containing images are not supported. 

OK, lets suppose you have a KMemo full of text and you want to make a word of it a hyperlink. It will be blue, have an underline and when clicked, do something. The hyperlink will be one block (remember, everything is a block), so if the text we want to convert is already a discrete block, easy, delete it and then insert a new hyperlink block with the same text. However, its more likely we need to split a block. The process is to delete the characters that we want to become the link, split the block at that spot, create a TKMemoHyperlink block, configure it and insert it between the split blocks. 

This procedure will make a hyperlink at the character position, Index and it will be Len long. It exits doing nothing if that spot is already a hyperlink. 
    
    
    procedure TForm1.MakeDemoLink(Index, Len : longint);
    var
        Hyperlink: TKMemoHyperlink;
        BlockNo, Offset : longint;
        DontSplit : Boolean = false;
        Link : ANSIString;
    begin
        // Is it already a Hyperlink ?
        BlockNo := KMemo1.Blocks.IndexToBlockIndex(Index, Offset);
        if KMemo1.Blocks.Items[BlockNo].ClassNameIs('TKHyperlink') then exit();
        Link := copy(KMemo1.Blocks.Items[BlockNo].Text, Offset+1, Len);
        if length(Kmemo1.Blocks.Items[BlockNo].Text) = Len then DontSplit := True;
        TheKMemo.SelStart := Index;                                     
        TheKMemo.SelLength :=  Len;
        TheKMemo.ClearSelection();
        if not DontSplit then BlockNo := KMemo1.SplitAt(Index);
        Hyperlink := TKMemoHyperlink.Create;
        Hyperlink.Text := Link;
        Hyperlink.OnClick := @OnUserClickLink;
        KMemo1.Blocks.AddHyperlink(Hyperlink, BlockNo);
    end;
    

Now, danger Will Robertson ! This function will fail if you pass it parameters of text that are not within one block. But only because of the "Link := copy(..." line. Easy to fix with a few extra lines of code. 

And you may need to first record the existing selection and restore it afterwards too. 

Oh, and whats this "@OnUserClickLink" ? Its the address of a procedure "OnUserClickLink()" that might be defined like this - 
    
    
    procedure TEditBoxForm.OnUserClickLink(sender : TObject);
    begin
    	showmessage('Wow, someone clicked ' + TKMemoHyperlink(Sender).Text);
    end;
    

# Search

Despite all those methods, I could not find a Search function. But if your application is just text based (ie no images or nested containers) its not that difficult to search KMemo1.Blocks.Text, treat it as an ANSIString. On Unix like systems, the position in KMemo1.Blocks.Text can be used directly as a TKMemoSelectionIndex. Sadly, in Windows, KMemo1.Blocks.Text has Window's two character line endings, but in KMemo itself, only one char is reserved for line endings. So, count the #13 characters from the start to the position of interest and subtract that from what becomes our TKMemoSelection index. 

This UTF8 compliant function will return 0 or the TKMemoSelectionIndex of the searched for term. It depends on the LazUTF8 unit so add it to your uses clause. It can be called repeatably, passing the previously found index plus 1 each time to find multiple incidences of a term. Set MoveCursor to True and it will show the user where the searched for term is. You will note most of the code is about fixing up the Windows problem. 
    
    
    function TForm1.Search(Term : ANSIString; StartAt : longint = 1; MoveCursor : Boolean = False) : longint;
    var
        Ptr, EndP : pchar;
        Offset   : longint;
        NumbCR : longint;
    begin
      	Result := UTF8Pos(Term, KMemo1.Blocks.text, StartAt);
    	{$IFDEF WINDOWS}	// Sadley we need to subtract the extra CR windows adds to a newline
      	if Result = 0 then exit();
    	NumbCR := 0;
    	Ptr := PChar(KMemo1.Blocks.text);
    	EndP := Ptr + Result-1;
    	while Ptr < EndP do begin
              if Ptr^ = #13 then inc(NumbCR);
              inc(Ptr);
    	end;
    	Result := Result - NumbCR;
      	{$ENDIF}			// does no harm in Unix but adds 66mS with my test note.
        if MoveCursor then begin
        	KMemo1.SelStart := Result;
        	KMemo1.SelEnd := Result;
            KMemo1.SetFocus;
        end;
    end;
    

# Bullets

Bullets are a popular way to display a series of (maybe) short items. KMemo does have an "out of the box" bullet capability but not every part of it works exactly as people generally expect. The underlying issue is that KMemo records the "bulletness" of a bit of text in the paragraph marker at the end of the text. This results in some unexpected (to the end user) behaviour, particularly when using backspace key at beginning or end of a line of bullet text - 

  * If the user backspaces from immediately after a bullet, perhaps hoping to delete the last character there, instead, they will delete the paragraph marker (not surprising) and remove the bullet attribute from that line of text (thats is surprising to end user).
  * If the user places the cursor at start of a bullet text and presses backspace, expecting to cancel the "bulletness", instead, it acts as a backspace should and removes the leading paragraph marked and merges the line with the line above. And if the line above was not already a bullet, it now becomes one !



There are a number of subcases of the above. Each has to be dealt with. The solution at the moment is to intercept the backspace key and, if the cursor is in one of the critical positions with respect to a bullet, prevent it being passed on and doing what is expected there and then. This works but but is complicated. 

Firstly, in KMemo1KeyDown() we look at every keystroke, we have to anyway, will need to intercept any one of the ctrl or similar key strokes for other reasons. But we are very interested in a Backspace - 
    
    
    procedure TEditBoxForm.KMemo1KeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
    begin
        // do all sorts of stuff and exit early if not a Backspace
        if BackSpaceBullet() then 
            Key := 0;                    // eat that key because it was used in BackSpaceBullet()
                                         // if BackspaceBullet() ret false, let the Backspace go through to the KMemo
    end;
    

OK, now BackspaceBullet() will check to see if the cursor is in a position near a bullet that needs our attention. 
    
    
    function TEditBoxForm.BackSpaceBullet() : boolean;
    var
        BlockNo, PosInBlock : longint;
        ParaBlockNo : integer = -1;
        IsBulletPara : boolean = false;                 // The para the cursor is in.
    begin
        Result := false;
        ParaBlockNo := Kmemo1.NearestParagraphIndex;                                      // Thats para blockno. that controls where cursor is.
        if ParaBlockNo < 2 then exit;                                                     // Don't mess with title.
        BlockNo := kmemo1.Blocks.IndexToBlockIndex(KMemo1.RealSelStart, PosInBlock);      // Block with cursor
        if PosInBlock > 0 then exit;                                                      // not the sort of BS we are looking for.
        // ======== BS when on first char of bullet line.========
        if TKMemoParagraph(KMemo1.Blocks[ParaBlockNo]).Numbering <> pnuNone then begin    // ie, this is a bullet para.
            IsBulletPara := True;
            if kmemo1.Blocks.Items[BlockNo-1].ClassNameIs('TkMemoParagraph') then begin   // Cursor is on first char of a bullet para.
                SetBullet(TKMemoParagraph(KMemo1.Blocks[ParaBlockNo]), False);
                exit(True);                                                               // My work here is done.
            end;
        end;
        // If previous para is already a bullet and current one is not, we set current one to  same
        // bullet level as previous and allow BS to do the rest. BlockNo would have been set above
        // and we know we are on the first char of the paragraph.
        if (BlockNo > 0) and (not IsBulletPara) then begin                                // Cursor is on first char of a non-bullet para.
             if kmemo1.Blocks.Items[BlockNo-1].ClassNameIs('TkMemoParagraph')             // previous block is a para ......
                 and (TKMemoParagraph(KMemo1.Blocks[BlockNo-1]).Numbering <> pnuNone) then begin  // and previous block is bullet !
                 SetBullet(TKMemoParagraph(KMemo1.Blocks[ParaBlockNo]), True, TKMemoParagraph(KMemo1.Blocks[BlockNo-1]).Numbering);
                 exit(False);                                                             // lets BS go through to KMemo.
             end;
        end;
    end;
    

If it decides it needs to act, it will what is necessary and return True, indicating to the calling process that it should not allow the Backspace keystroke to be passed on to the KMemo. If the cursor is not in a position that should affect existing bullets, it returns false, the calling procedure will allow the Backspace to go to the KMemo which will use it as expected. The SetBullet() method will increase of decrease the Bullet Level. Further details can be seen in <https://github.com/tomboy-notes/tomboy-ng/blob/master/source/editbox.pas>

# Font names, styles, and sizes

KMemo supports many of the same font style options as [TFont](<https://support.smartbear.com/testcomplete/docs/reference/user-forms/helper-objects/tfont/style.html>), its Delphi cousin. To set styles: 
    
    
    Block.TextStyle.Font.Name := 'monospace'; // make font fixed width (untested on macOS and Windows)
    Block.TextStyle.Font.Name := 'Arial'; // set font to Arial
    Block.TextStyle.Font.Style := Block.TextStyle.Font.Style + [fsBold]; // embolden it!
    Block.TextStyle.Font.Style := Block.TextStyle.Font.Style + [fsItalic]; // italicize
    Block.TextStyle.Font.Style := Block.TextStyle.Font.Style + [fsStrikeout]; // strikeout
    Block.TextStyle.Font.Style := Block.TextStyle.Font.Style + [fsUnderline]; // underline
    

You can use similar syntax for font size, color, subscript, and superscript. 
    
    
    Block.TextStyle.Font.Size := 16; // set font size to 16
    Block.TextStyle.Font.Color := clRed // use the TColor value of any color
    Block.TextStyle.ScriptPosition := tpoSubscript; // subscript
    Block.TextStyle.ScriptPosition := tpoSuperscript; // superscript
    

(The snippets above don't exhaustively cover all font style options provided with KMemo) 

# Copy on Selection and Middle Button Paste

Unix (ie Linux but not Mac) users have long been used to an alternative copy and paste model. Any text selected is automatically made available to current and other applications with a press of the middle mouse button (or, typically, three finger tap on the touch pad). Most LCL components already do this on Linux but KMemo does not. Its easily implemented by using a capability built into the clipbrd unit. 

When an app detects you have selected some text, it registers the address of a function that knows how to copy that text to where ever it happens to be required. Unlike the 'other' clipboard, the text is not copied until needed. You will use the PrimarySelection TClipboard object already declared in clipbrd unit and a couple of small procedures - 
    
    
    uses clipbrd;
    ....
    
    procedure TForm1.KMemo1MouseUp(Sender: TObject; Button: TMouseButton;
        Shift: TShiftState; X, Y: Integer);
    var
        Point : TPoint;
        LinePos : TKmemoLinePosition;
    begin
        if Button = mbMiddle then begin
          Point := TPoint.Create(X, Y);
          PrimaryPaste(KMemo1.PointToIndex(Point, true, true, LinePos));
          exit();
        end;
        if KMemo1.SelAvail and
            (Kmemo1.Blocks.SelLength <> 0) then
                SetPrimarySelection()
            else
                UnsetPrimarySelection();
    end;
     
    procedure TForm1.SetPrimarySelection;
    var
      FormatList: Array [0..1] of TClipboardFormat;
    begin
      if (PrimarySelection.OnRequest=@PrimaryCopy) then exit;
      FormatList[0] := CF_TEXT;
      try
        PrimarySelection.SetSupportedFormats(1, @FormatList[0]);
        PrimarySelection.OnRequest:=@PrimaryCopy;
      except
      end;
    end;
    
    procedure TForm1.UnsetPrimarySelection;
    begin
      if PrimarySelection.OnRequest=@PrimaryCopy then
        PrimarySelection.OnRequest:=nil;
    end;   
        
    procedure TForm1.PrimaryCopy(
      const RequestedFormatID: TClipboardFormat;  Data: TStream);
    var
      s : string;
    begin
        S := KMemo1.Blocks.SelText;
        if RequestedFormatID = CF_TEXT then
            if length(S) > 0 then
                Data.Write(s[1],length(s));
    end;
    
    procedure TForm1.PrimaryPaste(SelIndex : integer);
    var
      Buff : string;
    begin
        if PrimarySelection.HasFormat(CF_TEXT) then begin  // I don't know if this is useful at all ?
            Buff := PrimarySelection().AsText;
            if Buff <> '' then
                KMemo1.Blocks.InsertPlainText(SelIndex, Buff);
        end;        
        // PrimarySelection is a function that returns a TClipboard object.
        // so, here, we call TClipboard.astext, but not the 'normal' clipboard.
    end;
                   
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
        UnsetPrimarySelection;
    end;
    

Note we copy the address of PrimaryCopy() into the PrimarySelection system. As other applications may well be calling this function, you must ensure the function parameter list remains as presented. Primary paste on the other hand is up to you. It needs to know how to paste an arbitrary bit of text into your application. 

The Lazarus IDE does more than just plain text, see synedit.pp and search for PrimarySelection - thanks to Martin_fr for the pointer. 

Further research is indicated - 

  * Should we {$ifdef LINUX} it so it does no harm in Mac and Windows. Lazarus allows windows users to do this within the app. Mac users have no concept of a middle mouse button so can never use it. Test.
  * Need to do more through tests with UTF8.
  * Is the CF_TEXT test in PrimaryPaste useful ? Is there a better way to test if content is available ?



# Lines

You can refer to individual lines of text within the KMemo using KMemo1.Blocks.Lines.Items[I] which returns a TKMemoLine. It has useful data in it about the line including start and end blocks and indexes. Note in this case, 'Line' refers to a line as currently displayed in KMemo. Just to be clear, that means, for example, altering the width of the KMemo moves the text wrapping point and alters the line numbering. 

You can get the (0 based) line number the cursor is currently on or the text that line contains - 
    
    
        LineNo := Kmemo1.Blocks.IndexToLineIndex(KMemo1.Blocks.RealSelStart);
        debugln('Current Line number ' + inttostr(LineNo));
        debugln('[' + Kmemo1.Blocks.LineText[LineNo] + ']');
    

# Locking

As your app grows, you may find that some processes start to take a significant amount of time. Here, Locking is not refering to where we lock a process to avoid interruptions, it actually speeds up the process, and quite substantially. If you have a series of (write) operations to perform on the contents of a KMemo, apply the lock before you start, release it when finished. This sort of thing - 
    
    
    try
    	KMemo1.Blocks.LockUpdate;
    	while BlockNo < Kmemo1.Blocks.Count do begin
    		SomeCrazyProcedure(KMemo.Blocks.Items[BlockNo]);
    		inc(BlockNo);
    	end
    finally
    	KMemo1.Blocks.UnlockUpdate;
    end;
    

Locking does not seem to make much difference where you are not writing to the contents nor for one-off procedures. It makes a big difference when you are doing a lot of stuff all at once. 

  * WARNING: Do not use _KMemo1.LockUpdate_ (without _.Blocks._)!
  * WARNING: If your application starts behaving oddly (selecting text happens slowly or does not happen at all) you have most likely missed to do _KMemo1.Blocks.UnlockUpdate;_ Makes sense to use a try..finally to ensure unlock happens.
  * WARNING: Do not attempt to move the cursor while locked. Bad things happen ....


  * **WARNING** : LockUpdate uses an internal counter (type int32), to prevent undesired unlocking by nested routines. So in order to unlock something you need to call .UnlockUpdate as many times as you have called .LockUpdate.



Example: 
    
    
    	KMemo1.Blocks.LockUpdate;
    	KMemo1.Blocks.LockUpdate;
    	KMemo1.Blocks.LockUpdate;
    
    	KMemo1.Blocks.UnLockUpdate; // Kmemo1 is still locked
    	KMemo1.Blocks.UnlockUpdate; // Kmemo1 is still locked
    	KMemo1.Blocks.UnLockUpdate; // Kmemo1 is unlocked
    

In order to make sure that something is unlocked you could: 
    
    
    	for i:=0 to 2147483647 do // or some counter with lower value to prevent running the loop forever
    		if KMemo1.Blocks.UpdateUnlocked=False 
    			then KMemo1.Blocks.UnLockUpdate
    			else break;
    	if KMemo1.Blocks.UnLockUpdate=False then {error handling or whatever}
    

# Bugs

Yep, it has some bugs and some not yet implemented features. But not many ! Please report bugs at the link below. 

**Canceling Bullets**

Late 2017 its been noted that there is a problem canceling Bullets on linux. Its fixed in the 2017-09-24 github version. 

**Direction of Selection**

You select text (with a mouse) either from left to right or from right to left. You may notice that in KMemo, this direction is important if you use KMemo1.SelStart etc. This is **NOT** a bug, in fact, if it matters, you should be using KMemo1.RealSelStart (etc) instead. 

## Copy and Paste Bug

Kmemo, as of April, 2018 and previous has a Unicode bug that affects Linux and Mac. It shows up if you copy unicode characters between KMemos but not, for example if you copy and paste the same text via an intermediate (non KMemo) app. The reason is that when copying directly between KMemos the text is copied in RTF format (to retain any formatting). A similar and directly related issue may occur when reading or writing RTF that contains UNicode text or Bullet Points, noting the Bullet Point is, itself, a Unicode character. 

In fact this bug actually consists of two unrelated bugs, one affecting the copying (or writeRTF) behaviour, the other affecting pasting (or loadRTF). 

## The Copy (or WriteRTF) Problem

On windows, the call to StringToAnsiString(C, FCodePage); at kmemortf#4098 converts a single Unicode multibyte character (in C) to single byte (code page aware ?). For example the english pound becomes #163. On linux, it is not changed, that is, it remains a two byte unicode character. That is because in LConvEncoding#7392, the function ConvertEncoding(const s, FromEncoding, ToEncoding: string ): string; has decided the string does not need to be converted. Effectivly, as GetDefautTextEncoding() returns 'utf8'as we are asking it to convert UTF8 to UTF8. So, quite resonably, It decides to pass the string back as it is, still in multibyte form. 

However in kmemortf#4067 there is a procedure TKMemoRTFWriter.WriteUnicodeString(const AText: TKString) that gets the string 'converted' above. At line#4099 there is - 
    
    
     if (Ansi <> '') and (Ansi <> #0) then
       begin
         S := AnsiString(Format('%s\''%.2x', [S, Ord(Ansi[1])]));
         WasAnsi := True;
       end;
    

This code assumes all char in S are single byte but as discussed, on a Linux/Mac, being UTF8 ANSI codepage system, that is not the case. 

Interestingly, in the next few lines of WriteUnicodeString() is code obviously intended to deal with multibyte characters. But it does not get used because line#4099 has already stepped in. 

So, replace the test line #4099, the block becomes - 
    
    
     if (length(Ansi) = 1) and (Ansi <> #0) then
       begin
         S := AnsiString(Format('%s\''%.2x', [S, Ord(Ansi[1])]));
         WasAnsi := True;
       end;
    

This will ensure that a multibyte char is passed on to next part of the procedure as was probably intended. 

## Paste (or LoadRTF) Issue

KMemortf ignores "listtext" as an RTF tag (kmemortf#2381). It sets FActiveState.Group to RGUNKNOWN so other methods know not to do anything with it. However, it is still processed to some extent. In particular, if the listtext params include a unicode character, ReadSpecialCharacter() will set FIgnoreChars to one so we don't use the "code page escape" character In AddText(). But we are not using any part of the listtext section so we don't call AddText() on the "code page escape" character and therefore we don't clear FIgnoreChars until we move on to the next RTF section. 

For example KMemo with a Bullet Point writes an RTF file that looks like this - 
    
    
       {\listtext{\f0\fs20\cf1 \u8226\'3F\tab }}  
    

The "u8226" is a bullet point character (it is ignored by KMemo). The "'3F' is the "?" character, there in case the app cannot handle Unicode escapes. 

After that listtext block is processed FIgnore insists we still need to drop the first displayable character. So we loose first character of text following a bullet point. 

The Solution - Line 2712 at the end of procedure TKMemoRTFReader.ReadSpecialCharacter(...) - 
    
    
     if SetIgnoreChars  then 
       FIgnoreChars := FIgnoreCharsAfterUnicode; 
    

becomes - 
    
    
     if SetIgnoreChars and (FActiveState.Group <> RGUNKNOWN) then  // DRB - April 2018
       FIgnoreChars := FIgnoreCharsAfterUnicode;
    

# Executing commands

Some commands can be executed to facilitate your work, including clipboard tasks. First _keditcommon_ shall be added in the _uses_ section. 
    
    
    ...
    uses ...
    keditcommon...
    

Then these commands are used the following way: 
    
    
      KMemo1.ExecuteCommand(ecSelectAll);
      KMemo1.ExecuteCommand(ecCopy);
      KMemo1.ExecuteCommand(ecCut);
      //etc
    

  
See the content of _TKEditCommand_ in _keditcommon_ for the list of the available commands. 

# Installation

KMemo must be installed as part of the KControls Package. As is usual, its easy. You should be using a recent version of FPC and Lazarus. Download the Github vesion of KControls, unzip and put it somewhere suitable. Fire up Lazarus, click Packages, click "Open Pack File (.lpk)" and find the kcontrolslaz.lpk file in the kit you downloaded. 

Click Compile. 

Lazarus will do its install thing, shortly will ask you about rebuilding, answer yes, it will then shutdown. On startup, you will find an extra row of components at the far right, KMemo amongst them. 

If you find this as valuable a component as I did, I suggest you hit the link below and thank TK, it is one hell of an effort ! 

# See Also

  * [KControls](<../KControls.md> "KControls") \- the package KMemo is part of.
  * <https://github.com/kryslt/KControls> \- Github repository.
  * [RichMemo](<../RichMemo.md> "RichMemo") \- an alternative package.
  * [UTF8_strings_and_characters](<../UTF8_strings_and_characters.md> "UTF8 strings and characters")
  * How to hide / show the caret? - TBD

---

_Source: [https://wiki.freepascal.org/KControls/KMemo_notes](https://web.archive.org/web/20250208184719/https://wiki.freepascal.org/KControls/KMemo_notes)_
