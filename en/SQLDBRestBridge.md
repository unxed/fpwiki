# SQLDBRestBridge

│ **English (en)** │  **[русский (ru)](<../ru/SQLDBRestBridge.md>)** │

  


## Contents

  * 1 Purpose
    * 1.1 What does it do ?
    * 1.2 What it does not do ?
  * 2 Features
    * 2.1 SQL Database side.
    * 2.2 HTTP side
  * 3 Available Components
    * 3.1 TSQLDBRestSchema
      * 3.1.1 Properties
      * 3.1.2 Methods
    * 3.2 TSQLDBRestDispatcher
      * 3.2.1 Properties
      * 3.2.2 Methods
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



## Purpose

More and more applications move to Web: today Pas2JS allows to program pascal for the web. 

Data needs to be stored on a server, and REST is the architecture of choice these days for doing so. Most often, the data ends up in a database. 

So, an easy way to make data available through a REST service is needed. 

The `SQLDBRestBridge` unit in _fpc/packages/fcl-web/src/restbridge/sqldbrestbridge.pp_ offers a means to expose any database that can be accessed by SQLDB using a REST API. 

It is not meant as a general REST API framework, for this other frameworks exist. It is also not a RPC framework. 

The design is such that a complete database can be exposed using a single line of code, with reasonable default settings. 

### What does it do ?

Basically, `SQLDBRestBridge` can be used in a FCL-Web application to turn it into a REST server: 

The server will then accept the usual REST GET/POST/PUT/Delete commands and automatically translate these to the appropriate database CRUD (Create/Read/Update/Delete) statements. 

It can be used simply: one line of code is enough to expose a complete database in this way. 

Or it can be completely customized and fine tuned: The schema can be completely specified: all SQL statements can be specified, field lists can be defined, aliases can be given, business rules can be implemented. 

The resulting server can be used in a client/server setup (using pas2js as a client, for example) to serve data without needing to writing all the plumbing code needed in a REST server. 

### What it does not do ?

  * This is a server-side technology.



    To consume the data served by this component, you need to write a client.
    No assumptions are made about the client, except that it knows how to execute HTTP requests, and that it understands one of the available data formats.

  * This does not expose objects as REST resources.



    Using some tricks, this could be done, but this is not a design goal.
    Eventually, all data ends up in a database, and this component uses a direct mechanism to achieve that.

## Features

### SQL Database side.

  1. Every REST resource is defined by up to 4 SQL statements, corresponding to the 4 CRUD operations.
  2. The CRUD Statements can be auto generated on the fly, based on a table name and field definitions.
  3. For every field, an alias can be provided.
  4. The following client-side types are available, automatically mapped from the native database type: 
     1. String
     2. 64-bit integer
     3. 32-bit integer
     4. Boolean
     5. Date, Time, DateTime
     6. Blob (base64-encoded)
  5. Support for a sequence to generate IDs is built-in.
  6. Full control over which operations (GET,POST,PUT,DELETE) are allowed.
  7. Resources are collected in a schema.
  8. Multiple schemas can be attached to the service.
  9. Multiple databases (connections) can be defined.
  10. A schema can be tied to a single connection, or connections can share a schemas (use case: a different connection per client).
  11. Business processor components can be hooked to a resource, to make it easier to implement business logic.
  12. SQL Statements can contain parameters, values for the parameters will be picked up from the request URL.
  13. Support to get custom datasets.
  14. Support for client-side provided SQL SELECT statements (optional, disabled by default)
  15. Full support for configuration through an `.ini` file out of the box.



### HTTP side

  1. Authentication is handled using the HTTP protocol.
  2. Basic authentication is included by default, but is completely pluggable.
  3. Basic authentication can look up valid users in a database (by default the database being exposed)
  4. Output format can be fixed or detected per request (`?fmt=format`). Detection based on content-type is also available.
  5. list of fields to include in output can be specified in the URL:  
`fl=field1,field2`
  6. Field list of fields to exclude from output can be specified in the URL:  
