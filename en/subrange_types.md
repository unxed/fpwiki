# subrange types

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
Range of values: Corresponds to the definition defined by yourself 

Storage requirements: Corresponds to the underlying basic data type 

A **subrange type** : 

  * is a subrange of ​​a basic data type;
  * can only include values ​​from the range of values ​​that were defined when it was defined;



Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

There are two ways to define subrange types. 

Variant 1: 
    
    
    var
      // Defines an integer subrange type
      // in the range of 10 to 123
      tbNumber1 : 10 .. 123;
    
      // Defines a character subrange type
      // in the range of A to Z (all uppercase)
      tbLetter1 : 'A' .. 'Z';
    

Variant 2: 
    
    
    Type
      // Defines an integer subrange type
      // in the range of 10 to 123
      TNumber2 = 10 .. 123;
    
      // Defines a character subrange type
      // in the range of A to Z (all uppercase)
      TLetter2 = 'A' .. 'Z';
    
    var
      tbNumber2 : TNumber2 ;
      tbLetter2 : TLetter2 ;
    

Examples of assigning valid values: 
    
    
      tbNumber1 := 10;
      tbNumber1 := 123;
      tbNumber2 := 10;
      tbNumber2 := 123;
      tbLetter1 := 'F';
      tbLetter2 := 'F';
    

Examples of assigning invalid values: 
    
    
      // the values are outside the subrange
      tbNumber1 := 9;
      tbNumber2 := 124;
      tbLetter1 := 'f';
      tbLetter2 := 'f';
      // the values ​​are string literals
      tbNumber1 := '10';
      tbNumber1 := '123';
      tbNumber2 := '10';
      tbNumber2 := '123';

---

_Source: [https://wiki.freepascal.org/subrange_types](https://web.archive.org/web/20240304152202/https://wiki.freepascal.org/subrange_types)_
