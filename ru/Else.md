# Else

│ **[English (en)](<../en/Else.md>)** │  **русский (ru)** │

[Else at Language Reference](<http://www.freepascal.org/docs-html/ref/refsu51.html#x144-15400013.2.3>)

**Else** является [ключевым словом](<Keyword.md> "Keyword/ru"), представляющим действие, которое выполнится, если условие [ложно](<False.md> "False/ru"). 

# [If](<If.md> "If/ru") [Then](<Then.md> "Then/ru") Else
    
    
      If (condition)
      Then true_statement
      Else false_statement;

Вначале вычисляется значение условия _condition_. Если оно [истинно](<True.md> "True/ru"), то выполняется оператор _true_statement_ , в противном случае выполняется оператор _false_statement_. Значение условия _должно_ быть типа [Boolean](<Boolean.md> "Boolean/ru") иначе возникнет ошибка. 

### Более одного оператора в конструкции "if then else"

Если вам необходимо использовать два или более операторов в качестве инструкций _true_statement_ или _false_statement_ , то вам следует сгруппировать эти операторы, поместив их в [блок](</index.php?title=Block/ru&action=edit&redlink=1> "Block/ru \(page does not exist\)") [Begin](<Begin.md> "Begin/ru") ... [End](<End.md> "End/ru"). 
    
    
      if boolean_condition then
        begin
          statement_one;
          statement_two;
        end 
      else
        begin
          statement_three;
          statement_four;
        end;

При обычном использовании, оператор **else** является особым исключением из правил, согласно которому каждый оператор должен оканчиваться точкой с запятой. Ни для оператора **else** , ни для предшествующего ему оператора не требуется ставить точку с запятой. В примере выше первый оператор **end** не оканчивается точкой с запятой, а последний оканчивается. 

Однако, в случае вложенных операторов _if_ , если _else_ относится к _внутреннему_ оператору _if_ , то перед _else_ точку с запятой ставить _не нужно_ ; если оператор _else_ относится к внешнему оператору _if_ , то перед ним _нужно_ ставить точку с запятой: 
    
    
      if a then
          if b then 
            begin
               (..)
            end;
          else
            begin
               (..)
            end;

В этом случае **else** относится к _"if a"_
    
    
      if a then
          if b then 
            begin
               (..)
            end
          else
            begin
               (..)
            end;

В этом случае **else** относится к _"if b"_. Если это вызывает неясность, то это можно разрешить с помощью отсутствия кода в операторе **else** : 
    
    
      if a then
          if b then 
            begin
               (..)
            end
          else
      else
          begin
               (..)
          end

  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — else — [end](<End.md> "End/ru") — [for](<For.md> "For/ru") — [if](<If.md> "If/ru") — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/Else/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Else/ru)_
