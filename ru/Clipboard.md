# Clipboard

│ **[English (en)](<../en/Clipboard.md>)** │  **русский (ru)** │

  


## Contents

  * 1 Предопределенные типы
  * 2 Текст
  * 3 Текстовые элементы управления
  * 4 Текст в формате HTML
    * 4.1 Windows
  * 5 Изображения
    * 5.1 Загрузка из буфера обмена
    * 5.2 Сохранение в буфере обмена
  * 6 Пользовательский формат
  * 7 Multiple objects
  * 8 Получение уведомления об изменении буфера обмена
  * 9 Просмотр содержимого буфера обмена
  * 10 How to fix empty GTK2 clipboard on exit
  * 11 Внешние ссылки



## Предопределенные типы

TPredefinedClipboardFormat  |  тип MIME   
---|---  
pcfText  |  text/plain   
pcfBitmap  |  image/bmp   
pcfPixmap  |  image/xpm   
pcfIcon  |  image/lcl.icon   
pcfPicture  |  image/lcl.picture   
pcfMetaFilePict  |  image/lcl.metafilepict   
pcfObject  |  application/lcl.object   
pcfComponent  |  application/lcl.component   
pcfCustomData  |  application/lcl.customdata   
  
## Текст

Для использования с простым текстом объект **Clipboard** предоставляет свойство _AsText_ , которое может быть использовано для чтения и записи текста. 

Запись текста: 
    
    
    Clipboard.AsText := 'Hello clipboard!';

Чтение текста: 
    
    
    ShowMessage('Clipboard content: ' + Clipboard.AsText);

**Clipboard** является объектом класса **TClipboard** и для его использования необходимо подключить модуль _Clipbrd_ в разделе _uses_ : 
    
    
    uses
    ..., Clipbrd;

## Текстовые элементы управления

Некоторые визуальные компоненты, такие как [TEdit](<TEdit.md> "TEdit/ru"), [TMemo](<TMemo.md> "TMemo/ru"), [TStringGrid](<TStringGrid.md> "TStringGrid/ru"), [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit/ru"), [TMaskEdit](<TMaskEdit.md> "TMaskEdit/ru"), [TSpinEdit](<TSpinEdit.md> "TSpinEdit/ru") и [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit/ru") обладают возможностью выделения части текста, находящегося в них, и предоставляют дополнительные функциональные возможности для выделенного текста при работе с буфером обмена. 
    
    
      procedure CopyToClipboard; 
      procedure CutToClipboard; 
      procedure PasteFromClipboard;

## Текст в формате HTML

Объект **ClipBoard** поддерживает чтение и запись текста в формате HTML. 

Пример чтения из буфера обмена и записи в буфер обмена текста в формате HTML: 
    
    
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

Для обработки Html-текста в буфере обмена Windows требуется подключение заголовочных файлов.  
В то время, как раньше пользователям необходимо было делать это вручную, теперь это прозрачно делается с помощью подключения модуля _ClipBrd_.  


## Изображения

### Загрузка из буфера обмена
    
    
    uses 
      Clipbrd, LCLIntf, LCLType, ...;
     
    procedure LoadBitmapFromClipboard(Bitmap: TBitmap);
    begin
      if Clipboard.HasFormat(PredefinedClipboardFormat(pcfDelphiBitmap)) then
        Bitmap.LoadFromClipboardFormat(PredefinedClipboardFormat(pcfDelphiBitmap));
      if Clipboard.HasFormat(PredefinedClipboardFormat(pcfBitmap)) then
        Bitmap.LoadFromClipboardFormat(PredefinedClipboardFormat(pcfBitmap));
    end;

### Сохранение в буфере обмена
    
    
    uses 
      Clipbrd, ...;
     
    procedure SaveBitmapToClipboard(Bitmap: TBitmap);
    begin
      Clipboard.Assign(Bitmap);
    end;

## Пользовательский формат

## Multiple objects

## Получение уведомления об изменении буфера обмена

LCL не передает сообщения Windows (передаются только сообщения WM_USER). Это означает, что вы должны написать свой обработчик сообщения. 

Пример кода для реализации обработчика сообщения: 
    
    
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
        FNextClipboardOwner: HWnd;   // хэндл на следующий вьювер в цепочке
        // обработчики события буфера обмена
        function WMChangeCBChain(wParam: WParam; lParam: LParam):LRESULT;
        function WMDrawClipboard(wParam: WParam; lParam: LParam):LRESULT;
      public
      end;
     
    var
      Form1: TForm1;
     
    implementation
     
    {$R *.lfm}
    var
      PrevWndProc:windows.WNDPROC;
     
    function WndCallback(Ahwnd: HWND; uMsg: UINT; wParam: WParam; lParam: LParam): LRESULT; stdcall;
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
     
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PrevWndProc := Windows.WNDPROC(SetWindowLong(Self.Handle, GWL_WNDPROC, PtrInt(@WndCallback))); // для x64 необходимо использовать SetWindowLongPtr
      FNextClipboardOwner := SetClipboardViewer(Self.Handle);
    end;
     
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      ChangeClipboardChain(Handle, FNextClipboardOwner);
    end;
     
    function TForm1.WMChangeCBChain(wParam: WParam; lParam: LParam): LRESULT;
    var
      Remove, Next: THandle;
    begin
      Remove := WParam;
      Next := LParam;
      if FNextClipboardOwner = Remove then FNextClipboardOwner := Next
        else if FNextClipboardOwner <> 0 then
          SendMessage(FNextClipboardOwner, WM_ChangeCBChain, Remove, Next)
    end;
     
    function TForm1.WMDrawClipboard(wParam: WParam; lParam: LParam): LRESULT;
    begin
      if Clipboard.HasFormat(CF_TEXT) Then Begin
        ShowMessage(Clipboard.AsText);
      end;
      SendMessage(FNextClipboardOwner, WM_DRAWCLIPBOARD, 0, 0);   // ВАЖНО!
      Result := 0;
    end;
     
    end.

## Просмотр содержимого буфера обмена

Иногда бывает полезно посмотреть, что находится в данный момент в буфере обмена. Вот несколько методов, которые я использую (на форме расположен элемент [TMemo](<TMemo.md> "TMemo/ru") и один таймер с интервалом 1 секунда) - 
    
    
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
                //Memo1.Append(List.Strings[i]);        // раскомментируйте, чтобы увидеть все доступные форматы
                case List.Strings[i] of                 // показать конкретные форматы
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

## Внешние ссылки

  * [Базовые операции для работы с буфером обмена (вырезать/скопировать/вставить) при использовании объекта TClipboard](<http://delphi.about.com/od/vclusing/a/tclipboard.htm>)

---

_Source: [https://wiki.freepascal.org/Clipboard/ru](https://web.archive.org/web/20190424202932/https://wiki.freepascal.org/Clipboard/ru)_
