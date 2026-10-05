# Getting translation strings right

│ [**Deutsch (de)**](</Getting_translation_strings_right/de> "Getting translation strings right/de") │  [**English (en)**](<../en/Getting_translation_strings_right.md> "Getting translation strings right") │  [**español (es)**](</Getting_translation_strings_right/es> "Getting translation strings right/es") │  [**français (fr)**](</Getting_translation_strings_right/fr> "Getting translation strings right/fr") │  [**日本語 (ja)**](</Getting_translation_strings_right/ja> "Getting translation strings right/ja") │  **русский (ru)** │    
****

Правильный перевод строк. 

## Contents

  * 1 Resourcestrings и GNU gettext
  * 2 ResourceStrings в Лазарусе
  * 3 Перевод файлов пакета и/или проекта
    * 3.1 ResourceStrings в Вашем приложении



### Resourcestrings и GNU gettext

Для работы со строковыми константами, во Free Pascal, была введена специальная конструкция: секция "resourcestring". Подробнее вы сможете прочитать в руководстве по FPC (prog.pdf, стр 89 или [здесь](<http://www.freepascal.org/docs-html/prog/progch9.html#x184-1870009>)) 

  
GNU gettext это специальный набор утилит, для того, чтобы облегчить перевод вашей программы на различные языки. Подробнее описывается в руководстве по FPC (prog.pdf, стр 91 или [здесь](<http://www.freepascal.org/docs-html/prog/progse41.html#x210-2130009.4>)). 

_Внимание:_ GNU gettext имеет принципиальный недостаток. Одна исходная строка, может соответствовать только одной (и не более) переведённой строке. 

Это важно в тех случаях, когда одна и та же фраза на исходном языке может использоваться в разных значениях, и должна быть переведена по разному, в зависимости от контекста. 

Об этом нужно помнить и не употреблять выражений, в исходном языке со множественными значениями 

### ResourceStrings в Лазарусе

  * Для конвертации строковых констант в resourcestrings, в Лазурсе создана специальная утилита: [Make ResourceString](<../en/IDE_Window__Make_ResourceString.md> "IDE Window: Make ResourceString")


  * Для каждой секции resourcestring, компилятор создаёт .rst файл, но редактора для этих файлов нет. Лазарус может автоматический создавать .po файлы из .rst файлов. Для работы с .po файлами есть много утилит (например kbabel).



### Перевод файлов пакета и/или проекта

Чтобы разрешить создание .po файлов для пакета, выполните следующие шаги: 

  * Создайте директорию 'languages' (или 'locale', имя не существенно). Директория должна находится в директории пакета


  * Откройте пакет. Затем настройки пакета: Options -> IDE Integration -> Directory of .po files. Укажите созданную директорию _languages_.



В следующий раз когда вы будете компилировать пакет, Лазарус создаст необходимые .po файлы. 

_Внимание:_ .rst файлы должны принадлежать пакету, иначе Лазарус их проигнорирует. 

Точно так же для проектов. Директорию .po файлов нужно указать в Project -> Project Options -> IDE Integration -> Directory of .po files. 

  * Чтобы создать перевод на другой язык: скопируйте созданный .po файл, изменив его расширение. А затем, используя редактор, переведите все строки необходимые, в скопированном файле.



Например, для германского языка: чтобы перевести файл unit1.po, его нужно скопировать в unit1.de.po, а содержимое нового файла перевести на германский язык. 

  * Лазарус автоматически загружает .po файлы для установленных пакетов, если они есть. Для примера см: lazarus/components/projecttemplates/languages/.


  * ToDo: Implement and document updating the translated .po files when new resourcestrings are added.


  * ToDo: Implement and document collecting all .po files of statically linked packages.



  


#### ResourceStrings в Вашем приложении

Вы можете загрузить .po файл во время инициализации, для того чтобы перевести строки, указанные в секциях resourcestrings. Добавьте этот код в файл .lpr проекта: 
    
    
    ...
    uses
      ...
      Translations, LazUTF8;
    
    procedure TranslateLCL;
    var
      PODirectory, Lang, FallbackLang: String;
    begin
      PODirectory:='/path/to/lazarus/lcl/languages/';
      Lang:='';
      FallbackLang:='';
      LazGetLanguageIDs(Lang,FallbackLang); // in unit LazUTF8
      Translations.TranslateUnitResourceStrings('LCLStrConsts',
                          PODirectory+'lclstrconsts.%s.po',Lang,FallbackLang);
      // ... add here a TranslateUnitResourceStrings call for every po file ...
    end;
    
    begin
      TranslateLCL;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

Заметка для Mac OS X: ID поддерживаемых языков должны быть добавлены в список свойств приложения (файл в бандлеInfo.plist) в пункт CFBundleLocalizations. Для примера см. lazarus.app/Contents/Info.plist.

---

_Source: [https://wiki.freepascal.org/Getting_translation_strings_right/ru](https://web.archive.org/web/20200813000315/https://wiki.freepascal.org/Getting_translation_strings_right/ru)_
