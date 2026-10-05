# To

│ **English (en)** │  **[русский (ru)](<../ru/To.md>)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
[Keyword](<Keyword.md> "Keyword") used to indicate the final value of the control value in a [For](<For.md> "For") loop, and that the loop is to increment the control variable by 1 on each loop. The value specified by TO should be greater than the initial value of the for loop. 

## Contents

  * 1 For to do
    * 1.1 Basic example
    * 1.2 Start and end value same
    * 1.3 Start value bigger than end value
  * 2 Read more



## [For](<For.md> "For") to [do](<Do.md> "Do")
    
    
    var i : integer;
    begin
     for i := 1 to 10000 do
       begin
         //...
       end;
    end;
    

The for to allow to execute code repeatedly for a fixed number of times. 

### Basic example
    
    
    var
      loopValue, startValue, endValue, resultValue: integer;
    begin
      startValue := 10;
      endValue := 11;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    end;
    

The loop execute two times and variable resultValue value is 21. 

### Start and end value same
    
    
    var
      loopValue, startValue, endValue, resultValue: integer;
    begin
      startValue := 10;
      endValue := 10;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    end;
    

The loop execute one time and variable resultValue value is 10. 

### Start value bigger than end value
    
    
    var
      loopValue, startNumber, endNumber, resultValue: integer;
    begin
      startValue := 10;
      endValue := 9;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    end;
    

The loop execute none time and variable resultValue value is 0. 

Note that the value of the "for loop variable" is undefined after a loop has completed or if a loop is not executed at all. However, if the loop was terminated prematurely with an exception or a break (or even a goto statement), the loop variable retains the value it had when the loop was exited. 

## Read more

  * [Downto](<Downto.md> "Downto")

---

_Source: [https://wiki.freepascal.org/To](https://web.archive.org/web/20240920204117/https://wiki.freepascal.org/To)_
