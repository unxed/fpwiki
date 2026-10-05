# End

│ **[English (en)](<../en/End.md>)** │  **русский (ru)** │

**End** является [ключевым словом](<Keyword.md> "Keyword/ru"), предназначенным для: 

  * завершения [блока](</index.php?title=Block/ru&action=edit&redlink=1> "Block/ru \(page does not exist\)") инструкций, начинающихся зарезервированными словами [Begin](<Begin.md> "Begin/ru") или [Case](<Case.md> "Case/ru");
  * завершения объявлений [полей](</index.php?title=Field/ru&action=edit&redlink=1> "Field/ru \(page does not exist\)") в [записях](<Record.md> "Record/ru");
  * завершения конструкций [Try](<Try.md> "Try/ru") .. [Finally](</index.php?title=Finally/ru&action=edit&redlink=1> "Finally/ru \(page does not exist\)") или [Try](<Try.md> "Try/ru") .. [Except](</index.php?title=Except/ru&action=edit&redlink=1> "Except/ru \(page does not exist\)").



Оно также используется для завершения [модуля](</index.php?title=Unit/ru&action=edit&redlink=1> "Unit/ru \(page does not exist\)"), не имеющего кода в разделе **initialization**. 

Например: 
    
    
      procedure Proc1;
      
      var a,b: integer;
      
      begin
        (..)
      end;

Оператор **end** является одним из исключений из правил, согласно которому каждый оператор должен оканчиваться точкой с запятой. Для оператора, предшествующего оператору **end** , не требуется ставить точку с запятой. 

Оператор **end** также используется для указания конца файла с исходным кодом на языке [Pascal](<../en/Pascal.md> "Pascal"). В этом случае за ним ставится [точка](</index.php?title=Period/ru&action=edit&redlink=1> "Period/ru \(page does not exist\)"), а не [точка с запятой](</index.php?title=;/ru&action=edit&redlink=1> ";/ru \(page does not exist\)") (в приведенном ниже примере последняя точка с запятой является не обязательной): 
    
    
       
      program Proc2;
      var
        SL: TStrings;
      begin
        SL := TStringlist.Create;
        try
          (..)
        finally
          SL.Free;
        end;
      end.

Оператор **end** используется для указания конца модуля: 
    
    
      unit detent;
      uses math;
     
      procedure delta(r:real);
     
      implementation
     
      procedure delta;
      begin
     
      ...
     
      end;
     
      ...
      (* Примечание: Нет соответствующего оператора '''begin''' *)
     
      end.

Также оператор **end** предназначен для завершения описания [записей](<Record.md> "Record/ru"): 
    
    
     Type
       ExampleRecord = Record
                         Values: array [1..200] of real;
                         NumValues: Integer; { holds the actual number of points in the array }
                         Average: Real { holds the average or mean of the values in the array }
                       End;

  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — end — [for](<For.md> "For/ru") — [if](<If.md> "If/ru") — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/End/ru](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/End/ru)_
