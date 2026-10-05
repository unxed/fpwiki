# Public

│ **[Deutsch (de)](</Public/de> "Public/de")** │  **English (en)** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The modifier **public** : 

  * is part of the programming of subroutines;
  * makes a subroutine public within the unit;
  * makes sense if the subroutine is not used outside the unit.



As long as the subroutine has not been declared in the interface section of the unit, it is not visible to other units.  
However, it will be possible to access the function subTest at the assembly-language level, by using its mangled name. 
    
    
    function subTest : string; Public;
      begin
        subTest := 'abc';
      end;
    

In the old syntax, the modifier `Public` was surrounded by square brackets: **[Public]** , but the upcoming atrributes syntax uses square brackets as well, so better not to use them for this modifier.   
  
The public modifier can also be followed by a name directive to specify the assembler name, as follows: 
    
    
    function subTest : string; Public name 'subtest';
      begin
        subTest := 'abc';
      end;
    

The assembler symbol as specified by the ’public name’ directive will be ’subtest’, in all lowercase letters.

---

_Source: [https://wiki.freepascal.org/Public](https://web.archive.org/web/20250219105207/https://wiki.freepascal.org/Public)_
