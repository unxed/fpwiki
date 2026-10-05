# For

│ **[Deutsch (de)](</For/de> "For/de")** │  **English (en)** │  **[français (fr)](</For/fr> "For/fr")** │  **[русский (ru)](<../ru/For.md> "For/ru")** │    
****

` for` is a [keyword](<Keyword.md> "Keyword") used in conjunction with other keywords to create [loops](<Loops.md> "Loops"). 

## Contents

  * 1 loop with iterator variable
    * 1.1 behavior
    * 1.2 reverse direction
    * 1.3 constraints
      * 1.3.1 scope requirements
      * 1.3.2 immutable requirement
      * 1.3.3 type restrictions
      * 1.3.4 other step widths
    * 1.4 limits
    * 1.5 Loop Completion
    * 1.6 short syntax
  * 2 loop with elements
  * 3 special commands
  * 4 see also



## loop with iterator variable

`for` used along with `to`/`downto` and [`do`](<Do.md> "Do") constructs a loop in which the value of a control variable is incremented or decremented by `1` passing every iteration. 

### behavior
    
    
    for controlVariable := start to finalValue do
    begin
    	statement;
    end;
    

In this example `controlVariable` is first initialized with the value of `start` (but cmp. § “legacy” below). If and as long as `controlVariable` is not greater than `finalValue`, the [`begin`](<Begin.md> "Begin") … [`end`](<End.md> "End") [frame](<Frame.md> "Frame") with all its [statements](<statement.md> "statement") is executed. By reaching `end` `controlVariable` is [incremented](<Inc.md> "Inc") by `1` and the comparison is made, whether another iteration, whether the statement-frame is executed again. 

### reverse direction

By exchanging `to` with `downto`, the variable – in the example `controlVariable` – is [decremented](<Dec.md> "Dec") by `1` and the condition becomes “as long as the control variable is not less than the final value.” 

### constraints

#### scope requirements

The control variable has to be [local](<Local_variables.md> "Local variables") inside nested [routines](<Routine.md> "Routine"). A routine is nested, if [routine variables](</index.php?title=Procedural_variable&action=edit&redlink=1> "Procedural variable \(page does not exist\)") tagged with the modifier `is nested` can store its address. Nevertheless, a [global variable](<Global_variables.md> "Global variables") is always allowed as a control variable. 

#### immutable requirement

While inside in a loop, it is imperative not to mess with the loop variable. Plain assignments – e. g. `controlVariable := 2` – are caught by the compiler reporting “Illegal assignment to for-loop variable "controlVariable"”. However, _indirect_ manipulations are not prevented: 
    
    
    program iteratorManipulation(input, output, stderr);
    
    var
    	i: longint;
    
    procedure foo;
    begin
    	i := 1;
    end;
    
    begin
    	for i := 0 to 2 do
    	begin
    		writeLn(i);
    		foo;
    	end;
    end.
    

The global variable `i` is modified by calling `foo`. The program runs an [infinite loop](<Infinite_loop.md> "Infinite loop"). 

#### type restrictions

The [type](<Data_type.md> "Data type") of `controlVariable` has to be enumerable. Assuming your system operates on [ASCII](<ASCII.md> "ASCII") you can do 
    
    
    var
    	c: char;
    begin
    	for c := 'a' to 'z' do
    	begin
    		writeLn(c);
    	end;
    end.
    

or generally use any type that is enumerable. 

#### other step widths

Other step widths than `1` or `-1` are not possible by utilizing this syntax. You have to use other loop constructs such as [`while`](<While.md> "While") … `do` and manually initialize, compare and change the `controlVariable`. 

The matter, whether [FreePascal](<FPC.md> "FPC") could provide a syntax specifying other step widths, came up several times. In general it was regarded as “syntactic sugar”, though, and in order to stay compatible Pascal’s core, or Delphi’s extensions, in order to avoid any impetuous decisions, the developers remained rather conservative and rejected any changes in that direction. 