`fe=field1,field2`
  7. Various output formats are available out of the box 
     1. JSON (the default)
     2. XML (a custom format)
     3. CSV (for in and output)
     4. CDS (Format used by Delphi `TClientDataset`)
     5. Planned: ADO data packets (as used by MS Access)
  8. A factory pattern is used, new formats can be added at will.
  9. Simple URL schemes. 2 basic schemes are available
         
         BASEURL/Resource/
         BASEURL/Resource/ID

or using the connection as prefix:
         
         BASEURL/Connection/Resource/
         BASEURL/Connection/Resource/ID

  10. Support for introspection/discovery or metadata:
         
         BASEURL/metadata/

returns the list of available resources and their operation.
         
         BASEURL/metadata/resourcename

returns the structure of the resource: fields, types, etc.
  11. `limit` and `offset` parameters for paging of results:
         
         BASEURL/resourcename?limit=10
         BASEURL/resourcename?limit=10&Offset=50

A maximum limit can be enforced. When the SQL statements support it, the limit and offset are translated to SQL fetch/offset clauses.
  12. Sort order can be specified in the request URL, if the resource definition allows it, using `?sort=fieldname`. The ability to sort can be specified on a field basis.
         
         BASEURL/resourcename?sort=MyField
         BASEURL/resourcename?sort=MyField%20desc

Multiple sort fields can be specified, separated by a comma. 

  13. Filtering can be specified on a field basis in the request URL, and the filter operation is translated to SQL Where:
         
         BASEURL/resourcename?MyField=10
         BASEURL/resourcename?MyField_null=1
         BASEURL/resourcename?MyField_null=0
         BASEURL/resourcename?MyField_lt=10
         BASEURL/resourcename?MyField_lte=10
         BASEURL/resourcename?MyField_gt=10
         BASEURL/resourcename?MyField_gte=10

is translated to a SQL where clause:
         
         (MyField=10)
         (MyField is null)
         (MyField is not null)
         (MyField<10)
         (MyField_lte<=10)
         (MyField>10)
         (MyField>=10)

The ability to filter on a field can be specified in the schema for each field.
  14. The request can specify whether metadata should be included in the response:
         
         BASEURL/resourcename?metadata=1

  15. The request can specify whether the result should be human-readable or not
         
         BASEURL/resourcename?humanreadable=1

If set to 1, the request result will be formatted. Not all formats support this.




## Available Components

### TSQLDBRestSchema

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestschema.pp_

Represents a REST schema : a list of available resources, with their allowed operations, fields definitions, SQL statements. A schema can be designed visually using a [SQLDBRestSchemaEditor](<SQLDBRestSchemaEditor.md> "SQLDBRestSchemaEditor") in a separate application or in the lazarus IDE. 

#### Properties

  * **Resources** A collection of resources. Each item is of type _TSQLDBRestResource_
  * **ConnectionName** Set this to a connection name if you want all resources of this schema to use this connection.



#### Methods

  * **SaveToFile** save the resource definitions to file (as JSON)


    
    
    Procedure SaveToFile(Const aFileName : UTF8String);
    

  * **SaveToStream** save the resource definitions to stream (as JSON).


    
    
    Procedure SaveToStream(Const aStream : TStream);
    

  * **AsJSON** return the resource definitions as a JSON object.



    
    
    function AsJSON(const aPropName: UTF8String=''): TJSONData;
    

If specified, **aPropName** is the name of the property below which all definitions are stored. 

  * **LoadFromFile** Load the resource definitions from a JSON file.


    
    
    Procedure LoadFromFile(Const aFileName : UTF8String);
    

  * **LoadFromStream** Load the resource definitions from a JSON file.


    
    
    Procedure LoadFromStream(Const aStream : TStream);
    

  * **FromJSON** Load the resource definitions from a JSON file.


    
    
    Procedure FromJSON(aData: TJSONData;const aPropName: UTF8String='');
    

