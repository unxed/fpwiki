# Comparison of approaches for rounding to an integer

│ **English (en)** │

Real numbers can be converted to integers in many ways. Here's a comparison some of them. 
    
    
    program ComparisonOfApproachesForRoundingToInteger(input,output);
    // Comparison of approaches for rounding to an integer
    
    uses  SysUtils,Math;
    
    function RoundISO7185(x:real):integer;
    // If x is positive or zero, round(x) shall be equivalent to trunc(x+0 .5) ;
    // otherwise, round(x) shall be equivalent to trunc(x-0.5)
    begin
      If x >= 0 then result := trunc(x+0.5)
        else result := trunc(x-0.5);
    end;
    
    var
      r1,r2,r3,r4:real;
      fmt : string;
    begin
      r1 := 1.3;
      r2 := 1.6;
      r3 := 3.5;
      r4 := 4.5;
      fmt := '%10s%10s%10s%10s%10s%10s%10s';
      WriteLn(format(fmt,['Value','Write:x:0','Trunc','Round','ISO7185','Floor','Ceil']));
      fmt := '%10d%10d%10d%10d%10d';
      write (r1:10:1,r1:10:0);
      WriteLn(format(fmt,[Trunc(r1),Round(r1),RoundISO7185(r1),floor(r1),ceil(r1)]));
      write (r2:10:1,r2:10:0);
      WriteLn(format(fmt,[Trunc(r2),Round(r2),RoundISO7185(r2),floor(r2),ceil(r2)]));
      write (r3:10:1,r3:10:0);
      WriteLn(format(fmt,[Trunc(r3),Round(r3),RoundISO7185(r3),floor(r3),ceil(r3)]));
      write (r4:10:1,r4:10:0);
      WriteLn(format(fmt,[Trunc(r4),Round(r4),RoundISO7185(r4),floor(r4),ceil(r4)]));
      r1 := r1 *-1;
      r2 := r2 *-1;
      r3 := r3 *-1;
      r4 := r4 *-1;
      write (r1:10:1,r1:10:0);
      WriteLn(format(fmt,[Trunc(r1),Round(r1),RoundISO7185(r1),floor(r1),ceil(r1)]));
      write (r2:10:1,r2:10:0);
      WriteLn(format(fmt,[Trunc(r2),Round(r2),RoundISO7185(r2),floor(r2),ceil(r2)]));
      write (r3:10:1,r3:10:0);
      WriteLn(format(fmt,[Trunc(r3),Round(r3),RoundISO7185(r3),floor(r3),ceil(r3)]));
      write (r4:10:1,r4:10:0);
      WriteLn(format(fmt,[Trunc(r4),Round(r4),RoundISO7185(r4),floor(r4),ceil(r4)]));
      Readln;
    end.
    
    
    
     Output:
     Value Write:x:0     Trunc     Round   ISO7185     Floor      Ceil
       1.3         1         1         1         1         1         2
       1.6         2         1         2         2         1         2
       3.5         4         3         4         4         3         4
       4.5         5         4         4         5         4         5
      -1.3        -1        -1        -1        -1        -2        -1
      -1.6        -2        -1        -2        -2        -2        -1
      -3.5        -4        -3        -4        -4        -4        -3
      -4.5        -5        -4        -4        -5        -5        -4
    

  


## See also

  * [`round`](<Round.md> "Round")
  * [`trunc`](<Trunc.md> "Trunc") \- round towards zero
  * [`math.ceil`](<https://www.freepascal.org/docs-html/rtl/math/ceil.html>) \- round up
  * [`math.floor`](<https://www.freepascal.org/docs-html/rtl/math/floor.html>) \- round down


  * [Format_function](<Format_function.md> "Format function")

---

_Source: [https://wiki.freepascal.org/Comparison_of_approaches_for_rounding_to_an_integer](https://web.archive.org/web/20230328024657/https://wiki.freepascal.org/Comparison_of_approaches_for_rounding_to_an_integer)_
