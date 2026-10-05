# Mod

│ **[Deutsch (de)](</Mod/de> "Mod/de")** │  **English (en)** │  **[français (fr)](</Mod/fr> "Mod/fr")** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
Mod (**mod** ulus) divides two numbers and returns only the remainder. For instance, the expression "a:= 13 mod 4;" would evaluate to 1 (a=1), while "b := 12 mod 4;" would evaluate to 0 (b=0). 

From the [language reference](<http://freepascal.org/docs-html/current/ref/refsu45.html#x148-17000012.8.1>): 

    "The sign of the result of a Mod operator is the same as the sign of the left side operand of the Mod operator. In fact, the Mod operator is equivalent to the following operation :"
    
    
     I mod J = I - (I div J) * J
    

For example "c:= -13 mod 4;" results in c = -1 and "c:= 10 mod -3;" results in c = 1. 

This is also what most other languages like C++ and Java do (see note) 

From version 3.1.1 FreePascal also supports the mod operator for floating point values when you include the math unit.  
The precision used is the highest precision available for the platform. for instance, the expression "a:= 12.9 mod 2.2;" would evaluate to 1.9. This is equivalent to I mod J = I - int(I / J) * J. 

The result is the same as the fmod function for the highest precision available for the platform. 

In older versions of FPC that support operator overloading you can add this feature yourself. Here's an example for double precision modulo. 
    
    
    operator mod(const a, b: double) c: double; inline;
    begin
      c:= a - b * Int(a / b);
    end;
    

  


## Note regarding Delphi compatibility

Delphi conforms to ISO 7185 Pascal instead of the de facto standard. This ISO standard states: 

    Evaluation of a term of the form x mod y is an error if y is less than or equal to zero; otherwise there is an integer k such that x mod y satisfies the following relation:
    
    
     0 <= x mod y = x - k * y < y.
    

That means that in Delphi "c:= 10 mod -3;" results in an error and an exception will be thrown at run-time.

---

_Source: [https://wiki.freepascal.org/Mod](https://web.archive.org/web/20240914010712/https://wiki.freepascal.org/Mod)_
