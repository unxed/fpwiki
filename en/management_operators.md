# management operators

## Contents

  * 1 Management operators feature
  * 2 Initialize
  * 3 Finalize
  * 4 AddRef
  * 5 Copy
  * 6 Example of using Initialize and Finalize



## Management operators feature

From Free Pascal version 3.1.1 onwards, there is a new language feature called management operators for extended or advanced records. 

The new operators are: Initialize, Finalize, AddRef and Copy. 

These are a fairly unique feature, and are called "management operators" because: 

  * Each record, even non-managed or empty, that implements any management operators becomes a managed type.
  * They make it possible to implement new custom types with their own memory management, e.g: new string types, fast TValue implementations without hacks on the RTL, etc.



Management operators have no result type as opposed to normal operators, and work via a very simple VMT. 

Thanks to this, it is possible to combine management operators with all low-level RTL functions, such as InitializeArray / FinalizeArray / etc. 

Management operators can be used for many things: 

  * More granularly controlling the lifetimes of simple value types / primitives
  * Implementing "nullable" value types
  * Custom ARC implementations
  * A very fast RTTI.TValue implementation
  * As a replacement for manually-called Init/Done record methods like the popular "mORMot" library uses for many types (for example in SynCommons.TSynLocker).
  * Auto init/finit for pointers/classes/simple types or anything else we have in Pascal.
  * Much more



They work correctly in all possible ways with the RTL: 

  * New (Initialize).
  * Dispose (Finalize).
  * Initialize (Initialize).
  * Finalize (Finalize).
  * InitializeArray (Initialize).
  * FinalizeArray (Finalize).
  * SetLength (Initialize/Finalize).
  * Copy (AddRef).
  * RTTI.IsManaged.



Management operators, when implemented, are called implicitly at various times. For example: 

  * Global variables (Initialize/Finalize).
  * Local variables (Initialize/Finalize).
  * For fields inside records, objects or classes (Initialize/Finalize).
  * Variable assignment (Copy).
  * For parameters for routines - AddRef/Finalize/none - this depends on modifiers like var / constref / const.



## Initialize

The initialize operator is called directly after stack (or heap) memory allocation for a record happens. 

It allows for custom automatic initialization code. 
    
    
    program TestInitialize;
    
    {$if FPC_FULLVERSION < 30101}
    {$ERROR this demo needs version 3.1.1}
    {$endif}
    {$mode delphi}
    
    type
      PRec = ^TRec;
    
      TRec = record
        I: Integer;
        class operator Initialize(var aRec: TRec);
      end;
    
      class operator TRec.Initialize(var aRec: TRec);
      begin
        aRec.I := 0;
      end;
    
      procedure PrintTRec(r: PRec);
      begin
        WriteLn('Initialized TRec field i: ', r^.I);  // should always be zero, stack or heap
      end;
    
    var
      a, b: PRec;
    
    begin
      New(a);
      New(b); // standard "new" does not initialize, but now it does!
      PrintTRec(a);
      PrintTRec(b);
      Dispose(a);
      Dispose(b);
    end.
    

  


Warning: Do not use the Default() intrinsic inside the initialize operator because that causes initialize to be called twice. Use other methods to initialize a record, such as fillchar.  
An example on how to initialize properly without calling Default() or system.initialize is: 
    
    
    {$mode objfpc}{$modeswitch advancedrecords}
    type
      array10 = array[0..9] of byte;
      
      TTest = record
      fields:array10;
      class operator initialize(var value:TTest);
      end;
      
      class operator ttest.initialize(var value:TTest);
      begin
        // sizeof takes only the fields of the record in to account
        fillbyte(value,sizeof(TTest),0);// do not use default(), that triggers initialize twice
      end;
      
    procedure testme;
    var
      a,b:TTest;// stack
      i:integer;
    begin
      a.fields:=[1,2,3,4,5,6,7,8,9,10];
      a:=b;
      for i :=0 to 9 do
        write(a.fields[i]:3);
    end;
    
    begin
      testme;
    end.
    

## Finalize

Finalize is called when a record goes out of scope. 

It is useful for automatic custom finalization code. 
    
    
    program TestFinalize;
    
    {$if FPC_FULLVERSION < 30101}
    {$ERROR this demo needs version 3.1.1}
    {$endif}
    {$mode delphi}
    
    type
      PRec = ^TRec;
    
      TRec = record
        I: Integer;
        class operator Finalize(var aRec: TRec);
      end;
    
      class operator TRec.Finalize(var aRec: TRec);
      begin
        WriteLn('Just to let you know: I am finalizing..');
      end;
    
    var
      a, b: PRec;
      c: array of TRec;
    
    begin
      New(a);
      New(b);
      Dispose(a);
      Dispose(b);
      WriteLn('Just before program termination this will also be finalized');
      SetLength(c, 4);
    end.
    

## AddRef

AddRef is called after the contents of a record have been duplicated by a bitwise copy (for example **after** , not during, an assigment.) 

By itself it does not do any lifetime management, but you can use it to implement it. See also Copy. 
    
    
    program TestAddref;
    
    {$if FPC_FULLVERSION < 30101}
    {$ERROR this demo needs version 3.1.1}
    {$endif}
    {$mode delphi}
    
    uses
      SysUtils;
    
    type
      PRec = ^TRec;
    
      TRec = record
        I: Integer;
        class operator AddRef(var aRec: TRec);
      end;
    
      class operator TRec.AddRef(var aRec: TRec);
      begin
        WriteLn('Just to let you know: maybe you can do lifetime management here..');
      end;
    
    var
      a, b: array of TRec;
    
    begin
      SetLength(a, 4);
      b := Copy(a);
    end.
    

