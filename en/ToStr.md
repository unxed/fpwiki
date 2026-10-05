# About

This is helper function to convert any value to a string representation, including records and arrays, in the same spirit of 'print' functions found in scripting languages. Its usefull for logging and debugging. 
    
    
    function ToStr(var AX; ATypeInfo: PTypeInfo): AnsiString;overload;

Parametes: 

  * AX: The variable to convert to string. 
  * ATypeInfo: The type information returned by 'TypeInfo' compiler intrinsic. 



# Download

You can get the code at <https://github.com/correaelias/TypeUtils>

# Examples

Simple example. 
    
    
    program test;
    uses TypeUtils;
    var
      Arr: array[1 .. 3] of Integer = (1, 2, 3);
    begin
      WriteLn(ToStr(Arr, TypeInfo(Arr)));
    end.

Example converts array of record. 
    
    
    program Test;
     
    uses TypeUtils, SysUtils;
     
    type
      TEmployee = record
        FName: ShortString;
        FId: LongInt;
        FSalary: Currency;
      end;
     
    var
      Employees: array of TEmployee;
      I: LongInt;
    begin
      SetLength(Employees, 10);
     
      for I := 0 to High(Employees) do
      begin
        with Employees[I] do
        begin
          FName := 'John Doe ' + I.ToString;
          FId := I;
          FSalary := 2000.25 * I;
        end;
      end;
     
      WriteLn(ToStr(Employees, TypeInfo(Employees)));
    end.

It shows: 
    
    
    [('John Doe 0', 0, 0), ('John Doe 1', 1, 2000.25), ('John Doe 2', 2, 4000.5), 
    ('John Doe 3', 3, 6000.75), ('John Doe 4', 4, 8001), ('John Doe 5', 5, 10001.25), 
    ('John Doe 6', 6, 12001.5), ('John Doe 7', 7, 14001.75), ('John Doe 8', 8, 16002), 
    ('John Doe 9', 9, 18002.25)]

---

_Source: [https://wiki.freepascal.org/ToStr](https://web.archive.org/web/20190523045137/https://wiki.freepascal.org/ToStr)_
