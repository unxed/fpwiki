# Local variables

│ **[English (en)](<../en/Local_variables.md>)** │  **русский (ru)** │

Локальная [переменная](<Variable.md> "Variable/ru") определяется внутри [процедуры](<Procedure.md> "Procedure/ru"), [функции](<Function.md> "Function/ru"), [метода](<Method.md> "Method/ru") или в секции [implementation](</index.php?title=Implementation/ru&action=edit&redlink=1> "Implementation/ru \(page does not exist\)") [модуля](<Unit.md> "Unit/ru") и доступна только в них. Говорят, что она имеет локальную область видимости и не может быть доступна извне (т.е. в другой внешней процедуре, функции или модуле). 
    
    
     procedure DoSomething; 
     var 
      x : Tsome_type;
     begin
      
     end;
    

## Читайте подробнее

  * [Var](<Var.md> "Var/ru")
  * [Глобальные переменные](<Global_variables.md> "Global variables/ru")

---

_Source: [https://wiki.freepascal.org/Local_variables/ru](https://web.archive.org/web/20250426111802/https://wiki.freepascal.org/Local_variables/ru)_
