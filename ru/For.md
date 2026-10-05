# For

│ **[English (en)](<../en/For.md>)** │  **русский (ru)** │

[Ключевое слово](<Keyword.md> "Keyword/ru") **for** используется вместе с "[to](<To.md> "To/ru")"\"[downto](<Downto.md> "Downto/ru")" и "[do](<Do.md> "Do/ru")" для выполнения цикла, в котором значение управляющей переменной на каждом шаге увеличивается или уменьшается на 1: 
    
    
    FOR _control_variable_ := _start_ [TO](<To.md> "To/ru") _final_ DO _statement_
    

в котором _control_variable_ увеличивается на 1 на каждом шаге выполнения цикла до тех пор, пока её значение не будет больше или равно _final_ или 
    
    
    FOR _control_variable_ := _start_ [DOWNTO](<Downto.md> "Downto/ru") _final_ DO _statement_
    

в котором _control_variable_ уменьшается на 1 на каждом шаге выполнения цикла до тех пор, пока её значение не будет меньше или равно _final_

где _control_variable_ \- переменная, которая должна быть установлена в значение _start_. Управляющая переменная увеличивается или уменьшается на 1 на каждом шаге цикла до тех пор, пока её значение не достигнет или превысит значения _final_. 
    
    
    For I:=1 To 100 Do statement;
    

(повторяет _statement_ сто раз, увеличивая значение **I** от 1 до 100) 
    
    
      for I:=100 downto 1 do ''statement'';
    

(повторяет _statement_ сто раз, уменьшая значение **I** от 100 до 1) 

  * Цикл FOR будет выполнять только один единственный оператор, следующий за ним. Для выполнения большего количества операторов необходимо заключить их в блок [Begin](<Begin.md> "Begin/ru")/[End](<End.md> "End/ru").
  * Если в цикле **for..to** значение _start_ больше значения _final_ , то цикл не выполнится
  * Если в цикле **for..downto** значение _start_ меньше значения _final_ , то цикл не выполнится



После выполнения цикла значение _control_variable_ будет равно _final_. Если цикл не выполнился, то значение _control_variable_ не изменится. 

  * вы можете использовать [types](<Type.md> "Type/ru") вместо чисел.



  


## См. также

  * [Example: Why the loop variable should be of signed type](<../en/Example__Why_the_loop_variable_should_be_of_signed_type.md> "Example: Why the loop variable should be of signed type")



  
  
**Ключевые слова:** [begin](<Begin.md> "Begin/ru") — [do](<Do.md> "Do/ru") — [else](<Else.md> "Else/ru") — [end](<End.md> "End/ru") — for — [if](<If.md> "If/ru") — [repeat](<Repeat.md> "Repeat/ru") — [then](<Then.md> "Then/ru") — [until](<Until.md> "Until/ru") — [while](<While.md> "While/ru")

---

_Source: [https://wiki.freepascal.org/For/ru](https://web.archive.org/web/20250121230307/https://wiki.freepascal.org/For/ru)_
