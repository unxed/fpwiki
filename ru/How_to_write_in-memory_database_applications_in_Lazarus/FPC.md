# How to write in-memory database applications in Lazarus/FPC

│ **[English (en)](<../../en/How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")** │  **[français (fr)](</How_to_write_in-memory_database_applications_in_Lazarus/FPC/fr> "How to write in-memory database applications in Lazarus/FPC/fr")** │  **[日本語 (ja)](</How_to_write_in-memory_database_applications_in_Lazarus/FPC/ja> "How to write in-memory database applications in Lazarus/FPC/ja")** │  **русский (ru)** │    
****  
  
---  
[**Databases portal**](<../../en/Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<../../en/Databases.md> "Databases")
  * [Libraries](<../../en/Database_libraries.md> "Database libraries")
  * [Field types](<../../en/Database_field_type.md> "Database field type")
  * [Controls](<../../en/Data_Controls_tab.md> "Data Controls tab")
  * [FAQ](<../../en/Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [SQL how-to](<../../en/SqlDBHowto.md> "SqlDBHowto")
  * [Working With TSQLQuery](<../../en/Working_With_TSQLQuery.md> "Working With TSQLQuery")
  * [In-memory database applications](<../../en/How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")

Tutorials/practical articles: 

  * [Overview](<../../en/Lazarus_Database_Overview.md> "Lazarus Database Overview")
  * [0 - Database set-up](<../../en/SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<../../en/SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<../../en/SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<../../en/SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<../../en/SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<../../en/SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<../../en/Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<../../en/MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<../../en/mssqlconn.md> "mssqlconn") \- [Postgres](<../../en/postgres.md> "postgres") \- [Interbase](<../../en/Firebird.md> "Firebird") \- [Firebird](<../../en/Firebird.md> "Firebird") \- [Oracle](<../../en/Oracle.md> "Oracle") \- [ODBC](<../../en/ODBCConn.md> "ODBCConn") \- [Paradox](<../../en/TParadox.md> "TParadox") \- [SQLite](<../../en/SQLite.md> "SQLite") \- [dBASE](<../../en/Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<../../en/MS_Access.md> "MS Access") \- [Zeos](<../../en/Zeos_tutorial.md> "Zeos tutorial")  
  
## Contents

  * 1 Введение
  * 2 Сохранение MemDataset в постоянные файлы
  * 3 Автогенератор первичных ключей
  * 4 Обеспечение ссылочной целостности
  * 5 Известные проблемы
  * 6 TBufDataSet
  * 7 Сортировка DBGrid по событию OnTitleClick для TBufDataSet
  * 8 Сортировка нескольких столбцов в grid
  * 9 ZMSQL
  * 10 Авторство



## Введение

Существуют определенные обстоятельства, когда наборы данных в памяти имеют смысл. Если вам нужна быстрая, однопользовательская, не критически важная база данных, отличная от SQL, без транзакций, [TMemDataset](<../../en/TMemDataset.md> "TMemDataset") может удовлетворить ваши потребности. 

Некоторые преимущества: 

  * Быстрое выполнение. Поскольку вся обработка выполняется в памяти, данные не сохраняются на жестком диске до тех пор, пока это не будет задано явно. Память, безусловно, быстрее, чем жесткий диск.
  * Нет необходимости во внешних библиотеках (нет файлов .so или .dll), нет необходимости в установке сервера.
  * Код является мультиплатформенным и может быть скомпилирован в любой ОС.
  * Поскольку все программирование выполняется в Lazarus/FPC, такие приложения проще в обслуживании. Вместо того, чтобы постоянно переключаться с внутреннего программирования на внешнее, используя MemDatasets, вы можете сосредоточиться на своем коде Pascal.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** позже в этой статье будет представлен BufDataset. [TBufDataset](<../../en/TBufDataset.md> "TBufDataset") часто является лучшим выбором, чем [TMemDataset](<../../en/TMemDataset.md> "TMemDataset")

Я проиллюстрирую, как программировать реляционные не-SQL базы данных в памяти, сосредоточив внимание на обеспечении целостности отношений и фильтрации, моделировании основных полей с автоинкрементом и т.п. 

Эта страница поделится с вами тем, что я узнал, экспериментируя с TMemDatasets. Возможно даже, что есть какой-то другой, более эффективный способ сделать это. Если это так, пожалуйста, не стесняйтесь вносить свой вклад в этот документ в интересах сообщества Lazarus/FPC. 

Модуль memds предоставляет TMemDataset, так что вам нужно будет добавить его в раздел uses вашего проекта. 

## Сохранение MemDataset в постоянные файлы

В [интерфейсной](<../../en/Interface.md> "Interface") части вашего кода объявите тип массива для хранения информации обо всех TMemDataSets, которые вы хотите сделать постоянными в конце сеанса и восстановить в начале следующего сеанса. Вы также должны объявить переменную типа TSaveTables. 

Я также использую глобальную переменную vSuppressEvents типа boolean для подавления событий Dataset, используемых для обеспечения ссылочной целостности, во время восстановления данных. 

Вот, что у вас должно получиться: 
    
    
    type
      TSaveTables=array[1..15] of TMemDataset;    
    var
      //Глобальная переменная, которая хранит таблицы для сохранения/восстановления сеанса работы
      vSaveTables:TSaveTables;                  
      //Переменная-флаг подавления событий датасета. Используется при загрузке данных из файлов.
      vSuppressEvents:Boolean;
    

Вместо того, чтобы использовать глобальные переменные, как это сделал, например, я, вы также можете сделать их свойством главной формы. TMemDataset имеет способ хранения данных в постоянном файле: метод SaveToFile. Но вы, возможно, захотите сохранить данные в файлы [CSV](<../../en/CSV.md> "CSV") для упрощения работы с ними в дальнейшем. Поэтому я объединю оба способа в одни и те же процедуры. 

Я задаю константу cSaveRestore в интерфейсной части модуля, с помощью которой я могу определить, будут ли данные храниться и загружаться как нативные файлы MemDataset, или как файлы CSV. 
    
    
    const
      //Константа cSaveRestore определяет способ сохранения и восстановления MemDataset в постоянные файлы.
      cSaveRestore=0; //0=собственный формат MemDataset, 1=сохранение и восстановление из CSV
    

Теперь вы можете сохранить MemDataset'ы в событии OnFormClose и загрузить их в событие OnFormCreate. Заполнить элементы массива экземплярами MemDataset можно также в событии OnFormCreate. 
    
    
    procedure TMainForm.FormCreate(Sender: TObject);
    begin
      //Список таблиц, которые будут сохранены/восстановлены для сеанса работы
      vSaveTables[1]:=Products;
      vSaveTables[2]:=Boms;
      vSaveTables[3]:=Stocks;
      vSaveTables[4]:=Orders;
      vSaveTables[5]:=BomCalculationProducts;
      vSaveTables[6]:=BomCalculationComponents;
      vSaveTables[7]:=BomCalculationFooter;
      vSaveTables[8]:=BomCalculationProductsMultiple;
      vSaveTables[9]:=BomCalculationComponentsMultiple;
      vSaveTables[10]:=BomCalculationFooterMultiple;
      vSaveTables[11]:=ImportVariants;
      vSaveTables[12]:=ImportToTables;
      vSaveTables[13]:=ImportToFields;
      vSaveTables[14]:=ImportFromTables;
      vSaveTables[15]:=ImportFromFields;
      //Восстанавливаем сеанс работы
      RestoreSession;
      GetAutoincrementPrimaryFields;
    end;
    
    
    
    procedure TMainForm.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
     //Сохраняем наборы данных в файлы (чтобы сохранить текущий сеанс)
     SaveSession;
    end;
    
    
    
    procedure RestoreSession;
    var
      I:Integer;
    begin
      try
        MemoMessages.Append(TimeToStr(Now())+' Начало восстановления ранее сохраненного сеанса.');
        vSuppressEvents:=True; //Подавляем события, используемые для обеспечения ссылочной целостности
        //Отключаем элементы управления и обновляем все наборы данных
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].DisableControls;
          vSaveTables[I].Refresh; //Важный момент, если набор данных был отфильтрован
        end;
        //Загружаем memdataset'ы из файлов (для восстановления предыдущего сеанса)
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].First;
          MemoMessages.Append(TimeToStr(Now())+' Начинаем восстановление таблицы: '+vSaveTables[I].Name);
          try
            //Если данные загружаются из CSV-файла, то сначала необходимо удалить таблицу.
            if cSaveRestore=1 then begin
              MemoMessages.Append(TimeToStr(Now())+' Начинаем удаление всех записей в таблице: '+vSaveTables[I].Name);
              //Этот способ удаления всех записей невероятно медленный.
              {while not vSaveTables[I].EOF do begin
                vSaveTables[I].Delete;
              end;}
              //Этот метод для удаления всех записей намного быстрее
              EmptyMemDataSet(vSaveTables[I]);
              MemoMessages.Append(TimeToStr(Now())+' Все записи из таблицы: '+vSaveTables[I].Name+' удалены.');
            end;
          except
            on E:Exception do begin
              MemoMessages.Append(TimeToStr(Now())+' Ошибка при удалении записей из таблицы: '+vSaveTables[I].Name +'. '+E.Message);
            end;
          end;
          try
            try
              MemoMessages.Append(TimeToStr(Now())+' Восстановление таблицы: '+vSaveTables[I].Name);
              //Проверяем константу для выбора способа сохранения/восстановления данных и загрузки сохраненного сеанса
              case cSaveRestore of
                0:vSaveTables[I].LoadFromFile(vSaveTables[I].Name);
                1:LoadFromCsv(vSaveTables[I]);
              end;
            except
              on E:Exception do begin
                MemoMessages.Append(TimeToStr(Now())+' Ошибка при восстановлении таблицы: '+vSaveTables[I].Name +'. '+E.Message);
              end;
            end;
          finally
            vSaveTables[I].Active:=True;//Требуется из-за метода LoadFromFile....
          end;
          MemoMessages.Append(TimeToStr(Now())+' Таблица: '+vSaveTables[I].Name+' восстановлена.');
        end;
      finally
        vSuppressEvents:=False;
        //Обновляем все наборы данных и включаем элементы управления
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].Refresh; //Необходимо для таблиц, которые фильтруются.
          vSaveTables[I].EnableControls;
        end;
         MemoMessages.Append(TimeToStr(Now())+' Все таблицы восстановлены из сохраненных файлов.');
      end;
    end;
    
    
    
    procedure SaveSession;
    var
      I:Integer;
    begin
      try
        MemoMessages.Append(TimeToStr(Now())+' Начало сохранения сеанса в постоянные файлы.');
        vSuppressEvents:=True;
        //Отключаем элементы управления и обновляем все наборы данных
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].DisableControls;
          vSaveTables[I].Refresh; //Важный момент, если набор данных был отфильтрован
        end;
        //Сохраняем сеанс работы в файл
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].First;
          MemoMessages.Append(TimeToStr(Now())+' Сохранение таблицы: '+vSaveTables[I].Name);
          try
            //Проверяем константу для выбора способа сохранения/восстановления данных и загрузки сохраненного сеанса
            case cSaveRestore of
              0:vSaveTables[I].SaveToFile(vSaveTables[I].Name);
              1:SaveToCsv(vSaveTables[I]);
            end;
          except
            on E:Exception do begin
              MemoMessages.Append(TimeToStr(Now())+' Ошибка при сохранении таблицы: '+vSaveTables[I].Name +'. '+E.Message);
            end;
          end;
          MemoMessages.Append(TimeToStr(Now())+' Таблица: '+vSaveTables[I].Name+' сохранена.');
        end;
      finally
        vSuppressEvents:=False;
        //Обновляем все наборы данных и включаем элементы управления
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].Refresh; //Необходимо для таблиц, которые фильтруются
          vSaveTables[I].EnableControls;
        end;
         MemoMessages.Append(TimeToStr(Now())+' Все таблицы сохранены в файлы.');
      end;
    end;
    
    
    
    procedure EmptyMemDataSet(DataSet:TMemDataSet);
    var
      vTemporaryMemDataSet:TMemDataSet;
      vFieldDef:TFieldDef;
      I:Integer;
    begin
      try
        //Создаем временный MemDataSet
        vTemporaryMemDataSet:=TMemDataSet.Create(nil);
        //Сохраняем FieldDefs во временном MemDataSet
        for I:=0 to DataSet.FieldDefs.Count-1 do begin
          vFieldDef:=vTemporaryMemDataSet.FieldDefs.AddFieldDef;
          with DataSet.FieldDefs[I] do begin
            vFieldDef.Name:=Name;
            vFieldDef.DataType:=DataType;
            vFieldDef.Size:=Size;
            vFieldDef.Required:=Required;
          end;
        end;
        //Очищаем существующие fielddefs
        DataSet.Clear;
        //Восстанавливаем fielddefs
        DataSet.FieldDefs:=vTemporaryMemDataSet.FieldDefs;
        DataSet.Active:=True;
      finally
      vTemporaryMemDataSet.Clear;
      vTemporaryMemDataSet.Free;
      end;
    end;
    
    
    
    procedure LoadFromCsv(DataSet:TDataSet);
    var
      vFieldCount:Integer;
      I:Integer;
    begin
      try
        //Назначаем SdfDataSetTemporary
        with SdfDataSetTemporary do begin
          Active:=False;
          ClearFields;
          FileName:=DataSet.Name+'.txt';
          FirstLineAsSchema:=True;
          Active:=True;
          //Определяем количество полей
          vFieldCount:=FieldDefs.Count;
        end;
        //Выполняем итерацию по SdfDataSetTeditional и вставляем записи в MemDataSet.
        SdfDataSetTemporary.First;
        while not SdfDataSetTemporary.EOF do begin
          DataSet.Append;
          //Итерация по FieldDefs
          for I:=0 to vFieldCount-1 do begin
            try
              DataSet.Fields[I].Value:=SdfDataSetTemporary.Fields[I].Value;
            except
              on E:Exception do begin
                MemoMessages.Append(TimeToStr(Now())+' Ошибка при установке значения для поля: '
                 +DataSet.Name+'.'+DataSet.Fields[I].Name +'. '+E.Message);
              end;
            end;
          end;
          try
            DataSet.Post;
          except
            on E:Exception do begin
              MemoMessages.Append(TimeToStr(Now())+' Ошибка при сохранении записи в таблицу: '
               +DataSet.Name+'.'+E.Message);
            end;
          end;
          SdfDataSetTemporary.Next;
        end;
      finally
        SdfDataSetTemporary.Active:=False;
        SdfDataSetTemporary.ClearFields;
      end;
    end;
    
    
    
    procedure SaveToCsv(DataSet:TDataSet);
    var
      myFileName:string;
      myTextFile: TextFile;
      i: integer;
      s: string;
    begin
      myFileName:=DataSet.Name+'.txt';
      //создаем новый файл
      AssignFile(myTextFile, myFileName);
      Rewrite(myTextFile);
      s := ''; //инициализируем пустую строку
      try
        //записываем имена полей (как заголовки столбцов)
        for i := 0 to DataSet.Fields.Count - 1 do
          begin
            s := s + Format('%s,', [DataSet.Fields[i].FieldName]);
          end;
        Writeln(myTextFile, s);
        DataSet.First;
        //записываем значения полей
        while not DataSet.Eof do
          begin
            s := '';
            for i := 0 to DataSet.FieldCount - 1 do
              begin
                //Числовые поля без кавычек, строковые поля с кавычками
                if ((DataSet.FieldDefs[i].DataType=ftInteger)
                 or (DataSet.FieldDefs[i].DataType=ftFloat)) then
                  s := s + Format('%s,', [DataSet.Fields[i].AsString])
                else
                  s := s + Format('"%s",', [DataSet.Fields[i].AsString]);
              end;
            Writeln(myTextfile, s);
            DataSet.Next;
          end;
      finally
        CloseFile(myTextFile);
      end;
    end;
    

