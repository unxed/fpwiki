# Streaming JSON

│ **[Deutsch (de)](</Streaming_JSON/de> "Streaming JSON/de")** │  **[English (en)](<../en/Streaming_JSON.md> "Streaming JSON")** │  **[polski (pl)](</Streaming_JSON/pl> "Streaming JSON/pl")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Streaming_JSON/zh_CN> "Streaming JSON/zh CN")** │    
****

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Эта статья является перевод статьи с английского [Streaming_JSON](<../en/Streaming_JSON.md> "Streaming JSON") от 2014-10-02, которая была переведена с помощью Google Translate с немецкого (оригинал). Понятно, что результат перевода может быть ужасным, вполне может требоваться ручная корректировка. **Убедительная просьба не использовать автоматический перевод без окончательной обработки**

[JSON](<../en/JSON.md> "JSON")(JavaScript Object Notation) является текстовым, стандартизированным форматом данных. Как следует из названия, JSON документы являются правильным кодом JavaScript, где они могут быть непосредственно преобразованы в объекты JavaScript. Но сам по себе JSON может быть использован независимо от используемого языка программирования для обмена данными. 

В этом руководстве объясняется, как загрузить данные в формате JSON в программе Free Pascal для обработки. Оно также объясняет, как преобразовать данные FPC в формате JSON (например, чтобы отправить их на веб-сервер). 

## Contents

  * 1 Общие положения и условия
  * 2 Структура данных
  * 3 Загрузка JSON
  * 4 Сохранение JSON
  * 5 Вывод
  * 6 Смотрите также
  * 7 Примечания & Ссылки



## Общие положения и условия

Загрузка и хранение (маршалинг) FPC данных объекта осуществляется с помощью юнита "fpjsonrtti". Кроме того, может пригодиться класс Class в этом юните (см ниже). 

В связи с этим раздел USES должен содержать, по крайней мере, эти два юнита: 
    
    
    USES Classes, fpjsonrtti;
    

В настоящее время (май 2014 г.) есть некоторые отличия от потоковой системы Free Pascal с ??_предположительно_ JavaScript ??: 

  * Данные в формате JSON чувствительны к регистру. Таким образом, свойства объектов Free Pascal, как свойства JSON должны быть написаны с правильным регистром.
  * Нет свойств, которые могут быть определены как DefineProperties. Нельзя сохранить ссылки на методы (обработчики событий).2
  * [TCollection](<../en/TCollection.md> "TCollection") и [TStrings](<../en/TStrings.md> "TStrings") может быть использован в качестве замены массивов.



Демо-версия программы предоставляется с исходным кодом Free Pascal Compiler (в папке /packages/fcl-json/examples). 

В качестве примера, мы приведём пример со следующими данными JSON: 
    
    
    {
      "id"     : 123,                                                // Integer
      "obj"    : { "name": "Привет, мир!" },                          // Object
      "coll"   : [ { "name": "Объект 1" }, { "name": "Объект 2" } ], // два объекта  TCollection 
      "strings": [ "привет 1", "привет 2" ]                            // список строк
    }
    

Это может быть выполнено в Free Pascal с использованием констант следующим образом: 
    
    
    const JSON_TESTDATA =
    '{'+LineEnding+
    '  "id": 123,'+LineEnding+
    '  "obj": { "name": "Привет, Мир!" },'+LineEnding+
    '  "coll": [ { "name": "Объект 1" }, { "name": "Объект 2" } ],'+LineEnding+
    '  "strings": [ "привет 1", "привет 2" ]'+LineEnding+
    '}';
    

## Структура данных

В качестве базового класса для обслуживания данных существует класс [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)") из юнита Classes, так как для них и всех подклассов должен быть создан [Laufzeit-Typinformationen](<../en/Runtime_Type_Information_\(RTTI\).md> "Runtime Type Information \(RTTI\)") (RTTI). Это совершенно необходимо для маршаллинга. Поскольку fpjsonrtti не интегрирован в систему маршаллинга, любого другого класса, должна быть указана опция компилятора **{$M+}** , для использования. 

Все объекты должны быть доступны как [property](</Property> "Property") для чтения в разделе класса [published](<../en/Published.md> "Published"). В общем случае (переменная) должна быть доступна для чтения и записи непосредственно в поле данных. Если вы хотите, конечно, могут быть использованы методы getter и setter. 

Для структуры JSON результаты определении класса представлены ниже. 
    
    
    TYPE
      TNameObject = CLASS(TCollectionItem) // класс для свойства 'obj' и TCollection 
      PRIVATE
        fName: String;
      PUBLISHED
        property name: String read fName write fName;
      END;  
    
      TBaseObject = CLASS(TPersistent)  // класс для структуры JSON
      PRIVATE
        fid: Integer;
        fObj: TNameObject;
        fColl: TCollection;
        fStrings: TStrings;
      PUBLIC
        CONSTRUCTOR Create;
        DESTRUCTOR Destroy; override;
      PUBLISHED// все свойства должны быть published
        PROPERTY id: Integer read fid write fid;
        PROPERTY obj: TNameObject read fObj write fObj;
        PROPERTY coll: TCollection read fColl;
        PROPERTY strings: TStrings read fStrings;
      END;
    

