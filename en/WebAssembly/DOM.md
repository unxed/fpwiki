# WebAssembly/DOM

Accessing JS Objects from [WebAssembly](<Compiler.md> "WebAssembly/Compiler"). 

## Contents

  * 1 What is JOB?
  * 2 Using JOB
    * 2.1 JS Classes
    * 2.2 Callbacks
    * 2.3 Type casts
    * 2.4 Typeof
    * 2.5 Register a JS object/function
  * 3 Create JOB units using webidl2pas
    * 3.1 Not yet supported webidl elements
      * 3.1.1 Elements from other Pascal units
      * 3.1.2 callback interfaces
      * 3.1.3 function returning a Dictionary
      * 3.1.4 function returning a typed sequence
      * 3.1.5 function returning a callback
      * 3.1.6 passing an array as argument
      * 3.1.7 varargs
      * 3.1.8 constructor
      * 3.1.9 getter
      * 3.1.10 callback property
  * 4 JOB architecture
    * 4.1 Data transfer between JS/WebAssembly
    * 4.2 Function Arguments
    * 4.3 Function Result
    * 4.4 Variants
    * 4.5 Callbacks
  * 5 Implementation details
  * 6 ToDos



# What is JOB?

JS Object Bridge - JOB 

JOB provides: 

  * Units to communicate between fpc wasm and pas2js browser to call JS functions, get and set JS properties and set callbacks.
  * Units for fpc wasm with common browser classes.
  * A tool webidl2pas to help generating new units for JS classes from webidls.



# Using JOB

A JOB program has a webassembly program (fpc wasi) and a browser (pas2js) program. 

The browser side contains the html and registers all global JS variables needed by the webassembly side. 

See the pas2js demo/wasienv/button/BrowserButton1.lpi 

<https://gitlab.com/freepascal.org/fpc/pas2js/-/tree/main/demo/wasienv/button>

## JS Classes

Each JS class like **HTMLButtonElement** have a FPC class (_TJSHTMLButtonElement_) with a reference counted interface (_IJSHTMLButtonElement_). 

Normally functions return an interface and expect interfaces as arguments. 

## Callbacks

An event handler in WebAssembly can be called from Javascript. 

At the moment only methods (of object) are supported. 
    
    
      TWasmApp = class
        ...
        function OnButtonClick(Event: IJSEvent): boolean;
        ...
      end;
    
    function TWasmApp.OnButtonClick(Event: IJSEvent): boolean;
    begin
      JSWindow.Alert('You triggered TWasmApp.OnButtonClick');
      Result:=true;
    end;
    ...
      JSButton.addEventListener('click',@OnButtonClick);
    ...
    

## Type casts

Often a low level interface needs to be type casted to a descendant (or another type). For example _JSDocument.getElementById_ returns a IJSElement, which is type casted to IJSHTMLElement: 
    
    
    var 
      Elem: IJSElement;
      HTMLElem: IJSHTMLElement;
    begin
      Elem := JSDocument.getElementById('button');
      // type casting to IJSHTMLElement requires creating a new bridge object using TJSHTMLElement.Cast:
      HTMLElem := TJSHTMLElement.Cast(Elem);
      // Note: since Elem and HTMLElem are reference counted interfaces, the compiler automatically frees temporary objects.
    end;
    

This **HTMLElement** has the same **ObjectId** as **Elem** , but it does not own it. It merely keeps a reference to the **Elem**. When all references are released, the JS object is released, 

## Typeof

**InvokeJSTypeOf**

See the JOBResult_* constants in unit JOB_Shared. 

## Register a JS object/function

By default the TJSObjectBridge registers only some basic JS objects/functions like _Object, Function, Date, String, Array, JSON, Promise, ArrayBuffer, Int8Array, Uint8Array_ , etc. 

For example if you have the pas2js external class **TJSBird** for the JS object/function **Bird** , register it in JOB: 
    
    
    type
      TJSBird = class external name 'Bird'
        ...
      end;
    ...
      if WADomBridge.RegisterGlobalObject(TJSBird,'Bird')=0 then
        raise Exception.Create('failed to register TJSBird');
    