## Автогенератор первичных ключей

Тип поля Autoincrement не поддерживается MemDataset. Тем не менее, вы можете имитировать его, используя тип поля Integer и предоставляя калькулятор для полей автогенератора. Нам нужны глобальные переменные или открытые свойства для хранения текущего значения поля автогенератора. Я предпочитаю глобальные переменные, объявленные в интерфейсной части модуля. 
    
    
    var
      //Глобальные переменные, используемые для вычисления полей автогенератора первичного ключа MemDatasets
      vCurrentId:Integer=0;
      vProductsId:Integer=0;
      vBomsId:Integer=0;
      vBomCalculationProductsId:Integer=0;
      vBomCalculationComponentsId:Integer=0;
      vBomCalculationFooterId:Integer=0;
      vBomCalculationProductsMultipleId:Integer=0;
      vBomCalculationComponentsMultipleId:Integer=0;
      vBomCalculationFooterMultipleId:Integer=0;
      vStocksId:Integer=0;
      vOrdersId:Integer=0;
      vImportVariantsId:Integer=0;
      vImportToTablesId:Integer=0;
      vImportToFieldsId:Integer=0;
      vImportFromTablesId:Integer=0;
      vImportFromFieldsId:Integer=0;
    

Тогда у нас есть процедура для расчета значений полей автогенератора: 
    
    
    procedure GetAutoincrementPrimaryFields;
    var
      I:Integer;
      vId:^Integer;
    begin
      try
        MemoMessages.Lines.Append(TimeToStr(Now())+' Получение информации о полях автогенератора');
        vSuppressEvents:=True;
        //Отключаем элементы управления и обновляем все наборы данных
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].DisableControls;
          vSaveTables[I].Refresh; //Важный момент, если набор данных был отфильтрован
        end;
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          with vSaveTables[I] do begin
            //Используем соответствующую глобальную переменную
            case StringToCaseSelect(Name,
              ['Products','Boms','Stocks','Orders',
                'BomCalculationProducts','BomCalculationComponents','BomCalculationFooter',
                'BomCalculationProductsMultiple','BomCalculationComponentsMultiple','BomCalculationFooterMultiple',
                'ImportVariants','ImportToTables','ImportToFields','ImportFromTables','ImportFromFields']) of
              0:vId:=@vProductsId;
              1:vId:=@vBomsId;
              2:vId:=@vStocksId;
              3:vId:=@vOrdersId;
              4:vId:=@vBomCalculationProductsId;
              5:vId:=@vBomCalculationComponentsId;
              6:vId:=@vBomCalculationFooterId;
              7:vId:=@vBomCalculationProductsMultipleId;
              8:vId:=@vBomCalculationComponentsMultipleId;
              9:vId:=@vBomCalculationFooterMultipleId;
              10:vId:=@vImportVariantsId;
              11:vId:=@vImportToTablesId;
              12:vId:=@vImportToFieldsId;
              13:vId:=@vImportFromTablesId;
              14:vId:=@vImportFromFieldsId;
            end;
            try
              //Находим последнее значение ID и сохраняем его в глобальной переменной
              Last;
              vCurrentId:=FieldByName(Name+'Id').AsInteger;
              if (vCurrentId>vId^) then vId^:=vCurrentId;
            finally
              //Удаляем ссылку
              vId:=nil;
            end;
          end;
        end;
      finally
        vSuppressEvents:=False;
        //Обновляем все наборы данных и включаем элементы управления
        for I:=Low(vSaveTables) to High(vSaveTables) do begin
          vSaveTables[I].Refresh;
          vSaveTables[I].EnableControls;
        end;
         MemoMessages.Lines.Append(TimeToStr(Now())+' Автоинкрементные поля - готовы.');
      end;
    end;
    
    
    
    function StringToCaseSelect(Selector:string;CaseList:array of string):Integer;
    var 
      cnt: integer;
    begin
      Result:=-1;
      for cnt:=0 to Length(CaseList)-1 do
      begin
        if CompareText(Selector, CaseList[cnt]) = 0 then
        begin
          Result:=cnt;
          Break;
        end;
      end;
    end;
    

