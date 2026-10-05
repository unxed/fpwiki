# If

│ **English (en)** │


This [keyword](<Keyword.md> "Keyword") precedes a condition, must be followed by [then](<Then.md> "Then") and may optionally be followed by [else](<Else.md> "Else"). 

  


## If then
    
    
      if condition
        then true_statement
        else false_statement;

Condition is a [Boolean](<Boolean.md> "Boolean") expression that evaluates to [true](<True.md> "True") or [false](<False.md> "False"). true_statement is executed if condition evaluates to true. false_statement is executed if condition evaluates to false. An error (_What error? Run-time? Compile-time? What number? An exception perhaps? Please fix this_) occurs if condition does not evaluate to a boolean value. 

  


### More statements in "if then" statement

If you need two or more statements for true_statement or false_statement, place them within a [begin](<Begin.md> "Begin") ... [end](<End.md> "End") [Block](</index.php?title=Block&action=edit&redlink=1> "Block \(page does not exist\)"). 
    
    
      if boolean_condition then
        begin
          statement_one;
          statement_two;
          statement_three;
        end;

## Documentation

Official documentation: [[1]](<http://www.freepascal.org/docs-html/ref/refsu58.html#x163-18500013.2.3>)

  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — **if** — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/if](https://web.archive.org/web/20170310072750/https://wiki.freepascal.org/if)_
