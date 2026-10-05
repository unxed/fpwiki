# Creating IDE Help

│ **[Deutsch (de)](</Creating_IDE_Help/de> "Creating IDE Help/de")** │  **[English (en)](<../en/Creating_IDE_Help.md> "Creating IDE Help")** │  **[日本語 (ja)](</Creating_IDE_Help/ja> "Creating IDE Help/ja")** │  **русский (ru)** │ 

  


## Contents

  * 1 Обзор
  * 2 Демки и исследование
    * 2.1 Помощь для модулей LCL
    * 2.2 Помощь для окон IDE
      * 2.2.1 Пример
      * 2.2.2 Переадресация
    * 2.3 Справка по ключевым словам FPC
    * 2.4 Справка для сообщений
    * 2.5 Добавление справки Kylix



# Обзор

Среда IDE может выполнять поиск справки по различным вещам, и существуют различные источники в разных форматах справки. Вот краткий список, чтобы понять сложность: 

  * **FPC messages** : сообщение компилятора типа _Error: Illegal enum minimum-size specifier_. В IDE по умолчанию встроен поиск файла _FPCSrcDir/compiler/msg/errore.msg file_ , поиск сообщения там и отображение соответствующего комментария. Комментарии содержат латексные теги, которые удалены. На данный момент отображается простой MessageDlg. Некоторые печально известные сообщения нуждаются в дополнительных пояснениях и вручную связаны с URL-адресами.
  * **IDE windows, dialogs and controls** :
  * **Pascal sources** :
  * **FPC keywords** :
  * **FPC directives** :
  * **IDE directives** :



# Демки и исследование

Вы можете увидеть текущую установленную справку в разделе **View / IDE Internals / About IDE / Help**. 

В разделе **examples/idehelp/demoidehelp.lpk** находится тестовый и демонстрационный пакет. Он содержит пример кода, как вы можете добавить справку в IDE. 

Чтобы показать свою помощь в браузере пользователя, см. examples/helphtml/htmlhelp1.lpi. 

Чтобы просмотреть справку HTML в элементе управления LCL, см. examples/htmlhelp_ipro/htmlhelpsample.lpi. 

## Помощь для модулей LCL

Всякий раз, когда пользователь нажимает клавишу F1 для свойства Object Inspector или для идентификатора в редакторе исходного кода, среда IDE выполняет поиск объявления и вызывает средство просмотра HTML для элемента fpdoc. Элементы можно легко редактировать с помощью инструмента [FPDoc Editor](<FPDoc_Editor.md> "FPDoc Editor/ru"). 

Вы можете увидеть состояние документации LCL здесь: [LCL Documentation Roadmap](<../en/LCL_Documentation_Roadmap.md> "LCL Documentation Roadmap"). 

## Помощь для окон IDE

Эта функция существует с 0.9.15. 

Справка для окон IDE (т.е. 'Object Inspector' или 'Compiler Options') описана в этой вики. В среде IDE есть файл сопоставления _docs/IDEWindowHelpTree.xml_ , в котором содержатся пути от различных форм/элементов управления IDE до вики-страниц. Этот файл сопоставления редактируется с помощью редактора, который отображается в любом месте среды IDE с помощью `Ctrl`+`⇧ Shift`+`F1`, за исключением элементов управления, которые перехватывают все ключи, [такие] как synedit. Вы можете установить быструю клавишу в editor options -> key mapping. 

Корневой страницей документации окна IDE является [Lazarus IDE](<../en/Lazarus_IDE.md> "Lazarus IDE"). 

### Пример

  * Откройте настройки проекта: Project / Project Options
  * Откройте редактор справки: `⇧ Shift`+`Ctrl`+`F1`



