# TObject

│ **English (en)** │

**`TObject`** is the base [`class`](<Class.md> "Class") for any other `class`, whether you declare it or not. It provides only some very generic [methods](<Method.md> "Method"), mostly [`virtual`](</index.php?title=virtual&action=edit&redlink=1> "virtual \(page does not exist\)"). The rationale for `TObject` is that in [Object Pascal](<Object_Pascal.md> "Object Pascal") every `class` must be derived from another, `TObject` being the exception that ends the derivation chain. 

The [`constructor`](<Constructor.md> "Constructor") `TObject.Create()`, inherited by every other `class` by default, can be hidden declaring another `constructor` with the same name in the derived `class`, with or without parameters. This come handy implementing the [Singleton pattern](</index.php?title=Singleton_pattern&action=edit&redlink=1> "Singleton pattern \(page does not exist\)"). 
    
    
     { Defined in objpash.inc }
    
    
    constructor Create; 
    destructor  Destroy; virtual;
    class function newinstance : tobject; virtual;
    procedure CleanupInstance;
    procedure FreeInstance; virtual;
    
    function  SafeCallException(exceptobject : tobject; exceptaddr : codepointer) : HResult; virtual;
    procedure DefaultHandler(var message);virtual;
    procedure Free;
    
    class function InitInstance(instance : pointer) : tobject; 
    class function ClassType : tclass; 
    class function ClassInfo : pointer;
    class function ClassName : shortstring;
    class function ClassNameIs(const name : string) : boolean;
    class function ClassParent : tclass;
    class function InstanceSize : SizeInt;
    class function InheritsFrom(aclass : tclass) : boolean;
    class function StringMessageTable : pstringmessagetable;
    class function MethodAddress(const name : shortstring) : codepointer;
    class function MethodName(address : codepointer) : shortstring;
    function FieldAddress(const name : shortstring) : pointer;
    
    procedure AfterConstruction;virtual;
    procedure BeforeDestruction;virtual;
    
    procedure DefaultHandlerStr(var message);virtual;
    
    procedure Dispatch(var message);virtual;
    procedure DispatchStr(var message);virtual;
    
    function GetInterface(const iid : tguid; out obj) : boolean;
    function GetInterface(const iidstr : shortstring;out obj) : boolean;
    function GetInterfaceByStr(const iidstr : shortstring; out obj) : boolean;
    function GetInterfaceWeak(const iid : tguid; out obj) : boolean;
    
    class function GetInterfaceEntry(const iid : tguid) : pinterfaceentry;
    class function GetInterfaceEntryByStr(const iidstr : shortstring) : pinterfaceentry;
    class function GetInterfaceTable : pinterfacetable;
    
    class function UnitName : ansistring;
    function Equals(Obj: TObject) : boolean; virtual;
    function GetHashCode: PtrInt; virtual;
    function ToString: ansistring; virtual;

---

_Source: [https://wiki.freepascal.org/TObject](https://web.archive.org/web/20250323041405/https://wiki.freepascal.org/TObject)_
