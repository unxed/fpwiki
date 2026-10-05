# XML Tutorial

│ **[English (en)](<../en/XML_Tutorial.md>)** │  **русский (ru)** │

XML - Расширяемый Язык Разметки (e**X** tensible **M** arkup **L** anguage) рекомендован [W3C](<http://www.w3.org/>) как язык для обмена информацией между различными системами. Это ориентированный на текст способ сохранения информации. Современные языки обмена данными, такие как XHTML, так же как и большинство технологий WebServices, основаны на XML. 

Это текстовый способ хранения информации, а не хранения информации в двоичном формате. 

Современные языки обмена данными, такие как XHTML, а также большинство технологий WebServices, основаны на XML. Эта вики может действительно дать только краткий обзор XML, при этом основное внимание уделяется анализу и использованию файлов XML в приложениях Free Pascal. Если вас интересует более полное объяснение XML и его использования, см. [здесь](<https://ru.wikipedia.org/wiki/XML>). 

## Contents

  * 1 Введение
  * 2 Примеры
    * 2.1 Использование: Юникод или Ansi
    * 2.2 Чтение текстового узла
    * 2.3 Вывод имен узлов и атрибутов
    * 2.4 Загрузка XML в TreeView
    * 2.5 Изменение XML документа
    * 2.6 Создание TXMLDocument из строки
    * 2.7 Проверка достоверности документа
    * 2.8 Cимволы пробелов
    * 2.9 Потоковое чтение
    * 2.10 Генерация файла XML
    * 2.11 Кодировки
  * 3 Дополнительные ссылки
  * 4 Смотрите также



## Введение

В настоящее время в Free Pascal существует ряд модулей для поддержки XML. Эти модули называются "XMLRead", "XMLWrite" и "DOM", и являются частью Free Component Library (FCL) из комплекта Free Pascal. FCL уже находится в заданном по умолчанию пути поиска файлов для компилятора в Лазарусе, таким образом Вам нужно только добавить названия модулей в строку USES чтобы получить поддержку XML в Вашей программе. Использование XML пока не документировано, поэтому данная статья даёт необходимые вводные сведения для работы с модулями поддержки XML. 

DOM XML (Объектная модель документов) - это ряд стандартизированных объектов, которые предоставляют однотипный интерфейс для использования XML в различных языках и платформах. Стандарт определяет только методы, свойства и другие части интерфейса объекта, оставляя реализацию свободной для различных языков. FCL в настоящее время поддерживает полностью [DOM 1.0](<http://ru.wikipedia.org/wiki/DOM>). 

## Примеры

В статье даны примеры работы с XML-данными по принципу нарастающей сложности. 

### Использование: Юникод или Ansi

FPC предоставляет XML модули, использующие кодировку ANSI. Поэтому, они могут различаться в зависимости от платформы, в которой выполняется код и не иметь поддержку Юникода. Lazarus, в свою очередь, предоставляет собственный набор XML модулей, расположенных в пакете LazUtils. Они полностью поддерживают Юникод в формате UTF-8 и не зависят от выполняющей код платформы. Эти модули взаимозаменяемы и их использование определяется лишь наличием в списке uses. 

Следующие модули FPC XML используют системную кодировку: 

  * DOM
  * XMLRead
  * XMLWrite
  * XMLCfg
  * XMLUtils
  * XMLStreaming



Эти модули предоставлены Lazarus XML и имеют поддержку Юникода в формате UTF-8: 

  * laz2_DOM
  * laz2_XMLRead
  * laz2_XMLWrite
  * laz2_XMLCfg
  * laz2_XMLUtils
  * laz_XMLStreaming.



Не все из них нужны в каждом примере. Однако, вам понадобится DOM, т.к. в нем содержится несколько типов, включая [TXMLDocument](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/txmldocument.html>). 

### Чтение текстового узла

Для Delphi-программистов: 

Помните, что когда Вы работаете с TXMLDocument, текст в пределах узла считается **отдельным текстовым узлом**. Таким образом, Вы должны обратиться к текстовому значению узла, как к отдельному узлу. Альтернативно, свойство **TextContent** может быть использовано для возврата значения всех узлов лежащих ниже, которые связаны с данным. 

Процедура [**ReadXMLFile**](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_xmlread/readxmlfile.html>) всегда создаёт новый **TXMLDocument** , таким образом Вы не должны создавать его заранее. Однако Вы должны вызывать метод **Free** вручную после окончания работы с документом для освобождения ресурсов занятых объектом **TXMLDocument**. 

Для примера рассмотрим следующий XML-файл: 
    
    
     <?xml version="1.0"?>
     <request>
       <request_type>PUT_FILE</request_type>
       <username>123</username>
       <password>abc</password>
     </request>
    

Следующий пример показывает корректный и некорректный способы получения значений текстового узла xml: 
    
    
     var
      PassNode: TDOMNode;
      Doc:      TXMLDocument;
     begin
      // Читаем xml файл с жесткого диска
      ReadXMLFile(Doc, 'c:\xmlfiles\test.xml');
      try
        // Запрашиваем узел с именем "password"
        PassNode := Doc.DocumentElement.FindNode('password');
        // Выводим значение выбранного узла
        //Неправильный способ
        WriteLn(PassNode.NodeValue); // вывод будет пустым
        //Правильный способ
        // Текст узла - это отдельный дочерний узел
        WriteLn(PassNode.FirstChild.NodeValue); // правильно выведет "abc"
        // Альтернативный способ
        WriteLn(PassNode.TextContent);
      finally
        // В завершении делаем Free для документа
        Doc.Free;
      end; 
    end;
    

Обратите внимание, что ReadXMLFile (...) игнорирует все начальные символы пробелов при анализе документа. Раздел [символы пробелов](<XML_Tutorial.md> "XML Tutorial/ru") описывает, как их сохранить. 

### Вывод имен узлов и атрибутов

Маленькое замечание о навигации по дереву DOM: 

Для последовательного доступа к узлам лучше всего использовать свойства [FirstChild](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnode.firstchild.html>) и [NextSibling](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnode.nextsibling.html>) (чтобы шагать вперед по дереву) или [LastChild](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnode.lastchild.html>) и [PreviousSibling](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnode.previoussibling.html>) (назад с конца дерева). Для произвольного доступа к узлам дерева можно пользоваться функциями [FindNode](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnode.findnode.html>) (ищется первый узел верхнего уровня с подходящим именем) или [GetElementsByTagName](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomdocument.getelementsbytagname.html>) (создается объект [TDOMNodeList](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnodelist.html>), который после использования должен быть освобождён). Это поведение отличается от других реализаций DOM (например, MSXML), поскольку FCL реализация основана на объектах, а не на интерфейсах. 

* * *

[ Прим.перев](</User:Zoltanleo> "User:Zoltanleo"): кроме того, существует функция [GetNamedItem](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomnamednodemap.getnameditem.html>) для поиска аттрибута по имени в текущем узле. Упрощенный пример (оригинал кода [здесь](<https://forum.lazarus.freepascal.org/index.php/topic,50104.msg365179.html#msg365179>)): 
    
    
    var 
      aNode, AttrNode: TDOMNode;
    begin
    ...
     //в текущем узле aNode пытаемся найти аттрибут цвета с именем 'color' и присвоить его узлу AttrNode
     AttrNode:= aNode.Attributes.GetNamedItem('color');
     //если такой узел создан (и не равен nil)
     if Assigned(AttrNode) then
     begin
       //проверяем значение аттибута
       if AttrNode.NodeValue <> ''
       then
         //если значение задано, то считываем его и присваиваем свойству цвета Memo1
         Memo1.Color:= StringToColor(AttrNode.NodeValue)
       else
         Memo1.Color:= clInfoBk;
       end;
    end;
    

* * *

  
Следующий пример демонстрирует, как выводить имена узлов в компонент TMemo, расположенный на форме. 

Ниже приведён XML-файл с именем 'C:\Programs\test.xml': 
    
    
     <?xml version="1.0"?>
     <images directory="mydir">
      <imageNode URL="graphic.jpg" title="">
        <Peca DestinoX="0" DestinoY="0">Pecacastelo.jpg1.swf</Peca>
        <Peca DestinoX="0" DestinoY="86">Pecacastelo.jpg2.swf</Peca>
      </imageNode>
     </images>
    

И код на Pascal, который выполняет эту задачу: 
    
    
    var
      Doc: TXMLDocument;
      Child: TDOMNode;
      j: Integer;
    begin
      try
        ReadXMLFile(Doc, 'test.xml');
        Memo.Lines.Clear;
        // Используем свойства FirstChild и NextSibling
        Child := Doc.DocumentElement.FirstChild;
        while Assigned(Child) do
        begin
          Memo.Lines.Add(Child.NodeName + ' ' + Child.Attributes.Item[0].NodeValue);
          // Используем свойство ChildNodes
          with Child.ChildNodes do
          try
            for j := 0 to (Count - 1) do
              Memo.Lines.Add(format('%s %s (%s=%s; %s=%s)',
                                    [
                                      Item[j].NodeName,
                                      Item[j].FirstChild.NodeValue,
                                      Item[j].Attributes.Item[0].NodeName,  // 1-й атрибут
                                      Item[j].Attributes.Item[0].NodeValue,
                                      Item[j].Attributes.Item[1].NodeName,  // 2-й атрибут
                                      Item[j].Attributes.Item[1].NodeValue
                                    ]));
          finally
            Free;
          end;
          Child := Child.NextSibling;
        end;
      finally
        Doc.Free;
      end;
    end;
    

В результате программа выведет следующее: 
    
    
    imageNode graphic.jpg
    Peca Pecacastelo.jpg1.swf (DestinoX=0; DestinoY=0)
    Peca Pecacastelo.jpg2.swf (DestinoX=0; DestinoY=86)
    

* * *

[Прим. перев.](</User:Zoltanleo> "User:Zoltanleo"): Один из участников форума Werner Pamler (a.k.a. [wp](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=45589>)) [предложил](<https://forum.lazarus.freepascal.org/index.php/topic,39432.msg270619.html#msg270619>) рекурсивную функцию поиска по всему дереву _первого подходящего_ дочернего узла по имени, начиная с указанного родительского узла: 
    
    
    function FindNode(AParent: TDOMNode; AName: String): TDOMNode;
      var
        node: TDOMNode;
      begin
        Result := AParent.FindNode(AName);
        if Result = nil then begin
          node := AParent.FirstChild;
          while node <> nil do begin
            Result := FindNode(node, AName);
            if Result <> nil then
              exit;
            node := node.NextSibling;
          end;
        end;
      end;
    

Пример использования: 
    
    
    var aDoc: TXMLDocument;
        aChildNode: TDOMNode;
    begin
      //будем считать, что aDoc уже заполнен данными
      ...
      //ищем дочерний узел с именем 'MyChildNode',
      //начиная с самого первого узла (aDoc.DocumentElement) документа aDoc
      aChildNode:= FindNode(aDoc.DocumentElement,'MyChildNode');
      if not Assigned(aNode) then
        showmessage('узла с именем "MyChildNode" не существует')
      else
        showmessage('узел с именем "MyChildNode" найден');
    end;
    

### Загрузка XML в TreeView

Одно из обычных использований файла XML - разбор и показ информации в древовидном формате. Вы можете отыскать компонент TTreeView на вкладке "Common Controls" Lazarus'а. 

Функция, приведённая ниже, возмёт документ XML, предварительно загруженный из файла или сгенерированный программно, и заполнит TreeView его содержимым. Заголовком каждого узла будет содержимое первого атрибута этого узла. 
    
    
    procedure TForm1.XML2Tree(tree: TTreeView; XMLDoc: TXMLDocument);
    var
      iNode: TDOMNode;
    
      procedure ProcessNode(Node: TDOMNode; TreeNode: TTreeNode);
      var
        cNode: TDOMNode;
        s: string;
      begin
        if Node = nil then Exit; // выходим, если достигнут конец документа
        
        // добавляем узел в дерево
        if Node.HasAttributes and (Node.Attributes.Length>0) then
          s:=Node.Attributes[0].NodeValue
        else
          s:=''; 
        TreeNode := tree.Items.AddChild(TreeNode, s);
    
        // переходим к дочернему узлу
        cNode := Node.FirstChild;
    
        // проходим по всем дочерним узлам
        while cNode <> nil do
        begin
          ProcessNode(cNode, TreeNode);
          cNode := cNode.NextSibling;
        end;
      end;
        
    begin
      iNode := XMLDoc.DocumentElement.FirstChild;
      while iNode <> nil do
      begin
        ProcessNode(iNode, nil); // Рекурсия
        iNode := iNode.NextSibling;
      end;
    end;
    

Другой пример, который отображает полную структуру XML, включая все значения атрибутов (примечание: длинная строка, ссылающаяся на TreeView, была разделена, поэтому она будет переносить слова для этой вики; при записи в коде вам не нужно разбивать строку, если вам не нравится форматирование ): 
    
    
    procedure XML2Tree(XMLDoc:TXMLDocument; TreeView:TTreeView);
    
      //Локальная функция, которая выводит все атрибуты узла в виде строки
      function GetNodeAttributesAsString(pNode: TDOMNode):string;
      var i: integer;
      begin
        Result:='';
        if pNode.HasAttributes then
          for i := 0 to pNode.Attributes.Length -1 do
            with pNode.Attributes[i] do
              Result := Result + format(' %s="%s"', [NodeName, NodeValue]);
    
        //Удаляем начальные и конечные пробелы
        Result:=Trim(Result);
      end;
    
      // Рекурсивная функция для обработки узла и всех его дочерних узлов 
    
      procedure ParseXML(Node:TDOMNode; TreeNode: TTreeNode);
      begin
        //Выход из процедуры, если больше нет узлов для обработки
        if Node = nil then Exit;
    
        //Добавляем узел в TreeView
        TreeNode := TreeView.Items.AddChild(TreeNode, 
                                              Trim(Node.NodeName+' '+ 
                                               GetNodeAttributesAsString(Node)+ 
                                               Node.NodeValue)
                                            );
    
        //Обрабатываем все дочерние узлы
        Node := Node.FirstChild;
        while Node <> Nil do
        begin
          ParseXML(Node, TreeNode);
          Node := Node.NextSibling;
        end;
      end;
    
    begin
      TreeView.Items.Clear;
      ParseXML(XMLDoc.DocumentElement,nil);
    end;
    

### Изменение XML документа

Первая вещь, о которой следует помнить, TDOMDocument это хэндл (handle) к DOM. Вы можете получить экземпляр этого класса создавая или загружая XML документ. 

С другой стороны, узлы не могут быть созданы как обычные объекты. Вы должны использовать методы, которые предоставляет TDOMDocument для их создания и, впоследствии, использовать другие методы для помещения их в нужное место дерева. Это говорит о том, что узлы должны принадлежать вполне определённому документу DOM. 

Вот некоторые общераспространённые методы TDOMDocument: 
    
    
       function CreateElement(const tagName: DOMString): TDOMElement; virtual;
       function CreateTextNode(const data: DOMString): TDOMText;
       function CreateCDATASection(const data: DOMString): TDOMCDATASection; virtual;
       function CreateAttribute(const name: DOMString): TDOMAttr; virtual;
    

`[CreateElement](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomdocument.createelement.html>)` создает новый элемент. 

`[CreateTextNode](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomdocument.createtextnode.html>)` создает текстовый узел. 

`[CreateAttribute](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomdocument.createattribute.html>)` создает атрибут узла. 

`[CreateCDATASection](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_dom/tdomdocument.createcdatasection.html>)` создает раздел CDATA: обычные символы разметки XML, такие как <>, не интерпретируются в разделе CDATA. См. [статью по CDATA на Wikipedia](<https://ru.wikipedia.org/wiki/>)

Более удобный метод для управления атрибутами - использовать метод `TDOMElement.SetAttribute`, который также представлен как свойство по умолчанию `TDOMElement`: 
    
    
    // эти две конструкции эквивалентны
    Element.SetAttribute('name', 'value');
    Element['name'] := 'value';
    

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): вообще-то, автор оригинального текста немного ошибается, потому что выше приведенный код у вас просто не скомпилируется. Правильнее он выглядит так: 
    
    
    var Element: TDOMNode;
    begin
    ...
    // эти две конструкции эквивалентны
    TDOMElement(Element).SetAttribute('name', 'value');
    TDOMElement(Element).AttribStrings['name']:= 'value';
    ...
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Имена аттрибутов узла не могут начинаться с цифры! Если имена аттрибутов у узла одинаковые, то записывается значение последнего указанного аттрибута
    
    
    var Element: TDOMNode;
    begin
    ...
    TDOMElement(Element).SetAttribute('name', 'value 1');
    TDOMElement(Element).SetAttribute('name', 'value 2');
    //в файле мы увидим лишь <Element name="value 2">
    ...
    end;
    

* * *

  
Вот, в качестве примера, метод, который ищет выбранный элемент в TTreeView и затем вставляет дочерний узел в документ XML, где он должен находиться . TreeView должен быть предварительно заполнен содержанием из XML файла, используя [XML2Tree function](<XML_Tutorial.md> "XML Tutorial/ru"). 
    
    
    procedure TForm1.actAddChildNode(Sender: TObject);
    var
      position: Integer;
      NewNode: TDomNode;
    begin
      {*******************************************************************
      *  Определение выбраного элемента
      *******************************************************************}
      if TreeView1.Selected = nil then Exit;
    
      if TreeView1.Selected.Level = 0 then
      begin
        position := TreeView1.Selected.Index;
    
        NewNode := XMLDoc.CreateElement('item');
        TDOMElement(NewNode).SetAttribute('nome', 'Item');
        TDOMElement(NewNode).SetAttribute('file', 'File');
        with XMLDoc.DocumentElement.ChildNodes do
        begin
          Item[position].AppendChild(NewNode);
          Free;
        end;
    
        {*******************************************************************
        *  Обновление TreeView
        *******************************************************************}
        TreeView1.Items.Clear;
        XML2Tree(TreeView1, XMLDoc);
      end
      else if TreeView1.Selected.Level >= 1 then
      begin
        {*******************************************************************
        *  Эта функция работает только на верхнем уровне дерева
        *  и вы должны её модифицировать, чтобы использовать другие уровни
        *******************************************************************}
      end;
    end;
    

### Создание TXMLDocument из строки

Если данные XML находятся в строке MyXmlString, создать из нее DOM можно с помощью следующего кода: 
    
    
    var
      S: TStringStream;
      XML: TXMLDocument;
    begin
      S := TStringStream.Create('');
      try
        //Читаем XML документ полностью
        ReadXMLFile(XML, S);             
        //Альтернативно: чтение только фрагмента XML
        ReadXMLFragment(AParentNode, S); 
      finally
        S.Free;
      end;
    end;
    

### Проверка достоверности документа

Начиная с марта 2007, в FCL XML парсер добавлена возможность проверки достоверности (валидации) документа с помощью DTD. Проверка достоверности определяет соответствие логической структуры документа определенным правилам, заданных с помощью "Определение Типа документа" (DTD). 

Это пример XML-документа, содержащего DTD: 
    
    
      <?xml version='1.0'?>
      <!DOCTYPE root [
      <!ELEMENT root (child)+ >
      <!ELEMENT child (#PCDATA)>
      ]>
      <root>
        <child>This is a first child.</child>
        <child>And this is the second one.</child>
      </root>
    

Этот DTD определяет, что элемент `root` должен содержать один или более элемент `child`, а элементы `child` могут содержать только символьные данные. Если парсер обнаружит нарушение этих правил, то он сообщит о них. 

Загрузка такого документа несколько сложнее. Пусть исходные XML данные содержатся в потоке AStream: 
    
    
    procedure TMyObject.DOMFromStream(AStream: TStream);
    var
      Parser: TDOMParser;
      Src: TXMLInputSource;
      TheDoc: TXMLDocument;
    begin
      try
        // Создаём объект-парсер
        Parser := TDOMParser.Create;
        // и источник данных
        Src := TXMLInputSource.Create(AStream);
        // Включаем проверку достоверности
        Parser.Options.Validate := True;
        // Назначаем обработчик ошибок, который будет получать уведомления о них
        Parser.OnError := @ErrorHandler;
        // А теперь работаем
        Parser.Parse(Src, TheDoc);
      finally
        // ... и убираем за собой :)
        Src.Free;
        Parser.Free;
      end;  
    end;
    
    procedure TMyObject.ErrorHandler(E: EXMLReadError);
    begin
      if E.Severity = esError then  // Нас интересуют только ошибки проверки достоверности
        writeln(E.Message);
    end;
    

### Cимволы пробелов

Если необходимо сохранять пробельные символы в тексте узлов, следует пользоваться вышеприведённым методом загрузки XML документа. По умолчанию начальные пробельные символы игнорируются, именно поэтому функция ReadXML(...) пропускает все пробельные символы в начале текста узлов. Перед вызовом _Parser.Parse(Src, TheDoc)_ добавьте следующую строку: 
    
    
    Parser.Options.PreserveWhitespace := True;
    

После этого парсер будет оставлять все пробельные символы, включая все переводы строк, добавленные для повышения читаемости XML документа! 

Используйте следующий код, чтобы у вас была процедура ReadXMLFile, которая сохраняет первые пробелы: 
    
    
    procedure ReadXMLFilePreserveWhitespace(out Doc: TXMLDocument; FileName: string);
    var
      Parser: TDOMParser;
      Src: TXMLInputSource;
      InFile: TFileStream;
    begin
      try
        InFile := TFileStream.Create(FileName, fmOpenRead);
        Src := TXMLInputSource.Create(InFile);
        Parser := TDOMParser.Create;
        Parser.Options.PreserveWhitespace := True;
        Parser.Parse(Src, Doc);
      finally
        Src.Free;
        Parser.Free;
        InFile.Free;
      end;
    end;
    

### Потоковое чтение

Обработка XML с использованием DOM, требует загрузки всего документа в память. Это может быть не желательно или невозможно, если документ огромен. FCL предоставляет функции для чтения одного узла данных XML за один раз, используя класс TXMLReader и его потомков. Класс TXMLReader похож на класс XmlReader платформы .NET. Ниже приведен простой пример использования TXmlReader: 
    
    
    uses
      Classes,xmlreader,xmltextreader,xmlutils;
    
    procedure readme(AStream: TStream);
    var
      xtr: TXmlReader;
      settings: TXMLReaderSettings;
      inp: TXMLInputSource;
    begin
      settings := TXMLReaderSettings.Create;
      try
        settings.PreserveWhiteSpace := True;
        settings.Namespaces := True;
        inp := TXMLInputSource.Create(AStream);
        try
          xtr := TXmlTextReader.Create(inp,settings);
          try
            // Здесь начинается чтение
            while xtr.Read do
            begin
              write(xtr.NodeType:25);
              if xtr.name<>'' then
                write(xtr.Name:9)
              else
                write('*no name* ');
              write(xtr.Value);
              writeln;
              if xtr.NodeType=ntElement then
              begin
                // вывод атрибутов
                if xtr.MoveToFirstAttribute then
                begin
                  repeat
                    writeln('---',xtr.NodeType:21,xtr.Name:10,xtr.Value:10);
                  until not xtr.MoveToNextAttribute;
                  xtr.MoveToContent;
                end;  
              end;
            end;
          // Очистка  
          finally
            xtr.Free;
          end;
        finally
          inp.Free;
        end;
      finally
        settings.Free;
      end;
    end;
    

### Генерация файла XML

Ниже приведён код для записи XML в файле. (Взято из обучающей программы в блоге DeveLazarus). Пожалуйста помните, что модули DOM и XMLWrite должны быть включены в Uses ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo") в примерах ниже для сохранениея XML-документа в файл используется перегруженная(overload) процедура [WriteXMLFile](<https://lazarus-ccr.sourceforge.io/docs/lazutils/laz2_xmlwrite/writexmlfile.html>)) 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, StdCtrls,
      DOM, XMLWrite;
    
    type
      { TForm1 }
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        Label2: TLabel;
        procedure Button1Click(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
      
    var
      Form1: TForm1;
      
    implementation
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      XDoc: TXMLDocument;                                  // переменная документа
      RootNode, ParentNode, ChildNode: TDOMNode;           // переменные узлов документа
    begin
      //Создаём документ
      XDoc := TXMLDocument.create;
    
      //Создаём корневой узел
      RootNode := XDoc.CreateElement('register');
      XDoc.Appendchild(RootNode);                          // Добавляем корневой узел в документ
    
      RootNode:= XDoc.DocumentElement;                     //назначаем Root-узлом текущий узел
      ParentNode:= XDoc.CreateElement('user');             //создаём родительский узел
      TDOMElement(ParentNode).SetAttribute('id', '001');   // создаём атрибуты родительского узла
      RootNode.Appendchild(ParentNode);                    // добавляем родительский узел <user id="001"></user> внутри корневого <register></register>
    
      ParentNode:= XDoc.CreateElement('name');             //создаём дочерний по отношению к <user id="001"></user> узел
      //TDOMElement(ParentNode).SetAttribute('sex', 'M');   
      ChildNode:= XDoc.CreateTextNode('Fernando');         // вставляем значение в узел
      ParentNode.Appendchild(ChildNode);                   // сохраняем дочерний узел внутри родителя
      RootNode.ChildNodes.Item[0].AppendChild(ParentNode); // вставляем дочерний узел <name>Fernando</name> в соответствующий родительский узел 
                                                           // <user id="001"></user> (он будет в корневом узле RootNode первым - отсчет с нуля - RootNode.ChildNodes.Item[0])
    
      //Создаём ещё один дочерний узел (аналогично выше описанному)
      ParentNode := XDoc.CreateElement('age');               
      //TDOMElement(parentNode).SetAttribute('year', '1976');   
      ChildNode:= XDoc.CreateTextNode('32');                // вставляем значение в узел
      ParentNode.Appendchild(ChildNode);                    // сохраняем узел
      RootNode.ChildNodes.Item[0].AppendChild(ParentNode);  // вставляем дочерний узел в соответствующий родительский
    
      //создаем строку вида <!--это коммент--> внутри тега <user id="001"></user>
      ChildNode:= ADoc.CreateComment('это коммент');
      RootNode.ChildNodes.Item[0].AppendChild(ChildNode);
    
      //создаем строку вида <![CDATA[это CDATASection]]> внутри тега <user id="001"></user>
      ChildNode:= ADoc.CreateCDATASection('это CDATASection');
      RootNode.ChildNodes.Item[0].AppendChild(ChildNode);   
    
      writeXMLFile(XDoc,'test.xml');                          // записываем всё в XML-файл
      XDoc.free;                                               // освобождаем память
    end;
    
    initialization
      {$I unit1.lrs}
    
    end.
    

Вот такой XML-файл у нас должен получится в результате: 
    
    
    <?xml version="1.0"?>
    <register>
      <user id="001">
        <name>Fernando</name>
        <age>32</age>
        <!--это коммент-->
        <![CDATA[это CDATASection]]>
      </user>
    </register>
    

* * *

[Прим.переводчика](</User:Zoltanleo> "User:Zoltanleo"): обратите внимание, что функция `function CreateElement(const tagName: DOMString): TDOMElement; virtual;` допускает в качестве агрумента строки, содержащие **только цифры и буквы латиницы без пробелов** , т.е. в выше приведенном примере попытка написать: 
    
    
    ...
    RootNode:= xdoc.CreateElement('my register');
    //или RootNode:= xdoc.CreateElement('регданные');
    //или RootNode:= xdoc.CreateElement('re.gister');
    ...
    

немедленно вызовет исключение "EDOMError in DOMDocument.CreateElement" с кодом ошибки INVALID_CHARACTER_ERR. 

То же самое правило справедливо и для имен аттрибута/ов узла - см. мое замечание [выше](<XML_Tutorial.md> "XML Tutorial/ru"). 

* * *

Пример, где вам не нужно ссылаться на элемент по индексу. 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    var
      Doc: TXMLDocument;
      RootNode, ElementNode,ItemNode,TextNode: TDOMNode;
      i: integer;
    begin
      try
        // создаем документ
        Doc := TXMLDocument.Create;
        // создаем корневой узел
        RootNode := Doc.CreateElement('Root');
        Doc.Appendchild(RootNode);
        RootNode:= Doc.DocumentElement;
        // создаем узлы
        for i := 1 to 20 do
        begin
          ElementNode:=Doc.CreateElement('Element');
          TDOMElement(ElementNode).SetAttribute('id', IntToStr(i));
    
          ItemNode:=Doc.CreateElement('Item1');
          TDOMElement(ItemNode).SetAttribute('Attr1', IntToStr(i));
          TDOMElement(ItemNode).SetAttribute('Attr2', IntToStr(i));
          TextNode:=Doc.CreateTextNode('Item1Value is '+IntToStr(i));
          ItemNode.AppendChild(TextNode);
          ElementNode.AppendChild(ItemNode);
    
          ItemNode:=Doc.CreateElement('Item2');
          TDOMElement(ItemNode).SetAttribute('Attr1', IntToStr(i));
          TDOMElement(ItemNode).SetAttribute('Attr2', IntToStr(i));
          TextNode:=Doc.CreateTextNode('Item2Value is '+IntToStr(i));
          ItemNode.AppendChild(TextNode);
          ElementNode.AppendChild(ItemNode);
    
          RootNode.AppendChild(ElementNode);
        end;
        // сохраняем XML
        WriteXMLFile(Doc,'TestXML_v2.xml');
      finally
        Doc.Free;
      end;
    

Сгенерированный XML: 
    
    
    <?xml version="1.0"?>
    <Root>
      <Element id="1">
        <Item1 Attr1="1" Attr2="1">Item1Value is 1</Item1>
        <Item2 Attr1="1" Attr2="1">Item2Value is 1</Item2>
      </Element>
      <Element id="2">
        <Item1 Attr1="2" Attr2="2">Item1Value is 2</Item1>
        <Item2 Attr1="2" Attr2="2">Item2Value is 2</Item2>
      </Element>
      <Element id="3">
        <Item1 Attr1="3" Attr2="3">Item1Value is 3</Item1>
        <Item2 Attr1="3" Attr2="3">Item2Value is 3</Item2>
      </Element>
    </Root>
    

### Кодировки

Начинаясь с ревизии SVN 12582, XMLReader может читать данные в любой кодировке при условии использования внешнего декодера. Смотрите [XML_Decoders/ru](<XML_Decoders.md> "XML Decoders/ru") для более детальной информации. 

Согласно спецификации XML, атрибут encoding в первой строке XML является необязательным в случае, если фактическая кодировка - UTF-8 или UTF-16 (что определяется наличием BOM). В версии Lazarus 0.9.26 TXMLDocument имеет свойство Encoding, но оно игнорируется. При этом процедура WriteXMLFile всегда использует кодировку UTF-8 и не генерирует атрибут encoding в первой строке файла XML. 

## Дополнительные ссылки

  * [W3Schools](<http://www.w3schools.com/xml/default.asp>) Руководство по Xml


  * [Thomas Zastrow article](<http://freepascal.ru/article//freepascal/20080321115724/>) Работа с XML-файлами


  * [TXMLPropStorage](<TXMLPropStorage.md>) использование TXMLPropStorage для сохранения настроек программы.



## Смотрите также

  * [XML Документы](<XML_Decoders.md> "XML Decoders/ru")
  * [Использование INI файлов](<Using_INI_Files.md> "Using INI Files/ru")
  * [fcl-xml](<../en/fcl-xml.md> "fcl-xml")
  * [Интернет-Инструменты](<../en/Internet_Tools.md> "Internet Tools"), для обработки XPath 2 / XQuery
  * "Web data formats in Lazarus/FPC" - статья Michaël Van Canneyt ([оригинал](<https://www.freepascal.org/~michael/articles/webdata/webdata.pdf>), [русский перевод](<https://sites.google.com/view/leybasoft-arm/%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D0%BE%D0%B4%D1%8B/%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82%D1%8B-%D0%B2%D0%B5%D0%B1-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85-%D0%B2-lazarusfpc>))

---

_Source: [https://wiki.freepascal.org/XML_Tutorial/ru](https://web.archive.org/web/20250117073135/https://wiki.freepascal.org/XML_Tutorial/ru)_
