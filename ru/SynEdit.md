# SynEdit

│ **[Deutsch (de)](</SynEdit/de> "SynEdit/de")** │  **[English (en)](<../en/SynEdit.md> "SynEdit")** │  **[español (es)](</SynEdit/es> "SynEdit/es")** │  **[français (fr)](</SynEdit/fr> "SynEdit/fr")** │  **[日本語 (ja)](</SynEdit/ja> "SynEdit/ja")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</SynEdit/zh_CN> "SynEdit/zh CN")** │    
****

**SynEdit** \- пакет [подсветки синтаксиса](<../en/Syntax_highlighting.md> "Syntax highlighting") для edit/memo, доступный на [вкладке SynEdit](<SynEdit_tab.md> "SynEdit tab/ru") с поддержкой многих языков/синтаксиса. 

SynEdit, содержащийся в Lazarus, был ответветлен от [SynEdit 1.0.3](<https://sourceforge.net/projects/synedit/files/>), адаптирован и довольно сильно расширен. Изменения перечислены ниже. 

Пакет Lazarus содержит компонент редактора исходного кода под названием [TSynEdit/ru|[TSynEdit]], несколько подсветок синтаксиса и другие компоненты, используемые для редактирования исходного кода. 

Лицензирован на тех же условиях, что и исходный SynEdit (MPL или GPL). 

## Contents

  * 1 Оригинальная версия против версии Lazarus
  * 2 SynEdit 2.0 port
  * 3 SynEdit в IDE
  * 4 Использование SynEdit
    * 4.1 Подсветка
    * 4.2 Изменение существующего маркера подсветки
    * 4.3 Плагины для автозавершения кода
    * 4.4 Логическая/Физическая позиция каретки
    * 4.5 Изменение текста из кода
    * 4.6 Сворачивание/Разворачивание текста из кода
    * 4.7 Закладки
    * 4.8 Дополнительная информация
    * 4.9 Примеры приложений
    * 4.10 Добавление горячих клавиш для Cut/Copy/Paste/и т.д.
  * 5 Дальнейшее развитие, обсуждение
  * 6 См. также



## Оригинальная версия против версии Lazarus

Версия Lazarus поддерживается в основном [[[User:Martin|Martin Friebe]]. Martin написал на форуме, что было добавлено в версию Lazarus с момента появления fork: 

Большие изменения, добавленные в версию Lazarus: 

  * сворачивание блоков кода
  * настраиваемые боковое поле / части бокового поля
  * общий текст между несколькими редакторами
  * поддержка utf-8
  * плагин для синхронизации
  * базовая поддержка RTL/LTR
  * настройка мыши через MouseActions
  * переписаны различные модули подсветки/разметки



Кодовые базы версий Delphi / Lazarus были независимо переработаны. Осталось очень мало совпадений. 

## SynEdit 2.0 port

Существует альтернативный порт оригинальной версии SynEdit версии 2.0.x. Активность не поддерживается, последний коммит (сейчас июнь 2014) был в 2011 году. 

  * [SynEdit/port](<../en/SynEdit/port.md> "SynEdit/port")
  * [Github page](<https://github.com/rnapoles/>)



## SynEdit в IDE

SynEdit в Lazarus - это встроенный пакет, потому что среда IDE использует его сама. Поэтому пакет не может быть удален из списка установки. Чтобы удалить записи из палитры компонентов, пакет SynEditDsgn можно удалить из установки. 

## Использование SynEdit

### Подсветка

  * Есть несколько стандартных маркеров подсветки (см. [вкладку SynEdit](<SynEdit_tab.md> "SynEdit tab/ru") в [палитре компонентов](<Component_Palette.md> "Component Palette/ru"))
  * Существуют сценарии подсветки, которые можно адаптировать ко многим другим форматам файлов: 
    * [TSynAnySyn](</index.php?title=TSynAnySyn&action=edit&redlink=1> "TSynAnySyn \(page does not exist\)") (стандартный, на палитре компонентов по умолчанию)
    * [TSynPositionHighlighter](</index.php?title=TSynPositionHighlighter&action=edit&redlink=1> "TSynPositionHighlighter \(page does not exist\)") (стандартный, **не** на палитре компонентов)
    * [TSynUniHighlighter](</index.php?title=TSynUniHighlighter&action=edit&redlink=1> "TSynUniHighlighter \(page does not exist\)") (стандартный, **не** на палитре компонентов)
    * SynFacilSyn ([Github](<https://github.com/t-edson/SynFacilSyn>))
  * Существуют и другие сторонние маркеры подсветки: SynCacheSyn, SynGeneralSyn, SynRCSyn, SynRubySyn, SynSDDSyn, SynSMLSyn, SynSTSyn, SynTclTkSyn, SynUnrealSyn, SynURISyn, SynVBScriptSyn, SynVrml97Syn, [см. здесь](<http://bugs.freepascal.org/view.php?id=18248>).
  * Вы можете написать новый маркер подсветки, см. информацию в [SynEdit Highlighter](<../en/SynEdit_Highlighter.md> "SynEdit Highlighter").



### Изменение существующего маркера подсветки

Иногда у вас может появиться желание отредактировать существующие маркеры подсветки (как этого хотел я несколько дней назад), которые уже существуют. В этом примере мы собираемся отредактировать маркер подсветку для паскаль-подобного кода (classname: [TSynPasSyn](<../en/TSynPasSyn.md> "TSynPasSyn"); package: SynEdit V1.0; unit: SynHighlighterPas.pas). 

Скажем, мы хотим достичь того, чтобы наше приложение (в данном случае Lazarus) различало три типа комментариев, которые существуют в Pascal: 
    
    
      (* ansi *)
      { bor }
      // Slash
    

Это может быть полезно, если вы хотите различать различные типы ваших комментариев (например, "Description", "Note", "Reference" и т.д.) и хотите, чтобы они были, например, окрашивались по-разному. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** На случай, если вы что-то сломаете, я предлагаю сделать несколько "NEW" и "/NEW"-комментариев, но вам не нужно

  * Сначала откройте модуль "SynHighlighterPas", который должен находиться в вашем SynEdit-каталоге.
  * Поскольку мы не хотим создавать несовместимости, мы создаем новый перечислимый тип, который поможет нам позже идентифицировать наш комментарий:



Напр., под объявлением "tkTokenKind" напишите это: 
    
    
      {NEW}
      TtckCommentKind = (tckAnsi, tckBor, tckSlash);
      {/NEW}
    

  * В объявлении "TSynPasSyn" найдите "FTokenID" и добавьте следующее между "FTokenID" и следующим полем


    
    
      {NEW}
      FCommentID: TtckCommentKind;
      {/NEW}
      //Это создает новое поле, где мы можем хранить информацию, какой у нас комментарий
    

  * В объявлении "TSynPasSyn" найдите "fCommentAttri" и добавьте следующее между "fCommentAttri" и следующим полем


    
    
      {NEW}
      fCommentAttri_Ansi: TSynHighlighterAttributes;
      fCommentAttri_Bor: TSynHighlighterAttributes;
      fCommentAttri_Slash: TSynHighlighterAttributes;
      {/NEW}
      //Это позволяет нам возвращать различные атрибуты для каждого типа комментариев.
    

  * Затем найдите определение конструктора "TSynPasSyn", которое должно быть "constructor TSynPasSyn.Create(AOwner: TComponent);"
  * Нам нужно создать наши новые атрибуты, таким образом, мы добавляем наши атрибуты где-нибудь в конструкторе (я предлагаю после значения по умолчанию "fCommentAttri")


    
    
      (...)
      AddAttribute(fCommentAttri);
      {NEW}
      fCommentAttri_Ansi := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Ansi', SYNS_XML_AttrComment+'_Ansi'); // Последние две строки - это заголовок и сохраненное имя
      //Если вы хотите иметь настройки по умолчанию для вашего атрибута, вы можете, например, добавить это:
      //fCommentAttri_Ansi.Background := clBlack; //Установит "Background" в "clBlack" по умолчанию
      AddAttribute(fCommentAttri_Ansi);
      fCommentAttri_Bor := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Bor', SYNS_XML_AttrComment+'_Bor');
      AddAttribute(fCommentAttri_Bor);
      fCommentAttri_Slash := TSynHighlighterAttributes.Create(SYNS_AttrComment+'_Slash', SYNS_XML_AttrComment+'_Slash');
      AddAttribute(fCommentAttri_Slash);
      {/NEW}
      (...)
    

  * «Сложная» часть теперь состоит в том, чтобы найти места в коде, где "FTokenID" установлен в "tkComment", и установить наш «подтип» одинаково (конечно, я их уже поискал :)


    
    
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
    

  * Теперь нам просто нужно извлечь информацию при вызове "GetTokenAttribute" и вернуть правильный атрибут, поэтому мы отредактируем "GetTokenAttribute" следующим образом:


    
    
    function TSynPasSyn.GetTokenAttribute: TSynHighlighterAttributes;
    begin
      case GetTokenID of
        tkAsm: Result := fAsmAttri;
        {OLD
        tkComment: Result := fCommentAttri; //Это закомментировано и на всякий пожарный сохранено, поэтому оно будет игнорироваться
        /OLD}
        {NEW}
        tkComment: begin
          if (FCommentID=tckAnsi) then Result:=fCommentAttri_Ansi //Тип - AnsiComment
          else
          if (FCommentID=tckBor) then Result:=fCommentAttri_Bor //Тип - BorComment
          else
          if (FCommentID=tckSlash) then Result:=fCommentAttri_Slash //Тип - SlashComment
          else
            Result:=fCommentAttri //Если наш код каким-то образом упал, возврат к умолчанию
        end;
        {/NEW}
        tkIDEDirective: begin
    (...)
    

Если вы используете lazarus, просто переустановите SynEdit-Package, если нет, перекомпилируйте ваш проект/пакет/<аналог>. 

**ГОТОВО! Нет, серьезно, теперь вы готовы отличать различные типы комментариев.**

Lazarus-IDE автоматически определяет, какие атрибуты существуют, и показывает их в опциях, например сохраняет их, если вы их изменяете. Если ваше приложение/IDE не делает этого, вам придется установить Color/Font/ и т.д. новых атрибутов где-то вручную (например, в конструкторе TSynPasSyn) 

### Плагины для автозавершения кода

Для SynEdit есть 3 подключаемых плагина автозавершения кода: 

[TSynCompletion](</index.php?title=TSynCompletion&action=edit&redlink=1> "TSynCompletion \(page does not exist\)")

  * Предлагает список слов в раскрывающемся списке с помощью сочетания клавиш (по умолчанию: `Ctrl`+`space`).
  * Используется в IDE для автозавершения идентификатора.
  * Включен в примеры.
  * Доступен в палитре компонентов (начиная с 0.9.3x).



Пример кода, чтобы вызвать автозавершение, всплывает программно (т.е. без нажатия сочетания клавиш): 
    
    
    YourSynEdit.CommandProcessor(YourSynCompletion.ExecCommandID, '', nil)
    

[TSynAutoComplete](</index.php?title=TSynAutoComplete&action=edit&redlink=1> "TSynAutoComplete \(page does not exist\)")

  * Заменяет текущий токен фрагментом текста. **Не** интерактивный. **Не** выпадающий.
  * Включен в примеры.
  * Доступен в палитре компонентов.



[TSynEditAutoComplete](</index.php?title=TSynEditAutoComplete&action=edit&redlink=1> "TSynEditAutoComplete \(page does not exist\)")

  * Основной шаблон модуля. **Не** выпадающий.
  * Используется IDE для шаблонов кода. IDE содержит дополнительный код, расширяющий эту функцию (IDE добавляет макросы выпадающего списка и синхронизации).
  * **Не** включен в примеры.



Todo: Различия между 2-м и 3-м должны быть задокументированы. Может быть, они могут быть объединены. 

### Логическая/Физическая позиция каретки

SynEdit предлагает положение каретки (текстовый мигающий курсор) в 2 различных формах: 

  * Физический X/Y: соответствует визуальной (холст) позиции
  * Логический X/Y: соответствует байтовому смещению текста



Оба основаны на 1 (отсчет начинается с 1). В настоящее время координаты Y всегда одинаковы. Это может измениться в будущем. 

Физическая координата
    это позиция в сетке дисплея (без учета прокрутки). То есть:
    обе буквы "a" и "â" занимают ОДНУ ячейку в сетке, увеличивая физический x на 1. Несмотря на то, что в кодировке utf8 "a" занимает один байт, а "â" занимает несколько байтов.
    однако символ табуляции (#9), помимо одного байта и одного символа, может занимать несколько ячеек в сетке, увеличивая физический x более, чем на одну позицию. Есть также некоторые символы в китайском и восточном языках, которые занимают 2 позиции в сетке (гуглите отличие full-width(полной ширины) от half-width (полуширины) символа).

Логическая координата
    смещение байта в строчке, содержащей строку.
    буква «а» имеет 1 байт и увеличивается на 1
    буква "â" имеет 2 (или 3) байта и увеличивается на эту величину
    символ tab имеет 1 байт и увеличивается на эту величину.

Ни один из 2 не дает позицию в символах/кодовых точках UTF8 (например, для Utf8Copy или Utf8Length). 

Физический X всегда отсчитывается слева от текста, даже если он прокручивается. Чтобы получить grid-x текущего прокручиваемого элемента управления, выполните: 

    grid-X-in-visible-part-of-synedit := PhysicalX - SynEdit.LeftChar + 1
    grid-y-in-visible-part-of-synedit := SynEdit.RowToScreenRow(PhysicalY); // включает в себя складывание (схлопывание) текста
    используйте ScreenRowToRow для обратного процесса

### Изменение текста из кода

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Изменение текста с помощью свойства _SynEdit.Lines_ _не работает с undo/redo._

Доступ к тексту можно получить через SynEdit.Lines. Это свойство на основе TStrings, предлагающее доступ на чтение/запись к каждой строке. Основано на нумерации строк с 0. 
    
    
      SynEdit.Lines[0] := 'Text'; // первая строчка
    

SynEdit.Lines можно использовать для установки начальной версии текста (например, загруженного из файла). Обратите внимание, что методы SynEdit.Lines.Add/SynEdit.Lines.Append не поддерживают разрывы строк внутри добавленных строк. Вы должны добавлять строки одну за другой. 

Чтобы изменить содержимое SynEdit и позволить пользователю отменить действие, используйте следующие методы: 
    
    
        procedure InsertTextAtCaret(aText: String; aCaretMode: TSynCaretAdjustMode = scamEnd);
        property TextBetweenPoints[aStartPoint, aEndPoint: TPoint]: String // Логические точки
          read GetTextBetweenPoints write SetTextBetweenPointsSimple;
        property TextBetweenPointsEx[aStartPoint, aEndPoint: TPoint; CaretMode: TSynCaretAdjustMode]: String
          write SetTextBetweenPointsEx;
        procedure SetTextBetweenPoints(aStartPoint, aEndPoint: TPoint;
                                       const AValue: String;
                                       aFlags: TSynEditTextFlags = [];
                                       aCaretMode: TSynCaretAdjustMode = scamIgnore;
                                       aMarksMode: TSynMarksAdjustMode = smaMoveUp;
                                       aSelectionMode: TSynSelectionMode = smNormal );
    

Примеры: 
    
    
      // Вставляем текст в позицию каретки
      SynEdit.InsertTextAtCaret('Text');
      // Заменяем текст с (x=2,y=10) на (x=4,y=20) с помощью Str
      SynEdit.TextBetweenPoints[Point(2,10), Point(4,20)] := Str;
    
    
    
      // Удаляем/заменяем один символ в позиции каретки
      var p1, p2: TPoint;
      begin
        p1 := SynEdit.LogicalCaretXY;
        p2 := p1;
        // Высчитываем позицию байта следующего символа
        p2.x := p2.x + UTF8CharacterLength(@SynEdit.LineText[p2.x]);
        // p1 указывает на первый байт заменяемого символа
        // p2 указывает на первый байт символа после последнего заменяемого символа
        // Заменяем на "Text" (или используем пустую строку для удаления)
        SynEdit.TextBetweenPoints[p1, p2] := 'Text';
    

### Сворачивание/Разворачивание текста из кода

  * Это все еще находится в стадии разработки.
  * Это работает, только если текущий маркер подсветки поддерживает сворачивание (подробности в [SynEdit_Highlighter](<../en/SynEdit_Highlighter.md> "SynEdit Highlighter")).
  * Также обратите внимание, что некоторые маркеры подсветки поддерживают несколько независимых сворачиваемых деревьев. Например, в Паскале у вас есть сворачивание по ключевым словам (`begin`, `end`, `class`, `procedure` и т.д.), которое является основным, и сворачивание по `$ifdef` или `$region`, которое является дополнительным.
  * Сворачивание текущего выделения также отличается от сворачивания по ключевым словам.



Методы сворачивания: 

1) TSynEdit.CodeFoldAction 

Сворачивание в данной строчке. Если их больше одной, сворачивается самая внутренняя (самая правая). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Это не работает ни с выделением, ни с блоком сворачиваемого кода, которые уже полностью скрыт/требуется тестирование для дополнительных блоков сворачиваемого кода.

2) TSynEdit.FindNextUnfoldedLine 

3) TSynEdit.FoldAll / TSynEdit.UnfoldAll 

### Закладки

  * [Топик форума о добавлении меток и цветных линий](<http://forum.lazarus.freepascal.org/index.php/topic,14948.msg79794.html>)



### Дополнительная информация

Обсуждения на форуме, которые содержат информацию о SynEdit: 

  * [Search/replace; caret; position logical/physical](<http://forum.lazarus.freepascal.org/index.php/topic,19520.msg111158.html#msg111158>)
  * [Diff between SynEdit/SynMemo](<http://forum.lazarus.freepascal.org/index.php/topic,19645.msg111962.html#msg111962>)
  * [Mouse actions; disable paste on middle click](<http://forum.lazarus.freepascal.org/index.php/topic,23997.msg144100.html#msg144100>)
  * [TSynEditMarkup info](<http://forum.lazarus.freepascal.org/index.php?topic=24842>)



### Примеры приложений

Примеры приложений можно найти в папке "lazarus/examples/synedit". 

### Добавление горячих клавиш для Cut/Copy/Paste/и т.д.

Горячие клавиши могут быть реализованы с помощью команд SynEdit. 
    
    
    uses
      SynEdit, SynEditKeyCmds;
    
    procedure TForm1.SynEdit1KeyDown(Sender: TObject; var Key: Word;
      Shift: TShiftState);
    begin
      if (Shift = [ssCtrl]) then
      begin
        case Key of
        VK_C: SynEdit1.CommandProcessor(TSynEditorCommand(ecCopy), ' ', nil);
        VK_V: SynEdit1.CommandProcessor(TSynEditorCommand(ecPaste), ' ', nil);
        VK_X: SynEdit1.CommandProcessor(TSynEditorCommand(ecCut), ' ', nil);
        end;
      end;
    end;
    

## Дальнейшее развитие, обсуждение

  * RTL (справа налево): начат Mazen'ом (частично реализован в Windows)
  * SynEdit использует только UTF8; версия ASCII/ANSI больше не существует. Шрифт предварительно выбирается в зависимости от системы. Пользователь может выбрать другой шрифт, но затем должен позаботиться о выборе моноширинного шрифта. 
    * автоматический выбор моноширинного шрифта: в данный момент SynEdit запускается со использованием шрифта 'courier'. На данный момент TFont LCL не предоставляет свойство для фильтрации моноширинных шрифтов.
    * автоматический выбор шрифта UTF-8: по аналогии с выше описанным моноширинным шрифтом + также со шрифтом UTF-8, так что, например, умлауты отображаются правильно.
  * "Немые" клавиши. Большинство клавиатур поддерживают ввод с использованием двух или более клавиш для создания одного специального символа (например, символы ударения или умлаута). (Это обрабатывается LCL widgedset)
  * [Редизайн компонента SynEdit](<../en/Redesign_of_the_SynEdit_component.md> "Redesign of the SynEdit component"). Основная задача - более надежное отображение и навигация по тексту. Более модульный подход также позволяет лучше интегрировать расширения и специализированные элементы управления для использования вне Lazarus.
  * [Перенос слов](<http://bugs.freepascal.org/view.php?id=30395>). Это экспериментальная реализация, следующая идее классов TextTrimmer/TabExpansion. У связанной проблемы bugtraker есть класс и объяснение изменений, требуемых в других файлах для его работы.
  * Хуки в обработке клавиши/команды SynEdit. На форуме:<http://forum.lazarus-ide.org/index.php/topic,35592.msg243316.html#msg243316>



## См. также

  * [SynEdit Highlighter](<../en/SynEdit_Highlighter.md> "SynEdit Highlighter")
  * ["Изменение существующего маркера подсветки": немецкая ветка форума на "lazarusforum.de"](<http://www.lazarusforum.de/viewtopic.php?f=5&t=8723>)
  * [ATSynEdit](<../en/ATSynEdit.md> "ATSynEdit")

---

_Source: [https://wiki.freepascal.org/SynEdit/ru](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/SynEdit/ru)_
