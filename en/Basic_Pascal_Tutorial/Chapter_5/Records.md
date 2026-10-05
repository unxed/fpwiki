# Basic Pascal Tutorial/Chapter 5/Records

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_5/Records/bg> "Basic Pascal Tutorial/Chapter 5/Records/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_5/Records/fr> "Basic Pascal Tutorial/Chapter 5/Records/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_5/Records/ja> "Basic Pascal Tutorial/Chapter 5/Records/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_5/Records/zh_CN> "Basic Pascal Tutorial/Chapter 5/Records/zh CN")** │    
****

[ ◄ ](<Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers")  
---|---|---  
  
5E - Records (author: Tao Yue, state: unchanged) 

A record allows you to keep related data items in one structure. If you want information about a person, you may want to know name, age, city, state, and zip. 

To declare a record, you'd use: 
    
    
    TYPE
      TypeName = record
        identifierlist1 : datatype1;
        ...
        identifierlistn : datatypen;
      end;
    

For example: 
    
    
    type
      InfoType = record
        Name : string;
        Age : integer;
        City, State : String;
        Zip : integer;
      end;
    

Each of the identifiers `Name, Age, City, State`, and `Zip` are referred to as fields. You access a field within a variable by: 
    
    
     VariableIdentifier.FieldIdentifier
    

A period separates the variable and the field name. 

There's a very useful statement for dealing with records. If you are going to be using one record variable for a long time and don't feel like typing the variable name over and over, you can strip off the variable name and use only field identifiers. You do this by: 
    
    
    WITH RecordVariable DO
    BEGIN
      ...
    END;
    

Example: 
    
    
    with Info do
    begin
      Age := 18;
      ZIP := 90210;
    end;
    

[ ◄ ](<Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Records](https://web.archive.org/web/20250401082958/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Records)_
