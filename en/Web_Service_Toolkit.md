# Web Service Toolkit

│ **English (en)** │  **[français (fr)](</Web_Service_Toolkit/fr> "Web Service Toolkit/fr")** │  **[português (pt)](</Web_Service_Toolkit/pt> "Web Service Toolkit/pt")** │  **[русский (ru)](<../ru/Web_Service_Toolkit.md> "Web Service Toolkit/ru")** │    
****

"Web Service Toolkit” is a web services package for FPC, Lazarus and Delphi; “Web Service Toolkit” is meant to ease web services **consumption and creation** by FPC, Lazarus and Delphi users. 

## Contents

  * 1 Client Side ( service consumption )
    * 1.1 Overview
    * 1.2 Example
      * 1.2.1 The “WSDL importer” wizard
      * 1.2.2 Import by ws_helper.
    * 1.3 Connection Parameters
      * 1.3.1 HTTP Proxy Parameters
      * 1.3.2 TCP connection Parameters
      * 1.3.3 LIBRARY ( LIB ) connection Parameters
      * 1.3.4 Same Process (SAME_PROCESS) connection Parameters
      * 1.3.5 Multi-Address service ( Address per operation )
    * 1.4 Troubleshooting
      * 1.4.1 'Invalid parameter : "AProtocolData"' error message
  * 2 Server Side ( service creation )
    * 2.1 Overview
    * 2.2 Example
      * 2.2.1 Defining the service Interface
        * 2.2.1.1 Export the pascal files.
      * 2.2.2 Providing an implementation for the service
      * 2.2.3 Providing a binder for the service
      * 2.2.4 Host the service into an application server
    * 2.3 WSDL generation
      * 2.3.1 The Metadata Service
      * 2.3.2 WSDL generation API
        * 2.3.2.1 WSDL Customization
      * 2.3.3 Sample
    * 2.4 Services Extensions
  * 3 Services meta data
    * 3.1 Extended Meta data
  * 4 Headers support
    * 4.1 The THeaderBlock class
    * 4.2 Defining header class
    * 4.3 The ICallContext interface
    * 4.4 Client side headers
    * 4.5 Server side headers
      * 4.5.1 The ICallControl interface
    * 4.6 Headers that does not derived from THeaderBlock
  * 5 SOAP Specific
    * 5.1 Binding style
      * 5.1.1 Client side
      * 5.1.2 Server side
    * 5.2 Encoding style
      * 5.2.1 Client side
      * 5.2.2 Server side
  * 6 Provided examples
    * 6.1 Client side examples ( tested on Windows XP, Ubuntu and MacOS on PowerPC)
    * 6.2 Server side examples
  * 7 Status
    * 7.1 Serialization
      * 7.1.1 _SOAP serializer_
      * 7.1.2 _Binary serializer_
      * 7.1.3 _Class type serialization_
        * 7.1.3.1 The root **“TBaseRemotable”** class
        * 7.1.3.2 The “TBaseComplexRemotable” serialization
        * 7.1.3.3 TBaseComplexRemotable
        * 7.1.3.4 Provided array implementations
      * 7.1.4 _Test cases_
  * 8 TODO
    * 8.1 TODO Common
    * 8.2 TODO Client-Side
    * 8.3 TODO Server-Side
  * 9 License
  * 10 Download & NewsGroup
  * 11 FAQ
  * 12 Changes Log
    * 12.1 Version 0.7
    * 12.2 Version 0.6
    * 12.3 Version 0.5
    * 12.4 Version 0.4
    * 12.5 Version 0.3.1 ( 21 August 2006 )
    * 12.6 Version 0.3 ( 5 August 2006 )
    * 12.7 Version 0.2.3.2 ( 5 July 2006 )
    * 12.8 Version 0.2.3 ( 4 July 2006 )
    * 12.9 Version 0.2.2 ( 7 June 2006 )
  * 13 Author
  * 14 See also



## Client Side ( service consumption )

Mailing list: <http://groups.google.com/group/wst-list>

Subversion checkout: svn co <https://svn.code.sf.net/p/lazarus-ccr/svn/wst/trunk>

### Overview

“Web Service Toolkit” is made of two parts : 

  * a set of programs : “typ_lib_edtr” a WSDL based type library editor, a command line tool “ws_helper” and a Lazarus integration package which contains some wizards,
  * a collection of runtime units.



Given an interface definition file(a WSDL file or a pascal file describing a web service), “ws_helper” (or within Lazarus, the WSDL file importer wizard) will create a object pascal unit containing a proxy implementing that interface. At runtime when a call targeting the web service is issued, the proxy's role is to : 

  * marshall the call parameters,
  * make the call to the target web service,
  * receive the call return and unmarshall output parameters to the caller.



Behind the scene, the proxy will take care of the SOAP plumbing details. 

### Example

We will use the “user service” sample that ships with WST. The schema that describes the service is located in the _\samples_ folder, the file is _user_service_intf.wsdl_. In order to use this service, we have to translate the service’s publicly exposed interface, which is express using the _Web Services Description Language(WSDL)_ , to Object Pascal language. We will have to compile and execute the server that provides this service. To do so, please compile the project that is located at _\samples\http_server_ , then run the resulting program to actually provide the service; Note that on recent Microsoft Windows operating systems (OS) you may have to enable the service to have network capabilities in order to run properly. To translate the service definition to Object Pascal language, within Lazarus we can use the import wizard; we can also use _**ws_helper**_ as a standalone program. 

#### The “WSDL importer” wizard

This wizard is contained in the wst_design.lpk package located in the \ide\lazarus directory of the toolkit. Once the package installed, it adds a menu section “Web Services Toolkit” with two (2) sub-menus items to the Lazarus “Project” menu : 

  * Import WSDL file ...
  * Type Library Editor ...



