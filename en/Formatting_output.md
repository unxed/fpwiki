# Basic Pascal Tutorial/Chapter 2/Formatting output

│ **English (en)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files")  
---|---|---  
  
2C - Formatting Output (author: Tao Yue, state: unchanged) 

Formatting output is quite easy. For each identifier or literal value on the argument list, use: 
    
    
    Value : field_width
    

The output is right-justified in a field of the specified integer width. If the width is not long enough for the data, the width specification will be ignored and the data will be displayed in its entirety (except for real values — see below). 

Suppose we had: 
    
    
    write ('Hi':10, 5:4, 5673:2);
    

The output would be (that's eight spaces before the Hi and three spaces after): 
    
    
            Hi   55673
    

For real values, you can use the aforementioned syntax to display scientific notation in a specified field width, or you can convert to fixed decimal-point notation with: 
    
    
    Value : field_width : decimal_field_width
    

The field width is the total field width, including the decimal part. The whole number part is always displayed fully, so if you have not allocated enough space, it will be displayed anyway. However, if the number of decimal digits exceeds the specified decimal field width, the output will be displayed rounded to the specified number of places (though the variable itself is not changed). 
    
    
    write (573549.56792:20:2);
    

would look like (with 11 spaces in front): 
    
    
               573549.57
    

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Formatting_output](https://web.archive.org/web/20241117104605/https://wiki.freepascal.org/Formatting_output)_