[![HelpEditor1.png](https://wiki.freepascal.org/images/d/d3/HelpEditor1.png)](</File:HelpEditor1.png>)

  * Слева вы можете увидеть все элементы управления диалогового окна параметров проекта. Вы можете видеть, что там есть также опции компилятора.
  * Выберите **ProjectApplicationOptionsFrame**
  * Нажмите **Create Help node** (Создать узел справки). Это создаст новый пункт справа.
  * Отметьте **Has Help** (Имеет справку)
  * Отметьте **Is a root control** (Корневой элемент управления). Все элементы управления на фрейме должны использовать справку на нашей новой странице справки.
  * Смените путь на **Project_Options**



[![HelpEditorProjectOptions.png](https://wiki.freepascal.org/images/1/11/HelpEditorProjectOptions.png)](</File:HelpEditorProjectOptions.png>)

  * Вы можете нажать кнопку **Test** , и в вашем браузере должно появиться [IDE_Window:_Project_Options](<IDE_Window__Project_Options.md> "IDE Window: Project Options/ru").



### Переадресация

TreeView в диалоговом окне параметров IDE перенаправляет запрос в справку выбранного кадра: 

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
    

## Справка по ключевым словам FPC

Вы можете зарегистрировать справку по ключевым словам fpc. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Одна база данных может обрабатывать несколько контекстов, например справку по ключевым словам fpc, справку по исходникам и справку по сообщениям.
    
    
    uses
      IDEHelpIntfs, HelpIntfs, LazHelpIntf, LazHelpHTML...
    
      { TSimpleFPCKeywordHelpDatabase }
    
      TSimpleFPCKeywordHelpDatabase = class(THTMLHelpDatabase) // Вы можете использовать любой тип базы данных, которая вам нравится
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
        // HelpKeyword начинается с KeywordPrefix
        KeyWord:=copy(HelpKeyword,length(FPCKeyWordHelpPrefix)+1,length(HelpKeyword));
        // test: testfcpkeyword
        if KeyWord='testfcpkeyword' then begin
          // эта справочная база данных знает это ключевое слово
          // => добавляем узел, чтобы при наличии нескольких возможностей среда IDE могла
          //    показать пользователю диалог выбора
          // Примечание: в вашей собственной базе данных вы можете создать один фиктивный узел или
          // один узел на ключевое слово или один узел на группу ключевых слов
          // или как угодно.
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
      // Показываем URL ...
    end;
    
    procedure Register;
    begin
     FPCKeywordsHelpDB:=HelpDatabases.CreateHelpDatabase('NameYourHelpDB',
                                                TSimpleFPCKeywordHelpDatabase,true);
    end;
    

## Справка для сообщений

Эта функция существует с 0.9.15 и требует, чтобы исходники FPC были установлены локально. 

Она вызывается, когда пользователь нажимает клавишу F1 или использует пункт меню Help окна сообщения. 

Обычно среда IDE ищет файл errore.msg в исходниках FPC и показывает комментарий к сообщению. Но некоторые сообщения, такие как _unit not found_ («устройство не найдено»), нуждаются в дополнительной справке. Ниже описано, как это сделать: 

Чтобы добавить вики-страницу справки для сообщения, сделайте следующее: 

  * Создайте вики-страницу под [Build messages](<../en/Build_messages.md> "Build messages")
  * Щелкните правой кнопкой мыши на окне сообщений / Edit help
  * Это показывает редактор для добавления сообщений.



[![AdditionalHelpForMessages.png](https://wiki.freepascal.org/images/6/69/AdditionalHelpForMessages.png)](</File:AdditionalHelpForMessages.png>)

Пункты сохраняются в **< lazarusdir>/docs/additionalmsghelp.xml**. 

## Добавление справки Kylix

Как использовать файлы справки Borland в редакторе IDE: [Adding Kylix Help](<../en/Installing_Help_in_the_IDE.md> "Installing Help in the IDE")

---

_Source: [https://wiki.freepascal.org/Creating_IDE_Help/ru](https://web.archive.org/web/20250122100640/https://wiki.freepascal.org/Creating_IDE_Help/ru)_
