# Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL

│ **English (en)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN")  
---|---|---  
  
REPEAT...UNTIL 

The repeat .. until construct is termed a _post_ -test loop, because the controlling condition is tested _after_ each iteration of the loop. 

It has the following syntax: 
    
    
    repeat
      statement1;
      // statement2;
      // further statements...
    until BooleanExpression;
    

A `repeat` loop encloses its executed statements, which means they do not need to be further enclosed in a `begin ... end` block. Note that a repeat loop continues until its controlling Boolean expression is `True`; whereas the `while` loop continues until its Boolean expression is `False`. ``

``For instance, the following `repeat` loop executes at least once: ``

``
    
    
    repeat
      WriteLn(Node.Text);
      Node := GetNextNode;
    until not Assigned(Node);
    

``````

It assumes that Node is not Nil at the outset. If this assumption is incorrect, the code will fail, and the program may crash. 

A `while` loop is more defensive, since the needed check is performed before any loop statements are executed: 

``
    
    
    while Assigned(Node) do
      begin
        WriteLn(Node.Text);
        Node := GetNextNode;
      end;
    

``````

Use a `repeat` loop when the looping statement(s) must execute at least once, whatever the initial value of the controlling Boolean condition. 

It is not difficult to inadvertently write a Boolean expression controlling a repeat loop that never becomes True. This gives rise to an infinite or endless loop, and causes the program to hang. 

One programming style deliberately sets up an infinite loop, and inserts a `Break` or `Exit` instruction controlled by some condition evaluated in the middle of the loop to break out of the otherwise infinite loop: 

``
    
    
    repeat
      statement1;
      if Condition then
        Break;
      statement_that_might_affect_the_Condition;
    until False;
    

``````

Successful use of this style depends on Condition dependably becoming True, without exception, at some point. Otherwise you have built in an inescapable infinite loop. 

`` [ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN")  
---|---|---  
``

---

_Source: [https://wiki.freepascal.org/REPEAT..UNTIL](https://web.archive.org/web/20240526190556/https://wiki.freepascal.org/REPEAT..UNTIL)_
