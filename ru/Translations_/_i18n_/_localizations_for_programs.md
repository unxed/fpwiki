# Translations / i18n / localizations for programs

│ **[English (en)](<../../../en/Translations_/_i18n_/_localizations_for_programs.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
  * 2 Формат даты, времени и чисел
  * 3 Строковые ресурсы
  * 4 .po файлы
  * 5 Перевод
  * 6 Настройки IDE для автоматического обновления .po файлов
    * 6.1 Удаление устаревших записей
    * 6.2 Повторяющиеся записи
    * 6.3 Нечёткие записи
  * 7 Перевод Форм, Модулей данных и Фреймов
  * 8 Перевод при старте программы
  * 9 Добавление .po файлов в ресурсы программ
  * 10 Локализация IDE
    * 10.1 Файлы
    * 10.2 Переводчики
  * 11 Future work / ToDos



## Введение

В данном тексте речь пойдёт о том, как сделать, чтобы программа могла использовать разные строки для разных языков. То есть, как локализовать свою программу для использования с различными языками интерфейса. Для трансляции приложения необходимо выполнить следующие действия: 

  * Все отображаемые строки добавить в секцию **resourcestrings** для создания при компиляции **.rst** и/или **.po** файлов (IDE преобразование .rst в .po делает автоматически);
  * Для каждого языка создать перевод полученных .po файлов (есть бесплатный редактор);
  * Воспользоваться функциями LCL для автоматического перевода при запуске приложения.



## Формат даты, времени и чисел

В Linux, BSD, Mac OS X для локализации формата даты, времени и разделителя тысяч в числе, есть специальный модуль **clocale**. Чтобы инициализировать RTL локализованными системными параметрами необходимо в файл проекта (.lpr файл) добавить модуль **clocale**. 

## Строковые ресурсы

Пример 
    
    
     resourcestring
       Caption1 = 'Some text';
       HelloWorld1 = 'Hello World';
    

Все строки объявленные в секции **resourcestring** являются обыкновенными строковыми константами, это значит, что вы можете их присвоить любой строке. Например 
    
    
     Label1.Caption := HelloWorld1;
    

Во время компиляции FPC создаёт для каждого модуля содержащего секцию **resourcestring** файл **unitname.rst** , в котором будут все константы секции в виде **имя + значение**. 

## .po файлы

Есть много свободных утилит для редактирования **.po** файлов, которые являются просто текстовыми как и **.rst** файлы. Утилиты зачастую позволяют добавлять дополнительную информацию к файлам, такую как автор перевода, кодировка, язык и дата. В дистрибутиве FPC есть утилита **rstconv** (в Windows: rstconv.exe), которая позволяет преобразовать **.rst** файл в **.po**. IDE делает преобразование автоматически. 

Пример свободных утилит: kbabel, poedit. 

Пример использования rstconv: 
    
    
     rstconv -i unit1.rst -o unit1.po
    

## Перевод

Для каждого языка должен быть создан свой файл **.po**. В LCL используются стандартные коды для поиска языков (en=английский, de=немецкий, ru=русский, ...). Например, в русском переводе unit1.po будет unit1.ru.po. Это означает, что нужно скопировать файл unit1.po в unit1.ru.po, unit1.de.po и т.д. для всех языков, которые вы хотите использовать, а затем переводчики могут редактировать **.po** файл для конкретного языка. 

## Настройки IDE для автоматического обновления .po файлов

  * Модули, содержащие строки ресурсов должны быть добавлены в пакет или проект.
  * Вы должны создать каталог для **.po** файлов. Например: создать подкаталог **языка** в каталоге проекта/пакета. Для проектов перейти в меню IDE _Проект >Параметры проекта_ и на вкладке _i18n_ выбрать _Включить i18n_ и указать созданный вами каталог. В случае пакета соответственно _Параметры пакета_.



При включённом **i18n** IDE автоматически создаёт или обновляет **.po** файлы используя информацию содержащуюся в **.rst** и **.lrt** файлах (использовать в этом случае утилиту rstconv не нужно). Процесс обновления начинается со сбора всех записей из файлов **.rst** и **.lrt** и запись их в **.po** файл, так же автоматически обновляются и все файлы с переводами **.xx.po**. 

### Удаление устаревших записей

Записи файла **.po** не найденные в **.rst** и **.lrt** файлах автоматически удаляются. Также они удаляются из файлов перевода **.xx.po** , потому что нет смысла переводить записи, которые не используются. 

### Повторяющиеся записи

Повторяющиеся записи могут появиться, если один и тоже текст используется для разных строковых ресурсов. Например, возьмём файл _lazarus/ide/lazarusidestrconst.pas_ в нём текст 'Gutter' встречается у нескольких строковых констант: 
    
    
      dlfMouseSimpleGutterSect = 'Gutter';
      dlgMouseOptNodeGutter = 'Gutter';
      dlgGutter = 'Gutter';
      dlgAddHiAttrGroupGutter   = 'Gutter';
    

После преобразования из **.rst** файла содержимое **.po** файла будет примерно следующее: 
    
    
    #: lazarusidestrconsts.dlfmousesimpleguttersect
    msgid "Gutter"
    msgstr ""
    #: lazarusidestrconsts.dlgaddhiattrgroupgutter
    msgid "Gutter"
    msgstr ""
    и т.д.
    
    

Если строка начинается с "#:" то она считается комментарием и инструменты, используемые для перевода увидев многократное повторение msgid "Gutter" будут считать их дублирующимися записями и выдавать ошибку или предупреждение. Повторяющиеся записи считаются нормальными для **.po** файла, если они имеют контекст определённый ключевым словом msgctxt. Данное ключевое слово добавляет контекст дублирующимся записям для автоматического обновления и перевода. Имя для контекста берётся из строки после префикса "#:". Смотрите пример ниже: 
    
    
    #: lazarusidestrconsts.dlfmousesimpleguttersect
    msgctxt "lazarusidestrconsts.dlfmousesimpleguttersect"
    msgid "Gutter"
    msgstr ""
    #: lazarusidestrconsts.dlgaddhiattrgroupgutter
    msgctxt "lazarusidestrconsts.dlgaddhiattrgroupgutter"
    msgid "Gutter"
    msgstr ""
    и т.д.
    
    

В переведённые файлы **.xx.po** дублирующиеся записи автоматически не добавляются. Если в файле присутствует запись с таким же msgid что и добавляемая, то считается, что добавляемая запись уже переведена, и она просто не добавляется. 

Автоматическое обнаружение дублируемых строк ещё не совершенно. Дубликаты как элементы добавляются в список, и может случиться так, что некоторые непереведённые записи могут прочитаться первыми. 

### Нечёткие записи

Изменения в строковых ресурсах влияет на переводы, например, если строка сначала бала определена так: 
    
    
      dlgEdColor = 'Syntax highlight';
    

после преобразования получим следующую **.po** запись 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Syntax higlight"
    msgstr ""
    

которые после перевода на русский язык примет, следующий вид 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Syntax higlight"
    msgstr "Подсветка синтаксиса"
    

Предположим, что позднее строка была изменена на 
    
    
      dlgEdColor = 'Colors';
    

в результате запись в **.po** файле будет 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Colors"
    msgstr ""
    

Обратите внимание, что хотя идентификатор остаётся неизменным lazarusidestrconsts.dlgedcolor строку изменили с 'Syntax highlight' на 'Colors', так как строку уже перевели, перевод будет не совпадать с новым смыслом. Утилита автоматического обновления сообщит об этом факте и запишет запись следующего вида: 
    
    
    #: lazarusidestrconsts.dlgedcolor
    #, fuzzy
    #| msgid "Syntax highlight"
    msgctxt "lazarusidestrconsts.dlgedcolor"
    msgid "Colors"
    msgstr "Подсветка синтаксиса"
    

С точки зрения формата **.po** файла префикс "#," означает, что запись имеет флаг (нечёткой) и программа перевода может оповестить пользователя, что нынешний перевод является сомнительным и необходимо повторно перевести данную запись. В свою очередь префикс "#|" указывает предыдущую непереведённую строку. 

## Перевод Форм, Модулей данных и Фреймов

Когда опция i18n включена IDE автоматически создаёт **.lrt** файлы для каждой формы, модуля данных и фрейма. Если вы включили эту опцию в первый раз, то вы должны открыть каждую форму и немножко переместить её для того чтобы можно было сохранить её. После сохранения автоматически создаются **.lrt** фалы (если модуль с формой unit1.pas, то создастся соответственно файл unit1.lrt). При компиляции будет создан файл projectname.po или packagename.po в каталоге .po файлов, в данный файл будут помещены все записи из всех **.lrt** фалов проекта. 

Для форм, которые должны быть переведены во время выполнения, вы должны назначить транслятор LRSTranslator (определён в модуле LResources) в разделе инициализации одного из модулей. 
    
    
    ...
    uses
      ...
      LResources;
    ...
    ...
    initialization
      LRSTranslator:=TPoTranslator.Create('/path/to/the/po/file');
    

Однако в LCL нет никакого класса TPoTranslator (т.е. класса который умеет читать **.po** файлы). Его можно создать самому, подсмотрев реализацию класса TDefaultTranslator в модуле DefaultTranslator.pas. 
    
    
    unit PoTranslator;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, typinfo, Translations;
    
    type
    
     { TPoTranslator }
    
     TPoTranslator=class(TAbstractTranslator)
     private
      FPOFile:TPOFile;
     public
      constructor Create(POFileName:string);
      destructor Destroy;override;
      procedure TranslateStringProperty(Sender:TObject; 
        const Instance: TPersistent; PropInfo: PPropInfo; var Content:string);override;
     end;
    
    implementation
    
    { TPoTranslator }
    
    constructor TPoTranslator.Create(POFileName: string);
    begin
      inherited Create;
      FPOFile:=TPOFile.Create(POFileName);
    end;
    
    destructor TPoTranslator.Destroy;
    begin
      FPOFile.Free;
      inherited Destroy;
    end;
    
    procedure TPoTranslator.TranslateStringProperty(Sender: TObject;
      const Instance: TPersistent; PropInfo: PPropInfo; var Content: string);
    var
      s: String;
    begin
      if not Assigned(FPOFile) then exit;
      if not Assigned(PropInfo) then exit;
    {Нужно ли нам это?}
      if Instance is TComponent then
       if csDesigning in (Instance as TComponent).ComponentState then exit;
    {:)}
      if (AnsiUpperCase(PropInfo^.PropType^.Name)<>'TTRANSLATESTRING') then exit;
      s:=FPOFile.Translate(Content, Content);
      if s<>'' then Content:=s;
    end;
    
    end.
    

