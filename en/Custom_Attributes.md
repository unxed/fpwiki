# Custom Attributes

│ **English (en)** │  **[русский (ru)](<../ru/Custom_Attributes.md>)** │

Custom Attributes currently allow you to decorate type definitions and published properties of classes with additional metadata that can be queried using RTTI (runtime type information). 

**Currently (as of October 2020) the feature is available only in FPC trunk (aka 3.3.1)**

## Contents

  * 1 What can attributes be used for?
  * 2 How are attributes declared?
  * 3 How are attributes used?
    * 3.1 Querying attributes
  * 4 The attributes feature's compatibility with Delphi
  * 5 Complete example
  * 6 See Also



## What can attributes be used for?

An attribute is used to associate specific metadata with a class. For example you can use an attribute to mark a class with the name of its corresponding database table, or to annotate a web service class with a string identifying its base path. 

## How are attributes declared?

Attributes are simply classes that descend from the new System type TCustomAttribute. The constructors of such TCustomAttribute descendants are the most important feature to implement since they are used to pass additional parameters to the attribute (such as the associated database table name, or the base path in the above examples). 

**Important** : If you want to use your attribute type without any arguments then you _must_ declare a parameterless constructor yourself as the one of _TCustomAttribute_ is private. 

## How are attributes used?

Attributes are bound to a type or property by specifying at least one attribute clause ahead of the type or property. For a type the attribute is specified in the type definition (such as a class, record, or enum declaration) or in a unique type redeclaration (e.g. "TLongInt = type LongInt"). Mere type renames (e.g. "TLongInt = LongInt") are not allowed. 

Attribute clauses are only available if the new modeswitch PREFIXEDATTRIBUTES is set, which is the default in mode Delphi and DelphiUnicode. 

The syntax of an attribute clause is as follows: 
    
    
    ATTRIBUTECLAUSE::='[' ATTRIBUTELIST ']'
    ATTRIBUTELIST::=ATTRIBUTE [, ATTRIBUTELIST ]
    ATTRIBUTE::=IDENTIFIER [ ( PARAMLIST ) ]
    PARAMLIST::=CONSTEXPR [, PARAMLIST ]
    

The IDENTIFIER is the name of the attribute class. If you name the attribute class to end with "Attribute" (the case of the "attribute" suffix is immaterial) then the name may be used subsequently without the "Attribute" suffix. So TMyAttribute and TMy are equivalent alternatives in the following example: 
    
    
    program tcustomattr;
    
    {$mode objfpc}{$H+}
    {$modeswitch prefixedattributes}
    
    type
       TMyAttribute = class(TCustomAttribute)
         constructor Create;
         constructor Create(aArg: String);
         constructor Create(aArg: TGUID);
         constructor Create(aArg: LongInt);
       end;
    
       {$M+}
       [TMyAttribute]
       TTestClass = class
       private
         fTest: LongInt;
       published
         [TMyAttribute('Test')]
         property Test: LongInt read fTest;
       end;
       {$M-}
    
       [TMyAttribute(1234)]
       [TMy('Hello World')]
       TTestEnum = (
         teOne,
         teTwo
       );
    
       [TMyAttribute(IInterface), TMy(42)]
       TLongInt = type LongInt;
    
    constructor TMyAttribute.Create;
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: String);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: LongInt);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: TGUID);
    begin
    end;
    
    begin
    
    end.
    

### Querying attributes

Attributes can be accessed by both the TypInfo and Rtti units. 

For the TypInfo unit the ways to access attributes are as follows: 

For types: 

  * use the AttributesTable field in TTypeData
  * use GetAttributeTable on a PTypeInfo
  * use GetAttribute on the attribute table together with an index to get a TCustomAttribute instance



For properties: 

  * use the AttributesTable of TPropInfo
  * use GetAttribute on the attribute table together with an index to get a TCustomAttribute instance
  * use GetPropAttribute on the PPropInfo together with an index to get a TCustomAttribute instance



For the Rtti unit the ways to access attributes are as follows: 

For types: 

  * use GetAttributes on the TRttiType of the type in question



For properties: 

  * use GetAttributes on the TRttiProperty of the property in question



