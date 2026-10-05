# PUnicodeChar

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
The **PUnicodeChar** data type: 

  * has no size restriction.
  * is a pointer to a zero-terminated Unicode string with no length limit.



  
Definition of a data field of the PUnicodeChar data type: 
    
    
      var 
        p : PUnicodeChar;
    

Examples for the valid assignment of values: 
    
    
      p := 'This is a zero-terminated string.';
      p := IntToStr(45);
    

Examples of invalid assignment of values: 
    
    
      p := 45;
    

In the example above, the value to be transferred was not cast to the PUnicodeChar data type.

---

_Source: [https://wiki.freepascal.org/Punicodechar](https://web.archive.org/web/20250517160558/https://wiki.freepascal.org/Punicodechar)_