Или же вы можете конвертировать файл **.po** в **.mo** используя утилиту msgfmt и просто использовать модуль DefaultTranslator 
    
    
    ...
    uses
       ...
       DefaultTranslator;
    

который будет автоматически искать **.mo** фалы в нескольких стандартных местах. (Недостатком является то что вам придется с программой поставлять сразу два файла: **.mo** для модуля DefaultTranslator и **.po** файл для TranslateUnitResourceStrings). Если вы все-таки решили воспользоваться DefaultTranslator. Определение языка будет производиться автоматически по переменной окружения **LANG** или по параметру командной строки **\--lang**. **.mo** файлы должны иметь следующие и мена и каталоги размещения: 

  * <Application Directory>/LANG/<Application Filename>.mo
  * <Application Directory>/languages/LANG/<Application Filename>.mo
  * <Application Directory>/locale/LANG/<Application Filename>.mo
  * <Application Directory>/locale/LC_MESSAGES/LANG/<Application Filename>.mo



в Unix-подобных система файл может ещё находится в 

  * /usr/share/locale/LANG/LC_MESSAGES/<Application Filename>.mo



а также поиск будет производиться по короткому идентификатору языка (например, если локаль "ru_RU" или "ru_RU.UTF-8" и файла с данным языком не найдено, то поиск также будет производиться для "RU"). 