[![Web Services Toolkit Lazarus wizards menu.](https://wiki.freepascal.org/images/b/b8/Wst_lazarus_menu.PNG)](</File:Wst_lazarus_menu.PNG> "Web Services Toolkit Lazarus wizards menu.")

  
Click on the first menu item (Import WSDL file ...), it will bring up the dialog below. We specify the WSDL file and the directory where to store the generated files and click on OK button to finish. 

[![Web Services Toolkit Lazarus WSDL importer wizard.](https://wiki.freepascal.org/images/2/24/Wst_lazarus_importer.PNG)](</File:Wst_lazarus_importer.PNG> "Web Services Toolkit Lazarus WSDL importer wizard.")

  
It will produce two (2) pascal source files : 

  * user_service_intf.pas, the service definition file (Pascal equivalent of the WSDL file)
  * user_service_intf_proxy.pas, this file contains a proxy which implements the service interface defined in the first file.



#### Import by ws_helper.

The **ws_helper** program is a command line version of the import wizard. In order to present its capabilities, below are printed the command line arguments it supports. 
    
    
      ws_helper [-uMODE] [-gOPTION] [-p] [-b] [-i] [-w] [-x] [-y] [-d] -[fSPECIFCATIONS] [-oPATH] [-aPATH] inputFilename
        -u MODE Generate the pascal translation of the WSDL input file 
          MODE value may be U for used types or A for all types
        -g  Code generation option, with the following options : 
          A  : object arrays are generated as "array" derived from TBaseObjectArrayRemotable
          C  : object arrays are generated as "collection" derived from TObjectCollectionRemotable
          EP : enum type's items are prefixed with the enum name
          EN : enum type's items are not prefixed with the enum name, the default
        -p  Generate service proxy
        -b  Generate service binder
        -i  Generate service minimal implementation. This will erase any existing implementation file!
        -o  PATH  Relative output directory
        -a  PATH  Absolute output directory
        -w  Generate WSDL file; Can be used to get wsdl from pascal
        -x  Generate XSD file; Can be used to get xsd from pascal
        -y  Generate easy access interface for wrapped parameters
        -d  Generate documentation as comment in the interface file
        -c  Indicate the parser's case sensitivity : 
          S  : the paser is case sensitive
          I  : the paser is not case sensitive
        -f  Specify unit(s) renaming option : oldName= NewName(;oldName= NewName)* 
    
    

To translate the _user service_ WDSL file of the sample execute the following command at the prompt: 
    
    
      ws_helper.exe -uA -p -o. user_service_intf.wsdl
      ws_helper, Web Service Toolkit 0.6 Copyright (c) 2006-2014 by Inoussa OUEDRAOGO
    
      Parsing the file : user_service_intf.wsdl
      (...)
      Interface file generation...
      Proxy file generation...
      Metadata file generation...
      File "user_service_intf.wsdl" parsed successfully.
    

  
below is printed an extract of both files: 
    
    
    unit user_service_intf;
    (...)
    interface
    
    uses SysUtils, Classes, TypInfo, base_service_intf, service_intf;
    
    const
      sNAME_SPACE = 'urn:UserService';
      sUNIT_NAME = 'user_service_intf';
    
    type
    
      TUserArray = class;
      TUser = class;
      TNote = class;  
    (...)
      UserService = interface(IInvokable)
        ['{7537A085-DCD1-4B24-8B74-BC35ACB4896D}']
        function GetList():TUserArray;
        procedure Add(
          const  AUser : TUser
        );
        procedure Update(
          const  AUser : TUser
        );
        function Find(
          const  AName : string
        ):TUser;
        function Delete(
          const  AName : string
        ):boolean;
      end;
    
      procedure Register_user_service_intf_ServiceMetadata();
    
    
    
    Unit user_service_intf_proxy;
    {$IFDEF FPC} {$mode objfpc}{$H+} {$ENDIF}
    Interface
    
    Uses SysUtils, Classes, TypInfo, base_service_intf, service_intf, user_service_intf;
    
    Type
    
      TUserService_Proxy=class(TBaseProxy,UserService)
      Protected
        class function GetServiceType() : PTypeInfo;override;
        function GetList():TUserArray;
        procedure Add(
          const  AUser : TUser
        );
        procedure Update(
          const  AUser : TUser
        );
        function Find(
          const  AName : string
        ):TUser;
        function Delete(
          const  AName : string
        ):boolean;
      End;
    
      Function wst_CreateInstance_UserService(const AFormat : string = 'SOAP:'; const ATransport : string = 'HTTP:'; const AAddress : string = ''):UserService;
    
    Implementation
    uses wst_resources_imp, metadata_repository;
    
    
    Function wst_CreateInstance_UserService(const AFormat : string; const ATransport : string; const AAddress : string):UserService;
    Var
      locAdr : string;
    Begin
      locAdr := AAddress;
      if ( locAdr = '' ) then
        locAdr := GetServiceDefaultAddress(TypeInfo(UserService));
      Result := TUserService_Proxy.Create('UserService',AFormat+GetServiceDefaultFormatProperties(TypeInfo(UserService)),ATransport + 'address=' + locAdr);
    End;
    
    function TUserService_Proxy.GetList():TUserArray;
    Var
      locSerializer : IFormatterClient;
      locCallContext : ICallContext;
      locStrPrmName : string;
    Begin
      locCallContext := Self as ICallContext;
      locSerializer := GetSerializer();
      Try
        locSerializer.BeginCall('GetList', GetTarget(),locCallContext);
        locSerializer.EndCall();
    
        MakeCall();
    
        locSerializer.BeginCallRead(locCallContext);
          TObject(Result) := Nil;
          locStrPrmName := 'result';
          locSerializer.Get(TypeInfo(TUserArray), locStrPrmName, Result);
    
      Finally
        locSerializer.Clear();
      End;
    End;
    (...)
    

We are now ready to build a simple program for the service. This sample client program invokes the **GetList** method of the service in order to retrieve the list of users already registered in the service; The list is then print to the screen : 
    
    
    program user_client_console;
    uses SysUtils, fpc_http_protocol, soap_formatter,
         user_service_intf_proxy, user_service_intf;
    
    var
      locService : UserService;
      items : TUserArray;
      item : TUser;
      i : Integer;
    begin
      FPC_RegisterHTTP_Transport();
      locService := wst_CreateInstance_UserService();
      items := locService.GetList();
      try
        WriteLn('WST User Service Sample',sLineBreak);
        if (items.Length = 0) then
          WriteLn('  No user found.')
        else
          WriteLn(Format('  %d user(s) found : ',[items.Length]));
        for i := 0 to items.Length - 1 do begin
          item := items[i];
          WriteLn(Format('    Name= "%s", e-Mail= "%s"',[item.UserName,item.eMail]));
        end;
      finally
        items.Free();
      end;
    end.
    

The units _base_service_intf, service_intf, soap_formatter, fpc_http_protocol, wst_resources_imp and metadata_repository_ are provided by the toolkit; Below is the result of an execution session: 
    
    
    WST User Service Sample
    
      2 user(s) found :
        Name= "Lazarus FreePascal", e-Mail= "Lazarus@FreePascal.wst"
        Name= "Inoussa OUEDRAOGO", e-Mail= "sample@example.wst"
    

It finds two (2) users. The _wst_CreateInstance_UserService()_ function, located in the _user_service_intf.pas_ file, creates a proxy instance based on the service's informations contained in the WSDL file. The complete source code of this example is shipped with the toolkit in the _\samples\user_client_console_ directory; Note that the sample as it is provided in this directory is more completed that the code presented here. 

### Connection Parameters

The general format is: 
    
    
      protocol:paramName=paramValue(;paramName=paramValue)*
    

#### HTTP Proxy Parameters

For HTTP the supported parameters are: 

  * Address **( required for a service supporting a unique address )**
  * ProxyServer
  * ProxyPort
  * ProxyUsername
  * ProxyPassword



HTTP Connection through proxy are supported. Below is an example address string. 
    
    
      const
        sADDRESS = 'http:address=http://webservices.amazon.com/AWSECommerceService/2007-04-04'+
                     ';ProxyServer=197.150.10.10;ProxyPort=9881'+
                     ';ProxyUsername=inoussa;ProxyPassword=wst';
    

The toolkit has 3 HTTP implementations based on FPC's native implementation(fpc_http_server.pas), Synapse ( synapse_http_protocol.pas ) and Indy ( indy_http_protocol.pas ). 

#### TCP connection Parameters

TCP supported parameters are: 

  * address ( required for a service supporting a unique address )
  * Port
  * target( the target service )



Below is an example address string. 
    
    
      Const
        sADDRESS = 'TCP:Address=10.0.0.3;Port=1234;target=UserService';
    

The toolkit has 3 TCP implementations based on FPC's native implementation(fpc_tcp_server.pas), Synapse (synapse_tcp_protocol.pas) and Indy (indy_tcp_protocol.pas). 

#### LIBRARY ( LIB ) connection Parameters

The idea behind this protocol is to be able to host services in dynamic libraries ( DLL/DSO ). It can be viewed as a plug-in framework where plug-ins ( services ) are provided by dynamic libraries. 

LIB supported parameters are: 

  * FileName ( the DLL/SO filename )
  * target( the target service )



Below is an example address string. 
    
    
      Const
        sADDRESS = 'LIB:FileName=..\library_server\lib_server.dll;target=UserService';
    

The toolkit has one LIB implementation ( library_protocol.pas ). 

__**The samples folder contains 4 projects user_client_console, tcp_server, http_server and library_server which demonstrate the TCP, HTTP and LIBRARY protocols.**__

#### Same Process (SAME_PROCESS) connection Parameters

The idea behind this protocol is to be able to host services in the client process. It allows the build of a layered application within a unique program that can latter be transformed to a fully multi-layered application. It can also be used to facilitate debugging during the project development. 

SAME_PROCESS supported parameters are: 

  * Address (the target service)



Below is an example address string. 
    
    
      Const
        sADDRESS = 'SAME_PROCESS:Address=UserService';
    

The toolkit has one implementation (same_process_protocol.pas). 

#### Multi-Address service ( Address per operation )

Certain services ( like the eBay SOAP services ) use a address per operation. The toolkit uses extended meta data ( see the services meta data chapter below ) to set operation's addresses. The “eBay” SOAP sample located under the tests\ebay folder demonstrates the operation's address setting. 

### Troubleshooting

#### 'Invalid parameter : "AProtocolData"' error message

This happens when it is not clear wich protocol to use. Make sure that a valid formatter is selected (for example: 'SOAP:') and that the corresponding formatter is registered. This is done by adding the appropriate xxx_formatter unit to your uses-clausule. (for example: soap_formatter) 

## Server Side ( service creation )

### Overview

Web Service Toolkit contains a server side framework and a WSDL based type library editor for service creation. Key features are: 

  * Service definition ( interface ) is separated from implementation,
  * Interface and implementations are not bound to message protocol,
  * WSDL generation
  * Support for SOAP 1.1 serialization
  * Support for XMLRPC serialization
  * Support for custom binary serialization
  * The framework is not bound to a transport protocol
  * Easy to add support for application servers.



### Example

In order to create a service, we have to: 

  * define its interface,
  * provide an implementation and register that one for the service,
  * provide a binder that will route calls targeting the service to the implementation and register that one,
  * host the service into an application server( TCP server, HTTP Server, LIBRARY server, ... ).



#### Defining the service Interface

Starting from the 0.5 version, the WST provides a WSDL based Type library Editor to define types and services used by an implementation. The figure (3) below presents the general interface of this tool. The Type library Editor is provided as: 

  * a Lazarus wizard as a menu item “Project/Web Services Toolkit/Type Library Editor..”
  * a standalone program typ_lib_edtr.exe.



[![Web Services Toolkit Type Library editor](https://wiki.freepascal.org/images/0/0a/Wst_type_lib_edtr.PNG)](</File:Wst_type_lib_edtr.PNG> "Web Services Toolkit Type Library editor")

  
Type Library Editor features: 

  * Graphical user interface
  * WSDL source view
  * Pascal source view of the library
  * Pascal Proxy source view
  * Pascal implementation skeleton source view
  * Pascal Proxy binder view
  * enumeration creation interface
  * class creation interface
  * array creation interface
  * type alias creation interface
  * service interface creation interface.



Below some sample images are shown. We will use the user_service_intf.wsdl file located in the \samples directory for our sample. 

[![Web Services Toolkit Type library editor, class editor.](https://wiki.freepascal.org/images/b/ba/Wst_class_editor.PNG)](</File:Wst_class_editor.PNG> "Web Services Toolkit Type library editor, class editor.")

[![Web Services Toolkit Type library editor, service interface editor](https://wiki.freepascal.org/images/f/f6/Wst_interface_editor.PNG)](</File:Wst_interface_editor.PNG> "Web Services Toolkit Type library editor, service interface editor")

##### Export the pascal files.

The Type Library Editor has option to generate the pascal version of the WSDL file. To do so, click on the “Files\Save generated files ...” (it can also be done through the contextual menu ); It will bring up the dialog box shown in the following image. 

[![Web Services Toolkit Type library editor, exporting to pascal files.](https://wiki.freepascal.org/images/c/c7/Wst_type_library_export.PNG)](</File:Wst_type_library_export.PNG> "Web Services Toolkit Type library editor, exporting to pascal files.")

Click OK the button to complete. The program **ws_helper** has the same capabilities and the files can be generated with the following command : 
    
    
    ws_helper\ws_helper.exe -i -b -o. user_service_intf.wsdl
    ws_helper, Web Service Toolkit 0.5 Copyright (c) 2006, 2007 by Inoussa OUEDRAOGO
    Parsing the file : user_service_intf.wsdl
    Information : Parsing "tns:TUserArray" ...
    Information : Parsing "tns:TUser" ...
    Information : Parsing "tns:TUser" ...
    Information : Parsing "xsd:string" ...
    Information : Parsing "tns:TUser" ...
    Information : Parsing "xsd:string" ...
    Information : Parsing "xsd:boolean" ...
    Information : Parsing "TUserArray" ...
    Information : Parsing "TUser" ...
    Information : Parsing "TUserCategory" ...
    Information : Parsing "TNote" ...
    Interface file generation...
    Proxy file generation...
    Metadata file generation...
    File "user_service_intf.wsdl" parsed succesfully.
    

The complete projects of the example is located in the folder “samples”. Below is printed a extract of the generated interface file. 
    
    
    unit user_service_intf;
    {$IFDEF FPC} {$mode objfpc}{$H+} {$ENDIF}
    interface
    
    uses SysUtils, Classes, TypInfo, base_service_intf, service_intf;
    
    const
      sNAME_SPACE = 'urn:UserService';
      sUNIT_NAME = 'user_service_intf';
    
    type
    
      TUser = class;
      TUserArray = class;
    
      TUserCategory = ( 
        Normal
        ,Admin
      );
    
      TUser = class(TBaseComplexRemotable)
      published
        property Category : TUserCategory read FCategory write FCategory;
        property UserName : string read FUserName write FUserName;
        property eMail : string read FeMail write FeMail;
        property Preferences : string read FPreferences write FPreferences;
        property Note : TNote read FNote write FNote;
      end;
    
      TUserArray = class(TBaseObjectArrayRemotable)
      private
        function GetItem(AIndex: Integer): TUser;
      public
        class function GetItemClass():TBaseRemotableClass;override;
        property Item[AIndex:Integer] : TUser Read GetItem;Default;
      end;
    
      UserService = interface(IInvokable)
        ['{CA6F6192-C3DE-4D9C-B3DF-E616376A0DC9}']
        function GetList():TUserArray;
        procedure Add(
          Const AUser : TUser
        );
        procedure Update(
          Const AUser : TUser
        );
        function Find(
          Const AName : string
        ):TUser;
        function Delete(
          Const AName : string
        ):boolean;
      end;
    (...)
    

#### Providing an implementation for the service

The user_service_intf_imp.pas unit generated above contains a skeleton implementation class for the interface. It defines a procedure named RegisterUserServiceImplementationFactory. The procedure registers the class as the service implementation provider in the implementation registry. 
    
    
    Unit user_service_intf_imp;
    {$IFDEF FPC} {$mode objfpc}{$H+} {$ENDIF}
    Interface
    Uses SysUtils, Classes, 
         base_service_intf, server_service_intf, server_service_imputils,
         user_service_intf, cursor_intf;
    
    Type
      { TUserService_ServiceImp }
    
      TUserService_ServiceImp=class(TBaseServiceImplementation,UserService)
      Protected
        function GetList():TUserArray;
        procedure Add(
          Const AUser : TUser
        );
        procedure Update(
          Const AUser : TUser
        );
        function Find(
          Const AName : string
        ):TUser;
        function Delete(
          Const AName : string
        ):boolean;
      End;
    
      procedure RegisterUserServiceImplementationFactory();
    
    Implementation
    (...)
    
    procedure TUserService_ServiceImp.Add(Const AUser : TUser);
    var
      locObj : TUser;
    Begin
      locObj := Find(AUser.UserName);
      if ( locObj <> nil ) then
        raise Exception.CreateFmt('Duplicated user : "%s"',[AUser.UserName]);
      locObj := TUser.Create();
      locObj.Assign(AUser);
      FUserList.Add(locObj);
    End; 
    
    procedure RegisterUserServiceImplementationFactory();
    Begin
      GetServiceImplementationRegistry().Register(
        'UserService',
    TImplementationFactory.Create(TUserService_ServiceImp) as IServiceImplementationFactory);
    End;
    
    (...)
    

“ws_helper” has options to generate proxy file, basic implementation skeleton file and a binder file (see the following listing). 
    
    
    ws_helper, Web Service Toolkit 0.4 Copyright (c) 2006, 2007 by Inoussa OUEDRAOGO
    ws_helper [-uMODE] [-p] [-b] [-i] [-oPATH] inputFilename
      -u MODE Generate the pascal translation of the WSDL input file
           MODE value may be U for used types or A for all types
      -p  Generate service proxy
      -b  Generate service binder
      -i  Generate service minimal implementation
      -o  PATH  Relative output directory
      -a  PATH  Absolute output directory
    

The starting implementation file could also be create using **ws_helper** with the -i and -b options as above; 
    
    
    ws_helper\ws_helper.exe -i -b -o. user_service_intf.wsdl
    ws_helper, Web Service Toolkit 0.4 Copyright (c) 2006, 2007 by Inoussa OUEDRAOGO
    Parsing the file : user_service_intf.wsdl
    Proxy file generation...
    Binder file generation...
    Implementation file generation...
    Metadata file generation...
    File "user_service_intf.wsdl" parsed succesfully..
    

#### Providing a binder for the service

The binder's role is to: 

  * unpack the incoming message,
  * set up the call stack,
  * make the call against the registered implementation,
  * serialize the execution stack to create the return message.



The **user_service_intf_binder.pas** unit generated above, contains: 

  * **TUserService_ServiceBinder** : the actual binder class,
  * **TUserService_ServiceBinderFactory** a factory class for the binder and
  * **Server_service_RegisterUserServiceService** : the binder factory registration procedure.



The following code extract shows the unit interface part and a method handler of the binder. 
    
    
    unit user_service_intf_binder;
    {$IFDEF FPC} {$mode objfpc}{$H+} {$ENDIF}
    interface
    uses SysUtils, Classes, base_service_intf, server_service_intf, user_service_intf;
    
    type
      TUserService_ServiceBinder=class(TBaseServiceBinder)
      Protected
        procedure GetListHandler(AFormatter:IFormatterResponse);
        procedure AddHandler(AFormatter:IFormatterResponse);
        procedure UpdateHandler(AFormatter:IFormatterResponse);
        procedure FindHandler(AFormatter:IFormatterResponse);
        procedure DeleteHandler(AFormatter:IFormatterResponse);
      Public
        constructor Create();
      End;
    
      TUserService_ServiceBinderFactory = class(TInterfacedObject,IItemFactory)
      protected
        function CreateInstance():IInterface;
      End;
    
      procedure Server_service_RegisterUserServiceService();
    
    (...)
    
    procedure TUserService_ServiceBinder.AddHandler(AFormatter:IFormatterResponse);
    Var
      cllCntrl : ICallControl;
      tmpObj : UserService;
      callCtx : ICallContext;
      strPrmName : string;
      procName,trgName : string;
      AUser : TUser;
    Begin
      callCtx := GetCallContext();
      TObject(AUser) := Nil;
      
      strPrmName := 'AUser';  AFormatter.Get(TypeInfo(TUser),strPrmName,AUser);
      If Assigned(Pointer(AUser)) Then
        callCtx.AddObjectToFree(TObject(AUser));
      
      tmpObj := Self.GetFactory().CreateInstance() as UserService;
      if Supports(tmpObj,ICallControl,cllCntrl) then
        cllCntrl.SetCallContext(GetCallContext());
      
      tmpObj.Add(AUser);
      
      procName := AFormatter.GetCallProcedureName();
      trgName := AFormatter.GetCallTarget();
      AFormatter.Clear();
      AFormatter.BeginCallResponse(procName,trgName);
      AFormatter.EndCallResponse();
      
      callCtx := Nil;
    End;
    

#### Host the service into an application server

The application server's role is to route incoming service requests to the Web Service Toolkit runtime. For the runtime to process service requests: 

  * The services and their implementations have to be registered ,
  * The message protocol (SOAP, binary,...) have to be registered.



The runtime interface is defined in the server_service_intf unit. This unit contains : 

  * **GetServerServiceRegistry** , which returns the service registry,
  * **GetServiceImplementationRegistry** which returns the service implementation registry,
  * **GetFormatterRegistry** which returns the message format registry and
  * **HandleServiceRequest** which is the unique entry point for request processing.



Starting from the version 0.5, the toolkit provides a simplified model to develop applications server . This is achieved using the listener classes. A listener implements a transport between the server and its clients. The toolkit provides two (2) HTTP listeners implementations: 

  * TwstFPHttpListener (fpc_http_server.pas), using the FPC's native network component.
  * TwstIndyHttpListener ( indy_http_server.pas ),



Three (3) TCP listeners implementations are provided: 

  * TwstFPCTcpListener (fpc_tcp_server.pas), using the FPC's native network component.
  * TwstIndyTcpListener ( indy_tcp_server.pas ) and
  * TwstSynapseTcpListener ( synapse_tcp_server.pas ).



All listeners derive from TwstListener defined in the server_listener.pas file. 

Below is printed an HTTP server sample. The code is divided into three (3) parts : 

  * messaging format registration:


    
    
      Server_service_RegisterSoapFormat(); 
      Server_service_RegisterXmlRpcFormat();
      Server_service_RegisterBinaryFormat() ;
    

  * service implementation and binder registration:


    
    
      RegisterUserServiceImplementationFactory();
      Server_service_RegisterUserServiceService();
    

  * the listner creation and starting; the listener is created and started by the lines:


    
    
      AppObject  := TwstFPHttpListener.Create();
      AppObject.Start();
    

  
Complete listing: 
    
    
    program http_server;
    {$mode objfpc}{$H+}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes, SysUtils,
      fpc_http_server, metadata_service, logger_extension, server_listener,
      server_service_soap, server_binary_formatter, server_service_xmlrpc, config_objects,
      user_service_intf, user_service_intf_binder, user_service_intf_imp;
    var
      AppObject : TwstListener;
    begin
      Server_service_RegisterBinaryFormat();
      Server_service_RegisterSoapFormat();
      Server_service_RegisterXmlRpcFormat();
    
      RegisterUserServiceImplementationFactory();
      Server_service_RegisterUserServiceService();
      AppObject := TwstFPHttpListener.Create();
      try
        WriteLn('"Web Service Toolkit" HTTP Server sample listening at:');
        WriteLn('');
        WriteLn('http://127.0.0.1:8000/');
        WriteLn('');
        WriteLn('Press enter to quit.');
        AppObject.Start();
        ReadLn();
      finally
        FreeAndNil(AppObject);
      end;
    end.
    

**Server_service_RegisterUserServiceService** located in the **user_service_intf_binder** unit ( generated by ws_helper ) registers the UserService service by calling in turn **GetServerServiceRegistry** : 
    
    
    procedure Server_service_RegisterUserServiceService();
    Begin
      GetServerServiceRegistry().Register(
        'UserService',TUserService_ServiceBinderFactory.Create() as IitemFactory
      );
    End;
    

**Server_service_RegisterSoapFormat** located in the server_service_soap unit ( provided by the toolkit ) registers the SOAP implementation by calling in turn **GetFormatterRegistry** : 
    
    
      procedure Server_service_RegisterSoapFormat();
      begin
        GetFormatterRegistry().Register(
          sPROTOCOL_NAME,
          sSOAP_CONTENT_TYPE,
          TSimpleItemFactory.Create(TSOAPFormatter) as IitemFactory
        );
        RegisterStdTypes();
      end;
    

**Server_service_RegisterBinaryFormat** located in the server_binary_formatter unit ( provided by the toolkit ) registers the Binary message implementation by calling in turn **GetFormatterRegistry** : 
    
    
      procedure Server_service_RegisterBinaryFormat();
      begin
        GetFormatterRegistry().Register(
          sPROTOCOL_NAME,
          sBINARY_CONTENT_TYPE,
          TBinaryFormatterFactory.Create() as IitemFactory
        );
      end;
    

**Server_service_RegisterXmlRpcFormat** located in the server_service_xmlrpc unit ( provided by the toolkit ) registers the XMLRPC message implementation by calling in turn **GetFormatterRegistry** : 
    
    
      procedure Server_service_RegisterXmlRpcFormat();
      begin
        GetFormatterRegistry().Register(
          sPROTOCOL_NAME,
          sXMLRPC_CONTENT_TYPE,
          TSimpleItemFactory.Create(TXmlRpcFormatter) as IItemFactory
        );
      end;
    

In order to give it a try one have to: 

  1. compile the server ( \samples\tcp_server\tcp_server.lpi it is a console program),
  2. compile the client application ( \samples\user_client_console\user_client_console.lpi ),
  3. execute the server and start listening,
  4. execute the client.



### WSDL generation

Services in the toolkit are organized into meta data repositories. Conceptually a repository corresponds: 

  * at compile time to the pascal unit containing the service definition
  * at runtime to a name space.



The repository is the toolkit WSDL generation unit. 

#### The Metadata Service

The toolkit is provided with an easy to use metadata service implementation which in turn uses the raw interface defined in the metadata_repository unit (see above). A Lazarus GUI client application is located in the **tests\metadata_browser** folder. 

#### WSDL generation API

The **metadata_wsdl** pascal unit contains the **GenerateWSDL** function for WSDL generation from a repository (see the signature below). 
    
    
      PServiceRepository = ^TServiceRepository;
      TServiceRepository = record
        NameSpace        : ShortString;
        Name             : ShortString;
        RootAddress      : ShortString;
        ServicesCount    : Byte;
        Services         : PService;
      end;
    
      procedure GenerateWSDL(AMdtdRep : PServiceRepository; ADoc : TDOMDocument);
    

##### WSDL Customization

The WSDL generation is based on the **IWsdlTypeHandler** and the **IWsdlTypeHandlerRegistry** interfaces located in the **metadata_wsdl** unit. In order to customize the generated WSDL, one has to provide a class implementing the **IWsdlTypeHandler** interface. Then that class has to be registered in the registry. The **metadata_wsdl** unit contains implementations for pascal enumerations, **TBaseComplexRemotable** descendants, and **TBaseArrayRemotable** descendants. 

#### Sample

A functional sample project is located under **\samples\http_server**. It is an Indy base http server. 

### Services Extensions

Services extensions provide a mean to hook into all the services request processing stages. Services extensions may be used, for example, to implement authentication, request logging, data compression, etc. The **IServiceExtension** below is the interface used by the toolkit runtime to call services extensions. 
    
    
      TMessageStage = (
        msAfterDeserialize, msAfterSerialize, msBeforeDeserialize, msBeforeSerialize
      );
    
      IServiceExtension = interface
        ['{E192E6B3-7932-4D44-A8AC-135D7A0B8C93}']
        procedure ProcessMessage(
          const AMessageStage     : TMessageStage;
                   ACallContext   : ICallContext;
                   AMsgData       : IInterface
        );
      end;
    

The **AMsgData** parameter actual type depends on the message processing state and corresponds to: 

  * **IRequestBuffer** on "msBeforeDeserialize" and "msAfterSerialize"
  * **IFormatterResponse** on "msAfterDeserialize" and "msBeforeSerialize"



These types are located in the **server_service_intf** unit. Extensions have to be registered in the extensions registry ( located in the server_service_intf unit ) printed below 
    
    
      IServiceExtensionRegistry = Interface
        ['{68DC78F1-E6CF-4D6B-8473-75288794769C}']
        function Find(const AName : string):IServiceExtension;
        procedure Register(
          const AName    : string;
                AFactory : IItemFactory
        );
      end;
    

In order for an service implementation to use a service extension, it has to register himself to that extension. To that end, the IServiceImplementationFactory interface provides the RegisterExtension method. A complete sample is included in the \samples\http_server sample ( implemented in **\samples\logger_extension.pas** ). 

## Services meta data

Services in the toolkit are organized into _**meta data repositories( see the “services's meta data” below )**_. Conceptually a repository corresponds: 

  * at compile time to the pascal unit containing the service definition
  * at runtime to a name space.



The **ws_helper** tool, when parsing the interface definition file, records the meta data of the services contained in the file to a Lazarus resource file. The resource file is then embedded into the generated binder's unit file( see the unit **“initialization”** part ). At runtime the service's recorded meta data are accessible through the interface **IModuleMetadataMngr** defined in the **metadata_repository** unit ( see below ). The **GetModuleMetadataMngr** function defined in the same unit returns an instance of an object supporting that interface. 
    
    
      IModuleMetadataMngr = interface
        ['{B10ACF6A-A599-45A3-B083-BEEFB810C889}']
        function IndexOfName(const ARepName : shortstring):Integer;
        function GetCount():Integer;
        function GetRepositoryName(const AIndex : Integer):shortstring;
        procedure SetRepositoryNameSpace(const ARepName,ANameSpace : shortstring);
        function LoadRepositoryName(
          const ARepName,ARootAddress  : shortstring;
          out   ARepository  : PServiceRepository
        ):Integer;
        procedure ClearRepository(var ARepository : PServiceRepository);
        procedure SetServiceCustomData(
          const ARepName       : shortstring;
          const AServiceName   : shortstring;
          const ADataName,
                AData          : string
        );
        procedure SetOperationCustomData(
          const ARepName       : shortstring;
          const AServiceName   : shortstring;
          const AOperationName : shortstring;
          const ADataName,
                AData          : string
        );
        function GetServiceMetadata(const ARepName,AServiceName : shortstring) : PService;
        procedure ClearServiceMetadata(var AService : PService);
      end;
    

### Extended Meta data

The meta data interface provides a way to add custom data to recorded ones. Services's metadata can be set through **SetServiceCustomData** , operation's metadata be set through the **SetOperationCustomData** method. 

A repository's extended meta data has to be registered after the service meta data recorded in the resource file have been registered. So for client application the generated proxy unit contains a conditional code fragment to call a registration procedure like shown below for the eBay sample located in the **tests\ebay** folder. The procedure name is obtained from the interface unit name ( the repository's name ) : **Register_%UNIT_NAME%_ServiceMetadata** . 
    
    
      initialization
        {$i ebay.lrs}
    
        {$IF DECLARED(Register_ebay_ServiceMetadata)}
        Register_ebay_ServiceMetadata();
        {$ENDIF}
      End.
    

## Headers support

### The THeaderBlock class
    
    
      THeaderBlock = class(TBaseComplexRemotable)
      public
        property Direction : THeaderDirection read FDirection write FDirection;
        property Understood : Boolean read FUnderstood write FUnderstood;
      published
        property mustUnderstand : Integer read FmustUnderstand write SetmustUnderstand 
          stored HasmustUnderstand;
      end;
    

The **THeaderBlock** shown above ( the private part has been omitted for brevity), located in the **base_service_intf** unit, is the root class all header classes are derived from.. The **Direction** property indicate whether it is an incoming header or an outgoing one. The **mustUnderstand** property define whether the header is a mandatory one. 

### Defining header class

Soap headers are derived from the **THeaderBlock** base class located in the **base_service_intf** unit. They have to be registered in the type registry. Below is reproduced an header example extracted from the “calculator” sample project. 
    
    
      TCalcHeader = class(THeaderBlock)
      published
        property Login : string read FLogin write FLogin;
        property Password : string read FPassword write FPassword;
        property WantedPrecision : Integer read FWantedPrecision write FWantedPrecision;
      end;
    

### The ICallContext interface
    
    
      ICallContext = Interface
        ['{855EB8E2-0700-45B1-B852-2101023200E0}']
        procedure AddObjectToFree(const AObject : TObject);
        procedure Clear();
        function AddHeader(
          const AHeader        : THeaderBlock;
          const AKeepOwnership : Boolean
        ):Integer;overload;
        function AddHeader(
          const AHeader        : TBaseRemotable;
          const AKeepOwnership : Boolean;
          const AName          : string = ''
        ):Integer;overload;
        function GetHeaderCount(const ADirections : THeaderDirections):Integer;
        function GetHeader(const AIndex : Integer) : THeaderBlock;
        procedure ClearHeaders(const ADirection : THeaderDirection);
      End;
    

The **ICallContext** interface defined in the **base_service_intf** unit represents the service call context. The **AddHeader** method allows headers sending while the **GetHeader** method retrieves header in the call context. 

### Client side headers

An **ICallContext** reference may be obtained from the current service proxy instance simply by querying it for that interface as shown in the code fragment below extracted from the “calculator” client example project. 
    
    
      var
        ch : TCalcHeader;
        hdrs : ICallContext;
      begin
        FObj := TCalculator_Proxy.Create('Calculator', edtFormat.Text, edtAddress.Text);
    
        ch := TCalcHeader.Create();
        ch.mustUnderstand := 1;
        ch.Login := 'azerty';
        ch.Password := 'qwerty';
        ch.WantedPrecision := 1210;
        hdrs := FObj as ICallContext;
        hdrs.AddHeader(ch,true);
    

A header may be made mandatory by setting its **mustUnderstand** property to 1 as in the code above. 

### Server side headers

#### The ICallControl interface
    
    
      ICallControl = interface
        ['{7B4B7192-EE96-4B52-92C7-AE855FBC31E7}']
        procedure SetCallContext(ACallContext : ICallContext);
        function GetCallContext():ICallContext;
      end;
    

The **ICallControl** interface, located in the **server_service_intf** unit, is used by the toolkit runtime to share the executing call environment with service implementation classes. When the runtime is about to issue a call against a implementation class instance, it queries that instance for ICallControl interface support; If the implementation has ICallControl interface support then the obtained reference is used to set the call context through the **SetCallContext** method. The implementation instance can then access the call context by calling the **GetCallContex** method. 

The toolkit provides the **TBaseServiceImplementation** class which has support for the ICallControl interface and can be used as a base implementation class. It is the base class used by the **ws_helper** generated skeleton implementation class when invoked with the **-i** command line option. 

The method printed below, extracted from the calculator sample service demonstrates the access to headers for read and write. 
    
    
      function TCalculator_ServiceImp.AddInt(
        Const A : Integer;
        Const B : Integer
      ):TBinaryArgsResult;
      var
        hdr : TCalcResultHeader;
        h : TCalcHeader;
        cc : ICallContext;
      Begin
        hdr := TCalcResultHeader.Create();
        cc := GetCallContext();
        if Assigned(cc) and ( cc.GetHeaderCount([hdIn]) > 0 ) and ( cc.GetHeader(0).InheritsFrom(TCalcHeader) ) then begin
          h := cc.GetHeader(0) as TCalcHeader;
          h.Understood := True;
          hdr.Assign(h);
        end;
        hdr.TimeStamp := DateTimeToStr(Now());
        hdr.SessionID := 'testSession';
        cc.AddHeader(hdr,True);
        hdr := nil;
        Result := TBinaryArgsResult.Create();
        Try
          Result.Arg_OP := '+';
          Result.Arg_OpEnum := coAdd;
          Result.Arg_A := A;
          Result.Arg_B := B;
          Result.Arg_R := A + B;
          Result.Comment := 'Doing an + operation';
        Except
          FreeAndNil(Result);
          Raise;
        End;
      End;
    

  


### Headers that does not derived from THeaderBlock

Classes that inherit from _TBaseRemotable_ can also be used as headers; In that case a _THeaderBlockProxy_ ’s instance is automatically created and used as a wrapper to allow a _TBaseRemotable_ instance to be sent and received as a header block. The _ICallContext_ has an overloaded _AddHeader_ method that accepts _TBaseRemotable_ ’s instances. 

## SOAP Specific

### Binding style

The binding style is used to indicate whether the service is **RPC** oriented or **Document** oriented. 

#### Client side

The binding style may be specified in the SOAP protocol string on the creation of a service proxy. The default value for the binding style is **RPC**. Below is printed a sample code that demonstrates the use of Document style. 
    
    
      locService := TSampleService_Proxy.Create(
                      'SampleService',
                      'SOAP:Style=Document;EncodingStyle=Literal',
                      'http:Address=http://127.0.0.1/services/SampleService'
                    );
    

#### Server side

Currently services created with the toolkit use the **RPC** binding style. 

### Encoding style

The encoding style indicates the rules used to encode types in XML. Supported values are **Encoded** and **Literal**. 

#### Client side

The encoding style may be specified in the SOAP protocol string on the creation of a service proxy. The default value for the encoding style is **Encoded**. The above sample demonstrates the use of **Literal** style. 

#### Server side

Currently services created with the toolkit use the **Encoded** encoding style. 

## Provided examples

The samples are located under the **“tests”** folder. 

### Client side examples ( tested on Windows XP, Ubuntu and MacOS on PowerPC)

  * UserService, samples\http_server, samples\tcp_server, samples\user_client_console, sample\library_server : the client console uses the three client and server protocols (HTTP, TCP, LIBRARY)
  * Google sample : It demonstrates use of class and array data types .
  * Metadata Browser : This sample demonstrates use of class and array data types and mainly the toolkit metadata service.
  * eBay sample, this sample uses OpenSLL which can be found at <http://www.openssl.org> and SYNAPSE ( <http://www.ararat.cz/synapse/> ).
  * \samples\delphi\ : This folder contains Delphi ( compile with Delphi 7) client and server samples. Used protocol : the TCP, HTTP, LIBRARY; Used format : SOAP, XMLRPC and BINARY.



### Server side examples

  * samples\tcp_server : This is a sample TCP server based on the Synapse components. It uses the UserService.
  * samples\http_server : This is a sample HTTP server based on the Indy10 components. It uses the UserService and the toolkit metadata service. It demonstrates the WSDL generation.
  * samples\apache_module : Apache module sample, this sample demonstrates the hosting of the toolkit into the Apache HTTP web server. It is based on [Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat")'s Apache headers translation. It uses the UserService service and the toolkit metadata service. It demonstrates the WSDL generation.



## Status

The toolkit is usable for simple types and for class types. The serialization is designed to allow customization of basic types and class types by implementing classes derived from “TBaseRemotable”. This classes have to be registered in the type registry. 

### Serialization

The serialization is based on the IFormatterBase interface located in the **base_service_intf** unit. 

The toolkit has four serializers implementations : the SOAP serializer, the XMLRPC serializer, the JSONRPC serializer and a binary serializer. This serializers has been tested on fpc 2.x and Delphi 7. 

#### _SOAP serializer_

The SOAP serializer implements SOAP 1.1. It has support for the following pascal types: 

  * Available integers: 
    * Byte mapped to unsignedByte
    * ShortInt mapped to byte
    * SmallInt mapped to short
    * Word mapped to unsignedShort
    * LongInt mapped to int
    * LongWord mapped to unsignedInt
    * Int64 mapped to long
    * QWord mapped to int


  * String mapped to string
  * Boolean mapped to boolean
  * Enumerations mapped to their string representation


  * Float types : 
    * Single mapped to float
    * Double mapped to double
    * Extended mapped to double
    * Currency mapped to float


  * Object (class instances, not TP ones ) : The toolkit has support for instances of classes derived from TBaseRemotable. TBaseRemotable is the base class used by the formatter interface to allow customization of the serialization. The toolkit provides the TBaseComplexRemotable class which implements serialization for its ( or its descendants ) published properties.



#### _Binary serializer_

The Binary serializer is more efficient in time and space compared to the SOAP serializer . It uses big endian to stream data. It has support for the following pascal types: 

  * Available integers : 
    * Byte
    * ShortInt
    * SmallInt
    * Word
    * LongInt
    * LongWord
    * Int64
    * QWord


  * String
  * Boolean
  * Enumerations


  * Float types : 
    * Single
    * Double
    * Extended
    * Currency


  * Object (class instances, not TP ones ) :The toolkit has support for instances of classes derived from **TBaseRemotable**. TBaseRemotable is the base class used by the formatter interface to allow customization of the serialization. The toolkit provides the TBaseComplexRemotable class which implements serialization for its ( or its descendants ) published properties.



#### _Class type serialization_

The toolkit has support for instances of classes derived from **TBaseRemotable**. TBaseRemotable is the abstract base class used by the formatter interface to allow customization of the serialization. The toolkit provides the TBaseComplexRemotable class which implements serialization for its descendants classes published properties. It also provides TBaseObjectArrayRemotable class for serialization of array of TBaseRemotable descendant classes. 

##### The root **“TBaseRemotable”** class
    
    
      TBaseRemotable = class(TPersistent)
      Public
        constructor Create();virtual;
        class procedure Save(
                AObject   : TBaseRemotable;
                AStore    : IFormatterBase;
          Const AName     : String;
          Const ATypeInfo : PTypeInfo
        );virtual;abstract;
        class procedure Load(
          Var   AObject   : TObject;
                AStore    : IFormatterBase;
          var   AName     : String;
          const ATypeInfo : PTypeInfo
        );virtual;abstract;
      End;
    

**TBaseRemotable** is the abstract base class used by the formatter interface to allow customization of the serialization. This class defines a **virtual constructor** and mainly two(2) virtual abstract class methods : 

  * **Save** : this method is called when the toolkit needs to serialize the **AObject** parameter.
  * **Load** : this method is called when the toolkit needs to un-serialize to the **AObject** parameter.



##### The “TBaseComplexRemotable” serialization
    
    
      TBaseComplexRemotable = class(TAbstractComplexRemotable)
      public
        class procedure Save(
                AObject   : TBaseRemotable;
                AStore    : IFormatterBase;
          const AName     : string;
          const ATypeInfo : PTypeInfo
        );override;
        class procedure Load(
          var   AObject   : TObject;
                AStore    : IFormatterBase;
          var   AName     : string;
          const ATypeInfo : PTypeInfo
        );override;
        class procedure RegisterAttributeProperty(const AProperty : shortstring);virtual;
        class procedure RegisterAttributeProperties(const APropertList : array of shortstring);virtual;
        class function IsAttributeProperty(const AProperty : shortstring):Boolean;
        procedure Assign(Source: TPersistent); override;
      end;
    

**TBaseComplexRemotable** implements serialization for its descendants classes published properties. The serialization is based on runtime type information (RTTI) and can be customized to: 

  * ignore always some published properties.
  * ignore conditionally some published properties.



The following class shows a the serialization's customization sample. 
    
    
      TSampleClass = class(TBaseComplexRemotable)
      private
        FProp_Always: Integer;
        FProp_Never: Integer;
        FProp_Optional: Integer;
        function GetStoredProp_Optional: boolean;
      published
        //This property will always be serialized
        property Prop_Always : Integer read FProp_Always write FProp_Always;
        //This property will never be serialized
        property Prop_Never : Integer read FProp_Never write FProp_Never stored False;
        //This property will be serialized if "Self.GetStoredProp_Optional() = True"
        property Prop_Optional : Integer read FProp_Optional write FProp_Optional stored GetStoredProp_Optional;
      End;
    

  * Attribute properties



TBaseComplexRemotable allows properties serialization as attributes. Theses properties have to be registered as such with the RegisterAttributeProperty class method or RegisterAttributeProperties one. 

  


##### TBaseComplexRemotable

**TBaseComplexSimpleContentRemotable** provides implementation for the “XML Schema” complex types which extend simple types with attributes. The following example illustrates this : 
    
    
      <xs:complexType name="DecimalWithUnits">
        <xs:simpleContent>
          <xs:extension base="xs:decimal">
            <xs:attribute name="Units" type="xs:string"
                          use="required"/>
          </xs:extension>
        </xs:simpleContent>
      </xs:complexType>
    

This type will be translate by ws_helper to Pascal as: 
    
    
      DecimalWithUnits = class(TComplexFloatExtendedContentRemotable)
      private
        FUnits : string;
      published
        property Units : string read FUnits write FUnits;
      end;
    

using the predefined types ( in base_service_intf.pas ) 
    
    
      TBaseComplexSimpleContentRemotable =
         class(TAbstractComplexRemotable)
      protected
        class procedure SaveValue(
          AObject : TBaseRemotable; 
          AStore : IformatterBase
        );virtual;abstract;
        class procedure LoadValue(
          var AObject : TObject; 
          AStore : IformatterBase
        );virtual;abstract;
      public
        class procedure Save(
                AObject   : TBaseRemotable;
                AStore    : IFormatterBase;
          const AName     : string;
          const ATypeInfo : PTypeInfo
        );override;
        class procedure Load(
          var   AObject   : TObject;
                AStore    : IFormatterBase;
          var   AName     : string;
          const ATypeInfo : PTypeInfo
        );override;
      end;
    
      TComplexFloatExtendedContentRemotable =
         class(TBaseComplexSimpleContentRemotable)
      private
        FValue: Extended;
      protected
        class procedure SaveValue(
          AObject : TBaseRemotable; 
          AStore : IformatterBase
        );override;
        class procedure LoadValue(
           var AObject : TObject; 
           AStore : IformatterBase
        );override;
      public
        property Value : Extended read FValue write FValue;
      end;
    

An instance of this type looks like the one below. Every attribute must be registered using the **RegisterAttributeProperty()** method. The toolkit provides class for Pascal basic types( TComplexInt8UContentRemotable, TComplexInt8SContentRemotable, TComplexInt16SContentRemotable, ...). 
    
    
      <example Units = "meter">
        12.10
      </example>
    

##### Provided array implementations

The toolkit provides array implementation for basic types ( in the **base_service_intf** unit ) listed below. The implementations are based on the serialization's customization. 

  * Available integers : 
    * Byte **TArrayOfInt8URemotable**
    * ShortInt **TArrayOfInt8SRemotable**
    * SmallInt **TArrayOfInt16SRemotable**
    * Word **TArrayOfInt16URemotable**
    * LongInt **TArrayOfInt32SRemotable**
    * LongWord **TArrayOfInt32URemotable**
    * Int64 **TArrayOfInt64SRemotable**
    * Qword **TArrayOfInt64URemotable**
  * String **TarrayOfStringRemotable**( AnsiString )
  * Boolean **TArrayOfBooleanRemotable**
  * Float types : 
    * Single **TArrayOfFloatSingleRemotable**
    * Double **TArrayOfFloatDoubleRemotable**
    * Extended **TArrayOfFloatExtendedRemotable**
    * Currency **TArrayOfFloatCurrencyRemotable**



The toolkit array's implementation support “embedded” array serialization. This type of array occurs typically with types like the following one ( the "ResponseGroup" may be repeated ): 
    
    
      <xs:complexType name="CustomerContentSearchRequest">
        <xs:sequence>
          <xs:element name="CustomerPage" type="xs:positiveInteger"
             minOccurs="0"/>
          <xs:element name="Email" type="xs:string" minOccurs="0"/>
          <xs:element name="Name" type="xs:string" minOccurs="0"/>
          <xs:element name="ResponseGroup" type="xs:string"
               minOccurs="0" maxOccurs="unbounded"/>
        </xs:sequence>
      </xs:complexType>
    

which could be instantiated as 
    
    
      <search>
        <CustomerPage> 1 </CustomerPage>
        <Name>Sample name</Name>
        <ResponseGroup>Group 1</ResponseGroup>
        <ResponseGroup>Group 2</ResponseGroup>
        <ResponseGroup>Group 3</ResponseGroup>
      </search>
    

This type will be translate to Pascal by ws_helper as (the private and protected parts are omitted to be short) 
    
    
    (...)
      CustomerContentSearchRequest_ResponseGroupArray =
        class(TBaseSimpleTypeArrayRemotable)
      public
        class function GetItemTypeInfo():PTypeInfo;override;
        procedure SetLength(const ANewSize : Integer);override;
        property Item[AIndex:Integer] : string 
          read GetItem write SetItem; default;
      end;
    
      CustomerContentSearchRequest = class(TBaseComplexRemotable)
      published
        property CustomerPage : positiveInteger 
           read FCustomerPage 
           write FCustomerPage stored HasCustomerPage;
        property Email : string 
           read FEmail 
           write FEmail 
           stored HasEmail;
        property Name : string read FName write FName stored HasName;
        property ResponseGroup :
           CustomerContentSearchRequest_ResponseGroupArray
           read FResponseGroup 
           write FResponseGroup;
      end;
    
    implementation
    (...)
      GetTypeRegistry().ItemByTypeInfo[
        TypeInfo(CustomerContentSearchRequest_ResponseGroupArray)]
        .RegisterExternalPropertyName(sARRAY_STYLE,sEmbedded);
    (...)
    

The last instruction set the array style to “Embedded” and so the SOAP formatter will serialize the array accordingly. 

#### _Test cases_

  * The toolkit uses FPCUnit for test cases. The test project is located in the \tests\test_suite folder.
  * The Delphi tests suite is based on Dunit and is located in the \tests\test_suite\delphi folder.



## TODO

### TODO Common

  * Simple type support in headers
  * Header support for the binary format
  * True Attribute serialization support for the binary format



### TODO Client-Side

  * ~~Basic array support~~ for SOAP
  * ~~Basic array support~~ for Binary format
  * ~~XML-RPC formatter~~
  * More documentation and samples ! 
    * ~~eBay basic client~~ : demonstrates the **GetPopularKeywords** operation call
    * ~~"Amazon E-Commerce Service" sample~~
  * ~~WSDL to pascal compiler~~
  * ~~Enhance the pascal parser~~ : the toolkit now uses the fcl-passrc
  * ~~Client side services extensions~~ (See Data Filters)
  * import functions from XMLRPC server via introspect <http://scripts.incutio.com/xmlrpc/introspection.html>



### TODO Server-Side

Extend the toolkit to Server side for : 

  * ~~SOAP~~
  * ~~Binary serialization~~
  * ~~Bytes ordering in binary serialization : alaways use big-endian~~
  * ~~XML-RPC~~
  * ~~TCP transport~~ ( first implementation).
  * ~~WSDL generation~~
  * More documentation and samples !
  * ~~Apache support~~ : services as Apache modules using [Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat") 's [ Apache headers translation ](<http://www.freepascal.org/wiki/index.php/FPC_and_Apache_Modules> "fpc:FPC and Apache Modules")
    * See the **apache_module** sample located in the _**tests\apache_module**_ folder
  * ~~Classes inheritance generation in WSDL~~



## License

All the code in WST is under the FPC' RTL license (modified LGPL). 

## Download & NewsGroup

  * The lastest version can be found at <http://inoussa12.googlepages.com/webservicetoolkitforfpc%26lazarus> and from [Lazarus-CCR sourceforge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=196167>).
  * The discussion list is at <http://groups.google.com/group/wst-list>



## FAQ

The [Web Service Toolkit:FAQ](<Web_Service_Toolkit_FAQ.md> "Web Service Toolkit:FAQ"). 

## Changes Log

### Version 0.7

  * FPC 3.1.1+ support
  * Many fixes and enhancements (Please see the commit log).



### Version 0.6

  * License change : 
    * All the code in WST is now using the FPC' RTL license (modified LGPL see COPYING.modifiedLGPL)


  * WSDL/XSD Parsers : 
    * Referenced external schemes parsing : <import> and <include> handling
    * Pascal Unit renaming
    * Handle top level(global) declared attribute and references
    * More XSD constructs parsing
    * Parser case sensitivity can be enabled or disabled
    * Document wrapped parameters handling : generation of an easy access interface


  * Type Library Editor 
    * Beautification : +images for menu items, +Tool bar
    * Support for External XSD Schema referencing
    * Documentation for objects
    * Show object dependency
    * Show object XSD schema
    * Collection based arrays support
    * Items cloning
    * Better WSDL generation


  * Run Time : 
    * FPC's Native HTTP Client transport
    * FPC's Native TCP Client transport
    * FPC's Native HTTP Server
    * FPC's Native TCP Server
    * iOS http transport
    * Client Data Filter (interceptors used to manipulate the payload : encrypt, compress, ...)
    * Client side HTTP Cookie management
    * Better SOAP headers support
    * Better WSDL generation


  * Other : 
    * Documentation updated : the sample used for the tutorial is now one of the WST samples
    * More test cases
    * Bug Fixes



### Version 0.5

  * Lazarus IDE WSDL importer wizard
  * WSDL GUI editor ( Type library editor )
  * Listener design : easy server development 
    * Indy HTTP server listener
    * Indy TCP server listener
    * Synapse TCP server listener
  * XMLRPC support : client and server
  * Switch from the custom pascal parser to fcl-passrc
  * Server side : implementation objects pooling
  * Better Delphi support 
    * SOAP XMLRPC and binary format support
    * DUnit test suite
  * WST has been tested on 
    * Windows
    * Linux
    * macOS PowerPC
  * Better WSDL parsing
  * Services configuration in external file
  * TStringBufferRemotable ( reading a node's raw buffer )
  * Bugs fixes



### Version 0.4

  * WSDL to Pascal translation support in "ws_helper"
  * new TCP transport implementation ( using synapse library ) in synapse_tcp_protocol.pas
  * new library protocol implementation in library_protocol.pas
  * TCP server implementation ( using synapse library ) in synapse_tcp_server.pas
  * Delphi : first binary format support
  * Embedded array support
  * Object Pascal reserved words can now be used as service methods's name, enumeration, ... ( see TTypeRegistryItem.RegisterExternalPropertyName() )
  * The toolkit can now be used with FPC without Lazarus
  * "Amazon E-Commerce Service" sample
  * Bugs Fixes.



### Version 0.3.1 ( 21 August 2006 )

  * Apache Web Server support : services hosting as Apache's module. 
    * See the **apache_module** sample located in the _**tests\apache_module**_ folder.
  * Important : In the connection string the address token was mis-spelled as **adress** (one letter "d" instead of two), it has been corrected to **address**.
  * Bugs Fixes.



### Version 0.3 ( 5 August 2006 )

  * Header support ( client and server side )
  * Server side service extensions
  * Attribute serialization
  * New transport implementation : Synapse HTTP client support
  * Address per operation support ( client side )
  * Extended meta data
  * Bug Fixes



### Version 0.2.3.2 ( 5 July 2006 )

  * ICS and Indy HTTP Proxy "user" and "password" support.



### Version 0.2.3 ( 4 July 2006 )

  * WSDL generation
  * Metadata service
  * Metadata Browser sample
  * HTTP server sample
  * Pascal basic types array implementation
  * The ws_helper's parser now supports: 
    * {} comment style in the input file
    * service interfaces may have GUID
  * more test cases
  * bug fix



### Version 0.2.2 ( 7 June 2006 )

  * All pascal basic types are supported by the **SOAP serializer** and the **Binary serializer** ( Available integers, available floats, string, boolean, Enumerations, class intances )
  * Array support for Binary serializer
  * FPCUnit test cases 
    * SOAP serializer ( basic types and classes instances )
    * Binary serializer ( basic types and classes instances )
  * All interfaces now have GUID



## Author

Inoussa OUEDRAOGO, <http://inoussa12.googlepages.com/>

## See also

  * [fcl-web](<fcl-web.md> "fcl-web") FPC/Lazarus web server components
  * [[1]](<http://www.freepascal.org/~michael/articles/fpcrpc/fpcrpc.pdf>) Article on WST. Though written a while ago, it may still be useful

---

_Source: [https://wiki.freepascal.org/Web_Service_Toolkit](https://web.archive.org/web/20230105082710/https://wiki.freepascal.org/Web_Service_Toolkit)_
