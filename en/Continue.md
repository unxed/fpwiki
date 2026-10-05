# Continue

│ **English (en)** │

The `continue` (pseudo) [routine](<Routine.md> "Routine") effectively skips the remainder of the [loop](<Loops.md> "Loops") body for one iteration, thus jumping back (or forward) to the loop head. It is a non-standardized extension. 

`Continue`, with its special meaning of terminating one iteration, can only be written _within_ loops. It is not a reserved word, therefore you _could_ use it as an [identifier](<Identifier.md> "Identifier"), but still access the special routine by writing its _fully qualified_ identifier `system.continue`. In [`{$mode MacPas}`](<Mode_MacPas.md> "Mode MacPas") the `cycle` statement is an alias for `continue`. 

## application

The use of `continue` is usually disfavored since it “delegitimizes” the loop’s condition expression. If the number of iterations can be predicted _before_ the loop starts, it is even considered bad style to use `continue`. Instead it is highly recommended to improve the algorithm. Nevertheless, in the case of _unpredictable_ data, and thus an _unpredictable_ number of (complete) iterations, `continue` is generally acceptable. 
    
    
    program continueDemo(input, output, stdErr);
    var
      i, sum: integer;
    begin
      sum := 0;
      writeLn('Enter some positive numbers, line by line:');
      
      //       ┌─────────────────────────────────┐
      //       ↓ next iteration: check condition │
      while not eof do                        // │
        begin                                 // │
          readLn(i);                          // │
                                              // │
          if i < 1 then                       // │
            begin                             // │
              writeLn('Hey! Stay positive!'); // │
              continue; // → ────────────────────┘
            end;
          
          {$push}
          {$rangeChecks off}
          sum := sum + i;
          {$pop}
          if sum < 0 then
            begin
              writeLn('I can’t handle that much positivity.');
              halt(1);
            end;
        end;
      
      writeLn('The sum of all your positive numbers is ', sum, '.');
      writeLn('I like that positivity.');
    end.
    

After and at `continue` (or `cycle` in `{$mode MacPas}`) the program jumps back, or in the case of [`repeat … until`](<Repeat.md> "Repeat") forward, to the loop’s condition. The respective condition must be satisfied again before another iteration occurs. 

The use of `continue` can in practice always be avoided by proper [`if` statements](<If.md> "If"). It does not increase the power of Pascal. The above _demonstration_ example would be usually written _without_ a `continue`. However, if there would be _many_ or several _nested_ `if` statements, `continue` is preferred to retain _some_ degree of readability. 

## see also

  * [`continue`](<https://www.freepascal.org/docs-html/rtl/system/continue.html>)
  * [`break`](<Break.md> "Break")

---

_Source: [https://wiki.freepascal.org/Continue](https://web.archive.org/web/20250123171742/https://wiki.freepascal.org/Continue)_
