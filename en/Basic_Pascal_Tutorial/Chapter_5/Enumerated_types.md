# Basic Pascal Tutorial/Chapter 5/Enumerated types

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_5/Enumerated_types/bg> "Basic Pascal Tutorial/Chapter 5/Enumerated types/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_5/Enumerated_types/fr> "Basic Pascal Tutorial/Chapter 5/Enumerated types/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_5/Enumerated_types/ja> "Basic Pascal Tutorial/Chapter 5/Enumerated types/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_5/Enumerated_types/zh_CN> "Basic Pascal Tutorial/Chapter 5/Enumerated types/zh CN")** │    
****

[ ◄ ](<../Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges")  
---|---|---  
  
5A - Enumerated Types (author: Tao Yue, state: unchanged) 

You can declare your own ordinal data types. You do this in the type section of your program: 
    
    
    type
     datatypeidentifier = typespecification;
    

One way to do it is by creating an enumerated type. An enumerated type specification has the syntax: 
    
    
    (identifier1, identifier2, ... identifiern)
    

For example, if you wanted to declare the months of the year, you would do a type: 
    
    
    type
      TMonthType = (January, February, March, April,May, June, July, August, September, October, November, December);
    

To make months starting from 1: 
    
    
    type
      TMonthType = (January=1, February, March, April,May, June, July, August, September, October, November, December);
    

You can assign more than one enumeration per value: 
    
    
    type
      TMonthType = (January=1, FirstMonth=1,February=2, SecondMonth=2...);
    

Or the example below will result in January=1, February=2, May=5,June=6, July=7: 
    
    
    type
      TMonthType = (January=1, February, May=5,June, July);
    

You can then declare a variable: 
    
    
    var
      Month : TMonthType;
    

You can assign any enumerated value to the variable: 
    
    
    Month := January; or Month := TMonthType(0);
    

All the ordinal functions are valid on the enumerated type. `ord(January) = 0`, and `ord(December) = 11`. Or if the Month variable should go to next calendaristic month a call to Succ(Month) can be made. Consider verifying with Month < High(TMonthType) that there is a next month. Get number of elements in enumeration with ord(High(TMonthType)) + 1. 

A few restrictions apply, though: enumerated types are internal to a program -- they can neither be read from nor written to a text file. You must read data in and convert it to an enumerated type. Also, the identifier used in the type (such as `January`) cannot be used in another type. 

One purpose of an enumerated type is to allow you, the programmer, to refer to meaningful names for data. In addition, enumerated types allow functions and procedures to be assured of a valid parameter, since only variables of the enumerated type can be passed in and the variable can only have one of the several enumerated values. 

[Write](<../../Write.md> "Write") and Writeln can be used to print a string representing the current value in an enumerated type variable. [WriteStr](<../../WriteStr.md> "WriteStr")(string-var,enum-var) can be used to place a string representation of the enum value into a string, or in one shot GetEnumName(TypeInfo(TMonthType), Ord(Month)) if typinfo unit is added in uses clause. 

To walk over the enumerated definition: 
    
    
    for i := Ord(Low(TMonthType)) to Ord(High(TMonthType)) do
      begin
         x:= GetEnumName(TypeInfo(TMonthType), Ord(i));
      end;
    

[ ◄ ](<../Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Enumerated_types](https://web.archive.org/web/20250401030423/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Enumerated_types)_
