# Interfaces

Interfaces can be utilized as an alternative solution to the need of multiple inheritance, which Object Pascal currently does not support. These are distinct from the [Interface](<Interface.md> "Interface") reserved word which relates to program structure. 

## Full example program
    
    
    program project1;
    
    {$mode delphi}
    {$interfaces corba}
    
    type
      IMyDelegate = interface
        procedure DoThis (value: integer);
      end;
    
      TMyClass = class (TInterfacedObject, IMyDelegate)
        procedure DoThis (value: integer);
      end;
    
    procedure TestDelegate;
    var
      delegate: TMyClass;
      intfdelegate: IMyDelegate;
    begin
      delegate := TMyClass.Create;
      intfdelegate := IMyDelegate(delegate);
      intfdelegate.DoThis(1);
    end;
    
    { TMyClass }
    
    procedure TMyClass.DoThis(value: integer);
    var
      Str: string;
    begin
      WriteLn('Success!!! Type <enter> to continue');
      ReadLn(Str);
    end;
    
    begin
      TestDelegate;
    end.
    

## Detailed introduction text

Interfaces - are lightweight and very powerful way to implement multiple inheritance or, if we would be more accurate, multiple polymorphism. You can think about them as about abstract classes. Single inheritance is fast because descendant class is memory access compatible with it's ancestor. So simple assignment is enough to turn one to another. But it's not true in case of multiple inheritance and that fact causes many problems. That's why multiple inheritance isn't considered to be good practice. But there is one case, when it's allowed - multiple polymorphism with abstract ancestors. They don't add any data to class, that is inherited from them. And therefore only thing, that is needed to implement them - another virtual method table (VMT). 

Interface isn't object! It's just way to access object. I.e., well, interface. So it's name is self-explanatory. And it should be obvious, that object can have several ways to access it, i.e. several interfaces. Think about them as about "sockets" to plug other objects to your object. 

You should understand, that there are two kinds of interfaces. Pascal interfaces implement them both at the same time and sometimes it causes some confusion. 1) Interfaces as tool to implement multiple inheritance, as they work in some other languages, like Java 2) OLE/COM interfaces as Windows-specific language-independent way to implement OOP. So, first of all you should realize, which of this two cases is yours. 

Overall interfaces are easy to use. They're defined the same way, as abstract classes. Only thing - they don't support access restriction via public/protected/private. All methods and properties are public. 
    
    
        TMyInterface = interface
          function GetX:TData;
          procedure SetX(AX:TData);
          property X:TData read GetX write SetX;//Properties are allowed here!!! It's way to make interfaces to look more like objects.
        end;
    

Then you can "inherit" (it's called "implement interface") your class from any amount of interfaces. 
    
    
        TMyClass = class(TInterfacedObject, TMyInterface1, TMyInterface2, TMyInterface3)
        //You should implement all interface methods here!!!
        end;
    

TInterfacedObject implements IUnknown interface methods, that are required for normal interface operation, so you don't need to do it yourself. 

Inheriting one interface from another is also allowed - it just adds all methods of parent interface to it's descendant and makes descendant memory access compatible with parent, i.e. allows casting one to another. 
    
    
        TInterface2 = interface(TInterface1)
        //Some other methods here
        end;
    

Then you can "cast" your class to interfaces the same way, you would do it with classes: 
    
    
        MyInterface := MyClass;
    

GUID - is special thing. It's just way to assign super-unique (i.e., hopefully, worldwide-unique) ID to your interface, so interface IDs would never collide. 

It's 16 byte value, that has type TGUID. 
    
    
          IMyInterface= interface
            ['{000000-1111-2222-3333-444444444444}']
          end;
         
          MyIterfaceGuid:TGUID = '{000000-1111-2222-3333-444444444444}'; //TGUID is structure, but initialization via string is allowed
    

Please note, that Pascal has very convenient feature - there is no need to declare TGUID values separately, as interface name itself can be used as TGUID value. 
    
    
        procedure SomeProcedure(AGUID: TGUID);
        begin
        ...
        end;
         
        SomeProcedure(IMyInterface); //Instead of MyIterfaceGuid, so declaring it isn't even needed
    

It allows virtual "casting" of interfaces at runtime. I.e. one interface can be casted to another interface, supported by their base class - it's done via "as" operator. You can also check, if interface supports casting to another interface - it's done via "is" operator. Please note! "Is" and "as" operators don't work without GUID! GUID is usually generated randomly. Yeah, it doesn't guarantee 100% uniqueness, but amount of bits in GUID is enough to make chance of collision very small. Not sure about Lazarus, but it's done via Ctrl+Shift+G shortcut in Delphi. 
    
    
        MyInterface1 := MyInterface2; //Simply assigns pointer to memory access compatible variable - does work only if TMyInterface1 is parent of TMyInterface2
        MyInterface1 := MyInterface2 as TMyInterface1; //Virtual interface casting. Works for any interfaces. Even if MyIterface2 is IUnknown.
    

Interfaces are reference counted the same way, as strings and dynamic arrays are. It's convenient way to handle lifetime of class, that implements them. So, you don't need to call Destroy method for you base class manually. It's destroyed, when last interface variable goes out of scope. Please note!!! Calling Destroy manually would cause access violation! 
    
    
        procedure SomeProc;
        var
          MyClass: TMyClass;
          MyInterface: TMyInterface;
        begin
          MyClass := TMyClass.Create;
          MyInterface := MyClass;
          //Don't call MyClass.Destroy at the end!!! It's destroyed automatically!!!
        end;
    

So, 3 methods are required for interfaces, declared in IUnknown/IInterface and implemented by TInterfacedObject. 

_AddRef and _Release - they handle reference counting and are managed automatically by compiler. Please note, that different kinds of procedure/function interface arguments can cause different reference count handing. For example passing interface as simple procedure argument will imply automatic calling of AddRef and Release for this argument inside procedure, so you don't need to do it manually. Specifying "const" prevents compiler from doing it. So be careful! Interface passed as "const" parameter won't cause automatic object destroying in case of SomeProcedure(TMyClass.Create) syntax, where interface isn't assigned to any variable and therefore has RefCount = 0. 

QueryInterface - handles virtual casting. 

Conclusion: interfaces can be even more convenient for programming, than classes. Because they support 

  * Multiple inheritance
  * Reference counting
  * Virtual casting at runtime



(Text was written by **Mr.Madguy**) 

## See also

  * [Accessing the Interfaces directly](<Accessing_the_Interfaces_directly.md> "Accessing the Interfaces directly")
  * [How To Use Interfaces](<How_To_Use_Interfaces.md> "How To Use Interfaces")
  * [Understanding Interfaces](<Understanding_Interfaces.md> "Understanding Interfaces")

---

_Source: [https://wiki.freepascal.org/Interfaces](https://web.archive.org/web/20250422002400/https://wiki.freepascal.org/Interfaces)_