See also [FPC issue 25549](<https://gitlab.com/freepascal.org/fpc/source/-/issues/25549>).

### limits

Note, as it is common in mathematics when writing sums [math]\displaystyle{ \sum_{n=0}^{k} }[/math] or products [math]\displaystyle{ \prod_{n=1}^{k} }[/math] the limits are _inclusive_. 
    
    
    for i := 5 to 5 do
    begin
    	writeLn(i);
    end;
    

This excerpt will print one line with `5`, though you might be fooled by such thoughts as “5 to 5 – the difference is zero. So the body must never be executed.” This is in fact wrong. Not the _difference_ determines the number of iterations, but the cardinality of the set constructed by the expression `[start..finalValue]`. This can be an empty set, or in the case of `[5..5]` just the single-element set [math]\displaystyle{ \left\\{5\right\\} }[/math]. 

### Loop Completion

Note that the value of the "for loop variable" is undefined after a loop has completed or if a loop is not executed at all. However, if the loop was terminated prematurely with an exception or a break (or even a goto statement), the loop variable retains the value it had when the loop was exited. 

In case of empty loops (where the body is never executed), even the `start` value is not loaded. Rationale: The `controlVariable` exists for usage _inside_ the loop’s body. If the loop’s body is not entered, then the value might remain unused. We generally avoid unused values, i. e. any unnecessary assignment without successive reads. 

### short syntax

For _single_ statements writing a surrounding `begin` … `end` frame can be skipped resulting in: 
    
    
    for controlVariable := start to final do
    	statement;
    

It is advised though, to make use of that only in justified cases, where the readability is improved and it is very unlikely the loop is expanded by any additional statement. Too many programmers inadvertently fell for the pitfall before and added a properly indented line forgetting it requires a surrounding `begin` … `end` then, too. 

Make it a habit and always accompany `for`-loops with `begin` … `end`, leaving the option to possibly eliminate those at a _later_ stage, prior a complete code freeze (but _not_ right in the middle of development). 

## loop with elements

With [`for` … `in` loops](<for-in_loop.md> "for-in loop") the variable that is changed every iteration represents an element out of a collection. This works on strings, [arrays](<Array.md> "Array"), [sets](<Set.md> "Set"), and any other custom collection that implements the required iterators. Looping over an empty collection simply does nothing. 
    
    
    type
    	furniture = (chair, desk, bed, wardrobe);
    	arrangement = set of furniture;
    var
    	thing: furniture;
    begin
    	writeLn('all available pieces of furniture:');
    	for thing in arrangement do
    	begin
    		writeLn(thing);
    	end;
    end.
    

In contrast to other loops, an index variable is not provided. In the above example `ord(thing)` will return an index, but it has to be _additionally_ retrieved while it inherently exists yet still inaccessible. [Proposals](<for-in_loop.md> "for-in loop") were made, whether and how to extend the syntax allowing to specify an index variable that is adjusted with every iteration. 

## special commands

Inside loops (including `for`-loops) two special commands tamper with the regular loop-program-flow. [`system.continue`](<https://www.freepascal.org/docs-html/rtl/system/continue.html>) skips the rest of the statements in _one_ iteration. Effectively in a `for`-loop the next element/index is immediately assigned to the control variable, the regular check is performed, and processing of all the statements might start from the top again. Additionally [`system.break`](<Break.md> "Break") instantly skips the whole loop altogether. This is different to [`system.exit`](<https://www.freepascal.org/docs-html/rtl/system/exit.html>), which exits the whole frame, but loops do not create frames (but [blocks](<Block.md> "Block") do). 

Their usage however is usually discredited, since they “disqualify” the loop’s iteration condition. One has to _know_ a loop’s body contains such commands, in order to actually determine how often a loop is executed. Without such, you can tell how many times a loop is run just by inspecting the loop’s head or tail respectively. 

## see also

  * [Example: Why the loop variable should be of signed type](<Example__Why_the_loop_variable_should_be_of_signed_type.md> "Example: Why the loop variable should be of signed type")
  * [`{$rangechecks}`](</index.php?title=sRangechecks&action=edit&redlink=1> "sRangechecks \(page does not exist\)")



  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — for — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/Downto](https://web.archive.org/web/20250321095734/https://wiki.freepascal.org/Downto)_
