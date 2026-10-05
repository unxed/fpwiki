# CSV

│ **[English (en)](<../en/CSV.md> "CSV")** │  **русский (ru)** │    
****

## Contents

  * 1 Обзор
  * 2 CSV и SDF
  * 3 Реализации
    * 3.1 CsvDocument
    * 3.2 DelimitedText
      * 3.2.1 Формат SDF
    * 3.3 SDFDataset
      * 3.3.1 Файлы с примерами TSdfDataset и TFixedDataset
      * 3.3.2 Использование наборов данных: пример кода
    * 3.4 Экспорт данных
    * 3.5 CSV компоненты Jan
    * 3.6 ZMSQL



## Обзор

CSV означает **Comma-Separated Values** (значения, разделенные запятыми) и является популярным форматом файлов, который, к сожалению, не полностью стандартизирован. 

Это текстовый формат файла с 

  * полями данных, разделенными запятыми (или, в некоторых случаях, другими символами, такими как табуляции и точки с запятой)
  * строкой заголовка, в которой перечислены имена полей (которой может и не быть)
  * данными полей, содержащих разделители, которые могут быть запрещенными или быть заключенными в кавычки (чаще всего это знак двойной кавычки)
  * окончаниями строк (#13 и/или #10), которые могут или не могут быть разрешены в данных полей



RFC4180 (см.[здесь](<http://www.rfc-editor.org/rfc/rfc4180.txt>)) пытается кодифицировать и стандартизировать существующую практику; имеет смысл соответствовать этому стандарту при записи данных CSV (и принимать все данные RFC4180 при чтении). 

Другая, альтернативная, спецификация может быть найдена [здесь](<http://www.creativyst.com/Doc/Articles/CSV/CSV01.htm#FileFormat>)

Образец фрагмента CSV: 
    
    
    FirstName,Surname,DOB,Remarks
    Jim,Weston,19560818,"Also known as ""The Butcher"""
    Alice,Cooper,19760312,""
    

Как видно, здесь используется строка заголовка, как и квотирование с использованием двойных кавычек. 

Пакеты электронных таблиц, такие как Microsoft Excel и OpenOffice/LibreOFfice Calc, могут экспортировать и импортировать из этого формата. Однако, поскольку Microsoft Excel может интерпретировать некоторые поля, такие как поля даты, по-разному в зависимости от языкового стандарта операционной системы пользователя, возможно целесообразнее найти альтернативные способы передачи данных (например, используя код [FPSpreadsheet](<../en/FPSpreadsheet.md> "FPSpreadsheet")). 

## CSV и SDF

Delphi (и FreePascal) имеют очень похожий (но не идентичный) формат SDF. См. [SDF](<../en/SDF.md> "SDF") для получения более подробной информации. 

## Реализации

### CsvDocument

[CsvDocument](<CsvDocument.md> "CsvDocument/ru") является надежной реализацией как CSV-формата RFC 4180 ,так и альтернативного CSV-формата Creativyst/Excel. Он предлагает как линейный, так и документный доступ. Рекомендуется для использования с FPC/Lazarus. См. [CsvDocument](<CsvDocument.md> "CsvDocument/ru"). 

### DelimitedText

TStringList предлагает свойство `DelimitedText`. Оно разбивает строку текста на отдельные поля. Обратите внимание, однако, что `DelimitedText` должен быть в SDF-формате, специфичном для Delphi, который очень похож на CSV, но не полностью соответствует RFC4180. 

Совет: при чтении данных CSV установите для свойства _StrictDelimiter_ значение `true`. 

При записи CSV-данных установите для _StrictDelimiter_ значение `false` и выведите свойство _DelimitedText_. Тут есть одна странность, заключающаяся в том, что, например, символы табуляции удаляются при записи данных с использованием `StrictDelimiter:=false`

#### Формат SDF

См. [формат SDF](<../en/SDF.md> "SDF")

### SDFDataset

FreePascal предлагает [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet/ru"), который ~~хранит данные в формате SDF~~. Примечание: FPC 2.6.x и более ранние версии хранят данные sdfdataset в формате, который не является полностью CSV или SDF-совместимым. Sdfdataset планируется переписать для хранения данных в формате CSV. 

Как указано, [SDF](<../en/SDF.md> "SDF") отличается от CSV. В зависимости от разновидности CSV этот формат может быть достаточно близок к тому, что ожидает от него читающее приложение. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** TSDFDataset, скорее всего, не будет работать, по крайней мере, на Windows CE/Windows Mobile на базе ARM, см. [багрепорт](<http://bugs.freepascal.org/view.php?id=17871>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Ноябрь 2012: SDFDataset должен был быть _(но еще не был)_ переопределен для использования формата RFC4180 CSV. См., например, [багрепорт](<http://bugs.freepascal.org/view.php?id=22980>)

#### Файлы с примерами TSdfDataset и TFixedDataset

  * Пример SDF-файла



    Ниже приведен пример базы данных для TSdfDataset. Обратите внимание, что в первой строке указаны имена полей и мы используем запятые в качестве разделителей:
    
    
    ID,NAMEEN,NAMEPT,HEIGHT,WIDTH,PINS,DRAWINGCODE
    1,resistor,resistor,1,1,1,LINE
    2,capacitor,capacitor,1,1,1,LINE
    3,transistor npn,transistor npn

  * Пример файла TFixedDataset



    Каждая запись занимает фиксированное количество пробелов, и если поле меньше, пробелы должны использоваться для заполнения оставшегося размера.
    
    
    Name = 15 chars; Surname = 15 chars; e_mail = 20 chars;
    Piet           Pompies                  piet@pompies.net

#### Использование наборов данных: пример кода

Иногда полезно создать набор данных и работать с ним полностью в коде, и следующий код сделает это точно. Обратите внимание на некоторые особенности TSdfDataset/TFixedDataset: 

  * Максимальный размер строк в базе данных может составлять около 300. Проблема в процессе решения.
  * Необходимо добавить определения полей. Некоторые наборы данных могут заполнить эту информацию самостоятельно из файла базы данных.
  * Нужно установить `FirstLineAsSchema` в `true`, чтобы указать, что первая строка содержит имена и позиции полей.
  * Свойство `Delimiter` содержит разделитель для полей. Будет невозможно использовать этот символ в строках в базе данных. Точно так же в настоящее время не будет возможности иметь символа окончания строк в базе данных, поскольку они отмечают изменение между записями. Это можно преодолеть, подставив нужную запятую или окончание строки с другим редко используемым символом, например **#**. При отображении данных на экране все символы **#** могут быть преобразованы в окончания строк и наоборот при сохранении данных в базе данных. Здесь будет полезна процедура `ReplaceString`.


    
    
    uses sdfdata, db;
    
    constructor TComponentsDatabase.Create;
    var
      FDataset: TSdfDataset;
    begin
      inherited Create;
    
      FDataset := TSdfDataset.Create(nil);
      FDataset.FileName := vConfigurations.ComponentsDBFile;
      FDataset.FileMustExist := false; // существующий CSV-файл не требуется
    
      // Не обязательно с TSdfDataset:
      //  FDataset.TableName := STR_DB_COMPONENTS_TABLE;
      //  FDataset.PrimaryKey := STR_DB_COMPONENTS_ID;
    
      // Добавляем определения полей
      FDataset.FieldDefs.Add('ID', ftString);
      //необходимо добавить схему, чтобы sdfdataset правильно записал заголовок csv:
      FDataset.Schema.Add('ID'); 
      FDataset.FieldDefs.Add('NAMEEN', ftString);
      FDataset.Schema.Add('NAMEEN'); 
      FDataset.FieldDefs.Add('NAMEPT', ftString);
      FDataset.Schema.Add('NAMEPT'); 
      FDataset.FieldDefs.Add('HEIGHT', ftString);
      FDataset.Schema.Add('HEIGHT'); 
      FDataset.FieldDefs.Add('WIDTH', ftString);
      FDataset.Schema.Add('WIDTH'); 
      FDataset.FieldDefs.Add('PINS', ftString);
      FDataset.Schema.Add('PINS'); 
      FDataset.FieldDefs.Add('DRAWINGCODE', ftString);
      FDataset.Schema.Add('DRAWINGCODE'); 
    
      // Необходимо для TSdfDataset
      FDataset.Delimiter := ',';
      FDataset.FirstLineAsSchema := True;
    
      FDataset.Active := True;
    
      // Устанавливаем начальную запись
      CurrentRecNo := 1;
      FDataset.First;
    end;
    

При использовании [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet/ru") следует помнить, что `RecNo`, хоть и реализован, но не работает как способ перемещения по набору данных при чтении или записи записей. Стандартные процедуры навигации, такие как `First`, `Next`, `Prior` и `Last`, работают как и положено, поэтому вам нужно использовать их, а не `RecNo`. 

Если вы привыкли использовать абсолютные номера записей для навигации по базе данных, вы можете реализовать свою собственную версию `RecNo`. Объявите глобальную переменную `longint` с именем `CurrentRecNo`, которая будет содержать текущее значение `RecNo`. Помните, что эта переменная будет иметь то же соглашение, что и `RecNo`, поэтому первая запись имеет номер 1 (а не начинается с нуля). После активации базы данных инициализируйте базу данных для первой записи с помощью `TSdfDataset.First` и установите `CurrentRecNo:= 1`. 

Пример кода: 
    
    
    {@@
      Перемещение к нужной записи с использованием TDataset.Next и TDataset.Prior. 
      Это позволяет избегать использования TDataset.RecNo, который не обеспечивает надежную навигацию в любом наборе данных.
    
      @param AID Указывает номер записи. Первая запись имеет номер 1
    }
    procedure TComponentsDatabase.GoToRec(AID: Integer);
    begin
      // Мы находимся перед искомой записью, двигаемся вперед
      if CurrentRecNo < AID then
      begin
        while (not FDataset.EOF) and (CurrentRecNo < AID) do
        begin
          FDataset.Next;
          FDataset.CursorPosChanged;
          Inc(CurrentRecNo);
        end;
      end
      // Мы находимся после искомой записи, двигаемся назад
      else if CurrentRecNo > AID  then
      begin
        while (CurrentRecNo >= 1) and (CurrentRecNo > AID) do
        begin
          FDataset.Prior;
          FDataset.CursorPosChanged;
          Dec(CurrentRecNo);
        end;
      end;
    end;
    

### Экспорт данных

Функциональность экспорта базы данных FreePascal/Lazarus (например, TCSVExporter на вкладке «Data Export») предлагает функцию экспорта CSV для наборов данных. 

### CSV компоненты Jan

См. [JCSV (Jans CSV Components)](<../en/JCSV_\(Jans_CSV_Components\).md> "JCSV \(Jans CSV Components\)"). 

### ZMSQL

[ZMSQL](<../en/ZMSQL.md> "ZMSQL") хранит данные в файлах, разделенных точкой с запятой (используя SDF?).

---

_Source: [https://wiki.freepascal.org/CSV/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/CSV/ru)_
