# Global variables

│ **[English (en)](<../en/Global_variables.md> "Global variables")** │  **[suomi (fi)](</Global_variables/fi> "Global variables/fi")** │  **русский (ru)** │    
****

Глобальная [переменная](<Variable.md> "Variable/ru") \- это переменная, которая объявляется в главной секции программы или в разделе interface [модуля](<Unit.md> "Unit/ru"). Глобальные переменные, объявленные в программе, не могут быть доступны внутри [модуля](<Unit.md> "Unit/ru"). Глобальные переменные, объявленные в [модуле](<Unit.md> "Unit/ru"), могут быть доступны в программе и в других модулях. 
    
    
     program GlobalVariables;
     var
       g: integer;
     begin
     end.
    

**g** является глобальной переменной. 

  


## Внешние переменные

Внешняя переменная - это глобальная переменная, объявленная в разделе [interface](</index.php?title=Interface/ru&action=edit&redlink=1> "Interface/ru \(page does not exist\)") [модуля](<Unit.md> "Unit/ru"). 

## Читайте подробнее

  * [Var](<Var.md> "Var/ru")
  * [Локальные переменные](<Local_variables.md> "Local variables/ru")

---

_Source: [https://wiki.freepascal.org/Global_variables/ru](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/Global_variables/ru)_
