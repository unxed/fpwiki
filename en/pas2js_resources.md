# pas2js resources

## Contents

  * 1 Pas2js - Resources
    * 1.1 Compiler
    * 1.2 Command-line options
    * 1.3 API
    * 1.4 Examples
    * 1.5 Extending the compiler
  * 2 Navigation



## Pas2js - Resources

### Compiler

The compiler supports the **{$R somefile}** directive. 

This means that directives like 
    
    
    {$R *.html}
    

or 
    
    
    {$R left.png}
    
    {$R right.png}
    
    {$R up.png}
    
    {$R down.png}
    

will attempt to include the specified files. 

Custom properties are defined with the semicolon as separator and colon as the name/value assignment operator. An HTML file can be added as a resource with full file name including the extension and with text/html as file format with the following command: 
    
    
    {$R myFile.html;name:myFile.html;format:text/html}
    

This mechanism is pluggable, and the compiler currently supports 2 mechanisms: 

  * The resources are added to a javascript structure in the generated javascript.


  * The resources are emitted to a file with html snippet. this will look like this:



For a node.js program only the Javascript mechanism can be chosen. 
    
    
         <link rel="preload" id="resource-testres" data-unit="consoledemo"
    href="data:text/html;base64,PGgzPlRlc3QgcmVzb3VyY2UgaW4gSFRNTDwvaDM+ClRoaXMgaXMgYSBtYWluIHByb2dyYW0gZmlsZSByZXNvdXJjZSBpbiBIVE1MLgo=" />
         <link rel="preload" id="resource-testb" data-unit="unitb"
    href="data:text/html;base64,PGgzPkhlbGxvIFJlc291cmNlIFdvcmxkIFBhcnQgQjwvaDM+CjxwPgpUaGlzIG1lc3NhZ2UgaXMgc2F2ZWQgaW4gYSByZXNvdXJjZSBmaWxlIChQYXJ0IEIpLgo8L3A+Cgo=" />
         <link rel="preload" id="resource-testa" data-unit="unita"
    href="data:text/html;base64,PGgzPkhlbGxvIFJlc291cmNlIFdvcmxkIFBhcnQgQTwvaDM+CjxwPgpUaGlzIG1lc3NhZ2UgaXMgc2F2ZWQgaW4gYSByZXNvdXJjZSBmaWxlIChQYXJ0IEEpLgo8L3A+Cgo=" />
    

This html can be pasted into the HTML file of your project. Or the HTML snippet can be loaded dynamically. In both cases, the resource is base64-encoded before including it. 

### Command-line options

You can select the resource mechanism to use using the -JR mechanism: 
    
    
     -Jr<x> Control writing of linked resources
    

There are 3 options for the _x_ value: 

  * **none** : Do not write resources
  * **js** : Write resources in Javascript structure
  * **html=filename** : Write resources as preload links in the specified HTML file.



    The HTML filename can be omitted, by default _projectfile-res.html_ is used.

### API

The _p2jsres_ unit contains the API to access the resources that were linked in at compile time. 
    
    
    Function SetResourceSource(aSource : TResourceSource) : TResourceSource;
    
    Function GetResourceNames : TStringDynArray;
    Function GetResourceNames(aSource : TResourceSource) : TStringDynArray;
    
    Function EnumResources(aCallback : TResourceEnumCallBack) : Integer;
    Function EnumResources(aSource : TResourceSource; aCallback : TResourceEnumCallBack) : Integer;
    
    Function GetResourceInfo(Const aName : String; var aInfo : TResourceInfo) : Boolean;
    Function GetResourceInfo(aSource : TResourceSource; Const aName : String; var aInfo : TResourceInfo) : Boolean;
    
    Procedure LoadHTMLLinkResources(const aURL : String; OnLoad : TResourcesLoadedEnumCallBack = Nil; OnError : TResourcesLoadErrorCallBack = Nil);
    

These calls have the following purposes: 

  * **SetResourceSource** can be used to set the mechanism by which the resource values must be accessed. It currently has the following values



    * **rsJS** the resources are embedded in the Javascript.
    * **rsHTML** the resources are embedded in the HTML.
    This value is used whenever a call is made that does not specify the resource access mechanism.

  * **GetResourceNames** can be used to get a list of the available resource names. If _aSource_ is omitted, the source set with _SetResourceSource_ is used.
  * **EnumResources** calls the callback _aCallBack_ for every resource found. If _aSource_ is omitted, the source set with _SetResourceSource_ is used.
  * **GetResourceInfo*** returns resource info for resource _aName_ in the structure _aInfo_. If _aSource_ is omitted, the source set with _SetResourceSource_ is used.
  * **LoadHTMLLinkResources** will load HTML resources from URL _aURL_ , and will call _OnLoad_ if the resources were successfully loaded, or _OnError_ when the loading failed.



The API is the same for both kind of resources, this is transparant, but you must decide what kind of resources you want to use. You can mix the 2 kinds of resources, although this is not recommended. 

  


### Examples

There are 3 example programs in the demo directories which demonstrate the API. They can be viewed on-line 

[Using javascript-embedded resources](<https://www.freepascal.org/~michael/pas2js-demos/resources/htmldemo.html>)

[Using HTML <link> resources](<https://www.freepascal.org/~michael/pas2js-demos/resources/htmllinkdemo.html>)

[Using dynamically loaded HTML <link> resources](<https://www.freepascal.org/~michael/pas2js-demos/resources/htmlloadlinkdemo.html>)

There is also a nodejs demo, but this one needs to be compiled from source and run on your local machine. 

### Extending the compiler

The compiler handles the resource linking by invoking a callback for the $R linking directive. Currently, only 2 kinds of resources are supported, but it is possible to extend this by creating additional callbacks for the compiler. 

The following extensions are planned: 

  * Support {$R *.res}



    This will need to extract the actual resources from the *.res file and add them separately to the JS/HTML.

  * Add support in the Lazarus IDE to specify what resources you want to use,



    and automatically include the generated HTML snippet into the project HTML.

## Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [Pas2JS Version Changes](<Pas2JS_Version_Changes.md> "Pas2JS Version Changes")
  * Back to [lazarus_pas2js_integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/pas2js_resources](https://web.archive.org/web/20250323012421/https://wiki.freepascal.org/pas2js_resources)_
