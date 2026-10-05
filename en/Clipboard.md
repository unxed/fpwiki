# Clipboard

│ **English (en)** │  **[русский (ru)](<../ru/Clipboard.md>)** │



## Contents

  * 1 Predefined types
  * 2 Text
  * 3 Text oriented components
  * 4 HTML source
    * 4.1 Windows
  * 5 Image
    * 5.1 Load from clipboard
    * 5.2 Save to clipboard
  * 6 Custom format
  * 7 Multiple objects
  * 8 Getting notified of changes
  * 9 View the clipboard contents
  * 10 How to fix empty GTK2 clipboard on exit
  * 11 External links



## Predefined types

TPredefinedClipboardFormat | MIME type   
---|---  
pcfText | text/plain   
pcfBitmap | image/bmp   
pcfPixmap | image/xpm   
pcfIcon | image/lcl.icon   
pcfPicture | image/lcl.picture   
pcfMetaFilePict | image/lcl.metafilepict   
pcfObject | application/lcl.object   
pcfComponent | application/lcl.component   
pcfCustomData | application/lcl.customdata   
  
## Text

For use of simple text Clipboard offer property AsText which can be used for reading an writing plain text. 

Writing text: 
    
    
    Clipboard.AsText := 'Hello clipboard!';
    

Reading text: 
    
    
    ShowMessage('Clipboard content: ' + Clipboard.AsText);
    

Clipboard is a TClipboard variable and the Clipbrd unit should be added in the uses clause in order to use the variable: 
    
    
    uses
    ..., Clipbrd;
    

## Text oriented components

Some visual components like [TEdit](<TEdit.md> "TEdit"), [TMemo](<TMemo.md> "TMemo"), [TStringGrid](<TStringGrid.md> "TStringGrid"), [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit"), [TMaskEdit](<TMaskEdit.md> "TMaskEdit"), [TSpinEdit](<TSpinEdit.md> "TSpinEdit") and [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit") have ability to select a part of contained text and offer additional functionality for handling clipboard operations on selected text. 
    
    
      
    procedure CopyToClipboard; 
      procedure CutToClipboard; 
      procedure PasteFromClipboard;
    

## HTML source

The ClipBoard supports reading and writing of [HTML](</index.php?title=HTML&action=edit&redlink=1> "HTML \(page does not exist\)"). 

Example of reading from and writing HTML source to clipboard: 
    
    
    uses
      Clipbrd, ...;
    var
      Html, PlainText: String;
    ...
    begin
      Html := ClipBoard.GetAsHtml;
      ...
      Html := '<b>Formatted</b> text';
      PlainText := 'Simple Text';
      ClipBoard.SetAsHtml(Html, PlainText);
    end.
    

### Windows

Handling Html content on the clipboard in Windows requires fidling with headers on Windows.  
While in the past users had to do this manually, this now is handled transparently by the ClipBrd unit.  


## Image

### Load from clipboard
    
    
    uses 
      Clipbrd, LCLIntf, LCLType, ...;
    
    procedure LoadBitmapFromClipboard(Bitmap: TBitmap);
    begin
      if Clipboard.HasFormat(PredefinedClipboardFormat(pcfDelphiBitmap)) then
        Bitmap.LoadFromClipboardFormat(PredefinedClipboardFormat(pcfDelphiBitmap));
      if Clipboard.HasFormat(PredefinedClipboardFormat(pcfBitmap)) then
        Bitmap.LoadFromClipboardFormat(PredefinedClipboardFormat(pcfBitmap));
    end;
    

### Save to clipboard
    
    
    uses 
      Clipbrd, ...;
    
    procedure SaveBitmapToClipboard(Bitmap: TBitmap);
    begin
      Clipboard.Assign(Bitmap);
    end;
    

## Custom format

## Multiple objects

## Getting notified of changes

