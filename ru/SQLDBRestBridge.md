# SQLDBRestBridge

│ **[English (en)](<../en/SQLDBRestBridge.md>)** │  **русский (ru)** │

## Contents

  * 1 Цель
    * 1.1 Что компонент делает?
    * 1.2 Чего компонент не делает?
  * 2 Характеристики
    * 2.1 На стороне базы данных SQL
    * 2.2 На стороне HTTP
  * 3 Доступные компоненты
    * 3.1 TSQLDBRestSchema
      * 3.1.1 Свойства
      * 3.1.2 Методы
    * 3.2 TSQLDBRestDispatcher
      * 3.2.1 Свойства
      * 3.2.2 Методы
    * 3.3 TRestBasicAuthenticator
    * 3.4 TSQLDBRestModule
    * 3.5 TSQLDBRestResource
      * 3.5.1 properties
      * 3.5.2 Events
      * 3.5.3 Methods
    * 3.6 TSQLDBRestField
    * 3.7 TSQLDBRestBusinessProcessor
  * 4 Special Resources
    * 4.1 metaData
    * 4.2 metaData/resourceName
    * 4.3 customview
    * 4.4 _connection
  * 5 Available Output formats
    * 5.1 JSON (the default)
    * 5.2 XML (a custom format)
    * 5.3 CSV
    * 5.4 CDS
    * 5.5 ADO
  * 6 Ini file Support
  * 7 Lazarus Support
  * 8 Pas2JS Support
  * 9 Usage
    * 9.1 In a fcl-web application.
      * 9.1.1 Expose a single database, no authentication.
      * 9.1.2 Expose single database, HTTP Basic authentication.
      * 9.1.3 Expose single database, HTTP Basic authentication (2)
      * 9.1.4 Using a Config file and schema files.
    * 9.2 In the lazarus IDE.
      * 9.2.1 Using a datamodule
      * 9.2.2 Using a SQLDBRestdatamodule
  * 10 Examples
    * 10.1 FPC
    * 10.2 Lazarus
      * 10.2.1 Server examples
      * 10.2.2 TBufDataset client example
      * 10.2.3 TJSONDataset client example
      * 10.2.4 TCSVDataset client example
    * 10.3 Pas2JS
  * 11 TODO



## Цель

Все больше и больше приложений перемещаются в Интернет: сегодня Pas2JS позволяет программировать паскаль для Интернета. 

Данные должны храниться на сервере, и REST сегодня является предпочтительной архитектурой для этого. Чаще всего данные попадают в базу данных. 

Поэтому необходим простой способ сделать данные доступными через службу REST. 

Модуль `SQLDBRestBridge` предлагает средство для раскрытия любой базы данных, к которой SQLDB может получить доступ, используя REST API. 

Это не подразумевается как общая структура REST API, для этого существуют другие платформы. Это также не структура RPC. 

Конструкция такова, что полная база данных может быть представлена с использованием одной строки кода с разумными настройками по умолчанию. 

  


### Что компонент делает?

По сути, этот компонент можно использовать в приложении FCL-Web для превращения его в REST-сервер: 

Затем сервер примет обычные команды REST GET/POST/PUT/Delete и автоматически переведет их в соответствующую базу данных CRUD (создание/чтение/обновление/удаление). 

Его можно использовать просто: одной строки кода достаточно, чтобы таким образом представить полную базу данных. 

Или это может быть полностью настроено и точно подкручено: схема может быть полностью определена: все операторы SQL могут быть заданы, могут быть определены списки полей, могут быть заданы псевдонимы, могут быть реализованы бизнес-правила. 

Полученный в результате сервер можно использовать в установке клиента/сервера (например, используя pas2js в качестве клиента) для обслуживания данных без необходимости написания всего необходимого сантехнического кода на REST-сервере. 

### Чего компонент не делает?

  * Это технология на стороне сервера.



    Чтобы использовать данные, обслуживаемые этим компонентом, вам нужно написать клиент.
    Нет никаких предположений относительно клиента, за исключением того, что он знает, как выполнять HTTP-запросы, и что он понимает один из доступных форматов данных.

  * Он не предоставляет объекты в качестве ресурсов REST.



    Используя некоторые приемы, это можно сделать, но это не цель проекта.
    В конечном итоге все данные попадают в базу данных, и этот компонент использует прямой механизм для достижения этой цели.

## Характеристики

