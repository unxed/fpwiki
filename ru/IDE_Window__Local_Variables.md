# IDE Window: Local Variables

│ **[English (en)](<../en/IDE_Window__Local_Variables.md>)** │  **русский (ru)** │

## Contents

  * 1 Важно
  * 2 Локальные переменные
  * 3 Отображаемые данные
    * 3.1 Scope (Stackframe, Thread, History)
  * 4 См. также



# Важно

Вы должны [настроить отладчик](<Debugger_Setup.md> "Debugger Setup/ru") и запустить проект для отладки. Только после этого данное окно будет полезно. 

# Локальные переменные

[![Locals.png](https://wiki.freepascal.org/images/4/4e/Locals.png)](</File:Locals.png>)

Это список локальных переменных и их текущих значений в текущей функции/процедуре. 

# Отображаемые данные

Name
    Искаженное имя переменной. Обычно компилятор переводит идентификаторы в верхний регистр. Вы увидите локальные переменные только если процедура скомпилирована с отладочной информацией.
Values
    Текущее значение локальной переменной.

Примечание: Значения отображаются в очень упрощенной форме. Например объекты отображаются в виде указателя, вместо структуры. Вы можете получить больше информации, добавив переменную в [список наблюдений](<IDE_Window__Watch_list.md> "IDE Window: Watch list/ru")

### Scope (Stackframe, Thread, History)

The values are evaluated according to the scope set in the [Thread](<../en/IDE_Window__Threads.md> "IDE Window: Threads") and [Stack](<../en/IDE_Window__Call_Stack.md> "IDE Window: Call Stack") dialog. Default is the current Thread and top stack frame. Both (Stack and Frame) dialog offer to change the "current" Frame/Thread. The watch window will follow this selection. 

It is also possible to select previously displayed values, using the [History](<../en/IDE_Window__Debug_History.md> "IDE Window: Debug History") dialog. 

# См. также

  * [watch list](<../en/IDE_Window__Watch_list.md> "IDE Window: Watch list")
  * [Debug History](<../en/IDE_Window__Debug_History.md> "IDE Window: Debug History")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Local_Variables/ru](https://web.archive.org/web/20250210104406/https://wiki.freepascal.org/IDE_Window%3A_Local_Variables/ru)_