Процедура `GetAutoincrementPrimaryFields` вызывается каждый раз после восстановления (загрузки) данных из постоянных файлов, чтобы загрузить последние значения автогенератора в глобальные переменные (или свойства, как вы предпочитаете). Автоинкрементация выполняется в событии OnNewRecord каждого MemDataset. Например, для таблицы Orders MemDataset: 
    
    
    procedure TMainForm.OrdersNewRecord(DataSet: TDataSet);
    begin
      if vSuppressEvents then Exit;
      //Устанавливаем новое значение автогенератора
      vOrdersId:=vOrdersId+1;
      DataSet.FieldByName('OrdersId').AsInteger:=vOrdersId;
    end;
    

Как уже объяснялось, я использую глобальную переменную vSuppressEvents в качестве флага для случая восстановления данных из постоянных файлов. 

## Обеспечение ссылочной целостности

В компоненте MemDataset встроенная ссылочная целостность не реализована, поэтому вы должны сделать это самостоятельно. 

Предположим, у нас есть две таблицы: MasterTable и DetailTable. 

Существуют различные места, где необходимо использовать код ссылочной целостности: 

  * Код вставки/обновления находится в событии `BeforePost` DetailTable: перед сохранением новой/обновленной detail-записи ее необходимо проверить на соответствие требованиям ссылочной целостности
  * Код удаления находится в событии `BeforeDelete` в MasterTable: перед удалением master-записи необходимо убедиться, что все дочерние записи соответствуют требованиям ссылочной целостности


    
    
    procedure TMainForm.MasterTableBeforeDelete(DataSet: TDataSet);
    begin
      if vSuppressEvents then Exit;
      try
        DetailTable.DisableControls;
        // Принудительное удаление ссылок («каскадное удаление») для таблицы «MasterTable»
        while not DetailTable.EOF do begin
          DetailTable.Delete;
        end;
        DetailTable.Refresh;
      finally
        DetailTable.EnableControls;
      end;
    end;
    
    
    
    procedure TMainForm.DetailTableBeforePost(DataSet: TDataSet);
    begin
      if vSuppressEvents=True then Exit;
      // Принудительное использование ссылочной вставки/обновления для таблицы «DetailTable» с 
      // внешним ключом «MasterTableID», ссылающимся на 
      // поле первичного ключа идентификатора MasterTable ID
      DataSet.FieldByName('MasterTableId').AsInteger:=
        MasterTable.FieldByName('ID').AsInteger;
    end;
    

