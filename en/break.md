# Break

│ **English (en)** │


The reserved word **break** is one of the [loop commands](</index.php?title=Loops&action=edit&redlink=1> "Loops \(page does not exist\)")  
The command is used to exit a loop before its planned end.  
The **break** command can only be used within loops.  
  
Example:  

    
    
    var
      intI: Integer;
      intA: Integer = 50;
    begin
      for intI := 20 to 200 do
      begin
          ...
          if intI = intA then break; // If the condition is satisfied, the loop is terminated
          ...
      end;
    end;

  


## See also

  * [Reserved words](<Reserved_words.md> "Reserved words")

---

_Source: [https://wiki.freepascal.org/break](https://web.archive.org/web/20170216110503/https://wiki.freepascal.org/break)_
