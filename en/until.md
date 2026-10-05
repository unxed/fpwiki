# Until

│ **English (en)** │

This [keyword](<Keyword.md> "Keyword") is used in a control construct that is similar to a [while](<While.md> "While") or [do](<Do.md> "Do") loop. 

Syntax: 
    
    
     **repeat**
     ** <statement block>**
     **until <condition>;**
    

<statement block>: A single pascal statement or a begin-end statement block. 

<condition>: Expression that eveluates to a boolean value. 

Example: 
    
    
     **x := 1;**
     **repeat**
     **begin**
     **DoSomethingHere(x);**
     **x := x + 1;**
     **end;**
     **until x = 10;**
    

  
  
**Keywords:** [begin](<Begin.md> "Begin") — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — **until** — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/until](https://web.archive.org/web/20170527110929/https://wiki.freepascal.org/until)_