In the fpc/webassembly code create an interface: 
    
    
    type
      IJSBird = interface
        [{guid}]
      end;
    
      TJSBird = class(TJSObject,IJSBird)
        ...
      end;
    
    var JSBird: IJSBird; // this is the JS variable 'Bird'
    
      JSBird := TJSBird.JOBCreateGlobal('Bird') as IJSBird;
    

If the above Bird is a JS function you can create a **new** descendant with parameter 123 (in Javascript: **new Bird(123)** ; ) by calling: 
    
    
    var MyBird: IJSBird;
    
      MyBird := JSBird.NewJSObject([123]) as IJSBird; // in JS: new Bird(123)
    

For a full example see demo/wasienv/dom/WasiDomTest1.lpi and BrowserDomTest1.lpi 

# Create JOB units using webidl2pas

Compile the latest and greatest webidl2pas utility from fpc main: 

utils/pas2js/webidl2pas.lpi 

Download some webidls for example from <https://hg.mozilla.org/mozilla-central/raw-file/tip/dom/webidl/><JSClassName>

Concatenate them into one file _Foo.webidl_. 

Create a text file _FooAlias.txt_ containing the used classes from other units: 
    
    
    Object=IJSObject
    Set=IJSSet
    Map=IJSMap
    Function=IJSFunction
    Date=IJSDate
    RegExp=IJSRegExp
    String=IJSString
    Array=IJSArray
    ArrayBuffer=IJSArrayBuffer
    TypedArray=IJSTypedArray
    BufferSource=IJSBufferSource
    DataView=IJSDataView
    JSON=IJSJSON
    Error=IJSError
    TextEncoder=IJSTextEncode
    TextDecoder=IJSTextDecoder
    

Create a text file _FooGlobals.txt_ containing all the global JS variables in form Pascal variable name=JS class name,registered name: 
    
    
    JSFoo=Foo,foo
    

Run the tool: 
    
    
    webidl2pas -f wasmjob -i Foo.webidl --typealiases=@FooAlias.txt --globals=@FooGlobals.txt
    

If the tool stops, because it can not find an identifier or something is not yet supported, you can add another webidl or comment the problematic definition. Then run the tool again. 

In the browser side you must register the global JS variables: 
    
    
      FWADomBridge.RegisterGlobalObject(foo,'foo');
    

## Not yet supported webidl elements

### Elements from other Pascal units

At the moment you can only provide a list of type aliases for classes. This does not work for callbacks, arrays, sequences, etc. It would be better to give used units and parse them. 

### callback interfaces

"callback interfaces" are a legacy definition. Remedy: Replace them manually with a callback 

### function returning a Dictionary

ToDo: Define dictionary as class and interface and return an interface reference. 

### function returning a typed sequence

At the moment it returns an untyped IJSArray. 

ToDo: returned a typed array. 

### function returning a callback

### passing an array as argument

### varargs

Functions allowing to pass an arbitrary number of arguments. 

Workaround: User can call the InvokeX function directly. 

### constructor

### getter

### callback property

A property with a callback, e.g. **onAbort**

At the moment it is only added as a comment. Reading callbacks is not yet supported. Theoretically it could be added as a write only property. 

# JOB architecture

Pascal units containing ‘proxy’ classes: calling a method on a proxy class will call the corresponding class in JS. The proxy classes can be generated by the existing **webidl2pas** tool with **-f wasmjob** flag. 

## Data transfer between JS/WebAssembly

JS/Webassembly interface only supports passing atomic types like boolean, integers and floats, not objects or strings. 

