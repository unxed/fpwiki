# Lazarus-openapi

## Contents

  * 1 OpenAPI support in lazarus
  * 2 lazopenapi package
  * 3 The OpenAPI file generation wizard
    * 3.1 General tab
    * 3.2 Data tab
    * 3.3 Client tab
    * 3.4 Server tab
    * 3.5 GUID map tab
    * 3.6 Service name Map tab



## OpenAPI support in lazarus

FPC has [OpenAPI](<openapi.md> "openapi") support in trunk. 

Lazarus contains initial support for OpenAPI. It offers mainly the same interface as the _openapi2pas_ tool provided with Free Pascal. 

## lazopenapi package

The OpenAPI support is provided by the _lazopenapi_ package. It can be compiled with FPC 3.3.1, but also with FPC 3.2.2 

If you compile lazarus with the development version of FPC (3.3.1), then you can compile and install it in the usual manner: all the [openapi](<openapi.md> "openapi") and json-schema units are available. 

If you compile Lazarus with the release version of FPC (3.2.2) then you can still install the package: the openapi support and JSON-schema units are coded in such a way that they will compile with version 3.2.2 (in fact, they are developed using FPC 3.2.2). 

To compile the _lazopenapi"" package, you must first copy the files with JSON-Schema and OpenAPI support to the_ **ver_3_2_2** subdirectory of the packages' **lazarus/components/openapi** directory. 

The following files must be copied from FPC's main branch in gitlab: 

  1. All files under _packages/fcl-jsonschema/src', which you can find at_



    <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/packages/fcl-jsonschema/src>

  1. All files under _packages/fcl-openapi/src_ , which you can find at



    <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/packages/fcl-openapi/src>

  1. From _packages/fcl-json/src_ , you also need the _jsonwriter_ file, you can find it at



    <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/fcl-json/src/jsonwriter.pp>

Once you copied all these files to the **ver_3_2_2** subdirectory of the package, the _lazopenapi_ package can be compiled and installed in the IDE. 

## The OpenAPI file generation wizard

The package will install a menu item **OpenAPI code generator...** under the **Tools** menu. 

Clicking it will show a dialog where you can select the OpenAPI description file, and set the available options. At the bottom you can set the base filename. It serves as the base for all generated filenames. 

The wizard will always generate the data (_.Dto_) and serializer unit (_.Serializer_) 

Generate client
    Check this option to generate a client service interface and proxy implementation
Generate server
    Check this option to generate a server service implementation
Open files in IDE
    Check this to open all generated units in the IDE
Add to project
    Check this to add all generated units to the current project opened in the IDE

Note that files will be overwritten without warning. 

The code generation options are divided over various tabs, where the first one has some general options. 

### General tab

General code generation options 

[![openapi-wizard-1.png](https://wiki.freepascal.org/images/8/8e/openapi-wizard-1.png)](</File:openapi-wizard-1.png>)

  


Delphi code
    generate code for Dto/Serializer that can be compiled with Delphi.
verbose header
    When checked, adds the used command-line options to the header of the unit.
use enumerated types
    When checked, enums will be used instead of strings when the OpenAPI indicates there is an enumeration.
Unit name suffix template
    This is the template that is used to generate the unit names if they are not explicitly specified:
    the _kind' is replaced with the kind of unit (Dto, Serialize, Service.Intf etc.). The result is then appended to the base output name._
Unit name extension
    The file extension for the generated units.
Service name suffix
    The service names will have this suffix appended.
Service name prefix
    The service names will have this prefix prepended.

### Data tab

Data (Dto) code generation options 

[![openapi-wizard-2.png](https://wiki.freepascal.org/images/4/41/openapi-wizard-2.png)](</File:openapi-wizard-2.png>)

### Client tab

Client service code generation options 

[![openapi-wizard-3.png](https://wiki.freepascal.org/images/c/c6/openapi-wizard-3.png)](</File:openapi-wizard-3.png>)

Asynchrone service calls
    Generate calls that use a callback to return the result. You can use this if your HTTP requests are run in another thread, or in pas2js.
Generate \CancelRequest
    When checked, adds a _CancelRequest_ method to the interface: the calls return a unique ID that can be used to cancel the HTTP request.
Client service parent class
    The parent class for the service proxy classes: you can put the name of any descendant of _TFPOpenAPIServiceClient_

here, or any other class which implements the same methods as that class. 

Client service parent unit
    The unit in which the parent class for the service proxy class is defined.
Client service interface unit name
    the unit name for the client service interface definition: by default this is the base name appended with _.Service.Intf_.
Client service proxy implementation unit name
    the unit name for the client service proxy implementation: by default this is the base name appended with _.Service.Impl_.

  


### Server tab

Server service code generation options 

[![openapi-wizard-4.png](https://wiki.freepascal.org/images/2/22/openapi-wizard-4.png)](</File:openapi-wizard-4.png>)

Generate abstract service calls in HTTP handler module
    All service calls will be marked 'abstract' in the handler module, and moves the generation to the implementation unit
Skip implementation unit
    Check this if you are re-generating the definitions and do not wish to overwrite your implementation file.
Server service parent class
    The parent class for the service HTTP handler module classes: you can put the name of any descendant of _TFPOpenAPIModule_
    here, or any other class which implements the same methods as that class.
Server service parent unit
    The unit in which the parent class for the service HTTP handler module class is defined.
Server handler unit name
    the unit name for the service HTTP handler module: by default this is the base name appended with _.Module.Handler_.
    This unit is not generated when _Generate abstract service calls_ is checked.
Server implementation unit name
    the unit name for the server actual service implementation: by default this is the base name appended with _.Module.Impl_.
    This unit is not generated when _Generate abstract service calls_ and _Skip implementation unit_ are checked.

### GUID map tab

On this page, you can edit the UUID map contents: the map makes sure that whenever interfaces (such as the service interfaces) are regenerated, they have the same UUID. In the first column, you can set the interface name, in the second, the UUID to use. 

[![openapi-wizard-5.png](https://wiki.freepascal.org/images/6/6e/openapi-wizard-5.png)](</File:openapi-wizard-5.png>)

### Service name Map tab

On this page, you can edit the service map contents: this maps _operationId_ s or _verb:URLEndpoint_ to _servicename:methodname_ , allowing you full control over the generated service interfaces and method names. In the first column, you can set the operationId as specified in the OpenAPI file or a _verb:URLEndpoint_ pair. In the second column, you can edit the _ServiceName.Methodname_ pair. 

[![openapi-wizard-6.png](https://wiki.freepascal.org/images/9/91/openapi-wizard-6.png)](</File:openapi-wizard-6.png>)

---

_Source: [https://wiki.freepascal.org/Lazarus-openapi](https://web.archive.org/web/20260107162446/https://wiki.freepascal.org/Lazarus-openapi)_
