# Editor Macros PascalScript

│ **[English (en)](<../en/Editor_Macros_PascalScript.md> "Editor Macros PascalScript")** │  **русский (ru)** │    
****

  


## Contents

  * 1 Общее
  * 2 Доступность
  * 3 Простые действия
  * 4 Функции
  * 5 Представление объектов
    * 5.1 Caller: TSynEdit
    * 5.2 ClipBoard: TClipBoard
  * 6 Пример



# Общее

Макросы [PascalScript](<../en/PascalScript.md> "PascalScript") доступны в Lazarus 1.1 и более поздних версиях. Чтобы использовать эту функцию, вам необходимо установить пакет EditorMacroScript, который включает пакет [Pascal Script](<Pascal_Script.md> "Pascal Script/ru"). Pascal Script предоставляется объектами REM. Минимальный пакет предоставляется дистрибутивом Lazarus 1.1. 

См. также [IDE_Window:_Editor_Macros](<IDE_Window__Editor_Macros.md> "IDE Window: Editor Macros/ru")

# Доступность

Эта функциональность доступна только на определенных платформах/архитектурах: 

  * Windows 
    * 32/64bit Intel/AMD
  * Linux 
    * 32/64bit Intel/AMD
  * Mac 
    * 32(/maybe 64)bit Intel/AMD
    * 32bit PPC



# Простые действия

Все простые действия с клавиатурой представлены следующим образом: 

ecLeft;
    Перемещает каретку [на один символ] влево (в редакторе, который вызвал макрос)
ecChar('a');
    Вставляет [символ] 'a'

См. модуль SynEditKeyCmds в пакете [SynEdit](<SynEdit.md> "SynEdit/ru") и IDECommands в IDEIntf для [получения] полного списка. Или используйте Recorder для получения имен действий. 

# Функции
    
    
       Function MessageDlg( const Msg : string; DlgType : TMsgDlgType; Buttons : TMsgDlgButtons; HelpCtx : Longint) : Integer');
       Function MessageDlgPos( const Msg : string; DlgType : TMsgDlgType; Buttons : TMsgDlgButtons; HelpCtx : Longint; X, Y : Integer) : Integer');
       Function MessageDlgPosHelp( const Msg : string; DlgType : TMsgDlgType; Buttons : TMsgDlgButtons; HelpCtx : Longint; X, Y : Integer; const HelpFileName : string) : Integer');
       Procedure ShowMessage( const Msg : string);
       Procedure ShowMessagePos( const Msg : string; X, Y : Integer)');
       Function InputBox( const ACaption, APrompt, ADefault : string) : string');
       Function InputQuery( const ACaption, APrompt : string; var Value : string) : Boolean');
    

# Представление объектов

Сценарии могут ссылаться на вызывающий SynEdit через идентификатор "Caller". 

## Caller: TSynEdit

Доступны следующие методы и свойства: 

Caret (Каретка)
    
    
    
       property CaretX: Integer;
       property CaretY: Integer;
       property CaretXY: TPoint;
       property LogicalCaretXY: TPoint;
       property LogicalCaretX: Integer;
       procedure MoveCaretIgnoreEOL(const NewCaret: TPoint);
       procedure MoveLogicalCaretIgnoreEOL(const NewLogCaret: TPoint);
    

Selection (Выделение)
    
    
    
       property BlockBegin: TPoint;
       property BlockEnd: TPoint;
       property SelAvail: Boolean; // только для чтения
       property SelText: string;
       property SelectionMode: TSynSelectionMode;
       procedure ClearSelection;
       procedure SelectAll;
       procedure SelectToBrace;
       procedure SelectWord;
       procedure SelectLine(WithLeadSpaces: Boolean);
       procedure SelectParagraph;
    

  


