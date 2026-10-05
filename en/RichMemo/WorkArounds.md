# RichMemo/WorkArounds

[RichMemo](<../RichMemo.md> "RichMemo") is a great component, but its not finished. This page lists some work arounds for some incomplete RichMemo functions. Its important to note that the things mentioned here are NOT official characteristics of RichMemo. They are things found to work in the current release. Its likely they will change in the future, please don't assume that they will continue to work or even that they will even continue to be needed. 

If you use any of these work arounds, mark your code accordingly and test carefully when you update RichMemo ! If you don't need a work around, get rid of it, some of these here are quite ugly ! 

## Contents

  * 1 Method Status Table
  * 2 Observed Issues
    * 2.1 GetParaRange()
    * 2.2 Search() fails under Cocoa
    * 2.3 OnLinkAction does not set LinkStart and LinkLen on Win10
    * 2.4 I Cannot determine the system default or application font.
    * 2.5 SetLink() Fails on Linux
    * 2.6 SetLink() sometimes changes the font
    * 2.7 GetStyleRange(..) does not count changes in background colour
    * 2.8 Sometimes newly typed characters are not made visible, Windows
  * 3 See Also



### Method Status Table

Remember, we are talking about **RichMemo Trunk** here ! Very early draft, dated October 2020, many tests incomplete here.... 

Feature  | Win32  | Gtk2  | Qt5  | Cocoa   
---|---|---|---|---  
GetTextLen  |  | OK  | OK  | OK (Nov 2020)   
GetTextAttributes  |  | OK except for background colour, its always reported as black unless it has been explicitly set previously.  | Most settings don't work unless they have been explicitly set previously.  | OK   
SetTextAttributes / SetRangeParams  |  | OK  | not doing highlight  | OK   
GetStyleRange  |  | OK  | Broken, returns every char as a range, fix with <https://bugs.freepascal.org/view.php?id=37894> | OK   
SetRangeParams  |  | OK  | OK except background colour  | OK   
GetParaRange  |  | OK  | Broken, workaround below.  | OK   
Search  |  | OK  | broken, see workaround below  | broken, see workaround below   
SetLink  |  | OK  | broken  | broken   
GetParaMetric  |  | OK  | Not implemented  | OK   
SetParaMetric  |  | OK  | Not implemented  | OK   
GetParaNumbering  |  | Not implemented  | Not implemented  | Not implemented   
SetParaNumbering  |  | Bullets work but cannot cancel  | Not implemented  | Not implemented   
  
Note : as of Oct 2020, I have not tested any of the Windows functions above. 

### Observed Issues

#### GetParaRange()

**Status** : Linux GTK2 OK, Cocoa OK. Qt5 need this fix, Windows TBD 

**The Problem** : On qt5 this method is not implemented. However, all we need to know is available in the RichMemo widget at a pascal level so an easy implementation is as below. It may not be as fast as hitting the native API but if not, not by a big margin. Note that this is for Cocoa, exactly the same code for Qt5 but the method name is different (obviously!) : 

Declare it in the **interface** section for TCocoaWSCustomRichMemo, **public** \- 
    
    
        class function GetParaRange(const AWinControl: TWinControl; TextStart: Integer;
                        var rng: TParaRange): Boolean; override;
    

