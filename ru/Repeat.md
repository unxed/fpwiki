# Repeat

│ **[English (en)](<../en/Repeat.md>)** │  **русский (ru)** │

Является [ключевым словом](<Keyword.md> "Keyword/ru"), которое используется в управляющей конструкции аналогично циклу '[while](<While.md> "While/ru") [do](<Do.md> "Do/ru")'. 

## Синтаксис
    
    
      repeat
        <блок операторов>
      until <условие>;
    

<блок операторов>: одиночный оператор на языке [Pascal](</index.php?title=Pascal/ru&action=edit&redlink=1> "Pascal/ru \(page does not exist\)") или блок операторов, заключенный в [`begin`](<Begin.md> "Begin/ru")-[`end`](<End.md> "End/ru"). 

<условие>: [выражение](</index.php?title=expression/ru&action=edit&redlink=1> "expression/ru \(page does not exist\)"), значение которого является типом boolean. 

## Пример
    
    
      x := 1;
      repeat
        DoSomethingHere(x);
        x := x + 1;
      until x = 10;
    

  
  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — [end](<End.md> "End/ru") — [for](<For.md> "For/ru") — [if](<If.md> "If/ru") — repeat — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/Repeat/ru](https://web.archive.org/web/20250121215728/https://wiki.freepascal.org/Repeat/ru)_
