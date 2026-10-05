# Cppdecl

│ **[Deutsch (de)](</Cppdecl/de> "Cppdecl/de")** │  **English (en)** │    
****

Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The cppdecl modifier belongs to the calling conventions for internal and external subroutines. 

The cppdecl modifier is used to call a function according to the C++ calling convention. 

**Example 1:**
    
    
    function subTest : string; [cppdecl];
      begin
        subTest := 'abc';
      end;
    

**Example 2:**
    
    
      ...
      function funcTest(strTestdaten : Pchar) : LongWord;  cppdecl;  external 'Test.dll';
      ...

---

_Source: [https://wiki.freepascal.org/cppdecl](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/cppdecl)_
