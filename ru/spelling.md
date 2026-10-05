# spelling

│ **[English (en)](<../en/spelling.md>)** │  **русский (ru)** │

  


## Contents

  * 1 Использование hunspell с Lazarus
  * 2 О библиотеке Hunspell
  * 3 Замечание насчет словарей
  * 4 Платформозависимость
    * 4.1 Linux
    * 4.2 Windows
    * 4.3 Mac
  * 5 Модуль Hunspell
    * 5.1 Demo 2 в полном графическом интерфейсе
    * 5.2 Demo 3 - Lazspell - Tmemo
    * 5.3 Demo 4 - Lazspell 2-version 2 - TRichMemo
    * 5.4 Demo 5 - Простая проверка орфографии
    * 5.5 Дальнейшее чтение и ссылки



  


# Использование hunspell с Lazarus

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Эта страница актуальна по состоянию на август 2018 года, но все меняется .....

Эта страница посвящена использованию библиотеки hunspell с Lazarus. Он описывает модель, которая работает, вроде как. Вам почти наверняка понадобится внести некоторые изменения для ваших конкретных целей, но, надеюсь, эта страница послужит вам хорошим началом. 

Во-первых, на форуме есть несколько ссылок на некоторый код, который будет работать с библиотекой hunspell. Модуль hunspell.pas в значительной степени основан на этих блоках кода. У большинства нет информации о лицензии, и делается предположение, что она является «общеизвестной» и, следовательно, свободна от каких-либо ограничений. Я добавил немного, что решает проблему поиска файлов библиотеки и словаря. И установил разумный интерфейс. 

Кроме того, пользователь [[rvk](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=55059>)] с форума создал Windows 64-битную DLL, так как для пользователей Windows не было никакой альтернативы. 

# О библиотеке Hunspell

Hunspell - это активный проект с открытым исходным кодом, распространяемый по открытой лицензии Mozilla. Библиотека hunspell используется в таких продуктах, как Libra Office, Open Office и Firefox. Его можно заставить работать на Windows, Linux и Mac (и, возможно, на кучах других платформ). См. ссылки на платформоспецифичные страницы ниже. Словари Hunspell легко доступны и, возможно, уже установлены на многих машинах. Даже если вы не можете получить доступ к библиотеке другого приложения, вы можете использовать его словарь. 

Словари Hunspell поставляются в виде пары файлов *.dic и *.aff. Например, австралийский словарь состоит из en_AU.dic и en_AU.aff. Префикс 'en' обозначает его английский, а суффикс 'AU' говорит о его специфичности специально для Австралии. Как говорящий по-английски, я отмечаю, что словари en_US, кажется, всегда установлены, и я добавляю австралийские. Я не знаю, насколько распространен этот шаблон в не-англоязычных системах. 

Словари можно найти здесь <https://github.com/LibreOffice/dictionaries>, а также некоторую информацию, связанную с этим, здесь <https://wiki.documentfoundation.org/Development/Dictionaries>

# Замечание насчет словарей

