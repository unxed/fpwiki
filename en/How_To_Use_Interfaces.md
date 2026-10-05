# How To Use Interfaces

Copy the text below and it will demonstrate how to use [Interfaces](<Interfaces.md> "Interfaces") to write less code, avoid code repetition,..., this is a fully working program. The default interface in FPC is COM, which makes that once a class with an interface is instantiated as that interface it will automatically be released. Note if you change the interface type to CORBA it WILL leak. 

This example has two unrelated classes, but share the same COM interface. Since they are instantiated through that interface, destruction is automatic: 
    
    
    program interfacesygenerics;
    {$mode objfpc}{$H+}
    type
      ITestInterface = interface
        ['{3FB19775-F5FA-464C-B10C-D8137D742088}']
        procedure DoSomething;
        procedure DoItAll;
      end;
    
      TOneObject = class(TInterfacedObject,ITestInterface)
        procedure DoSomething;
        procedure DoItAll;
      end;
    
      TOtherObject = class(TInterfacedObject,ITestInterface)
        procedure DoSomething;
        procedure DoItAll;
      end;
     
      procedure TOneObject.DoSomething;
      begin
        Writeln('TOneObject DoSomething !');
      end;
    
      procedure TOneObject.DoItAll;
      begin
        Writeln('TOneObject DoItAll !');
      end;
      
      procedure TOtherObject.DoSomething;
      begin
        Writeln('TOtherObect DoSomething !');
      end;
    
      procedure TOtherObject.DoItAll;
      begin
        Writeln('TOtherObect DoItAll !');
      end;
      
    var
      I: ITestInterface = nil;
    begin
      Writeln('Using regular interfaces');
      I := TOneObject.Create as ITestInterface;
      if I <> nil then
        Writeln('Got interface OK. Calling it');
      I.DoSomething;
      I.DoItAll;
      I:=nil;  // releases TOneObject
      I := TOtherObject.Create as ITestInterface;
      if I <> nil then
        Writeln('Got interface OK. Calling it');
      I.DoSomething;
      I.DoItAll;
    end.
    

## See also

  * [Accessing the Interfaces directly](<Accessing_the_Interfaces_directly.md> "Accessing the Interfaces directly")
  * [Interfaces](<Interfaces.md> "Interfaces")
  * [Understanding Interfaces](<Understanding_Interfaces.md> "Understanding Interfaces")

---

_Source: [https://wiki.freepascal.org/How_To_Use_Interfaces](https://web.archive.org/web/20250326020921/https://wiki.freepascal.org/How_To_Use_Interfaces)_
