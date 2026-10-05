# GDB Debugger Tips

│ **[English (en)](<../en/GDB_Debugger_Tips.md> "GDB Debugger Tips")** │  **русский (ru)** │    
****

  


## Contents

  * 1 Вступление
  * 2 См.также
    * 2.1 Установка (GDB и LLDB)
    * 2.2 Другое
  * 3 Основное
    * 3.1 Тип отладочной информации (GDB и LLDB)
      * 3.1.1 Stabs (только GDB)
      * 3.1.2 Dwarf (GDB и LLDB)
        * 3.1.2.1 Dwarf 2 (-gw)
        * 3.1.2.2 Dwarf 2 с наборами (-gw -godwarfsets)
        * 3.1.2.3 Dwarf 3 (-gw3)
      * 3.1.3 Различия
    * 3.2 Проверка типов данных (Watch/Hint)
      * 3.2.1 Строки
      * 3.2.2 Свойства
      * 3.2.3 Вложенные процедуры / функции
      * 3.2.4 Массивы
    * 3.3 Указание GDB вида сборки
  * 4 Windows
    * 4.1 Консольный вывод для GUI-приложений
    * 4.2 Отладка приложений с правами Администратора
  * 5 Win 64 bit
    * 5.1 Использование 32-битного Lazarus на 64-битной Windows
    * 5.2 Больше проблем и решений Win 64 bit
  * 6 Win CE
    * 6.1 Отладчик не находит никаких исходных файлов
  * 7 Linux
    * 7.1 Известные проблемы
  * 8 macOS
    * 8.1 Известные проблемы (GDB)
      * 8.1.1 Отладка 32-битного приложения на 64-битной архитектуре
      * 8.1.2 TimeOuts (только 64 бит)
      * 8.1.3 Hardware exceptions under macOS
    * 8.2 Использование альтернативных отладчиков (GDB)
    * 8.3 Xcode 5
    * 8.4 Замороженный UI с GDB
    * 8.5 Ссылки
  * 9 FreeBSD
  * 10 Использование переведенного GDB (неанглийские сообщения от GDB)
  * 11 Известные проблемы/ошибки, о которых сообщает IDE
    * 11.1 During startup program exited normally
    * 11.2 SIGFPE
    * 11.3 Error 193 (Error creating process / Exe not found)
    * 11.4 SigSegV - даже с новым пустым приложением
    * 11.5 SigSegV - и продолжить отладку
    * 11.6 На Windows Open/Save/File или System Dialog вызывает сбой GDB
    * 11.7 "Step over" шаги внутри функции (Win 64)
    * 11.8 Перестал работать gdb.exe
    * 11.9 internal-error: clear_dangling_display_expressions
    * 11.10 "PC register is not available" ("SuspendThread failed. (winerr 5)")
    * 11.11 Can not insert breakpoint (Не могу вставить точку останова)
  * 12 Сообщения об ошибках
    * 12.1 Проверьте существующие отчеты
      * 12.1.1 Баги в GDB
      * 12.1.2 Проблемы с GDB 7.5.9 или 7.6
    * 12.2 Create a new Report (GDB and LLDB)
      * 12.2.1 Основная информация
      * 12.2.2 Логирование информации для сеанса отладки
  * 13 Тестовые запуски
  * 14 Ссылки
    * 14.1 Внешние ссылки
    * 14.2 Экспериментальные отладчики в Паскале
    * 14.3 Связанные темы форума



# Вступление

Lazarus поставляется с [GDB](<../en/GDB.md> "GDB") в качестве отладчика по умолчанию. Начиная с Lazarus 2.0, это значение по умолчанию изменилось на LLDB на MacO. 

  * Эта страница для Lazarus 1.0 и новее. Для более старых версий см. [предыдущая версия этой страницы](<../en/GDB_Debugger_Tips.md>)
  * _Примечание по GDB 7.5_ : GDB 7.5 не поддерживается выпущенной версией 1.0. Исправления для поддержки были сделаны в 1.1.



# См.также

## Установка (GDB и LLDB)

Чтобы получить наилучшие результаты, вы должны убедиться, что ваша IDE и Project правильно настроены. 

См. раздел [установка отладчика](<Debugger_Setup.md> "Debugger Setup/ru"), чтобы настроить IDE и ваш проект для использования отладчика. 