**Solution** : 

  * Global objects like **document** and **window** are registered and queried by name.
  * Every object is stored in an array with ID: TJOBObjectID
  * ID is used to pass references to object between JS and Webassembly
  * Lifetime is controlled from WebAssembly.
  * By using interfaces, the lifetime of objects are controlled by the compiler.
  * Methods can be called using an invoke mechanism.
  * Due to limited type support in Javascript, only a handful of types must be supported by invoke, e.g. undefined, null, boolean, number, unicodestring, Object, callbacks and the union JSValue.



## Function Arguments

When calling a JS function from wasm, you can pass the following types/constants: 

  * boolean
  * integers (limited to double, because all numbers in JS are double, so up to 54 bits)
  * double
  * nil or Variants.Null
  * string (utf8 converted to utf16, either use UTF8Encode/UTF8Decode or install a widestringmanager by using the units unicodeducet, unicodedata, fpwidestring)
  * unicodestring
  * widestring
  * PChar - using strlen to get the size and utf8 converted to utf16
  * PWideChar - using strlen to get the size
  * TJSObject and IJSObject - its ObjectID is passed to the JS side, where the corresponding JS object is used
  * Variant
  * Variants.UnAssigned or JSUndefined
  * TJOB_JSValue - at the moment needed for TJOB_Method.



## Function Result

Calling a JS function is done via the _InvokeJS*Result_ functions, e.g. _aJSDate.InvokeJSUnicodeStringResult('toLocaleDateString',[])_ which returns a _UnicodeString_. 

If the function does not exist, an _EJSInvoke_ exception is raised. 

If the function returns the JS undefined value, JOB returns the default value, e.g. InvokeJSUnicodeStringResult returns the empty string, InvokeJSDoubleResult returns NaN, InvokeJSObjectResult returns nil, InvokeJSBooleanResult returns false. 

If the function returns an incompatible type, e.g. InvokeJSUnicodeStringResult returns a number, an _EJSInvoke_ exception is raised. 

To retrieve any kind of JS value, use _InvokeJSVariantResult_. 

## Variants

When an argument or result type does not have a simple type, webidl2pas uses **Variant**. Variants have a few newbie traps: 

For JS **undefined** you can use **Variants.Unassigned** and _VarIsEmpty(v)_. 

For JS **null** you cannot use _nil_ , you can use **Variants.Null** or _VarIsNull(v)_. _nil_ cannot be used, because fpc treats nil with variants like an empty Unicodestring. E.g. "if aVariant=nil then" is wrong, use "if aVariant=Variants.Null then" instead. 

Variants do not support 8bit strings, only 16bit Unicodestrings. For example: _ansistring:=aVariant_ works only for ascii strings, but for unicodestrings you must either use **UTF8Encode** or a proper widestringmanager. 

**Interfaces** : When a function returns a Variant containing an object, the variant will contain a **IJSObject or Variants.Null**. Note that _SomeIntf:=aVariant_ will compile, but does no type check, so _SomeIntf_ might only be a _IJSObject_ instead of _IJSSomeIntf_. If you know that _aVariant_ has a _IJSSomeIntf_ you can use a type cast: **SomeIntf:=TJSSomeIntf.Cast(aVariant);**. There is no general way in JS to check what "class" a JS object is. 

## Callbacks

An event handler in WebAssembly can be called from Javascript. 

At the moment only methods (of object) are supported. 

Every function type needs a callback, which decodes the arguments and encode the result. 

For example TJSEventHandler: 
    
    
    type
      TJSEventHandler = function(Event: IJSEventListenerEvent): boolean of object;
    ...
    function JOBCallTJSEventHandler(const aMethod: TMethod; var H: TJOBCallbackHelper): PByte;
    var
      Event: IJSEventListenerEvent;
    begin
      // get arguments. First as IJSEventListenerEvent
      Event:=H.GetObject(TJSEventListenerEvent) as IJSEventListenerEvent;
      // call the method and encode the result
      Result:=H.AllocBool(TJSEventHandler(aMethod)(Event));
    end;
    

TJOBCallbackHelper provides functions to decode arguments and encode the result. 

