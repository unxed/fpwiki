# Until

│ **[English (en)](<../en/Until.md>)** │  **русский (ru)** │

Является [ключевым словом](<Keyword.md> "Keyword/ru"), которое используется в управляющей конструкции аналогично циклу '[while](<While.md> "While/ru") [do](<Do.md> "Do/ru")'. 

Синтаксис: 
    
    
     **repeat**
     ** <блок операторов>**
     **until <условие>;**
    

<блок операторов>: одиночный оператор на языке pascal или блок операторов, заключенный в _begin-end_. 

<условие>: выражение, значение которого является типом boolean. 

Пример: 
    
    
     **x := 1;**
     **repeat**
     **begin**
     **DoSomethingHere(x);**
     **x := x + 1;**
     **end;**
     **until x = 10;**
    

  
  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — [end](<End.md> "End/ru") — [for](<For.md> "For/ru") — [if](<If.md> "If/ru") — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — until — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/Until/ru](https://web.archive.org/web/20240714011652/https://wiki.freepascal.org/Until/ru)_
