# PWideChar

│ **English (en)** │    
****

  
Back to [data types](<Data_type.md> "Data type"). 

  
The **PWideChar** data type: 

  * has no size restriction;
  * is a pointer to a zero-terminated wide string with no length limit.



Definition of a data field of data type PWideChar: 
    
    
      var 
        p : PWideChar;
    

Examples for the valid assignment of values: 
    
    
      p := 'This is a zero-terminated string.' ;
      p := IntToStr(45);
    

Examples of invalid assignment of values: 
    
    
      p := 45;
    

In the example above, the value to be transferred was not cast to the PWideChar data type.

---

_Source: [https://wiki.freepascal.org/Pwidechar](https://web.archive.org/web/20250517164143/https://wiki.freepascal.org/Pwidechar)_