And then, in **implementation** section - 
    
    
    class function TCocoaWSCustomRichMemo.GetParaRange(const AWinControl: TWinControl;
                    TextStart: Integer; var rng: TParaRange): Boolean;
    var
      Str : string;
      Len, Tmp : integer;
    begin
        // This a new function for RichMemo Cocoa DRB 2020-10-01
        // As well as getting GetParaRnge working, it also fixes Indents.
        //This may not be either the fastest nor most memory efficent but will do for now.
        if not AWinControl.ClassNameIs('TRichMemo') then exit(False);
        if TextStart < 0 then exit(false);
        Len := TRichMemo(AWinControl).GetTextLen;   // some overhead so lets not repeat call
        if (Len = 0) and (TextStart = 0) then begin // Special case.
            rng.start := 0;
            rng.length := 0;
            exit(true);
        end;
        if TextStart >= Len then exit(False);
        Str := TRichMemo(AWinControl).Text;         // some overhead so lets not repeat call
        rng.start := TextStart;
    
        if (Str[rng.start+1] = #10) and (rng.start > 0) and (Str[rng.start] <> #10)
            then dec(rng.start);
        if (Str[rng.start+1] <> #10) then begin     // If on a newline, we dont need to search for it
            while (Str[rng.start+1] <> #10) and (rng.start > 0) do
                dec(rng.Start);
            if  Str[rng.start+1] = #10 then inc(rng.start);     // We may have overrun
        end;
    
        if (Str[rng.start+1] = #10) or (rng.start >= Len) then
            rng.length := 1
        else begin
            rng.length := 1;
            while (rng.start + rng.length < Len) and (Str[rng.start + rng.length {+1}] <> LineEnding) do
                inc(rng.length);
        end;
        Result := True;
    end;
    

#### Search() fails under Cocoa

**Status** : Linux,Windows - OK; Mac Cocoa fails. 

**The Problem** : On Cocoa, always returns false. 

Logged : 

Documented at [RichMemo#Search](<../RichMemo.md> "RichMemo")

**The Test** : 
    
    
    var
        Start, len : longint;
    begin
        RichMemo1.Clear;
        RichMemo1.Append('This is some text to search');
        if Richmemo1.Search('some', 1, 25, [], Start, Len) then
            showmessage('found ' + inttostr(Start) + ' ' + inttostr(Len))
        else showmessage('Failed to find');
    end;
    

Tells us it found nothing. 

**The Fix** : Nothing tested, it should/could be possible to search RichMemo1.Text, as its a Mac only issue and Mac has single char line ending, reasonably fast and easy ? 

#### OnLinkAction does not set LinkStart and LinkLen on Win10

**Problem** : OnLinkAction does not set LinkStart and LinkLen on Win10. A call back procedure, eg Form1.RichMemo1LinkAction(..) has those two parameters to indicate where the link the user clicked is. However, on Win10 (at least) they are set randomly. 

**The test** : On a Form, put a RichMemo and a button. Make the Button's Click event look like this - 
    
    
    procedure TForm1.ButtonLinkClick(Sender: TObject);
    begin
        RichMemo1.Clear;
        RichMemo1.Append('A Link OK ? (give it a click)');
        RichMemo1.SetLink(2, 4, True);
    end;
    

Then make the RichMemo's OnLinkAction event look like - 
    
    
    procedure TForm1.RichMemo1LinkAction(Sender: TObject; ALinkAction: TLinkAction;
      const info: TLinkMouseInfo; LinkStart, LinkLen: Integer);
    begin
         Showmessage('clicked [' + RichMemo1.GetText(LinkStart, LinkLen) + '] at ' + inttostr(LinkStart) + ' '	+ inttostr(LinkLen));
    end;
    

Then run it, click the button, click the link. 

**The Fix** : Fortunately, SelStart is set to where the user clicked, must be somewhere on the link. 
    
    
    procedure TEditBoxForm.RichMemo1LinkAction(Sender: TObject;
        ALinkAction: TLinkAction; const info: TLinkMouseInfo; LinkStart,
        LinkLen: Integer);
    var
         Index, Len : longint;
    begin
        Index := RichMemo1.SelStart;
        Len := 0;
        while RichMemo1.IsLink(Index) do dec(Index);
        inc(Index);
        While RichMemo1.isLink(Index + Len) do inc(Len);
        Showmessage('clicked [' + RichMemo1.GetText(Index, Len) + '] at ' + inttostr(Index) + ' '	+ inttostr(Len));
    end;
    

#### I Cannot determine the system default or application font.

**The Problem** : Cannot determine the system default or application font. Maybe someone else can suggest a more elegant way to find out this information, I cannot ! And it turns out that we need to know that font because a call to SetLink() changes the in use font to that one ! 

**The Fix** : Well, this is tacky but works. Make a call to SetLink() from OnShow(..), record the font and then clear it. Should work on all systems. 
    
    
    procedure TEditBoxForm.FormShow(Sender: TObject);
    var
      FP :  TFontParams;
    begin
        RichMemo1.Clear;                     
        RichMemo1.Append('a line of text');
        Richmemo1.SetLink(2, 4, true);       
        RichMemo1.GetTextAttributes(4, FP);
        Showmessage('Looks like we are using ' + inttostr(FP.Size) + ' ' + FP.Name);
        RichMemo1.Clear;
     end;
    

#### SetLink() Fails on Linux

**Status** : Linux - needs patch; Windows - OK; Mac - Not Working. 

**The Problem** : SetLink() Fails on Linux. 

Reported : <http://forum.lazarus.freepascal.org/index.php/topic,37850.0.html>

Seems that if there is an existing tag covering the spot where we want to put a link, it fails. But if we clear any tags in that area first, all good. This may not suit all applications however ! Add a line to GTK2RichMemo, #1349 in class procedure Gtk2WSCustomRichMemo.SetTextUIParams(...) just before the call to gtk_text_buffer_apply_tag_by_name(...) - 
    
    
    gtk_text_buffer_remove_all_tags(buffer,  @istart, @iend);
    

This will clear all tags and so gtk_text_buffer_apply_tag_by_name(...) has no problem setting 'link'. I am afraid I do not know why this is necessary and, yes, it will obviously clear tags on that text you may want there so, I don't think its a great solution, just one that suits me and is, perhaps, better than the existing behaviour. 

#### SetLink() sometimes changes the font

**The Problem** : SetLink() sometimes changes the font. This happens when, for example, your code has detected that the user has just typed in a link address, SetLink() is called but as the user continues to type the font may look different. Its because after the call, the in use font is changed to be the system or application font. But before that call, it was what ever font RichMemwas using. 

Documented : Not officially. This function, eg RichMemo1.SetLink(Start, Len, True), marks an existing section of text as being a 'Link'. Start is the zero based position of the character of the link, Len is the number of characters in the link, True means make a link, False means remove an existing link. An optional forth parameter is "LinkData" (that might be returned to the event) however, I could not get that to work and did not need it anyway... 

**The Fix** : One solution to this problem is to make sure we are using the System Font all along. See above item on this page .... 

#### GetStyleRange(..) does not count changes in background colour

**The Problem** : GetStyleRange(..) does not count changes in background colour when determining range boundaries under Windows. This function is documented at [RichMemo#GetStyleRange](<../RichMemo.md> "RichMemo")

**The Fix** : Its trivial to make a new version of GetStyleRange(..) to replace the one built into RichMemo as long as you have already patched RichMemo1.GetTextAttributes(Index, FPr), as detailed above. 
    
    
    function Form1.FPisEqual(const FP, FPRef : TFontParams) : boolean;
    begin
      Result := false;
      if FP.name  FPRef.name then exit();
      if FP.size  FPRef.size then exit();
      if FP.bkColor  FPRef.bkcolor then exit();
      if FP.style  FPRef.style then exit();
      if FP.HasBkClr  FPRef.HasBkClr then exit();
      Result := true
    end;
    
    function Form1.GetStyleRange(const Index : longint; out Start, Len : longint) : boolean;
    var
            FP, FPr :  TFontParams;
    begin
        Result := false;
        Start := Index;
        if not RichMemo1.GetTextAttributes(Index, FPr) then exit();
        repeat
        	dec(Start);
            if not RichMemo1.GetTextAttributes(Start, FP) then break;
        until not FPisEqual(FP, FPr);
        inc(Start);
        Len := 0;
        repeat
            inc(Len);
            if not RichMemo1.GetTextAttributes(Start + Len, FP) then break;
        until not FPisEqual(FP, FPr);
        Result := True;
    end;
    

So, instead of calling RichMemo1.GetStyleRange(..) just use GetStyleRange(..). The above fix has been added to CocoaRichMemo, trunk. 

#### Sometimes newly typed characters are not made visible, Windows

**Status** : Linux - OK; Windows - Needs Workaround; Mac - OK. 

**The Problem** : Newly typed characters are not made visible, Windows. Seems to happen for exmple when you scan over the document after each keypress by hooking into the OnChange(..) event. Or you write something into the RichEdit and immediatly call (eg) GetTextAttributes(). Wiping over with the mouse or moving the control makes the newly written text visible again. Strange. 

**The Test** : On a form, put a RichText and a button. Make the Button's Click event look like - 
    
    
    procedure TForm1.ButtonCheckVisibleClick(Sender: TObject);
    var
    	FP :  TFontParams;
    begin
        RichMemo1.Append('Can you see this line of text ?');
        RichMemo1.GetTextAttributes(1, FP);              
    end;
    

  
**The Fix** : (I warned you some of these are pretty ugly) I found that a call to SetLink(..) will refresh things ! Passing it 'false' says turn the link off, there is not one there anyway so all good. So make the above look like this - 
    
    
    procedure TForm1.ButtonCheckVisibleClick(Sender: TObject);
    var
    	FP :  TFontParams;
    begin
        RichMemo1.Append('Can you see this line of text ?');
        RichMemo1.GetTextAttributes(1, FP);
        RichMemo1.SetLink(0, 1, False);          
    end;
    

This is not necessary under Linux or Mac but it does no real harm, technically its a waste of cycles so you may want to use an $IFDEF 

  


## See Also

  * [RichMemo](<../RichMemo.md> "RichMemo") \- the main page

---

_Source: [https://wiki.freepascal.org/RichMemo/WorkArounds](https://web.archive.org/web/20250418110406/https://wiki.freepascal.org/RichMemo/WorkArounds)_