[Setup Video Tutorial](<http://www.youtube.com/watch?v=cf4G06k2YL8>)

## Другое

[ Debugging console applications](<../en/Debugger_Console_App.md> "Debugger Console App")

# Основное

## Тип отладочной информации (GDB и LLDB)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Эти настройки применяются только к проекту или пакету, для которого они установлены. Если вы изменяете настройки в любом из параметров проекта или пакета, следует убедиться, что это сделано для **всех** пакетов и проекта.  
В противном случае ваш проект будет иметь смешанную информацию отладки, что может привести к ухудшению процесса отладки.   


Настройки могут быть применены к проекту или пакетам с использованием [Additions and Overrides](<../en/IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** При компиляции **_32-битных_** приложений (нативных или кросс) FPC по умолчанию имеет значение "Stabs" (по крайней мере, в некоторых ОС). Рекомендуется изменить вашу конфигурацию на "Dwarf".

Для 64-битных приложений FPC поддерживает только "Dwarf". (Старые FPC также поддерживают "Stabs", но это не рекомендуется) 

  


### Stabs (только GDB)

|  **-g** или **-gs** Вы должны использовать только если ваша версия GDB не поддерживает [режим] dwarf. Есть очень мало других случаев, когда вам это нужно. Вам может понадобиться это [в случае использования] «var param» (param by ref): `procedure foo(var a: integer);` Однако IDE имеет дело с этим в 99% всех случаев. Отладчик на основе LLDB не поддерживает [режим] Stabs   
---|---  
  
### Dwarf (GDB и LLDB)

| 

#### Dwarf 2 (-gw)

Это устанавливает формат в Dwarf2. Это самая основная настройка dwarf. 

#### Dwarf 2 с наборами (-gw -godwarfsets)

Этот параметр добавляет возможность проверки наборов: "type TFoo=set of (a,b,c);". Это заимствовано из спецификаций Dwarf 3, но поддерживается большинством версий GDB (любой GDB начиная с версии 7 и выше должен это делать). **Это рекомендуемая настройка.**

#### Dwarf 3 (-gw3)

Dwarf 3 может кодировать дополнительную информацию для некоторых типов (таких как строки и массивы). Это также сохраняет регистр идентификаторов в отладочной информации. Однако все еще есть проблемы с произведенной отладочной информацией. Некоторая информация может быть неправильно закодирована, а другая не понята GDB. В некоторых случаях это может привести к _падению gdb_. Этот параметр можно использовать при использовании отладчика на основе FpDebug (добавить пакет для IDE)   
  
  
### Различия

|  Этот список никоим образом не полон: 

  * dwarf допускает некоторые свойства (те, которые непосредственно сопоставлены с полем)
  * stabs (и современный gdb) может делать -gp (сохранить регистр символов вместо того, чтобы получать все заглавные буквы).
  * stabs имеет проблемы с некоторыми типами классов. IDE исправляет это в некоторых случаях. (Влияет только на GDB 7.0 и выше) [[1]](<http://bugs.freepascal.org/view.php?id=19920>)

  
  
## Проверка типов данных (Watch/Hint)

### Строки

|  GDB не знает тип данных строк Паскаля. Информация о типе, которую возвращает GDB, в настоящее время не позволяет IDE различать PChar (индекс равен 0) и строку (индекс равен 1). В результате "mystring[10]" может быть 10-ым символом в строке или 11-м в PChar Поскольку среда IDE не может быть уверена, какая из них применима, она покажет оба варианта. 2 результата будут иметь префикс String/PChar.   
---|---  
  
### Свойства

|  В настоящее время отладчик не поддерживает выполнение каких-либо методов. Поэтому могут быть проверены только те свойства, которые относятся непосредственно к переменной. (Это работает только при использовании dwarf). 
    
    
    TFoo = Class
    private
      FBar: Integer;
      function GetValue: Integer;
    public
      property Bar: Integer read FBar;        // Может быть проверено (если используется Dwarf)
      property Value: Integer read GetValue;  // *Не* может быть проверен
    end;
      
  
### Вложенные процедуры / функции

| 
    
    
    procedure SomeObject.Outer(NumText: string);
    var 
      OuterVarInScope: Integer;
    
    procedure Nested;
    var 
      I: Integer;
    begin
      WriteLn(OuterVarInScope);  
    end;
    
    var 
      OuterVarOutsideScope: Integer;
    
    begin
      Nested;
    end;
    

Если вы входите в «Nested», то IDE позволяет вам проверять переменные из обоих стековых фреймов. Это вы можете проверить: I, OuterVarInScope, NumText (без необходимости изменять текущий фрейм стека, в окне стека) Однако есть несколько оговорок: Вы также можете проверить: OuterVarOutsideScope. Это против правил определения границ Паскаля. Это имеет значение, только если у вас есть несколько вложенных уровней, и все они содержат переменную с тем же именем, но область видимости Паскаля скрыла бы переменную из среднего фрейма. Тогда вы увидите неправильное значение. Вы не можете оценить операторы по 2 фреймам: «OuterVarnScope-I» не работает.  [![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>) **Предупреждение:** Вы можете увидеть неправильное значение. Если в любом другом модуле есть глобальная (или иным образом видимая для правил области действия GDB) переменная с тем же именем, что и локальная переменная из внешнего фрейма, будет показана эта глобальная переменная. Там нет никакого предупреждения для этого. Безопасным способом является явный выбор внешнего стекового фрейма, в котором определен локальный var, и проверка значения.   
  
  
### Массивы

| 
    
    
    array [x..y] of Integer;
    

показывает как символы, а не int. Только GDB 7.2 или выше, кажется, справиться с этим правильно. Кроме того, могут возникнуть проблемы при использовании массивов (неназванных) записей. Динамические массивы будут показывать только ограниченный объем своих данных. Начиная с Lazarus 1.1, предел может быть указан.   
Если значения Watches/Hint отображаются некорректно, отключите REGVARS. Установите -OoNOREGVAR в «Параметрах пользователя». 

## Указание GDB вида сборки

GDB позволяет указать вид сборки (стиль Intel или AT&T), который будет использоваться при отображении кода сборки. Если кто-то хочет перейти на конкретный вариант, это можно изменить в свойстве AssemblerStyle в параметрах отладчика Lazarus для GDB (функция доступна в Lazarus 2.0). Альтернативно (и единственный способ добиться этого внутри Lazarus в более старых версиях до Lazarus 2.0) можно указать эту опцию, передав команду _set disassembly-flavor_ в [Debugger_Startup_options](<../en/images/9/98/Dbg_setup_options2.md> "Dbg setup options2.png"). Можно использовать несколько вариантов синтаксиса (в зависимости от версии GDB), некоторые из которых показаны ниже: 
    
    
     -ex "set disassembly-flavor intel"
     --eval-command="set disassembly-flavor intel"
     -eval-command="set disassembly-flavor intel"
     --eval-command "set disassembly-flavor intel"
    

Обратите внимание, что может потребоваться сбросить отладчик _(Run | Reset Debugger)_ перед загрузкой GDB с новой настройкой. 

# Windows

## Консольный вывод для GUI-приложений

В Windows в среде IDE нет окна «Отладчик - вывод на консоль». Это потому, что консольные приложения открывают свое собственное окно консоли. Приложения с графическим интерфейсом по умолчанию не имеют консоли. Чтобы иметь консоль для приложения с графическим интерфейсом, необходимо изменить настройки компилятора (Project options(Параметры проекта) / Compiler Options(Параметры компилятора) / Config and Target(Настройка и целевая платформа) / Win32 Gui Application(Графическое приложение Win32): -WC / -WG) 

## Отладка приложений с правами Администратора

В Windows Vista+ в разделе Project Options(Параметры проекта) при установке разрешений в файле манифеста для программы «от имени Администратора» программа будет запускаться с правами администратора. Если ваша IDE запущена не от имени Администратора, отладка будет запускаться нормально, программа будет отображаться в списке задач, но ее графический интерфейс не будет отображаться. 

Поэтому имейте в виду, что вы должны сопоставить уровень привилегий между IDE (и gdb) и приложением, которое необходимо отладить. 

# Win 64 bit

  * Требуется Lazarus 1.0+ (с FPC 2.6+)
  * Рекомендуется использовать dwarf



## Использование 32-битного Lazarus на 64-битной Windows

В качестве альтернативы можно отлаживать приложения как 32-разрядные приложения (используя 32-разрядную версию GDB). После успешной отладки: 

  * приложение может быть кросс-скомпилировано до 64 бит или
  * можно использовать 2-ую установку Lazarus (используя другую конфигурационную опцию --primary-config-path из 32-битной Lazarus)



При установке 32-битного Lazarus убедитесь, что вы изменили конфигурацию, чтобы использовались правильные 32-битные FPC и GDB. 

## Больше проблем и решений Win 64 bit

См. также <http://forum.lazarus.freepascal.org/index.php/topic,13188.0/topicseen.html>

# Win CE

## Отладчик не находит никаких исходных файлов

Меню: "Tools"(Сервис) Страница: "Debugger"/"Generic" (Отладчик/Общее) Поле: "Additional search path" (Дополнительный путь поиска) 

Введите в это поле букву вашего каталога проекта. 

# Linux

## Известные проблемы

  * При использовании старой версии GDB могут возникнуть проблемы, по крайней мере, на некоторых процессорах (например, SPARC, с GDB 6.4, поставляемой Debian "Etch"). В частности, фоновые потоки могут блокироваться, даже если программа работает нормально автономно.
  * Если ваша программа блокируется при запуске, перейдите в Tools(Сервис) / Options(Настройки) / Debugger(Отладчик) / General(Общее) и установите `DisableLoadSymbolsForLibraries=True`



# macOS

Под macOS вы обычно устанавливаете GDB с инструментами разработчика Apple. Версия GDB, доступная на момент написания, - 6.3.50. 

Этот раздел - первый подход к сбору информации об известных проблемах и обходных путях. Информация, представленная здесь, во многом зависит от отзывов пользователей macOS. 

Начиная с Lazarus 2.0, в среде IDE имеется отладчик на основе LLDB. Отладчик на основе GDB также может использоваться, но требует много работы по сборке и подписи кода GDB. 

## Известные проблемы (GDB)

### Отладка 32-битного приложения на 64-битной архитектуре

IЭто возможно, а иногда и необходимо отладить 32-битный exe-файл в 64-битной системе. 

Это возможно только с Lazarus 0.9.29 rev 28640 и выше. 

### TimeOuts (только 64 бит)

Похоже, что некоторые команды некорректно (или не полностью) обрабатываются GDB 6.3.50. Обычно GDB заканчивает каждую команду запросом "<gdb>" для следующей команды. Но версия, предоставляемая Apple, иногда не может этого сделать. В этом случае IDE придется использовать тайм-аут, чтобы избежать бесконечного ожидания. 

Это было замечено с: 

  * Определенными выражениями Watch (по крайней мере, если наблюдаемая переменная не была доступна в текущей выбранной функции)
  * Обработкой исключений
  * Вставленными точками останова (окончание последнего модуля / отсутствие кода)



Это не может быть гарантировано: 

  * что GDB не будет позже возвращать некоторые результаты от такой команды
  * что внутреннее состояние GDB все еще действительно



Приглашение, отображаемое для тайм-аутов, может быть отключено в конфигурации отладчика: 

Warn On Timeout
    True/False. Автопродолжение, после тайм-аута не показывается предупреждение.
TimeOutForEval
    Задайте [интервал] в миллисекундах, как долго не срабатывает обнаружение тайм-аута (само обнаружение может занять некоторое время).

Возможно, вам придется «СБРОСИТЬ» отладчик после внесения этих изменений. 

Больше информации смотрите здесь: 

  * <http://bugs.freepascal.org/view.php?id=19262#c47944>
  * <http://bugs.freepascal.org/view.php?id=21653>



Альтернативное решение, похоже, заключается в использовании более новой версии GDB (см. ниже). 

### Hardware exceptions under macOS

When a hardware exception occurs under macOS, such as an invalid pointer access (SIGSEGV) or an integer division-by-zero on Intel processors, the debugger will catch this exception at the Mach level. Normally, the system translates these Mach exceptions into Unix-style signals where the FPC run time library handles them. The debugger is however unable to propagate Mach exceptions in this way. 

The practical upshot is that it is impossible under macOS to continue a program in the debugger once a hardware exception has been triggered. This is not FPC-specific, and cannot be fixed by us. 

## Использование альтернативных отладчиков (GDB)

Вы можете установить GDB 7.1 (или более позднюю версию), используя MacPorts, fink или homebrew. 

Было замечено следующее: 

  * GDB 7.1, похоже, имеет проблемы с отладочной информацией [режима] stabs из fpc.



    **Убедитесь, что вы выбрали _"generate dwarf debug information (-gw)"_(генерировать отладочную информацию dwarf (-gw)) на прилагаемой вкладке _project options_(параметры проекта)**
    **Следите за предупреждениями компоновщика "unknown stabs"**

    Если у вас все еще есть проблемы, убедитесь, что никакой код вообще не скомпилирован [в режиме] stabs. Ваш LCL может содержать [код, скомпилированный в режиме] stabs, и это закончится в вашем приложении, даже если вы скомпилируете приложение с -gw. Поэтому вам, возможно, придется перекомпилировать LCL с помощью -gw (или без какой-либо отладочной информации). То же самое для любого другого модуля, пакета, RTL, ...., которые могут иметь [код, скомпилированный в режиме] stabs.

**Даже при соблюдении этих правил GDB не всегда работает с приложениями, скомпилированными в fpc.**

  
Эти наблюдения могут быть неполными или неправильными, пожалуйста, обновите их, если у вас есть дополнительная информация. 

Lazarus версии от 1.0 до 1.0.12
    

В настройках отладчика сконфигурируйте 
    
    
     EncodeCurrentDirPath = gdfeNone
     EncodeExeFilename = gdfeNone
    

И вы должны использовать _"run param"_ , чтобы указать фактический исполняемый файл внутри пакета приложения (project.app/Content/MacOS/project или аналогичный) 

В Lazarus версии от 1.0.14 до 1.2 и выше эти шаги больше не требуются. 

## Xcode 5

Начиная с Mavericks 10.9, Xcode 5 больше не устанавливает GDB по умолчанию и не [делает его] глобальным. 

  * Для 10.9 вы можете установить старый Xcode 4 параллельно. Это не поддерживается Apple, поэтому он может порвать с одной из следующих версий.


  * Вы можете скомпилировать и установить GDB. См. [GDB on OS X Mavericks and Xcode 5](<../en/GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5").



См., пожалуйста, [issue 25157 on mantis](<http://bugs.freepascal.org/view.php?id=25157>)

  * <http://forum.lazarus.freepascal.org/index.php/topic,22529.0.html> \- об установке Xcode 4.3
  * <http://forum.lazarus.freepascal.org/index.php/topic,22328.0.html> \- установка gdb



Тема "[Lazarus] Help: Проблемы macOS" на 

  * <http://lists.lazarus.freepascal.org/pipermail/lazarus/2013-November/thread.html#84126>
  * <http://lists.lazarus.freepascal.org/pipermail/lazarus/2013-October/thread.html#84095>



## Замороженный UI с GDB

Чтобы запустить программу из оболочки, вам нужно запустить «open <program1>.app», прямой доступ к ./program1 не даст вам доступа к пользовательскому интерфейсу. Чтобы заставить GDB работать изнутри вашей IDE, вы должны указать этот путь в качестве параметра run. Выберите в меню Run > Run parameter и введите полный путь к хост-приложению. Например, /User/<yourid>/sources/myproject/project1.app/Content/MacOS/project1 

## Ссылки

  * macOS поставляется с множеством полезных инструментов для отладки и профилирования. Просто запустите /Developer/Applications/Instruments.app и попробуйте их.



# FreeBSD

GDB, предоставляемый системой, является древним (версия 6.1.1 во FreeBSD 9) и не очень хорошо работает с Lazarus. 

Вы можете установить более новый GDB из дерева портов, например, 
    
    
    cd /usr/ports/devel/gdb
    make -DBATCH install clean
    

Новый GDB находится в /usr/local/bin/gdb 

# Использование переведенного GDB (неанглийские сообщения от GDB)

  * Это относится только к Lazarus *до* 1.2 или *до* 1.0.14.  
В этом больше нет необходимости.



IDE ожидает ответов на английском языке. Если GDB переведен, это может повлиять на то, насколько хорошо IDE может его использовать. 

Во многих случаях это все еще будет работать, но с ограничениями, такими как: 

  * Нет сообщений или класса для исключений
  * Нет живого обновления потоков, пока приложение работает
  * Сообщение об ошибке отладчика после остановки приложения (в конце отладки)
  * другое ...



Пожалуйста, прочитайте всю ветку: [<http://forum.lazarus.freepascal.org/index.php/topic,20756.msg120720>] 

Попробуйте запустить lazarus с 
    
    
     export LANG=en_US.UTF-8
     lazarus
    

# Известные проблемы/ошибки, о которых сообщает IDE

Сообщения  | OС | Описание |   
---|---|---|---  
  
## During startup program exited normally

|   
| **Все** |  В редких случаях в среде IDE появляется сообщение "During startup program exited normally"(Во время запуска программа завершается нормально). Это сообщение появляется, когда отлаженное приложение закрыто. Если это произойдет, никакие точки останова в приложении не будут запущены, и приложение будет работать так, как если бы не было отладчика. Эта ошибка может произойти по разным причинам. Есть как минимум 2 известных: 

  * не зависящий от позиции exe (PIE): GDB не может переместить определенные точки останова, которые Lazarus использует для запуска exe (это происходит до того, как будут установлены какие-либо точки останова пользователя)
  * определенные библиотеки dll/so, которые определяют те же символы, что и основной exe.

Причин может быть еще больше. Второе происходит только при повторных запусках отладчика. IDE пытается обойти это, но это не всегда удается. Вполне вероятно, что в тех случаях, когда в данный момент происходит сбой, это необходимо обойти с помощью пользовательского набора настроек (см. ниже). Даже если вы можете обойти это, рассмотрите возможность отправки файла журнала: [логирование информации для сеанса отладки](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru") **Способы обхода: (все на странице options / debugger)**

  * Только в случае 2: [перестал работать gdb.exe](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru")



Lazarus 1.2.2 и более ранние версии
    перейдите к параметрам отладчика и в поле «debugger_startup_options» введите:  
\--eval-command="set auto-solib-add off"
Lazarus 1.2.4 и выше
    перейдите к параметрам отладчика и установите для поля "DisableLoadSymbolsForLibraries" значение "True".
Это можно использовать, только если вы не отлаживаете библиотеки (если вы не написали свою собственную библиотеку) 

  * Только в случае 2: проверьте "reset debugger after each run"(сбрасывать отладчик после каждого запуска)

Это добавляет очень небольшое увеличение времени, необходимого для запуска отладчика, так как GDB должен каждый раз перезагружаться. 

  * Во всех случаях:

Попробуйте любое из значений, доступных для «InternalStartBreak» (в сетке свойств параметров)   
  
## SIGFPE

|   
| **Все** |  SIGFPE является исключением операций с плавающей точкой. Источник: [[forum thread](<http://forum.lazarus.freepascal.org/index.php/topic,21586.0.html>)] Исключения SIGFPE возникают в следующей инструкции FPU; поэтому строка ошибки, которую вы получаете от Lazarus, может быть отключена/неверна. Delphi, а, следовательно, и Freepascal, имеют нестандартную FPEMask. Многие библиотеки C не используют исключения для проверки проблем FPU, но проверяют состояние FPU. Обходной путь: попробуйте изменить маску с помощью [SetExceptionMask](<http://www.freepascal.org/docs-html/rtl/math/setexceptionmask.html>) как можно раньше в вашей программе: 
    
    
    uses  math;
    ...
     SetExceptionMask([exInvalidOp, exDenormalized, exZeroDivide, 
                       exOverflow, exUnderflow, exPrecision])
    

Если вы считаете, что в коде FPC что-то не так, вы можете использовать директиву [$ SAFEFPUEXCEPTIONS](<http://www.freepascal.org/docs-html/prog/progsu69.html>). Это добавит FWAIT после каждого сохранения значения FPU в памяти. Большинство операций FPC с плавающей запятой и двойных операций заканчиваются сохранением результата где-то в памяти, поэтому в результате отладчик останавливается на правильной строке FPC. FWAIT - недопустимая задача для FPU, но она вызовет немедленное возникновение исключения, а не где-то дальше по вашему коду. Это не делается по умолчанию, потому что это сильно замедляет операции с плавающей запятой.   
  
## Error 193 (Error creating process / Exe not found)

|   
| **Windows** |  Подробнее см. [здесь](<http://bugs.freepascal.org/view.php?id=18238>). Эта проблема наблюдалась в Win XP и, по-видимому, вызвана самой Windows. Проблема возникает, если путь к вашему приложению имеет пробел, и существует второй путь/файл, имя которого совпадает с частью пути перед пробелом, например: 

    Ваше приложение: C:\test folder\project1.exe
    Какой-то файл: C:\test  
  
## SigSegV - даже с новым пустым приложением

|   
| **Windows** | 

Примечание
    в _большинстве_ случаев SigSegV - просто ошибка в вашем коде.
Нижеследующее применимо только [в том случае], если вы получаете SigSegv даже с новым пустым приложением (пустая форма и никаких изменений, внесенных в unit1). Иногда SigSegV может быть вызван несовместимостью между GDB и некоторыми другими продуктами. Не известно, является ли это проблемой GDB или вызвано другим продуктом. Нет никаких признаков того, что какой-либо из перечисленных продуктов каким-либо образом виноват. Предоставленная информация может относиться только к некоторым версиям названных продуктов: 

Comodo firewall
    [Lazarus cant run with comodo firewall](<http://forum.lazarus.freepascal.org/index.php/topic,7065.msg60621.html#msg60621>)
BitDefender
    включен режим игры  
  
## SigSegV - и продолжить отладку

|   
| **Windows** , **Mac** |  [Bug 10004 - gdb cannot continue after SIGFPE or SIGSEGV happen on windows](<http://sourceware.org/bugzilla/show_bug.cgi?id=10004>) [Workaround for SIGFPE bug in GDB for Windows?](<http://forum.lazarus.freepascal.org/index.php/topic,18121.msg101798.html#msg101798>)  
  
## На Windows Open/Save/File или System Dialog вызывает сбой GDB

См. следующую запись [перестал работать gdb.exe](<GDB_Debugger_Tips.md> "GDB Debugger Tips/ru") Или загрузите "alternative GDB"(альтернативный GDB) 7.7.1 (Win32) с сайта Lazarus SourceForge. 

## "Step over" шаги внутри функции (Win 64)

|   
| **Windows 64** |  Иногда нажатие клавиши F8 не переходит [мимо] функции, а входит в нее. Можно продолжить с F8, и иногда GDB возвращается в вызывающую функцию после 1-го или 2-ого дальнейших шагов (переступая через оставшуюся часть функции). В Lazarus 2.0 обходной путь был добавлен, но его необходимо включить. Перейдите в Tools(Сервис) > Options(Настройки) > Debugger(Отладчик) и включите (в сетке свойств) «FixIncorrectStepOver».   
  
## Перестал работать gdb.exe

|   
| **Windows** |  Поврежден сам GDB. Это будет связано с ошибкой в GDB и может быть невозможно исправить в Lazarus. Тем не менее, возможно, стоит отправить информацию (см. Раздел «Сообщения об ошибках» ниже). Возможно, Лазарус может избежать вызова неисправной функции. Есть одна уже известная ситуация. GDB (от 6,6 до 7,4 (последний на момент тестирования)) может аварийно завершить работу во время запуска вашего приложения или сразу после его остановки. Это происходит во время загрузки библиотек (DLL) для вашего приложения (смотрите окно «[Debug output](<IDE_Window__Debug_Output.md> "IDE Window: Debug Output/ru")»). 

Lazarus 1.2.2 и более ранние версии
    перейдите к параметрам отладчика и в поле «debugger_startup_options» введите:  
\--eval-command="set auto-solib-add off"
Lazarus 1.2.4 и выше
    перейдите к параметрам отладчика и установите для поля «DisableLoadSymbolsForLibraries» значение «True».
Это можно использовать, только если вы не отлаживаете в библиотеках (если вы не написали свою собственную библиотеку) [![gdb start opt nolib.png](https://wiki.freepascal.org/images/5/55/gdb_start_opt_nolib.png)](</File:gdb_start_opt_nolib.png>)  
  
## internal-error: clear_dangling_display_expressions

|   
| **Windows, может быть другая ОС** |  В конце сеанса отладки отладчик сообщает: 
    
    
    internal-error: clear_dangling_display_expressions: 
    Assertion `objfile->pspace == solib->pspace' failed.
    

Это ошибка в GDB, и иногда решается так: 

Lazarus 1.2.2 и более ранние версии
    перейдите к параметрам отладчика и в поле "debugger_startup_options" введите:
    
    
    --eval-command="set auto-solib-add off" 
    

Lazarus 1.2.4 и выше
    перейдите к параметрам отладчика и установите для поля "DisableLoadSymbolsForLibraries" значение "True"
Это можно использовать, только если вы не отлаживаете библиотеку (если вы не пишете свою собственную библиотеку)   
  
## "PC register is not available" ("SuspendThread failed. (winerr 5)")

|   
| **Windows** |  GDB может упасть с этим сообщением. Это связано с [Bug 14018](<http://sourceware.org/bugzilla/show_bug.cgi?id=14018>). Если вы получаете эту проблему, вы можете при желании перейти с GDB 7.4 на GDB 7.2   
  
## Can not insert breakpoint (Не могу вставить точку останова)

|   
| **Все** |  **Для точек останова с отрицательными числами:** См. сюда, плз: [How to reset gdb settings](<http://forum.lazarus.freepascal.org/index.php/topic,10317.msg121818.html#msg121818>) Вы также можете попробовать: 

Lazarus 1.2.2 и более ранние версии
    перейдите к параметрам отладчика и в поле «debugger_startup_options» введите:  
\--eval-command="set auto-solib-add off"
Lazarus 1.2.4 и выше
    перейдите к параметрам отладчика и установите для поля «DisableLoadSymbolsForLibraries» значение «True».
Это можно использовать, только если вы не отлаживаете библиотеку (если вы не пишете свою собственную библиотеку) **Для точек останова с положительными числами** Это может произойти из-за неправильных [настроек отладчика](<Debugger_Setup.md> "Debugger Setup/ru"). Убедитесь, что интеллектуальные ссылки отключены. Обычно это означает, что точка останова находится в процедуре, которая не вызывается и не включается в ваш исполняемый файл. В Windows, если это происходит несмотря на правильную настройку, добавьте -Xe (внешний компоновщик) к пользовательским параметрам.   
  
# Сообщения об ошибках

  * Вы проверили свои [настройки отладчика](<Debugger_Setup.md> "Debugger Setup/ru")?
  * Вы пробовали [переключать в режим] Dwarf и Stabs



## Проверьте существующие отчеты

Пожалуйста, проверьте каждую из следующих ссылок 

  * [Search Mantis for category "debugger"](<http://bugs.freepascal.org/search.php?project_id=0&category=Debugger&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&status_id%5B%5D=80&sticky_issues=on&sortby=last_updated&dir=DESC&per_page=250&hide_status_id=-2>)
  * [Search Mantis for text "debugger"](<http://bugs.freepascal.org/search.php?project_id=1&search=debugger&category%5B%5D=-&category%5B%5D=Compiler&category%5B%5D=Converter&category%5B%5D=Custom+Drawn&category%5B%5D=Database&category%5B%5D=Database+Components&category%5B%5D=Documentation&category%5B%5D=FCL&category%5B%5D=FPSpreadsheet&category%5B%5D=FV&category%5B%5D=Free+Vision&category%5B%5D=IDE&category%5B%5D=Installer&category%5B%5D=LCL&category%5B%5D=LazDataDesktop&category%5B%5D=LazReport&category%5B%5D=Misc&category%5B%5D=OnGuard&category%5B%5D=Other&category%5B%5D=Packages&category%5B%5D=Patch&category%5B%5D=Printer&category%5B%5D=RTL&category%5B%5D=TAChart&category%5B%5D=Utilities&category%5B%5D=Virtual+Treeview&category%5B%5D=Web+site&category%5B%5D=Website&category%5B%5D=Widgetset&category%5B%5D=glscene&category%5B%5D=lNet&category%5B%5D=rx&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&status_id%5B%5D=80&sticky_issues=on&sortby=last_updated&dir=DESC&per_page=250&hide_status_id=-2>)
  * [Search Mantis for text "gdb"](<http://bugs.freepascal.org/search.php?project_id=1&search=gdb&category%5B%5D=-&category%5B%5D=Compiler&category%5B%5D=Converter&category%5B%5D=Custom+Drawn&category%5B%5D=Database&category%5B%5D=Database+Components&category%5B%5D=Documentation&category%5B%5D=FCL&category%5B%5D=FPSpreadsheet&category%5B%5D=FV&category%5B%5D=Free+Vision&category%5B%5D=IDE&category%5B%5D=Installer&category%5B%5D=LCL&category%5B%5D=LazDataDesktop&category%5B%5D=LazReport&category%5B%5D=Misc&category%5B%5D=OnGuard&category%5B%5D=Other&category%5B%5D=Packages&category%5B%5D=Patch&category%5B%5D=Printer&category%5B%5D=RTL&category%5B%5D=TAChart&category%5B%5D=Utilities&category%5B%5D=Virtual+Treeview&category%5B%5D=Web+site&category%5B%5D=Website&category%5B%5D=Widgetset&category%5B%5D=glscene&category%5B%5D=lNet&category%5B%5D=rx&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&status_id%5B%5D=80&sticky_issues=on&sortby=last_updated&dir=DESC&per_page=250&hide_status_id=-2>)
  * [Search Mantis for text "dwarf"](<http://bugs.freepascal.org/search.php?project_id=0&search=dwarf&category%5B%5D=-&category%5B%5D=Compiler&category%5B%5D=Converter&category%5B%5D=Custom+Drawn&category%5B%5D=Database&category%5B%5D=Database+Components&category%5B%5D=Documentation&category%5B%5D=FCL&category%5B%5D=FPSpreadsheet&category%5B%5D=FV&category%5B%5D=Free+Vision&category%5B%5D=IDE&category%5B%5D=Installer&category%5B%5D=LCL&category%5B%5D=LazDataDesktop&category%5B%5D=LazReport&category%5B%5D=Misc&category%5B%5D=OnGuard&category%5B%5D=Other&category%5B%5D=Packages&category%5B%5D=Patch&category%5B%5D=Printer&category%5B%5D=RTL&category%5B%5D=TAChart&category%5B%5D=Utilities&category%5B%5D=Virtual+Treeview&category%5B%5D=Web+site&category%5B%5D=Website&category%5B%5D=Widgetset&category%5B%5D=glscene&category%5B%5D=lNet&category%5B%5D=rx&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&status_id%5B%5D=80&sticky_issues=on&sortby=last_updated&dir=DESC&per_page=250&hide_status_id=-2>)
  * [Search Mantis for text "stabs"](<http://bugs.freepascal.org/search.php?project_id=0&search=stabs&category%5B%5D=-&category%5B%5D=Compiler&category%5B%5D=Converter&category%5B%5D=Custom+Drawn&category%5B%5D=Database&category%5B%5D=Database+Components&category%5B%5D=Documentation&category%5B%5D=FCL&category%5B%5D=FPSpreadsheet&category%5B%5D=FV&category%5B%5D=Free+Vision&category%5B%5D=IDE&category%5B%5D=Installer&category%5B%5D=LCL&category%5B%5D=LazDataDesktop&category%5B%5D=LazReport&category%5B%5D=Misc&category%5B%5D=OnGuard&category%5B%5D=Other&category%5B%5D=Packages&category%5B%5D=Patch&category%5B%5D=Printer&category%5B%5D=RTL&category%5B%5D=TAChart&category%5B%5D=Utilities&category%5B%5D=Virtual+Treeview&category%5B%5D=Web+site&category%5B%5D=Website&category%5B%5D=Widgetset&category%5B%5D=glscene&category%5B%5D=lNet&category%5B%5D=rx&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&status_id%5B%5D=80&sticky_issues=on&sortby=last_updated&dir=DESC&per_page=250&hide_status_id=-2>)



### Баги в GDB

Описание | ОС | GDB Version(s) | Ссылка  
---|---|---|---  
Падение на win64 с неправильными символами dwarf2  |  win64  |  GDB 7.4 (может быть и выше)  |  <http://sourceware.org/bugzilla/show_bug.cgi?id=14014>  
Отладчик не видит переменные окружения, установленные в переменной окружения  |  win  |  Решено в GDB 7.4  |  <http://sourceware.org/bugzilla/show_bug.cgi?id=10989>  
Win32 падает с ошибкой "winerr 5" (реестр ПК недоступен)   
*очевидно* не проявляется от 7.0 до 7.2  |  win  |  GDB 7.3 и выше (может быть также в 6.x)   
Кажется, должно быть исправлено в 7.7.1  |  <http://sourceware.org/bugzilla/show_bug.cgi?id=14018>  
Неверный индекс массива с dwarf  |  *  |  GDB от 7.3 до 7.5 (может быть в 7.6.0)   
исправлено в 7.6.1  |  <http://sourceware.org/bugzilla/show_bug.cgi?id=15102>  
GDB падает, если пытается проверить или просмотреть строки ресурсов   
протестировано только на win32, не известно только для других платформ, если используется dwarf |  *  |  GDB от 7.0 до 7.2.xx  |   
Записи (особенно: указатель на запись) могут быть ошибочно приняты за classes/objects   
Данные/значения отображаются корректно / ожидаются дальнейшие тесты  
Это также влияет на способность проверять короткие строки shortstring  |  *  |  GDB 7.6.1 и выше (проверено и не работает до GDB 8.1)  |  <https://sourceware.org/bugzilla/show_bug.cgi?id=16016>  
Объектные переменные (члены) методов текущего класса (self.xxx) не могут быть отслежены/проверены   
Чтобы обойти, используйте верхний регистр или префикс в виде self.  |  *  |  GDB от 7.7 до 7.9.0 (включительно)   
Обходной путь присутствует в Lazarus 1.4 и выше  |  <https://sourceware.org/bugzilla/show_bug.cgi?id=17835>  
GDB не может продолжить работу после SIGFPE или SIGSEGV на Windows  |  win  |  *  |  <https://sourceware.org/bugzilla/show_bug.cgi?id=10004>  
GDB усекает значения для "const x = qword(...)"  |  Win х64   
может быть и другие  |  Исправлено в GDB 7.7 и выше, возможно, исправлено даже между 7.3 и 7.7  |   
  
### Проблемы с GDB 7.5.9 или 7.6

Существуют различные сообщения (не подтвержденные) для разных платформ о сбоях в gdb 7.5.9 или 7.6. 

Настоятельно рекомендуется не использовать эти версии. 

  * <http://bugs.freepascal.org/view.php?id=24401>
  * <http://forum.lazarus.freepascal.org/index.php/topic,20756.msg120842.html#msg120842>
  * <http://sourceware.org/bugzilla/show_bug.cgi?id=15453>



## Create a new Report (GDB and LLDB)

Если вы когда-либо в прошлом обновляли, изменяли, переустанавливали свой Lazarus, тогда, пожалуйста, проверьте диалоговое окно "Options"(Параметры) ("Environment"(Окружение) или меню "Tools"(Сервис)) для [проверки] версии используемых GDB и FPC. Они все еще могут указывать на старые настройки. 

### Основная информация

  * Ваша операционная система и версия
  * Ваш CPU (Intel, Power, ...) **х32 или х64**
  * Версия... 
    * Lazarus (последний стабильный релиз или ревизия SVN) (включите настройки, использующие перекомпиляцию LCL, пакетов или IDE (если используется пользовательская компиляция Lazarus))
    * FPC, если отличается от значения по умолчанию. Пожалуйста, проверьте диалоговое окно "Options"(Настройки)("Environment"(Окружение) или меню "Tools"(Сервис))
    * GDB, пожалуйста, проверьте диалоговое окно "Options"(Настройки)("Environment"(Окружение) или меню "Tools"(Сервис))
  * Настройки компилятора: (меню: "Project"(Проект), "Project Options"(Настройки проекта), указать все настройки, которые вы использовали) 
    * Страница "Compilation and linking"(Компиляция и компоновка)



    

    фрейм "Optimization levels"(Уровни оптимизации) (-O???): Level (-O1 / ...) Other (-OR -Ou -Os) (Пожалуйста, всегда тестируйте с отключенной оптимизацией, и -O1)
    фрейм "Linking"(Компоновка) 

    Smart Link(Умная компоновка) (-XX); **Должно быть отключено(снята галочка)**

  *     * Страница "Debugging"(Отладка)



    

    фрейм "Debugger Info"(Отладочная информация): (-g -gl -gw) (**Пожалуйста, убедитесь, что используется как минимум -g или -gw**) 

    Strip Symbols from executable(Вырезать символы из исполняемого файла) (-Xs); **Должно быть отключено (снята галочка)**

### Логирование информации для сеанса отладки

    Запустите Lazarus со следующими параметрами:
    
    
     --debug-log=LOG_FILE  --debug-enable=DBG_CMD_ECHO,DBG_STATE,DBG_DATA_MONITORS,DBGMI_QUEUE_DEBUG,DBGMI_TYPE_INFO,DBG_ERRORS
    

    

На Windows
    вам нужно создать ярлык для Lazarus.exe (например, на рабочем столе) и отредактировать его свойства. Добавьте " --debug-log=C:\laz.log --debug-enable=..." в командную строку.
На Linux
    запустите Lazarus из оболочки и добавьте " --debug-log=/home/yourname/laz.log --debug-enable=..." в командную строку
На macOS
    запустите Lazarus из оболочки /path/to/lazarus/lazarus.app/Contents/MacOS/lazarus --debug-log=/path/to/yourfiles/laz.log --debug-enable=...

Приложите файл журнала после воспроизведения ошибки. 

Если вы не можете сгенерировать файл журнала, попробуйте открыть окно "Debug output"(Вывод отладки) (из меню "View"(Вид) -> "Ide Internals"(Внутреннее состояние IDE)). **Вы должны открыть его перед запуском приложения (по F9)**. А затем запустите ваше приложение. После возникновения ошибки скопируйте, заархивируйте и прикрепите содержимое окна "Debug output"(Вывод отладки). 

# Тестовые запуски

Если вы используете другую версию GDB, вы можете запустить тестовый пример. Обратите внимание, что не все тесты имеют правильные ожидания для каждой возможной установки. Поэтому некоторые тесты могут не работать на некоторых системах. 

Тест отладчика находится в каталоге: 

  * Lazarus 1.2.x: debugger\test\Gdbmi\
  * Lazarus 1.3 и выше: components\lazdebuggergdbmi\test\



В приведенном ниже пути используется v.1.3. Если используется 1.2.x, замените его соответствующим образом. 

  


Одноразовая настройка
    

Создайте каталог components\lazdebuggergdbmi\test\Logs: Необязательно, но помогает хранить результат теста в одном месте 

Создайте конфигурационные файлы: В каталоге components\lazdebuggergdbmi\test\ 

    copy fpclist.txt.sample to fpclist.txt
    copy gdblist.txt.sample to gdblist.txt

откройте 2 текстовых файла и отредактируйте путь к fpc/gdb, а также отредактируйте версию (тест может запускать разные тесты для разных версий). Можно указать более одного GDB/FPC 

  


Запуск теста
    

  * Откройте проект: components\lazdebuggergdbmi\test\TestGdbmi.lpi
  * Только для Lazarus 1.2.x: пересоберите IDE с опцией "Clean"(Очистить) (Меню "Tools"(Сервис) --> "Configure build Lazarus"(Параметры сборки Lazarus) --> снять галочку с "Restart after building IDE"(Перезапускать после сборки IDE)). Если это пропущено, компиляция проекта может завершиться неудачно. Если компиляция не удалась, ВСЕ *.ppu/*.o файлы в [папке] lazdebuggergdbmi\test должны быть удалены вручную.
  * Запустите проект (если тест не прошел, возможно, вам придется удалить файлы *.exe в components\lazdebuggergdbmi\test\TestApps)



# Ссылки

## Внешние ссылки

  * [Официальный сайт GDB](<https://www.gnu.org/software/gdb/>)



## Экспериментальные отладчики в Паскале

Это работа в процессе: 

  * [duby](<http://sourceforge.net/projects/duby/>)
  * [fpdebug](<https://github.com/graemeg/fpdebug>) от Graeme Geldenhuys
  * [vdbg](<https://code.google.com/p/vdbg/>)
  * fpdebug in Lazarus/components/fpdebug and debugger/components/fpgdbmidebugger
  * [blog](<http://lazarus-dev.blogspot.co.uk/2014/05/de-bug-wars-new-hope.html>)
  * [FpGdbmiDebugger](<../en/FpGdbmiDebugger.md> "FpGdbmiDebugger")


  * [How to register debugger from package?](<http://forum.lazarus.freepascal.org/index.php/topic,22252.0.html>)



## Связанные темы форума

  * [Issues (crash) with gdb 7.6.2 on arch linux](<http://forum.lazarus.freepascal.org/index.php/topic,23221.0.html>)
  * [About the LLDB based debugger](<https://forum.lazarus.freepascal.org/index.php/topic,42869.0.html>)

---

_Source: [https://wiki.freepascal.org/GDB_Debugger_Tips/ru](https://web.archive.org/web/20250418103257/https://wiki.freepascal.org/GDB_Debugger_Tips/ru)_
