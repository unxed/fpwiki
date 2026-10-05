# NativeInt

│ **[Deutsch (de)](</NativeInt/de> "NativeInt/de")** │  **English (en)** │  **[français (fr)](</NativeInt/fr> "NativeInt/fr")** │    
****

  
Back to the [data types](<Data_type.md> "Data type"). 

  
Range of values: processor-dependent. Examples for Intel and Intel compatible processors: 

  * 32 bit processors: -2,147,483,648 .. 2,147,483,647
  * 64 bit processors: -9,223,372,036,854,775,808 .. 9,223,372,036,854,775,807



Memory requirements: processor-dependent. Examples of Intel and Intel compatible processors: 

  * 32 bit processors: 4 bytes or 32 bits
  * 64 bit processors: 8 bytes or 64 bits



A data field of the data type **NativeInt** can hold negative and positive [integer](<Integer.md> "Integer") values. Assigning other values ​​leads to compiler error messages when the program is compiled and the compilation process is aborted. That is, the executable program is not created. 

Definition of a data field of the type **NativeInt** : 
    
    
      var 
        nint : NativeInt;
    

Examples of assigning valid values: 
    
    
      nint := -2147483648;
      nint := 2147483647;
    

Examples of assigning invalid values: 
    
    
      nint := '-2147483648';
      nint := '2147483647';
    

The difference between the two examples is that the upper example is the assignment of literals of the type [Integer](<Integer.md> "Integer"), while the assignment of the lower example is literals of the type [String](<String.md> "String").

---

_Source: [https://wiki.freepascal.org/NativeInt](https://web.archive.org/web/20250517155941/https://wiki.freepascal.org/NativeInt)_
