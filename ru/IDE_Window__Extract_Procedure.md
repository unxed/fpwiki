# IDE Window: Extract Procedure

│ [**Deutsch (de)**](</IDE_Window:_Extract_Procedure/de> "IDE Window: Extract Procedure/de") │  [**English (en)**](<../en/IDE_Window__Extract_Procedure.md> "IDE Window: Extract Procedure") │  [**español (es)**](</IDE_Window:_Extract_Procedure/es> "IDE Window: Extract Procedure/es") │  [**suomi (fi)**](</IDE_Window:_Extract_Procedure/fi> "IDE Window: Extract Procedure/fi") │  [**français (fr)**](</IDE_Window:_Extract_Procedure/fr> "IDE Window: Extract Procedure/fr") │  **русский (ru)** │  [**slovenčina (sk)**](</IDE_Window:_Extract_Procedure/sk> "IDE Window: Extract Procedure/sk") │  [**中文（中国大陆）‎ (zh_CN)**](</IDE_Window:_Extract_Procedure/zh_CN> "IDE Window: Extract Procedure/zh CN") │    
****

  


Аннотация: "Extract Procedure" [извлечение процедуры] принимает некоторые выбранные инструкции pascal и создает новую процедуру/метод из этого кода. Этот инструмент полезен для разделения больших процедур или для легкого создания новой процедуры из некоторого кода. 

Основной пример: 
    
    
     procedure DoSomething;
     begin
       CallSomething;
     end;

Выберите строку "CallSomething;" и сделайте Extract Proc. Откроется диалоговое окно[, где] вы можете выбрать тип и имя создаваемой процедуры. Например: procedure, "NewProc". Результат: 
    
    
     procedure NewProc;
     begin
       CallSomething;
     end;
     
     procedure DoSomething;
     begin
       NewProc;
     end;

Вы можете видеть, что новая процедура "NewProc" была создана с выделением в качестве тела [процедуры], и старый код был заменен [ее] вызовом. 

Локальные переменные и параметры:  
«Extract Proc» сканирует используемые переменные и автоматически создает список параметров и локальные переменные. Пример: 
    
    
     procedure TForm1.DoSomething(var Erni, Bert: integer);
     var
       i: Integer; // Комментарий
     begin
       Erni:=Erni+Bert;
       for i:=Erni to 5 do begin
       |
       end;
     end;

Выберите цикл for и создайте новую процедуру "NewProc". Локальная переменная i используется только в выбранном [тексте], поэтому она будет перенесена в новую процедуру. [Параметр-переменная] Erni также используется в остальном коде, поэтому она станет параметром [новой процедуры]. 

Результат: 
    
    
     procedure NewProc(const Erni: integer);
     var
       i: Integer; // Комментарий
     begin
       for i:=Erni to 5 do begin
       |
       end;
     end;
     
     procedure TForm1.DoSomething(var Erni, Bert: integer);
     begin
       Erni:=Erni+Bert;
       NewProc(Erni);
     end;

Вы можете увидеть, что "i" перенесено в новую процедуру (Примечание: включая ее комментарий) и Erni. 

Ограничения:  
Pascal - очень мощный язык, поэтому не ожидайте, что он будет работать с каждым кодом. Текущие ограничения/ToDo: 

  * проверить, ограничены ли границы выбора на границах операторов
  * операторы "with"

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Extract_Procedure/ru](https://web.archive.org/web/20221207010311/https://wiki.freepascal.org/IDE_Window%3A_Extract_Procedure/ru)_