If specified, **aPropName** is the name of the property below which all definitions are located. 

  * **PopulateResourceFields** create resource fields based on table name.


    
    
    procedure PopulateResourceFields(aConn: TSQLConnection; aRes: TSQLDBRestResource; aMinFieldOpts : TRestFieldOptions = []);
    

_PopulateResourceFields_ populates the field definitions in _aRes_ by checking the table in database connection _aConn_. Every created field gets at least _aMinFieldOptions_ in its _Options_ property. 

  * **PopulateResources** creates a resource definition for each table in a database connection.


    
    
    procedure PopulateResources(aConn: TSQLConnection; aTables: array of string; aMinFieldOpts: TRestFieldOptions= []);
    Procedure PopulateResources(aConn : TSQLConnection; aTables : TStrings = Nil; aMinFieldOpts : TRestFieldOptions = []);
    

  * _PopulateResources_ retrieves the list of tables from _aConn_ , creates a resource definition for the table and calls _PopulateResourceFields_ to fill the resource with fields. If _aTables_ is specified, definitions will only be created for the tables in that list.



### TSQLDBRestDispatcher

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestbridge.pp_

#### Properties

  * **Active** (Boolean) Register or unregister HTTP routes.
  * **Connections** (TSQLDBRestConnectionList) List of database connections to connect to.
  * **Schemas** (TSQLDBRestSchemaList) List of REST schemas to serve
  * **BasePath** (UTF8String) Base URL for REST URLS.
  * **DefaultConnection** (UTF8String) Default connection to use if none is detected from request/schema
  * **Strings** (TRestStringsConfig Property) Input/Output strings configurations
  * **OutputOptions** (TRestOutputOptions) default Output options, modifiable by query. One or more of


  1. _ooMetadata_ Send metadata in request
  2. _ooSparse_ Do not send null values (if format allows it)
  3. _ooHumanReadable_ Pretty-print output.


  * **InputFormat** (string) Name of Input format to use for all requests.
  * **OutputFormat** (string) Name of Output format to use for all requests.
  * **DispatchOptions** (TRestDispatcherOptions) Dispatcher options. One or more of


  1. _rdoConnectionInURL_ Use the connection as a path element in the URL.
  2. _rdoExposeMetadata_ Expose the metadata URL.
  3. _rdoConnectionResource_ Expose the connection resource, allowing to manage connections through the REST protocol.
  4. _rdoCustomView_ allow the use of a customview URL (Select SQL specified in request)
  5. _rdoEmptyCorsDomainToOrigin_ if the CORDAllowedOrigins is empty, then the request server will be echoed to the client.
  6. _rdoHandleCORS_ Handle CORS preflight request.
  7. _rdoAccessCheckNeedsDB_ This controls when the access check is done for resources: before or after a connection with the database is made. If you need to make additional queries on a database to decide whether access is granted (such as checking ACLs in the Db) then this option should be enabled.


  * **AdminUserIDs** A list of user IDs (one per line) which are allowed to use the connection resource. If empty, all users can use it.
  * **LogOptions** Logging options. Logging happens through the OnLog event handler. This property controls which events are logged. The following values can be specified:


  1. _rloUser_ Include username in log messages, when available
  2. _rtloHTTP_ Log HTTP request (remote, URL)
  3. _rloResource_ Log resource requests (operation, resource)
  4. _rloConnection_ Log database connections (connect to database)
  5. _rloAuthentication_ Log authentication attempt
  6. _rloSQL_ Log SQL statements. (not on user-supplied connection)
  7. _rloResultStatus_ Log result status.


  * **LogSQLOptions** SQL Logging options. if rloSQL is specified, this option controls which SQLDB events are logged for new connections.
  * **Authenticator** (TRestAuthenticator) Authenticator for requests. If _OnBasicAuthentication_ is set, this can be left empty.
  * **EnforceLimit** (Integer) If larger than zero, Enforce a limit on output results.
  * **CORSAllowedOrigins** (String) Comma separated list of Domains that are allowed to use this REST service. If empty, * will be returned in preflight request.
  * **CORSMaxAge** (Integer) Value for HTTP _Access-Control-Max-Age_ header. If zero, the header is not sent.
  * **CORSAllowCredentials** (Boolean) value for HTTP header _Access-Control-Allow-Credentials_. Set to false not to send the header



