# Variant

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
A **Variant** data type allows a single variable to store values of multiple types. This allows a statically-typed programming language like Free Pascal some of the flexibility normally found only in a dynamically-typed language like Python. 

A Variant data type takes more memory than simple data types.The memory requirements of a variant type for 32 bit compilation: 16 bytes or 128 bits. Memory requirements for a variant type in 64 bit compilation: 24 bytes, or 192-bits. Values of any simple data type can be stored in a variant variable. However, operations with the data type Variant are considerably slower than with statically typed simple data type variable. 

There are two useful functions for working with Variants in the Variants unit: 

  * The **VarType()** function checks which data type the variant holds.
  * The function **VarToStr()** converts the value of a variable of type Variant to a string representation.



Definition of a data field of the data type Variant. It is recommended that you use the [Variants unit](</index.php?title=Variants_unit&action=edit&redlink=1> "Variants unit \(page does not exist\)"): 
    
    
    Uses 
      Variants;
    var
      v: Variant;
    

Example usage: 
    
    
    uses 
      Variants;
    var
      v: Variant;
      s: string;
      i: integer;
    
    procedure ProcessVariant(v : Variant);
    begin
    
     // Example of how to determine the contents of a Variant type:
     Case varType(v) of
        varEmpty:
            Writeln('Empty');
        varNull:
            Writeln('Null');
        varSingle:
            Writeln('Datatype: Single');
        varDouble:
            Writeln('Datatype: Double');
        varDecimal:
            Writeln('Datatype: Decimal');
        varCurrency:
            Writeln('Datatype: Currency');
        varDate:
            Writeln('Datatype: Date');
        varOleStr:
            Writeln('Datatype: UnicodeString');
        varStrArg:
            Writeln('Datatype: COM-compatible string');
        varString:
            Writeln('Datatype: Pointer to a dynamic string');
        varDispatch:
            Writeln('Datatype: Pointer to an Automation object');
        varBoolean:
            Writeln('Datatype: Wordbool');
        varVariant:
            Writeln('Datatype: Variant');
        varUnknown:
            Writeln('Datatype: unknown');
        varShortInt:
            Writeln('Datatype: ShortInt');
        varSmallint:
            Writeln('Datatype: Smallint');
        varInteger:
            Writeln('Datatype: Integer');
        varInt64:
            Writeln('Datatype: Int64');
        varByte:
            Writeln('Datatype: Byte');
        varWord:
            Writeln('Datatype: Word');
        varLongWord:
            Writeln('Datatype: LongWord');
        varQWord:
            Writeln('Datatype: QWord');
        varError:
            Writeln('ERROR determining variant type');
     else
       Writeln('Unable to determine variant type');
     end;
    end;
    
    begin
    
      // Example 1:
      // Value assignment:
      v := 1;
    
      //Examples of permissible type assignments, auditions / conversions of data type:
    
      //Explicit data type conversions:
      s := VarToStr(v);
      Writeln('s:',s);
      s := AnsiString(v);
      Writeln('s:',s);
      i := Integer(v);
      Writeln('i:',i);
      //Implicit data type conversion:
      i := v;
      Writeln('i:',i);
    
      ProcessVariant(10);
      ProcessVariant(True);
      ProcessVariant('Pascal');
      ProcessVariant(10.244);
    
    end.
    

Outputs: 
    
    
    s:1
    s:1
    i:1
    i:1
    Datatype: ShortInt
    Datatype: Wordbool
    Datatype: Pointer to a dynamic string
    Datatype: Double

---

_Source: [https://wiki.freepascal.org/Variant](https://web.archive.org/web/20240618211000/https://wiki.freepascal.org/Variant)_
