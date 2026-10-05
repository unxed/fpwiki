# If

│ **[English (en)](<../en/If.md>)** │  **русский (ru)** │

[Ключевое слово](<Keyword.md> "Keyword/ru") **If** предшествует условию, за которым должно следовать слово [Then](<Then.md> "Then/ru") и необходимый оператор. За оператором может следовать необязательное слово [Else](<Else.md> "Else/ru") или другие операторы. 

## If then
    
    
    if condition
     then true_statement
     else false_statement;
    

Условие _condition_ является выражением типа [Boolean](<Boolean.md> "Boolean/ru"), принимающим значение [True](<True.md> "True/ru") или [False](<False.md> "False/ru").  
оператор _true_statement_ выполнится, если значение условия равно **True**.  
оператор _false_statement_ выполнится, если значение условия равно **False**.  
Если значение условия не является типом [Boolean](<Boolean.md> "Boolean/ru"), то в процессе компиляции возникнет ошибка. 

### Несколько операторов в ветви if then

Если вам необходимо использовать два или более операторов в качестве инструкций _true_statement_ или _false_statement_ , то вам следует заключить их в [блок](</index.php?title=Block/ru&action=edit&redlink=1> "Block/ru \(page does not exist\)") [Begin](<Begin.md> "Begin/ru") ... [End](<End.md> "End/ru") (составной оператор). 
    
    
    if boolean_condition then
    begin
    	statement_zero;
    	statement_one;
    	statement_two;
    end;
    

  


## См. также

  * Официальная документация: [Справочное руководство: оператор If..then..else](<https://www.freepascal.org/docs-html/ref/refsu57.html>)
  * [IF](<../en/Basic_Pascal_Tutorial/Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF"), Tao Yue, Basic Pascal Introduction
  * [If statement and semicolon](</;#If_statement_and_semicolon> ";")



  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — [end](<End.md> "End/ru") — [for](<For.md> "For/ru") — if — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/If/ru](https://web.archive.org/web/20240714023519/https://wiki.freepascal.org/If/ru)_
