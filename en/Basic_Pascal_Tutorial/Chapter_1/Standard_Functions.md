# Basic Pascal Tutorial/Chapter 1/Standard Functions

│ **English (en)** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions.md>)** │

[ ◄ ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation")  
---|---|---  
  
1F - Standard Functions (author: Tao Yue, state: unchanged) 

Pascal has several standard mathematical functions that you can utilize. For example, to find the value of sin of pi radians: 
    
    
    value := sin (3.141592653589793238462643383);
    

Note that the sin function operates on angular measure stated in radians, as do all the trigonometric functions. If everything goes well, value should become 0. 

Functions are called by using the function name followed by the argument(s) in parentheses. Standard Pascal functions include: 

Function | Description | Argument type | Return type   
---|---|---|---  
abs | absolute value | real or integer | same as argument   
arctan | arctan in radians | real or integer | real   
cos | cosine of a radian measure | real or integer | real   
exp | e to the given power | real or integer | real   
ln | natural logarithm | real or integer | real   
round | round to nearest integer | real | integer   
sin | sin of a radian measure | real or integer | real   
sqr | square (power 2) | real or integer | same as argument   
sqrt | square root (power 1/2) | real or integer | real   
trunc | truncate (round down) | real or integer | integer   
  
For ordinal data types (integer or char), where the allowable values have a distinct predecessor and successor, you can use these functions: 

Function | Description | Argument type | Return type   
---|---|---|---  
chr | character with given ASCII value | integer | char   
ord | ordinal value | integer or char | integer   
pred | predecessor | integer or char | same as argument type   
succ | successor | integer or char | same as argument type   
  
Real is not an ordinal data type! That's because it has no distinct successor or predecessor. What is the successor of 56.0? Is it 56.1, 56.01, 56.001, 56.0001? 

However, for an integer 56, there is a distinct predecessor — 55 — and a distinct successor — 57. 

The same is true of characters: 
    
    
     'b'
     Successor: 'c'
     Predecessor: 'a'
    

The above is not an exhaustive list, as modern Pascal compilers include thousands of functions for all sorts of purposes. Check your compiler documentation for more. 

[ ◄ ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions)_
