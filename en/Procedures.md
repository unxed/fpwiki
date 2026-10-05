# Procedures

│ **English (en)** │

[ ◄ ](<Solution_3.md> "Solution 3") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Parameters.md> "Parameters")  
---|---|---  
  
4A - Procedures (author: Tao Yue, state: unchanged) 

A procedure is a subprogram. Subprograms help reduce the amount of redundancy in a program. Statements that are executed over and over again but not contained in a loop are often put into subprograms. 

Subprograms also facilitate top-down design. Top-down design is the tackling of a program from the most general to the most specific. For example, top down design for going from one room to another starts out as: 

  * Get out of first room
  * Go to second room
  * Go into second room



Then it is refined to 

  * Get out of first room 
    * Go to door
    * Open the door
    * Get out of door
    * Close door
  * ...



Just going to the door can be refined further: 

  * Get out of first room 
    * Go to door 
      * Get out of seat
      * Turn towards door
      * Walk until you almost bump into it



This, of course, can be further refined to say how much exercise should be given to your cardiac myofibrils, and how much adenosine diphosphate should be converted to adenosine triphosphate by fermentation or aerobic respiration. This may seem to be too detailed, but for computer programming, this is, in effect what you have to do. The computer can't understand general statements -- you must be specific. 

Main tasks should be contained in procedures, so in the main program, you don't have to worry about the details. This also makes for reusable code. You can just keep your procedures in one file and link that into your program. 

A procedure has the same basic format as a program: 
    
    
    procedure Name;
    
    const
      (* Constants *)
    
    var
      (* Variables *)
    
    begin
      (* Statements *)
    end;
    

There is a semicolon (not a period) at the end. 

To call the procedure from the main program, just use the name, like you would `writeln`. 
    
    
     Name;
    

Procedures are very often used to output data. It's that simple (until the next lesson, of course). 

[ ◄ ](<Solution_3.md> "Solution 3") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Parameters.md> "Parameters")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Procedures](https://web.archive.org/web/20221129165517/https://wiki.freepascal.org/Procedures)_
