# Defensive programming techniques

## Contents

  * 1 How to catch and prevent Range Errors
    * 1.1 The bug
    * 1.2 Turn range checks on
    * 1.3 Declare ranges and use low() and high()
    * 1.4 Use for … in … do
    * 1.5 Bonus: Using a range? You may want a set, too!
    * 1.6 Conclusion
  * 2 How to prevent Overflow Errors, catch them and even misuse Overflow
    * 2.1 The bug
  * 3 How to prevent Input and Output Errors (and how to catch them…)
  * 4 How to use meaningful Assertions
  * 5 To serve and protect: the story of try..finally
  * 6 Do you know your String Type? Really?



## How to catch and prevent Range Errors

Range errors are easy to introduce and sometimes hard to find. They can exist for years without being noticed. I have seen production units where range checks were deliberately turned off by adding [`{$R-}`](</index.php?title=sRangechecks&action=edit&redlink=1> "sRangechecks \(page does not exist\)") in a unit and nobody noticed this for years. When I compiled the code during a review with range checks on `{$R+}` I found a huge bug that potentially could crash a vital piece of software. Mind you, there can be reasons to turn range checks off but never for a whole unit or a whole program, unless it is fully tested for a release. 

I will show you how to find range errors, how to debug them and how to prevent them. Defensive programming is important with ranges. 

### The bug

Let's introduce you to a small piece of code with a range bug. 
    
    
    program dtp_1a;
    {$mode objfpc}
    var
      anArray:array[0..9] of integer; // ten elements
      i:integer;
    begin
      for i := 1 to 10 do
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    

This code compiles without error and on some systems it even runs! without error: 
    
    
    $ fpc -glh dtp_1a.pas
    

Note `-glh` obtains line info in case of an error. Running the program yields: 
    
    
    dtp
      1  2  3  4  5  6  7  8  9 10
    

That may seem right, but is wrong! It could also SEGFAULT or worse … Which you know if you have spotted the bug. 

### Turn range checks on

Now let's see what happens when we compile with range checks: 
    
    
    program dtp_1b;
    {$mode objfpc}
    {$R+}
    var
      anArray:array[0..9] of integer; // ten elements
      i:integer;
    begin
      for i := 1 to 10 do
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    
    
    
    $ fpc -glh dtp_1b.pas
    

You may not expect this code to compile if you discovered the error, but unfortunately it compiles without error or warning. The fun starts when you run it: 
    
    
    dtp
      1  2  3  4  5  6  7  8  9Runtime error 201 at $000101B8
      $000101B8  main,  line 10 of dtp.pas
      $00010124
    
    No heap dump by heaptrc unit
    Exitcode = 201
    

Ok, we found a bug at line 10 of our program and 201 means range error. Useful, but not very, since we had to run the program to make it crash. Hardly acceptable. Furthermore not every programmer sees what the bug is since it occurs in a loop. Which is wrong? `i` or `anArray[i]` or both? And when it goes wrong is also not obvious to all. 

Both the FP [textmode IDE](<Textmode_IDE.md> "Textmode IDE") and [Lazarus](<Lazarus.md> "Lazarus") are able to debug our program, so we set a breakpoint on line 10 and press `F9` a couple of times. Note I also set a watch on `i`. 

