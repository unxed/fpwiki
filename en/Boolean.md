# Boolean

│ **English (en)** │  **[русский (ru)](<../ru/Boolean.md>)** │

## Overview

The [simple type](<simple_type.md> "simple type") **boolean** is a logical [data type](<Data_type.md> "Data type"). Data of type boolean has one of only two values, either [`true`](<True.md> "True") or [`false`](<False.md> "False"). A boolean [variable](<Variable.md> "Variable") is 1 byte in size. 

The `true` value can be assigned directly to a boolean variable or from the result of a comparison or test that was successful (`true`). Similarly the `false` value can be assigned directly or from the result of a comparison or test that was not successful (`false`). The [Write](<Write.md> "Write")() and Writeln() [procedures](<Procedure.md> "Procedure") will print a [string](<String.md> "String") that corresponds to the value of a boolean variable (either "TRUE" or "FALSE"). A Boolean variable can be used as the expression in an [if statement](<If.md> "If"). The WriteStr() procedure can be used to store a string literal representing a boolean variable's value in a string variable. 
    
    
    program booleantest;
    var
        tooLarge   : Boolean = false;
        boolString : ShortString;
    begin
      Writeln(tooLarge);
      tooLarge := (0 = 0);
      Writeln(tooLarge);
      tooLarge := (3 > 5);
      Writeln(tooLarge);
      tooLarge := true;
      Writeln(tooLarge);
      if tooLarge then
        Writeln('tooLarge is true')
      else
        Writeln('tooLarge is false');
      WriteStr(boolString,tooLarge);
      Writeln(boolString);
    end.
    

Outputs:  

    
    
    **FALSE**
    **TRUE**
    **FALSE**
    **TRUE**
    **tooLarge is true**
    **TRUE**
    

## See also

  * [Boolean Expressions](<Boolean_Expressions.md> "Boolean Expressions")



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  `boolean` [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Boolean](https://web.archive.org/web/20241207120437/https://wiki.freepascal.org/Boolean)_
