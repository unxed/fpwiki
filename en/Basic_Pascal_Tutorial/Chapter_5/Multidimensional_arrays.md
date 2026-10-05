# Basic Pascal Tutorial/Chapter 5/Multidimensional arrays

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays/bg> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays/fr> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays/ja> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays/zh_CN> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays/zh CN")** │    
****

[ ◄ ](<1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Records.md> "Basic Pascal Tutorial/Chapter 5/Records")  
---|---|---  
  
5D - Multidimensional Arrays (author: Tao Yue, state: unchanged) 

You can have arrays in multiple dimensions: 
    
    
    type
      datatype = array [enum_type1, enum_type2] of datatype;
    

The comma separates the dimensions, and referring to the array would be done with: 
    
    
    a [5, 3]
    

Two-dimensional arrays are useful for programming board games. A tic tac toe board could have these type and variable declarations: 
    
    
    type
      StatusType = (X, O, Blank);
      BoardType = array[1..3,1..3] of StatusType;
    var
      Board : BoardType;
    

You could initialize the board with: 
    
    
    for count1 := 1 to 3 do
      for count2 := 1 to 3 do
        Board[count1, count2] := Blank;
    

You can, of course, use three- or higher-dimensional arrays. 

[ ◄ ](<1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Records.md> "Basic Pascal Tutorial/Chapter 5/Records")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays](https://web.archive.org/web/20250401090433/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays)_
