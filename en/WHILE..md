# Basic Pascal Tutorial/Chapter 3/WHILE..DO

│ **English (en)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL")  
---|---|---  
  
## While ... DO loops

3Db - WHILE..DO (author: Tao Yue, state: unchanged) 

The pretest loop has the following format: 
    
    
    while BooleanExpression do
      statement;
    

The loop continues to execute until the Boolean expression becomes `FALSE`. In the body of the loop, you must somehow affect the Boolean expression by changing one of the variables used in it. Otherwise, an infinite loop will result: 
    
    
    a := 5;
    while a < 6 do
      writeln (a);
    

Remedy this situation by changing the variable's value: 
    
    
    a := 5;
    while a < 6 do
    begin
      writeln (a);
      a := a + 1
    end;
    

The `WHILE ... DO` loop is called a pretest loop because the condition is tested before the body of the loop executes. So if the condition starts out as `FALSE`, the body of the `while` loop never executes. 

## See also

  * [FOR ...DO loops](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")
  * [Repeat... Until loops](<Until.md> "Until")
  * [For... in loops](<for-in_loop.md> "for-in loop")

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL")  
---|---|---

---

_Source: [https://wiki.freepascal.org/WHILE..DO](https://web.archive.org/web/20250325080012/https://wiki.freepascal.org/WHILE..DO)_
