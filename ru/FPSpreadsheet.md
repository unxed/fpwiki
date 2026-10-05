# FPSpreadsheet

│ **[Deutsch (de)](</FPSpreadsheet/de> "FPSpreadsheet/de")** │  **[English (en)](<../en/FPSpreadsheet.md> "FPSpreadsheet")** │  **[español (es)](</FPSpreadsheet/es> "FPSpreadsheet/es")** │  **[français (fr)](</FPSpreadsheet/fr> "FPSpreadsheet/fr")** │  **[polski (pl)](</FPSpreadsheet/pl> "FPSpreadsheet/pl")** │  **русский (ru)** │    
****

FpSpreadsheet библиотека предоставляет удобный способ создания и чтения табличных документов в различных форматах. Библиотека написана на очень гибкой основе, и поэтому её можно легко расширять поддержкой любого количества форматов. 

Скриншот демонстрационной программы spready, предоставляемой с fpspreadsheet, показывающим файл XLS: 

  
[![fpsgrid.png](https://wiki.freepascal.org/images/0/02/fpsgrid.png)](</File:fpsgrid.png>)

  


## Contents

  * 1 Документация
  * 2 Документация по API
    * 2.1 Ссылка на API
    * 2.2 Основы
      * 2.2.1 Рабочая книга
      * 2.2.2 Рабочий лист
      * 2.2.3 Ячейка
        * 2.2.3.1 Основной тип TCell record
        * 2.2.3.2 Как добавлять и считывать данные в/из ячейки
        * 2.2.3.3 Индексирование формата ячейки
      * 2.2.4 Столбцы и строки
    * 2.3 Формулы
      * 2.3.1 Строковые формулы
      * 2.3.2 Формулы RPN (Обратной польской нотации)
      * 2.3.3 Общие формулы и формулы массива
      * 2.3.4 Список встроенных формул
      * 2.3.5 Расширение FPSpreadsheet по пользовательским формулам
      * 2.3.6 Неподдерживаемые формулы
    * 2.4 Форматирование ячеек
      * 2.4.1 Форматы числа и даты/времени
        * 2.4.1.1 Встроенные форматы чисел
        * 2.4.1.2 Строки формата чисел
      * 2.4.2 Цвета
      * 2.4.3 Фон ячейки
      * 2.4.4 Границы ячеек
      * 2.4.5 Шрифты
      * 2.4.6 Форматирование Rich-text
      * 2.4.7 Вращение текста
      * 2.4.8 Выравнивание текста
      * 2.4.9 Перенос слов
      * 2.4.10 Объединенные ячейки
      * 2.4.11 Защита ячейки
    * 2.5 Дополнительная информация
      * 2.5.1 Комментарии к ячейке
      * 2.5.2 Гиперссылки
      * 2.5.3 Изображения
    * 2.6 Сортировка
    * 2.7 Поиск и замена
    * 2.8 Операции со столбцами и строками
    * 2.9 Макет страницы
      * 2.9.1 Общие сведения
      * 2.9.2 Заголовки и колонтитулы
    * 2.10 Защита
      * 2.10.1 Защита рабочей книги
      * 2.10.2 Защита рабочего листа
      * 2.10.3 Защита ячейки
      * 2.10.4 Защита
    * 2.11 Загрузка и сохранение
      * 2.11.1 Добавление новых форматов файлов
      * 2.11.2 Выбор потока
      * 2.11.3 Виртуальный режим
    * 2.12 Экспорт данных
      * 2.12.1 Экспорт в DBF
  * 3 Визуальные элементы управления для FPSpreadsheet
  * 4 Примеры
    * 4.1 Модули
    * 4.2 Примеры Excel 5
    * 4.3 Открытие существующей таблицы
    * 4.4 Запись электронной таблицы в файл на основе расширения
    * 4.5 Итерация по всем рабочим листам
    * 4.6 Итерация через ячейки
    * 4.7 Преобразование базы данных в электронную таблицу
    * 4.8 Преобразование большой таблицы базы данных в электронную таблицу с использованием виртуального режима
    * 4.9 Преобразование между двумя форматами электронных таблиц
    * 4.10 Чтение и запись CSV-файлов
    * 4.11 Примеры проектов в папке установки fpspreadsheet
  * 5 Загрузка
    * 5.1 Система контроля версий (Subversion)
      * 5.1.1 Журнал изменений SVN
      * 5.1.2 Несовместимые изменения
    * 5.2 Стабильные версии
      * 5.2.1 Version 1.10.1
        * 5.2.1.1 Change log (with respect to v1.8x)
        * 5.2.1.2 Incompatible changes
      * 5.2.2 Version 1.8
        * 5.2.2.1 Change log (with respect to v1.6x)
        * 5.2.2.2 Incompatible changes
      * 5.2.3 Version 1.6
        * 5.2.3.1 Change log (with respect to v1.4.x)
        * 5.2.3.2 Incompatible changes
      * 5.2.4 Version 1.4
        * 5.2.4.1 Change log (with respect to v1.2.x)
        * 5.2.4.2 Incompatible changes
      * 5.2.5 Wiki documentation of old releases
  * 6 Установка
    * 6.1 Параметры компилятора
  * 7 Поддержка и отчетность об ошибках
  * 8 Текущие достижения
    * 8.1 Достижения в поддержке содержимого ячеек
    * 8.2 Достижения опций форматирования
    * 8.3 Достижения [реализации] пользовательских интерфейсов рабочей книги/рабочего листа
    * 8.4 Список [запланированных] дел
  * 9 Changelog
  * 10 Лицензия
  * 11 Ссылки
    * 11.1 Wiki ссылки
    * 11.2 Внешние ссылки



## Документация

Эта страница вики включает последнюю версию FPSpreadsheet, доступную через subversion. Пожалуйста, см. раздел Стабильные версии для [получения] документации по стабильной версии, которую вы можете скачать. 

## Документация по API

### Ссылка на API

Файл справки в формате CHM можно найти в папке установки FPSpreadsheet _docs_. Если вы еще не установили пакет, тогда следуйте сюда <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/fpspreadsheet/docs>

Второй файл CHM, доступный в папке _docs_ , _fpspreadsheet-wiki.chm_ , представляет собой снимок связанных с FPSpreadsheet страниц вики, объединенных в один файл справки. 

### Основы

Наименьшими объектами в электронной таблице являются «ячейки», содержащие данные. Ячейки могут содержать различные типы данных, такие как строки, числа, даты, время, логические значения или формулы. Кроме того, ячейки могут содержать информацию о форматировании, то есть стиль шрифта, цвет фона, выравнивание текста и т.д. 

Ячейки расположены в сетке, называемой «рабочей таблицей», или «электронной таблицей», состоящей из «строк» и «столбцов». Каждая ячейка имеет уникальный адрес, заданный индексом строки и столбца. 

Рабочие листы связаны вместе, формируя «книгу», которая представляет собой документ электронной таблицы приложения. В книге также хранится информация, необходимая для всех листов, то есть список шрифтов, списки сотовых и числовых форматов и т.д. 

FPSpreadsheet следует этой же структуре - существует `TCell`, `TsWorksheet` и `TsWorkbook `. 

#### Рабочая книга

Класс `TsWorkbook` является основным классом, видимым для пользователя. Он предоставляет методы для чтения данных и записи в файлы. Универсальная структура библиотеки обеспечивает доступ к различным популярным форматам файлов, таким как Excel _.xls_ или _.xlsx_ , или OpenOffice/LibreOffice _.ods_. 

Формат файла определяется типом `TsSpreadsheetFormat`, определенным в модуле `fpstypes`
    
    
    type
      TsSpreadsheetFormat = (sfExcel2, sfExcel5, sfExcel8, sfExcelXML, sfOOXML, 
        sfOpenDocument, sfCSV, sfHTML, sfWikiTable_Pipes, sfWikiTable_WikiMedia, sfUser);
    

где 

  * `sfExcel2, sfExcel5, sfExcel8` означает версии бинарного формата xls, используемые Excel («BIFF» = «Формат файла двоичного обмена»), причем `sfExcel8 ` является самым современным.
  * `sfOOXML` соответствует новому формату xlsx, представленному Excel2007
  * `sfExcelXML` \- это xml-формат, который был представлен Microsoft для Office XP и 2003. Не очень популярен.
  * `sfOpenDocument` \- формат электронных таблиц, используемый OpenOffice/LibreOffice; по умолчанию файлы имеют расширение _.ods_.
  * `sfCSV` относится к текстовым файлам с разделителями-запятыми (расширение по умолчанию _.csv_); они могут быть поняты любым текстовым редактором и всеми программами электронных таблиц, но не содержат информации о форматировании.
  * `sfHTML` обозначает стандартный формат HTML, используемый в веб-браузерах.
  * `sfWikiTable_Pipes` и `sfWikiTable_WikiMedia` \- это формат, используемый таблицами в вики-сайтах.
  * `sfUser` необходим для регистрации пользовательского формата. Планируется внедрение «древних» форматов файлов, таких как Excel3.0/4.0 или Lotus. Тем не менее, вы можете предоставить свои собственные классы чтения и письма для расширения функциональности FPSpreadsheet - см. ниже раздел Добавление новых форматов файлов



При применении fpspreadsheet первая задача - создать экземпляр рабочей книги: 
    
    
    var
      MyWorkbook: TsWorkbook;
    begin
      MyWorkbook := TsWorkbook.Create;
      ...
    

  
Чтение файлов электронных таблиц выполняется (среди прочего) методами рабочей книги 

  * `procedure ReadFromFile (AFileName: string):`   
Читает файл с заданным именем и автоматически определяет правильный формат файла.
  * `procedure ReadFromFile (AFileName: string; AFormat: TsSpreadsheetFormat):`   
Читает файл, но предполагает, что формат файла соответствует `AFormat `.



Для сохранения файла можно использовать следующие методы работы с книгой: 

  * `procedure WriteToFile (const AFileName: string; const AFormat: TsSpreadsheetFormat; const AOverwriteExisting: Boolean = False)`:   
Записывает книгу в данный файл с использованием заданного формата электронной таблицы. Если файл уже существует, он автоматически перезаписывается, если `AOverwriteExisting` \- `true `:
  * `procedure WriteToFile (const AFileName: String; const AOverwriteExisting: Boolean = False)`:   
так же, но формат файла определяется из предоставленного расширения файла (в случае Excel больше xls используется последняя версия, `sfExcel8`).



После вызова этих методов полезно посмотреть на свойство рабочей книги `ErrorMsg`, в котором собраны сообщения из-за ошибок или предупреждений, которые могли возникнуть во время чтения/записи. Это свойство возвращает многострочную строку, которая лучше всего отображается в компоненте memo; если все в порядке, это пустая строка. 

**Примечание.** FPSpreadsheets предоставляет специализированные блоки для чтения и записи в каждый формат файла. Эти блоки недоступны автоматически, вы должны явно добавить их в предложение uses. FPSpreadsheet будет жаловаться на «неподдерживаемый формат файла», если запрошенный читатель/писатель не найден. Ниже приведен список названий устройств: 

  * _xlsbiff2_ , _xlsbiff5_ и _xlsbiff8_ для двоичных форматов файлов xls `sfExcel2`, `sfExcel5` и `sfExcel8` , соответственно,
  * _xlsOOXML_ для формата xlsx `sfOOXML` для Excel 2007 и более поздних версий,
  * _xlsXML_ для XML-формата Excel XP и 2003,
  * _fpsopendocument_ для формата файла `sfOpenDocument` OpenOffice/LibreOffice,
  * _fpsCSV_ для текстовых файлов с разделенными запятыми значениями (csv),
  * _fpsHTML_ для файлов HTML,
  * _wikitables_ для `sfWikiTable_Pipes` и `sfWikiTable_WikiMedia`,
  * или просто добавьте _fpsallformats_ , чтобы получить поддержку чтения/записи для всех поддерживаемых форматов файлов.



#### Рабочий лист

В книге содержится список экземпляров `TsWorksheet`. Они соответствуют вкладкам, которые вы видите в Excel или Open/LibreOffice. При чтении файла электронной таблицы рабочие листы создаются автоматически в соответствии с содержимым файла. Когда электронная таблица создается вручную для хранения в файле, рабочий лист должен быть создан путем «добавления» в рабочую книгу: 
    
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
    begin
      MyWorkbook := TsWorkbook.Create;
      MyWorksheet := MyWorkbook.AddWorksheet('My_Table');  
        // 'My_Table' - это «имя» рабочего листа
      ...
    

Доступ к уже имеющимся рабочим листам можно получить с помощью методов `TsWorkbook`

  * `function GetFirstWorksheet: TsWorksheet`: извлекает первый рабочий лист книги.
  * `Функция GetWorksheetByIndex (AIndex: Cardinal): TsWorksheet`: возвращает рабочий лист с заданным индексом (начиная с 0).
  * `function GetWorksheetByName (AName: String): TsWorksheet`: возвращает рабочий лист с заданным именем, которое было использовано при добавлении рабочего листа.



Количество уже существующих рабочих листов можно запросить, вызвав `GetWorksheetCount`. 

#### Ячейка

Рабочий лист, наконец, дает доступ к ячейкам. Недавно созданный рабочий лист, как в приведенном выше примере, пуст и не содержит никаких ячеек. Клетки добавляются путем присвоения им данных или атрибутов одним из методов `WriteXXXX` рабочего листа. Как уже упоминалось, ячейка адресуется индексом строки и столбца, к которой она принадлежит. Как обычно, индексы строк и столбцов начинаются с 0. Поэтому ячейка «A1» относится к строке 0 и столбцу 0. Следует отметить, что индекс строки и столбца всегда указывается в этом порядке, это _отличается от соглашения_ `TStringGrid`. В следующем примере создается ячейка по адресу A1 и помещается в нее число `1.0`. 
    
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
    begin
      MyWorkbook := TsWorkbook.Create;
      MyWorksheet := MyWorkbook.AddWorksheet('My_Table');
      MyWorksheet.WriteNumber(0, 0, 1.0);  // "A1" имеет строку = 0 и столбец = 0
      ...
    

Также можно напрямую обращаться к ячейкам с помощью методов `FindCell(ARow, ACol)` или `GetCell(ARow, ACol)` рабочей книги. Обе функции существуют также в перегруженной версии, к которой адрес ячейки может быть передан в нотации Excel: `FindCell(ACellStr: String)` или `GetCell(ACellStr: String)`. Имейте в виду, что эти функции возвращают указатель `pointer` к данным ячейки (тип `PCell`). Не забудьте разыменовать указатели! Разница между `FindCell` и `GetCell` заключается в том, что первая возвращает `nil`, если ячейка еще не существует, а вторая создает пустую ячейки в этом случае. 
    
    
    if MyWorksheet.FindCell('A1') = nil then
      WriteLn('Ячейка A1 не существует.');
    

  


##### Основной тип TCell record

Это объявление типа данных ячейки: 
    
    
    type
      TCell = record
        { Location of the cell }
        Worksheet: TsWorksheet;
        Col: Cardinal; // zero-based
        Row: Cardinal; // zero-based
    
        { Index of format record }
        FormatIndex: Integer;
    
        { Status flags }
        Flags: TsCellFlags;        // (cfHasComment, cfMerged, cfHyperlink, ...)
    
        { Cell content }
        UTF8StringValue: String;   // строки не могут быть частью варианта записи 
        case ContentType: TCellContentType of  // должно быть в конце объявления
          cctEmpty      : ();      // не имеет данных вообще
          cctFormula    : ();      // UTF8StringValue находится вне варианта записи
          cctNumber     : (Numbervalue: Double);
          cctUTF8String : ();      // FormulaValue находится вне варианта записи 
          cctDateTime   : (DateTimevalue: TDateTime);
          cctBool       : (BoolValue: boolean);
          cctError      : (ErrorValue: TsErrorValue);
      end;
      PCell = ^TCell;
    

  
Поле `ContentType` указывает, какой тип данных хранится в ячейке: 
    
    
    type
      TCellContentType = (cctEmpty, cctFormula, cctNumber, cctUTF8String, cctDateTime, cctBool, cctError);
    

Согласно этому полю соответствующие данные можно найти в полях 

  * `NumberValue` (для `ContentType = cctNumber`), или
  * `UTF8StringValue` (для `ContentType = cctUTF8String`), или
  * `DateTimeValue` (для `ContentType = cctDateTime`) или
  * `BoolValue` (для `ContentType = cctBool`), то есть `TRUE` или `FALSE`, или
  * `ErrorValue` (для `ContentType = cctError`).



Из-за использования варианта записи большинство из этих значений перекрываются, то есть модификация `NumberValue` влияет также на другие значения. Поэтому всегда обращайте внимание на `ContentType` при непосредственном доступе к записи `TCell` (описанные ниже методы рабочего листа рассматривают это автоматически). 

Поле `Flags` указывает, связаны ли дополнительные данные с ячейкой, которые не включены в запись ячейки, как правило, для сохранения памяти: 
    
    
    type 
      TsCellFlag = (cfHasComment, cfHyperlink, cfMerged, cfHasFormula, cf3dFormula);
      TsCellFlags = set of TsCellFlag;
    

  * `cfHasComment`: комментарий записи находится в `comments` рабочего листа.
  * `cfHyperlink`: ячейка содержит гиперссылку, хранящуюся в `Hyperlinks` рабочего листа.
  * `cfMerged`: ячейка принадлежит блоку объединенные ячейки и распространяется через несколько ячеек.
  * `cfHasFormula`: ячейка связана с формулой, которая хранится в `Formulas` рабочего листа.
  * `cf3dFormula`: Формула, связанная с ячейкой, содержит элементы, ссылающиеся на другие листы той же книги.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** После вычисления формулы или после прочтения файла `ContentType` ячейки формулы преобразуется в формулу результата. Тогда наличие формулы может быть обнаружено только путем вызова функции `HasFormula(cell)` для запрашиваемой ячейки (`cell: PCell`) в модуле _fpsUtils_ ; эта функция проверяет наличие элемента `cfHasFormula` в флажках ячейки.

##### Как добавлять и считывать данные в/из ячейки

Добавление значений в ячейку наиболее легко сопровождается использованием одного из методов `WriteXXXX` рабочего листа. Наиболее важными являются: 
    
    
    type
      TsWorksheet = class
        ...
        { Запись денежных значений }
        function WriteCurrency(ARow, ACol: Cardinal; AValue: Double;
          ANumFormat: TsNumberFormat = nfCurrency; ADecimals: Integer = 2;
          ACurrencySymbol: String = '?'; APosCurrFormat: Integer = -1;
          ANegCurrFormat: Integer = -1): PCell; overload;
        procedure WriteCurrency(ACell: PCell; AValue: Double;
          ANumFormat: TsNumberFormat = nfCurrency; ADecimals: Integer = -1;
          ACurrencySymbol: String = '?'; APosCurrFormat: Integer = -1;
          ANegCurrFormat: Integer = -1); overload;
        function WriteCurrency(ARow, ACol: Cardinal; AValue: Double;
          ANumFormat: TsNumberFormat; ANumFormatString: String): PCell; overload;
        procedure WriteCurrency(ACell: PCell; AValue: Double;
          ANumFormat: TsNumberFormat; ANumFormatString: String); overload;
    
        {Запись значений дата/время }
        function WriteDateTime(ARow, ACol: Cardinal; AValue: TDateTime): PCell; overload;
        procedure WriteDateTime(ACell: PCell; AValue: TDateTime); overload;
        function WriteDateTime(ARow, ACol: Cardinal; AValue: TDateTime;
          ANumFormat: TsNumberFormat; ANumFormatStr: String = ''): PCell; overload;
        procedure WriteDateTime(ACell: PCell; AValue: TDateTime;
          ANumFormat: TsNumberFormat; ANumFormatStr: String = ''); overload;
        function WriteDateTime(ARow, ACol: Cardinal; AValue: TDateTime;
          ANumFormatStr: String): PCell; overload;
        procedure WriteDateTime(ACell: PCell; AValue: TDateTime;
          ANumFormatStr: String); overload;
    
        { Запись числовых значений } 
        function WriteNumber(ARow, ACol: Cardinal; ANumber: double): PCell; overload;
        procedure WriteNumber(ACell: PCell; ANumber: Double); overload;
        function WriteNumber(ARow, ACol: Cardinal; ANumber: double;
          ANumFormat: TsNumberFormat; ADecimals: Byte = 2): PCell; overload;
        procedure WriteNumber(ACell: PCell; ANumber: Double;
          ANumFormat: TsNumberFormat; ADecimals: Byte = 2); overload;
        function WriteNumber(ARow, ACol: Cardinal; ANumber: double;
          ANumFormat: TsNumberFormat; ANumFormatString: String): PCell; overload;
        procedure WriteNumber(ACell: PCell; ANumber: Double;
          ANumFormat: TsNumberFormat; ANumFormatString: String); overload;
    
        { Запись строковых значений }
        function WriteText(ARow, ACol: Cardinal; AText: ansistring;
          ARichTextParams: TsRichTextParams = nil): PCell; overload;
        procedure WriteText(ACell: PCell; AText: String;
          ARichTextparams: TsRichTextParams = nil); overload;
    
        // старые методы строк «WriteUTF8Text» сейчас устарели
        ...
    

Некоторые из этих методов существуют в перегруженных версиях, в которых параметры форматирования ячейки могут быть добавлены вместе со значением ячейки. Соответственно, для записи также существует несколько методов `worksheet` для чтения значений ячеек: 
    
    
    type
      TsWorksheet = class
        ...
        { Чтение содержимого ячейки как строки }
        function  ReadAsText(ARow, ACol: Cardinal): string; overload;
        function  ReadAsText(ACell: PCell): string; overload;
        function  ReadAsText(ACell: PCell; AFormatSettings: TFormatSettings): string; overload;
    
        { Чтение содержимого ячейки как числа }
        function  ReadAsNumber(ARow, ACol: Cardinal): Double; overload;
        function  ReadAsNumber(ACell: PCell): Double; overload;
        function  ReadNumericValue(ACell: PCell; out AValue: Double): Boolean;
    
        { Чтение содержимого ячейки как значения дата/время }
        function  ReadAsDateTime(ARow, ACol: Cardinal; out AResult: TDateTime): Boolean; overload;
        function  ReadAsDateTime(ACell: PCell; out AResult: TDateTime): Boolean; overload;
        ...
    

##### Индексирование формата ячейки

`FormatIndex` является индексом записи формата ячейки cell format record. Он описывает атрибуты форматирования ячейки. Эти записи собираются по внутреннему списку рабочей книги и определяются следующим образом: 
    
    
    type
      TsCellFormat = record
        FontIndex: Integer;
        TextRotation: TsTextRotation;
        HorAlignment: TsHorAlignment;
        VertAlignment: TsVertAlignment;
        Border: TsCellBorders;
        BorderStyles: TsCellBorderStyles;
        Background: TsFillPattern;
        NumberFormatIndex: Integer;
        NumberFormat: TsNumberFormat;
        NumberFormatStr: String;
        BiDiMode: TsBiDiMode;           // bdDefault, bdLTR {left-to-right}, bdRTL {right-to-left)
        Protection: TsCellProtection;   // cpLockCell, cpHideFormulas
        UsedFormattingFields: TsUsedFormattingFields;
          //uffTextRotation, uffFont, uffBold, uffBorder, uffBackground, uffNumberFormat, uffWordWrap, uffHorAlign, uffVertAlign, uffBiDi
      end;
    

  * `FontIndex`: текстовый шрифт, указывает индекс в списке шрифтов книги
  * `TextRotation`: указывает, будет ли текст ячейки написан горизонтально или вертикально
  * `HorAlignment`: выравнивание по левому краю, по горизонтали или по правому краю
  * `VertAlignment`: верхний, нижний или вертикально центрированный текст
  * `Border`: набор флагов, указывающий, что - если установлено - линия границы нарисована в левом, верхнем, правом или нижнем краях ячеек. Линии рисуются в соответствии с `BorderStyles`, которые определяют линейность и цвет границы.
  * `Фон`: запись, определяющая фоновое заполнение ячейки (стиль паттерна, цвет паттерна и цвет фона - см. главу фон ячейки ниже).
  * `NumberFormat` и `NumberFormatStr` указывает способ форматирования числа или даты/времени (например, числа десятичных знаков, длинного или короткого формата даты и т.д.).
  * При форматировании ячейки учитываются только те атрибуты формата, для которых установлен флаг в `UsedFormattingFields`. Если флаг не включен, соответствующий атрибут игнорируется и заменяется значением по умолчанию.



  
Для указания формата для данной ячейки вызывается соответствующий метод рабочего листа `WriteXXXX`, для получения вызова формата `ReadXXXX`. Эти методы обычно получают указатель на ячейку в качестве параметра, но также есть перегруженные версии, которые принимают индекс строки и столбца. Более того, стили форматирования также могут быть применены непосредственно к ячейке с помощью вспомогательного средства записи, реализованного в модуле _fpsCell_. 

См. форматирование ячейки ниже для [получения] более подробного описания. 

#### Столбцы и строки

Записи столбцов и строк добавляются для каждого столбца и строки с размером, отличным от значения по умолчанию: 
    
    
    type
      TCol = record
        Col: Cardinal;
        Width: Single;
        ColWidthType: TsColWidthType;     // = (cwtDefault, cwtCustom)
        FormatIndex: Integer;
      end;
      PCol = ^TCol;
    
      TRow = record
        Row: Cardinal;
        Height: Single;
        RowHeightType: TsRowHeightType;   // = (rhtDefault, rhtCustom, rhtAuto)
        FormatIndex: Integer;
      end;
      PRow = ^TRow;
    

[Свойства] **Column widths** и **row heights** могут быть указаны в различных единицах, определенных типом `TsSizeUnits = (suChars, suLines, suMillimeters, suCentimeters, suPoints, suInches)`. `suChars` относится к счету символов `0`, вписывающихся в ширину столбца, - таким образом Excel определяет ширину столбца. `suLines` \- количество линий, вписывающихся в высоту строки. Оба устройства основаны на размере символа шрифта по умолчанию для книги. Другие единицы - условные физические единицы длины (1 см = 10 мм, 1 дюйм = 25,4 мм = 72 пт). Принимаются дробные значения. Рабочая книга и рабочие листы хранят длины внутри в миллиметрах (`MyWorkbook.Units`). 

Приложения Office обычно настраивают высоту строки автоматически в соответствии с шрифтом или вращением текста [внутри] содержимого ячейки. Этот случай идентифицируется с помощью `RowHeightType`, имеющего значение `rhtAuto`. Поскольку рабочий лист не может рассчитывать размер текста очень точно, автоматические высоты строк не записываются в FPSpreadsheet; они заменяются на **default row height**. Высота строки по умолчанию также используется, если строка пуста, т.е. не содержит никаких ячеек данных. Его значение можно изменить, вызвав `WriteDefaultRowHeight()` или используя свойство `DefaultRowHeight`. В `WriteDefaultRowHeight` единицы должны быть указаны в `DefaultRowHeight`, [где] они считаются линиями. Аналогично, **default column width** может быть задана с помощью `WriteDefaultColWidth()` или свойством `DefaultColWidth` (в символах). 

Для того, чтобы перехватить [установку] высоты строк автоматически и по умолчанию, вызовите метод `WriteRowHeight()`. Эти записи строк идентифицируются с помощью `RowHeightType`, имеющего значение `rhtCustom`. Точно так же ширина столбцов может быть установлена на определенное значение вызовом `WriteColWidth()`. `ColWidthType` этих столбцов `cwtCustom`. 

Высоту/ширину конкретной строки/столбца можно получить с помощью методов `GetRowHeight` или `GetColHeight`. Обратите внимание, что эти методы возвращают ширину строк/столбцов по умолчанию, если нет записей `TRow`/`TCol`. 
    
    
    type TsWorksheet = class
      ...
      { Установка высоты строки }
      procedure WriteRowHeight(ARowIndex: Cardinal; AHeight: Single; AUnits: TsSizeUnits);
      { Установка ширины столбца }
      procedure WriteColWidth(AColIndex: Cardinal; AWidth: Single; AUnits: TsSizeUnits);
      { Установка высоты строки по умолчанию }
      procedure WriteDefaultRowHeight(AHeight: Single; AUnits: TsSizeUnits);
      {Установка ширины столбца по умолчанию }
      procedure WriteDefaultColWidth(AWidth: Single; AUnits: TsSizeUnits);
    
      { Возврат высоты строки }
      function GetRowHeight(ARowIndex: Cardinal; AUnits: TsSizeUnits): Single;
      { Возврат ширины столбца }
      function GetColWidth(AColIndex: Cardinal; AUnits: TsSizeUnits): Single;
      { Возврат высоты строки по умолчанию }
      function ReadDefaultRowHeight(AUnits: TsSizeUnits): Single;
      { Возврат ширины столбца по умолчанию }
      function ReadDefaultColWidth(AUnits: TsSizeUnits): Single;
    
      property DefaultRowHeight: Single;  // в линиях
      property DefaultColWidht: Single;   // в символах
    

Существуют также перегруженные версии этих методов, для которых не требуется параметр `AUnits`. В этом случае высота строк определяются в терминах количества строк, а ширина столбцов определяется в терминах количества символов. Обратите внимание, что эти варианты из предыдущих версий и теперь устарели. 

Элемент `FormatIndex` формата строк и столбцов применяется ко всей строке или столбцу. Как и в ячейках, эти форматы хранятся в виде записей `TsCellFormat` во внутреннем списке книг. **Row and column formats** в основном применяются к пустым ячейкам, но если добавлена новая ячейка, она автоматически получит формат строки или столбца. (Если обе строки и столбец имеют разные форматы, тогда будет использоваться формат строки). 

### Формулы

Два вида формул поддерживаются FPSpreadsheet: 

  * **Строковые формулы** : написаны в строковом [формате], как в офисных приложениях, например `"=ROUND(A1+B1,0)"`. Используются внутри файлов Open/LibreOffice и Excel _.xlsx_.
  * **Формулы RPN** [(польской нотации)] используются внутри [приложения] двоичными файлами Excel _.xls_. Они написаны в Reverse Polish Notation (RPN)[«Обратной польской нотации»], например: `A1, B1, Add, 0, ROUND`. Если электронная таблица, содержащая формулы, должна быть сохранена в двоичном формате Excel, необходимые формулы RPN генерируются автоматически.



FPSpreadsheet может [производить] преобразования между формулами строк и RPN. Формулы в обоих типах могут быть рассчитаны. 

В более старых версиях FPSpreadsheet формулы сохранялись непосредственно в записи ячейки. Это было сделано, чтобы получить анализируемые формулы для более быстрого вычисления. Формулы хранятся в дереве `Formulas` рабочего листа. Запись формулы содержит индекс строки и столбца ячейки, к которой относится формула, строковое представление формулы, а также дерево парсера для быстрой оценки. 

FPSpreadsheet поддерживает большинство формул, предоставляемых обычными приложениями электронных таблиц. Однако при чтении файла, созданного этими приложениями, всегда существует вероятность того, что [в нем] содержится не поддерживаемая формула. Чтобы избежать сбоев fpspreadsheet, чтение формул по умолчанию отключено; ячейка отображает только результат формулы, написанной приложением Office. Чтобы активировать чтение формул, перед открытием файла добавьте элемент `boReadformulas` в [свойстве] `Options` книги. Если в этом случае возникает ошибка, читатель [(парсер)] обычно обходит исключение, записывает сообщение об исключении в журнал ошибок книги и продолжает чтение. Если вы хотите, чтобы чтение останавливалось, вы должны добавить `boAbortReadingOnFormulaError` в `Options` книги. 

Формулы могут ссылаться на данные в других листах той же книги ("3d формулы"), применяя синтаксис Excel (см. ниже). Внешние ссылки на электронные таблицы в других файлах не поддерживаются. 

Вычисление формул обычно не требуется, когда файл написан [при помощи] FPSpreadsheet для открытия в приложении Office, потому что он автоматически вычисляет результаты формулы. Однако, если один и тот же файл открывается в приложении, основанном на FPSpreadsheet, вычисленные ячейки будут пустыми, потому что формулы по умолчанию автоматически не вычисляются. Чтобы активировать вычисление формул перед написанием электронной таблицы в файл, вы должны добавить опцию `boCalcBeforeSaving` в [свойство] `Options` книги. 

Если FPSpreadshet используется в интерактивном приложении (таком, как демка «spready», расположенной в папке _examples_ в [корневом каталоге] установки FPSpreadsheet), желательно автоматически вычислять формулы, когда пользовательские строки или значения ячеек изменяются пользователем. Этого можно достичь с помощью опции `boAutoCalc` в [свойстве] `Options` книги. 

Таким образом, наиболее общая установка в отношении формул [такова] 
    
    
      MyWorkbook.Options := MyWorkbook.Options + [boReadFormulas, boCalcBeforeSaving, boAutoCalc];
    

Вычисление формул можно запускать вручную, вызывая метод `CalcFormulas` рабочего листа или рабочей книги. Последнее абсолютно необходимо, когда книга содержит 3d формулы, где результат одной ячейки может влиять на ячейки в других листах. Если есть формулы [, используемые] только внутри рабочего листа, то достаточно [использование свойства] `CalcFormulas` рабочего листа. 

#### Строковые формулы

Строковые формулы записываются так же, как и в приложениях Office. Метод рабочей таблицы для создания строковой формулы `WriteFormula`: 
    
    
    var
      MyWorksheet: TsWorksheet;
    //...
      MyWorksheet.WriteFormula(0, 1, '=ROUND(A1+B2+1.215,0)');
      // По умолчанию используйте десятичную точку и запятую как разделитель списков!
    

Несколько замечаний по **syntax** : 

  * Ведущий символ `=`, который идентифицирует формулы в приложениях Office, здесь не является абсолютно необходимым и может быть удален. Формула хранится в записи ячейки без нее.
  * Случай с именем формулы игнорируется.
  * Пробелы могут быть добавлены для лучшей читаемости, но при сохранении они будут потеряны.
  * Строки должны быть заключены в двойные кавычки.
  * Угловые точки диапазона ячеек должны быть разделены двоеточием (":"), напр. `A1:C3`. Неопределенные диапазоны будут переупорядочены при анализе формулы, т.е. `C3:A1` станет `A1:C3`.
  * Ссылки на другие рабочие листы должны следовать синтаксису Excel, который разделяет имя листа и адрес ячейки на "!". Например, одну ячейку можно связать с помощью `Sheet1!A1`. Ряд листов должен быть помещен перед ячейкой или диапазоном ячейки, напр. `Sheet1:Sheet2!A1:C3`. Обратите внимание, что синтаксис Open/LibreOffice с разделителем и ссылкой на угловые точки 3d-блока (т.е. `Sheet1.A1:Sheet2.C3`) не поддерживается. Также обратите внимание на то, что рабочий(-ие) лист(-ы), на которые ссылается [добавляемая] формула, должна(-ы) существовать на момент добавления формулы; в противном случае ссылка будет заменена кодом ошибки `#REF!`, и формула не будет использоваться, даже если недостающий лист будет добавлен позже.
  * Обычно числа с плавающей запятой должны вводиться с точкой в качестве десятичного разделителя, а запятая должна использоваться для разделения аргументов функции.
  * Установка параметра `ALocalized` метода рабочего листа `WriteFormula` в `TRUE` не является обязательной, однако позволяет использовать локализованные десятичные [разделители] и разделители списков, взятые из `FormatSettings` рабочего листа - см. демо _spready_.


    
    
    var
      MyWorksheet: TsWorksheet;
    //...
      MyWorksheet.WriteFormula(0, 1, '=ROUND(A1+B2+1,215;0)', true);
      // Из-за "true" парсер формулы воспринимает запятую как десятичный разделитель и
      // точку с запятой в качестве разделителя списков, если формат FormatSettings книги yстановлен как здесь.
    

Use the worksheet methods `ReadFormula` or `ReadFormulaAsString` to retrieve the string formula assigned to the cell. The pointer to the cell must be given as a parameter. The latter function accepts an additional (boolean) parameter `ALocalized` to use the decimal and list separators of the workbook's `FormatSettings` for creation of the formula string. 

Используйте методы `ReadFormula` или `ReadFormulaAsString` рабочего листа для получения строковой формулы, присвоенной ячейке. Указатель на ячейку должен быть определен как параметр. Последняя функция принимает дополнительный (логический) параметр `ALocalized` для использования десятичных [разделителей] и разделителей списков свойства `FormatSettings` рабочей книги для создания строки формулы. 
    
    
    var
      MyWorksheet: TsWorksheet;
      cell: PCell;
    //...
      cell := MyWorksheet.FindCell(0, 1);
      WriteLn('Формула во внутреннем формате - ', MyworkSheet.ReadFormula(cell));
      WriteLn('Локализованная формула - ', MyWorksheet.ReadFormulaAsString(cell, true));
    //-------------------------------------------------------------------
    // Предыдущий пример приведет к следующему результату
    Формула во внутреннем формате - ROUND(A1+B2+1.215,0)
    Локализованная формула - ROUND(A1+B2+1,215;0)
    

#### Формулы RPN (Обратной польской нотации)

На уровне приложения в основном используются строковые формулы, а формулы RPN имеют мало практического значения. Поэтому документация формул RPN была удалена из этой основной вики FPSpreadsheet и может быть найдена в статье ["Формулы RPN в FPSpreadsheet"](<../en/RPN_Formulas_in_FPSpreadsheet.md> "RPN Formulas in FPSpreadsheet"). 

#### Общие формулы и формулы массива

  * **Общие формулы** поддерживаются только для чтения (из файлов Excel).
  * **Формулы массива** в настоящее время не поддерживаются.



#### Список встроенных формул

FPSpreadsheet поддерживает более 80 встроенных формул. Чтобы не взорвать эту страницу вики, большая часть документации по этим формулам была перенесена в [отдельный документ "Список формул"](<../en/FPSpreadsheet__List_of_formulas.md> "FPSpreadsheet: List of formulas"). 

Чтобы узнать больше о доступных функциях, посмотрите файл _testcases_calcrpnformula.inc_ в папке _tests_ [в корневом каталоге] установки FPSpreadsheet, где каждая функция включена с хотя бы одним примером. 

#### Расширение FPSpreadsheet по пользовательским формулам

Хотя встроенные формулы охватывают большинство приложений, может потребоваться доступ к формуле, доступной в приложении Office, но не в таблице FPSpreadsheet. По этой причине библиотека поддерживает механизм регистрации, который позволяет добавлять пользовательские функции к электронным таблицам. Это можно сделать, вызвав процедуру `RegisterFunction` из модуля _fpsExprParser_ : 
    
    
    procedure RegisterFunction(const AName: ShortString; const AResultType: Char;
      const AParamTypes: String; const AExcelCode: Integer; ACallBack: TsExprFunctionCallBack); overload;
    
    procedure RegisterFunction(const AName: ShortString; const AResultType: Char;
      const AParamTypes: String; const AExcelCode: Integer; ACallBack: TsExprEventCallBack); overload;
    

  * `AName` указывает имя, под которым функция будет вызываться в электронной таблице. Он должен соответствовать имени формулы в приложении Office.
  * `AResultType` \- это символ, который идентифицирует тип данных результата функции: 
    * `'F'` \- число с плавающей запятой
    * `'I'` \- целое число
    * `'D'` \- дата / время
    * `'B'` \- boolean
    * `'S'` \- строка
  * `AParamTypes` \- это строка, в которой каждый символ идентифицирует тип данных соответствующего аргумента. В дополнение к приведенному выше списку могут использоваться следующие символы: 
    * `'?'` \- любой тип
    * `'+'` \- должен быть последний символ. Это означает, что предыдущий символ повторяется бесконечно. Это позволяет подсчитать произвольный аргумент. Обратите внимание, однако, что Excel поддерживает только до 30 аргументов.
    * нижний регистр `'f', 'i', 'd', 'b', 's' ` указывают необязательные параметры описанного выше типа. Конечно, символы верхнего регистра не могут следовать за строчными символами.
  * `AExcelCode` \- это идентификатор функции в файлах xls. См. «Документация OpenOffice формата файлов Microsoft Excel», раздел 3.11, для списка.
  * `ACallback` определяет, какую функцию вызывает FPSpreadsheet для вычисления формулы. Это может быть процедура или обработчик событий.


    
    
    type
      TsExprFunctionCallBack = procedure (var Result: TsExpressionResult; const Args: TsExprParameterArray);
      TsExprFunctionEvent = procedure (var Result: TsExpressionResult; const Args: TsExprParameterArray) of object;
    

`TsExpressionResult` \- это вариантная запись, содержащая данные результата или аргумента нескольких типов: 
    
    
    type
      TsResultType = (rtEmpty, rtBoolean, rtInteger, rtFloat, rtDateTime, rtString,
        rtCell, rtCellRange, rtError, rtAny);
    
      TsExpressionResult = record
        Worksheet       : TsWorksheet;
        ResString       : String;
        case ResultType : TsResultType of
          rtEmpty       : ();
          rtError       : (ResError       : TsErrorValue);
          rtBoolean     : (ResBoolean     : Boolean);
          rtInteger     : (ResInteger     : Int64);
          rtFloat       : (ResFloat       : TsExprFloat);
          rtDateTime    : (ResDateTime    : TDatetime);
          rtCell        : (ResRow, ResCol : Cardinal);
          rtCellRange   : (ResCellRange   : TsCellRange);
          rtString      : ();
      end;
    
      TsExprParameterArray = array of TsExpressionResult;
    

В качестве примера мы приводим здесь код для формулы `CONCATENATE()`, которая объединяет две или более строки: 
    
    
    const
      INT_EXCEL_SHEET_FUNC_CONCATENATE = 336;  
    ...
      RegisterFunction('CONCATENATE', 'S', 'S+', INT_EXCEL_SHEET_FUNC_CONCATENATE, @fpsCONCATENATE);
    
    procedure fpsCONCATENATE(var Result: TsExpressionResult; const Args: TsExprParameterArray);
    // CONCATENATE( text1, text2, ... text_n )
    var
      s: String;
      i: Integer;
    begin
      s := '';
      for i:=0 to Length(Args)-1 do
      begin
        if Args[i].ResultType = rtError then
        begin
          Result := ErrorResult(Args[i].ResError);
          exit;
        end;
        s := s + ArgToString(Args[i]);
        // "ArgToString" упрощает получение строки из TsExpressionResult, 
        // поскольку строка может содержаться в ResString и в полях ResCell.
        // Такая функция существует для каждого базового типа данных.
      end;
      Result := StringResult(s);
      //"StringResult" хранит строку s в поле ResString объекта 
      // TsExpressionResult и устанавливает для ResultType значение rtString.
      // Такая функция существует для каждого базового типа данных.
    end;
    

В папке _examples/other_ в [корневом каталоге] установки FPSpreadsheet имеется разработанный пример (_demo_formula_func.pas_). В этой демке добавлены четыре финансовые функции (`FV()`, `PV()`, `PMT()`, `RATE()`) в FPSpreadsheet. 

#### Неподдерживаемые формулы

Иногда требуется создавать файлы для приложений Office с формулами, не поддерживаемыми [компонентом] FPSpreadsheet. Это возможно в некоторой степени, когда опция `boIgnoreFormulas` рабочей книги активна. Тогда любая произвольная формула может быть записана в ячейку, а формула не проверяется и не оценивается. [Рабочая] книга может быть записана в файл .ods или .xlsx. Старый формат файла xls не может быть использован, потому что формулу необходимо проанализировать для создания необходимой формулы RPN. 

В папке _examples/other_ вы можете найти образец проекта _demo_ignore_formula_ , который создает файл ods со ссылками на другой файл данных. Внешние ссылки обычно не поддерживаются FPSpreadsheet, поэтому должны быть использованы обходные методы [их] игнорирования. Обратите внимание, что этот пример не работает с xlsx, потому что Excel записывает информацию о внешних ссылках для разделения XML-файлов в контейнере xlsx. 

### Форматирование ячеек

#### Форматы числа и даты/времени

Значения чисел и даты/времени могут отображаться в различных форматах. В FPSpreadsheet это может быть достигнуто двумя способами: 

  * использование **встроенных форматов чисел** путем указания значения для `NumberFormat` ячейки
  * используя пользовательскую строку **format**.



Форматы чисел могут быть указаны с помощью этих методов [рабочего] листа: 
    
    
    type
      TsWorksheet = class
      public
        // Установка только числовых форматов
        function WriteNumberFormat(ARow, ACol: Cardinal; ANumberFormat: TsNumberFormat;
          const AFormatString: String = ''): PCell; overload;
        procedure WriteNumberFormat(ACell: PCell; ANumberFormat: TsNumberFormat;
          const AFormatString: String = ''); overload;  
    
        function WriteNumberFormat(ARow, ACol: Cardinal; ANumFormat: TsNumberFormat;
          ADecimals: Integer; ACurrencySymbol: String = ''; APosCurrFormat: Integer = -1;
          ANegCurrFormat: Integer = -1): PCell; overload;
        procedure WriteNumberFormat(ACell: PCell; ANumFormat: TsNumberFormat;
          ADecimals: Integer; ACurrencySymbol: String = '';
          APosCurrFormat: Integer = -1; ANegCurrFormat: Integer = -1); overload;
    
        function WriteFractionFormat(ARow, ACol: Cardinal; AMixedFraction: Boolean;
          ANumeratorDigits, ADenominatorDigits: Integer): PCell; overload;
        procedure WriteFractionFormat(ACell: PCell; AMixedFraction: Boolean;
          ANumeratorDigits, ADenominatorDigits: Integer); overload;
    
        // Установка только формата дата/время  
        function WriteDateTimeFormat(ARow, ACol: Cardinal; ANumberFormat: TsNumberFormat;
          const AFormatString: String = ''): PCell; overload;
        procedure WriteDateTimeFormat(ACell: PCell; ANumberFormat: TsNumberFormat;
          const AFormatString: String = ''); overload;
    
    
        // Установка значений ячеек и числовых форматов за один вызов
    
        // числовые значения
        function WriteNumber(ARow, ACol: Cardinal; ANumber: double;
          AFormat: TsNumberFormat = nfGeneral; ADecimals: Byte = 2;
          ACurrencySymbol: String = ''): PCell; overload;
        procedure WriteNumber(ACell: PCell; ANumber: Double; AFormat: TsNumberFormat = nfGeneral;
          ADecimals: Byte = 2; ACurrencySymbol: String = ''); overload;
        function WriteNumber(ARow, ACol: Cardinal; ANumber: double;
          AFormat: TsNumberFormat; AFormatString: String): PCell; overload;
        procedure WriteNumber(ACell: PCell; ANumber: Double;
          AFormat: TsNumberFormat; AFormatString: String); overload;   
    
        // значения дата/время
        function WriteDateTime(ARow, ACol: Cardinal; AValue: TDateTime;
          AFormat: TsNumberFormat = nfShortDateTime; AFormatStr: String = ''): PCell; overload;
        procedure WriteDateTime(ACell: PCell; AValue: TDateTime;
          AFormat: TsNumberFormat = nfShortDateTime; AFormatStr: String = ''); overload;
        function WriteDateTime(ARow, ACol: Cardinal; AValue: TDateTime;
          AFormatStr: String): PCell; overload;
        procedure WriteDateTime(ACell: PCell; AValue: TDateTime;
          AFormatStr: String); overload;  
    
        // значения денежной валюты
        function WriteCurrency(ARow, ACol: Cardinal; AValue: Double;
          AFormat: TsNumberFormat = nfCurrency; ADecimals: Integer = 2;
          ACurrencySymbol: String = '?'; APosCurrFormat: Integer = -1;
          ANegCurrFormat: Integer = -1): PCell; overload;
        procedure WriteCurrency(ACell: PCell; AValue: Double;
          AFormat: TsNumberFormat = nfCurrency; ADecimals: Integer = -1;
          ACurrencySymbol: String = '?'; APosCurrFormat: Integer = -1;
          ANegCurrFormat: Integer = -1); overload;
        function WriteCurrency(ARow, ACol: Cardinal; AValue: Double;
          AFormat: TsNumberFormat; AFormatString: String): PCell; overload;
        procedure WriteCurrency(ACell: PCell; AValue: Double;
          AFormat: TsNumberFormat; AFormatString: String); overload;     
      ...
    

##### Встроенные форматы чисел

**Встроенные форматы** определяются перечислением `TsNumberFormat`. Несмотря на свое название, элементы охватывают как числа, так и дату/время: 
    
    
    type
      TsNumberFormat = (
        // универсальный для всех номеров
        nfGeneral,
        // числа
        nfFixed, nfFixedTh, nfExp, nfPercentage, nfFraction,
        // денежная валюта
        nfCurrency, nfCurrencyRed, 
        // дата и время
        nfShortDateTime, nfShortDate, nfLongDate, nfShortTime, nfLongTime,
        nfShortTimeAM, nfLongTimeAM, nfDayMonth, nfMonthYear, nfTimeInterval,
        // другие (использующие строку формата)
        nfCustom);
    

  * **nfGeneral** соответствует форматированию по умолчанию, показывающему как можно больше десятичных знаков [после запятой] (число `3.141592654 ` не изменится.)
  * **nfFixed** ограничивает десятичные знаки. Число знаков после запятой должно быть указано при вызове `WriteNumber`. Пример: с 2 десятичными знаками число `3.141592654` становится `3.14`.
  * **nfFixedTh** : аналогично `nfFixed `, но добавляет разделитель тысячных [разрядов], когда число отображается как строка: число `3.141592654` останется таким же, как и в предыдущем примере, потому что оно слишком мало, чтобы показать разделитель тысячных [разрядов]. Но число `314159.2654` станет `314,159.26`, для 2 десятичных знаков.
  * **nfExp** выбирает экспоненциальное представление, т.е. разделяет экспоненту. Параметр `ADecimals` в `WriteNumber` определяет, сколько десятичных знаков используется. (Число `3.141592654` становится `3.14E+00` в случае двух десятичных знаков).
  * **nfPercentage** отображает число в процентах. Это означает, что значение умножается на 100, а знак процента добавляется. Опять же, укажите в `ADecimals`, сколько десятичных знаков должно быть показано. (Число `3.141592654` отображается как `314.92%`, в случае 2 десятичных знаков).
  * **nfFraction** представляет число в виде доли. Подробности (смешанная фракция?, максимальное количество цифр для числителя или знаменателя) для могут быть указаны в методе рабочего листа `WriteFractionFormat`.
  * **nfCurrency** отображает номер вместе с символом валюты, и существуют специальные правила, как отображать отрицательные значения (в скобках или знак минус до или после номера). Формат форматирования рабочей книги используется для определения знака валюты и способа отображения номеров (`FormatSettings.CurrencyString` для символа валюты, `FormatSettings.CurrencyFormat` для положительных, `FormatSettings.NegCurrFormat` для отрицательных значений). Эти настройки можно переопределить, указав их непосредственно в вызове `WriteCurrency`.
  * **nfCurrendyRed** аналогично `nfCurrency`, кроме того, отрицательные значения отображаются красным цветом.
  * **nfShortDateTime** представляет DateTimeValue ячейки в 'коротком формате даты/времени", т.е. дни + двухзначный [номер] месяца + двухзначный год + часы + минуты, без секунд. Порядок частей даты берется из `FormatSettings` рабочей книги. Это относится также к другим форматам даты и времени.
  * **nfShortDate** создает строку даты, показывающую день + двухзначный [номер] месяца + двузначный год
  * **nfShortTime** создает строку времени, показывающую часы + минуты.
  * **nfLongTime** , аналогично [**nfShortTime**], но включает и секунды
  * **nfShortTimeAM** , аналогично `nfShortTime`, но использует формат времени AM/PM, т.е. часы идут до 12, а AM или PM добавляются для указания [половины дня] утром или вечером/пополудни.
  * **nfLongTimeAM** , аналогично `nfShortTimeAM`, но включает секунды
  * **nfTimeInterval** , аналогично `nfLongTime`, но может быть больше 24 часов. Интервал может также выражаться в минутах или секундах, если используются строки формата `[n]:ss` или `[s]`, соответственно.
  * **nfCustom** позволяет указать выделенную строку форматирования.



Как уже отмечалось, в рабочей книге есть свойство `FormatSettings`, которое предоставляет дополнительную информацию для управления окончательным форматированием. Это, по существу, копия `DefaultFormatSettings`, объявленная в модуле _sysutils_ (элементы `LongDateFormat` и `ShortDateFormat` слегка изменены для лучшего соответствия настройки по умолчанию в основных приложениях электронных таблиц). Основная цель `FormatSettings` \- добавить простой способ локализации в числовые форматы. 

##### Строки формата чисел

В дополнение к этим заранее определенным форматам более специализированное форматирование может быть достигнуто с использованием константы формата `nfCustom` вместе со специальной **строкой формата**. Строка формата построена в соответствии с синтаксисом Excel, который близок к синтаксису команд `FormatFloat` и `FormatDateTime` команды fpc (см. интерактивную справку для этих функций). 

Вот основной список используемых символов: 

Символ | Определение | Строка формата: число --> Выходная строка   
---|---|---  
`General` | Отображает все десятичные знаки числа | `'General'`:  
1.2345678 --> `'1.2345678'`  
`0` | Отображает не значащие нули, если число имеет меньше знаков, чем [количество] нулей в формате. Если [строка формата] используется для десятичных знаков, то число округляется до такого количества знаков после запятой, что и 0. | `'000'`: 1 --> `'001'`  
`'0.0'`: 1 --> `'1.0'`  
`'0.0'`: 1.2345678 --> `'1.2'`  
`*` | Как [и для] "0" выше, но не отображает не значащие нули. | `'0.*'`: 1 --> `'1.'`  
`'0.*'`: 1.2345678 --> `'1.2'`  
`?` | Как [и для] "0" выше, но не существенные нули заменяются пробелами. Хорошо для выравнивания десятичных точек и фракций |  `'??0'`: 1 --> `' 1'`  
`'0.0??'`: 1 --> `'1.0 '`  
`.` | Десятичный разделитель; будет заменено значением, используемым в `DecimalSeparator` [параметра] `FormatSettings` рабочей книги | `'0.00'`: 8.9 --> `'8.90'`  
`,` | Разделитель тысячных [разрядов]; будет заменено значением, используемым в `ThousandSeparator` [параметра] `FormatSettings` рабочей книги. Если [сепаратор указан] в конце последовательности форматирования числа, [то] отображаемое значение делится на 1000. | `'#,##0.00'`: 1200 --> `'1,200.00'`  
`'0.00,'`: 1200 --> `'1.20'`  
`E+`, `e+` | Отображает число в экспоненциальном формате. Цифры, используемые для показателя, определяются числом нулей, добавленным этим символом. Знак экспонента показан для положительных и отрицательных показателей. | `'0.00E+00'`: 1200 --> `1.20E+03`  
`E-`, `e-` | Отображает число в экспоненциальном формате. Цифры, используемые для показателя, определяются числом нулей, добавленным этим символом. Знак экспонента показан только для отрицательных показателей. | `'0.00e-000'`: 1200 --> `1.20e003`  
`%` | Отображает число как «процент», т.е. число умножается на 100 и добавляется знак `%`. | `'0.0%'`: 0.75 --> `75.0%`  
`/` | Этот символ имеет два значения: если ячейка представляет собой "число", то косая черта означает форматирование как дробь, числитель и знаменатель должны следовать зарезервированному месту. Если ячейка представляет "дату/время", то косая черта обозначает разделитель даты, который будет заменен на `DateSeparator` в [параметре] `FormatSettings` рабочего листа | `'#/#'`: 1.5 --> `'3/2'`  
`'# #/#'`: 1.5 --> `'1 1/2'`  
`'# #/16'`: 1.5 --> `'1 8/16'`  
также: см. примеры даты/времени ниже   
`:` | Разделитель между часами, минутами и секундами значения даты/времени. Будет заменен [на] `TimeSeparator` [параметра] `FormatSettings` рабочей книги. | смотри пример ниже   
`yyyy` | Зарезервированное место для года значения даты/времени. Год отображается как четырехзначное число. | `'yyyy/mm/dd'`:  
Jan 3, 2012 --> `'2012-01-03'`  
В этом примере `DateSeparator` является символом тире (`-`).   
`yy` | Зарезервированное место для года значения даты/времени. Год отображается как двузначное число. | `'yy/mm/dd'`:  
Jan 3, 2012 --> `'12-01-03'`  
`m` | Зарезервированное место для месяца значения даты/времени. Месяц отображается как число без дополнительных цифр.  
Обратите внимание, что код `m` также можно интерпретировать как «минуты» значения времени (см. ниже). | `'yyyy/m/dd'`:  
Jan 3, 2012 --> `'2012-1-03'`  
`mm` | Зарезервированное место для месяца значения даты/времени. Месяц отображается как двузначное число, то есть начальный ноль добавляется с января по сентябрь.   
Обратите внимание, что код `mm` также может быть интерпретирован как "минуты" (см. ниже). | `'yyyy/mm/dd'`:  
Jan 3, 2012 --> `'2012-01-03'`  
`mmm` | Зарезервированное место для месяца значения даты/времени. Месяц отображается [в виде] аббревиатуры. | `'yyyy/mmm/dd'`:  
Jan 3, 2012 --> `'2012-Jan-03'`  
`mmmm` | Зарезервированное место для месяца значения даты/времени. Месяц отображается полным именем.   
| `'yyyy/mmm/dd'`:  
Jan 3, 2012 --> `'2012-January-03'`  
`d` | Зарезервированное место дня значения даты/времени, которое будет отображаться в виде числа. День отображается как простое число без добавления начального нуля. | `'yyyy/mm/d'`:  
Jan 3, 2012 --> `'2012-01-3'`  
`dd` | Зарезервированное место дня значения даты/времени, которое будет отображаться в виде числа. `dd` добавляет [лидирующий] ноль в однозначные числа. | `'yyyy/mm/dd'`:  
Jan 3, 2012 --> `'2012-01-03'`  
`ddd` | Зарезервированное место дня значения даты/времени. День отображается в виде аббревиатуры. | `'dddd, yyyy/mm/ddd'`:  
Jan 03, 2012 --> `'Tue 2012-01-03'`  
`dddd` | Зарезервированное место дня значения даты/времени. День отображается полным именем. | `'dddd, yyyy/mmmm/dd'`:  
Jan 03, 2012 --> `'Tuesday 2012-01-03'`  
`h` | Зарезервированное место часовой части значения даты/времени. Час отображается как простое число без добавления начального нуля. | `'h:mm'`:  
0.25 --> `'6:00'`  
`hh` | Зарезервированное место часовой части значения даты/времени. Час отображается с начальным нулем, если час меньше 10. | `'hh:mm'`:  
0.25 --> `'06:00'`  
`[hh]`, или `[h]` | Отображает прошедшее время, так что часовая часть может стать больше 23 | `'[h]:mm'`:  
1.25 --> `'30:00'`  
`m` | Зарезервированное место минутной части значения даты/времени. Минуты показаны как простое число без добавления начального нуля. Заметим, что если коды `m` окружены символами даты (`y`, `d`), то они интерпретируются как "месяц". | `'h:m'`:  
0.25 --> `'6:0'`  
`mm` | Зарезервированное место минутной части значения даты/времени. Минуты с одной цифрой отображаются с начальным нулем. Заметим, что если код `mm` окружен символами даты (`y`, `d`), то он интерпретируется как "месяц". | `'h:mm'`:  
0.25 --> `'6:00'`  
`[mm]`, или `[m]` | Отображает прошедшее время так, что минутная часть может стать больше 59 | `'[mm]:ss'`:  
1.25 --> `'1800:00'`  
`s` | Зарезервированное место секундной части значения даты/времени. Секунды отображаются как простое число без добавления начального нуля. | `'hh:mm:s'`:  
0.25 --> `'06:00:0'`  
`ss` | Зарезервированное место секундной части значения даты/времени. Секунды с одной цифрой отображаются с начальным нулем. | `'hh:mm:ss'`:  
0.25 --> `'06:00:00'`  
`[ss]`, или `[s]` | Отображает прошедшее время так, что секундная часть может стать больше, чем 59 | `'[ss]'`:  
1.25 --> `'108000'`  
`AM/PM`, `am/pm`, `A/P`, или `a/p` | Отображает время в 12-часовом формате. | `'hh:mm:ss AM/PM'`:  
0.25 --> `'6:00:00 AM'`  
`"` | Текст, заключенный в кавычки, вставляется в форматированные строки буквально. | `'yyyy"/"mm"/"dd'`:  
Jan 3, 2012 --> `'2012/01/03'` (т. е. `/` не заменяется свойством `DateSeparator` рабочей книги).   
`\` | Следующий символ строки формата отображается в строке результата буквально. Сам `\` не отображается. |  `'yyyy\/mm\/dd'`:  
Jan 3, 2012 --> `'2012/01/03'`  
`;` | Строка формата может содержать до трех разделов, разделенных точкой с запятой. Первый раздел используется для положительных чисел, второй - для отрицательных чисел, а третий - для нулевых чисел. Если третий раздел отсутствует, то нулевое значение форматируется, как указано в первом разделе. Если второй раздел также отсутствует, то все значения форматируются в соответствии с первым разделом. | `'"#,##0"$"';-#,##0"$";"-"'`:  
1200 --> `'1,200$'`  
-1200 --> `'1,200$'`  
0 --> `'-'`  
`(`, и `)` | Иногда используется для значений валюты, указывающих отрицательные числа, вместо знака "минус" | `'#,##0"$";(#,##0)"$"'`:  
-1200 --> `'(1200)$'`  
`[red]` | Отформатированная строка отображается в указанном цвете. Вместо `[red]` вы можете использовать соответственно `[black]`, `[white]`, `[green]`, `[blue]`, `[magenta]`, `[yellow]`, или `[cyan]`. Часто используется для выделения отрицательных значений валюты. | `'"$" #,##0.00;[red]("$" #,##0.00)'`:  
-1200 --> '`($ 1200.00)`'   
  
#### Цвета

FPSpreadsheet поддерживает цвета для текста, фона ячейки и границ ячеек. Основные цвета EGA объявлены в модуле _fpstypes_ как константы: 

[![Predefined colors](https://wiki.freepascal.org/images/7/7b/DefaultPalette.png)](</File:DefaultPalette.png> "Predefined colors")
    
    
    type
      TsColor = DWORD;
    
    const
      scBlack       = $00000000;
      scWhite       = $00FFFFFF;
      scRed         = $000000FF;
      scGreen       = $0000FF00;
      scBlue        = $00FF0000;
      scYellow      = $0000FFFF;
      scMagenta     = $00FF00FF;
      scCyan        = $00FFFF00;
      scDarkRed     = $00000080;
      scDarkGreen   = $00008000;
      scDarkBlue    = $00800000;
      scOlive       = $00008080;
      scPurple      = $00800080;
      scTeal        = $00808000;
      scSilver      = $00C0C0C0;
      scGray        = $00808080;
      scGrey        = scGray; // переопределить, чтобы разрешить различное правописание
    
      // Идентификатор неопределенного цвета 
      scNotDefined  = $40000000;
    
      // Идентификатор прозрачного цвета 
      scTransparent = $20000000;
    

  
[Свойство] `TsColor` представляет значение **rgb [значение]** цвета, а для красных, зеленых и синих компонентов используется один байт. Результирующее число представлено обратным порядком байт (Little Endian Notation), т. е. сначала в памяти появляется значение красного [цвета]: `$00BBGGRR`. (Это напрямую совместимо с значениями цвета, как [оно] определено в модуле _graphics_.) 

Старший байт обычно равен нулю, но используется внутри для определения специальных значений цвета, например, для неопределенных или прозрачных цветов. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В более старых версиях цвета библиотеки были определены как индексы в цветовой палитре. **ЭТО НЕ БОЛЬШЕ РАБОТАЕТ**.

Модуль _fpsutils_ содержит некоторые полезные функции для **модификации цветов** : 

  * `function GetColorName(AColor: TsColor): String;`  
возвращает имя цветов, определенных выше, или строку, показывающую компоненты rgb для других цветов.
  * `function HighContrastColor(AColor: TsColor): TsColor;`  
возвращает `scBlack` для «яркого», `scWhite` для "темного" входного цвета.
  * `function TintedColor(AColor: TsColor; tint: Double): TsColor;`  
осветляет или затемняет цвет, применяя коэффициент `tint = -1 .. + 1`, где `-1` означает "100% затемнение", `+1` означает "100% осветление", а `0` означает "без изменений". Цветовой оттенок сохраняется.



#### Фон ячейки

Фон ячейки может быть заполнен предопределенными шаблонами, которые идентифицируются записью `TsFillPattern`:: 
    
    
    type
      TsFillPattern = record
        Style: TsFillStyle;   // заполняет шаблон стиля, как определено ниже
        FgColor: TsColor;     // цвет переднего плана заливки шаблона
        BgColor: TsColor;     // цвет фона заливки шаблона
      end;
    
      TsFillStyle = (fsNoFill, fsSolidFill, fsGray75, fsGray50, fsGray25, fsGray12, fsGray6,
        fsStripeHor, fsStripeVert, fsStripeDiagUp, fsStripeDiagDown,
        fsThinStripeHor, fsThinStripeVert, fsThinStripeDiagUp, fsThinStripeDiagDown,
        fsHatchDiag, fsThinHatchDiag, fsThickHatchDiag, fsThinHatchHor);
    

  * Используйте метод `WriteBackground` рабочего листа, чтобы назначить шаблон заполнения конкретной ячейке. Помимо адреса ячейки, этот метод требует тип заполнения шаблона (`TsFillStyle`) и цвета переднего плана и фона, значения [которых] указаны [свойством] `TsColor`.
  * Шаблон заполнения конкретной ячейки можно получить, вызвав метод `ReadBackground` рабочей книги.
  * Для достижения однородного цвета фона можно использовать упрощенный метод `WriteBackgroundColor`.
  * **Ограничения:**
    * Файлы OpenDocument поддерживают только однородные заливки. Цвет фона представляет собой смешивание переднего плана и фона rgb компонентов в соотношении, определяемом шаблоном заполнения.
    * Файлы BIFF2 поддерживают только 12,5% черно-белый затененный шаблон.


    
    
    type
      TsWorksheet = class
      public
        function WriteBackground(ARow, ACol: Cardinal; AStyle: TsFillStyle; APatternColor, ABackgroundColor: TsColor): PCell; overload;
        procedure WriteBackground(ACell: PCell; AStyle: TsFillStyle; APatternColor, ABackgroundColor: TsColor); overload;
    
        function WriteBackgroundColor(ARow, ACol: Cardinal; AColor: TsColor): PCell; overload;
        procedure WriteBackgroundColor(ACell: PCell; AColor: TsColor); overload;
    
        function ReadBackground(ACell: PCell): TsFillPattern;
        function ReadBackgroundColor(ACell: PCell): TsColor; overload;
        // ...
      end;
    
    var
      cell: PCell;
    ...
      // Пример 1: Присваивает шаблон тонких, горизонтальных, желтых полос на синем фоне, пустой ячейке A1 (строка 0, столбец 0)
      MyWorksheet.WriteBackground(0, 0, fsThinStripeHor, scYellow, scBlue);
    
      // Пример 2: Равномерный серый цвет фона ячейки B1 (строка 0, столбец 1), содержащий номер 3.14
      cell := MyWorksheet.WriteNumber(0, 1, 3.14);
      MyWorksheet.WriteBackgroundColor(cell, clSilver);
    

#### Границы ячеек

Ячейки можно обвести, рисуя границы вдоль их краев или диагональных линий. Существует четыре границы плюс две диагонали, перечисленные в типе данных `TsCellBorder`: 
    
    
    type
      TsCellBorder = (cbNorth, cbWest, cbEast, cbSouth, dbDiagUp, dbDiagDown);
      TsCellBorders = set of TsCellBorder;
    

Чтобы показать линию края, добавьте соответствующую границу к ячейке `Borders` (тип `TsCellBorders`, см. выше). Таким образом, каждый край ячейки может обрабатываться отдельно. Для этого используйте метод рабочего листа `WriteBorders`. Этот пример добавляет верхнюю и нижнюю границы к краям A1 и B1: 
    
    
      MyWorksheet.WriteBorders(0, 0, [cbNorth, cbSouth]);  // A1: строка 0, столбец 0
      MyWorksheet.WriteBorders(0, 1, [cbNorth, cbSourth]); // B1: строка 0, столбец 1
    

Линии обычно рисуются как тонкие, сплошные черные линии. Но можно изменить **стиль линии** и **цвет** каждой строки. Для этой цели ячейка предоставляет массив записей `TsCellBorderStyle`: 
    
    
    type
      TsLineStyle = (lsThin, lsMedium, lsDashed, lsDotted, lsThick, lsDouble, lsHair,
        lsMediumDash, lsDashDot, lsMediumDashDot, lsDashDotDot, lsMediumDashDotDot, lsSlantDashDot); 
    
      TsCellBorderStyle = record
        LineStyle: TsLineStyle;   
        Color: TsColor;
      end;
    
      TsCellBorderStyles = array[TsCellBorder] of TsCellBorderStyle;   
    
      TsWorksheet = class
      public
        function WriteBorderStyle(ARow, ACol: Cardinal; ABorder: TsCellBorder; AStyle: TsCellBorderStyle): PCell; overload;
        procedure WriteBorderStyle(ACell: PCell; ABorder: TsCellBorder; AStyle: TsCellBorderStyle); overload;
    
        function WriteBorderStyle(ARow, ACol: Cardinal; ABorder: TsCellBorder; ALineStyle: TsLineStyle; AColor: TsColor): PCell; overload;
        procedure WriteBorderStyle(ACell: PCell; ABorder: TsCellBorder; ALineStyle: TsLineStyle; AColor: TsColor); overload;
    
        function WriteBorderColor(ARow, ACol: Cardinal; ABorder: TsCellBorder; AColor: TsColor): PCell; overload;
        proceure WriteBorderColor(ACell: PCell; ABorder: TsCellBorder; AColor: TsColor): PCell; overload;
    
        function WriteBorderLineStyle(ARow, ACol: Cardinal; ABorder: TsCellBorder; ALineStyle: TsLineStyle): PCell; overload;
        procedure WriteBorderLineStyle(ACell: PCell; ABorder: TsCellBorder; ALineStyle: TsLineStyle): PCell; overload;
    
        function WriteBorderStyles(ARow, ACol: Cardinal; const AStyles: TsCellBorderStyles): PCell; overload;      
        procedure WriteBorderStyles(ACell: PCell; const AStyles: TsCellBorderStyles); overload;
    
        function WriteBorders(ARow, ACol: Cardinal; ABorders: TsCellBorders): PCell; overload
        procedure WriteBorders(ACell: PCell; ABorders: TsCellBorders); overload
    
        function ReadCellBorders(ACell: PCell): TsCellBorders;
        function ReadCellBorderStyle(ACell: PCell; ABorder: TsCellBorder): TsCellBorderStyle;
        function ReadCellBorderStyles(ACell: PCell): TsCellBorderStyles;
        ...
      end;
    

Стиль границы данной ячейки может быть задан следующими способами, представленными в рабочем листе: 

  * **WriteBorderStyle** присваивает запись стиля границы ячейки одной границе ячейки. Есть две перегруженные версии этого метода: одна занимает целую запись `TsCellBorderStyle`, другая - отдельные элементы записи.
  * **WriteBorderColor** изменяет цвет данной границы, не затрагивая стиль линии этой границы.
  * **WriteBorderLineStyle** устанавливает стиль линии только границы, но оставляет цвет неизменным.
  * **WriteBorderStyles** устанавливает стиль границы всех границ данной ячейки сразу. Полезно для копирования стилей границ из одной ячейки в другие ячейки.



Этот пример добавляет тонкую черную границу к вершине [ячейки] и толстую синюю границу по нижней [кромке] ячеек A1 и B1: 
    
    
    var
      cellA1, cellB1: PCell;
    ...
      cellA1 := MyWorksheet.WriteBorders(0, 0, [cbNorth, cbSouth]);    // ячейка A1: строка 0, столбец 0
      MyWorksheet.WriteBorderStyle(cellA1, cbNorth, lsThin, scBlack);  
      MyWorksheet.WriteBorderStyle(cellA1, cbSouth, lsThick, scBlue);
    
      cellB1 := MyWorksheet.WriteBorders(0, 1, [cbNorth, cbSouth]);    // ячейка B1: строка 0, столбец 1
      MyWorksheet.WriteBorderStyles(cellB1, cellA1^.BorderStyles);     // копируем все стили границ из ячейки A1 в B1
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Диагональные линии не поддерживаются [форматами] _sfExcel2_ и _sfExcel5_ \- они просто опускаются. _sfExcel8_ и _sfOOXML_ используют один и тот же стиль линии, и цвет для обеих диагоналей; стиль границы, [определенный для] диагональной [снизу - вверх] линии, считается действительным также для диагональной линии [сверху - ] вниз. Некоторые [парсеры] не поддерживают все стили линий, перечисленных в `TsLineStyle`. В этом случае используются соответствующие стили замены линий.

#### Шрифты

Текст ячейки может отображаться в разных шрифтах. С этой целью в книге содержится список элементов `TsFont`: 
    
    
    type
      TsFont = class
        FontName: String;
        Size: Single;   
        Style: TsFontStyles;
        Color: TsColor;
        Position: TsFontPosition;
      end;
    

  * **FontName** [имя шрифта] соответствует имени шрифта, используемому операционной системой. В Windows примером будет «Times New Roman».
  * **FontSize** [размер шрифта] указывается в «точках», т.е. в единицах 1/72 дюйма, которые обычно используются в приложениях Office.
  * **FontStyle** [стиль шрифта] представляет собой набор элементов [списка со значениями] `fssBold`, `fssItalic`, `fssStrikeout` и `fssUnderline` [(соответственно **жирный** , _наклонный_ , ~~зачеркнутый~~ и _подчеркнутый_)], перечислимого типа `TsFontStyle`. «Нормальный» шрифт соответствует пустому набору.
  * **Color** [цвет] определяет цвет переднего плана текстовых символов, указанных в представлении rgb, как обсуждалось выше.
  * **Position** [положение] один из двух [параметров свойства] `fpNormal` \- `fpSuperscript` или `fpSubscript` указывает, должен ли размер шрифта уменьшаться примерно на 1/3 со смещением символов вверх (верхний [регистр]) или вниз (нижний [регистр]).



Каждая ячейка снабжается **индексом в списке шрифтов**. 

Чтобы назначить конкретный шрифт ячейке, используйте один из следующих методов `TsSpreadsheet`: 
    
    
    type
      TsSpreadsheet = class
      public
        function  WriteFont(ARow, ACol: Cardinal; const AFontName: String;
          AFontSize: Single; AFontStyle: TsFontStyles; AFontColor: TsColor): Integer; overload;
        procedure WriteFont(ARow, ACol: Cardinal; AFontIndex: Integer); overload;
        function WriteFontColor(ARow, ACol: Cardinal; AFontColor: TsColor): Integer;
        function WriteFontSize(ARow, ACol: Cardinal; ASize: Integer): Integer;
        function WriteFontStyle(ARow, ACol: Cardinal; AStyle: TsFontStyles): Integer; 
        // плюс: перегруженные версии, принимающие указатель на ячейку записи, а не индекс строки и столбца в качестве параметра
        // ...
      end;
    

  * **WriteFont** назначает шрифт ячейке. Если шрифт еще не существует в списке шрифтов, создается новая запись. Функция возвращает индекс шрифта в списке шрифтов. Кроме того, есть перегруженная версия, которая учитывает только индекс шрифта в качестве параметра.
  * **WriteFontColor** заменяет цвет шрифта, который в настоящее время назначается ячейке новым [цветом]. Опять же, новый элемент списка шрифтов создается, если шрифт с новым цветом еще не существует. Функция возвращает индекс шрифта в списке.
  * **WriteFontSize** заменяет размер используемого в настоящее время шрифта ячейки.
  * **WriteFontStyle** заменяет стиль (обычный, полужирный, курсив и т. д.) используемого в настоящее время шрифта ячейки.



Список шрифтов рабочей книги содержит по крайней мере один элемент, который является **шрифтом по умолчанию** для ячеек с немодифицированными шрифтами. По умолчанию это [шрифт] "Arial" [размером] 10pt. Используйте метод `SetDefaultFont` рабочей книги, чтобы назначить другой шрифт первому элементу списка. 

Шрифт в указанном индексе списка шрифтов можно просмотреть, вызвав функцию рабочей книги `GetFont`. Количество доступных шрифтов возвращается [свойством] `GetFontCount`. 

Вот **пример** , который уменьшает размер всех 10pt-овых шрифтов Arial до 9pt: 
    
    
    var
      i: Integer;
      font: TsFont;
    begin
      for i := 0 to MyWorkbook.GetFontCount-1 do
      begin
        font := MyWorkbook.GetFont(i);
        if (font.FontName = 'Arial') and (font.Size = 10.0) then
          font.Size := 9.0;
      end;
    end;
    

#### Форматирование Rich-text

В дополнение к использованию конкретного шрифта для каждой ячейки также можно указать конкретные атрибуты шрифта для отдельных символов или групп символов в каждом тексте ячейки. Следуя нотации Excel, мы называем эту функцию "форматированием Rich-text" (хотя это не имеет ничего общего с форматом файла *.rtf). 

С этой целью модуль _fpstypes_ объявляет тип `TsRichTextParams`, который представляет собой массив записей `TsRichTextParam`: 
    
    
    type
      TsRichTextParam = record
        FontIndex: Integer;
        FirstIndex: Integer;
      end;
    
      TsRichTextParams = array of TsRichTextParam;
    

`FontIndex` ссылается на индекс шрифта в FontList рабочей книги, который будет использоваться для форматирования символов, начинающихся с индекса `FirstIndex`. Нумерация индекса строковых символов `FirstIndex` начинается с 1. 

Существует два способа добавления форматирования "rich-text" в ячейке: 

  * Вставка соответствующих HTML-кодов в текст ячейки. Это можно сделать, используя метод `WriteTextAsHTML` рабочего листа. Чтобы добавить в ячейку A1 текст "Area (m2)", передайте следующую HTML-кодированную строку для этой функции


    
    
      MyWorksheet.WriteTextAsHTML(0, 0, 'Area (m<sup>2</sup>');
    

  * Альтернативно, стандартный метод текстовой записи `WriteText` может быть вызван с дополнительным параметром, определяющим параметры форматирования rich-text, которые будут использоваться напрямую:


    
    
    var
      richTextParams: TsRichTextParams;
      fnt: TsFont;
    begin
      SetLength(rtp, 2);
      fnt := MyWorksheet.ReadCellFont(0, 0);
      richTextParams[0].FirstIndex := 8; // Группа верхнего индекса начинается с «2», который является символом #8 текста ячейки.
      richTextParams[0].FontIndex := MyWorkbook.AddFont(fnt.FontName, fnt.Size, fnt.Style, fnt.Color, fpSuperscript);
      richTextParams[1].FirstIndex := 9; // Обычный шрифт снова начинается с символа # 9.
      richTextParams[1].FontIndex := MyWorksheet.ReadCellFontIndex(0, 0);
      MyWorksheet.WriteUTF8Text(0, 0, 'Area (m2)', richTextParams);
    end;
    

Используйте метод рабочего листа `DeleteRichTextParams`, чтобы удалить форматирование rich-text из ранее отформатированной ячейки. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если ячейка должна иметь шрифт, отличный от шрифта по умолчанию рабочего листа, то этот шрифт должен быть записан в ячейку **перед** записью форматированного текста rich-text. В противном случае шрифт в неотформатированных регионах не будет обновлен.

#### Вращение текста

Обычно текст отображается в ячейках по горизонтали. Тем не менее, можно также поворачивать его на 90 градусов по часовой стрелке или против часовой стрелки. Кроме того, существует возможность установки горизонтальных символов вертикально друг над другом. 

Если вам нужна эта функция, используйте метод рабочего листа `WriteTextRotation` и укажите направление текста элементом [свойства] перечислимого типа `TsTextRotation`: 
    
    
    type
      TsTextRotation = (trHorizontal, rt90DegreeClockwiseRotation,
        rt90DegreeCounterClockwiseRotation, rtStacked);
    
      TsWorksheet = class
      public
        function WriteTextRotation(ARow, ACol: Cardinal; ARotation: TsTextRotation): PCell; overload;
        procedure WriteTextRotation(ACell: PCell; ARotation: TsTextRotation); overload;
    
        function ReadTextRotation(ACell: PCell): TsTextRotation;
        // ...
      end;
    
      // пример для повернутого против часовой стрелки текста в ячейке A1:
      WriteTextRotation(0, 0, rt90DegreeCounterClockwizeRotation);
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Более мелкие степени вращения, которые могут поддерживаться некоторыми форматами файлов электронной таблицы, игнорируются.

#### Выравнивание текста

По умолчанию текст ячейки выровнен по левому и нижнему краям ячейки, за исключением чисел, выровненных по правому краю. Это поведение можно изменить, используя методы рабочего листа `WriteHorAlignment` и `WriteVertAlignment`: 
    
    
    type
      TsHorAlignment = (haDefault, haLeft, haCenter, haRight);
      TsVertAlignment = (vaDefault, vaTop, vaCenter, vaBottom);    
    
      TsWorkbook = class
      public
        function WriteHorAlignment(ARow, ACol: Cardinal; AValue: TsHorAlignment): PCell; overload;
        procedure WriteHorAlignment(ACell: PCell; AValue: TsHorAlignment); overload;
        function ReadHorAlignment(ACell: PCell): TsHorAlignment;
    
        function WriteVertAlignment(ARow, ACol: Cardinal; AValue: TsVertAlignment): PCell; overload; 
        procedure WriteVertAlignment(ACell: PCell; AValue: TsVertAlignment); overload;
        function ReadVertAlignment(ACell: PCell): TsVertAlignment;
        // ...
      end;
    
      // Пример: [так] центрируется текст в ячейке А1 как по горизонтали, так и по вертикали
      MyWorkbook.WriteHorAlignment(0, 0, haCenter);
      MyWorkbook.WriteVertAlignment(0, 0, vaCenter);
    

#### Перенос слов

Текст, длина которого превышает ширину ячейки, может быть перенесен на несколько строк вызовом метода `WriteWordwrap` электронной таблицы: 
    
    
    type
      TsWorksheet = class
      public
        function WriteWordwrap(ARow, ACol: Cardinal; AValue: Boolean): PCell; overload;
        procedure WriteWordwrap(ACell: PCell; AValue: Boolean); overload;
        function ReadWordwrap(ACell: PCell): Boolean;
        //...
      end;
    
      // Пример: активация переноса слов в ячейке А1
      MyWorksheet.WriteWordwrap(0, 0, true);
    

#### Объединенные ячейки

Подобно приложениям Office, FPSpreadsheet поддерживает также возможность слияния ячеек в одну большую ячейку, которая часто используется как общий заголовок выше аналогичных столбцов. Просто вызовите `MergeCells` и передайте параметр, чтобы указать диапазон ячеек, которые нужно объединить, либо строку диапазона Excel (например, `A1:D5`), либо первую и последнюю строку и столбец: 
    
    
      MyWorksheet.MergeCells('A1:D5');
      // или: MyWorksheet.MergeCells(0, 0, 4, 3);  // первая строка, первый столбец, последняя строка, последний столбец
    

Содержимое и формат, отображаемые для объединенного диапазона, берутся из верхнего левого угла диапазона ячейки `A1` в приведенном выше примере. Эта ячейка вызывается [свойством] `MergeBase` в библиотеке. За исключением этой угловой ячейки, [там] не должно быть никаких других ячеек в диапазоне. Если [они там окажутся], их содержимое и формат будут скрыты. 

Чтобы разбить объединенный диапазон на отдельные ячейки, используйте команду `Unmerge` и передайте любую ячейку, находящуюся в объединенном диапазоне: 
    
    
      MyWorksheet.UnmergeCells('B1');
      // или: MyWorksheet.UnmergeCells(0, 1);   // строка, столбец любой ячейки в пределах диапазона
    

Объединенные ячейки могут быть прочитаны/записаны во все форматы файлов, за исключением `sfCSV`, `sfExcel2` и `sfExcel5`, которые не поддерживают эту функцию изначально. 

Информация, с которой объединены ячейки, хранится во внутреннем списке. В отличие от предыдущих версий, больше невозможно получить доступ к `MergeBase` из ячейки напрямую. Используйте следующие функции для извлечения информации об объединенных ячейках: 
    
    
    var
      cell, base: PCell;
      r1,c1,r2,c2: Cardinal;
    ...
      cell := MyWorksheet.FindCell('B1');
      if MyWorksheet.IsMerged(cell) then
      begin
        WriteLn('Ячейка B1 объединена.');
    
        MyWorksheet.FindMergedRange(cell, r1, c1, r2, c2);
        WriteLn('Объединенный диапазон - ' + GetCellRangeString(r1, c1, r2, c2));
    
        base := MyWorksheet.FindMergeBase(cell);
        WriteLn('Основная ячейка для слияния - ' + GetCellString(base^.Row, base^.Col));
      end;
    

#### Защита ячейки

Это описано в отдельном разделе ниже. 

### Дополнительная информация

#### Комментарии к ячейке

Комментарии могут быть прикреплены к любой ячейке путем вызова 
    
    
      MyWorksheet.WriteComment(0, 0, 'Это комментарий для ячейки A1');
    

Они хранятся во внутреннем списке рабочего листа. Используйте соответствующие методы рабочего листа для доступа к комментариям: 

  * Если вы хотите узнать, содержит ли конкретная ячейка комментарий, вызовите метод рабочего листа `HasComment(cell)`.
  * Для получения комментария ячейки используйте метод `ReadComment(cell)` или его перегруженный вариант `ReadComment(ARow, ACol)`.
  * Общее количество комментариев может быть получено из `worksheet.Comments.Count`.



#### Гиперссылки

Гиперссылки могут быть прикреплены к ячейкам, чтобы связать ячейки с другими документами или другими ячейками в одной книге. Общий синтаксис для создания гиперссылок 
    
    
      procedure TWorksheet.WriteHyperlink(ARow, ACol: Cardinal; ATarget: String; ATooltip: String = '');
    

  * Целью гиперссылки, переданной как параметр `ATarget`, должна быть полностью квалифицированной [(Унифицированный идентификатор ресурса)](<https://ru.wikipedia.org/wiki/URI>), состоящей из [определенных] фраз протокола [в определенной последовательности](например, `http://`, `file:///`, `mailto:` и т. д.), за которой следует определенная информация, такая как веб-адрес, имя файла или e-mail адрес и необязательный идентификатор закладки, разделенный символом `'#'`.
  * Необязательный параметр `Tooltip` [(подсказка)] расценивается Excel для отображения его в окне подсказки, если мышь находится над гиперссылкой.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Гиперссылки могут быть добавлены в пустые ячейки или в ячейки с контентом. В последнем случае отображаемый контент не изменяется гиперссылкой; в первом случае ячейка преобразуется в метку ячейки, показывающую цель гиперссылки. Имейте в виду, что OpenOffice/LibreOffice принимает гиперссылки только с нетекстовыми ячейками.

**Примеры:**
    
    
      // Открытие веб-сайта www.google.com
      MyWorksheet.WriteText(0, 0, 'Открыть google');
      MyWorksheet.WriteHyperlink(0, 0, 'http://www.google.com');
    
      // Открытие локального файла с абсолютным путем "C:\readme.txt" (предполагается, [что это] Windows)
      MyWorksheet.WriteHyperlink(1, 0, 'file:///c:\readme.txt');
    
      // Открытие почтового клиента для отправки почты
      MyWorksheet.WriteText('Отправить письмо');
      MyWorksheet.WriteHyperlink(3, 0, 'mailto:somebody@gmail.com?subject=Test');
    
      // Переход к определенной ячейке 
      MyWorksheet.WriteText(5, 0, 'Переход к ячейке A10 на листе2');
      MyWorksheet.WriteHyperlink(5, 0, '#Sheet2!A10');
    
      // Переход в ячейку A10 на текущем листе и отображение всплывающей подсказки
      MyWorksheet.WriteHyperlink(5, 0, '#A10', 'Переход к ячейке A10');
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** FPSpreadsheet не «следит» за ссылками, он предоставляет только механизм для получения доступа к связанным данным. Однако [компонент] TsWorksheetGrid из пакета _laz_fpspreadsheet_visual_ запускает событие `OnClickHyperlink`, если ячейку с внешней гиперссылкой щелкают за доли секунды. В соответствующем обработчике событий вы можете, например, загрузить целевую электронную таблицу или открыть веб-браузер для отображения связанного веб-сайта. Если ссылка является внутренней ссылкой на другую ячейку в одной книге, таблица переходит к связанной ячейке.

#### Изображения

FPSpreadsheet поддерживает встраивание изображений в рабочие листы. Используйте один из методов рабочего листа `WriteImage()`, чтобы добавить изображение на рабочий лист: 
    
    
      MyWorksheet.WriteImage(row, col, filename, offsetX, offsetY, scaleX, scaleY);
      MyWorksheet.WriteImage(row, col, stream, offsetX, offsetY, scaleX, scaleY);
      MyWorksheet.WriteImage(row, col, imageindex, offsetX, offsetY, scaleX, scaleY);
    

Верхний/левый угол изображения помещается в верхний/левый угол ячейки в указанной строке и столбце. Параметры плавающей запятой `offsetX`, `offsetY`, `scaleX` и `scaleY` являются необязательными: они определяют смещение точки привязки этого изображения от угла ячейки и коэффициент увеличения. Путь к файлу изображения указывается как параметр `filename`. В качестве альтернативы можно использовать перегруженные версии, которые принимают поток [данных] вместо имени файла или индекса изображения в списке EmbeddedObj рабочей книги - используйте `MyWorkbook.FindEmbeddedObj(имя файла)`, чтобы получить этот индекс для ранее загруженного изображения файл. 

Обратите внимание, что FPSpreadsheet должен знать тип изображения для успешного импорта картинок. В настоящее время поддерживаются типы _png_ , _jpg_ , _tiff_ , _bmp_ , _gif_ , _svg_ , _wmf_ , _emf_ и _pcx_ (Excel2007 не может читать импортированные изображения _svg_ and _pcx_). Другие форматы могут быть зарегистрированы путем записи функции, которая определяет размер изображения и плотность пикселей, а также путем регистрации нового формата с помощью процедуры `RegisterImageType` \- см. модуль _fpsImages_ для [получения] примеров: 
    
    
    type
      TsImageType = integer;
      TGetImageSizeFunc = function (AStream: TStream; out AWidth, AHeight: DWord; out dpiX, dpiY: Double): Boolean;
    
    function RegisterImageType(AMimeType, AExtension: String; AGetImageSize: TGetImageSizeFunc): TsImageType;
    

Из-за различий в расчете высоты строки и ширины столбцов между приложениями FPSpreadsheet и Office невозможно правильно позиционировать изображения. Если точное положение изображения важно, вы должны соблюдать следующие правила: 

  * Предопределите ширину всех столбцов, по крайней мере, до той, которая содержит правый край изображения.
  * Предопределите высоты всех строк, по крайней мере, до той, которая содержит нижний край изображения.
  * Если рабочая книга должна быть сохранена в формате OpenDocument, добавьте изображение после изменения ширины столбцов и высоты строк, поскольку ods привязывает изображение к листу, а не к ячейке (как Excel и FPSpreadsheet).
  * Если точный размер изображения важен, убедитесь, что он помещается в одну ячейку.



### Сортировка

Ячейки на рабочем листе можно сортировать по множеству критериев, вызывая метод `Sort` рабочего листа. Этот метод принимает запись `TsSortParams`, и края прямоугольника ячейки сортируются как параметры; в перегруженной версии прямоугольник ячейки также может быть задан с помощью строки диапазона типа Excel (например, `'A1:G10'`): 
    
    
    type
      TsWorksheet = class
        // ...
        procedure Sort(const ASortParams: TsSortParams;
          ARowFrom, AColFrom, ARowTo, AColTo: Cardinal); overload;
        procedure Sort(ASortParams: TsSortParams; ARange: String); overload;
        // ...
      end;
    

Критерии сортировки определяются записью типа `TsSortParams`: 
    
    
    type
      TsSortParams = record
        SortByCols: Boolean;
        Priority: TsSortPriority;  // spNumAlpha ("Сначала числа") или spAlphaNum ("Сначала текст")
        Keys: TsSortKeys;
      end;    
    
      TsSortKey = record
        ColRowIndex: Integer;
        Options: TsSortOptions;    // set of [spDescending, spCaseInsensitive]
      end;
    

  * Логическое значение `SortByCols` определяет, что сортировка выполняется вдоль столбцов (`true`) или строк (`false`). Соответственно, `ColRowIndex`, указанный в ключах сортировки, соответствует индексу столбца или строки (см. ниже).
  * [Свойство] `Priority` определяет в диапазонах ячеек смешанного содержимого, будет ли возрастающий тип ставить числовые значения перед текстовыми значениями или нет. Пустые ячейки всегда перемещаются в конец отсортированного столбца или строки. В Excel приоритетом является [флаг] "сначала числа" (`spNumAlpha`).
  * Массив `Keys` указывает несколько параметров сортировки. Они состоят из индекса отсортированного столбца или строки (`ColRowIndex`) и набора [свойств] `Options` для [указания] направления сортировки (`spoDescending`) и РЕГИСТРОзависимости (`spCaseInsensitive`). Если [набор свойств] `Options` пуст, сравнение ячеек чувствительно к регистру, а ячейки упорядочены в порядке возрастания. Если две ячейки считаются "равными" на основе первого ключа (`sortParams.Keys[0]`), сравнение выполняется со следующими условиями в массиве `Keys` до тех пор, пока [не будет] обнаружена разница [между ячейками] или все условия [не будут] исчерпаны.



`InitSortParams` \- это удобная утилита для инициализации параметров сортировки: 
    
    
    function InitSortParams(ASortByCols: Boolean = true; ANumSortKeys: Integer = 1;
      ASortPriority: TsSortPriority = spNumAlpha): TsSortParams;
    

Следующий фрагмент кода показывает типичный вызов сортировки: 
    
    
    var
      sortParams: TsSortParams;
    begin
      sortParams := InitSortParams(true, 2);  // сортировка по столбцам, 2 сортировочных ключа
    
      // первичный ключ сортировки: столбец 3, восходящая [сортировка], нечувствительный к регистру
      sortParams.Keys[0].ColRowIndex := 3;
      sortParams.Keys[0].Options := [ssoCaseInsensitive];
    
      // вторичный ключ сортировки: столбец 1, нисходящая [сортировка]
      sortParams.Keys[1].ColRowIndex := 1;
      sortParams.Keys[1].Options := [ssoDescending];
    
      // Блок сортировки распространяется между ячейкой A1 (строка=0, столбец=0) и F10 (строка=9, столбец=5)
      MyWorksheet.Sort(sortParams, 0, 0, 9, 5);
      // или: MyWorksheet.Sort(sortParams, 'A1:F10');
    end;
    

### Поиск и замена

Модуль _fpsSearch_ реализует **поисковую систему** , которая может использоваться для поиска определенного содержимого ячейки в рабочей книге или для замены найденного содержимого ячейки какой-либо другой строкой. 

**Например:**
    
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyRow, MyCol: Cardinal;
      MySearchParams: TsSearchParams;
    begin
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorkbook.ReadFromFile(AFileName);
    
        // Specify search criteria
        MySearchParams.SearchText := 'Привет';
        MySearchParams.Options := [soEntireDocument];
        MySearchParams.Within := swWorkbook;
    
        // или: MySearchParaams := InitSearchParams('Привет', [soEntireDocument], swWorkbook);
    
        // Создать поисковую систему и выполнить поиск
        with TSearchEngine.Create(MyWorkbook) do begin
          if FindFirst(MySearchParams, MyWorksheet, MyRow, MyCol) then begin
            WriteLn('Первый [искомый текст] "', MySearchparams.SearchText, '" найден в ячейке ', GetCellString(MyRow, MyCol));
            while FindNext(MySeachParams, MyWorksheet, myRow, MyCol) do
              WriteLn('Следующий [искомый текст] "', MySearchParams.SearchText, '" найден в ячейке ', GetCellString(MyRow, MyCol));
          end;
          Free;
        end;
      finally
        MyWorkbook.Free;
      end;
    end;
    

Поисковая система предоставляет два метода для **поиска** : `FindFirst` и`FindNext`. Они очень похожи, но различаются только местом, где начинается поиск. В случае `FindFirst` начальная ячейка определяется [набором опций] из списка `Options`, описанных ниже. В случае `FindNext` поиск начинается в ячейке, следующей за предыдущей найденной ячейкой. Оба метода возвращают рабочий лист, [а также] индексы строк и столбцов ячейки, в которой найден искомый текст. Если поиск безуспешен, результат функции `FALSE`. 

Запись `TsSearchParams` указывает критерии, используемые для поиска: 
    
    
    type
      TsSearchParams = record
        SearchText: String;
        Options: TsSearchOptions;
        Within: TsSearchWithin;
      end;
    

Помимо текста для поиска (`SearchText`), она предоставляет набор опций для сужения поиска: 

  * `soCompareEntireCell`: сравнивает `SearchText` со всем содержимым ячейки. Если [эта опция] не содержится в `Options`, текст ячейки сравнивается только частично.
  * `soMatchCase`: [позволяет] выполнить поиск с учетом регистр
  * `soRegularExpr`: [здесь] `SearchText` рассматривается как регулярное выражение
  * `soAlongRows`: Поисковая система осуществляет поиск сначала вдоль строк. Если [эта опция] не содержится в `Options`, поиск продолжается по столбцам.
  * `soBackward`: Поиск начинается с конца документа или выполняется назад из активной ячейки [(к началу документа)]. Если [эта опция] не содержится в `Options`, поиск начинается с начала документа или выполняется вперед из активной ячейки [(к концу документа)].
  * `soWrapDocument`: Если поиск достиг конца документа, поиск возобновляется с самого начала (или наоборот, если используется `soBackward`).
  * `soEntireDocument`: Поиск начинается с первой ячейки (или последней ячейки, если используется `soBackward`). Если [эта опция] не содержится в `Options`, поиск начинается с активной ячейки рабочего листа. [При этом] игнорируется [опция] `FindNext`.



Поле записи `Within` идентифицирует часть электронной таблицы для поиска: 

  * `swWorkbook`: Идет поиск по всей рабочей книге. Если поисковая фраза не найдена на первом листе (или последнем листе, если используется `soBackward`), поиск продолжается со следующего (предыдущего) листа.
  * `swWorksheet`: Поиск ограничен текущим активным рабочим листом
  * `swColumn`: Поиск ограничен столбцом активной ячейки
  * `swRow`: Поиск ограничен строкой активной ячейки



Запись параметров поиска может быть инициализирована вызовом `InitSearchParams` (в модуле _fpsutils_) с элементами записи в качестве необязательных параметров. 

В дополнение к поиску поисковая система также может использоваться для **замены** найденного текста другой строкой. Для этого вызовите функции `ReplaceFirst` или `ReplaceNext`. Они действуют как аналог [функции] `FindXXXX`, поэтому для определения критериев поиска требуется запись `TsSearchParams`. Но в дополнение к поиску эти функции также выполняют замену текста в соответствии со спецификацией в записи `TsReplaceParams`: 
    
    
    type
      TsReplaceParams = record
        ReplaceText: String;
        Options: TsReplaceOptions;
      end;
    

`ReplaceText` идентифицирует строку, которая заменит найденный шаблон текста. Параметры `Options` определяют набор критериев, по которым выполняется замена: 

  * `roReplaceEntirecell`: Заменяет весь текст ячейки текстом `ReplaceText`. Если [эта опция] не содержится в `Options`, тогда заменяется только часть, соответствующая `SearchText`.
  * `roReplaceAll`: Выполняет замену во всех найденных ячейках (т.е. просто вызывает `ReplaceFirst`, чтобы автоматически заменить все).
  * `roConfirm`: Вызывает обработчик для события `OnConfirmReplacement`, в котором пользователь должен указать, должна ли выполняться замена или нет. Обратите внимание, что этот обработчик событий является обязательным, если установлен `roConfirm`.



Используйте функцию `InitReplaceParams` (в модуле _fpsutils_) для инициализации записи параметров замены с предопределенными (но необязательными) значениями. 

### Операции со столбцами и строками

Рабочий лист предоставляет эти методы для вставки, удаления, скрытия или отображения столбцов и строк: 
    
    
    type
      TsWorksheet = class
      ...
        procedure DeleteCol(ACol: Cardinal);
        procedure DeleteRow(ARow: Cardinal);
    
        procedure InsertCol(ACol: Cardinal);
        procedure InsertRow(ARow: Cardinal);
    
        procedure RemoveCol(ACol: Cardinal);
        procedure RemoveRow(ARow: Cardinal);
    
        procedure RemoveAllCols;
        procedure RemoveAllRows
    
        procedure HideCol(ACol: Cardinal);
        procedure HideRow(ARow: Cardinal);
        
        procedure ShowCol(ACol: Cardinal);
        procedure ShowRow(ARow: Cardinal);
    
        function ColHidden(ACol: Cardinal): Boolean;
        function RowHidden(ARow: Cardinal): Boolean;
    

  * Когда столбец или строка **удалены** [методом] `DeleteCol` или `DeleteRow`, любые данные, назначенные этому столбцу или строке, т.е. ячейки, комментарии, гиперссылки, записи TCol или TRow, удаляются. Данные справа или ниже удаленной колонки/строки перемещаются [соответственно] влево или вверх.


  * `RemoveCol` и `RemoveRow` по соглашению **удаляют** только запись столбца или строки, т.е. сбрасывают ширину столбца и высоту строки до значений по умолчанию. Данные ячейки, комментариев и гиперссылок не затрагиваются.


  * `RemoveAllCols` **удаляет все** записи столбцов, т.е. сбрасывает ширину всех столбцов; `RemoveAllRows` делает то же самое с записями строки и высотой строк.


  * Столбец или строка **вставляются** перед индексом, указанным в качестве параметра метода `InsertXXX`.



### Макет страницы

#### Общие сведения

Пока что FPSpreadsheet не поддерживает печать рабочих листов, но приложения Office делают [это], и они предоставляют секцию информации в своих файлах для этой цели. В FPSpreadsheets эта информация доступна в классе `TsPageLayout`, который принадлежит структуре данных `TsWorksheet`. Его свойства и методы сочетают в себе самые важные функции из мира Excel и OpenDocument. 
    
    
    type 
      TsPageOrientation = (spoPortrait, spoLandscape);
    
      TsPrintOption = (poPrintGridLines, poPrintHeaders, poPrintPagesByRows,
        poMonochrome, poDraftQuality, poPrintCellComments, poDefaultOrientation,
        poUseStartPageNumber, poCommentsAtEnd, poHorCentered, poVertCentered,
        poDifferentOddEven, poDifferentFirst, poFitPages);
    
      TsPrintOptions = set of TsPrintOption;  
    
      TsHeaderFooterSectionIndex = (hfsLeft, hfsCenter, hfsRight);
    
      TsCellRange = record
        Row1, Col1, Row2, Col2: Cardinal;
      end;
    
      TsPageLayout = class
      ...
      public
        ...
        { Methods }
        // встроенные изображения заголовка/нижнего колонтитула
        procedure AddHeaderImage(AHeaderIndex: Integer;
          ASection: TsHeaderFooterSectionIndex; const AFilename: String);
        procedure AddFooterImage(AFooterIndex: Integer;
          ASection: TsHeaderFooterSectionIndex; const AFilename: String);
        procedure GetImageSections(out AHeaderTags, AFooterTags: String);
        function HasHeaderFooterImages: Boolean;
    
        // Повторитель строк и столбцов 
        function HasRepeatedCols: Boolean;
        function HasRepeatedRows: Boolean;
        procedure SetRepeatedCols(AFirstCol, ALastCol: Cardinal);
        procedure SetRepeatedRows(AFirstRow, ALastRow: Cardinal);
    
        // диапазоны печати 
        function AddPrintRange(ARow1, ACol1, ARow2, ACol2: Cardinal): Integer; overload;
        function AddPrintRange(const ARange: TsCellRange): Integer; overload;
        function GetPrintRange(AIndex: Integer): TsCellRange;
        function NumPrintRanges: Integer;
        procedure RemovePrintRange(AIndex: Integer);
    
        { Properties }
        property Orientation: TsPageOrientation read FOrientation write FOrientation;
        property PageWidth: Double read FPageWidth write FPageWidth;
        property PageHeight: Double read FPageHeight write FPageHeight;
        property LeftMargin: Double read FLeftMargin write FLeftMargin;
        property RightMargin: Double read FRightMargin write FRightMargin;
        property TopMargin: Double read FTopMargin write FTopMargin;
        property BottomMargin: Double read FBottomMargin write FBottomMargin;
        property HeaderMargin: Double read FHeaderMargin write FHeaderMargin;
        property FooterMargin: Double read FFooterMargin write FFooterMargin;
        property StartPageNumber: Integer read FStartPageNumber write SetStartPageNumber;
        property ScalingFactor: Integer read FScalingFactor write SetScalingFactor;
        property FitHeightToPages: Integer read FFitHeightToPages write SetFitHeightToPages;
        property FitWidthToPages: Integer read FFitWidthToPages write SetFitWidthToPages;
        property Copies: Integer read FCopies write FCopies;
        property Options: TsPrintOptions read FOptions write FOptions;
        property Headers[AIndex: Integer]: String read GetHeaders write SetHeaders;
        property Footers[AIndex: Integer]: String read GetFooters write SetFooters;
        property RepeatedCols: TsRowColRange read FRepeatedCols;
        property RepeatedRows: TsRowColRange read FRepeatedRows;
        property PrintRange[AIndex: Integer]: TsCellRange read GetPrintRange;
        property FooterImages[ASection: TsHeaderFooterSectionIndex]: TsHeaderFooterImage read GetFooterImages;
        property HeaderImages[ASection: TsHeaderFooterSectionIndex]: TsHeaderFooterImage read GetHeaderImages;
      end;
    
      TsWorksheet = class
      ...
      public
        property PageLayout: TsPageLayout;
        ...
      end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Тот факт, что `PageLayout` принадлежит рабочему листу, указывает на то, что в одной книге может быть несколько макетов страниц, по одному для каждого листа.

  * **Orientation** [(ориентация)] определяет ориентацию печатной бумаги, как портретной, так и альбомной.


  * **Page width** [(ширина страницы)] и **page height** [(высота страницы)] ссылаются на стандартную ориентацию бумаги, обычно портретную.


  * **Left** , **top** , **right** and **bottom margins** не требуют пояснений и приведены в миллиметрах.


  * **HeaderMargin** [(отступ заголовка)] понимается - как и в Excel - как расстояние между верхним краем бумаги и верхней частью заголовка, и **TopMargin** [(отступ сверху)] - соответственно, как расстояние между верхним краем бумаги и верхней частью первой строки таблицы, т.е. если заголовок содержит несколько разрывов строк, он может располагаться в части таблицы вне печати. Это отличается от файлов OpenDocument, где заголовок может соответственно расти [по высоте].


  * **StartPageNumber** [(номер начальной страницы)] должен быть изменен, если печать [документа] не должна начинаться со страницы 1. Для этого параметра [вручную] необходимо добавить опцию `poUseStartPageNumber` в `Options` макета страницы, но обычно это делается автоматически.


  * **ScalingFactor** [(коэффициент масштабирования)] указывается в процентах и может использоваться для уменьшения количества печатаемых страниц. Изменение этого свойства очищает опцию `poFitToPages` [свойства] `Options` макета страницы.


  * В качестве альтернативы ScalingFactor вы также можете использовать **FitHeightToPages** ([подгонять высоту до страниц]) или **FitWidthToPages** ([подгонять ширину до страниц]). Опция `poFitToPages` должна быть активной, чтобы переопределить параметр `ScalingFactor`. `FitHeightToPages` указывает целое количество страниц, на которых должна уместиться вся высота рабочего листа. Соответственно, `FitWidthToPages` может использоваться для определения целого количества страниц, которым должна соответствовать вся ширина рабочего листа. Значение `0` определяет специальное понятие "использовать столько страниц, сколько необходимо". Таким образом, этот вариант может быть установлен для параметра Excel "Устанавливать все столбцы на одной странице", например:


    
    
      MyWorksheet.PageLayout.Options := MyWorksheet.PageLayout.Options + [poFitPages];
      MyWorksheet.PageLayout.FitWidthToPages := 1;     // все столбцы на ширине одной страницы
      MyWorksheet.PageLayout.FitHeightToPages := 0;    // использовать как можно больше страниц
    

  * **Header rows and columns repeated on every printed page** [(заголовки строк и столбцов повторяются на каждой печатной странице)] может быть определена [свойствами] `RepeatedCols` и `RepeatedRows` записей; для повтора [на каждой странице] их элементы `FirstIndex` и `LastIndex` ссылаются на индексы первого и последнего столбца или строки, соответственно. Для определения этих чисел используйте методы `SetRepeatedCols` и `SetRepeatedRows`. Обратите внимание, что второй параметр для последнего индекса может быть опущен при использовании только одной строки или столбца заголовка.


  * **Print ranges** [(диапазон печати)] или **print areas** [(область печати)] (пользуясь терминологией Excel) может использоваться для ограничения печати только в диапазоне ячеек. Используйте методы `AddPrintRange` для определения диапазона ячеек для печати: укажите индексы левого столбца, верхней строки, правого столбца и нижней строки диапазона для печати. Рабочий лист может содержать несколько диапазонов печати.


  * **Copies** [(копии)] определяет, как часто печатается рабочий лист.


  * **Options** [(опции)] определяет дополнительные свойства печати, их имена являются самообучающимися. Они были определены в соответствии с [спецификацией] файлов Excel, некоторые из них не существуют в файлах ODS и там игнорируются.



#### Заголовки и колонтитулы

**Текст заголовков и колонтитулов** может состоять из выровненных по левому краю, центрированных и выровненных по правому краю строк. Добавьте символ `&L`, чтобы указать, что следующая строка должна быть напечатана с выравниванием слева; используйте `&C` соответственно для центрируемых [строк] и `&R` для [строк] с выравниванием по правому краю. Существуют другие символы, которые во время печати будут дополнять [действие символов]: 

  * `&L`: начинает описание **раздела с выравниванием по левому краю** текста заголовка или нижнего колонтитула
  * `&C`: начинает описание **раздела с выравниванием по центру** текста заголовка или нижнего колонтитула
  * `&R`: начинает описание **раздела с выравниванием по правому краю** текста заголовка или нижнего колонтитула
  * `&P`: **номер страницы**
  * `&N`: **количество страниц**
  * `&D`: текущая **дата** печати
  * `&T`: текущее **время** печати
  * `&A`: **имя рабочего листа**
  * `&F`: **имя файла** без пути [к файлу]
  * `&P`: **путь к файлу** без имени файла
  * `&G`: встроенное **изображение** \- используйте методы `AddHeaderImage` или `AddFooterImage`, чтобы указать файл изображения; это также добавляет [символ] `&G` к другим кодам текущего раздела header/footer. Обратите внимание, что не все типы изображений, известные в приложении Office, могут быть приняты. В настоящее время изображение может быть [с расширением] _jpeg_ , _png_ , _gif_ , _bmp_ , _tiff_ , _pcx_ , _svg_ , _wmf_ или _emf_.
  * `&B`: **жирный** вкл/выкл
  * `&I`: **курсив** вкл/выкл
  * `&U`: **подчеркивание** вкл/выкл
  * `&E`: **двойное подчеркивание** вкл/выкл
  * `&S`: **зачеркнутый** вкл/выкл
  * `&H`: **тень** вкл/выкл
  * `&O`: **контур** вкл/выкл
  * `&X`: **верхний индекс** вкл/выкл
  * `&Y`: **нижний индекс** вкл/выкл
  * `&"font"`: начинает использовать **шрифт** с определенным именем, напр., `&"Arial"`
  * `&number`: начинает использовать определенный **размер шрифта** (в точках), напр., `&16`
  * `&Krrggbb`: переключает **цвет шрифта** в определенное двоичное значение указанного цвета, напр., используйте `&KFF0000` для [переключения] в красный.



Массивы `Headers[]`/`Footers[]` предоставляют пространство для использования трех разных верхних и нижних колонтитулов: 

  * `Headers[0]` относится к заголовку, используемому только на **первой странице** , [также] аналогично для `Footers[0]`. Вместо индекса `0` вы можете использовать константу `HEADER_FOOTER_INDEX_FIRST`. Оставьте эту строку пустой, если на первой странице нет специального заголовка/нижнего колонтитула.
  * `Headers[1]` относится к заголовку на страницах с **нечетными номерами страниц** , [также] аналогично для `Footers[1]`. Вместо индекса `1` вы можете использовать константу `HEADER_FOOTER_INDEX_ODD `.
  * `Headers[2]` относится к заголовку на страницах с **четными номерами страниц** , [также] аналогично для `Footers[2]`. Вместо индекса `2` вы можете использовать константу `HEADER_FOOTER_INDEX_EVEN`.



Оставьте строки с индексом 0 и 2 пустыми, если в распечатке всегда должен быть один и тот же верхний/нижний колонтитул. Для большей ясности вы можете использовать константу `HEADER_FOOTER_INDEX_ALL`. Например: 
    
    
      MyWorksheet.PageLayout.Headers[HEADER_FOOTER_INDEX_ALL] := '&C&D &T';           // выровненный по центру заголовок "дата время" на всех страницах 
      MyWorksheet.PageLayout.Footers[HEADER_FOOTER_INDEX_ODD] := '&RPage &P of &N';   // выровненный  по правому краю нижний колонтитул на нечетных страницах "Страница .. из .." 
      MyWorksheet.PageLayout.Footers[HEADER_FOOTER_INDEX_EVEN] := '&LPage &P of &N';  // то же, но на четных страницах с выравниванием слева
    

### Защита

В приложениях Office книги могут быть защищены от непреднамеренных изменений пользователем. fpspreadsheet способен читать и записывать структуры данных, связанные с защитой, но не применяет их. Это означает, например, что ячейки могут быть изменены пользователем, хотя рабочий лист указан как заблокированный. 

Защита осуществляется на трех уровнях: защита рабочей книги, защита рабочего листа и защита элементов. 

#### Защита рабочей книги

`TsWorkbook` содержит набор параметров защиты рабочей книги или документов: 

  * `bpLockRevision`: указывает, что книга заблокирована для ревизии
  * `bpLockStructure`: если этот параметр установлен, рабочие листы в книге нельзя перемещать, удалять, скрывать, скрывать или переименовывать, а новые рабочие листы не могут быть вставлены.
  * `bpLockWindows`: указывает, что окна книги в приложении Office заблокированы. Окна имеют одинаковый размер и положение каждый раз, когда рабочая книга открывается приложением Office.



В отношении защиты рабочей книги используется опция рабочего листа `soPanesProtection`, которая предотвращает изменение панелей рабочего листа, если рабочая книга защищена. 

В зависимости от формата файла могут поддерживаться только некоторые из этих параметров. В этих случаях не поддерживаемые параметры являются общепринятыми значениями по умолчанию. 

#### Защита рабочего листа

`TsWorksheet` содержит аналогичный набор параметров защиты. Всякий раз, когда опция включена в набор `Protection` рабочей книги, соответствующее действие не разрешено и заблокировано: 

  * `spCells`: ячейки в листе защищены. Это зависит от уровня защиты ячейки, может ли конкретная ячейка быть изменена или нет. По умолчанию ни одна ячейка не может быть изменена.
  * `spDeleteColumns`: удаление столбцов не допускается
  * `spDeleteRows`: удалить строки невозможно
  * `spFormatCells`: форматирование ячеек не допускается
  * `spFormatColumns`: столбцы не могут быть отформатированы
  * `spFormatRows`: строки не могут быть отформатированы
  * `spInsertColumns`: не допускается вставка столбцов
  * `spInsertRows`: строки не могут быть вставлены
  * `spInsertHyperlinks`: невозможно вставить новые гиперссылки
  * `spSort`: рабочий лист не может быть отсортирован
  * `spSelectLockedCells`: ячейки, которые заблокированы, больше не могут быть выбраны
  * `spSelectUnlockedCells`: невозможно выбрать даже [те] ячейки, которые разблокированы. Вместе с `spSelectLockedCells` это означает, что выбор на листе заморожен.



Эти уровни защиты становятся активными, если опция `soProtected` добавлена в [набор] `Options` рабочего листа или [посредством] вызова метода `Protect(true)` рабочего листа. 

#### Защита ячейки

Защита ячеек становится активной, когда включена защита рабочего листа. Он управляется набором элементов `TsCellProtection`, которые принадлежат к формату ячейки: 

  * `cpLockCell`: этот параметр определяет, может ли содержимое ячейки быть изменено пользователем. Поскольку по умолчанию ячейки защищенного рабочего листа обычно не могут редактироваться. Чтобы разблокировать некоторые ячейки для пользовательского ввода, опция `cpLockCell` должна быть удалена из защиты этих ячеек.
  * `cpHideFormulas`: предотвращает отображение формул в приложении Office.



Защита ячейки может быть изменена путем вызова метода `WriteCellProtection` рабочего листа. И наоборот, [свойство] `ReadCellProtection` может использоваться для извлечения состояния защиты конкретной ячейки: 
    
    
    // запрос и изменение состояния защиты ячейки A1 (строка=0, столбец=0)
    var
      cell: PCell;
      cellprot: TsCellProtections;
    ...
      // Находим ячейку
      cell := worksheet.FindCell(0, 0);
      // запрос защиты ячейки
      cellprot := worksheet.ReadCellProtection(cell);
      // Разблокировка ячейки для редактирования без изменения видимости формул
      worksheet.WriteCellProtection(cell, cellprot - [cpLockCell]);
      // Сокрытие формулы ячейки и разблокировка ячейки.
      worksheet.WriteCellProtection(cell, [cpHideFormulas]);
    

#### Защита

Защита рабочей книги и рабочего листа может быть обеспечена паролем. Обратите внимание, что эти пароли не шифруют файл (кроме защиты рабочей книги в Excel 2007). В приложениях Office пользователь должен ввести этот пароль, чтобы отключить защиту или изменить элементы защиты. Зашифрованный пароль хранится в записи `CryptoInfo` рабочей книги и рабочих листов соответственно: 
    
    
    type
      TsCryptoInfo = record
        PasswordHash: String;
        Algorithm: TsCryptoAlgorithm;  // caExcel, caSHA1, caSHA256, etc.
        SaltValue: String;
        SpinCount: Integer;
      end;
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** FPSpreadsheet не выполняет никаких расчетов хэширования, запись `CryptoInfo` просто передается от чтения до записи. Это вызывает проблемы, если при чтении и записи используются разные форматы файлов. Компонент пытается обнаружить **несовместимые комбинации**. В этих случаях **защита паролем удаляется** , и в рабочей книге записывается ошибка. 

### Загрузка и сохранение

#### Добавление новых форматов файлов

FPSpreadsheet открыт для любого формата файлов электронных таблиц. В дополнение к встроенным форматам файлов, которые указаны одной из деклараций `sfXXXX`, можно предоставить выделенные блоки чтения и записи для доступа к специальным форматам файлов. 

  * Напишите модуль, реализующий читателя и писателя для нового формата файла. Они должны наследоваться от базовых [типов] `TsCustomSpreadReader` и `TsCustomWriter` соответственно, - оба [типа] реализованы в модуле _fpsReaderWriter_ \- или из одного из более продвинутых, принадлежащих к встроенным форматов файлов.
  * Зарегистрируйте новый читатель/писатель, вызвав функцию `RegisterSpreadFileFormat` в разделе `initialization` этого модуля (реализованной в модуле _fpsRegFileFormats_):


    
    
    function RegisterSpreadFormat(AFormat: TsSpreadsheetFormat; AReaderClass: TsSpreadReaderClass; AWriterClass: TsSpreadWriterClass;
      AFormatName, ATechnicalName: String; const AFileExtensions: array of String): TsSpreadFormatID;
    

  * `AFormat` должно иметь значение `sfUser` для регистрации внешнего формата файла.
  * `AReaderClass` является классом читателя (или nil, если функция чтения не реализована).
  * `AWriterClass` является классом писателя (или nil, если функция записи не реализована).
  * `AFormatName` определяет имя формата, используемого, например, в списке фильтров диалогов открытия файла.
  * `ATechnicalName` определяет более короткое имя формата.
  * `AFileExtensions` представляет собой массив файловых расширений, используемых в файлах. Первый элемент массива обозначает расширение по умолчанию. Расширения должны начинаться с периода, [такого] как в `.xls`.
  * Функция регистрации возвращает числовое значение (`TsSpreadFormatID`), которое может использоваться как идентификатор формата в функциях чтения и записи рабочей книги, которые существуют в перегруженной версии, принимая числовое значение для спецификатора формата. В соглашении со встроенными форматами FormatID отрицательный.
  * Наконец, в вашем приложении добавьте новый модуль в секцию `uses`. Это вызовет функцию регистрации при загрузке устройства и сделает новый формат файла доступным для таблицы FPSpreadsheet.



#### Выбор потока

Рабочие книги загружаются и сохраняются с помощью методов `ReadFromFile` и `WriteToFile`, соответственно (или их потоковыми аналогами, `ReadFromStream` и `WriteToStream`). 

По умолчанию доступ к файлам данных осуществляется с помощью "потока в памяти", что обеспечивает быстрый доступ к файлам. Однако в случае очень больших файлов (например, десятков тысяч строк) в системе может случиться нехватка памяти. Существует два способа отложить переполнение памяти в некоторой степени. 

  * Добавьте элемент `boBufStream` в `Options` рабочей книги. В этом случае для доступа к данным используется **буферизированный поток**. Этот вид потока содержит буфер памяти заданного размера и свопирует (сбрасывает на диск) данные в файл, если буфер становится слишком маленьким.
  * Добавьте элемент `boFileStream` в `Options` рабочей книги. Эта опция позволяет избежать [создания] потоков в памяти целиком и создает временные **файлы** , если это необходимо. Это, однако, самый медленный способ доступа к данным.
  * Если оба параметра установлены, то `boBufStream` игнорируется.
  * Однако на практике эффект выбранных потоков невелик, если память должна быть сохранена.



#### Виртуальный режим

Помимо использования временной памяти при чтении/записи потребление основной памяти происходит во внутренней структуре FPSpreadsheet, которая хранит все данные в памяти. Чтобы преодолеть это ограничение, был введен "виртуальный режим". В этом режиме данные принимаются из источника данных (например, таблицы базы данных) и передаются писателю без сбора на рабочем листе. Понятно, что данные, загруженные в виртуальный режим, не могут отображаться в визуальных элементах управления. Виртуальный режим хорош для преобразования между различными форматами данных. 

Ряд шагов, необходимых для использования этого режима: 

  * Активируйте виртуальный режим, добавив опцию `boVirtualMode` в `Options` рабочей книги.
  * Сообщите писателю таблицы, сколько строк и столбцов должно быть записано. Соответствующими свойствами рабочего листа являются `VirtualRowCount` и `VirtualColCount`.
  * Напишите обработчик для события `OnWriteCellData` рабочего листа. Этот обработчик получает индекс строки и столбца сохраняемой ячейки. Вы должны вернуть значение, которое будет сохранено в этой ячейке. Вы также можете указать ячейку шаблона, которая физически существует в рабочей книге, с которой стиль форматирования копируется в ячейку назначения. Имейте в виду, что при экспорте базы данных вы несете ответственность за продвижение указателя набора данных к следующей записи базы данных при заполнении строки.
  * Вызовите метод `WriteToFile` рабочей книги.



Виртуальный режим также работает для чтения файлов электронных таблиц. 

В папке _example/other_ содержится разработанный образец проекта, демонстрирующий виртуальный режим с использованием случайных данных. Более реалистичные примеры баз данных приведены в _example/db_import_export_ и в главе преобразование большой таблицы базы данных с использованием виртуального режима. 

### Экспорт данных

FPC содержит набор модулей, которые позволяют экспортировать наборы данных в различные форматы (XML, SQL-запросы, DBF-файлы, ...). Существует основной пакет, который позволяет вам выбирать формат экспорта во время разработки или времени выполнения (пакет lazdbexport для Lazarus). 

FPSpreadsheet имеет [свойство] `TFPSExport`, которое подключается к этой системе. Он позволяет экспортировать содержимое набора данных в файл новой электронной таблицы (_.xls_ , _.xlsx_ , _.ods_ , формат wiki) с таблицей на первом листе по умолчанию. Кроме того, если `MultipleSheets` установлено значение `TRUE`, можно скомпоновать несколько листов в отдельные рабочие листы в том же файле. Вы можете дополнительно указать имена полей в качестве ячеек заголовка в первой строке, используя свойство `HeaderRow` в настройках экспорта. Компонент экспорта пытается найти формат номера ячеек в соответствии с типами полей набора данных. 

Для более сложного экспорта вам необходимо вручную запрограммировать решение (см. примеры ниже), но для простой передачи данных/динамического экспорта по запросу пользователя этого модуля, вероятно, будет достаточно. 

Простой пример того, как это работает: 
    
    
    uses
    ...
    fpsexport
    ...
    var
      Exp: TFPSExport;
      ExpSettings: TFPSExportFormatSettings;
      TheDate: TDateTime;
    begin
      FDataset.First; //предположим, что у нас есть набор данных FDataset
      Exp := TFPSExport.Create(nil);
      ExpSettings := TFPSExportFormatSettings.Create(true);
      try
        ExpSettings.ExportFormat := efXLS; // выбираем формат файла
        ExpSettings.HeaderRow := true; // включаем строку заголовка с именами полей
        Exp.FormatSettings := ExpSettings; // применяем настройки для экспорта объекта
        Exp.Dataset:=FDataset; // указываем источник
        Exp.FileName := 'c:\temp\datadump.xls';
        Exp.Execute; // запускаем экспорт
      finally
        Exp.Free;
        ExpSettings.Free;
      end;
    

#### Экспорт в DBF

Вы можете легко сохранить информацию из Excel в dbf. Вот пример из форума. Обратите внимание, что этот пример должен работать с кодировкой Win1251 по умолчанию. Чтобы получить дополнительную информацию, обратитесь к этой теме [[1]](<http://forum.lazarus.freepascal.org/index.php/topic,41636.0.html>)
    
    
    procedure TForm1.ExportToDBF(AWorksheet: TsWorksheet; AFileName: String);
    var
      i: Integer;
      f: TField;
      r, c: Cardinal;
      cell: PCell;
    begin
      DbfGlobals.DefaultCreateCodePage := 1251; //кодировка по умолчанию dbf
      DbfGlobals.DefaultOpenCodePage := 1251; //кодировка по умолчанию dbf
      if Dbf1.Active then Dbf1.Close;
      if FileExists(AFileName) then DeleteFile(AFileName);
    
      Dbf1.FilePathFull := ExtractFilePath(AFileName);
      Dbf1.TableName := ExtractFileName(AFileName);
      Dbf1.TableLevel := 25;  // DBase IV: 4 - наиболее широко используется; или 25 = FoxPro поддерживает nfCurrency
      Dbf1.LanguageID := $C9; //русский язык по умолчанию
      Dbf1.FieldDefs.Clear;
      //ниже находятся поля в excel
      Dbf1.FieldDefs.Add('fam', ftString);
      //добавляем другие поля, которые вы хотите сохранить
    
      Dbf1.CreateTable;
      Dbf1.Open;
    
      for f in Dbf1.Fields do
        f.OnGetText := @DbfGetTextHandler;
    
      // Пропускаем строку 0, которая содержит заголовки
      for r := 1 to AWorksheet.GetLastRowIndex do begin
        Dbf1.Append;
        for c := 0 to Dbf1.FieldDefs.Count-1 do begin
          f := Dbf1.Fields[c];
          cell := AWorksheet.FindCell(r, c);
          if cell = nil then
            f.Value := NULL
          else
            case cell^.ContentType of
              cctUTF8String: f.AsString := UTF8ToCP1251(cell^.UTF8StringValue);
              cctNumber: f.AsFloat := cell^.NumberValue;
              cctDateTime: f.AsDateTime := cell^.DateTimeValue;
              else f.AsString := UTF8ToCP1251(AWorksheet.ReadAsText(cell));
            end;
        end;
        Dbf1.Post;
      end;
    end;
    
    //Эта процедура вызывается так, чтобы текст в ячейках мог корректно отображаться в сетке рабочего листа на форме, иначе отобразятся симовлы '?'
    procedure TForm1.DbfGetTextHandler(Sender: TField; var AText: string; DisplayText: Boolean);
    begin
      if DisplayText then
        AText := CP1251ToUTF8(Sender.AsString);
    end;
    

## Визуальные элементы управления для FPSpreadsheet

Пакет _'laz_fpspreadsheet_visual'_ реализует серию элементов управления, которые имитируют создание визуальных графических приложений: 

  * **TsWorkwookSource** связывает элементы управления с рабочей книгой и уведомляет элементы управления об изменениях в рабочей книге.
  * **[TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid/ru")** реализует управление сеткой с возможностями редактирования и форматирования; он может применяться аналогично TStringGrid.
  * **TsWorkbookTabControl** предоставляет набор вкладок для каждого листа рабочей книги. Это идеальный контейнер для [TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid/ru").
  * **TsCellEdit** соответствует строке редактирования в Excel или Open/LibreOffice. Однако возможно прямое редактирование в сетке.
  * **TsCellIndicator** отображает имя текущей выбранной ячейки; его можно использовать с целью навигации [посредством] ввода строки адреса ячейки.
  * **TsCellCombobox** предлагает выбрать свойства ячейки для определения атрибутов форматирования: имя шрифта, размера шрифта, цвета шрифта, цвета фона.
  * **TsSpreadsheetInspector** является инструментом, главным образом, для отладки; он отображает различные свойства рабочей книги, рабочего листа, значения ячейки и форматирования ячеек, [компонент] аналогичен ObjectInspector Lazarus'а. Однако он доступен только для чтения.
  * В модуле **fpsActions** предусмотрены различные стандартные действия. Примененные к меню или панели инструментов, они упрощают типичные задачи форматирования и редактирования без необходимости писать строки кода.
  * **TsWorkbookChartSource** связывает рабочую книгу с библиотекой [TAChart](<TAChart.md> "TAChart/ru"). Он определяет диапазоны ячеек, из которых диаграмма может получить свои данные. Существует также старый компонент [TsWorksheetChartSource](<../en/TsWorksheetChartSource.md> "TsWorksheetChartSource"), который требует, чтобы все ячейки были на одном листе. По этой причине он не был полностью интегрирован в среду элементов управления FPSpreadsheet и будет удален из библиотеки в будущем.



См. [Учебник FPSpreadsheet: написание приложения для мини-таблиц](<../en/FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application") для получения дополнительной информации и обучения, а также см. demo projects [, чтобы посмотреть] примеры применения этих компонентов. 

## Примеры

Чтобы создать проект, который использует библиотеку fpspreadsheet, добавьте пакет fpspreadsheet_pkg в проект Lazarus или добавьте корневой каталог fpspreadsheet к вашим параметрам компилятора, если используете другую IDE. 

### Модули

Весь пакет FPSpreadsheet состоит из нескольких модулей. Приложение электронной таблицы обычно "использует" следующие модули: 

  * _fpspreadsheet_ : реализует **TsWorkbook** и **TsWorksheet** , а также **Основные методы чтения/записи файлов**
  * _fpstypes_ : объявляет большинство **типов данных** и **констант** , используемых во всех пакетах. **Примечание:** в старых версиях эти объявления содержались в файле fpspreadsheet.pas.
  * модули, реализующие [возможность] чтения/записи для заданного **формата файла** , напр. _xlsbiff8_ для двоичных файлов Excel. Если приложение будет в состоянии обрабатывать все форматы, "используйте" модуль _fpsallformats_.



Следующие модули необходимы только изредка: 

  * _fpsutils_ : набор **полезных функций** , которые иногда [бывают] необходимы (напр., преобразование индексов col/row в Excel-подобную строку адреса ячейки).
  * _fpscell_ : этот модуль требуется, если вы используете **форматирование ячеек напрямую** (например, `cell^.BackgroundColor := scYellow`) вместо вызова соответствующего метода рабочего листа `MyWorksheet.WriteBackgroundColor(cell, scYellow)` в этом примере).
  * _fpsnumformat_ : сосредотачивает все служебные функции, связанные с **форматированием чисел**.



Все остальные модули, вероятно, не нужны на уровне приложений. В случае **визуальных элементов управления spreadsheet** необходимые модули обычно вставляются во время разработки автоматически. 

### Примеры Excel 5

Примечание: этот пример требует [наличия модулей] _avglvltree.pas, lazutf8.pas, asiancodepagefunctions.inc, asiancodepages.inc_ и _lconvencoding.pas_ ([расположенных] в _$(LazarusDir)\components\lazutils\_ ), по крайней мере для fpspreadsheet транковой (для разработчиков) версии Лазаруса 
    
    
    {
    excel5demo.dpr
    
    Демонстрирует, как написать файл Excel 5.x, используя библиотеку fpspreadsheet
    
    Вы можете изменить выходной формат, изменив константу OUTPUT_FORMAT
    
    АВТОР: Felipe Monteiro de Carvalho
    }
    program excel5demo;
    
    {$mode delphi}{$H+}
    
    uses
      Classes, SysUtils, fpstypes, fpspreadsheet, fpsallformats, laz_fpspreadsheet;
    
    const 
      OUTPUT_FORMAT = sfExcel5;
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyFormula: TsRPNFormula;
      MyDir: string;
    begin
      // Инициализация
      MyDir := ExtractFilePath(ParamStr(0));
    
      // Создание рабочей книги
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
    
        // Записываем некоторые номера ячеек
        MyWorksheet.WriteNumber(0, 0, 1.0);
        MyWorksheet.WriteNumber(0, 1, 2.0);
        MyWorksheet.WriteNumber(0, 2, 3.0);
        MyWorksheet.WriteNumber(0, 3, 4.0);
    
        // Записываем формулу E1 = A1 + B1
        MyWorksheet.WriteFormula(0, 4, 'A1+B1');
    
        // Создаем новый рабочий лист
        MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet 2');
    
        // Записываем некоторые строки ячеек
        MyWorksheet.WriteText(0, 0, 'First');
        MyWorksheet.WriteText(0, 1, 'Second');
        MyWorksheet.WriteText(0, 2, 'Third');
        MyWorksheet.WriteText(0, 3, 'Fourth');
    
        // Сохраняем электронную таблицу в файл
        MyWorkbook.WriteToFile(MyDir + 'test' + STR_EXCEL_EXTENSION, OUTPUT_FORMAT);
      finally
        MyWorkbook.Free;
      end;
    end.
    

### Открытие существующей таблицы

Чтобы открыть таблицу, указав конкретный формат для использования, задействуйте [процедуру] `ReadFromFile` с двумя параметрами: 
    
    
    MyWorkbook.ReadFromFile(AFileName, sfExcel5);
    

Также можно вызвать `ReadFromFile` только с одним параметром - именем файла. Тогда в книге будет использоваться расширение [на основе] автоматического определения формата файла. В случае неоднозначного расширения _.xls_ (Excel 2-8) [механизм автодетектирования] просто попробует различные возможности до тех пор, пока не сработает. Хотя типичные шаблоны байтов отпечатков пальцев теперь проверяются, все же возможно, что исключение будет поднято для каждого неправильного формата, если он запускается из среды IDE во время разработки; это не происходит во время выполнения. 
    
    
    MyWorkbook.ReadFromFile(AFileName);
    

### Запись электронной таблицы в файл на основе расширения

Подобно процедуре `ReadFromFile`, также существует процедура `WriteToFile` для определения типа электронной таблицы на основе суффикса имени файла. Она использует процедуру `GetFormatFromFileName` в коде предыдущего раздела, поэтому фактический код прост. Тем не менее, она всегда будет записывать файлы с заданным расширением, используя последний формат, который использует это расширение (например, файлы Excel _.xls_ будут записаны как `sfExcel8`), поэтому, если вы хотите их записать в более раннем формате, то должны использовать базовую процедуру. 

Как и выше, этот код исправляет модуль _fpspreadsheet.pas_. 
    
    
    procedure TsWorkbook.WriteToFile(const AFileName: string; const AOverwriteExisting: Boolean = False); overload;
    var SheetType: TsSpreadsheetFormat;
    begin
      if getFormatFromFileName(AFileName, SheetType) then
        WriteToFile(AFileName, SheetType, AOverwriteExisting)
      else raise Exception.Create(Format(
        '[TsWorkbook.WriteToFile] [Сделана] попытка сохранить электронную таблицу по расширению, но расширение %s недействительно.', [ExtractFileExt(AFileName)]));
    end;
    

### Итерация по всем рабочим листам
    
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      i: Integer;
    begin
      // Здесь загружаем MyWorkbook из файла или создаем его
     
      for i := 0 to MyWorkbook.GetWorksheetCount() - 1 do
      begin
        MyWorksheet := MyWorkbook.GetWorksheetByIndex(i);
        // Делаем что-нибудь с MyWorksheet
      end;
    

### Итерация через ячейки

Первая идея - использовать простой цикл **for-to** : 
    
    
    var
      MyWorksheet: TsWorksheet;
      col, row: Cardinal;
      cell: PCell;
    begin
      for row:=0 to MyWorksheet.GetLastRowIndex do
        for col := 0 to MyWorksheet.GetLastColIndex do
        begin
          cell := MyWorksheet.FindCell(row, col);
          WriteLn(MyWorksheet.ReadAsUTF8Text(cell));
        end;
    end;
    

`FindCell` инициирует поиск ячейки независимо от ранее найденной ячейки. Ячейки, однако, организованы между собой в отсортированной древовидной структуре, и каждая ячейка "знает" своих предыдущих и следующих соседей. Более того, `FindCell` тратит время на поиск несуществующих ячеек в случае редко занятых рабочих листов. В общем, более эффективно использовать синтаксис **for-in** , который использует внутреннюю древовидную структуру с помощью специальных **счетчиков**. Обратите внимание, что существуют также специализированные счетчики для поиска по строкам, столбцам или диапазонам ячеек: 
    
    
    var
      MyWorksheet: TsWorksheet;
      cell: PCell;
    begin
      // Поиск по всем ячейкам
      for cell in Myworksheet.Cells do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      // Поиск только в столбце 0
      for cell in MyWorksheet.Cells.GetColEnumerator(0) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      // Поиск только в строке 2
      for cell in MyWorksheet.Cells.GetRowEnumerator(2) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    
      //Поиск только в диапазоне A1:C2 (строки 0..1, столбцы 0..2)
      for cell in MyWorksheet.Cells.GetRangeEnumerator(0, 0, 1, 2) do
        WriteLn(MyWorksheet.ReadAsText(cell));
    end;
    

### Преобразование базы данных в электронную таблицу

Самое простое решение - использовать DatasetExport component ([прим.перев](</User:Zoltanleo> "User:Zoltanleo"): ссылка на компонент не корректна). 

Если вам нужно больше контролировать процесс, используйте что-то вроде: 
    
    
    program db5xls;
     
    {$mode delphi}{$H+}
     
    uses
      Classes, SysUtils, 
      // добавить модули базы данных
      fpstypes, fpspreadsheet, fpsallformats;
     
    const OUTPUT_FORMAT = sfExcel5;
     
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyDatabase: TSdfDataset;
      MyDir: string;
      i, j: Integer;
    begin
      // Инициализация
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Открываем базу данных
      MyDatabase := TSdfDataset.Create;
      MyDatabase.Filename := 'test.dat';
      // Добавляем здесь описание таблицы
      MyDatabase.Active := True;
    
      // Создаем электронную таблицу
      MyWorkbook := TsWorkbook.Create;
      MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
     
      // Записываем имена полей
      for i := 0 to MyDatabase.Fields.Count - 1 do
        MyWorksheet.WriteText(0, i, MyDatabase.Fields[i].FieldName);
    
      // Записываем все ячейки в рабочий лист
      MyDatabase.First;
      j := 0;
      while not MyDatabase.EOF do
      begin
        for i := 0 to MyDatabase.Fields.Count - 1 do
          MyWorksheet.WriteText(j + 1, i, MyDatabase.Fields[i].AsString);
    
        MyDatabase.Next;
        Inc(j);
      end;
    
      // Закрываем базу данных
      MyDatabase.Active := False;
      MyDatabase.Free;
    
      // Сохраняем электронную таблицу в файл
      MyWorkbook.WriteToFile(MyDir + 'test' + STR_EXCEL_EXTENSION, OUTPUT_FORMAT);
      MyWorkbook.Free;
    end.
    

### Преобразование большой таблицы базы данных в электронную таблицу с использованием виртуального режима

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Пример программы в [папке] _examples\db_import_export_ демонстрирует использование виртуального режима для экспорта наборов данных в файлы электронных таблиц.

Мы хотим записать большую таблицу базы данных в файл электронной таблицы. Первая строка электронной таблицы - это отображение названий полей жирным шрифтом и серым фоном. 

Обычно FPSpreadsheet загружает полное представление электронной таблицы в память, поэтому мы будем использовать виртуальный режим для минимизации использования памяти. 
    
    
    type
      TDataProvider = class;
    
    var
      MyWorkbook: TsWorkbook;
      MyWorksheet: TsWorksheet;
      MyDatabase: TSdfDataset;
      MyDir: string;
      MyHeaderTemplateCell: PCell;
      DataProvider: TDataProvider;
    
    // Внедрите TDataProvider здесь - см. ниже ...
    
    begin
      // Инициализация
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Открываем базу данных
      MyDatabase := TSdfDataset.Create;
      try
        MyDatabase.Filename := 'test.dat';
        // Добавляем описание таблицы здесь
        MyDatabase.Active := True;
    
        // Создаем электронную таблицу
        MyWorkbook := TsWorkbook.Create;
        try
          MyWorksheet := MyWorkbook.AddWorksheet('My Worksheet');
    
          // Создаем шаблон ячейки для строки заголовка, мы хотим, 
          // чтобы заголовок [был начертан] жирным и на сером фоне
          // Шаблон ячейки может быть, где угодно, в рабочей книге, просто выберите ячейку A1.
          MyWorksheet.WriteFontStyle(0, 0, [fssBold]);
          MyWorksheet.WriteBackgroundColor(0, 0, scGray);
          // Нам понадобится эта ячейка снова и снова, поэтому давайте сохраним указатель на нее в переменной
          MyHeaderTemplateCell := MyWorksheet.Find(0, 0);
     
          // Входим в виртуальный режим
          MyWorkbook.Options := MyWorkbook.Options + [boVirtualMode];
    
          // Определяем количество столбцов - нам нужен столбец для каждого поля
          MyWorksheet.VirtualColCount := MyDatabase.FieldCount;
    
          // Определяем количество строк - нам нужна каждая запись, плюс 1 строка для заголовка
          MyWorksheet.VirtualRowCount := MyDatabase.RecordCount + 1;
    
          // Связываем обработчик событий, который передает данные из базы данных в "писатель" электронной таблицы
          MyWorksheet.OnWriteCellData := @DataProvider.WriteCellData;
    
          // Записываем все ячейки в файл Excel8
          // Данные, которые должны быть записаны, указаны в обработчике событий OnWriteCellData.
          MyWorkbook.WriteToFile(MyDir + 'test.xls', sfExcel8);
    
        finally
          // Освобождаем
          MyWorkbook.Free;
        end;
    
      finally
        // Закрываем базу данных и освобождаем ее
        MyDatabase.Active := False;
        MyDatabase.Free;
      end;
    
    end.
    

Осталось написать обработчик событий для `OnWriteCellData`. Для консольной программы выше мы устанавливаем конкретный класс поставщика данных (в визуальной программе обработчик событий также может быть методом любой формы): 
    
    
    type
      TDataProvider = class
        procedure WriteCellData(Sender: TsWorksheet; ARow, ACol: Cardinal; var AValue: variant; var AStyleCell: PCell);
      end;
    
    procedure TDataProvider.WriteCellData(Sender: TsWorksheet; ARow, ACol: Cardinal; AValue: variant; var: AStyleCell: PCell);
    begin
      // Сначала обработаем строку заголовка:
      if ARow = 0 then begin
        // Значение, которое должно быть записано в электронную таблицу, - это имя поля.
        AValue := MyDatabase.Fields[ACol].FieldName;
        // Форматирование определено в HeaderTemplateCell.
        AStyleCell := MyHeaderTemplateCell;
        //Перемещаемся на первую запись
        MyDatabase.First;
      end else begin
        // Значение, которое должно быть записано в электронную таблицу, является значением записи в поле, соответствующем столбцу.
        // Никаких особых требований к форматированию --> оставляем AStyleCell по умолчанию (nil).
        AValue := MyDatabase.Fields[ACol].AsVariant;
        // Передвигаем курсор базы данных, если было записано последнее поле записи
        if ACol = MyDatabase.FieldCount-1 then MyDatabase.Next;
      end;
    end;
    

### Преобразование между двумя форматами электронных таблиц
    
    
    program ods2xls;
     
    {$mode delphi}{$H+}
     
    uses
      Classes, SysUtils, 
      fpstypes, fpspreadsheet, fpsallformats, fpspreadsheet_pkg;
     
    const 
      INPUT_FORMAT = sfOpenDocument;
      OUTPUT_FORMAT = sfExcel8;
     
    var
      MyWorkbook: TsWorkbook;
      MyDir: string;
    begin
      // Инициализация
      MyDir := ExtractFilePath(ParamStr(0));
     
      // Преобразование электронной таблицы
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorkbook.ReadFromFile(MyDir + 'test.ods', INPUT_FORMAT);
        MyWorkbook.WriteToFile(MyDir + 'test.xls', OUTPUT_FORMAT);
      finally
        MyWorkbook.Free;
      end;
    end.
    

### Чтение и запись CSV-файлов

CSV-файлы (CSV = значения, разделенные запятыми) - это текстовые файлы без метаданных. Поэтому требуется дополнительная информация для правильного чтения записи из/в рабочий лист. Глобальная запись`CSVParams` предоставляет основные настройки для этой цели: 
    
    
    type
      CSVParams: TsCSVParams = record    // W = запись, R = чтение, RW = чтение/запись
        SheetIndex: Integer;             // W: Индекс листа, который должен быть записан
        LineEnding: TsCSVLineEnding;     // W: Спецификация для конца строки, который должен быть записан
        Delimiter: Char;                 // RW: Разделитель столбцов
        QuoteChar: Char;                 // RW: Символ для квотированного ([заключенного в двойные кавычки]) текста
        Encoding: String;                // RW: Кодировка файла
        DetectContentType: Boolean;      // R: попытка конвертации строк в типы контента 
        NumberFormat: String;            // W: если пустые записи чисел, как на листе; в противном случае используется этот формат
        AutoDetectNumberFormat: Boolean; // R: автоматически определяет десятичный/тысячный разделитель, используемый в числах
        TrueText: String;                // RW: Строка для логического TRUE
        FalseText: String;               // RW: Строка для логического FALSE
        FormatSettings: TFormatSettings; // RW: добавляет параметры для преобразования
      end;
    

Эта запись содержит поля, которые оцениваются как только для чтения, только для записи или для обоих - см. прилагаемые комментарии. 

Общей ситуацией является чтение файла с использованием десятичного разделителя чисел, который отличается от десятичного разделителя системы: предположим, что вы находитесь в европейской системе, в которой десятичный разделитель является запятой, но файл csv, который должен быть прочитан, берется из машины, которая использует десятичную точку. Предположим также, что файл содержит символы табуляции в качестве разделителя столбцов вместо запятой по умолчанию. В этом случае просто установите `CSVParams.FormatSettings.DecimalSeparator` в `'.'`, а `CSVParams.Delimiter` в `#9` (символ TAB) перед чтением файла: 
    
    
    uses
      fpstypes, fpspreadsheet, fpscsv;
    var
      MyWorkbook: TsWorkbook;
    begin
      CSVParams.FormatSettings.DecimalSeparator := '.';
      CSVParams.Delimiter := #9;
      MyWorkbook := TsWorkbook.Create;
      try
        MyWorkbook.ReadFromFile('machine-data.csv', sfCSV);
      finally
        MyWorkbook.Free;
      end;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если вы хотите дать пользователю возможность интерактивного изменения записи `CSVParams`, посмотрите в [код] модуля _scsvparamsform.pas_ демоверсии _spready_ , который предоставляет готовый к использованию диалог.

### Примеры проектов в папке установки fpspreadsheet

Установку FPSpreadsheet сопровождает куча проектов с примерами. Их можно найти в папке "examples". Вот краткое описание этих типовых проектов 

  * **db_import_export** представляет собой пример, показывающий, как экспортировать большую таблицу базы данных в файл электронной таблицы с использованием виртуального режима или **TFPSExport**. Он также показывает импорт электронной таблицы в базу данных с использованием виртуального режима.
  * **fpsspeedtest** сравнивает влияние формата файла и различных параметров чтения/записи на скорость записи и чтения очень больших файлов электронных таблиц. Опять же, сначала запустите тест записи, который создает тестовые файлы, используемые для теста чтения.
  * Папка **read_write** : 
    * **excel2demo** содержит программы командной строки для записи и чтения xls-файлов Excel 2.x. Пожалуйста, запустите демонстрацию записи перед чтением демонстрации, чтобы создать необходимый файл электронной таблицы.
    * **excel5demo** аналогично excel2demo, но для xls-файлов Excel 5.
    * **excel8demo** аналогично excel2demo, но для xls-файлов Excel 97-2003.
    * **csvdemo** аналогично excel2demo, но для CSV-файлов.
    * **htmldemo** аналогично excel2demo, но для HTML-файлов (в настоящее время только на запись).
    * **ooxmldemo** аналогично excel2demo, но для новых xlsx-файлов Excel .
    * **opendocdemo** аналогично excel2demo, но для ods-файлов OpenOffice/LibreOffice.
    * **wikitabledemo** аналогично excel2demo, но для файлов wiki-таблиц. Обратите внимание, что **пример записи** в настоящее время записывает формат, который **пример чтения** не может понять.
  * **other** : простые консольные программы, показывающие различные аспекты пакета fpspreadsheet. Посмотрите _readme.txt_ для более подробной информации.
  * Папка **visual** : 
    * **fpschart** показывает применение [TsWorksheetChartSource](<../en/TsWorksheetChartSource.md> "TsWorksheetChartSource") и TsWorkbookChartSource, а также взаимодействие с плагином [TAChart](<TAChart.md> "TAChart/ru").
    * **fpsctrls** и **fpsctrls_no_install** создает графическое приложение электронной таблицы с минимальным количеством написанного кода; последнее демо полезно для тестирования, потому что не требует установки пакетов FPSpreadsheet. Пошаговые инструкции о том, как делается _fpsctrls_ , можно найти в [ FSpreadsheet tutorial](<../en/FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application").
    * **fpsgrid** и **fpsgrid_no_install** показывает основное применение [TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid/ru") без использования компонента TsWorkbookSource; последнее демо не требует установки пакета FPSpreadsheet.
    * **wikitablemaker** это небольшое приложение для создания кода, который будет использоваться для таблиц на страницах вики. Введите данные в [TsWorksheetGrid](<TsWorksheetGrid.md> "TsWorksheetGrid/ru") (или загрузите существующий файл электронной таблицы), перейдите на страницу "Code", чтобы увидеть сгенерированный код вики, нажмите "Скопировать в буфер обмена" и вставьте код на страницу вики.
  * **spready** представляет собой расширенное приложение всей библиотеки, отображающее файлы электронных таблиц с форматированием, редактированием ячеек и т.д. Поскольку это автономное приложение, оно было перенесено в папку _applications/spready_ из _Lazarus Components and Code Library_.



## Загрузка

### Система контроля версий (Subversion)

Вы можете загрузить FPSpreadsheet с использованием приложений subversion и следующей командной строки: 
    
    
    svn checkout svn://svn.code.sf.net/p/lazarus-ccr/svn/components/fpspreadsheet fpspreadsheet
    

#### Журнал изменений SVN

  * Скрыть (и показать) строки и столбцы



#### Несовместимые изменения

  * (пока ничего)



### Стабильные версии

Вы можете найти все выпуски FPSpreadsheet на [sourceforge](<https://sourceforge.net/projects/lazarus-ccr/files/FPSpreadsheet/>). 

  


#### Version 1.10.1

Это последний стабильный релиз. 

##### Change log (with respect to v1.8x)

  * **Workbook, worksheet and cell protection** (read/write in BIFF2/BIFF5/BIFF8/OOXML/ODS, write in ExcelXML).
  * New package **laz_fpspreadsheet_crypto** to decipher the encryption for worksheet-protection in xls files. Requires DCPcrypt.
  * TsWorksheetGrid can **display embedded images**.
  * **Drag and drop** in TsWorksheetGrid
  * New, **highDPI-aware** component palette icons.
  * New workbook optiond **boAbortReadingOnFormulaError** and **boIgnoreFormulas**.
  * Formulas with **references to other sheets** , i.e. `'=Sheet1!A1+Sheet2!A2'`



##### Incompatible changes

  * The field **FormulaValue** has been removed from the cell record. The cell formula now can be retrieved by calling `Worksheet.ReadFormula(cell)`.



#### Version 1.8

##### Change log (with respect to v1.6x)

  * **"Rich-text" formatting** of label cells, i.e. assignment different fonts to groups of characters within the cell text. For this purpose, HTML codes (such as `<B>...</B>`) can be embedded in the cell text to identify the parts with different font (--> `TsWorksheet.WriteTextAsHTML`).
  * **Searching** for cells with specified content in worksheet or workbook.
  * Support for reading and writing of **HTML format**
  * Support for writing of the **ExcelXML format** (Excel XP and 2003)
  * Ability to use **user-provided file reader/writer classes** to extend FPSpreadsheet to new file formats.
  * Readers and writers now support all the **line styles** of Excel8 and OOXML.
  * xls, xlsx and ods readers/writers now support the **active worksheet** and **selected cell**.
  * Ability to write to/read from the system's **clipboard** for copy & paste using the visual controls.
  * Support for **print ranges** and **repeated header rows and columns** in the Office applications.
  * Support for **embedded images** (currently only writing to xlsx and ods, no reading).
  * Improved compatibility of **TsWorksheetGrid** with TStringGrid (`Cells[Col,Row]` property). Standalone application as an advanced StringGrid replacement.
  * Support for **Right-to-left mode** in TsWorksheetGrid. In addition to the system-wide RTL mode, there are also parameters `BiDiMode` in the Worksheet and cells allowing to controls text direction at worksheet and cell level individually, like in Excel or LibreOffice Calc.
  * Support of several **units** for specification of column widths and row heights.
  * The library now supports **localization** using po files. Translations are welcome.
  * **Zoom factor** read and written by the worksheet, and applied by the TsWorksheetGrid.
  * Support of **column and row formats**
  * Support of **hidden** worksheets



##### Incompatible changes

  * VirtualMode was changed in order to be able to treat worksheets of the same workbook differently. `VirtualRowCount` and `VirtualColCount` are now properties of the worksheet, and similarly, the event handler `OnWriteCellData`. In older versions, these properties had belonged to the workbook.
  * The worksheet methods `ReadAsUTF8Text` and `WriteUTF8Text` have been renamed to `ReadAsText` and `WriteText`, respectively. The old ones are still available and marked as deprecated; they will be removed in later versions.
  * The public properties of `TsWorksheetGrid` using a `TRect` as parameter were modified to use the Left, Top, Right, Bottom values separately.
  * The `PageLayout` is a class now, no longer a record. As a consequence, some array properties cannot be set directly any more, use the corresponding methods instead.
  * Most of the predefined color constants were marked as deprecated; only the basic EGA colors will remain.
  * Unit fpsNumFormatParser is integrated in fpsNumFormat. Old code which "uses" fpsNumFormatParser must "use" fpsNumFormat now.
  * The source files of the laz_fpspreadsheet, laz_fpspreadsheet_visual and laz_fpspreadsheetexport_visual packages have been moved to separate folders in order to resolve some occasional compilation issues. Projects which do not use the packages but the path to the sources must adapt the paths.



  


#### Version 1.6

##### Change log (with respect to v1.4.x)

  * TsWorkbookChartSource is a new component which facilitates creation of charts from non-contiguous spreadsheet data in various worksheets. It interfaces to a workbook via tha WorkbookSource component. In the long run, it will replace the older TsWorksheetChartSource which required contiguous x/y data blocks in the same worksheet.
  * Major reconstruction of the cell record resulting in strong reduction of memory consumption per cell (from about 160 bytes per cell down to about 50)
  * Implementation of a record helper for the TCell which simplifies cell formatting (no need to set a bit in UsedFormattingFields any more, automatic notification of visual controls)
  * Comments in cells
  * Background fill patterns
  * Hyperlinks
  * Enumerators for worksheet's internal AVLTrees for faster iteration using a for-in loop.
  * Formatting of numbers as fractions.
  * Improved number format parser for better recognition of Excel-like number formats.
  * Page layout (page margins, headers, footer, used for only when printing in the Office applications - no direct print support in fpspreadsheet!)
  * Improved color management: no more palettes, but direct rgb colors. More pre-defined colors.
  * A snapshot of the wiki documentation is added to the library as chm help file.



##### Incompatible changes

  * All type declarations and constants are moved from fpspreadsheet.pas to the new unit **fpstypes.pas**. Therefore, most probably, this unit has to be added to the uses clause.
  * Because fpspreadsheet supports now **background** fill patterns the cell property `BackgroundColor` has been replaced by `Background`. Similarly, the `UsedFormattingFields` flag `uffBackgroundColor` is called `uffBackground` now.
  * Another `UsedFormattingFields` flag has been dropped: **`uffBold`**. It is from the early days of fpspreadsheet and has become obsolete since the introduction of full font support. For achieving a bold type-face, now call `MyWorksheet.WriteFont(row, col, BOLD_FONTINDEX)`, or `Myworksheet.WriteFontStyle(row, col, [fssBold])`.
  * **Iteration through cells** using the worksheet methods `GetFirstCell` and `GetNextCell` has been removed - it failed if another iteration of this kind was called within the loop. Use the new `for-in` syntax instead.
  * Support for **shared formulas** has been reduced. The field `SharedFormulaBase` has been deleted from the TCell record, and methods related to shared formulas have been removed from TsWorksheet. Files containing shared formulas can still be read, the shared formulas are converted to multiple normal formulas.
  * The **color palettes** of previous versions have been abandoned. `TsColor` is now a `DWord` representing the rgb components of a color (just like TColor does in the _graphics_ unit), it is not an index into a color palette any more. The values of pre-defined colors, therefore, have changed, their names, however, are still existing. The workbook functions for palette access have become obsolete and were removed.



#### Version 1.4

##### Change log (with respect to v1.2.x)

  * Full support for **string formulas** ; calculation of RPN and string formulas for all built-in formulas either directly or by means of registration mechanism. Calculation occurs when a workbook is saved (activate workbook option `boCalcBeforeSaving`) or when cell content changes (active workbook option `boAutoCalc`).
  * **Shared formulas** (reading for sfExcel5, sfExcel8, sfOOXML; writing for sfExcel2, sfExcel5, sfExcel8).
  * Significant **speed-up** of writing of large spreadsheets for the xml-based formats (ods and xlsx), speed up for biff2; **speedtest demo** program
  * **VirtualMode** allowing to read and write very large spreadsheet files without loading entire document representation into memory. Formatting of cells in VirtualMode.
  * Demo program for database export using virtual mode and TFPSExport.
  * Added db export unit allowing programmatic exporting datasets using **TFPSExport**. Similar export units are e.g. [fpdbfexport](<../en/fpdbfexport.md> "fpdbfexport"), [fpXMLXSDExport](<../en/fpXMLXSDExport.md> "fpXMLXSDExport").
  * Reader for **xlsx** files, now fully supporting the same features as the other readers.
  * Reader/writer for **CSV files** based on [CsvDocument](<../en/CsvDocument.md> "CsvDocument").
  * **Wikitables writer** supports now most of the fpspreadsheet formatting options (background color, font style, font color, text alignment, cell borders/line styles/line colors, merged cells, column widths, row heights); new "wikitablemaker" demo
  * **Insertion and deletion of rows and columns** into a worksheet containing data.
  * Implementation of **sorting** of a worksheet.
  * Support of **diagonal "border" lines**
  * **Logging of non-fatal error messages** during reading/writing (TsWorksheet.ErrorMsg)
  * **Merged** cells
  * **Registration** of currency strings for automatic conversion of strings to **currency values**
  * A set of **visual controls** (TsWorkbookSource, TsWorkbookTabControl, TsSpreadsheetInspector, TsCellEdit, TsCellIndicator, TsCellCombobox, in addition to the already-existing TsWorksheetGrid) and pre-defined standard actions to facilitate [creation of GUI applications](<../en/FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application").
  * **Overflow cells** in TsWorksheetGrid: label cells with text longer than the cell width extend into the neighboring cell(s).



##### Incompatible changes

  * The option `soCalcBeforeSaving` now belongs to the workbook, no longer to the worksheet, and has been renamed to `boCalcBeforeSaving` (it controls automatic calculation of formulas when a workbook is saved).
  * The workbook property `ReadFormulas` is replaced by the option flag `boReadFormulas`. This means that you have to add this flag to the workbook's `Options` in order to activate reading of formulas.
  * With full support of string formulas some features related to RPN formulas were removed: 
    * The field `RPNFormulaResult` of TCell was dropped, as well as the element `cctRPNFormula` in the `TsContentType` set.
    * Sheet function identifiers were removed from the `TsFormulaElement` set, which was truncated after `fekParen`.
    * To identify a sheet function, its name must be passed to the function `RPNFunc` (instead of using the now removed `fekXXXX` token). In the array notation of the RPN formula, a sheet function is identified by the new token `fekFunc`.
    * The calling convention for registering user-defined functions was modified. It now also requires the Excel ID of the function (see "OpenOffice Documentation of Microsoft Excel Files", section 3.11, or unit _xlsconst_ containing all token up to ID 200 and some above).
    * Code related to RPN formulas was moved to a separate unit, `fpsRPN`. Add this unit to the uses clause if you need RPN features.



#### Wiki documentation of old releases

This wiki page is work in progress and updated whenever a new feature is added; therefore, its state corresponds to the svn trunk version of the package. If you work with an older stable version please use these "historic" wiki versions: 

  * Version 1.10.1: [http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=118771](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=118771>)
  * Version 1.10: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=118441](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=118441>)
  * Version 1.8: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=107616](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=107616>)
  * Version 1.6.2: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=100723](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=100723>)
  * Version 1.6: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=91469](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=91469>)
  * Version 1.4: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=85299](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=85299>)
  * Version 1.2 and 1.2.1: [wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=81375](<http://wiki.lazarus.freepascal.org/index.php?title=FPSpreadsheet&oldid=81375>)



## Установка

  * Если вам нужны только компоненты без GUI: в Lazarus: Package/Open Package File, выберите **laz_fpspreadsheet.lpk** , нажмите Compile. Теперь пакет известен Lazarus (и должен, например, отображаться в Package/Package Links). Теперь вы можете добавить зависимость от `laz_fpspreadsheet` в параметрах вашего проекта и `fpspreadsheet` в раздел uses модуля проекта, который должен его использовать.
  * Если вы также хотите использовать визуальные компоненты ([TsWorksheetGrid](<../en/TsWorksheetGrid.md> "TsWorksheetGrid") и [TsWorksheetChartSource](<../en/TsWorksheetChartSource.md> "TsWorksheetChartSource")): Package/Open Package File, выберите **laz_fpspreadsheet_visual.lpk** , нажмите Compile, затем нажмите Use, Install и следуйте подсказкам [с предложением] пересобрать Lazarus с новым пакетом. Бросьте необходимые компоненты сетки/диаграммы на ваши формы, как обычно.
  * Если вы хотите иметь визуальный компонент для экспорта набора данных: Package/Open Package File, выберите **laz_fpspreadsheetexport_visual.lpk** , нажмите Compile, затем нажмите Use, Install и следуйте подсказкам [с предложением] пересобрать Lazarus с новым пакетом. Бросьте необходимые компоненты экспорта с вкладки **Data Export** на вашу форму, как обычно.
  * FPSpreadsheet разработан с последней стабильной версией fpc (в настоящее время fpc 3.0.2). Мы лишь иногда проверяем старые версии.
  * Основная функциональность электронных таблиц работает с версиями Lazarus до версии 1.0. Однако некоторые визуальные средства управления или демонстрационные программы требуют более новых версий. Обновите свой Lazarus, если у вас старая версия и возникли проблемы.



### Параметры компилятора

Вот список **условных определений** , которые могут быть активированы для настройки некоторых режимов работы пакетов и/или их компиляции с более старыми версиями Lazarus/FPC: 

  * `FPS_DONT_USE_CLOCALE`: В Unix-системах модуль `clocale` автоматически добавляется в раздел uses _fpspreadsheet.pas_. Этот модуль устанавливает параметры локализации, необходимые для языковых зависимостей, и форматов даты и времени. Однако это добавляет зависимость от библиотеки C к пакету [[2]](<http://lazarus-ccr.sourceforge.net/docs/rtl/clocale/index.html>). Если это не требуется, определите `FPS_DONT_USE_CLOCALE`.
  * `FPS_VARISBOOL`: для fpspreadsheet требуется функция VarIsBool, которая была введена [с версии] fpc 2.6.4. Если используются более старые версии FPC, определите `FPS_VARISBOOL`. Не сохраняйте неопределенный вариант для текущей версии FPC.
  * `FPS_LAZUTF8`: fpspreadsheet требует некоторых функций из модуля LazUTF8, которые были введены [с версии] Lazarus 1.2. Если используется более старая версия Lazarus, определите `FPS_LAZUTF8`. Не изменяйте текущую версию Lazarus.



Все эти определения собираются во включенном файле _fps.inc_. 

## Поддержка и отчетность об ошибках

Рекомендуемое место для обсуждения FPSpreadsheet и получения поддержки [по интересующим вопросам находится тут] [Lazarus Forum](<http://forum.lazarus.freepascal.org/index.php?board=49.0>). 

Отчеты об ошибках должны быть отправлены на [Lazarus/Free Pascal Bug Tracker](<http://bugs.freepascal.org/>); пожалуйста, указывайте [с пометкой] проект "Lazarus-CCR". 

## Текущие достижения

### Достижения в поддержке содержимого ячеек

Формат | Несколько  
страниц | Поддержка  
юникода | Поддержка  
чтения | Поддержка  
записи | Текст | Число | Строковая  
формула | RPN  
формула | Ссылки  
на 3D-ячейки | Дата/Время | Комментарии | Гиперссылки | Изображения +++ | Защита   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
CSV files | Нет | Да + | Работает ++ | Работает ++ | Работает ++ | Работает ++ | Недоступно | Недоступно | Недоступно | Работает ++ | Недоступно | Недоступно | Недоступно | Недоступно   
Excel 2.x | Нет | Нет * | Работает ** | Работает | Работает | Работает | Работает | Работает *** | Недоступно | Работает **** | Работает | Недоступно | Недоступно | Работает   
Excel 5.0 (Excel 5.0 and 95) | Да | Нет * | Работает ** | Работает | Работает | Работает | Работает | Работает *** | Работает | Работает **** | Работает | Недоступно | Недоступно | Работает   
Excel 8.0 (Excel 97- 2003) | Да | Да | Работает ** | Работает | Работает | Работает | Работает | Работает *** | Работает | Работает **** | Только чтение | Работает | Не работает | Работает   
Excel OOXML | Да | Да | Работает ** | Работает | Работает | Работает | Работает *** | Работает | Работает | Работает **** | Работает | Работает | Только запись | Работает   
OpenDocument | Да | Да | Работает ** | Работает | Работает | Работает | Работает *** | Работает | Работает | Работает **** | Работает | Работает | Работает | Работает   
HTML | Нет | Да | Работает ++ | Работает ++ | Работает ++ | Работает ++ | Недоступно | Недоступно | Недоступно | Работает ++ | Недоступно | Работает | Не работает | Недоступно   
Wikitable files (Mediawiki) | Нет | Да | планируется | Работает ++ | Работает ++ | Работает ++ | Недоступно | Недоступно | Недоступно | Работает ++ | Недоступно | Недоступно | Не работает | Недоступно   
  
(+) Зависит от файла.  
(++) Нет поддержки "true" формата чисел, поскольку файл не содержит информации о форматировании числа. Но числовой формат, используемый в настоящее время в электронной таблице, понятен.  
(+++) Только очень простая поддержка изображений: без преобразований, без обрезки, без манипуляций с изображениями.  
(*) В форматах, которые не поддерживают Unicode, данные сохраняются по умолчанию как **ISO 8859-1** (Latin 1). Вы можете изменить кодировку в `TsWorkbook.Encoding`. Обратите внимание, что FPSpreadsheet предлагает процедуры чтения и записи [в кодировке] UTF-8, но данные могут быть преобразованы в ISO при чтении или записи на диск. Будьте осторожны, потому что символы, которые не соответствуют выбранной кодировке, будут потеряны в этих операциях. Замечания здесь применимы только для форматов, которые не поддерживают Unicode.  
(**) Некоторые ячейки могут быть пустыми из-за отсутствующих или [до сих пор] не реализованных форматов чисел и текста.  
(***) Это формат, в котором формулы записываются в файл (определяется дизайном формата файла).  
(****) Поддерживается запись всех форматов. Однако некоторые редкие пользовательские форматы могут быть неправильно распознаны. **BIFF2** в дизайне поддерживает только встроенные форматы. 

### Достижения опций форматирования

Доступны некоторые параметры форматирования: 

Формат | Выравнивание  
текста | Вращение  
текста | Шрифт | Rich  
text | Границы | Поддержка  
цвета | Задний  
фон | Перенос  
слов | Размер  
столбца и строки | Числовой  
формат | Слияние  
ячеек | Макет  
страницы | Печать  
диапазонов | Изображения  
шапки/подвала  
[таблицы] | Формат  
столбца/строки | Скрытие  
столбцов/строк   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
CSV files | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно | Недоступно   
Excel 2.x | Работает* | Недоступно | Работает | Недоступно | Работает | Работает | Работает** | Недоступно | Работает | Работает | Недоступно | Работает | Недоступно | Недоступно | Работает | Недоступно   
Excel 5.0  
(Excel 5.0 and 95) | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Недоступно | Работает | Работает | Недоступно | Работает | Работает   
Excel 8.0  
(Excel 97 - XP) | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Не работает | Работает | Работает   
Excel OOXML (xlsx) | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Только запись | Работает | Работает   
OpenDocument | Работает | Работает | Работает | Работает | Работает | Работает | Работает*** | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает | Работает   
HTML (+) | Работает | баги | Работает | Работает | баги | Работает | Работает **** | Только запись | Только запись | Недоступно | Работает | Недоступно | Недоступно | Не работает | Только запись | предстоит сделать   
Wikitable (Mediawiki) | Только запись | Недоступно | Только запись | Не работает | Только запись | Только запись |  | Только запись | Только запись | Недоступно | Только запись | Недоступно | Недоступно | Не работает | Только запись | предстоит сделать   
  
(Недоступно) Функционал недоступен для этого формата по существу.   
(*) BIFF2 поддерживает только горизонтальное выравнивание текста, вертикальное выравнивание игнорируется.  
(**) BIFF2 не поддерживает цвет фона; вместо этого используется черно-белый фон с пунктиром.  
(***) OpenDocument поддерживает только однородные фоны; Вместо этого записывается цвет заливки, интерполированный между цветами переднего и заднего плана.  
(****) Только однородный цвет фона, без стилей заполнения.  
(+) HTML-ридер не поддерживает стили. Поскольку автор использует стили, эти файлы не считываются правильно. 

### Достижения [реализации] пользовательских интерфейсов рабочей книги/рабочего листа

Были добавлены дополнительные опции для интерфейса содержимого файла с помощью `TsWorksheetGrid`: 

Формат | Скрытие   
линий сетки | Скрытие   
заголовков | Заморозка   
панелей | Активный   
лист/ячейка | Масштабирование | режим BiDi   
---|---|---|---|---|---|---  
Excel 2.x | Работает | Работает | Не работает | Недоступно | Недоступно | Недоступно   
Excel 5.0 (Excel 5.0 and 95) | Работает | Работает | Работает | Работает | Работает | Недоступно   
Excel 8.0 (Excel 97 - XP) | Работает | Работает | Работает | Работает | Работает | Работает   
Excel OOXML | Работает | Работает | Работает | Работает | Работает | Работает   
OpenDocument | Работает | Работает | Работает | Работает | Работает | Работает   
HTML | Только запись | Только запись | Недоступно | Недоступно | Недоступно | Не работает   
  
### Список [запланированных] дел

Примечание: этот список является предварительным, поддерживается разработчиками и может меняться без предварительного уведомления. Если вы заинтересованы в содействии, пожалуйста, свяжитесь с нами и отправьте патч - спасибо! 

  * Узнать, почему файлы BIFF2 повреждаются при сохранении с замороженными рядами/столбцами.
  * ~~Добавить форматы строк и столбцов~~
  * Добавить поддержку чтения для файлов таблиц wiki (Mediawiki)
  * Сбой чтения xls для некоторых неправильно написанных xls-файлов (которые Excel может читать), см. <http://forum.lazarus.freepascal.org/index.php/topic,25624.0.html>.
  * ~~Улучшение поддержки цвета: соответствующие палитре цвета в настоящее время могут меняться от файла к файлу.~~
  * Добавить встроенные изображения.
  * Исправить запись комментариев ячейки в файлах BIFF8.



**Долгосрочные:**

  * Предоставить более общий пользовательский интерфейс для fpspreadsheet ([реализация механизма] setter/getter и public-свойств вместо методов Read*/Write*, MyWorksheet.Cells [ARow, ACol]), сделать TCell классом, избегая указателей на PCell.
  * ~~Сохранить форматирование в списке формата книги, а не в ячейке, чтобы уменьшить использование памяти.~~
  * Использовать файловое хранилище строк BIFF8 вместо хранения строк в ячейках (должно уменьшить использование памяти в случае часто повторяющихся строк)
  * Повторное использование чтений ooxml и ods, основанных на SAX/TXmlReader вместо DOM, чтобы уменьшить использование больших файлов в памяти.
  * ~~Добавить блок и компонент fpspreadsheetexport на вкладку "Data Export", аналогичный демке[fpdbfexport](<../en/fpdbfexport.md> "fpdbfexport") FPSpreadsheetexport предпочтительно со всеми компонентами экспортных форматов.~~ Найти способ регистрации формата экспорта во все форматы (смотрите, как lazreport pdf export выполняет аналогичную процедуру).



## Changelog

Changes in the development version that have not yet been released are already documented on this page. 

  * 13 August 2018:   
**Version 1.10.1** bug fix release, based on revision 6598.
  * 6 July 2018:   
**Version 1.10** release, based on svn revision 6547.
  * 16 June 2017:   
**Version 1.8.3**. Fixes compilation with Lazarus 1.8. New, highDPI-aware component palette icons
  * 11 April 2017:   
**Version 1.8.2** , based on svn revision 5832. Fixes writing distorted images due to incorrect row height calculation.
  * 2 March 2017:   
**Version 1.8** , based on svn revision 5781.
  * 13 April 2016:   
**Version 1.6.2** released, based on svn revision 4619. Fixes integer overflow in formulas ([Issue #29999](<https://bugs.freepascal.org/view.php?id=29999>)).
  * 28 June 2015:   
**Version 1.6** released, based on svn release 4106. Improved memory consumption, comments, hyperlinks, background fill patterns, cell enumerator, fraction number format, page layout, modified color management, offline wiki version included.
  * 13 March 2015:   
**Version 1.4.3** released. Fixes formula calculation in worksheet grid.
  * 31 January 2015:  
**Version 1.4.2** released. Fixes incomplete reading of ods files with repeated cells ([Issue #0027389](<https://bugs.freepascal.org/view.php?id=0027389>))
  * 26 January 2015:  
**Version 1.4.1** released. A bugfix release for 1.4 that fixes incorrect streaming of TsWorksheetGrid and reading errors of biff5 and biff8 readers.
  * 31 December 2014:  
**Version 1.4** released (based on subversion revision 3856 with full formula support, xlsx reader, csv reader and writer, wikitables writer, speed enhancement, virtual mode, db export component, row and column insertion/deletion, sorting, merged cells, visual controls in addition to (improved) fpspreadsheet grid.
  * 3 September 2014:  
**Version 1.2.1** released. This is a bugfix release for 1.2 that fixes a bug in spreadsheetGrid ([Issue #26521](<https://bugs.freepascal.org/view.php?id=26521>))
  * 26 June 2014:  
**Version 1.2** released (based on subversion revision 3241) with fpsgrid improvements, date/time cell support for .xls, .ods, vastly improved cell formatting support, improved formula support, unit tests.
  * Jan 2013:  
**Version 1.1** released with all improvements up to revision 2621
  * Nov 2011:  
Released **version 1.0** , which includes OO XML generation support with bold formatting supported and also a lot of improvements from the last years
  * Jan 2009:  
Implemented a cross-platform support for OLE file. Now Excel 5.0 files can be created in any operating system. Adds read support for Excel 2.1
  * Feb 2008:  
**Initial commit** to lazarus-ccr with write support for Excel 2.1, Excel 5.0 (Windows only) and experimental OOXML and OpenDocument



## Лицензия

LGPL с исключением статического связывания. Это та же лицензия, что и в библиотеке компонентов Lazarus. 

## Ссылки

### Wiki ссылки

  * [Office Automation](<../en/Office_Automation.md> "Office Automation")
  * [CsvDocument](<../en/CsvDocument.md> "CsvDocument")
  * [RPN formulas in FPSpreadsheet](<../en/RPN_Formulas_in_FPSpreadsheet.md> "RPN Formulas in FPSpreadsheet")
  * [FPSpreadsheet: List of formulas](<../en/FPSpreadsheet__List_of_formulas.md> "FPSpreadsheet: List of formulas")
  * [FPSpreadsheet tutorial: Writing a mini spreadsheet application](<../en/FPSpreadsheet_tutorial__Writing_a_mini_spreadsheet_application.md> "FPSpreadsheet tutorial: Writing a mini spreadsheet application")



### Внешние ссылки

  * [Microsoft OLE Document Format](<http://sc.openoffice.org/compdocfileformat.pdf>)
  * [Excel file format description](<http://sc.openoffice.org/excelfileformat.pdf>)
  * буфер файлов *.xls Excel и *.ppt PowerPoint, написанный на Python - очень удобно перечислять все содержимое BIFF-файлов (напр., ./xls-dump.py file.xls) - <http://cgit.freedesktop.org/libreoffice/contrib/mso-dumper/>. Аналогичным приложением является "BIFFExplorer", который можно найти в папке _applications_ ccr.
  * Иконки, используемые в демонстрационных программах: 
    * [silk icons](<http://www.famfamfam.com/lab/icons/silk/>)
    * [fugue icons](<http://p.yusukekamiyamane.com/>)

---

_Source: [https://wiki.freepascal.org/FPSpreadsheet/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/FPSpreadsheet/ru)_
