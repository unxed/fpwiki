# fpwebfile

## Contents

  * 1 Intro
  * 2 Usage
    * 2.1 Various locations
    * 2.2 Default location
    * 2.3 Registration order
  * 3 API
    * 3.1 GET
    * 3.2 POST
    * 3.3 PUT
    * 3.4 DELETE
  * 4 Customization
    * 4.1 File serving
    * 4.2 API
    * 4.3 CORS Considerations



## Intro

fpwebfile is a unit with a simple HTTP module (_TFPCustomFileModule_ and _TSimpleFileModule_) to serve files in fcl-web. 

You don't need to do anything except register a location from which to serve files. The module does the rest. 

The unit also has a _TFPWebFileLocationAPIModule_ module which allows to remotely manage the file locations which are served by the _TSimpleFileModule_ class in a JSON & REST fashion. 

## Usage

### Various locations

To serve files from a location, all you need to do is call **RegisterFileLocation** : 
    
    
    Procedure RegisterFileLocation(Const ALocation,ADirectory : String);
    

like so: 
    
    
      RegisterFileLocation('home','/home/myuser/public_html');
    

after this call, an url 
    
    
    http://localhost:8080/home/index.html
    

will attempt to serve the file 
    
    
    /home/myuser/public_html/index.html
    

You can stop serving files with **UnRegisterFileLocation**
    
    
    Procedure UnRegisterFileLocation(Const ALocation: String);
    

like this: 
    
    
      UnRegisterFileLocation('home');
    

### Default location

Note that all paths registered with _RegisterFileLocation_ must have a prefix: the name of the location. This means that the following URL cannot be served: 
    
    
    http://localhost:8080/index.html
    

However, to serve this kind of file anyway, the **TSimpleFileModule** can be registered as the default route: 
    
    
    Class Procedure TSimpleFileModule.RegisterDefaultRoute(OverAllDefault : Boolean = True);
    

when invoked like this: 
    
    
    TSimpleFileModule.BaseDir:='/home/myuser/public_html/';
    TSimpleFileModule.RegisterDefaultRoute;
    

The URL 
    
    
    http://localhost:8080/index.html
    

will be correctly served. 

If the URL does not contain a document, but only a directory: 
    
    
    http://localhost:8080/
    

You can tell the **TSimpleFileModule** to serve file _index.html_ by setting 
    
    
    TSimpleFileModule.IndexPageName:='index.html';
    

You can mix _RegisterFileLocation_ and _TSimpleFileModule.RegisterDefaultRoute_. When an URL is checked, the various locations registered with _RegisterFileLocation_ will be tried first. If no location matches, the default location will be tried. 

### Registration order

Note that the order in which you register locations is important: the first match on the initial element of the URL path will be used. 

When you use both _RegisterFileLocation_ and _TSimpleFileModule.RegisterDefaultRoute_ , then the locations created with _RegisterFileLocation_ will always be checked before the default route. 

## API

The _TFPWebFileLocationAPIModule_ module is a ready-to-run module which allows to remotely manage the file locations. It is basically a REST interface to the _RegisterFileLocation_ and _UnRegisterFileLocation_ routines. 

The class can be registered with the followin class method: 
    
    
    Class procedure RegisterFileLocationAPI(Const aPath,aPassword : String);
    

For example, the following call: 
    
    
    TFPWebFileLocationAPIModule.RegisterFileLocationAPI('_locations','mysecret');
    

Will enable the following REST URL: 
    
    
    http://localhost:8080/_locations/
    

If a password was supplied in the _RegisterFileLocationAPI_ call, you must authenticate requests to the locations endpoint. 

This can be done in one of 2 ways: 

  1. Provide the key with the APIKey query variable.
  2. Or provide the key as an Authorization bearer token.



The first way results in an URL like this: 
    
    
    http://localhost:3003/_locations/?APIKey=mysecret
    

For the second way, the following header must be added to the request 
    
    
    Authorization: Bearer mysecret
    

The 4 CRUD operations are supported in the REST API. 

### GET

use the GET command to get a list of locations: 
    
    
    wget -q 'http://localhost:3003/_locations/?APIKey=mysecret&fmt=1' -O -
    

