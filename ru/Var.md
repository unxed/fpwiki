# Var

**Var** является [ключевым словом](<Keyword.md> "Keyword/ru"), которое используется для двух разных целей: 

  * обозначает начало секции объявления переменных
  * указывает, что параметры в функцию или процедуру передаются по ссылке вместо передачи по значению



## Объявление переменных

Var используется для обозначения секции, где объявляются [переменные](<Variable.md> "Variable/ru") и их [типы](<Type.md> "Type/ru"). Переменные обычно объявляются в начале [программы](<Program.md> "Program/ru"), [процедуры](<Procedure.md> "Procedure/ru"), [функции](<Function.md> "Function/ru") или [модуля](<Unit.md> "Unit/ru"). 
    
    
    var
      age: integer;
    

Если вы собираетесь использовать несколько переменных одного и того же типа, они могут быть сгруппированы, поэтому они определяются одинаково. В этом случае переменные должны отделяться друг от друга [запятой](<Comma.md> "Comma/ru"). 
    
    
    var
      FirstName, LastName, address: string;
    

## Передача по ссылке

Когда **var** используется перед параметром [процедуры](<Procedure.md> "Procedure/ru") или [функции](<Function.md> "Function/ru"), то это означает, что параметр является [параметром-переменной](</index.php?title=Variable_parameter/ru&action=edit&redlink=1> "Variable parameter/ru \(page does not exist\)"). Параметр-переменная используется для получения данных из процедуры или функции, а также для передачи данных в процедуру или функцию: 
    
    
    procedure foo( var v1: sometype; out v2: sometype; const v3: sometype )
    begin
      v1 := v1 + v3; // ввод и возврат значения
      v2 := v3;      // только возврат значения
      v3 := myconst; // неизменный параметр... только ввод
    end;
    

  


## См. также

  * [Параметр-переменная](</index.php?title=Variable_parameter/ru&action=edit&redlink=1> "Variable parameter/ru \(page does not exist\)")
  * [Локальные переменные](<Local_variables.md> "Local variables/ru")
  * [Глобальные переменные](<Global_variables.md> "Global variables/ru")

---

_Source: [https://wiki.freepascal.org/Var/ru](https://web.archive.org/web/20250326012216/https://wiki.freepascal.org/Var/ru)_