Search/Replace (Поиск/Замена)
    
    
    
       function SearchReplace(const ASearch, AReplace: string; AOptions: TSynSearchOptions): integer;
       function SearchReplaceEx(const ASearch, AReplace: string; AOptions: TSynSearchOptions; AStart: TPoint): integer;
    
    
    
       TSynSearchOptions = set of
       ( ssoMatchCase, ssoWholeWord,
         ssoBackwards,
         ssoEntireScope, ssoSelectedOnly,  // По умолчанию от [текущего положения] каретки до конца 
                                           // текста (или к началу текста, если наоборот)
         ssoReplace, ssoReplaceAll,        // В противном случае функция выполняет только поиск
         ssoPrompt,                        // Показывать приглашение перед заменой
         ssoSearchInReplacement,           // продолжить поиск-замену в замещенном (с помощью ssoReplaceAll)/рекурсивная замена
         ssoRegExpr, ssoRegExprMultiLine,
         ssoFindContinue                   // Предположим, что текущее выделение является последним совпадением и 
                                           // начинается поиск позади выделения (впереди [выделения], если ssoBackward)
                                           // По умолчанию [поиск] начинается с [текущего положения] каретки (только 
                                           // для SearchReplace/SearchReplaceEx, [который] имеет параметр start/end);
    

Возвращает количество выполненных замен. При поиске, возвращает 1, если [совпадение] найдено, 0, если [совпадение] не найдено. Если найдено совпадение, [оно] будет выделено (используйте BlockBegin/End). 
    
    
     if Caller.SearchReplace('FindMe', ' ', []) > 0 then begin
       // Выделение устанавливается на первое появление FindMe (поиск с позиции каретки)
     end;
    

Text (Текст)
    
    
    
       property Lines[Index: Integer]: string; // read only
       property LineAtCaret: string;  // read only
       procedure InsertTextAtCaret(aText: String; aCaretMode : TSynCaretAdjustMode);
       property TextBetweenPoints[aStartPoint, aEndPoint: TPoint]: String              // Логическая точка
       procedure SetTextBetweenPoints(aStartPoint, aEndPoint: TPoint;
                                      const AValue: String;
                                      aFlags: TSynEditTextFlags = [];
                                      aCaretMode: TSynCaretAdjustMode;
                                      aMarksMode: TSynMarksAdjustMode;
                                      aSelectionMode: TSynSelectionMode
                                     );
    

Clipboard (Буфер обмена)
    
    
    
       procedure CopyToClipboard;
       procedure CutToClipboard;
       procedure PasteFromClipboard;
       property CanPaste: Boolean     // только для чтения
    

Logical / Physical (Логический / Физический)
    
    
    
       function LogicalToPhysicalPos(const p: TPoint): TPoint;
       function LogicalToPhysicalCol(const Line: String; Index, LogicalPos
                                 : integer): integer;
       function PhysicalToLogicalPos(const p: TPoint): TPoint;
       function PhysicalToLogicalCol(const Line: string;
                                     Index, PhysicalPos: integer): integer;
       function PhysicalLineLength(Line: String; Index: integer): integer;
    

## ClipBoard: TClipBoard
    
    
       property AsText: String;
    

# Пример

Макрос, показанный ниже, выравнивает выбранный код к определенному токену. Он ищет определенный токен в каждой выбранной строке и выравнивает каждое его появление. 

Выберите 3 строки. Если выделение начинается прямо перед ":", тогда символ ":" будет обнаружен. В подсказке будет предложено подтвердить ":". Затем все ":" будут выровнены. (Если вы используете слово для выравнивания, обратите внимание, что [выравнивание] будет подогнано независимо от границ слова) 
    
    
      text: string;
      a: Integer;
      foo: boolean
    

Макрос: 
    
    
    function IsIdent(c: Char): Boolean;
    begin
      Result := ((c >= 'a') and (c <= 'z')) or
                ((c >= 'A') and (c <= 'Z')) or
                ((c >= '0') and (c <= '9')) or
                (c = '_');
    end;
    
    var
      p1, p2: TPoint;
      s1, s2: string;
      i, j, k: Integer;
    begin
      if not Caller.SelAvail then exit;
      p1 := Caller.BlockBegin;
      p2 := Caller.BlockEnd;
      if (p1.y > p2.y) or ((p1.y = p2.y) and (p1.x > p2.x)) then begin
        p1 := Caller.BlockEnd;
        p2 := Caller.BlockBegin;
      end;
      s1 := Caller.Lines[p1.y - 1];
      s2 := '';
      i := p1.x
      while (i <= length(s1)) and (s1[i] in [#9, ' ']) do inc(i);
      j := i;
      if i <= length(s1) then begin
        if IsIdent(s1[i]) then // pascal identifier
          while (i <= length(s1)) and IsIdent(s1[i]) do inc(i)
        else
          while (i <= length(s1)) and not(IsIdent(s1[i]) or (s1[i] in [#9, ' '])) do inc(i);
      end;
      if i > j then s2 := copy(s1, j, i-j);
    
      if not InputQuery( 'Align', 'Token', s2) then exit;
    
      j := 0;
      for i := p1.y to p2.y do begin
        s1 := Caller.Lines[i - 1];
        k := pos(s2, s1);
        if (k > j) then j := k;
      end;
      if j < 1 then exit;
    
      for i := p1.y to p2.y do begin
        s1 := Caller.Lines[i - 1];
        k := pos(s2, s1);
        if (k > 0) and (k < j) then begin
          Caller.LogicalCaretXY := Point(k, i);
          while k < j do begin
            ecChar(' ');
            inc(k);
          end;
        end;
      end;
    
    end.
    

  
См. <http://forum.lazarus.freepascal.org/index.php/topic,27186.msg167883.html#msg167883> для подсчета элементов массива.

---

_Source: [https://wiki.freepascal.org/Editor_Macros_PascalScript/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Editor_Macros_PascalScript/ru)_
