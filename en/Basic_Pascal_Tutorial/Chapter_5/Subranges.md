# Basic Pascal Tutorial/Chapter 5/Subranges

│ **English (en)** │

[ ◄ ](<Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
---|---|---  
  
5B - Subranges (author: Tao Yue, state: unchanged) 

A _subrange_ type is defined in terms of another ordinal data type. The type specification is: 
    
    
    lowest_value .. highest_value
    

where `lowest_value < highest_value` and the two values are both in the range of another ordinal data type. 

For example, you may want to declare the days of the week as well as the work week: 
    
    
    type
      DaysOfWeek = (Sunday, Monday, Tuesday, Wednesday,
                    Thursday, Friday, Saturday);
      DaysOfWorkWeek = Monday..Friday;
    

You can also use subranges for built-in ordinal types such as `char` and `integer`. 

[ ◄ ](<Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Subranges](https://web.archive.org/web/20250331225742/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Subranges)_
