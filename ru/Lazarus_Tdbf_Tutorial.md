# Lazarus Tdbf Tutorial

│ [**Deutsch (de)**](</Lazarus_Tdbf_Tutorial/de> "Lazarus Tdbf Tutorial/de") │  [**English (en)**](<../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") │  [**español (es)**](</Lazarus_Tdbf_Tutorial/es> "Lazarus Tdbf Tutorial/es") │  [**français (fr)**](</Lazarus_Tdbf_Tutorial/fr> "Lazarus Tdbf Tutorial/fr") │  [**português (pt)**](</Lazarus_Tdbf_Tutorial/pt> "Lazarus Tdbf Tutorial/pt") │  **русский (ru)** │  [**中文（中国大陆）‎ (zh_CN)**](</Lazarus_Tdbf_Tutorial/zh_CN> "Lazarus Tdbf Tutorial/zh CN") │    
****

## Contents

  * 1 Обзор
    * 1.1 Что нам понадобится
    * 1.2 Что обеспечивает TDbf
  * 2 Как создать новую таблицу базы данных
    * 2.1 Установка пути
    * 2.2 Выбор TableLevel
    * 2.3 Добавление полей
    * 2.4 Идем дальше и создаем!
  * 3 Как добавить индексы в таблицу
  * 4 Соберем все вместе и получим...
  * 5 Внешние индексные файлы
  * 6 Как присоединить TDbf к компонентам доступа к данным
  * 7 Упаковка и перестройка таблиц
  * 8 Отношение главной/подчиненной таблиц
  * 9 Пример приложения - DB Browser
  * 10 Необходимо учесть
  * 11 См. также



## Обзор

Это руководство описывает основы разработки баз данных с использованием компонента [TDbf](<https://tdbf.sf.net>) (by Micha Nelissen) в Lazarus. Дополнительная документация по TDbf также доступна. Эта страница была создана [ Tony Maro](</User:Tonymaro> "User:Tonymaro") но другие контрибьюторы также приветствуются! 

Документацию компонента TDbf в формате pdf смотрите на [SourceForge](<https://sourceforge.net/project/showfiles.php?group_id=34085&package_id=26371>). Полезно сохранить этот pdf и обращаться к нему по мере прочтения этой статьи. 

### Что нам понадобится

Этот документ был написан с версией Free Pascal Compiler 2.3.1 /Lazarus 0.9.23. 

Пакет DbfLaz установлен по умолчанию. 

### Что обеспечивает TDbf

TDbf обеспечивает доступ к таблицам баз данных dBase и FoxPro для Lazarus (и других IDE), учитывая чтение, запись и создание dBase III +, dBase IV, Visual dBase VII и FoxPro таблиц. Все это делается без использования дополнительных библиотек или движков базы данных. Просто разместите компонент TDbf на вашей форме, и Вы имеете мгновенный доступ в окружающей среде базы данных на разных платформах. TDbf работает и в Windows и в Linux используя Лазарус. 

## Как создать новую таблицу базы данных

Поскольку для Lazarus нет приложения наподобии "Database Desktop", мы должны создать новую базу данных в коде. 

### Установка пути

Хорошей мыслью будет выделение для базы данных Вашего приложения собственного каталога. Это упростит резервирование данных. Есть два варианта установки пути. Вы можете установить полный путь используя свойство FilePathFull, или установить путь относительно пути приложения с помощью свойства FilePath. Нипример, установка "FilePath" во время выполнения к "data/" будет использовать субдиректорию data только в директории исполнимого файла. Установка свойства "FilePathFull" к "/var/data/" будет размещать точно по указанному пути, игнорируя расположение приложения. 

### Выбор TableLevel

По умолчанию, TDbf создает таблицы dBase IV. Хотя это и наиболее совместимо, некоторые возможности, которые Вам могут понадобиться, не поддерживаются. Для поддержки автоинкрементных полей вы должны использовать кое-что поновее. Типы таблиц могут быть следующими:: 

  * 3 dBase III+
  * 4 dBase IV
  * 7 Visual dBase VII
  * 25 FoxPro



Вы выбираете тип таблицы установкой свойства TableLevel соответственно. 

### Добавление полей

Создание полей для Вашей новой таблицы во время выполнения в большинстве следует старому стандарту Delphi. Как только вы установили свои значения для свойств FilePath, TableLevel, и TableName, манипулируйте свойством FieldDefs чтобы определить структуру. Например: 
    
    
    MyDbf.FilePathFull := '/location/to/my/data';
    MyDbf.TableLevel := 7;
    MyDbf.TableName := 'customers.dbf'; // примечание: действительно ли необходимо .dbf?
    With MyDbf.FieldDefs do begin
      Add('Id', ftAutoInc, 0, True);
      Add('Name', ftString, 80, True);
    End;
    

Типы полей определены как: 

  * ftUnknown
  * **ftString**
  * **ftSmallInt**
  * **ftInteger**
  * **ftWord**
  * **ftBoolean**
  * **ftFloat**
  * **ftCurrency** (TableLevel 25)
  * **ftBCD** (TableLevel 25)
  * **ftDate**
  * ftTime
  * **ftDateTime**
  * **ftBytes** (TableLevel 25)
  * ftVarBytes
  * **ftAutoInc** (TableLevel 7 or 25)
  * **ftBlob**
  * **ftMemo**
  * ftGraphic
  * ftFmtMemo
  * ftParadoxOle
  * **ftDBaseOle**
  * ftTypedBinary
  * ftCursor
  * **ftFixedChar**
  * **ftWideString**
  * **ftLargeInt**
  * ftADT
  * ftArray
  * ftReference
  * ftDataSet
  * ftOraBlob
  * ftOraClob
  * ftVariant
  * ftInterface
  * ftIDispatch
  * ftGuid
  * ftTimeStamp
  * ftFMTBcd



Выделенные жирным типы полей поддерживаются 

### Идем дальше и создаем!

Как только вы определили поля, которые желаете использовать в вашей новой таблице, вы можете идти дальше и создать таблицу: 
    
    
       MyDbf.CreateTable;
    

## Как добавить индексы в таблицу

Если Ваша база данных состоит из большого числа записей, Вам может понадобиться определить индексы чтобы осуществлять более быстрый поиск по таблице. Чтобы изменить структуру индексов таблицы, мы должны иметь эксклюзивный доступ к таблице - который мы так или иначе име ли бы, создавая таблицу. 
    
    
           MyDbf.Exclusive := True;
           MyDbf.Open;
    

Теперь, нам необходимо просто добавить индексы. 
    
    
           MyDbf.AddIndex('custid', 'Id', [ixPrimary, ixUnique]);
           MyDbf.AddIndex('custname','Name', [ixCaseInsensitive]);
           MyDbf.Close;
    

## Соберем все вместе и получим...

Следующий пример создает новую таблицу "customers" в коде. Конечно, это необходимо выполнить только один раз, и после этого всего лишь ОТКРЫВАТЬ таблицу, не создавая её заново ;-) 
    
    
    {$MODE OBJFPC}
    Program DatabaseTest;  
    { We will require the following units be in the USES clause: }
     uses Dbf, db, Dbf_Common;                                   
    { The Dbf is put there when you drop the TDbf on a form...   }
    { but you will need db for the DataSet object and Dbf_Common }
    { for things such as the field type definitions              }
    var
      MyDbf: TDbf;
    begin
      MyDbf := TDbf.Create(nil);
      try
        { use relative path to "data" directory }
        MyDbf.FilePath := 'data/'; 
        { we want to use Visual dBase VII compatible tables }
        MyDbf.TableLevel := 7;
        MyDbf.Exclusive := True;
        MyDbf.TableName := 'customers.dbf';
        With MyDbf.FieldDefs do begin
          Add('Id', ftAutoInc, 0, True);
          Add('Name', ftString, 80, True);
        End;
        MyDbf.CreateTable;
        MyDbf.Open;
        MyDbf.AddIndex('custid', 'Id', [ixPrimary, ixUnique]);
        { add a secondary index }
        MyDbf.AddIndex('custname','Name', [ixCaseInsensitive]);
        MyDbf.Close;
      finally
        MyDbf.Free;
      end;
    end;
    

## Внешние индексные файлы

TDbf также поддерживает хранение вторичных индексов в раздельных файлах. Это может помочь в случае если ожидается очень большой размер базы данных. Файлы вторичных индексов создаются идентично обычным индексам, но с добавлением расширения файла '.ndx': 
    
    
        MyDbf.AddIndex('custname.ndx','Name', [ixCaseInsensitive]);
    

  
Каждый раз при открытии TDbf, необходимо загрузить файл индексов: 
    
    
        MyDbf.OpenIndexFile('custname.ndx');
    

  
Также индексы должны быть описанны включая расширение: 
    
    
        MyDbf.IndexName := 'custname.ndx';
    

  
Индексные файлы могут быть упакованы простым использованием: 
    
    
        MyDbf.CompactIndexFile('custname.ndx');
    

## Как присоединить TDbf к компонентам доступа к данным

Пример, описанный ранее, показывает, как создать новую таблицу базы данных программно. Использование этой таблицы более чем простое. 

Компоненты доступа к данным в Lazarus (как например TDbEdit) указывают на компонент TDataSource используя свои свойства "DataSource" и "DataField". Компонент TDataSource обрабатывает соединение между движком базы данных и компонентами доступа к данным. Затем компонент TDataSource указывает на компонент TDbf используя его свойство "DataSet". Связь напоминает следующее: 
    
    
    TDbEdit-------
                 |
    TDbEdit------|-->TDataSource-->TDbf
                 |
    TDbNavigator--
    

  
Убедитесь, что Вы установили свойства FilePath (или FilePathFulll), TableLevel, и TableName вашего компонента TDbf перед вызовом 
    
    
    TDbf.Active := True;
    

  
Можно сказать гораздо больше о программировании баз данных в Lazarus, и я рекомендую хорошую книгу или две о программировании баз данных в Delphi, так как основные понятия одинаковые. Я постоянно обращаюсь к своей копии "Delphi 2 Unleashed" потому что концепции и базовый код не меняются уже 8 лет. 

## Упаковка и перестройка таблиц

Когда запись удаляется, на самом деле она физически остается в таблице. Периодически Вы должны "паковать" таблицу чтобы восстановить потерянное место. Это может быть сделано установкой эксклюзивного режима доступа. 
    
    
    MyDbf.Exclusive := True;
    MyDbf.Open;
    MyDbf.PackTable;
    // давайте также перестроим индексы
    MyDbf.RegenerateIndexes;
    MyDbf.Close;
    MyDbf.Exclusive := False;
    

## Отношение главной/подчиненной таблиц

Настоящая сила программирования баз данных начинается тогда, когда Вы имеете несколько таблиц, которые ссылаются друг на друга. Пока TDbf не поддерживает ссылочную целостность, но поддерживает представление "главная/подчиненная" между таблицами TDbf. 

Предположим, что есть две взаимосвязанные таблицы, например: 
    
    
    [customers]
    Id       <----|
    Name          |
    Phone         |
    Address       |
                  |  CustID в накладных ссылается на первоначальное  поле клиента
    [invoices]    |
    Id            |
    Amount        |
    CustID   -----|  * Это поле проиндексировано как "idxcustid"
    

  
Если вы хотели показать все накладные для предоставленного клиента, подчиненная таблица (invoices) может оставаться синхронизированной с главной таблицей (customers) автоматически. 

На компоненте накладные TDbf устанавливают следующее: 
    
    
    InvDbf.IndexName := 'idxcustid'; // our field that will match the customers table ID
    InvDbf.MasterSource := dsCustomers; // datasource that is linked to the customers TDbf
    InvDbf.MasterFields := 'Id'; // field on the customers table we are matching against our index
    

## Пример приложения - DB Browser

Я написал простое приложение использующее TDbf для открытия и показа таблиц базы данных используя компонент dbGrid. Исполняемая программа Linux вместе с исходниками проекта, которые должны отлично компилироваться и в Windows, доступна по адресу: [tony.maro.net](<http://tony.maro.net/mod.php?mod=downloads&op=showcat&id=3&level=1>)

## Необходимо учесть

В настоящее время нет никакой поддержки ссылочной целостности или шифрования внутри .dbf файлов. 

## См. также

  * [Guide for usage of Tdbf component/ru](<Guide_for_usage_of_Tdbf_component.md> "Guide for usage of Tdbf component/ru")
  * [Example: TDbf (creating table and indexes, selecting of index)](<../en/Example__TDbf_\(creating_table_and_indexes,_selecting_of_index\).md> "Example: TDbf \(creating table and indexes, selecting of index\)")

---

_Source: [https://wiki.freepascal.org/Lazarus_Tdbf_Tutorial/ru](https://web.archive.org/web/20191214182026/https://wiki.freepascal.org/Lazarus_Tdbf_Tutorial/ru)_
