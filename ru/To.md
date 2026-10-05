# To

│ [**Deutsch (de)**](</To/de> "To/de") │  [**English (en)**](<../en/To.md> "To") │  [**français (fr)**](</To/fr> "To/fr") │  **русский (ru)** │    
****

[Ключевое слово](<Keyword.md> "Keyword/ru") **To** используется для указания того, что значение переменной-счетчика в цикле [For](<For.md> "For/ru") _увеличивается_ на 1 на каждом шаге цикла. Значение переменной-счетчика, указанное после слова **to** , должно быть больше, чем начальное значение в инструкции цикла [For](<For.md> "For/ru"). 

## Contents

  * 1 Цикл For to do
    * 1.1 Основной пример
    * 1.2 Одинаковые начальное и конечное значения
    * 1.3 Начальное значение больше конечного значения
  * 2 См. также



## Цикл [For](<For.md> "For/ru") to [do](<Do.md> "Do/ru")
    
    
    var i : integer;
    begin
     for i := 1 to 10000 do
       begin
         // инструкции цикла
       end;
    end;
    

Цикл **for** выполняет инструкции кода фиксированное число раз. 

### Основной пример
    
    
    var
      loopValue, startValue, endValue, resultValue: integer;
    begin
      startValue := 10;
      endValue := 11;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    end;
    

Цикл выполнится два раза и значение переменной resultValue станет равным 21. 

### Одинаковые начальное и конечное значения
    
    
    var
      loopValue, startValue, endValue, resultValue: integer;
    begin
      startValue := 10;
      endValue := 10;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    
    end;
    

Цикл выполнится один раз и значение переменной resultValue станет равным 10. 

### Начальное значение больше конечного значения
    
    
    var
      loopValue, startNumber, endNumber, resultValue: integer;
    begin
      startValue := 10;
      endValue := 9;
      resultValue := 0;
      for loopValue := startValue to endValue do
        begin
          resultValue := loopValue + resultValue;
        end;
    
    end;
    

Цикл не выполнится ни разу и значение переменной resultValue останется равным 0. 

## См. также

  * [Downto](<Downto.md> "Downto/ru")

---

_Source: [https://wiki.freepascal.org/To/ru](https://web.archive.org/web/20200813002325/https://wiki.freepascal.org/To/ru)_
