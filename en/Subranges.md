# Basic Pascal Tutorial/Chapter 5/Subranges

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_5/Subranges/bg> "Basic Pascal Tutorial/Chapter 5/Subranges/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_5/Subranges/fr> "Basic Pascal Tutorial/Chapter 5/Subranges/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_5/Subranges/ja> "Basic Pascal Tutorial/Chapter 5/Subranges/ja")** │  **[中文（中国大陆）‎ (zh_CN)](</Basic_Pascal_Tutorial/Chapter_5/Subranges/zh_CN> "Basic Pascal Tutorial/Chapter 5/Subranges/zh CN")** │    
****

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
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

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Subranges](https://web.archive.org/web/20230202122116/https://wiki.freepascal.org/Subranges)_
