# fcl-json

│ **[English (en)](<../en/fcl-json.md> "fcl-json")** │  **[polski (pl)](</fcl-json/pl> "fcl-json/pl")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</fcl-json/zh_CN> "fcl-json/zh CN")** │    
****

## Contents

  * 1 Информация
  * 2 Маршаллинг
  * 3 Примеры
    * 3.1 Итак, начнём
    * 3.2 Перемещение пунктов
    * 3.3 Сохранение/загрузка позиции/размера диалога
    * 3.4 Сохранение/загрузка TStringList
    * 3.5 Использование JsonViewer
    * 3.6 Изменение формата чисел с плавающей запятой
    * 3.7 Смотрите также



# Информация

fcl-json реализация стандарта [JSON](<../en/JSON.md> "JSON"). 

Пакет содержит юниты: 

  * fpjson: базовый юнит, который реализует TJsonData и их потомков, например TJsonObject
  * JsonParser: реализует TJsonParser, используется в примере ниже
  * jsonConf: реализует TJsonConfig, что удобно для чтения/записи данных файлов приложения
  * jsonScanner: лексический анализатор исходников json



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В fpjson, доступ, например, `SomeJSONObject.Integers['цена']` может дать Нарушение SIGSEGV/Access, если это целая переменная не существует. Это, очевидно, сделано намеренно, см [[[1]](<http://bugs.freepascal.org/view.php?id=22273>)].

Вы должны были бы использовать метод Find (доступен в FPC 2.6.2), чтобы сначала проверить, существует ли элемент ('цена' в данном примере). 

# Маршаллинг

fcl-json содержит юнит "fpjsonrtti", который используется для загрузки из объектов или сохранения их в формате JSON(экземпляров TObject). 

Смотрите [Streaming JSON](<Streaming_JSON.md> "Streaming JSON/ru") для короткого примера. 

# Примеры

## Итак, начнём
    
    
    USES fpjson, jsonparser;
    
    PROCEDURE JSONTest;
    VAR
       jData : TJSONData;
       jObject : TJSONObject;
       jArray : TJSONArray;
       s : string;
    BEGIN   
       // это лишь минимальный пример того, что можно сделать с помощью этого API
    
       // создать строки JSON
       jData := GetJSON('{"поле1" : "Привет", "поле2" : 42, "Цвет" : ["Красный", "Зелёный", "Голубой"]}');
    
       // вывести как плоскую строку
       s := jData.AsJSON;
    
       //  вывести замечательно-отформатированный JSON
       s := jData.FormatJSON;
    
       // передан как TJSONObject для простого доступа
       jObject := TJSONObject(jData);
    
       // передача значения ключа "поле1"
       s := jObject.Get('поле1');
    
       // установка значения ключа "поле2"
       jObject.Integers['поле2'] := 123;
    
       // передача второго цвета
       s := jData.FindPath('Цвет[1]').AsString;
    
       // добавить новый элемент
       jObject.Add('Happy', True);
    
       // передать новый подмассив
       jArray := TJSONArray.Create;
       jArray.Add('Север');
       jArray.Add('Юг');
       jArray.Add('Восток');
       jArray.Add('Запад');
       jObject.Add('Направление', jArray);
    
    END;
    

## Перемещение пунктов
    
    
    uses
      Classes, TypInfo, fpjson, jsonparser;
    
    procedure JSONItems(Info: TStrings);
    var
      jData : TJSONData;
      jItem : TJSONData;
      i, j: Integer;
      object_name, field_name, field_value, object_type, object_items: String;
    begin
      jData := GetJSON('{"A":{"field1":0, "field2": false},"B":{"field1":0, "field2": false}}');
    
      for i := 0 to jData.Count - 1 do
      begin
        jItem := jData.Items[i];
     
        object_type := GetEnumName(TypeInfo(TJSONtype), Ord(jItem.JSONType));
        object_name := TJSONObject(jData).Names[i];
        WriteStr(object_items, jItem.Count);
     
        Info.Append('object type: ' + object_type + '|object name: ' + object_name + '|number of fields: ' + object_items);
     
        for j := 0 to jItem.Count - 1 do
        begin
          field_name := TJSONObject(jItem).Names[j];
          field_value := jItem.FindPath(TJSONObject(jItem).Names[j]).AsString;
     
          Info.Append(field_name + '|' + field_value);
        end;
      end;
     
      jData.Free;
    end;
    

## Сохранение/загрузка позиции/размера диалога
    
    
    USES jsonConf;
    
    PROCEDURE TfmMain.SaveOptionsPos;
    VAR
      c: TJSONConfig;
    BEGIN
      c:= TJSONConfig.Create(nil);
      TRY
        c.Filename:= GetAppPath(cFileHistory);
        c.SetValue('/dialog/max', WindowState=wsMaximized);
        if WindowState<>wsMaximized then
        BEGIN
          c.SetValue('/dialog/posx', Left);
          c.SetValue('/dialog/posy', Top);
          c.SetValue('/dialog/sizex', Width);
          c.SetValue('/dialog/sizey', Height);
        END;
      FINALLY
        c.Free;
      END;
    END;
    
    procedure TfmMain.LoadOptionsPos;
    var
      nLeft, nTop, nW, nH: integer;
      c: TJSONConfig;
    begin
      c:= TJSONConfig.Create(nil);
      try
        c.Filename:= GetAppPath(cFileHistory);
    
        nLeft:= c.GetValue('/dialog/posx', Left);
        nTop:= c.GetValue('/dialog/posy', Top);
        nW:= c.GetValue('/dialog/sizex', Width);
        nH:= c.GetValue('/dialog/sizey', Height);
        SetBounds(nLeft, nTop, nW, nH);
    
        if c.GetValue('/dialog/max', false) then
          WindowState:= wsMaximized;
      finally
        c.Free;
      end;
    end;
    

## Сохранение/загрузка TStringList
    
    
    //Пример пути: '/list_find'
    
    PROCEDURE SLoadStringsFromFile(cfg: TJsonConfig; const path: string; List: TStrings);
    VAR
      i: integer;
      s: UnicodeString;
    BEGIN
      List.Clear;
      FOR i:= 0 TO OptMaxHistoryItems-1 DO
      BEGIN
        s:= cfg.GetValue(path+'/'+inttostr(i), '');
        IF s='' THEN break;
        List.Add(Utf8Encode(s));
      END;
    END;
    
    PROCEDURE SSaveStringsToFile(cfg: TJsonConfig; const path: string; List: TStrings);
    VAR
      i: integer;
      s: string;
    BEGIN
      FOR i:= 0 TO OptMaxHistoryItems-1 DO
      BEGIN
        IF i<List.Count THEN
          s:= List[i]
        ELSE
          s:= '';
        cfg.SetDeleteValue(path+'/'+inttostr(i), Utf8Decode(s), '');
      END;
    END;
    

## Использование JsonViewer

Пример использования можно найти в инструментах Lazarus: jsonviewer (расположенный в lazarus/tools/jsonviewer). В частности, эта часть инструмента показывает, как использовать JSON: 
    
    
    PROCEDURE TMainForm.OpenFile(Const AFileName : String);
    VAR
      S : TFileStream;
      P : TJSONParser;
      D : TJSONData;
    BEGIN
      S:=TFileStream.Create(AFileName,fmOpenRead);
      TRY
        P:=TJSONParser.Create(S);
        TRY
          P.Strict:=FStrict;
          D:=P.Parse;
        FINALLY
          P.Free;
        END;
      FINALLY
        S.Free;
      END;
      FFileName:=AFileName;
      SetCaption;
      FreeAndNil(FRoot);
      FRoot:=D;
      ShowJSONDocument;
    END;
    
    PROCEDURE TMainForm.ShowJSONDocument;
    BEGIN
      WITH TVJSON.Items DO
        BEGIN
          BeginUpdate;
          TRY
            TVJSON.Items.Clear;
            SHowJSONData(Nil,FRoot);
            WITH TVJSON DO
              IF (Items.Count>0) AND Assigned(Items[0]) THEN
                BEGIN
                  Items[0].Expand(False);
                  Selected:=Items[0];
                END;
          FINALLY
            EndUpdate;
          END;
        END;
    END;
    
    PROCEDURE TMainForm.ShowJSONData(AParent : TTreeNode; Data : TJSONData);
    VAR
      N,N2 : TTreeNode;
      I : Integer;
      D : TJSONData;
      C : String;
      S : TStringList;
    BEGIN
      N:=Nil;
      IF Assigned(Data) THEN
        BEGIN
        CASE Data.JSONType OF
          jtArray,
          jtObject:
            BEGIN
            IF (Data.JSONType=jtArray) THEN
              C:=SArray
             ELSE
               C:=SObject;
            N:=TVJSON.Items.AddChild(AParent,Format(C,[Data.Count]));
            S:=TstringList.Create;
            TRY
              FOR I:=0 TO Data.Count-1 DO
                IF Data.JSONtype=jtArray THEN
                  S.AddObject(IntToStr(I),Data.items[i])
                ELSE
                  S.AddObject(TJSONObject(Data).Names[i],Data.items[i]);
              IF FSortObjectMembers and (Data.JSONType=jtObject) THEN
                S.Sort;
              FOR I:=0 TO S.Count-1 DO
                BEGIN
                  N2:=TVJSON.Items.AddChild(N,S[i]);
                  D:=TJSONData(S.Objects[i]);
                  N2.ImageIndex:=ImageTypeMap[D.JSONType];
                  N2.SelectedIndex:=ImageTypeMap[D.JSONType];
                  ShowJSONData(N2,D);
                END
            FINALLY
              S.Free;
            END;
            END;
          jtNull:
            N:=TVJSON.Items.AddChild(AParent,SNull);
        ELSE
          N:=TVJSON.Items.AddChild(AParent,Data.AsString);
        END;
        IF Assigned(N) THEN
          BEGIN
            N.ImageIndex:=ImageTypeMap[Data.JSONType];
            N.SelectedIndex:=ImageTypeMap[Data.JSONType];
            N.Data:=Data;
          END;
        END;
    END;
    

Библиотека [Components and Code examples](<../en/Components_and_Code_examples.md> "Components and Code examples") использует JSON для передачи/приема данных. 

## Изменение формата чисел с плавающей запятой

Вопрос: моя программа генерирует и записывает данные в файл JSON. Я использую метод `FormatJSON()`, чтобы сделать вывод более читабельным. Меня не совсем устраивает, как выглядят числа с плавающей точкой: 
    
    
    "coordinates" : [
         5.5978631048365003E+001,
         2.2100000000000000E+002
    ]
    

Я хочу видеть нормальную форму: 
    
    
    "coordinates" : [
         55.978631048365003,
         221.0
    ]
    

Ответ: (пользователи форума **y.ivanov** , **rvk** , **wp**): 
    
    
    {$mode objfpc}{$h+}
    uses
      fpjson,
      jsonparser,
      SysUtils;
     
    type
      TJSONFloat4Number = class(TJSONFloatNumber)
      protected
        function GetAsString: TJSONStringType; override;
      end;
     
    function TJSONFloat4Number.GetAsString: TJSONStringType;
    var
      F: TJSONFloat;
      fs: TFormatSettings;
    begin
      fs := DefaultFormatSettings;
      fs.DecimalSeparator := '.';
      F := GetAsFloat;
      Result := FormatFloat('0.0###############', F, fs); // форматирование с вашими предпочтениями
    end;
    
    procedure JSONTest;
    var
      jData: TJSONData;
    begin
      jData := GetJSON('{"coordinates": [5.5978631048365003E+001, 2.2100000000000000E+002]}');
      writeln(jData.FormatJSON);
      jData.Free;
    end;
     
    begin
      SetJSONInstanceType(jitNumberFloat, TJSONFloat4Number);
      JSONTest;
      Readln;
    end.
    

## Смотрите также

  * Статья охватывает использование XML и JSON в Free Pascal: [PDF](<http://www.freepascal.org/~michael/articles/webdata/webdata.pdf>)
  * [Package List](<../en/Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-json/ru](https://web.archive.org/web/20250212235555/https://wiki.freepascal.org/fcl-json/ru)_