Класс `TNameObject` является потомком [TCollectionItem](</index.php?title=TCollectionItem&action=edit&redlink=1> "TCollectionItem \(page does not exist\)"). Таким образом, они могут оба для свойства _obj_ использовать коллекции. Если это не желательно, здесь должны быть определены два различных класса. 

В конструкторе класса TBaseObject класс TCollection и список строк должен быть создан конструктором и уничтожен в деструкторе. 
    
    
    CONSTRUCTOR TBaseObject.Create;
    BEGIN
      // конструктор коллекции и StringList
      fColl    := TCollection.Create(TNameObject);
      fStrings := TStringList.Create;
      fObj     := TNameObject.Create(nil);
    END;
    
    DESTRUCTOR TBaseObject.Destroy;
    BEGIN
      // деструктор коллекции и StringList
      fColl.Free;
      fStrings.Free;
      fObj.Free;
      INHERITED Destroy;
    END;
    

Если вы не хотите больше функций для данных классов, их определение закончено. 

## Загрузка JSON

С помощью метода `Procedure JSONToObject(Const JSON : TJSONStringType; AObject : TObject);` в классе [TJSONDeStreamer](</index.php?title=TJSONDeStreamer&action=edit&redlink=1> "TJSONDeStreamer \(page does not exist\)") вы можете присвоить данные в формате JSON непосредственно _существующему_ объекту; перед тем, как вызвать этот метод, вы должны создать TJSONDeStreamer и целевой объект. 

Следующий метод загружает данные из структуры JSON `JSON_TESTDATA` в объект "о" и затем выводит текущие значения свойств в консоли. 
    
    
    PROCEDURE DeStreamTest;
    VAR
      DeStreamer: TJSONDeStreamer;
      o: TBaseObject;
      no: TNameObject;
      s: String;
    BEGIN
      WriteLn('тест DeStream');
      WriteLn('======================================');
    
      // объект DeStreamer и целевой объект 
      DeStreamer := TJSONDeStreamer.Create(nil);
      o := TBaseObject.Create;
      TRY
        // загрузка данных JSON в объект o
        DeStreamer.JSONToObject(JSON_TESTDATA, o);
        // ID
        WriteLn(o.id);
        // имя объекта
        WriteLn(o.obj.name); 
        // напечатать имена всех объектов
        for TCollectionItem(no) in o.coll do
          Writeln(no.name);
        // вывести все строки
        for s in o.strings do
          WriteLn(s);
    
      // очистка
      FINALLY
        o.Destroy;
        DeStreamer.Destroy;
      END;
    END;
    

## Сохранение JSON

Для преобразования объекта в текст JSON, используется класс [TJSONStreamer](</index.php?title=TJSONStreamer&action=edit&redlink=1> "TJSONStreamer \(page does not exist\)"). Здесь используется метод `FUNCTION ObjectToJSONString(AObject : TObject) : TJSONStringType;`. 

В следующей процедуре, объект создается, заполняется данными теста, а затем преобразуются в формат JSON. Порядок, в котором сохраняются свойства не могут зависеть от программиста. 

И, наконец, текст JSON выводится на консоль. 
    
    
    PROCEDURE StreamTest;
    VAR
      Streamer: TJSONStreamer;
      o: TBaseObject;
      JSONString: String;
    BEGIN
      WriteLn('Stream test');
      WriteLn('======================================');
    
      Streamer := TJSONStreamer.Create(nil);
      o := TBaseObject.Create;
      TRY
        // установка данных
        o.id := 123;
        o.obj.name := 'Привет, мир!';
        TNameObject(o.coll.Add).name := 'Объект 1';
        TNameObject(o.coll.Add).name := 'Объект 2';
        o.strings.Add('Привет 1');
        o.strings.Add('Привет 2');
    
        Streamer.Options := Streamer.Options + [jsoTStringsAsArray]; // сохранение строки в JSON
        // конвертация вывод и JSON
        JSONString := Streamer.ObjectToJSONString(o);
        WriteLn(JSONString);
    
      // очистка
      FINALLY
        o.Destroy;
        Streamer.Destroy;
      END;
    END;
    

## Вывод

С помощью представленного знания, простые и сложные структуры данных JSON могут быть загружены в ваши программы на Free Pascal. Если необходима какая-либо пред- или постобработка данных JSON, текстовые данные могут быть изначально загружены в структуру данных в формате JSON (классом [TJSONParser](</index.php?title=TJSONParser&action=edit&redlink=1> "TJSONParser \(page does not exist\)") из юнита [jsonparser](</index.php?title=jsonparser&action=edit&redlink=1> "jsonparser \(page does not exist\)")), а затем можно обрабатывать с помощью юнита [fpJSON](</index.php?title=fpJSON&action=edit&redlink=1> "fpJSON \(page does not exist\)"). 

Опции свойства TJSONStreamer могут быть использованы для управления представлением ваших собственных структур данных в формате JSON. 

## Смотрите также

  * [JSON](<JSON.md> "JSON/ru")
  * [fcl-json](<fcl-json.md> "fcl-json/ru")
  * [Streaming components](<../en/Streaming_components.md> "Streaming components")



## Примечания & Ссылки

  1. <http://lists.freepascal.org/fpc-pascal/2013-January/036254.html>

  2. <http://lists.lazarus.freepascal.org/pipermail/lazarus/2011-January/058878.html>



  *[RTTI]: Runtime Type Information

---

_Source: [https://wiki.freepascal.org/Streaming_JSON/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Streaming_JSON/ru)_
