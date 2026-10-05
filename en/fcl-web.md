# fcl-web

│ **English (en)** │  **[русский (ru)](<../ru/fcl-web.md>)** │

## Contents

  * 1 What is fpWeb
  * 2 Using fpWeb together with Lazarus
    * 2.1 Installing the weblaz fpWeb Lazarus Package
    * 2.2 Creating a CGI application
    * 2.3 Deploying the CGI application
    * 2.4 Formatting the HTML and Reading Query fields
    * 2.5 Reading GET data
    * 2.6 Reading POST data
    * 2.7 Reading POST binary data
  * 3 Specialized modules
  * 4 Using multiple modules
  * 5 Using Actions
  * 6 Using HTML Templates
  * 7 Tips & troubleshooting
    * 7.1 Sending result codes
    * 7.2 Sending binary data
    * 7.3 Error: Could not determine HTTP module for request
    * 7.4 Error: response code 500 Internal Server error when trying to handle DELETE method
    * 7.5 fpWeb/FCGI and Apache 2.4 (mod_proxy_fcgi mode)
    * 7.6 fpWeb/FCGI and nginx
  * 8 Notes
  * 9 See also



## What is fpWeb

fpWeb can be used to build web server applications. A web server application can be one of the following 

  * CGI application
  * FastCGI application
  * Apache module
  * A standalone HTTP(s) server using Free Pascal HTTP server components.
  * A standalone HTTP(s) server using GNU libmicrohttp
  * A windows SysHTTP application.



In all cases, the development model is the same: you register handlers for a set of URLS (called routes) that your application will support. 

Then you embed these routes in one of the above environments: This means you choose the application object that you will be using. 

The lazarus IDE you have the choice of applications when you start a new project. It's possible to change the application type once the application was started, this usually just means changing the name of a unit in the project's uses clause. 

It is possible to support multiple environments within a single binary, although this takes some extra work. 

fpWeb also offers some out-of-the box support for a database REST server or for JSON RPC 2.0 mechanisms. These are built on top of the above structure. The Brook framework is also built on top of the basic architecture outlined above. 

More about this can be found in the [fpWeb Tutorial](<fpWeb_Tutorial.md> "fpWeb Tutorial")

The [simpleserver](<simpleserver.md> "simpleserver") example is a small stand alone server application that showcases many of the possibilities of fcl-web. 

## Using fpWeb together with Lazarus

### Installing the weblaz fpWeb Lazarus Package

The first step to do is installing the package which comes in the path lazarus/components/fpweb/weblaz.lpk. As usual with design-time packages, you'll have to rebuild Lazarus. 

### Creating a CGI application

After the weblaz package is installed, a very simple CGI web application which displays an HTML page can be created by going to the Lazarus menu "File->New...". From the list of possible applications select "CGI Application" as in the image below, which will create a main CGI project file and a fpweb web module. 

