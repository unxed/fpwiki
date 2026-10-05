# IDE Window: Watch list

│ **[Deutsch (de)](</IDE_Window:_Watch_list/de> "IDE Window: Watch list/de")** │  **[English (en)](<../en/IDE_Window__Watch_list.md> "IDE Window: Watch list")** │  **русский (ru)** │    
****  
****

## Contents

  * 1 Важно
  * 2 Список наблюдения
    * 2.1 Отображаемые данные
      * 2.1.1 Область видимости (Stackframe, Thread, History)
      * 2.1.2 Специальные значения
    * 2.2 Интерфейс
  * 3 Свойства наблюдения
  * 4 См.также



# Важно

Вы должны [настроить отладчик](<Debugger_Setup.md> "Debugger Setup/ru") и запустить проект для его отладки. Только тогда это окно будет полезно. Чтобы открыть список наблюдения, нажмите `Ctrl` \+ `Alt` \+ `W`. 

# Список наблюдения

[![Watch List.png](https://wiki.freepascal.org/images/6/63/Watch_List.png)](</File:Watch_List.png>)

«Список наблюдения» показывает значения переменных и выражений ("watches", отслеживаемые элементы), когда отлаживаемое приложение приостановливается (например, достигнута точка останова). 

Выражениями могут быть локальные или глобальные переменные, ([определенные](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru")) свойства или выражения паскаля (ограниченная поддержка, например, «a + 1»). [ Подробнее см. здесь](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru")

## Отображаемые данные

Данные отображаются в виде 2 столбцов: 

  * Expression(Выражение): отслеживаемые переменная или выражение
  * Value(Значение): текущее значение выражения



Записи можно дважды щелкнуть, чтобы отредактировать их. 

### Область видимости (Stackframe, Thread, History)

Значения оцениваются в соответствии с областью видимости, установленной в диалоговых окнах [Thread](<../en/IDE_Window__Threads.md> "IDE Window: Threads") и [Stack](<../en/IDE_Window__Call_Stack.md> "IDE Window: Call Stack"). По умолчанию используется текущий поток и стек вызовов верхнего уровня. Оба диалога (Stack and Frame) предлагают изменить «текущий» Frame/Thread. Окно просмотра будет следовать за этим выбором. 

Также можно выбрать ранее отображаемые значения, используя диалоговое окно [ History](<../en/IDE_Window__Debug_History.md> "IDE Window: Debug History"). 

### Специальные значения

<invalid> (недействительный)
    Значение в настоящее время недоступно. Так бывает, если отладчик не активен или отлаживаемое приложение в данный момент не приостановлено.
<evaluating> (оценочный)
    Значение в настоящее время получено. Результат ожидается
<disabled> (отключенный)
    Выражение исключается из оценки. См. Раздел «Отключение/включение кнопок (лампочек)».
Error... (ошибочный)
    Значение не может быть оценено. (Ошибка в выражении или переменная недоступна в выбранной области)

## Интерфейс

_Панель инструметов_

[![debugger power.png](https://wiki.freepascal.org/images/b/ba/debugger_power.png)](</File:debugger_power.png>) Power
    Включает/отключает все обновления. Это не влияет на состояние включено/отключено отдельных отслеживаемых элементов. Это заморозит текущее отображение.
[![laz add.png](https://wiki.freepascal.org/images/0/07/laz_add.png)](</File:laz_add.png>) Add
    Добавляет новое выражение. Откроется диалоговое окно свойств Watch (Также можно дважды щелкнуть пустую строку в списке).
[![debugger enable.png](https://wiki.freepascal.org/images/7/76/debugger_enable.png)](</File:debugger_enable.png>) Enable/[![debugger disable.png](https://wiki.freepascal.org/images/c/c0/debugger_disable.png)](</File:debugger_disable.png>) Disable
    Включает/отключает отдельные отслеживаемые элементы из оценки. Можно использовать, чтобы не тратить время на оценку, если отслеживаемые элементы недоступны в текущей области видимости.
[![laz delete.png](https://wiki.freepascal.org/images/6/63/laz_delete.png)](</File:laz_delete.png>) Remove
    Удаляет выбранные отслеживаемые элементы.
[![debugger enable all.png](https://wiki.freepascal.org/images/6/61/debugger_enable_all.png)](</File:debugger_enable_all.png>) Enable all/[![debugger disable all.png](https://wiki.freepascal.org/images/8/8b/debugger_disable_all.png)](</File:debugger_disable_all.png>) Disable all
    Включает/отключает все отслеживаемые элементы из оценки.
[![menu clean.png](https://wiki.freepascal.org/images/7/74/menu_clean.png)](</File:menu_clean.png>) Delete all
    Очищает список.
[![menu environment options.png](https://wiki.freepascal.org/images/1/1f/menu_environment_options.png)](</File:menu_environment_options.png>) Properties
    Изменяет выражение или свойства текущих/выбранных отслеживаемых элементов (Это также возможно сделать, дважды щелчкнув по отслеживаемому элементу).

_Контекстное меню_

[![Watch List popup.png](https://wiki.freepascal.org/images/4/4b/Watch_List_popup.png)](</File:Watch_List_popup.png>)

В дополнение к вышеуказанным функциям контекстное меню позволяет: 

Inspect (Посмотреть)
    открывает текущие отслеживаемоые элементы в Debug-Inspector'е
Evaluate/Modify (Вычислить/Изменить)
    открывает текущие отслеживаемые элементы в окне Evaluate/Modify
Create Data/Watch Breakpoint (Создать точку останова с наблюдением...)
    открывает диалоговое окно для создания новой точки наблюдения на основе текущего отслеживаемого элемента (останавливается, если текущее значение изменяется или становится доступным)
Copy Name (Копировать имя)
    копирует выражение в буфер обмена
Copy Value (Копировать значение)
    копирует значение в буфер обмена

  


# Свойства наблюдения

[![Watch Properties.png](https://wiki.freepascal.org/images/2/28/Watch_Properties.png)](</File:Watch_Properties.png>)

Expression (Выражение)
    выражение, для которого должно отображаться вычисленное значение. Выражения могут быть локальными или глобальными переменными, ([определенными](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru")) свойствами или выражениями паскаля (ограниченная поддержка, например, «a + 1»).

Repeat Count (Число повторов)
    может использоваться для получения срезов массива. Наблюдение определяет первый элемент массива "A[7]" (должен иметь индекс). При "количестве повторов" равным 20 оно показывает элементы массива от A[7] до A[26]. Его также можно использовать с динамическим массивом (без индекса). Тогда оно указывает, сколько элементов нужно показать, начиная с Item[0].

Digits (Разряды)
    не реализовано.
Enabled (Включить)
    См. Enable/Disable выше.
Allow function calls (Разрешить вызовы функций)
    пока не поддерживается.
Use Instance class type (Использовать тип экземпляра класса)
    объекты обычно отображаются в соответствии с объявлением наблюдаемого выражения. Наблюдение "Sender: TObject" покажет вам только данные, объявленные в TObject. Однако объектные переменные могут содержать объекты унаследованных классов. Sender'ом может быть TForm. Используя это, отладчик найдет фактический класс объекта и отобразит все данные.
Style (Стиль)
    каким образом отображать данные. Если стиль не может быть применен, будет использоваться значение по умолчанию.

# См.также

  * Watch-Points (Data-[Breakpoints](<../en/IDE_Window_Breakpoints.md> "IDE Window:Breakpoints"))
  * [Evaluate Window](<../en/IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify")
  * [Debug Inspector](<../en/IDE_Window__Variable_Inspector.md> "IDE Window: Variable Inspector")
  * [Debug History](<../en/IDE_Window__Debug_History.md> "IDE Window: Debug History")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Watch_list/ru](https://web.archive.org/web/20240524204556/https://wiki.freepascal.org/IDE_Window%3A_Watch_list/ru)_
