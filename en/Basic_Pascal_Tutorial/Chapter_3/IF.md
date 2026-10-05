# Basic Pascal Tutorial/Chapter 3/IF

│ **English (en)** │

[ ◄ ](<Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---  
  
  
Back to [Reserved words](<../../Reserved_words.md> "Reserved words"). 

  
3Ca - IF (author: Tao Yue, state: changed) 

The `IF` statement allows you to branch based on the result of a Boolean operation. 

## Format

    **IF** _expression_ THEN
    _statement1_
    [ ELSE
    _statement2_]

Where 

    _expression_ is any comparison, constant or function which returns a [boolean](</index.php?title=boolean&action=edit&redlink=1> "boolean \(page does not exist\)") value, and
    _statement1_ and _statement2_ are either a single [statement](<../../statement.md> "statement"), a [Begin](</index.php?title=begin&action=edit&redlink=1> "begin \(page does not exist\)")-[end](<../../End.md> "End") [block](<../../Block.md> "Block"), a [repeat](<../../Repeat.md> "Repeat")-until block, or the [null statement](</;#empty_statement> ";").

The **ELSE** clause is optional. 

There are two ways to use an `IF` statement, a one-way branch or a two-way branch/ 

## One-way branch

The one-way branch format is: 
    
    
    if BooleanExpression then
      StatementIfTrue;
    

If the Boolean expression evaluates to `true`, the statement executes. Otherwise, it is skipped. 

The `IF` statement accepts only one statement. If you would like to use a [compound statement](</index.php?title=compound_statement&action=edit&redlink=1> "compound statement \(page does not exist\)"), you must use a `begin-end` [frame](<../../Frame.md> "Frame") to enclose the statements: 
    
    
    if BooleanExpression then
    begin
      Statement1;
      Statement2;
    end;
    

## Two-way branch

There is also a two-way selection: 
    
    
    if BooleanExpression then
      StatementIfTrue
    else
      StatementIfFalse;
    

Note there is never a semicolon `;` immediately before the `else`. 
    
    
    if BooleanExpression then
    begin
      Statement1;  // semicolon here is mandatory
      Statement2;  // semicolon here is optional
    end    // semicolon here is forbidden
    else
    begin
      Statement3;
      Statement4;
    end;
    

If the Boolean expression evaluates to `FALSE`, the statement following the `else` will be performed. Note that you may _never_ use a semicolon after the statement preceding the `else`. That causes the computer to treat it as a one-way selection, leaving it to wonder where the else came from. And when a compiler wonders, it usually gets mad and throws a tantrum, or rather, it throws an error 

If you need multi-way selection, simply nest `if` statements: 
    
    
    if Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2
      else
        Statement3;
    

Be careful with nesting. Sometimes the computer won't do what you want it to do: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
    else
      Statement1;
    

The `else` is always matched with the most recent `if`, so the computer interprets the preceding block of code as: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
        Statement1;
    

You can get by with a null statement: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
    else
      Statement1;
    

Or you could use a `begin-end` block. 

The following proves a semicolon is _absolutely forbidden_ before an else: 
    
    
    // Paul Robinson 2020-12-16
    
    // Compiler test program  Err03.pas
    // tests the proposition that ; is
    // never legal before ELSE
    
    
    program Err03;
    Var
        Test,Test2: Boolean;
    
    
    Begin
    
        Test := True;
        Test2 := True;
    
        if Test then
           if Test2 then
               Writeln('Reached Part 1');  // semi-colon here should be illegal
         else
            Writeln('Reached Part 2');
    
    end.
    

But the best way to clean up the code would be to rewrite the condition. 
    
    
    if not Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2;
    

This example illustrates where the not operator comes in very handy. If Condition1 had been a Boolean like: `(not(a < b) or (c + 3 > 6)) and g`, reversing the expression would be more difficult than NOTting it. 

Also notice how important indentation is to convey the logic of program code to a human, but the compiler ignores the indentation. 

[ ◄ ](<Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/IF](https://web.archive.org/web/20250401020811/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/IF)_