[![dtp 1b.png](https://wiki.freepascal.org/images/b/ba/dtp_1b.png)](</File:dtp_1b.png>)

So I pressed `F9` ten times and hey presto, the error occurs when `i` becomes 10 and we try to access `anArray[10]`. But that means the actual error is on line 9. We are over-indexing because the array is from `0..9`, not from `1` to `10`. 

**Bug found and cause of bug found.** But not fixed, remember we found it at runtime, not at compile time. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** To summarize, turning range checks on finds range errors at run time, but not always at compile time.

### Declare ranges and use `low()` and `high()`

Object Pascal has a nice feature that is a bit underused, but is very useful in our case, **ranges**. Basically, by declaring a range we can find range errors at compile time and that is exactly what we want. 
    
    
    program dtp_1c;
    {$mode objfpc}{$R+}
    var
      anArray: array[0..9] of integer; // ten elements
      i: 0..9; // range of 10 elements, same as array
    begin
      for i := 1 to 10 do
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    

By declaring a range instead of an integer we probably also immediately see the discrepancy in the for to code, but that is not always the case, so let's try to compile the code: 

[![dtp 1c.png](https://wiki.freepascal.org/images/6/68/dtp_1c.png)](</File:dtp_1c.png>)

Does not work, as you can see. The code will not compile because we protected our index variable by applying a range to it. And that is exactly what we want, code that contains bugs should not compile. 

It is a bit difficult to maintain such code, since we have to keep the array and the range in sync, but that is easy to fix with code like this: Note I also fixed the bug here, because we found the bug and a proper debugging message that the range was wrong. 
    
    
    program dtp_1d;
    {$mode objfpc}{$R+}
    var
      anArray:array[0..9] of integer; // ten elements
      // if we change the array size this is automatically also correct.
      i: low(anArray)..high(anArray);
    begin
      for i := 0 to 9 do   // can't write 10 here...
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    

For completeness you can also use it like this. If any size needs to change, simply change the type: 
    
    
    program dtp_1e;
    {$mode objfpc}{$R+}
    type
      TmyRange = 0..9;
    var
      i:TMyRange;
      anArray:array[TmyRange] of integer; // ten elements
    begin
      for i := Low(TMyRange) to High(TMyRange) do
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** To summarize: Declaring a specific range can help you find range errors at compile time. 

Using `low()` and `high()` can prevent you from making range errors.

### Use `for` … `in` … `do`

Now, forget all the above. When it is possible, you should use [`for`](<For.md> "For") … [`in`](<In.md> "In") … [`do`](<Do.md> "Do"). The Pascal language has `low()` and `high()` for many years and as shown above it can prevent you from introducing range errors. Modern Pascal has a new similar construct but with a new syntax: [`for` … `in` … `do`](<for-in_loop.md> "for-in loop"). This syntax will simply iterate over all possible values in a collection of data like an array, but without an explicit index. 

We can get rid of our bug by preventing it in the first place by removing the index altogether. 
    
    
    program dtp_1d;
    {$mode objfpc}{$R+}
    var
      anArray:array[0..9] of integer; // ten elements
      i:0..9;  // could use j, but this is for clarity.
      Item:integer; // Item is an integer here: it is not an index, but a value from the array
    begin
      // data to show what for in do does
      for i := Low(anArray) to High(anArray) do anArray[i] := 100+i;
      for Item in anArray do  // for every integer value that is contained in the array
        write(Item:4); // writes the value of an array cell, this is not an index.
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** To summarize: 

with `for` … `in` … `do` you can safely iterate over a collection of data without using an explicit index and the risk of range errors.

### Bonus: Using a range? You may want a set, too!

If you have declared a range, why not declare a [set](<Set.md> "Set") as well? This will give you a safe way of performing filters on a data collection like an array. 

A simple example looks like this: 
    
    
    program dtp_1f;
    {$mode objfpc}{$R+}
    type
      TmyRange = 0..9;
    var
      i:TMyRange;
      j:set of TMyRange;
      anArray:array[TmyRange] of integer; // ten elements
    begin
      j:=[1,3,5,7,9];// odd elements
      for i in j do
      begin
        anArray[i] := i;
        write(anArray[i]:3);
      end;
    end.
    

Ranges are powerful, sets are even more so! And makes your code safe and readable. 

### Conclusion

Range errors are common in every language, often hard to find, but if you are reading this you are probably using Pascal. 

And with the right mindset a Pascal programmer can write code in such a way that range errors should hardly exist in the code. 

Because Pascal is so strongly typed and has so many features to help you prevent range errors. 

  * use [`{$rangechecks on}`](</index.php?title=sRangechecks&action=edit&redlink=1> "sRangechecks \(page does not exist\)") or `{$R+}` during development and run your code. Turn it off if you are sure there are no range errors but protect your code with ranges.
  * use _ranges_ instead of integers for your index and think about range when writing your code! It will prevent you from introducing range errors and you will catch them at compile time.
  * use `low()` and `high()` not `1` to `10` or `0` to `9` when you iterate a data collection. Make it a habit.
  * use `for` … `in` … `do` if applicable, try to make that your first option!
  * use a _set of range_ to safely filter



There is more to this subject, but if you follow these simple rules you avoid bugs and trust me: there is no speed penalty. A bit of “brains instead of fingers” will prevent this nasty category of bugs and prevents you from spending more debug time than coding time! 

## How to prevent Overflow Errors, catch them and even misuse Overflow

### The bug

Let's introduce you to a small piece of code with an overflow bug. 
    
    
    program dtp_2a;
    {$mode objfpc}
    var
      a:NativeInt = high(NativeInt);
    begin
      a:= a + 1;
      writeln(a);
    end.
    

Can you spot the bug? Concentrate, look again… Can you see it? 

Now compile that like `fpc dtp_2a.pas`. Then run it: 
    
    
    $ ./dtp_2a
    -2147483648  //depending on nativeint: this is 32 bit
    

It does not crash, it simply prints `-2147483648`. But is that correct? Of course not! Now with overflowchcks on: 
    
    
    program dtp_2a;
    {$mode objfpc}{$overflowchecks on}
    var
      a:NativeInt = high(NativeInt);
    begin
      a:= a + 1;
      writeln(a);
    end.
    

This code will compile, but it will generate an overflow error when you run it: 215. See the programmers guide on overflow checks. 

## How to prevent Input and Output Errors (and how to catch them…)

## How to use meaningful Assertions

## To serve and protect: the story of try..finally

## Do you know your String Type? Really?

_[This should be written by[Juha](</User:JuhaManninen> "User:JuhaManninen")… not [me](</User:Thaddy> "User:Thaddy")…]_

**string** is a devil with many faces: It can be ShortString, AnsiString and UnicodeString.  
I have the habit to declare the exact species of string I am using, especially in library code, but what if the code just says **string**? Well, here's a little utility function to obtain the string type you are actually working with: 
    
    
    //{$mode delphi}  // tkAString AnsiString
    //{$mode delphi}{$H-} // tkSString ShortString
    //{$mode delphiunicode} // tkUString Unicode string
    //{$mode delphiunicode}{$H-} // tkSString ShortString
    //{$mode objfpc} // tkSString ShortString 
    //{$mode objfpc}{$H+} //tkAString AnsiString
    //{$mode fpc}{$modeswitch result} // tkSString ShortString
    //{$mode fpc}{$H+}{$modeswitch result} // tkAString AnsiString
    // etc.
    
    uses typinfo;
     
      function StringType(const s:string):TTypeKind;inline; 
      var info:PTypeInfo;
      begin
        info:=TypeInfo(s);
        Result := Info^.Kind;
      end;
     
    var s: string = 'testme';
    begin
      writeln('My string type is ',StringType(s));
    end.
    

**string** depends on mode,and this little gem will tell you what kind of string you are dealing with.  
That is not always obvious. Try to experiment with some of the mode settings and see what happens.  
The result may not always be what you expected, so use this function as a debug utility. You can be sure it returns what **string** means at any given unit.

---

_Source: [https://wiki.freepascal.org/Defensive_programming_techniques](https://web.archive.org/web/20250326005732/https://wiki.freepascal.org/Defensive_programming_techniques)_