В некоторых случаях для корректного отображения предлагаемых слов словари должны быть закодированы в UTF-8. Если вы заметили, что некоторые слова отображаются неправильно — например, яблоко на польском — это jabłko, а вы получаете jab�ko, — это означает, что словари pl_PL.aff и pl_PL.dic должны быть преобразованы в UTF-8. Чтобы преобразовать словари в UTF-8 в Linux Debian (это должно работать в любом Linux, конвертированные словари можно использовать в Windows - проверьте также [эту ветку форума](<http://forum.lazarus.freepascal.org/index.php/topic,44298.0.html>)) выполните следующие действия: 

1\. Запустить терминал как root 
    
    
      su
    

2\. Если он еще не установлен, установите hunspell (hunspell-pl для польского словаря — подробнее на [Debian Hunspell package](<https://packages.debian.org/search?keywords=hunspell>)) 
    
    
      apt-get install hunspell
      apt-get install hunspell-pl
    

3\. Перейдите в /usr/share/hunspell/, где хранятся словари, и создайте папку dic 
    
    
      cd /usr/share/hunspell/
      mkdir dic
    

4\. Преобразуйте, например, словарь pl_PL.aff и pl_PL.dic. Используйте [ISO-8859-2](<https://en.wikipedia.org/wiki/ISO/IEC_8859-2>) (для Восточной Европы), [ISO-8859-1](<https://en.wikipedia.org/wiki/ISO/IEC_8859-1>) (или ISO-8859-15) для Западной Европы, [ISO-8859-5](<https://ru.wikipedia.org/wiki/ISO_8859-5>) для кириллицы и т.д. 
    
    
      iconv -f ISO-8859-2 -t UTF-8 /usr/share/hunspell/pl_PL.aff | sed 's/^SET ISO8859-2$/SET UTF-8/g' > dic/pl_PL.aff
      iconv -f ISO-8859-2 -t UTF-8 /usr/share/hunspell/pl_PL.dic > dic/pl_PL.dic
    

5\. Скопируйте папку dic или словари из папки dic в свое приложение. 

# Платформозависимость

## Linux

Во многих дистрибутивах Linux по умолчанию установлен Hunspell вместе с соответствующими языковыми словарями. Если нет, то, вероятно, это просто случай использования менеджера пакетов дистрибутива. Если ничего не помогает, возьмите исходник с сайта hunspell github и создайте его самостоятельно. Пользователи Linux любят это. 

Чтобы проверить, установлена ли у вас библиотека hunspell, попробуйте эту команду 
    
    
     ldconfig -p | grep hunspell 
    

Точно так же вы можете найти некоторые словари с помощью 
    
    
     ls -l /usr/share/hunspell 
    

Если это не сработает, попробуйте 
    
    
     find /usr -name *.aff 
    

это займет немного больше времени. 

## Windows

Установка библиотеки hunspell на Windows является более серьезной проблемой. По-видимому, нет предварительно скомпилированного 'комплекта', и большинство приложений Windows, которые используют Hunspell, похоже, статически связывают его, поэтому не осталось никаких hunspell.dll для использования. Но просто, чтобы быть уверенным, попробуйте поискать `*hunspell*.dll`. На сайте Hunspell github приведен рецепт его создания, но он включает установку MSYS2 и довольно сложен. Получающаяся DLL также нуждается вдобавок в паре gcc DLL. 

К счастью, пользователь [rvk](</User:Rvk> "User:Rvk") с форума Lazarus создал нам хорошую статически (т.е. автономно) связанную DLL-библиотеку с использованием Microsoft Visual Studio Community 2015. Таким образом, вы можете использовать и распространять эту DLL-библиотеку вместе с вашей программой, подпадающей под действие публичной лицензии Mozilla. 

Вы найдете эту DLL в комплекте с файлом лицензии на <https://github.com/davidbannon/hunspell4pas>, щелкните «DLL», щелкните по файлу dll, и вы увидите кнопку «Загрузить». Не забудьте также получить файл лицензии, он должен распространяться, так или иначе, с вашим приложением. 

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): Вы также можете посмотреть исходники интерфейса Hunspell для Lazarus здесь: <https://github.com/cutec-chris/hunspell>

* * *

## Mac

На Mac'е автора, по-видимому, была установлена библиотека Hunspell при установке Sierra. Но, может быть, просто возможно, это пришло вместе с Firefox. Я хотел бы получить обратную связь .... [уже установлено на Mojave и Catalina] 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** См <https://opensource.apple.com/tarballs/hunspell/> для загрузки исходников v1.2.8.

Чтобы проверить, установлена ли у вас библиотека hunspell, попробуйте эту команду 
    
    
     find / 2>&1 | grep "\hunspell"
    

она будет выполняться некоторое время, в зависимости от того, сколько файлов в вашей системе. Скорее всего, она найдет несколько файлов, включая некоторые, в вашем каталоге XCode. Однако конечным пользователям, вероятно, не будет установлен XCode. Один особенно интересный файл для меня был 
    
    
     /usr/lib/libhunspell-1.2.dylib 
    

Версия 1.2 немного старше, чем где-либо, но работала нормально. Если вы не можете найти пригодную для использования библиотеку, я предлагаю вам установить его с помощью менеджера пакетов, такого как MacPorts, Fink или brew. Следующая проблема заключается в том, что вам понадобятся словари. Аналогичная команда 
    
    
     find / 2>&1 | grep "\.aff"
    

опять же, медленная, она ищет по всему вашему диску. Я нашел команду 
    
    
     /Applications/Firefox.app/Contents/Resources/dictionaries/en-US.aff 
    

  
И быстрый 'ls' заверил меня, что есть соответствующие файлы en-US.dic, так что все хорошо. 

Более опытный пользователь Mac может предложить лучшие стратегии поиска. Пожалуйста! 

# Модуль Hunspell

При первоначальном создании этой страницы (2017?) я допустил серьезную ошибку и использовал код, у которого не было четкой лицензии или истории. Было высказано предположение, что тем самым я нарушил лицензию Hunspell, как и все, кто использовал ее с тех пор. 

Чтобы исправить эту ситуацию, я выпустил новую версию Hunspell Wrapper, которая имеет довольно небольшую часть, только определения функций, с Hunspell (Лицензия: MPL 1.1/GPL 2.0/LGPL 2.1) и Hunspell Unit (Лицензия: Clear BSD License) в отдельных файлах. С точки зрения кодирования это почти идентично тому, что было здесь раньше. 

См. <https://github.com/davidbannon/hunspell4pas>

Я удалил оболочку hunspell, которая раньше находилась на этой странице. Тот, что на Github выше, является заменой, вам нужны hunspell.pas и hunspell.inc. Оставшаяся часть файла представляет собой базовый test/demo проект Lazarus командной строки, который заменяет Demo 1. 

## Demo 2 в полном графическом интерфейсе

Демо графического интерфейса Lazarus имеет больше смысла и была протестирована на Linux, Mac и Windows. Но это немного сложнее, [чем] скопировать и вставить. 

Для этой демо вам понадобится форма с двумя TMemo: Memo1 и MemoMsg. Кнопка ButtonSpell, Tlistbox Listbox1. Сделайте следующие обработчики событий: FormCreate для главной формы; дважды щелкните по Listbox1 и [дважды] кликните по ButtonSpell. 

Сначала вы должны создать объект hunspell и посмотреть, нашел ли он свою библиотеку, вот пример метода FormCreate () .... 
    
    
    uses hunspell;
    var
        Form1: TForm1;
        Sp: THunspell;
        DictPath : AnsiString;              
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
        SetDefaultDicPath();
        Sp := THunspell.Create();
        if Sp.ErrorMessage = '' then begin
            MemoMsg.append('Library Loaded =' + Sp.LibraryFullName);
            ButtonSpell.enabled := CheckForDict();
        end else
            MemoMsg.append(SP.ErrorMessage);
    end;
    

В этом примере мы пишем сообщение о статусе в MemoMsg, это простой способ увидеть, что происходит. ButtonSpell НЕ включен, пока не будут установлены словари. Ожидаем этого .... 

Теперь нам нужны два метода: один читает назначенный каталог и ищет возможные файлы словарей, другой управляет решениями. Если мы найдем только один словарный набор, используем его, если мы не найдем - пожалуемся. Но если мы найдем несколько, и это наиболее вероятно, мы должны спросить пользователя, какой словарь (то есть язык) он хочет использовать. 
    
    
    function TForm1.FindDictionary(const Dict : TStrings; const DPath : AnsiString) : boolean;
    var
        Info : TSearchRec;
    begin
        Dict.Clear;
        if FindFirst(AppendPathDelim(DPath) + '*.dic', faAnyFile and faDirectory, Info)=0 then begin
            repeat
                Dict.Add(Info.Name);
            until FindNext(Info) <> 0;
        end;
        FindClose(Info);
        Result := Dict.Count >= 1;
    end;
    
    function TForm1.CheckForDict() : boolean;
    begin
        Result := False;
        EditDictPath.Caption := DictPathAlt;
        if not FindDictionary(ListBox1.Items, DictPath) then
            MemoMsg.Append('ERROR - no dictionaries found in ' + DictPath);//[прим.перев.]: словари в DictPath не найдены
        if ListBox1.Items.Count = 1 then begin                   // Один [словарь] вернулся точно.
            if not Sp.SetDictionary(AppendPathDelim(DictPath) + ListBox1.Items.Strings[0]) then
                MemoMsg.Append('ERROR ' + SP.ErrorMessage)
            else
                MemoMsg.Append('Dictionary set to ' + DictPath + ListBox1.Items.Strings[0]);// [прим.перев.]: словарь (первый из списка ListBox1) установлен в DictPath
        end;
        Result := SP.GoodToGo;   // если count был точно один или FindDict не вернул ничего и ничего не изменилось
    end;
    

Ах, вы спросите, но где нам искать словари? К сожалению, у меня нет хорошего решения для этого. Вот где я нашел свой - 
    
    
    procedure TForm1.SetDefaultDicPath();
    begin
        {$ifdef LINUX}
        DictPath := '/usr/share/hunspell/';
        {$ENDIF}
        {$ifdef WINDOWS}
        DictPath := ExtractFilePath(Application.ExeName);
        //DictPath := 'C:\Program Files\LibreOffice 5\share\extensions\dict-en\';
        {$ENDIF}
        {$ifdef DARWIN}
        DictPath := '/Applications/Firefox.app/Contents/Resources/dictionaries/';
        //DictPathAlt := ExtractFilePath(Application.ExeName);
        {$endif}
    end;
    

Возможно, если другие пользователи сообщат, где они нашли пригодные для использования словари Hunspell, мы можем составить список для каждой платформы. Или просто выберите легкий путь и попросите пользователя найти несколько словарей и поместить их в каталог приложений на Windows и Mac. Ваши мысли очень приветствуются .... 

Пока, если в указанном каталоге есть ровно один словарь, все хорошо. Но что, если их несколько? Наш ListBox1 содержит их список, если пользователь дважды щелкнет один из них, он вызовет этот метод - 
    
    
    procedure TForm1.ListBox1DblClick(Sender: TObject);
    begin
        if ListBox1.ItemIndex > -1 then
            ButtonSpell.enabled := Sp.SetDictionary( AppendPathDelim(DictPath) + ListBox1.Items.Strings[ListBox1.ItemIndex]);
        if SP.ErrorMessage = '' then begin
            MemoMsg.Append('Good To Go =' + booltostr(Sp.GoodToGo, True));
            MemoMsg.Append('Dictionary set to ' + AppendPathDelim(DictPath) + ListBox1.Items.Strings[ListBox1.ItemIndex]);
        end else
            MemoMsg.append('ERROR ' + SP.ErrorMessage);
    end;
    

Предполагая, что у нас теперь есть все, что нужно, мы можем нажать кнопку ButtonSpell и вызвать это - 
    
    
    procedure TForm1.ButtonSpellClick(Sender: TObject);
    begin
        if not Sp.Spell(Edit1.text) then begin
            Memo1.Lines.BeginUpdate;
            Sp.Suggest('badspeller', Memo1.lines);
            Memo1.Lines.EndUpdate;
        end else
            Memo1.Lines.Clear;
    end;
    

Memo1 теперь содержит несколько советов о лучших способах написания [слова] неграмотному! 

Важно! Не забудьте освободить наш объект hunspeller, утечки памяти - зло! 
    
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
        Sp.free;
        Sp := nil;
    end;
    

Этот модуль делает значительно больше, но он представлен здесь в его наиболее урезанной форме для удобства чтения. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** для новичков в Lazarus, методы с "(Sender: TObject)", показанные выше, не могут быть просто вставлены в ваш исходник, сначала используйте инспектор объектов формы, чтобы создать события, а затем вставьте мой пример кода в метод.

## Demo 3 - Lazspell - Tmemo

В этом примере использовался компонент TMemo. 

[![lazspell.gif](https://wiki.freepascal.org/images/7/71/lazspell.gif)](</File:lazspell.gif>)

Lazspell - пример проверки орфографии - из <https://github.com/Raf20076/Lazspell>

## Demo 4 - Lazspell 2-version 2 - TRichMemo

В этом примере использовался компонент TRichMemo. 

[![lazspell-ver2.gif](https://wiki.freepascal.org/images/3/36/lazspell-ver2.gif)](</File:lazspell-ver2.gif>)

Lazspell 2 v2 - пример проверки орфографии - изm <https://github.com/Raf20076/Lazspell-2-version-2>

## Demo 5 - Простая проверка орфографии

1\. Запустите IDE Lazarus 

2\. Кликните Project -> New Project -> Choose -> Application. Вы только что создали новое приложение. Теперь сохраните его 

3\. Кликните File -> Save as. Выберите папку, в которой будет сохранено ваше приложение. Сначала будет сохранен project1.lpi, затем unit1.pasunit1.pas 

4\. Поместите hunspell.pas и hunspell.inc (from <https://github.com/davidbannon/hunspell4pas>) в вашу папку 

5\. Поместите libhunspell.dll в папку с вашим приложением (вы должны загрузить ее отсюда <https://github.com/Raf20076/Lazspell> или <https://github.com/davidbannon/hunspell4pas>) в любом случае или соберите ее самостоятельно. 

6\. Поместите словарь в папку приложения (оба файла), например pl_PL.aff, pl_PL.dic. Словари должны быть закодированы в UTF8. Скачать их можно отсюда <https://github.com/Raf20076/Lazspell/tree/master/dict>

7\. Положите на Form1 компонент TButton (Button1) с вкладки Standard 

8\. Положите на Form1 компонен TMemo (Memo1) с вкладки Standard 

9\. Положите на Form1 компонен TListbox (ListBox1) с вкладки Standard (здесь будут отображаться слова с ошибками) 

10\. Кликните по кнопке Button1 на Form1 затем перейдите в ObjectInspector, щелкните по вкладке Events, затем дважды щелкните рядом с событием OnClik: это создаст событие OnClick, и код вставки (см. код всего приложения), начинающийся с 
    
    
       
    {Check spelling}
     var
         i : Integer;
         MAX : Integer;
         FillInArrayWithWords:TStringArray;
         FillInString1: String;
         FillInString2: String;
    

до 
    
    
    //Если слова нет в словаре, показать его в Listbox как ошибку
    

11\. Перейдите в ObjectInspector и щелкните Form1, затем щелкните вкладку Events, затем дважды щелкните рядом с OnCreate, это создаст событие OnCreate и вставит код (см. код всего приложения) 
    
    
       SpellCheck := THunspell.Create(True);
       SpellCheck.SetDictionary('pl_PL.dic');//загружаем словарь
       SpellCheck.GoodToGo := True;
    

12\. Оставаясь в ObjectInspector, щелкните Form1, затем щелкните вкладку Events, затем дважды щелкните рядом с OnDestroy, это создаст событие OnDestroy и вставит код (см. код всего приложения) 
    
    
      SpellCheck.free;
      SpellCheck := nil;
    

13\. Затем в вашем коде разместите такие функции, как `ArrayValueCount` и т.д. 

См. ниже код всего приложения, чтобы сравнить его с вашим. 
    
    
    //SpellChecker by Raf20076, Poland 2019
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      LazFileUtils, LCLProc, LazUtils, LazUtf8;
    
    type
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        Label2: TLabel;
        ListBox1: TListBox;
        ListBox2: TListBox;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    uses hunspell;//поместите hunspell.pas и hunspell.inc в папку вашего приложения
                  //из https://github.com/davidbannon/hunspell4pas
                  
    var
      SpellCheck: THunspell;
    
    {$R *.lfm}
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       SpellCheck := THunspell.Create(True);
       SpellCheck.SetDictionary('pl_PL.dic');//загружаем словарь
       SpellCheck.GoodToGo := True;
    end;
    
    {Извлекаем слова из строки: НЕсимволы, пробелы, возврат каретки}
    
    function ArrayValueCount(const InputArray: Array of string): Integer;
    {Подсчет элементов в массиве}
    var
      i:Integer;
    begin
      result := 0;
      for i := low(InputArray) to high(InputArray) do
        if InputArray[i] <> ' ' then  // 'между ними один пробел'
          inc(result);
    end;
    
    function StripOffNonCharacter(const aString: string): string;
    {Удаляем НЕсимволы из строки}
    var
      a: Char;
    begin
      Result := '';
      for a in aString do begin //под знаками препинания цифры, которые нужно удалить из строки
          if not CharInSet( a, ['.', ',', ';', ':', '!', '/',
          '?', '@', '#', '$', '%', '&', '*', '(', ')', '{',
          '}', '[', ']', '-', '\', '|', '<', '>', '''', '"', '^',
          '1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '_', '+',
          '=', '~']) then //'„', '”'])эти метки делают ошибку: ожидается порядковое выражение, необходимо исправить
          begin
            Result := Result + a;
          end;
      end;
    end;
    
    
    function ReplaceCarriageReturn(s: string) : string;
    {Заменяем возврат каретки одним пробелом}
    var
      i: Integer;
    begin
      Result:=s;
      for i := 1 to Length(Result) do
        if Result[i] in [#3..#13] then
          Result[i] := ' ';//'Между ними один пробел'
    end;
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    {Проверяем орфографию}
    var
         i : Integer;
         MAX : Integer;
         FillInArrayWithWords:TStringArray;
         FillInString1: String;
         FillInString2: String;
    begin
        ListBox1.clear;
        ListBox1.Items.Clear;
    
        FillInString1 := ReplaceCarriageReturn(Memo1.Lines.Text);//берем текст из Memo1; заменяем возврат каретки
        //одним пробелом (используя функцию ReplaceCarriageReturn) и помещаем внутрь FillInString1
    
        FillInString2 := StripOffNonCharacter(FillInString1);//удаляем все НЕсимволы из FillInString1
        //(используя функцию StripOffNonCharacter) и помещаем строку без НЕсимволов внутрь FillInString2
    
        FillInArrayWithWords := FillInString2.split(' '); //разделяем строку на слова с помощью ' ' одного пробела
        //(используя .split) и помещаем слова по отдельности в массив FillInArrayWithWords
    
        MAX := ArrayValueCount(FillInArrayWithWords); //узнаем, сколько элементов в массиве 
        //(используя функцию ArrayValueCount)
    
        for i := 0 to MAX -1 do
            if not SpellCheck.Spell(FillInArrayWithWords[i]) then 
                ListBox1.Items.add(FillInArrayWithWords[i]);
        //Берем слово из массива и проверяем по словарю через hunspell (используя функцию SpellCheck.Spell)
        //Если слова нет в словаре, показываем его в Listbox, как ошибку
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
        SpellCheck.free;
        SpellCheck := nil;
    end;
    
    end.
    

Когда вы запустите свое приложение и введете какой-либо текст в Memo, а затем нажмете кнопку, приложение проверит, обнаружены ли какие-либо ошибки, и, если они будут найдены, отобразит их в ListBox. Основная проблема заключалась в том, как удалить возврат каретки и НЕсимволы, а затем как разбить текст на отдельные слова. Это непростая задача. Поэтому для этого есть три функции: функция `ReplaceCarriageReturn`, функция `StripOffNonCharacter` и функция `FillInString2.split`. Функция `FillInString2.split` на самом деле использует функцию `.split` из `SysUtils`. Надеюсь, код всего приложения не требует пояснений. 

## Дальнейшее чтение и ссылки

<https://github.com/hunspell/hunspell>

<https://github.com/Homebrew> \- вероятно, разумный способ получить Hunspell на вашем Mac, если его еще нет. 

<https://github.com/tomboy-notes/tomboy-ng/releases> \- содержит 64-битную Windows DLL в tomboy-ng_win64_<ver>.zip 

<https://github.com/cutec-chris/hunspell>

---

_Source: [https://wiki.freepascal.org/spelling/ru](https://web.archive.org/web/20250122180815/https://wiki.freepascal.org/spelling/ru)_