## Перевод при старте программы

Для каждого **.po** файла вы должны вызвать функцию TranslateUnitResourceStrings для перевода каждого модуля. Например: 
    
    
        {Прежде всего добавьте модули "gettext" и "translations"}
        procedure TForm1.FormCreate(Sender: TObject);
        var
          PODirectory, Lang, FallbackLang: String;
        begin
          PODirectory := '/path/to/lazarus/lcl/languages/';
          GetLanguageIDs(Lang, FallbackLang); // определено в модуле gettext
          TranslateUnitResourceStrings('LCLStrConsts', PODirectory + 'lclstrconsts.%s.po', Lang, FallbackLang);
          MessageDlg('Title', 'Text', mtInformation, [mbOk, mbCancel, mbYes], 0);
        end;
    

## Добавление .po файлов в ресурсы программ

Если вы хотите поместить **.po** файлы в исполняемый файл, то выполните следующие действия: 

  * Создайте новый модуль (не модуль формы!).
  * Конвертируйте **.po** файл(ы) в **.lrs** с помощью утилиты tools/lazres:


    
    
    ./lazres unit1.lrs unit1.ru.po
    

Создастся файл unit1.lrs начинающийся строкой 
    
    
    LazarusResources.Add('unit1.ru','PO',[
      ...
    

  * Добавьте в код:


    
    
    uses LResources, Translations;
    
    resourcestring
      MyCaption = 'Caption';
    
    function TranslateUnitResourceStrings: boolean;
    var
      r: TLResource;
      POFile: TPOFile;
    begin
      r:=LazarusResources.Find('unit1.ru','PO');
      POFile:=TPOFile.Create;
      try
        POFile.ReadPOText(r.Value);
        Result:=Translations.TranslateUnitResourceStrings('unit1',POFile);
      finally
        POFile.Free;
      end;
    end;
    
    initialization
      {$I unit1.lrs}
    

  * Выполните функцию TranslateUnitResourceStrings в начале программы. Вы можете это сделать в секции initialization.



## Локализация IDE

### Файлы

Файлы **.po** находятся в каталогах исходников IDE Lazarus: 

  * lazarus/languages перевод для IDE
  * lcl/languages/ перевод для LCL
  * ideintf/languages/ перевод для интерфейса IDE



### Переводчики

  * Немецкий перевод поддерживает Joerg Braun.
  * Финский перевод поддерживает Seppo Suurtarla
  * Русский перевод поддерживает Максим Ганецкий



Если вы хотите начать новый перевод, задайте вопрос в рассылку, может уже кто-то над ним работает. 

Пожалуйста, прочитайте внимательно: [Переводы](<../../../en/Lazarus_Documentation.md> "Lazarus Documentation")

## Future work / ToDos

[IDE Development: Переводы, i18n, lrt файлы, po файлы](<../../IDE_Development.md> "IDE Development/ru")

---

_Source: [https://wiki.freepascal.org/Translations_/_i18n_/_localizations_for_programs/ru](https://web.archive.org/web/20250318235453/https://wiki.freepascal.org/Translations_/_i18n_/_localizations_for_programs/ru)_
