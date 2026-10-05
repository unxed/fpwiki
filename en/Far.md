# Far

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **far** : 

  * belongs to 16 bit programming (DOS, Windows 3.x);
  * allows subroutines to be started in memory areas beyond the 64KB limit;
  * allows DLLs to be jumped to in memory areas beyond the 64KB limit;
  * became obsolete with 32-bit programming.



  
Examples: 
    
    
    //procedures
    procedure subTest; far;
    begin
    end;
    
    function fHandler: boolean; far;
    begin
      fHandler:= true;
    end;
    
    //types
    type
      PFarChar = ^char; far;
    
    //vars
    var
      prcf: Procedure; far;

---

_Source: [https://wiki.freepascal.org/Far](https://web.archive.org/web/20250324221740/https://wiki.freepascal.org/Far)_