После того, как вы предоставили ссылочную вставку/обновление/удаление, все, что вам нужно сделать, это предоставить код для master/detail фильтрации данных. Это делается в событии `AfterScroll` в MasterTable и в событии `OnFilter` в DetailTable. 

Не забудьте установить для свойства `Filtered` DetailTable значение True. 
    
    
    procedure TMainForm.MasterTableAfterScroll(DataSet: TDataSet);
    begin
      if vSuppressEvents=True then Exit;
      DetailTable.Refresh;
    end;
    
    
    
    procedure TMainForm.DetailTableFilterRecord(DataSet: TDataSet;
      var Accept: Boolean);
    begin
      if vSuppressEvents=True then Exit;
      // Показывем только дочерние поля, внешний ключ которых указывает на текущую 
      // запись master таблицы
      Accept:=DataSet.FieldByName('MasterTableId').AsInteger=
        MasterTable.FieldByName('ID').AsInteger;
    end;
    

## Известные проблемы

Есть несколько ограничений при использовании MemDatasets. 

  * Метод locate не работает
  * Фильтрация с использованием свойства Filter и Filtered не работает. Вы должны использовать жесткое кодирование в событии OnFilter.
  * Повторное удаление записей кажется невероятно медленным. Поэтому я использую мою процедуру EmptyMemDataset вместо `while not EOF do Delete;`
  * В FPC 2.6.x и более ранних версиях метод CopyFromDataSet копирует данные только с текущей позиции курсора в конец набора исходных данных. Итак, вы должны написать `MemDataset1.First;` перед `MemDataSet2.CopyFromDataSet(MemDataset1);`. Исправлено в транке ревизии FPC 26233. 
    * Обратите внимание, что более старые версии FPC не имеют CopyFromDataset в Bufdataset, в то время как это - преимущество для MemDs.
    * См. багрепорт <http://bugs.freepascal.org/view.php?id=25426>.



