# Begin

│ **[Deutsch (de)](</Begin/de> "Begin/de")** │  **[English (en)](<../en/Begin.md> "Begin")** │  **[español (es)](</Begin/es> "Begin/es")** │  **[suomi (fi)](</Begin/fi> "Begin/fi")** │  **[français (fr)](</Begin/fr> "Begin/fr")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Begin/zh_CN> "Begin/zh CN")** │    
****

[Ключевое слово](<Reserved_word.md> "Reserved word/ru") **begin** используется для начала исполняемой секции [функции](<Function.md> "Function/ru"), [метода](<Method.md> "Method/ru") [объекта](</index.php?title=Object/ru&action=edit&redlink=1> "Object/ru \(page does not exist\)"), [процедуры](<Procedure.md> "Procedure/ru"), [программы](<Program.md> "Program/ru"), [свойства](</Property/ru> "Property/ru") объекта или используется для отделения начала выражения [блока](</index.php?title=Block/ru&action=edit&redlink=1> "Block/ru \(page does not exist\)"). 

Для функции, метода, процедуры, программы или свойства оно используется после всех объявлений [const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)"), [type](<Type.md> "Type/ru") и [var](<Var.md> "Var/ru") и перед первым исполняемым выражением. Оно всегда завершается выражением [end](<End.md> "End/ru"): 
    
    
      program Project1;
      var (..);
      begin
        (..);
      end.
    

Для выражения блока оно отделяет начало блока и также завершает выражение end: 
    
    
      if (..) then
        begin
          (..)
        end
      else
        begin
          (..)
        end;
    

**begin** _должно_ быть закрыто **[end](<End.md> "End/ru")**. 

  
**Ключевые слова:** begin — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — [end](<End.md> "End/ru") — [for](<For.md> "For/ru") — [if](<If.md> "If/ru") — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/Begin/ru](https://web.archive.org/web/20250121211431/https://wiki.freepascal.org/Begin/ru)_