The LCL does not pass on Windows messages. It only passes on messages > WM_USER. This means you have to write your own message handler. [Processing non - user messages in your window](<http://wiki.lazarus.freepascal.org/Win32/64_Interface#Processing_non-user_messages_in_your_window>)

Sample code to implement message handler: 
    
    
    unit Unit1;
    
    {$mode delphi}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs,
      Clipbrd, StdCtrls, Windows, Messages;
    
    type
      TForm1 = class(TForm)
        Button1: TButton;
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        FNextClipboardOwner: HWnd;   // handle to the next viewer
        // Here are the clipboard event handlers
        function WMChangeCBChain(AwParam: WParam; AlParam: LParam):LRESULT;
        function WMDrawClipboard(AwParam: WParam; AlParam: LParam):LRESULT;
      public
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    var
      PrevWndProc:windows.WNDPROC;
    
    function WndCallback(Ahwnd: HWND; uMsg: UINT; wParam: WParam;
      lParam: LParam): LRESULT; stdcall;
    begin
      if uMsg = WM_CHANGECBCHAIN then begin
        Result := Form1.WMChangeCBChain(wParam, lParam);
        Exit;
      end 
      else if uMsg=WM_DRAWCLIPBOARD then begin
        Result := Form1.WMDrawClipboard(wParam, lParam);
        Exit;
      end;
      Result := CallWindowProc(PrevWndProc, Ahwnd, uMsg, WParam, LParam);
    end;
    
    { TForm1 }
    
    {Don't forget to assign to the OnCreate event.}
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PrevWndProc := Windows.WNDPROC(SetWindowLongPtr(Self.Handle, GWL_WNDPROC, PtrUInt(@WndCallback)));
      FNextClipboardOwner := SetClipboardViewer(Self.Handle);
    end;
    
    {Don't forget to assign to the OnDestroy event.}
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      ChangeClipboardChain(Handle, FNextClipboardOwner);
    end;
    
    function TForm1.WMChangeCBChain(AwParam: WParam; AlParam: LParam): LRESULT;
    var
      Remove, Next: THandle;
    begin
      Remove := AwParam;
      Next := AlParam;
      if FNextClipboardOwner = Remove then FNextClipboardOwner := Next
        else if FNextClipboardOwner <> 0 then
          SendMessage(FNextClipboardOwner, WM_ChangeCBChain, Remove, Next)
      Result := 0;
    end;
    
    function TForm1.WMDrawClipboard(AwParam: WParam; AlParam: LParam): LRESULT;
    begin
      if Clipboard.HasFormat(CF_TEXT) Then Begin
        ShowMessage(Clipboard.AsText);
      end;
      SendMessage(FNextClipboardOwner, WM_DRAWCLIPBOARD, 0, 0);   // VERY IMPORTANT
      Result := 0;
    end;
    
    end.
    

## View the clipboard contents

Sometimes its useful to see whats actually in the clipboard at any one time. Here are a couple of methods I use, on a form with a TMemo and a one second timer to do just that - 
    
    
    procedure TForm1.CheckClipboard();
    var
        I : integer;
        List : TStringList;
    begin
        memo1.clear;
        Memo1.Append('[' + Clipboard.AsText + ']');
        List := TStringList.Create;
        try
            ClipBoard.SupportedFormats(List);
            for i := 0 to List.Count-1 do begin
                //Memo1.Append(List.Strings[i]);        // uncomment to see all available formats
                case List.Strings[i] of                 // show these specific ones
                    'Rich Text Format', 'text/plain', 'UTF8_STRING' :
                        ReadClip(List.Strings[i]);
                end;
            end;
        finally
          List.Free;
        end;
    end;
    
    function TForm1.ReadClip(TheFormat : ANSIString) : ANSIString;
    var
      Stream: TMemoryStream;
      Fmt : TClipboardFormat;
      List : TStringList;
    begin
        if TheFormat = '' then exit;
        Stream := TMemoryStream.Create;
        List := TStringList.Create;
      try
        if Clipboard.HasFormatName(TheFormat) then begin
            Memo1.Append(#10+TheFormat);
            Fmt := ClipBoard.FindFormatID(TheFormat);
            ClipBoard.GetFormat(Fmt, Stream);
            if Stream.Size > 0 then begin
                Stream.Seek(0, soFromBeginning);
                List.LoadFromStream(Stream);
                Memo1.Lines.AddStrings(List, False);
            end;
        end;
      finally
        List.Free;
        Stream.Free;
      end;
    end;
    

## How to fix empty GTK2 clipboard on exit

Usually when your GTK2 app exits, it's clipboard becomes empty. Bad for usual user. This unit is a dirty fix, add it to "uses" somewhere. 
    
    
    unit fix_gtk_clipboard;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      gtk2, gdk2, Clipbrd;
    
    implementation
    
    var
      c: PGtkClipboard;
      t: string;
    
    finalization
      c := gtk_clipboard_get(GDK_SELECTION_CLIPBOARD);
      t := Clipboard.AsText;
      gtk_clipboard_set_text(c, PChar(t), Length(t));
      gtk_clipboard_store(c);
    end.
    

In May 2018 I (dbannon) found that putting this bit of code in a finalization section did fix the problem when the clipboard contents had come from the application itself but if it was there before the application started, that is, the application did not write to the clipboard, it introduces another, similar problem. Clipboard contents are, again, cleared in that case. And appears to happen because by time the finalization clause is executed, the clipboard has already been cleared. 

An easy solution is to put that same code into the main form's onClose event. Its early enough that the contents, from either source are still there and late enough not to be subsequently cleared. 
    
    
    uses .... {$ifdef LINUX}gtk2, gdk2, Clipbrd{$endif};
    .....
    procedure TMainForm.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    var
      c: PGtkClipboard;
      t: string;
    begin
        {$ifdef LINUX}
        c := gtk_clipboard_get(GDK_SELECTION_CLIPBOARD);
        t := Clipboard.AsText;
        gtk_clipboard_set_text(c, PChar(t), Length(t));
        gtk_clipboard_store(c);
        {$endif}
    end;
    

## External links

  * [Basic Clipboard Operations (Cut/Copy/Paste) using the TClipboard object](<http://delphi.about.com/od/vclusing/a/tclipboard.htm>)

---

_Source: [https://wiki.freepascal.org/Clipboard](https://web.archive.org/web/20230202080622/https://wiki.freepascal.org/Clipboard)_