## TBufDataSet

Как упоминалось ранее, в MemDataSet отсутствуют пользовательские фильтры, тип данных автоинкремента и метод Locate, поэтому взамен лучше использовать TBufDataSet. TBufDataset предоставляется модулем BufDataset. 

Поскольку нет компонента для редактирования TBufDataSet во время разработки (но вы можете настроить определения полей во время разработки), вы можете создать пользовательский компонент-обертку или использовать его через код так же, как ClientDataSet в Delphi. Подробности смотрите в документации Delphi, касающейся наборов данных клиента для подробностей. 

Вы можете использовать те же методы для обеспечения ссылочной целостности и первичных полей автоинкремента, как описано для MemDataSet. 

Между MemDataSet и BufDataset есть только небольшие различия: 

MemDataSet  | BufDataset   
---|---  
DataSet.ClearFields | DataSet.Fields.Clear   
DataSet.CreateTable | DataSet.CreateDataSet   
  
## Сортировка DBGrid по событию OnTitleClick для TBufDataSet

Если вы хотите включить последовательную сортировку DBGrid по возрастанию и убыванию, показывающую некоторые данные из TBufDataSet, вы можете использовать следующий метод: 
    
    
    Uses
      BufDataset, typinfo;
    
    function SortBufDataSet(DataSet: TBufDataSet;const FieldName: String): Boolean;
    var
      i: Integer;
      IndexDefs: TIndexDefs;
      IndexName: String;
      IndexOptions: TIndexOptions;
      Field: TField;
    begin
      Result := False;
      Field := DataSet.Fields.FindField(FieldName);
      //Если неверное имя поля, выйдем.
      if Field = nil then Exit;
      //Если неверный тип поля, выйдем.
      if {(Field is TObjectField) or} (Field is TBlobField) or
        {(Field is TAggregateField) or} (Field is TVariantField)
         or (Field is TBinaryField) then Exit;
      //Получаем IndexDefs и IndexName, используя RTTI
      if IsPublishedProp(DataSet, 'IndexDefs') then
        IndexDefs := GetObjectProp(DataSet, 'IndexDefs') as TIndexDefs
      else
        Exit;
      if IsPublishedProp(DataSet, 'IndexName') then
        IndexName := GetStrProp(DataSet, 'IndexName')
      else
        Exit;
      //Убедитесь, что IndexDefs об-нов-лен
      IndexDefs.Updated:=false; {<<<<---Эта строка имеет решающее значение, так как IndexDefs.Update ничего не будет делать при следующей сортировке, если она уже верна}
      IndexDefs.Update;
      //Если восходящий индекс уже используется, 
      //переключаемся на нисходящий индекс
      if IndexName = FieldName + '__IdxA'
      then
        begin
          IndexName := FieldName + '__IdxD';
          IndexOptions := [ixDescending];
        end
      else
        begin
          IndexName := FieldName + '__IdxA';
          IndexOptions := [];
        end;
      //ищем существующий индекс
      for i := 0 to Pred(IndexDefs.Count) do
      begin
        if IndexDefs[i].Name = IndexName then
          begin
            Result := True;
            Break
          end;  //if
      end; // for
      //Если существующий индекс не найден, создаем его
      if not Result then
          begin
            if IndexName=FieldName + '__IdxD' then
              DataSet.AddIndex(IndexName, FieldName, IndexOptions, FieldName)
            else
              DataSet.AddIndex(IndexName, FieldName, IndexOptions);
            Result := True;
          end; // if not
      //Устанавливаем индекс
      SetStrProp(DataSet, 'IndexName', IndexName);
    end;
    