#### Methods

  * **ExposeDatabase** exposes a database as a REST service. This will creates a connection definition with the provided parameters and calls **ExposeConnection** with this definition.
        
        Function ExposeDatabase(Const aType,aHostName,aDatabaseName,aUserName,aPassword : String; aTables : Array of String; aMinFieldOp
        Function ExposeDatabase(Const aType,aHostName,aDatabaseName,aUserName,aPassword : String; aTables : TStrings = nil; aMinFieldOpt
        

The connection gets the name **ConnectionN** , where N is a unique number, and the schema is named **SchemaConnectionN**. The Tables and aMinFieldOpt arguments are the same as in the _PopulateResources_ call of **TSQLDBRestSchema**
  * **ExposeConnection** exposes a database (represented by _aConnection_) as a REST service. This will create a schema with a resource definition.
        
        Function ExposeConnection(Const aConnection : TSQLDBRestConnection; aTables : TStrings = nil; aMinFieldOpts : TRestFieldOptions
        

The Tables and aMinFieldOpt arguments are the same as in the _PopulateResources_ call of **TSQLDBRestSchema**




### TRestBasicAuthenticator

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestauth.pp_

This class implements HTTP Basic authentication for the SQLDB REST Dispatcher 

  * AuthConnection (TSQLConnection) a SQL connection that will be used to run the SQL. By default the connection of the request is used. Set this if your users are in a separate database.
  * AuthenticateUserSQL (TStrings) an SQL Statement that will be executed on the connection (AuthConnection or connection of the request). This must contain 2 parameters **userName** and **password** and return a single field. This field will be used as **UserID** (available in the rest of the request flow.
  * DefaultUserName (UTF8String) a valid username, not in the database.
  * DefaultPassword (UTF8String) password of a default user.
  * DefaultUserID (UTF8String) the user ID reported if the default user is authenticated.
  * AuthenticationRealm : (UTF8String) Real sent in the **WWW-Authenticate** challenge to the client.
  * OnBasicAuthentication (event) triggered when basic authentication is requested. Here you can enter custom code to validate the user.



### TSQLDBRestModule

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestmodule.pp_

  * This is a simple **TWebModule** descendant which is usable in Lazarus IDE. It has a single _Dispatcher_ property which must be set to a **TSQLDBRestDispatcher** instance.



### TSQLDBRestResource

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestschema.pp_

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

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestschema.pp_

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

Unit: _fpc/packages/fcl-web/src/restbridge/sqldbrestschema.pp_

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

This is a resource that allows you to manage the connections through REST. This can be useful in development environments, where you can simply create a single binary that does not need any configuration, the whole configuration can be done through REST, including the management of the available connections. 

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

Requires Lazarus 2.1+ 

Lazarus support for the SQLDB REST Bridge is implemented in 2 packages under the **components/sqldbrest** directory: 

  1. The design/sqldbrestschemadesigner.lpk package.
  2. The lazsqldbrest.lpk package.



The latter package registers the **TSQLDBRestDispatcher** , **TSQLDBRestSchema** and **TRestBasicAuthenticator** components on the component palette: The former includes the schema editor. They should be compiled and installed in the order presented here. 

[![sqldbrestcomponents.png](https://wiki.freepascal.org/images/5/57/sqldbrestcomponents.png)](</File:sqldbrestcomponents.png>)

Additionally it registers some component- and property editors. In particular the GUI schema editor can be called from the **TSQLDBRestSchema** component context menu. 

The GUI schema editor is included in the IDE, it is discussed in a separate page [SQLDBRestSchemaEditor](<SQLDBRestSchemaEditor.md> "SQLDBRestSchemaEditor")

Further integration with Lazarus is planned: 1\. Let the IDE act as a REST server. (alternatively, add a tool with a menu entry under tools for easy configuration) 

Lazarus support can work with FPC 3.0.4. To make this work, the files can be copied from SVN or a 3.2 release to the lazarus components/sqldbrest/src directory. 

To this end, 

  1. create a directory **src** below the **components/sqldbrest directory** (where the package is located)
  2. copy all files in directory


    
    
     fpc/packages/fcl-web/src/restbridge
    

    to the new _src_ directory
    
    
     lazarus/components/sqldbrest/src
    

  1. compile the **lazarus/components/sqldbrest/design/sqldbrestschemadesigner.lpk** package. It has unitpath ../src, so it should find the files in the _src_ directory.
  2. compile the **lazarus/components/sqldbrest/lazsqldbrest.lpk** package.



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

To work in the lazarus IDE with the SQLDB Rest bridge, you must install the **lazsqldbrest** package, which can be found in the directory components/sqldbrest. 

  


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

Requires Lazarus 2.1+ 

There are 2 server examples available, and several client examples. The client examples are all the same, they just use a different kind of _TDataset_ to show the data coming from the server: the **buf** , **json** and **CSV** formats are demonstrated in this way. 

#### Server examples

  * The lazarus/components/sqldbrest/demo/restbridge directory contains the same example program restserver.lpi, using a datamodule.
  * The lazarus/components/sqldbrest/demo/restmodule directory contains the same example program demorestmodule.lpi, using a SQLDB Rest Web datamodule.



#### TBufDataset client example

  * The lazarus/components/sqldbrest/demo/bufclient directory contains a demo, that shows how to load data from a SQLDB REST bridge server in a TBUFDataset dataset. It uses the 'buf' format to do so.



    [![sqldbrestbufclientdata.png](https://wiki.freepascal.org/images/8/8a/sqldbrestbufclientdata.png)](</File:sqldbrestbufclientdata.png>)

    [![sqldbrestbufclientraw.png](https://wiki.freepascal.org/images/0/0d/sqldbrestbufclientraw.png)](</File:sqldbrestbufclientraw.png>)
    it also demonstrates how to use the metadata resource of the server to list the available resources

#### TJSONDataset client example

  * The lazarus/components/sqldbrest/demo/jsonclient directory contains a demo that is similar to the bufclient demo:



    it shows how to load data from a SQLDB REST bridge server in a TBaseJSONDataset dataset, using the 'csv' format
    [![sqldbrestjsonclientdata.png](https://wiki.freepascal.org/images/8/86/sqldbrestjsonclientdata.png)](</File:sqldbrestjsonclientdata.png>)

    [![sqldbrestjsonclientraw.png](https://wiki.freepascal.org/images/1/1d/sqldbrestjsonclientraw.png)](</File:sqldbrestjsonclientraw.png>)
    Like the bufdataset demo, it also demonstrates how to use the metadata resource of the server to list the available resources

#### TCSVDataset client example

  * The lazarus/components/sqldbrest/demo/csvclient directory contains a demo that is similar to the bufclient demo:



    it shows how to load data from a SQLDB REST bridge server in a TCSVDataset dataset, using the 'csv' format

### Pas2JS

Requires pas2js 1.5+ 

  * A similar application to the lazarus buffclient/jsonclient programs exists for pas2JS in pas2js/demo/restbridge/simple/restbridgeclient.lpi



    [![sqldbrestbridgepas2js.png](https://wiki.freepascal.org/images/4/41/sqldbrestbridgepas2js.png)](</File:sqldbrestbridgepas2js.png>)

## TODO

The following extensions are still planned: 

  * Support for `?q=filterexpression` in URL filters.
  * Connection management API. (zero-config service)
  * Use HTTP credentials to connect to the database.
  * Unknown query params are now ignored. Allow to check for valid query parameters and raise error if unknown query param is encountered.

---

_Source: [https://wiki.freepascal.org/SQLDBRestBridge](https://web.archive.org/web/20230320135039/https://wiki.freepascal.org/SQLDBRestBridge)_