Passing a method as argument works like this: 
    
    
    type
      IJSEventTarget = interface
        ['{1883145B-C826-47D1-9C63-47546BA536BD}']
        procedure addEventListener(const aName: UnicodeString; const aListener: TJSEventHandler);
      end;
    
      TJSEventTarget = class(TJSObject,IJSEventTarget)
        procedure addEventListener(const aName: UnicodeString; const aListener: TJSEventHandler);
      end;
    ...
    procedure TJSEventTarget.addEventListener(const aName: UnicodeString; const aListener: TJSEventHandler);
    var
      m: TJOB_JSValueMethod;
    begin
      // combine the users method and the callback into one argument m
      m:=TJOB_JSValueMethod.Create(TMethod(aListener),@JOBCallTJSEventHandler);
      try
        // call the JS function addEventListener(aName,m)
        InvokeJSNoResult('addEventListener',[aName,m]);
      finally
        m.Free;
      end;
    end;
    

See units job_js and job_web for more examples. 

# Implementation details

Here are some technical notes describing the various architectural decisions. 

The webidl2pas tool was extended to generate an interface from the .webidl files (-f wasmjob). These files for example exist in the mozilla firefox repo on github: [WebIDL](<https://github.com/mozilla/gecko-dev/tree/master/dom/webidl>)
    
    
    IJSElement = interface(IJSObject)
      ['someawfulGUID']
      function childElementCount : Integer;
      function firstElementChild : IJSElement;
      // all other
    end;
    

In implementation, the following kind of code can be found: 
    
    
    // Hand crafted in e.g. JSObject unit
      TJSObject = class(TInterfacedObject,IJSObject)
      public
        constructor JOBCast(Intf: IJSObject); overload;
        constructor JOBCreateFromID(aID: TJOBObjectID); virtual; // use this only for the owner (it will release it on free)
        constructor JOBCreateGlobal(const aID: UnicodeString); virtual;
        class function Cast(Intf: IJSObject): IJSObject; overload;
        destructor Destroy; override;
        property JOBObjectID: TJOBObjectID read FJOBObjectID;
        property JOBObjectIDOwner: boolean read FJOBObjectIDOwner write FJOBObjectIDOwner;
        property JOBCastSrc: IJSObject read FJOBCastSrc; // nil means it is the original, otherwise it is a typecast
        property ObjectID: TJOBObjectID read FObjectID;
        // call a function
        procedure InvokeJSNoResult(const aName: string; Const Args: Array of const; Invoke: TJOBInvokeType = jiCall); virtual;
        function InvokeJSBooleanResult(const aName: string; Const Args: Array of const; Invoke: TJOBInvokeType = jiCall): Boolean; virtual;
        function InvokeJSDoubleResult(const aName: string; Const Args: Array of const; Invoke: TJOBInvokeType = jiCall): Double; virtual;
        function InvokeJSUnicodeStringResult(const aName: string; Const Args: Array of const; Invoke: TJOBInvokeType = jiCall): UnicodeString; virtual;
        function InvokeJSObjectResult(const aName: string; Const Args: Array of const; aResultClass: TJSObjectClass; Invoke: TJOBInvokeType = jiCall): TJSObject; virtual;
        function InvokeJSVariantResult(const aName: string; Const Args: Array of const; Invoke: TJOBInvokeType = jiCall): Variant; virtual;
        function InvokeJSUtf8StringResult(const aName: string; Const args: Array of const; Invoke: TJOBInvokeType = jiCall): String; virtual;
        function InvokeJSLongIntResult(const aName: string; Const args: Array of const; Invoke: TJOBInvokeType = jiCall): LongInt; virtual;
        function InvokeJSMaxIntResult(const aName: string; Const args: Array of const; Invoke: TJOBInvokeType = jiCall): int64; virtual;
        function InvokeJSTypeOf(const aName: string; Const Args: Array of const): TJOBResult; virtual;
        // read a property
        function ReadJSPropertyBoolean(const aName: string): boolean; virtual;
        function ReadJSPropertyDouble(const aName: string): double; virtual;
        function ReadJSPropertyUnicodeString(const aName: string): UnicodeString; virtual;
        function ReadJSPropertyObject(const aName: string; aResultClass: TJSObjectClass): TJSObject; virtual;
        function ReadJSPropertyUtf8String(const aName: string): string; virtual;
        function ReadJSPropertyLongInt(const aName: string): LongInt; virtual;
        function ReadJSPropertyInt64(const aName: string): Int64; virtual;
        function ReadJSPropertyVariant(const aName: string): Variant; virtual;
        // write a property
        procedure WriteJSPropertyBoolean(const aName: string; Value: Boolean); virtual;
        procedure WriteJSPropertyDouble(const aName: string; Value: Double); virtual;
        procedure WriteJSPropertyUnicodeString(const aName: string; const Value: UnicodeString); virtual;
        procedure WriteJSPropertyUtf8String(const aName: string; const Value: String); virtual;
        procedure WriteJSPropertyObject(const aName: string; Value: IJSObject); virtual;
        procedure WriteJSPropertyLongInt(const aName: string; Value: LongInt); virtual;
        procedure WriteJSPropertyInt64(const aName: string; Value: Int64); virtual;
        procedure WriteJSPropertyVariant(const aName: string; const Value: Variant); virtual;
        // create a new object using the new-operator
        function NewJSObject(Const Args: Array of const; aResultClass: TJSObjectClass): TJSObject; virtual;
      end;
    

The various **Invoke*** functions encode the arguments in a memory block so they can be read on the JS side, then calls a **Invoke_*Result** function which lives in Javascript, and which is imported from the browser. 

That function does the actual call: it uses **ObjectID** to look for the _Self_ object in an array: 

  * Negative IDs are special: window, document.
  * positive IDs are temporary objects created via the **InvokeJSObjectResult** , see below.



The **Invoke_*Result** pas2js function decodes the arguments and uses **TJSFunction.apply** to execute the requested function. The result is checked for the requested type and then returned to the wasm. 

If the result is an object, an ID is generated (simple counter), the result value is stored in an array **FLocalObjects**. 

The ID is returned to the webassembly, which will use the ID to create a _TJSObject_ descendent. 

The destructor of **TJSObject** calls a **__job_release_object** function in javascript if the **ObjectID** is positive. The **ReleaseObject** function simply sets **FLocalObjects[id]** to null, so the browser also releases it. 

The above is a basic invoke mechanism for Javascript code. 

This basic mechanism is then used by a modified version of the webidl program to generate proxy definitions. For each object in Javascript, 2 definitions are generated: 

  * The interface (see above for an example)
  * An implementation object as below, descendant of **TJSObject**


    
    
    // Generated from webIDL in jsweb/jsdom unit.
     
    IJSElement = interface(IJSNode)
      function childElementCount : Integer;
      function firstElementChild : IJSElement;
    end;
    
    TJSElementImpl = class(TJSObject,IJSElement)
      function childElementCount : Integer;
      function firstElementChild : IJSElement;
      // all other
    end;
     
    function TJSElementImpl.childElementCount : Integer;
    begin
      Result:=ReadJSPropertyLongInt('childElementCount');
    end;
     
    function TJSElementImpl.firstElementChild : IJSElement;
    begin
      Result:=ReadJSPropertyObject('firstElementChild',TJSElementImpl) as IJSElement;
    end;
    

# ToDos

  * read/write array elements
  * store/cache callbacks to support removeEventListener
  * move job_web+job_js units to fpc
  * webidl2pas: 
    * stringifier
    * check Getter+Setter name conflicts
    * argument of sequence<something>
    * overloaded version for dictionary args
    * varargs
    * js getter
    * js constructor
  * pas2jsdsgn: new wasmjob browser project

---

_Source: [https://wiki.freepascal.org/WebAssembly/DOM](https://web.archive.org/web/20250323014114/https://wiki.freepascal.org/WebAssembly/DOM)_