Итак, вы можете вызвать эту функцию из DBGrid следующим образом: 
    
    
    procedure TFormMain.DBGridProductsTitleClick(Column: TColumn);
    begin
      SortBufDataSet(Products, Column.FieldName);
    end;
    

## Сортировка нескольких столбцов в grid

Я написал TDBGridHelper для сортировки grid по нескольким столбцам, удерживая клавишу Shift. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** MaxIndexesCount должен быть достаточно большим для TBufDataSet, потому что могут быть довольно большие комбинации возможных вариантов сортировки.

Но я думаю, что люди не будут использовать больше 10, поэтому установка 100 должна быть теоретически приемлемой. 
    
    
      { TDBGridHelper }
    
      TDBGridHelper = class helper for TDBGrid
      public const
        cMaxColCOunt = 3;
      private
        procedure Interbal_MakeNames(Fields: TStrings; out FieldsList, DescFields: String);
        procedure Internal_SetColumnsIcons(Fields: TStrings; AscIdx, DescIdx: Integer);
        function Internal_IndexNameExists(IndexDefs: TIndexDefs; IndexName: String): Boolean;
      public
        procedure Sort(const FieldName: String; AscIdx: Integer = -1; DescIdx: Integer = -1);
        procedure ClearSort;
      end;  
    
    { TDBGridHelper }
    
    procedure TDBGridHelper.Interbal_MakeNames(Fields: TStrings; out FieldsList, DescFields: String);
    var
      FldList: TStringList;
      DscList: TStringList;
      FldDesc, FldName: String;
      i: Integer;
    begin
      if Fields.Count = 0 then
      begin
        FieldsList := '';
        DescFields := '';
        Exit;
      end;
    
      FldList := TStringList.Create;
      DscList := TStringList.Create;
      try
        FldList.Delimiter := ';';
        DscList.Delimiter := ';';
    
        for i := 0 to Fields.Count - 1 do
        begin
          Fields.GetNameValue(i, FldName, FldDesc);
          FldList.Add(FldName);
    
          if FldDesc = 'D' then
            DscList.Add(FldName);
        end;
    
        FieldsList := FldList.DelimitedText;
        DescFields := DscList.DelimitedText;
      finally
        FldList.Free;
        DscList.Free;
      end;
    end;
    
    procedure TDBGridHelper.Internal_SetColumnsIcons(Fields: TStrings; AscIdx, DescIdx: Integer);
    var
      i: Integer;
      FldDesc: String;
    begin
      for i := 0 to Self.Columns.Count - 1 do
      begin
        FldDesc := Fields.Values[Self.Columns[i].Field.FieldName];
    
        if FldDesc = 'A' then
          Self.Columns[i].Title.ImageIndex := AscIdx
        else
        if FldDesc = 'D' then
          Self.Columns[i].Title.ImageIndex := DescIdx
        else
          Self.Columns[i].Title.ImageIndex := -1
      end;
    end;
    
    function TDBGridHelper.Internal_IndexNameExists(IndexDefs: TIndexDefs; IndexName: String): Boolean;
    var
      i: Integer;
    begin
      for i := 0 to IndexDefs.Count - 1 do
      begin
        if IndexDefs[i].Name = IndexName then
          Exit(True)
      end;
    
      Result := False
    end;
    
    procedure TDBGridHelper.Sort(const FieldName: String; AscIdx: Integer;
      DescIdx: Integer);
    var
      Field: TField;
      DataSet: TBufDataset;
      IndexDefs: TIndexDefs;
      IndexName, Dir, DescFields, FieldsList: String;
      Fields: TStringList;
    begin
      if not Assigned(DataSource.DataSet) or
         not DataSource.DataSet.Active or
         not (DataSource.DataSet is TBufDataset) then
        Exit;
      DataSet := DataSource.DataSet as TBufDataset;
    
      Field := DataSet.FieldByName(FieldName);
      if (Field is TBlobField) or (Field is TVariantField) or (Field is TBinaryField) then
        Exit;
    
      IndexDefs := DataSet.IndexDefs;
      IndexName := DataSet.IndexName;
    
      if not IndexDefs.Updated then
        IndexDefs.Update;
    
      Fields := TStringList.Create;
      try
        Fields.DelimitedText := IndexName;
        Dir := Fields.Values[FieldName];
    
        if Dir = 'A' then
          Dir := 'D'
        else
        if Dir = 'D' then
          Dir := 'A'
        else
          Dir := 'A';
    
        //Если нажата клавиша Shift, добавляем поле в список полей.
        if ssShift in GetKeyShiftState then
        begin
          Fields.Values[FieldName] := Dir;
          //Мы не добавляем в сортировку больше полей, если общее количество полей превышает cMaxColCOunt
          if Fields.Count > cMaxColCOunt then
            Exit;
        end
        else
        begin
          Fields.Clear;
          Fields.Values[FieldName] := Dir;
        end;
    
        IndexName := Fields.DelimitedText;
        if not Internal_IndexNameExists(IndexDefs, IndexName) then
        begin
          Interbal_MakeNames(Fields, FieldsList, DescFields);
          TBufDataset(DataSet).AddIndex(IndexName, FieldsList, [], DescFields, '');
        end;
    
        DataSet.IndexName := IndexName;
        Internal_SetColumnsIcons(Fields, AscIdx, DescIdx)
      finally
        Fields.Free;
      end;
    end;
    
    procedure TDBGridHelper.ClearSort;
    var
      DataSet: TBufDataset;
      Fields: TStringList;
    begin
      if not Assigned(DataSource.DataSet) or
         not DataSource.DataSet.Active or
         not (DataSource.DataSet is TBufDataset) then
        Exit;
      DataSet := DataSource.DataSet as TBufDataset;
    
      DataSet.IndexName := '';
    
      Fields := TStringList.Create;
      try
        Internal_SetColumnsIcons(Fields, -1, -1)
      finally
        Fields.Free
      end
    end;
    