## The attributes feature's compatibility with Delphi

The feature itself is Delphi compatible with the proviso that FPC is much more unforgiving regarding unbound properties. If the attribute class is not known or the attribute clauses are not bound to a valid type or property the compiler will generate an error. 

FPC's RTTI is not considered Delphi compatible. However it covers the same functionality. In contrast to Delphi (which uses Invoke to create the attribute instance) FPC uses a constructor function. FPC's implementation has the advantage of working on systems that don't have full Invoke support. 

Additionally using the PREFIXEDATTRIBUTES modeswitch disables the directive clauses for functions, methods and procedure/method types. The following is not allowed any more with the PREFIXEDATTRIBUTES modeswitch enabled: 
    
    
    procedure Test; [cdecl];
    begin
    end;
    

## Complete example
    
    
    program testattributes;
    {
      This is a simple example on how to use custom attributes.
      The class uses a custom attribute to retrieve a static date 
      at program start-up. This is just for the purpose of demo.
      This demo's:
      - Creation
      - Decoration
      - Retrieval
    }
       
    {$mode delphi}{$H+}{$M+}
    {$warn 5079 off} { turn warning experimental off }
    uses
      sysutils, typinfo, rtti, classes;
     
    type
       {A custom attribute to decorate a class with a certain date }
       ADateTimeAttribute = class(TCustomAttribute)
       private
         FArg:TDateTime;
       public
       { Just to show a Custom Attribute can have mutiple constructors }
         constructor Create(const aArg: TDateTime);overload;
         { We can use the predefined %DATE% compiler include
           In the context of a custom attribute we need a
           constant expression for the default parameter }
         constructor Create(const aArg: String = {$I %DATE%});overload;
        published
         property Date:TDateTime read Farg;
       end;
     
       { A datetime class that is decorated with our custom attribute     }
       { Note you can leave out 'Attribute', the compiler resolves it     }
       { [ADateTime(21237.0)] uses first constructor,displays some date in 1958 }
     
       {This calls the second constructor }
       [ADateTime]
       TMyDateTimeClass = class
       private
         FDateTime:TDateTime;
       public
         constructor create;
       published
         [ADateTime]
         property Day:TDateTime read FDatetime write FdateTime;
       end;
     
       constructor ADateTimeAttribute.Create(const aArg: TDateTime);
       begin
         FArg := aArg;
       end;
     
       constructor ADateTimeAttribute.Create(const aArg: string );
       var
         MySettings:Tformatsettings;
       begin
         { set up the date format according to how
           the compiler formats %DATE% include }
         MySettings :=DefaultFormatSettings;
         MySettings.ShortDateFormat:='yyyymmdd';
         MySettings.DateSeparator :='/';
         { Now convert }
         FArg := StrToDateTime(aArg, MySettings);
       end;
       
       { We query the rtti to set the value }
       constructor TMyDateTimeClass.Create;
       var
        Context : TRttiContext;
        AType : TRttiType;
        Attribute : TCustomAttribute;
       begin
         inherited;  
         Context := TRttiContext.Create;
         try
           AType := Context.GetType(typeinfo(TMyDateTimeClass));
           for Attribute in  AType.GetAttributes do begin
             if Attribute is ADateTimeAttribute then
               FDateTime := ADateTimeAttribute(Attribute).Date;
           end;
         finally
           Context.Free
         end;    
       end;
     
     
    var
     Test:TMyDateTimeClass;      
    begin
       Test := TMyDateTimeClass.Create;
       try
         writeln('Compile date is :',DateTimeToStr(Test.Day));
      finally
        test.free;
      end;
    end.
    

## See Also

  * [FPC New Features Trunk](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")
  * <http://docwiki.embarcadero.com/RADStudio/XE6/en/Overview_of_Attributes> \- Delphi Attributes reference
  * <http://delphi.about.com/od/oopindelphi/a/delphi-attributes-understanding-using-attributes-in-delphi.htm> Delphi Tutorial on property attributes

---

_Source: [https://wiki.freepascal.org/Custom_Attributes](https://web.archive.org/web/20240718110929/https://wiki.freepascal.org/Custom_Attributes)_