### На стороне базы данных SQL

  1. Каждый ресурс REST определяется до 4 операторов SQL, соответствующих 4 операциям CRUD.
  2. Операторы CRUD могут быть автоматически сгенерированы на лету на основе имени таблицы и определений полей.
  3. Для каждого поля можно указать псевдоним.
  4. Доступны следующие типы на стороне клиента, автоматически сопоставленные с собственным типом базы данных: 
     1. String
     2. 64-bit integer
     3. 32-bit integer
     4. Boolean
     5. Date, Time, DateTime
     6. Blob (base64-encoded)
  5. Встроена поддержка последовательностей для генерации идентификаторов.
  6. Полный контроль над тем, какие операции (GET, POST, PUT, DELETE) разрешены.
  7. Ресурсы собраны в схему.
  8. К сервису могут быть прикреплены несколько схем.
  9. Могут быть заданы несколько баз данных (соединений).
  10. Схема может быть привязана к одному соединению, или соединения могут совместно использовать схемы (вариант использования: другое соединение для клиента).
  11. Компоненты бизнес-процессоров могут быть подключены к ресурсу, чтобы упростить реализацию бизнес-логики.
  12. Выражения SQL могут содержать параметры, значения для параметров будут взяты из URL запроса.
  13. Поддержка для получения пользовательских наборов данных.
  14. Поддержка клиентских операторов SQL SELECT (необязательно, по умолчанию отключено)
  15. Полная поддержка конфигурации через файл `.ini` из коробки.



### На стороне HTTP

  1. Аутентификация обрабатывается с использованием протокола HTTP.
  2. Базовая аутентификация включена по умолчанию, но полностью подключаема.
  3. Обычная аутентификация может искать действительных пользователей в базе данных (по умолчанию база данных незащищенная).
  4. Выходной формат может быть зафиксирован или определен для каждого запроса (`?Fmt=format`). Обнаружение по типу контента также доступно.
  5. Список полей для включения в вывод можно указать в URL:  
`fl=field1,field2`
  6. Список полей, исключаемых из вывода, может быть указан в URL:  