Чтобы использовать сортировку, нужно вызвать вспомогательные методы в OnCellClick и onTitleClick. OnTitleClick - если вы удерживаете клавишу shift, добавляется новый столбец в список сортировки, или меняется направление сортировки выбранного столбца, или просто сортируется один столбец. OnCellClick - если дважды щелкнуть ячейку [0, 0], сетка очищает ее сортировку. 
    
    
    procedure TForm1.grdCountriesCellClick(Column: TColumn);
    begin
      if not Assigned(Column) then
        grdCountries.ClearSort
    end;
    
    procedure TForm1.grdCountriesTitleClick(Column: TColumn);
    begin
      grdCountries.Sort(Column.Field.FieldName, 0, 1);
    end;
    

Если вы назначили TitleImageList, вы можете указать, какое изображение использовать для восходящей, а какое для нисходящей сортировки. 

## ZMSQL

Другой, часто лучший способ написания баз данных в памяти - это использование пакета ZMSQL: 

  * [ZMSQL](<../../en/ZMSQL.md> "ZMSQL")
  * <http://sourceforge.net/projects/lazarus-ccr/files/zmsql/>
  * <http://www.lazarus.freepascal.org/index.php/topic,13821.30.html>



## Авторство

Оригинальный текст написан: Zlatko Matić (matalab@gmail.com) 

Вклад других авторов, как показано на странице History.

---

_Source: [https://wiki.freepascal.org/How_to_write_in-memory_database_applications_in_Lazarus/FPC/ru](https://web.archive.org/web/20250117043538/https://wiki.freepascal.org/How_to_write_in-memory_database_applications_in_Lazarus/FPC/ru)_