This will result in something like 
    
    
    {
      "data" : [
        {
          "location" : "tmp",
          "path" : "/tmp/"
        },
        {
          "location" : "*",
          "path" : "/home/michael/public_html/"
        }
      ]
    }
    

if you omit the _fmt=1_ query parameter then the returned JSON will not be formatted. 

### POST

a _POST_ command will register a new location. The content-type must be **application/json** , and the payload a JSON object with 2 keys: 

location
    the name of the location
path
    the directory for the location

Several checks are done: 

  1. The path must exist and be a directory.
  2. The location name may not be empty.
  3. The location name may not contain **/** characters.
  4. The location name may not yet be registered.



So, for example, the above 'tmp' location may be registered as: 
    
    
    wget -q --post-data='{ "location" : "tmp", "path": "/tmp" }' \
      --header="Content-Type: application/json" \
      'http://localhost:3003/_locations/?APIKey=mysecret&fmt=1' -O -
    

if all went well, the output is the new location: 
    
    
    { "location" : "tmp", "path" : "/tmp" }
    

Using fphttpclient: 
    
    
    Uses classes, fphttpclient, fpjson;
    ...
      JSON:=TStringStream.Create('{'
        +'"location" : "'+StringToJSONString(Location)+'",'
        +'"path": "'+StringToJSONString(Path)+'"'
        +'}');
      Client:=TFPHTTPClient.Create(Nil);
      Response:=TMemoryStream.Create;
      try
        Client.RequestBody:=JSON;
        Client.AddHeader('Content-Type','application/json');
        Client.HTTPMethod('POST',URL,Response,[201,200]);
        Response.Position:=0;
        // maybe: check response
      finally
        Response.Free;
        JSON.Free;
        Client.Free;
      end;
    

### PUT

a _PUT_ command will update an existing location. The content-type must be **application/json** , and the payload a JSON object with 1 or 2 keys as for the POST call. The name of the location can be part of the url: 
    
    
    wget -q --post-data='{ "path": "/tmp2" }' \
      --header="Content-Type: application/json" \
      --method=PUT 'http://localhost:3003/_locations/tmp?APIKey=mysecret&fmt=1' -O -
    

If the _location_ element is present in the payload, it will be used to rename the location. 

### DELETE

a _DELETE_ command will delete an existing location. No payload must be present. 
    
    
    wget -q --method=DELETE 'http://localhost:3003/_locations/tmp?APIKey=mysecret&fmt=1' -O -
    

If the _location_ element is present in the payload, it will be used to rename the location. 

Using fphttpclient: 
    
    
    uses classes, fphttpclient;
    ... 
      URL:='http://127.0.0.1:'+IntToStr(Port)+'/'+APIPath+'/'+Location+'?APIKey='+APIKey+'&fmt=1';
      Response:=TMemoryStream.Create;
      Client:=TFPHTTPClient.Create(Nil);
      try
        Client.HTTPMethod('DELETE',URL,Response,[204,200]);
        Response.Position:=0;
        // maybe: check response
      finally
        Response.Free;
        Client.Free;
      end;
    

## Customization

### File serving

If you want to customize the request treatment, you can always create a descendant of the _TSimpleFileModule_ class and register that: 
    
    
    TSimpleFileModule.DefaultSimpleFileModuleClass:=TMySimpleFileModule;
    

requests will be handled by creating an instance of TMySimpleFileModule for each file served. 

You can use this for example to implement authentication or file filtering. 

### API

Similarly if you want to customize the API request treatment, you can always create a descendant of the _TFPWebFileLocationAPIModule_ class and register that: 
    
    
    TFPWebFileLocationAPIModule.LocationAPIModuleClass:=TMyFileAPIModule;
    

requests will be handled by creating an instance of TMyFileAPIModule for each API request. 

### CORS Considerations

The API allows CORS requests by default. You can disable this by creating a descendent and setting _Cors.Enabled_ to _False_ in the constructor. The file serving mechanism is not CORS enabled.

---

_Source: [https://wiki.freepascal.org/fpwebfile](https://web.archive.org/web/20240422004859/https://wiki.freepascal.org/fpwebfile)_