`fe=field1,field2`
  7. Различные форматы вывода доступны из коробки 
     1. JSON (по умолчанию)
     2. XML (пользовательский формат)
     3. CSV (для ввода и вывода)
     4. CDS (Формат, используемый Delphi'йским `TClientDataset`)
     5. Запланировано: пакеты данных ADO (как он используется MS Access)
  8. Используется заводской шаблон, новые форматы могут быть добавлены по желанию.
  9. Простые схемы URL. Доступны 2 основные схемы
         
         BASEURL/Resource/
         BASEURL/Resource/ID

или используя соединение в качестве префикса:
         
         BASEURL/Connection/Resource/
         BASEURL/Connection/Resource/ID

  10. Поддержка самоанализа/обнаружения или метаданных:
         
         BASEURL/metadata/

возвращает список доступных ресурсов и их операций.
         
         BASEURL/metadata/resourcename

возвращает структуру ресурса: поля, типы и т. д.
  11. параметры `limit` и `offset` для подкачки результатов:
         
         BASEURL/resourcename?limit=10
         BASEURL/resourcename?limit=10&Offset=50

Может быть применен максимальный лимит. Когда операторы SQL поддерживают это, `limit` и `offset` переводятся в предложения fetch/offset [языка] SQL.
  12. Порядок сортировки можно указать в URL-адресе запроса, если это позволяет определение ресурса, используя `?Sort=fieldname`. Возможность сортировки может быть указана на основе поля.
         
         BASEURL/resourcename?sort=MyField
         BASEURL/resourcename?sort=MyField%20desc

  13. Фильтрация может быть указана на основе поля в URL-адресе запроса, и операция фильтрации преобразуется в SQL-предложение Where:
         
         BASEURL/resourcename?MyField=10
         BASEURL/resourcename?MyField_null=1
         BASEURL/resourcename?MyField_null=0
         BASEURL/resourcename?MyField_lt=10
         BASEURL/resourcename?MyField_lte=10
         BASEURL/resourcename?MyField_gt=10
         BASEURL/resourcename?MyField_gte=10

переводится в SQL-предложение WHERE:
         
         (MyField=10)
         (MyField is null)
         (MyField is not null)
         (MyField<10)
         (MyField_lte<=10)
         (MyField>10)
         (MyField>=10)

Возможность фильтрации по полю можно указать в схеме для каждого поля.
  14. Запрос может указать, должны ли метаданные быть включены в ответ:
         
         BASEURL/resourcename?metadata=1

  15. Запрос может указывать, должен ли результат быть понятным для человека или нет
         
         BASEURL/resourcename?humanreadable=1

Если установлено значение 1, результат запроса будет отформатирован. Не все форматы поддерживают это.




## Доступные компоненты

### TSQLDBRestSchema

Представляет схему REST: список доступных ресурсов с их разрешенными операциями, определения полей, операторы SQL. 

#### Свойства

  * **Resources** : коллекция ресурсов. Каждый пункт имеет тип _TSQLDBRestResource_
  * **ConnectionName** : установите это для имени соединения, если вы хотите, чтобы все ресурсы этой схемы использовали это соединение.



#### Методы

  * **SaveToFile** сохраняет определения ресурса в файл (как JSON)


    
    
    Procedure SaveToFile(Const aFileName : UTF8String);
    

  * **SaveToFile** сохраняет определения ресурсов в поток (как JSON)


    
    
    Procedure SaveToStream(Const aStream : TStream);
    

  * **AsJSON** возвращает определения ресурса в виде объекта JSON


    
    
    function AsJSON(const aPropName: UTF8String=''): TJSONData;
    

Если указано, **aPropName** \- это имя свойства, ниже которого хранятся все определения. 

  * **LoadFromFile** загружает определения ресурсов из файла JSON


    
    
    Procedure LoadFromFile(Const aFileName : UTF8String);
    

  * **LoadFromStream** загружает определения ресурсов из потока JSON


    
    
    Procedure LoadFromStream(Const aStream : TStream);
    

  * **FromJSON** загружает определения ресурсов из файла JSON


    
    
    Procedure FromJSON(aData: TJSONData;const aPropName: UTF8String='');
    

Если указано, **aPropName** \- имя свойства, под которым расположены все определения. 

  * **PopulateResourceFields** создает поля ресурсов на основе имени таблицы


    
    
    procedure PopulateResourceFields(aConn: TSQLConnection; aRes: TSQLDBRestResource; aMinFieldOpts : TRestFieldOptions = []);
    

_PopulateResourceFields_ заполняет определения полей в _aRes_ , проверяя таблицу в соединении с базой данных _aConn_. Каждое созданное поле получает как минимум _aMinFieldOptions_ в его свойстве _Options_

  * **PopulateResources** создает определение ресурса для каждой таблицы в соединении с базой данных


    
    
    procedure PopulateResources(aConn: TSQLConnection; aTables: array of string; aMinFieldOpts: TRestFieldOptions= []);
    procedure PopulateResources(aConn : TSQLConnection; aTables : TStrings = Nil; aMinFieldOpts : TRestFieldOptions = []);
    

  * _PopulateResources_ извлекает список таблиц из _aConn_ , создает определение ресурса для таблицы и вызывает _PopulateResourceFields_ , чтобы заполнить ресурс полями. Если указано _aTables_ , определения будут создаваться только для таблиц в этом списке.



### TSQLDBRestDispatcher

#### Свойства

  * **Active** (Boolean) Регистрирует или отменяет регистрацию HTTP маршрутов
  * **Connections** (TSQLDBRestConnectionList) Список подключений к базе данных для соединения
  * **Schemas** (TSQLDBRestSchemaList) Список схем REST для обслуживания
  * **BasePath** (UTF8String) Базовый URL для REST URL
  * **DefaultConnection** (UTF8String) Соединение по умолчанию для использования, если ни одно не обнаружено из запроса/схемы
  * **Strings** (TRestStringsConfig Property) Конфигурации строк ввода/вывода
  * **OutputOptions** (TRestOutputOptions) Параметры вывода по умолчанию, изменяемые по запросу. Один или несколько из:


  1. _ooMetadata_ отправка метаданных в запросе
  2. _ooSparse_ не отправлять null-значения (если формат позволяет)
  3. _ooHumanReadable_ "человекочитабельная" печать


  * **InputFormat** (string) - Имя формата ввода, используемого для всех запросов.
  * **OutputFormat** (string) - Имя формата вывода, используемого для всех запросов.
  * **DispatchOptions** (TRestDispatcherOptions) - Опции диспетчера. Один или несколько из


  1. _rdoConnectionInURL_ \- использовать соединение в качестве элемента пути в URL
  2. _rdoExposeMetadata_ \- выставлять URL метаданных
  3. _rdoCustomView_ \- разрешить использование пользовательского URL-адреса (выбирается SQL, указанный в запросе)
  4. _rdoHandleCORS_ \- обрабатывать предпосылаемый запрос CORS
  5. _rdoAccessCheckNeedsDB_ \- эта [опция] контролирует, когда проверка доступа выполняется для ресурсов: до или после установления соединения с базой данных. Если вам необходимо выполнить дополнительные запросы к базе данных, чтобы решить, предоставляется ли доступ (например, проверка ACL в БД), этот параметр должен быть включен


  * **Authenticator** (TRestAuthenticator) - аутентификатор для запросов. Если установлено значение _OnBasicAuthentication_ , его можно оставить пустым.
  * **EnforceLimit** (Integer) - если больше нуля, установить ограничение на выходные результаты.
  * **CORSAllowedOrigins** (String) - Разделенный запятыми список доменов, которым разрешено использовать эту службу REST. Если пусто, в предварительном запросе будет возвращено *.



#### Методы

  * **ExposeDatabase** выставляет базу данных в качестве службы REST. Это создаст определение соединения с предоставленными параметрами и вызовет **ExposeConnection** с этим определением.
        
        Function ExposeDatabase(Const aType,aHostName,aDatabaseName,aUserName,aPassword : String; aTables : Array of String; aMinFieldOpt)
        Function ExposeDatabase(Const aType,aHostName,aDatabaseName,aUserName,aPassword : String; aTables : TStrings = nil; aMinFieldOpt)
        

The connection gets the name **ConnectionN** , where N is a unique number, and the schema is named **SchemaConnectionN**. The Tables and aMinFieldOpt arguments are the same as in the _PopulateResources_ call of **TSQLDBRestSchema**
  * **ExposeConnection** exposes a database (represented by _aConnection_) as a REST service. This will create a schema with a resource definition.
        
        Function ExposeConnection(Const aConnection : TSQLDBRestConnection; aTables : TStrings = nil; aMinFieldOpts : TRestFieldOptions
        

The Tables and aMinFieldOpt arguments are the same as in the _PopulateResources_ call of **TSQLDBRestSchema**




### TRestBasicAuthenticator

This class implements HTTP Basic authentication for the SQLDB REST Dispatcher 

  * AuthConnection (TSQLConnection) a SQL connection that will be used to run the SQL. By default the connection of the request is used. Set this if your users are in a separate database.
  * AuthenticateUserSQL (TStrings) an SQL Statement that will be executed on the connection (AuthConnection or connection of the request). This must contain 2 parameters **userName** and **password** and return a single field. This field will be used as **UserID** (available in the rest of the request flow.
  * DefaultUserName (UTF8String) a valid username, not in the database.
  * DefaultPassword (UTF8String) password of a default user.
  * DefaultUserID (UTF8String) the user ID reported if the default user is authenticated.
  * AuthenticationRealm : (UTF8String) Real sent in the **WWW-Authenticate** challenge to the client.
  * OnBasicAuthentication (event) triggered when basic authentication is requested. Here you can enter custom code to validate the user.



### TSQLDBRestModule

  * This is a simple **TWebModule** descendant which is usable in Lazarus IDE. It has a single _Dispatcher_ property which must be set to a **TSQLDBRestDispatcher** instance.



### TSQLDBRestResource

This is a collection item, found in the 'Resources' property of the **TSQLDBRestSchema**. It represents a single REST resource. 

#### properties

It has the following properties: 

  * **Fields** (TSQLDBRestFieldList) A list of fields that will be included in the output, or can be supplied in the input.
  * **Enabled** (Boolean) Is this resource enabled ?
  * **InMetadata** (Boolean) Should this resource be shown in the metadata list ?
  * **ConnectionName** (UTF8String) Connection name to use for this resource.
  * **TableName** (UTF8String) Database table name. This must only be specified if the SQL is auto generated.
  * **ResourceName** (UTF8String) Name of this resource as exposed to the outside world.
  * **AllowedOperations** (TRestOperations) The operations allowed on this resources (Get/Put/Post/Delete/Options/Head)
  * **SQLSelect** (TStrings) SQL select statement for a GET request. Left empty, it is autogenerated based on the table name. The where clause uses the fields with the _foInKey_ flag set for single resources.
  * **SQLInsert** (TStrings) SQL INSERT statement for a POST request. Left empty, it is autogenerated based on the table name.
  * **SQLUpdate** (TStrings) SQL UPDATE statement for a PUT request. Left empty, it is autogenerated based on the table name. The where clause uses the fields with the _foInKey_ flag set.
  * **SQLDelete** (TStrings) SQL DELETE statement for a DELETE request. Left empty, it is autogenerated based on the table name. The where clause uses the fields with the _foInKey_ flag set.
  * '_SQL_ an array of SQL statements, based on the previous ones.
  * **BusinessProcessor** (TSQLDBRestCustomBusinessProcessor) at runtime this contains the business processor for this resource (if any)



#### Events

The following events are available: 

  * **OnResourceAllowed** (TSQLDBRestAllowResourceEvent) Called to check whether the user is allowed to access this resource.
  * **OnAllowedOperations** (TSQLDBRestAllowedOperationsEvent) Called to allow to fine-tune the allowed operations for the user.
  * **OnGetDataset** (TSQLDBRestGetDatasetEvent) You can return a custom dataset for GET operations.
  * **OnCheckParams** (TSQLDBRestCheckParamsEvent) You can check the INSERT/UPDATE/DELETE SQL Statement parameters in this request/
  * **OnAllowRecord** (TSQLDBRestAllowRecordEvent) Called for each record, you can ddecide whether the record should be included in the output.



Note: when set, the corresponding event in the business processor is not triggered. 

#### Methods

The following methods exist: 
    
    
     Function GetDataset(aContext : TBaseRestContext; aFieldList : TRestFieldPairArray; aOrderBy : TRestFieldOrderPairArray; aLimit, aOffset : Int64) : TDataset;
    

Get the custom dataset for this resource. 
    
    
     function GenerateDefaultSQL(aKind: TSQLKind): UTF8String; virtual;
    

Get the default SQL for this resource if none was specified. 
    
    
     function GetFieldList(aListKind: TFieldListKind): UTF8String;
    

Return a list of fields as a string, separated by a correct separator token 
    
    
     function GetFieldArray(aListKind: TFieldListKind): TSQLDBRestFieldArray;
    

Return a list of fields, similar to GetFieldList, but returns the raw fields. 
    
    
     Function GetResolvedSQl(aKind : TSQLKind; Const AWhere : UTF8String; Const aOrderBy : UTF8String = _; aLimit : UTF8String =_) : UTF8String;
    

Return the actual SQL Statement that will be executed, with macros 
    
    
     Procedure PopulateFieldsFromFieldDefs(Defs : TFieldDefs; aIndexFields : TStringArray; aProcessIdentifier : TProcessIdentifier; aMinFieldOpts : TRestFieldOptions);
    

Populate the fields collection from a TFieldDefs collection. 

### TSQLDBRestField

The TSQLDBREstResource has a collection of fields (TSQLDBRestField) that determine the output fields of the REST resource 

It has the following properties; 

  * FieldName (UTF8String Read FFieldName Write FFieldName;
  * PublicName (UTF8String Read GetPublicName Write FPublicName;
  * GeneratorName (String) name of a generator to privide a unique value.
  * FieldType (TRestFieldType) Data type of the exposed field.One of (the names speak for themselves) 
    * rftInteger
    * rftLargeInt
    * rftFloat
    * rftDate
    * rftTime
    * rftDateTime
    * rftString
    * rftBoolean
    * rftBlob (sent as base64-encoded data)
  * NativeFieldType (TFieldType) Native database field type of this field.
  * Options (TRestFieldOptions) options for this field. One of more of: 
    * foInKey This is a key field.
    * foInInsert The field can be inserted (if statement is autogenerated)
    * foInUpdate The field can be updated (if statement is autogenerated)
    * foRequired A field value is required.
    * foFilter Filtering is allowed on this field.
    * foOrderBy Ordering is allowed on this field.
    * foOrderByDesc Ordering (descendent) is allowed on this field.
  * Filters (TRestFieldFilters) Allowed filter expressions for this field. One or more of: 
    * rfEqual
    * rfLessThan
    * rfGreaterThan
    * rfLessThanEqual
    * rfGreaterThanEqual
    * rfNull
  * MaxLen (integer) for string fields, the maximum allowed length of the field.



### TSQLDBRestBusinessProcessor

This class serves to implement any business rules you want to attach to your rest resource. You can drop one TSQLDBRestBusinessProcessor instance per resource on a module, set the schema and resource name (a list of resource names is available in the Object Inspector) and implement the events. 

The following properties and events are available: 

  * **Schema** : The schema in which the resource resides.
  * **ResourceName** The resource for which the business rules are valid.
  * **OnGetDataset** : Event called when you want to create a custom dataset. You must free the dataset yourself.
  * **OnCheckParams** : Event in which you can verify whether the parameters for insert/update/delete/select queries are OK.
  * **OnAllowResource** : Event to decide whether a property is allowed for a request. You can use this e.g. to forbid certain users from accessing a resource.
  * **OnAllowedOperations** : Event to decide whether a REST operation is allowed for a request. You can use this to forbid certain users from writing to a resource, but allow them to read.
  * **OnAllowRecord** : Called for each record which will be output. This can be used to prevent certain records from being streamed to the output. Note that if _Limit_ is specified, this will not be taken into account, i.e. if the limit is 10, and you forbid 2 records from being streamed at this point, only 8 records will be returned.



All calls get passed a TRestContext instance. This contains 2 properties and a method: 
    
    
      Function GetVariable(Const aName : UTF8String; aSources : TVariableSources; Out aValue : UTF8String) : Boolean;
    

Call this to get a HTTP Query variable, header,... The function returns true if the variable was found. 
    
    
      Property UserID : UTF8String
    

This will be set when calling. 
    
    
      Property Data : TObject
    

You can attach data to this if you want to. It will be kept for the duration of the request. You are responsible for freeing this data, though. 

## Special Resources

### metaData

The _/baseURL/metaData_ resource lists all the resources defined in the schema, together with the allowed operations. 

The usual formats and parameters are supported. 

### metaData/resourceName

The _/baseURL/metaData/resourceName_ resource lists all the fields in the **resourceName** resource, together with the properties. 

The usual formats and parameters are supported for this resource. 

  


### customview

The customView resource is special. Its availability must be enabled using the _Dispatcher'_ s Options. When _rdoCustomView_ is included in the options, the 
    
    
     baseURL/customView
    

URL becomes available. It allows the client to specify an SQL Select statement using the _SQL_ parameter: 
    
    
     baseURL/customView?SQL=select count(*) as thecount from person
    

(you must encode the sql using the usual URI encoding mechanism, it is omitted for readability reasons here) The usual parameters for fieldlists, format are supported, but not the _limit_ and _offset_ parameters. 

The engine will check that only SELECT statements passed on. Nevertheless, this is an inherently less safe mechanism which should only be used in case of need. 

### _connection

(Work in progress) This is a resource that allows you to manage the connections through REST. This can be useful in development environments, where you can simply create a single binary that does not need any configuration, the whole configuration can be done through REST, including the management of the available connections. 

Because this is security-wise a dangerous option to enable, it is disabled by default, and must be enabled explicitly with the _rdoConnectionResource_ option of the dispatcher. 

Use cases are a development environment where this can be used to confiure the restserver from within the IDE. When moving to production, the option can be disabled, and only fixed and known connections will be allowed/ 

## Available Output formats

### JSON (the default)

  * A simple JSON object encapsulating data, metadata and possibly error nodes.
  * rows are exported as JSON objects, field names are property names of the object.
  * Name: 'json'
  * Content-type: 'application/json'
  * Input and output.
  * Data by default under 'data'
  * Metadata by default under 'metaData'
  * Errors reported under 'error'
  * Property names are configurable in REST dispatcher.



### XML (a custom format)

  * A simple XML document encapsulating data, metadata and possibly error nodes, under a root element 'datapacket'
  * rows are exported as XML elements (named 'row') , the contents as text in the element.
  * Name: 'xml'
  * Content-type: 'text/xml'
  * Input and output.
  * Data by default under 'data'
  * Metadata by default under 'metadata'
  * Errors reported under 'error'
  * Element names are configurable in REST dispatcher.



### CSV

  * Simple comma-separated CSV list.
  * Name: 'csv'
  * Content-type: 'text/csv'
  * Input and output.
  * Separator: Comma
  * Quotes when needed.
  * Metadata means first line contains fieldnames.



### CDS

  * XML package using the XMLFormat used by Delphi `TClientDataset`.
  * Name: 'cds'
  * Content-type: 'text/csv'
  * Input and output.



A Delphi demo program is provided. 

### ADO

  * XML format as used by ADO recordsets (as used by MS Access)
  * Name: 'ado'
  * Input and output.



## Ini file Support

The following classes can load/save their configuration settings from/to an .ini file 

  * TSQLDBRestDispatcher
  * TRestBasicAuthenticator
  * TSQLDBRestConnection
  * TRestStringsConfig



The support for this is implemented in 2 units; 

  * the sqldbauthini unit has a type helper for the TRestBasicAuthenticator class.
  * The sqldbrestini unit has the type helpers for TSQLDBRestDispatcher and TSQLDBRestConnection and TRestStringsConfig.



The helpers implement LoadFromFile/SaveToFile methods, as well as LoadFromIni/SaveToIni methods. There are some options to control the level of depth. 

The schemas (if so required) are saved as JSON files in the same directory as the .ini file, using the name of the schema as the filename. 

## Lazarus Support

Lazarus support for the SQLDB REST Bridge is in the lazsqldbrest.lpk package, under the fpweb directory. It registers the TSQLDBRestDispatcher, TSQLDBRestSchema and TRestBasicAuthenticator components on the component palette: 

[![sqldbrestcomponents.png](https://wiki.freepascal.org/images/5/57/sqldbrestcomponents.png)](</File:sqldbrestcomponents.png>)

Additionally it registers some component- and property editors: 

Further integration with Lazarus is planned: 1\. Add a nicer schema editor 1\. Let the IDE act as a REST server. (alternatively, add a tool with a menu entry under tools for easy configuration) 

Lazarus support can work with FPC 3.0.4. To make this work, the files can be copied from SVN or a 3.2 release to the lazarus fpweb directory. 

To this end, 

  1. create a directory **src** below the **components/fpweb directory** (where the package is located)
  2. copy all files in directory


    
    
     packages/fcl-web/src/restbridge de
    

    to directory
    
    
     lazarus/components/fpweb/src
    

  1. compile the lazsqldbrest.lpk package. It should find the files in the src directory and use them.



## Pas2JS Support

The SQLDB Rest bridge will be integrated in the compile server that comes with pas2JS: This means that pas2js will come with a toll that 

  * Can recompile your web project on the fly
  * Can serve files on disc
  * Can act as a REST server.



So no actual server is needed during development. 

## Usage

### In a fcl-web application.

The rest bridge was designed to be simple to use. 

#### Expose a single database, no authentication.

Simple expose of a database. Run the following code in the startup of your program (for example, DoRun of the web application class, or startup code of the program) 
    
    
    FDisp:=TSQLDBRestDispatcher.Create(Self);
    FDisp.ExposeDatabase(
        'postgres','localhost','expensetracker','me','secret'
        ,Nil,
        [foFilter,foInInsert,foInUpdate,foOrderByDesc]);
    FDisp.Active:=True;   
    FDisp.SaveToFile('demo.ini');
    

This exposes all tables of the database 'expenstracker', allows to filter,update and sort on all fields. The connection will be named 'Connection1', the schema 'SchemaConnection1' 

The configuration will be saved in a file called 'demo.ini'.  
It can be reused to quickly set up a REST dispatcher. 

#### Expose single database, HTTP Basic authentication.

Similar to the previous example, but with HTTP BASIC authentication for a single user: 
    
    
    FAuth:=TRestBasicAuthenticator.Create(Self);
    FAuth.DefaultUserName:='me';
    FAuth.DefaultPassword:='secret';
    FDisp:=TSQLDBRestDispatcher.Create(Self);
    FDisp.Authenticator:=Fauth;
    FDisp.ExposeDatabase(
        'postgres','localhost','expensetracker','me','secret'
        ,Nil,
        [foFilter]);
    FDisp.Active:=True;
    

This exposes all tables of the database 'expenstracker', allows to filter,update and sort on all fields. The connection will be named 'Connection1', the schema 'SchemaConnection1'. No updates are possible: the fields will not be in update/insert statements. Sorting is also not allowed. 

#### Expose single database, HTTP Basic authentication (2)

Smilar to the previous example, but with HTTP BASIC authentication, using the database to authenticate a user: 
    
    
    FAuth:=TRestBasicAuthenticator.Create(Self);
    FAuth.AuthenticateUserSQL.Text:='select uID from users where (uLogin=:UserName) and (uPassword=:Password)';
    FDisp:=TSQLDBRestDispatcher.Create(Self);
    FDisp.Authenticator:=Fauth;
    FDisp.ExposeDatabase(
        'postgres','localhost','expensetracker','me','secret'
        ,['projects','expenses'],
        []);
    FDisp.Active:=True;
    

Here only the 'projects' and 'expenses' tables are exposed, read-only. 

#### Using a Config file and schema files.

Start a dispatcher, loading the file demo.ini. The dioSkipReadSchemas tells it not to load schema files, the schemas will be created from the connections. 
    
    
    FDisp:=TSQLDBRestDispatcher.Create(Self);
    FDisp.LoadFromFile('demo.ini',[dioSkipReadSchemas]);
    FDisp.Active:=True;
    

The .ini file can be created using SaveToFile on a configured dispatcher instance. 

### In the lazarus IDE.

To work in the lazarus IDE with the SQLDB Rest bridge, you must install the **lazsqldbrest** package, which can be found in the directory components/fpWeb. 

  


#### Using a datamodule

In this approach, a datamodule is created that is in memory throughout the whole lifetime of the application (fcl-web application) 

  


  1. Create a HTTP server application or FCGI application. (a CGI will also work but is very inefficient)
  2. Add a plain datamodule to the application.
  3. Drop a TSQLDBRestSchema class from the 'FCL-Web' tab of the component palette
  4. Edit the schema (property resources), or import it from file using the component editor context menu
  5. Drop a TSQLDBRestDispatcher class from the 'FCL-Web' tab of the component palette
  6. Edit the connections
  7. Add an item to the Schemas property and point it to the TSQLDBRestSchema added previously
  8. For HTTP Basic authentication, add a TRESTBAsicAuthenticator class, and set the TSQLDBRestDispatcher instance's Authentocator property to it.



The end result should look more or less like this: 

[![restmodule.png](https://wiki.freepascal.org/images/b/b2/restmodule.png)](</File:restmodule.png>)

That's it. The project is ready to run. 

#### Using a SQLDBRestdatamodule

This approach is basically the same as the previous one. 

  1. Create a HTTP application
  2. Under File - New, create a SQLDB Rest Bridge Module.
  3. Drop a SQLDBRestDispatcher component on the module.
  4. The Dispatcher property of the module should be set to the SQLDBRestDispatcher component.
  5. Set up the component as required, as in the previous example.



## Examples

### FPC

  * in FPC Packages, the fcl-web/examples/restbridge directory, there is a small example that serves a expenses tracker database.
  * The directory (delphiclient) below contains a small Delphi project that shows how to connect a TClientDataset to the SQLDB Rest bridge.



    Design time view:
    [![delphirestclientdesign.png](https://wiki.freepascal.org/images/5/5f/delphirestclientdesign.png)](</File:delphirestclientdesign.png>)
    Run time view:
    [![delphirestclient.png](https://wiki.freepascal.org/images/4/48/delphirestclient.png)](</File:delphirestclient.png>)

### Lazarus

There are 2 server examples available, and several client examples. The client examples are all the same, they just use a different kind of _TDataset_ to show the data coming from the server: the **buf** , **json** and **CSV** formats are demonstrated in this way. 

#### Server examples

  * The demo/restbridge directory of Lazarus/Components contains the same example program, using a datamodule.
  * The demo/restmodule directory of Lazarus/Components contains the same example program, using a SQLDB Rest Web datamodule.



#### TBufDataset client example

  * The demo/bufclient directory contains a demo that shows how to load data from a SQLDB REST bridge server in a TBUFDataset dataset. It uses the 'buf' format to do so.



    [![sqldbrestbufclientdata.png](https://wiki.freepascal.org/images/8/8a/sqldbrestbufclientdata.png)](</File:sqldbrestbufclientdata.png>)

    [![sqldbrestbufclientraw.png](https://wiki.freepascal.org/images/0/0d/sqldbrestbufclientraw.png)](</File:sqldbrestbufclientraw.png>)
    it also demonstrates how to use the metadata resource of the server to list the available resources

#### TJSONDataset client example

  * The demo/jsonclient directory contains a demo that is similar to the bufclient demo:



    it shows how to load data from a SQLDB REST bridge server in a TBaseJSONDataset dataset, using the 'csv' format
    [![sqldbrestjsonclientdata.png](https://wiki.freepascal.org/images/8/86/sqldbrestjsonclientdata.png)](</File:sqldbrestjsonclientdata.png>)

    [![sqldbrestjsonclientraw.png](https://wiki.freepascal.org/images/1/1d/sqldbrestjsonclientraw.png)](</File:sqldbrestjsonclientraw.png>)
    Like the bufdataset demo, it also demonstrates how to use the metadata resource of the server to list the available resources

#### TCSVDataset client example

  * The demo/csvclient directory contains a demo that is similar to the bufclient demo:



    it shows how to load data from a SQLDB REST bridge server in a TCSVDataset dataset, using the 'csv' format

### Pas2JS

  * A similar application to the lazarus bufflien/jsonclient programs exists for pas2JS in demo/restbridge/simple



    [![sqldbrestbridgepas2js.png](https://wiki.freepascal.org/images/4/41/sqldbrestbridgepas2js.png)](</File:sqldbrestbridgepas2js.png>)

## TODO

The following extensions are still planned: 

  * Support for `?q=filterexpression` in URL filters.
  * Connection management API. (zero-config service)
  * Use HTTP credentials to connect to the database.
  * Unknown query params are now ignored. Allow to check for valid query parameters and raise error if unknown query param is encountered.

---

_Source: [https://wiki.freepascal.org/SQLDBRestBridge/ru](https://web.archive.org/web/20231204172948/https://wiki.freepascal.org/SQLDBRestBridge/ru)_