## Copy

The Copy operator, if implemented, is called instead of the default copy behavior. This operator is responsible for copying everything that's needed from the source to the target. 

This is very helpfull if your record contains e.g. a dynamic array.  
  
Consider the following program: 
    
    
    uses
      SysUtils;
    
    type
      TArr = array of integer;
      TRec = record
        Arr: TArr;
      end;
    
    implementation
    
    function ArrToString(Arr: TArr): String;
    var
      X: Integer;
    begin
      Result := '';
      for X in Arr do Result := Result + IntToStr(X) + ',';
      if Result <> '' then Delete(Result, Length(Result), 1);
      Result := '[' + Result + ']';
    end;
    
    var
      R1, R2: TRec;
    
    begin
      writeln('Normal record type with a dynamic array');
      R1.Arr := [1,2,3];
      R2 := R1;
      writeln('R1.Arr = ',ArrToString(R1.Arr));  //[1,2,3]
      writeln('R2.Arr = ',ArrToString(R2.Arr));  //[1,2,3]
      writeln('After R2.Arr[0] := 666');
      R2.Arr[0] := 666;  
      writeln('R1.Arr = ',ArrToString(R1.Arr)); //[666,2,3]
      writeln('R2.Arr = ',ArrToString(R2.Arr)); //[666,2,3]
    end.
    

This will output: 
    
    
    Normal record type with a dynamic array
    R1.Arr = [1,2,3]
    R2.Arr = [1,2,3]
    After R2.Arr[0] := 666
    R1.Arr = [666,2,3]
    R2.Arr = [666,2,3]
    

Notice that changing `R2.Arr` changes `R1.Arr` as well, since both the arrays point to the same memory location.  
When you do `R2 := R1`, you do not copy the contents of R1.Arr into R2, you copy the pointer that points to the first element of R1.Arr.  
If you want to avoid this, you could write a workaround like: 
    
    
    function CopyFrom(Src: TRec): TRec;
    begin
      Result.Arr := Copy(Src.Arr);
    end;
    

And then always use `R2 := CopyFrom(R1)` instead of `R2 := R1`.  
  
Or you can solve this problem using the Copy management operator: 
    
    
    {$modeswitch advancedrecords}
    uses
      SysUtils;
    
    type
    TArr = array of integer;
      TManagedRec = record
        Arr: TArr;
        class operator Copy(constref aSrc: TManagedRec; var aDst: TManagedRec);
      end;
    implementation
    { TMangedRec }
    class operator TManagedRec.Copy(constref aSrc: TManagedRec; var aDst: TManagedRec);
    begin
      aDst.Arr := Copy(aSrc.Arr);
    end;
    
    function ArrToString(Arr: TArr): String;
    var
      X: Integer;
    begin
      Result := '';
      for X in Arr do Result := Result + IntToStr(X) + ',';
      if Result <> '' then Delete(Result, Length(Result), 1);
      Result := '[' + Result + ']';
    end;
    
    var
      M1, M2: TManagedRec;
    begin
      writeln('Managed record type with a dynamic array and class operator Copy');
      M1.Arr := [1,2,3];
      M2 := M1;
      writeln('M1.Arr = ',ArrToString(M1.Arr)); //[1,2,3]
      writeln('M2.Arr = ',ArrToString(M2.Arr)); //[1,2,3]
      writeln('After M2.Arr[0] := 666');
      M2.Arr[0] := 666; 
      writeln('M1.Arr = ',ArrToString(M1.Arr)); //[1,2,3]
      writeln('M2.Arr = ',ArrToString(M2.Arr)); //[666,2,3]
    end.
    

This will output: 
    
    
    Managed record type with a dynamic array and class operator Copy
    M1.Arr = [1,2,3]
    M2.Arr = [1,2,3]
    After M2.Arr[0] := 666
    M1.Arr = [1,2,3]
    M2.Arr = [666,2,3]
    

There is also a (more complex) example in [[1]](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/test/tmoperator8.pp>) within the FPC sources. 

## Example of using Initialize and Finalize
    
    
    unit UResourceHandlers;
    
    {$if FPC_FULLVERSION < 30101}
    {$ERROR this demo needs version 3.1.1}
    {$endif}
    {$mode delphi}
    
    interface
    
    uses
      Classes, SysUtils;
    
    type
    
      { TObjectHandler }
    
      TObjectHandler = record
        obj: TObject;
        class operator Initialize(var hdl: TObjectHandler);
        class operator Finalize(var hdl: TObjectHandler);
      end;
    
    implementation
    
    { TObjectHandler }
    
    class operator TObjectHandler.Initialize(var hdl: TObjectHandler);
    begin
      hdl.obj := nil;
    end;
    
    class operator TObjectHandler.finalize(var hdl: TObjectHandler);
    begin
      FreeAndNil(hdl.obj);
    end;
    
    end.
    

How to use it 
    
    
    procedure ExtractionResultTests.ObjectHandlerTest;
    var
      a: TRow;
      ah: TObjectHandler;
    begin
      a := TRow.Create;
      ah.obj := a;
    end;
    

In this case the destructor of the TRow object is called when the handler goes out of scope. The same idea could be used for other resources like TMutex / TCriticalSection / anything else along those lines.

---

_Source: [https://wiki.freepascal.org/management_operators](https://web.archive.org/web/20250523133659/https://wiki.freepascal.org/management_operators)_