[![New cgi.PNG](https://wiki.freepascal.org/images/e/ec/New_cgi.PNG)](</File:New_cgi.PNG>)

The TFPWebModule allows you to manipulate properties and events using the Object Inspector. 

To add code to show the page, a request handler should be added. To do this, double click the OnRequest property in the object inspector, as in the image below: 

[![Webmodule.PNG](https://wiki.freepascal.org/images/3/3d/Webmodule.PNG)](</File:Webmodule.PNG>)

In the event handler one should write the HTML code which will be displayed by the browser. To avoid mixing Pascal and HTML, this page can be loaded from the directory the CGI executable is in by using AResponse.Contents.LoadFromFile(). 

The type of the response should be set in AResponse.ContentType. For HTML pages this should have the value 'text/html;charset=utf-8'. 

Finally, Handled should be set to True to indicate that the request was successfully handled (and return a 200 OK status code to the browser). After adding this code, the web module should look like this: 
    
    
    unit mainpage;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, HTTPDefs, websession, fpHTTP, fpWeb; 
    
    type
    
      { TFPWebModule1 }
    
      TFPWebModule1 = class(TFPWebModule)
        procedure DataModuleRequest(Sender: TObject; ARequest: TRequest;
          AResponse: TResponse; var Handled: Boolean);
      private
        { private declarations }
      public
        { public declarations }
      end; 
    
    var
      FPWebModule1: TFPWebModule1; 
    
    implementation
    
    {$R *.lfm}
    
    { TFPWebModule1 }
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    begin
      AResponse.ContentType := 'text/html;charset=utf-8';
      AResponse.Contents.LoadFromFile(ExtractFilePath(ParamStr(0)) + 'mainpage.html');
      Handled := True;
    end;
    
    begin
      RegisterHTTPModule('TFPWebModule1', TFPWebModule1);
    end.
    

### Deploying the CGI application

This section assumes the Apache web server is used. Of course, other web servers that support CGI (nginx, cherokee) can be used, too. 

Apache can be downloaded here: <http://httpd.apache.org/download.cgi> or installed using your distribution's package manager. 

The default installation of Apache will treat all files located in its cgi-bin directory as CGI programs, so the user won't be able to access plain HTML files placed there. This directory can be set in the file httpd.conf in the following section: 
    
    
       #
       # ScriptAlias: This controls which directories contain server scripts.
       # ScriptAliases are essentially the same as Aliases, except that
       # documents in the target directory are treated as applications and
       # run by the server when requested rather than as documents sent to the
       # client.  The same rules about trailing "/" apply to ScriptAlias
       # directives as to Alias.
       #
       ScriptAlias /cgi-bin/ "C:/Program Files/Apache Software Foundation/Apache2.2/cgi-bin/"
    

If you place an executable called "mywebpage.cgi" in this directory, then the page can be accessed as <http://localhost/cgi-bin/mywebpage.cgi> (or remotely with the corresponding IP address or domain name). 

fcl-web with Lazarus on Windows produces .exe files. For Apache to serve these files you have to add this: 
    
    
        AddHandler cgi-script .exe
    

And to serve your executables from another directory, you add this: 
    
    
       ScriptAlias /bin/ "C:/lazarus_projects/test-fclweb/bin/"
       <Directory "C:/lazarus_projects/test-fclweb/bin/">
           AllowOverride None
           Options None
           Order allow,deny
           Allow from all
       </Directory>
    

### Formatting the HTML and Reading Query fields

The previous example just showed a plain HTML page. One might wish to e.g. 

  * dynamically change the HTML page output, and
  * as read the variables the browser passed to the webpage in Query fields (in e.g. HTTP GET and HTTP POST actions).



A simple solution for the first problem is simply using the standard Pascal routine Format and adding %s or %d in the HTML file in the appropriate places which will receive a custom value. 

### Reading GET data

To read the GET variables one can use ARequest.QueryFields, which is a TStrings descendent. Each variable will be in a separate line in the TStrings in the format variablename=value, similarly to how they are shown in the browser page address. To search for a specific variable one can use ARequest.QueryFields.Values[], passing the variable name in the brackets to receive its value back, instead of parsing the string manually. 

The resulting code: 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    var
      HexText, AsciiText: string;
    begin
      HexText := ARequest.QueryFields.Values['hex'];
      AsciiText := HexToAnsii(HexText);
    
      AResponse.ContentType := 'text/html;charset=utf-8';
      AResponse.Contents.LoadFromFile(ExtractFilePath(ParamStr(0)) + 'mainpage.html');
      AResponse.Contents.Text := Format(AResponse.Contents.Text,
        [HexText, AsciiText]);
      Handled := True;
    end;
    

### Reading POST data

Data submitted by POST requests can be obtained from `TRequest.Content`. It will come without the request headers - in other words it contains the body of the request. 

Submitted form data can also be accessed using `TRequest.ContentFields` which is the content parsed as fields separated by & and decoded. For example, the following set of form values: 
    
    
    login: dfg 345&&
    login_senha: ====
    email: dfg
    

Will be encoded in 'APPLICATION/X-WWW-FORM-URLENCODED' like this (see [wikipedia POST (HTTP)](<http://en.wikipedia.org/wiki/POST_%28HTTP%29>)): 
    
    
    login=dfg+345%26%26&login_senha=%3D%3D%3D%3D&email=dfg
    

And will be available in TRequest.ContentFields via the line index or using the Values property, which is more convenient: 
    
    
    TRequest.ContentFields[0]: login=dfg 345&&
    TRequest.ContentFields[1]: login_senha=====
    TRequest.ContentFields[2]: email=dfg
    TRequest.ContentFields.Values['email']: dfg
    

If using other mime-types than 'MULTIPART/FORM-DATA' and 'APPLICATION/X-WWW-FORM-URLENCODED' (the types supported by HTML forms): 

  * content is only available in TRequest.Content, not TRequest.ContentFields
  * use of these mime-types raises an exception for FPC 2.4.2 (fixed in FPC 2.5+).



Note that HTTP POST can also send Query fields (in the URL), e.g. <http://server/bla?question=how>, and those are accessed by TRequest.QueryFields as explained in the previous section. 
    
    
    procedure TFPWebModule1.DataModuleRequest(Sender: TObject; ARequest: TRequest;
      AResponse: TResponse; var Handled: Boolean);
    var
      lData: String;
    begin
      lData := ARequest.Content;
    

### Reading POST binary data

Uploaded files are saved to a temporary location with a temporary filename, to retain the file you should move it to a permanent location. 

To receive data on the server that has been POSTed e.g. as multipart/form-data, use something like this: 
    
    
    procedure TMainWebModule.TFPWebActions2Request(Sender: TObject;
      ARequest: TRequest; AResponse: TResponse; var Handled: Boolean);
    var
      i: Integer;
    begin  
      // Process all received files
      for i := 0 to ARequest.Files.Count - 1 do
      begin
        // Doing something else than writeln is highly recommended ;)
        writeln ('Received Filename: '+ARequest.Files[i].LocalFileName);
      end;
          
      Handled := true;
    end;
    

The client can send data using e.g. FileFormPost. 

## Specialized modules

The *TFPWebModule* class (used below) is a simple example of a WEb module that can be used for all kinds of HTTP requests. 

However, fpweb comes with some specialized modules, that have extra support for specialized tasks: 

  * The **TSimpleFileModule** class in unit [fpwebfile](<fpwebfile.md> "fpwebfile") can be used to send files. You point it to a directory, and it does the rest.
  * The **TFPHTMLModule** class in unit _fphtml.pp_ can be used to produce HTML.
  * The **TProxyWebModule** class in unit _fpwebproxy.pp_ is a ready-made forwarding proxy.
  * The **TFPWebProviderDataModule** class in unit _fpwebdata.pp_ serves data in JSON format that can be consumed by ExtJS stores.
  * The **TSQLDBRestModule** class in unit _sqldbrestmodule.pp_ implements a full REST server backed by SQLDB. See more info in [SQLDBRestBridge](<SQLDBRestBridge.md> "SQLDBRestBridge").
  * The **TJSONRPCModule** class in unit _webjsonrpc.pp_ implements a JSON-RPC service.
  * The **TExtDirectModule** class in unit _fpextdirect.pp_ implements a Ext.Direct variant of a JSON-RPC service.



## Using multiple modules

If there is only one module in the web application, all requests will be directed to this module. 

As your web application grows, multiple modules can be used. A new module can be added by choosing 'File - New' and then one of 'Web module' or 'HTML Web Module'. 

FCL-web uses the URL to determine how a HTTP request should be handled. It must therefore know which web-modules exist in the application. To achieve this, each module must be registered. 

Each module is registered with fcl-web in the initialization section of the unit it is defined in: 
    
    
    RegisterHTTPModule('location', TMyModule);
    

The module will then be invoked if an URL of the form 
    
    
    http://www.mysite.org/mycgi.cgi/location
    

or 
    
    
    http://www.mysite.org/mycgi.cgi?module=location
    

is used. 

If multiple modules are present, the name of the module must appear in the URL, or an error will be raised. 

This behaviour can also be forced for applications that have only a single module by setting the Application's property AllowDefaultModule to false: 
    
    
    Application.AllowDefaultModule := False;
    

In that case, the fcl-web application will always require the name of the module in the URL. 

The name of the request variable that determines the module name (by default, this is 'module') can be set in the Application.ModuleVariable property. The following code 
    
    
    Application.ModuleVariable := 'm';
    

ensures that the following URL is directed to TMyModule: 
    
    
    http://www.mysite.org/mycgi.cgi?m=location
    

If all this is not enough to determine the module to which the request should be passed, the Application.OnGetModule event can be used. It is of type TGetModuleEvent: 
    
    
    type
      TGetModuleEvent = Procedure (Sender : TObject; ARequest : TRequest;
                                   Var ModuleClass : TCustomHTTPModuleClass) of object;
    

Creating an event handler for this event allows fine control over the module that is created to handle the request: the request (passed in ARequest) can be examined, and the 'ModuleClass' variable must be set to the class of the module that should handle the request. 

If 'ModuleClass' is 'Nil' on return, an error will be sent to the browser. 

## Using Actions

A module can be used to group certain kinds of actions that logically belong together. Imagine a module TUserModule that is used to handle user management in the webpages. There can be multiple actions associated with a user: 

  * Creating
  * Deleting
  * Editing
  * Displaying



These different actions can be handled by the same module. One can determine the action from the URL manually, as in: 
    
    
    http://mysite/mycgi.cgi/user?action=delete&id=12
    

This can be automated. 

In order to make it easier to distinguish between various actions, the module has a property actions: this is a collection, in which each item is associated with a different response to the request. The actions have various properties: 

Name
    The name of the action. The URL will be examined to determine the name of the action.
Content
    A string. If set, this is sent to the browser.
Contents
    A stringlist. If set, this is sent to the browser.
ContentProducer
    If set, the contentproducer will handle the request.
Default
    if set to 'True', then this action is the default action. That means that if FCL-Web cannot determine the action name, this action will be executed.
Template
    If set, this template will be processed, and the results sent to the browser.

There are also some events: 

BeforeRequest
    executed before the request is processed. Can be used to set the 'Content' or other properties.
AfterRequest
    executed after the request is processed.
OnRequest
    an event handler to handle the request. If set, the handler is used to handle the request.

Again, as in the case of multiple modules, the URL is used to determine which action to execute. The part right after the module part ("user" in this example) is used: 
    
    
    http://mysite/mycgi.cgi/user/delete&id=12
    

would execute the action named 'delete'. 

The 'ActionVar' property of the module can be used to set the name of the request variable to use. Setting 
    
    
    UserModule.ActionVar := 'a';
    

may be used to change the above URL to 
    
    
    http://mysite/mycgi.cgi/user?a=delete&id=12
    

If there is only one module in the application, the URL can be shortened to 
    
    
    http://mysite/mycgi.cgi/delete&id=12
    

## Using HTML Templates

For information about using templates, template tags and template tag parameters to generate response pages, please refer to the fptemplate.txt file under your FPC directory in /packages/fcl-base/texts/. 

Example projects that demonstrate using templates can be found under your FPC directory in /packages/fcl-web/examples/fptemplate/ (see the README.txt in there for more). (In earlier versions, these examples were in the Lazarus directory in /components/fpweb/demo/fptemplate/) 

## Tips & troubleshooting

### Sending result codes

To send a different HTTP response than 200 OK, use **AResponse.Code** and **AResponse.CodeText** , e.g. 
    
    
    AResponse.Code     := 404;
    AResponse.CodeText := 'Document not found';
    

As of FPC 3.3.1 you can use the **SetStatus** method: 
    
    
    AResponse.SetStatus(404);
    

it will use the _GetHTTPStatusText_ routine of the _httpprotocol_ unit to look up standard status texts. 

You can also tell **SetStatus** to send the response at once: 
    
    
    AResponse.SetStatus(404,true);
    

This will set the status and call _SendResponse_ to send the response. Any headers or content you have set will be sent as well. 

### Sending binary data

An approach that seems to work to send e.g. a tiff file from the web server to the client - adapted from $(fpcdirectory)\packages\fcl-web\examples\jsonrpc\demo1\wmdemo.pp - something like: 
    
    
    AResponse.ContentStream := TMemoryStream.Create;
    
    try
      AResponse.ContentStream.LoadFromFile('/tmp/sample.tiff');
      AResponse.ContentType := 'image/tiff'; //or whatever MIME type you want to send
    // to do: there is an fpweb example that gets the mime type from the file extension...
      AResponse.ContentLength:=AResponse.ContentStream.Size; //apparently doesn't happen automatically?
      AResponse.SendContent;
    finally
      AResponse.ContentStream.Free;
    end;
    
    Handled := true;
    

### Error: Could not determine HTTP module for request

You may have multiple modules and multiple actions. If you specify an URL with only 1 item, like: 
    
    
    http://localhost/cgi-bin/somemodule
    

then fpweb assumes you're specifying an action. If you don't have a default module set, you will get a 500 internal server error (Could not determine HTTP module for request) 

You can modify this behaviour to let fpweb map to a module name instead by setting the application's **PreferModuleName** property to true. 

### Error: response code 500 Internal Server error when trying to handle DELETE method

In FPC 2.6.2 and lower, fcl-web does not accept the DELETE method and generates an error. 

### fpWeb/FCGI and Apache 2.4 (mod_proxy_fcgi mode)

When you want to deploy FCGI application behind Apache 2.4+ reverse proxy, you need PATH_INFO variable in headers that are sent via socket to FCGI daemon. 

1, Enable following modules: setenvif_module, proxy_module and proxy_fcgi_module   
(on CentOS 7 modules are defined in file /etc/httpd/conf.modules.d/00-base.conf and /etc/httpd/conf.modules.d/00-proxy.conf). 
    
    
    LoadModule setenvif_module   modules/mod_setenvif.so
    LoadModule proxy_module      modules/mod_proxy.so
    LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
    

2, Reverse proxy configuration for fpWeb/FCGI listening on port 9000.  
$1 = module name  
$2 = action name 
    
    
    SetEnvIf Request_URI . proxy-fcgi-pathinfo=full
    ProxyPassMatch "/fcgi/(.*)/(.*)" "fcgi://127.0.0.1:9000/$1/$2"
    

### fpWeb/FCGI and nginx

For correct routing, fcl-web requires the PATH_INFO variable in headers sent from nginx. For this you have to split the whole URL into FastCGI application name and the path info part whith the configuration statement _fastcgi_split_path_info_. The statement accepts a regular expression and puts the second match into _$fastcgi_path_info_ configuration variable. This variable can be passed to your application with _fastcgi_param_ statement. 

The following configuration example includes a full virtual host for nginx which passes everything from _<http://myserver:8080/mycgiapp/>_ to an external FastCGI application. 
    
    
    server {
    
        listen      8080;
        listen      [::]:8080;
    
        server_name _;
    
        root    "/var/www";
    
        location /mycgiap/ {
            # setting up PATH_INFO
            fastcgi_split_path_info ^(/mycgiapp/)(.*)$;
            fastcgi_param  PATH_INFO          $fastcgi_path_info;
    
            # other parameters
            fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
            fastcgi_param  QUERY_STRING       $query_string;
            fastcgi_param  REQUEST_METHOD     $request_method;
            fastcgi_param  CONTENT_TYPE       $content_type;
            fastcgi_param  CONTENT_LENGTH     $content_length;
            fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
            fastcgi_param  REQUEST_URI        $request_uri;
            fastcgi_param  DOCUMENT_URI       $document_uri;
            fastcgi_param  DOCUMENT_ROOT      $document_root;
            fastcgi_param  SERVER_PROTOCOL    $server_protocol;
            fastcgi_param  REQUEST_SCHEME     $scheme;
            fastcgi_param  HTTPS              $https if_not_empty;
            fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
            fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;
            fastcgi_param  REMOTE_ADDR        $remote_addr;
            fastcgi_param  REMOTE_PORT        $remote_port;
            fastcgi_param  SERVER_ADDR        $server_addr;
            fastcgi_param  SERVER_PORT        $server_port;
            fastcgi_param  SERVER_NAME        $server_name;
    
            # pass requests to your FastCGI application
            fastcgi_pass 127.0.0.1:9000;
        }
    }
    

## Notes

  * The cgiapp unit is deprecated, please use fpcgi as much as possible.


  * The fastcgi, custfcgi, and fpfcgi units are not supported on Darwin at this time, because it uses a different mechanism than the MSG_NOSIGNAL option for recv() to indicate that no SIGPIPE should be raised in case the connection breaks. See <http://lists.apple.com/archives/macnetworkprog/2002/Dec/msg00091.html> for how this should be fixed.


  * If you deploy your CGI application and get errors like "Error: No action name and no default action", you should make sure there's an action assigned to the URL, or catch non-supported actions with an action marked Default. In both cases, an OnRequest event handler for the action that sets Handled:=true should be used.



## See also

  * [fpWeb Tutorial](<fpWeb_Tutorial.md> "fpWeb Tutorial")
  * [fphttpclient](<fphttpclient.md> "fphttpclient") Part of fcl-web that can be used stand-alone in client applications
  * [fphttpserver](<fphttpserver.md> "fphttpserver") Small stand alone Object Pascal web server component. Can be used to serve fcl-web applications.
  * [simpleserver](<simpleserver.md> "simpleserver") Small stand alone server application.
  * [CGI_Web_Programming#Debugging_CGI](<CGI_Web_Programming.md> "CGI Web Programming") Information about debugging CGI applications
  * [Part of a tutorial by Leonardo Ramé that shows how to program an fcl-web CGI application; includes example of generating JSON content](<https://github.com/leonardorame/Ext4-MVC-Tutorial/wiki/Lazarus-Server>)
  * [WCHTTPServer](<WCHTTPServer.md> "WCHTTPServer") HTTP/2+HTTP1.1 server based on fcl-web

---

_Source: [https://wiki.freepascal.org/fcl-web](https://web.archive.org/web/20250114033250/https://wiki.freepascal.org/fcl-web)_
