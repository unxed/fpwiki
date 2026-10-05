# Creating a Backtrace with GDB

│ **[English (en)](<../en/Creating_a_Backtrace_with_GDB.md>)** │  **русский (ru)** │

## Contents

  * 1 Причины для создания обратной трассировки
    * 1.1 Отчеты об ошибках
    * 1.2 Помоги себе найти проблему
    * 1.3 Это просто весело!!!
  * 2 Создание Backtrace
    * 2.1 Очень короткая инструкция
    * 2.2 Напрямую из окна стека вызовов
    * 2.3 Пользователи Windows
    * 2.4 Пользователи Linux/BSD
    * 2.5 macOS
      * 2.5.1 macOS: Графическое приложение
      * 2.5.2 macOS: приложение без GUI, консольная программа
    * 2.6 GDB, готовый принимать команды
  * 3 Использование lldb на Mac
  * 4 Получение Backtrace из терминала и на электронную почту
  * 5 Интерпретация результатов Backtrace
  * 6 См. также



## Причины для создания обратной трассировки

### Отчеты об ошибках

Файлы обратной трассировки могут быть очень полезны при отправке отчетов об ошибках. Они, несомненно, повысят вероятность того, что ошибка будет исправлена раньше. 

### Помоги себе найти проблему

Обратная трассировка может действительно облегчить поиск проблемы с вашей собственной программой. Часто проблема выявляется сразу. 

### Это просто весело!!!

Хорошо, это может быть преувеличением, но делать обратную трассировку просто весело! 

Если серьезно, если вы можете придумать другую причину создания обратной трассировки, добавьте ее сюда. 

## Создание Backtrace

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** **Mac-пользователям** следует использовать _lldb_ вместо _gdb_.

Чтобы создать обратную трассировку с помощью [GDB](<../en/GDB.md> "GDB") (Gnu DeBugger): 

  1. У вас должна быть программа [GDB](<http://www.gnu.org/software/gdb/gdb.html>). Она доступна на большинстве установленных ОС-*nix. Если вы скачали версию fpc или Lazarus для Windows, GDB уже включен в поставку. В Lazarus он установлен, как отладчик, по умолчанию. Посмотрите в Tools (Сервис) -> Options (Параметры) -> Debugger (Отладчик) -> General Tab (Общие). Путь по умолчанию для GDB в Windows: c:\lazarus\mingw\bin\gdb.exe.
  2. GDB должен быть прописан в переменной окружения PATH. Если это Windows, из командной строки введите (пример): установите PATH =% PATH%; set PATH=%PATH%;c:\lazarus\mingw\bin\
  3. Программа, которую вы хотите отладить, ДОЛЖНА иметь отладочную информацию, включенную в исполняемый файл. Если вы используете "strip", "upx", компилируете с "Xs" или использовали какой-либо другой exe-упаковщик, это не сработает!



* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): если вы используете транковые svn-сборки FPC для Windows, то после компиляции gdb.exe в них отсутствует.Его необходимо скопировать (вместе с некоторыми другими утилитами) из папки _< Stable-FPC RootDir>\bin\_ дистрибутива стабильной версии компилятора (при помощи которой вы и собираете транк FPC, например, на момент перевода статьи это fpc 3.0.4) в папку _< Trank-FPC RootDir>/bin/$(TargetCPU)-$(TargetOS)/_, где находится fpc.exe. 

* * *

### Очень короткая инструкция

Linux терминал: 
    
    
      $ cd program_folder
      $ gdb program_name
      run
    

Если случилась AV. Пишем: 
    
    
      bt
    

### Напрямую из окна стека вызовов

Чтобы захватить Backtrace изнутри Lazarus, убедитесь, что вы скомпилировали программу, включающую оба параметра "Generate debug info for GDB" (Генерировать отладочную информацию для GDB) и "Display line numbers in run-time error backtraces (-gl)" (Выдавать номера строк в ошибках времени исполнения (-gl)), и вы правильно настроили GDB в параметрах отладчика. 

  1. Запускайте ваше приложение, пока не появится проблема
  2. Если появится диалоговое окно с предупреждением о проблеме, показывающее пару кнопок: "Break" (Прервать) и "Continue" (Продолжить), нажмите кнопку "Break".
  3. Откройте окно Call Stack (Стек вызовов) (Menu View (Вид) -> Debug Windows (Окна отладки) ->Call Stack (Стек вызовов)).
  4. Просмотрите список вызовов, при необходимости нажимая "(+)More" (Подробнее)
  5. Нажмите кнопку "Copy All" (Копировать все).
  6. Backtrace теперь в вашем буфере обмена и готов к вставке.



### Пользователи Windows

1\. Откройте окно приглашения MS-DOS 

2\. Перейдите на диск, на котором у вас есть exe: 
    
    
    C: {нажмите enter}
    

3\. Перейдите в каталог, содержащий exe: 
    
    
    cd \myprograms\Project1 {нажмите enter}
    

4\. Загрузите вашу программу с использованием GDB: 
    
    
    gdb Project1.exe {нажмите enter}
    

### Пользователи Linux/BSD

1\. Откройте вашу любимую программу Terminal 

2\. Перейдите в каталог, содержащий исполняемый файл: 
    
    
    cd $HOME/myprograms/project1 {нажмите enter}
    

3\. Загрузите вашу программу с использованием GDB: 
    
    
    gdb project1 {нажмите enter}
    

### macOS

GDB по умолчанию недоступен для macOS. Вы можете рассмотреть возможность использования отладчика MacOS по умолчанию [lldb](<../en/Creating_a_Backtrace_with_lldb.md> "Creating a Backtrace with lldb"). 

#### macOS: Графическое приложение

1\. Если это еще не сделано: создайте комплект приложений (Project (Проект)/ Project Options (Параметры проекта)/ Application (Приложение)/ Create Application Bundle (Создать комплект приложений)) 

2\. Откройте вашу любимую программу терминала 

3\. Перейдите в каталог, содержащий пакет приложения / исполняемый файл: 
    
    
    cd $HOME/myprograms/project1 {нажмите Enter}
    

4\. Загрузите вашу программу с GDB: 
    
    
    gdb project1.app/Contents/MacOS/project1 {нажмите Enter}
    

#### macOS: приложение без GUI, консольная программа

1\. Откройте вашу любимую программу терминала 

2\. Перейдите в каталог, содержащий исполняемый файл: 
    
    
    cd $HOME/myprograms/project1 {нажмите Enter}
    

3\. Загрузите вашу программу с GDB: 
    
    
    gdb project1 {нажмите Enter}
    

### GDB, готовый принимать команды

Теперь вы должны увидеть подсказку, которая выглядит примерно так: 
    
    
    GNU gdb 6.3.50.20050217-cvs
    Copyright 2004 Free Software Foundation, Inc.
    GDB is free software, covered by the GNU General Public License, and you are
    welcome to change it and/or distribute copies of it under certain conditions.
    Type "show copying" to see the conditions.
    There is absolutely no warranty for GDB.  Type "show warranty" for details.
    This GDB was configured as "i686-pc-linux-gnu".
    (gdb) _
    

**Пусть GDB остановится при возникновении исключения**
    
    
    (gdb) break fpc_raiseexception {Нажмите Enter}
    

**Теперь запустите вашу программу:**
    
    
    (gdb) run {Нажмите Enter}
    

Ваша программа будет работать в обычном режиме, пока не произойдет сбой или не произойдет ошибка. 

Когда ваша программа аварийно завершает работу вместо обычной ошибки «Нарушение прав доступа» или «Деление на ноль», ваша программа просто останавливается, не отвечает и не перерисовывается. Это потому, что GDB поймал ошибку и ждет, когда вы скажете ему, что делать. 

Переключитесь в окно MS-DOS или Терминал, и у вас должен появиться вывод на экран, похожий на этот: 
    
    
    Program received signal SIGFPE, Arithmetic exception.
    0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    Current language:  auto; currently pascal
    (gdb) _
    

Чтобы создать обратную трассировку, введите: 
    
    
    backtrace {Нажмите Enter}
    

или 
    
    
    bt {Нажмите Enter}
    

Теперь у вас должен быть вывод на экран, который выглядит примерно так: 
    
    
    #0  0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    #1  0x080ecd78 in TCONTROL__CLICK (this=0x403d8bd8) at control.inc:1768
    #2  0x081274bd in TBUTTONCONTROL__CLICK (this=0x403d8bd8) at buttoncontrol.inc:57
    #3  0x08129171 in TCUSTOMBUTTON__CLICK (this=0x403d8bd8) at buttons.inc:186
    #4  0x08129198 in TCUSTOMBUTTON__WMDEFAULTCLICKED (MESSAGE={MSG = 1031, WPARAM = 
            136821192, LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, 
            WPARAMHI = 2087, LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at buttons.inc:198
    #5  0x0805bbf7 in SYSTEM_TOBJECT_$__DISPATCH$formal ()
    #6  0x080ec6e0 in TCONTROL__WNDPROC (THEMESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548,  PARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at control.inc:1447 
    #7  0x080e4ed9 in TWINCONTROL__WNDPROC (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548,  PARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at wincontrol.inc:2154
    #8  0x08161955 in DELIVERMESSAGE (TARGET=0x403d8bd8, AMESSAGE=void) at gtkproc.inc:3242
    #9  0x0818897c in GTKWSBUTTON_CLICKED (AWIDGET=0x8285380, AINFO=0x40453794) at gtkwsbuttons.pp:112
    ... { Truncated for simplicity} Complete listing [here](<../en/Creating_a_Backtrace_with_GDB_Backtrace.md> "Creating a Backtrace with GDB\\Backtrace")
    (gdb) _
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если лог обратной трассировки слишком длинный, вам будет предложено нажать Enter для перехода к следующей странице лога обратной трассировки, пока она не будет завершена.

Информация, отображаемая здесь, имеет последовательность событий до того места, где произошла ошибка. См. [ Интерпретация результатов Backtrace](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB/ru")

Для выхода из GDB (и вашей программы) вы можете набрать: 
    
    
    quit {Нажмите Enter}
    

или продолжить работу до следующего типа исключения: 
    
    
    cont {Нажмите Enter}
    

_Подсказка: в любой момент, когда ваша программа запущена, вы можете нажать` Ctrl` \+ `C` (из терминала или из командной строки MS-DOS), чтобы получить приглашение "(gdb) _"_

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Использование английской раскладки клавиатуры может предотвратить проблемы с GDB (например, если вы вводите специальные символы, такие как ' \ ').

## Использование lldb на Mac

Запускаем lldb 
    
    
    lldb
    

Загружаем ваш проект 
    
    
    target create project1.app/MacOs/Contents/project1
    

Устанавливаем точку останова  
Вы можете пропустить это, если проект падает, вместо того, чтобы вызывать исключение fpc. Если есть ожидаемые исключения, которые будут обработаны блоком `try..xcept`, тогда используйте "c" для продолжения 
    
    
    breakpoint set --func fpc_raiseexception
    

Запускаем его 
    
    
    r
    

Когда он остановится, получаем backtrace 
    
    
    bt
    

показать локальные снимки для разных кадров (замените 1 в выделенном кадре на другой кадр, например, верхний 3 или верхний 5) 
    
    
    frame select 1
    frame variable  -P 1
    

## Получение Backtrace из терминала и на электронную почту

В настоящее время мы создали лог обратной трассировки и увидели его в терминале (или в приглашении MS-DOS), но как мне получить его в электронном письме? Есть два способа, о которых я знаю: 

  1. Использовать функцию копирования/вставки вашего терминала (или приглашения MS-DOS)
  2. Перенаправить вывод GDB в файл следующим образом:


    
    
    gdb project1 > output.txt
    

Теперь вы можете открыть файл и скопировать/вставить его в свою электронную почту. 

Предостережение: второй метод требует, чтобы вы печатали вслепую. Таким образом, вы должны набрать "run", чтобы запустить программу, и "backtrace", чтобы создать обратную трассировку как обычно, но на экране ничего не будет отображаться. 

Исключение: в Linux вы можете обойтись без возможности увидеть следующую команду: 
    
    
    touch output.txt & tail output.txt -f & gdb project1 > output.txt
    

## Интерпретация результатов Backtrace

Стек содержит все вызовы в тот момент, когда произошла ошибка деления на ноль. В позиции #0 находится последний вызов "button1-click()". subproc-call #37 - это первый вызов main () всего приложения. 

TODO: Напишите мне :) 
    
    
    (gdb) bt
    #0  0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    #1  0x080ecd78 in TCONTROL__CLICK (this=0x403d8bd8) at control.inc:1768
    #2  0x081274bd in TBUTTONCONTROL__CLICK (this=0x403d8bd8) at buttoncontrol.inc:57
    #3  0x08129171 in TCUSTOMBUTTON__CLICK (this=0x403d8bd8) at buttons.inc:186
    #4  0x08129198 in TCUSTOMBUTTON__WMDEFAULTCLICKED (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at buttons.inc:198
    #5  0x0805bbf7 in SYSTEM_TOBJECT_$__DISPATCH$formal ()
    #6  0x080ec6e0 in TCONTROL__WNDPROC (THEMESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at control.inc:1447
    #7  0x080e4ed9 in TWINCONTROL__WNDPROC (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI =   16416},
            this=0x403d8bd8) at wincontrol.inc:2154
    #8  0x08161955 in DELIVERMESSAGE (TARGET=0x403d8bd8, AMESSAGE=void) at gtkproc.inc:3242
    #9  0x0818897c in GTKWSBUTTON_CLICKED (AWIDGET=0x8285380, AINFO=0x40453794) at gtkwsbuttons.pp:112
    #10 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #11 0x40206e5a in gtk_signal_remove_emission_hook () from /usr/lib/libgtk-1.2.so.0
    #12 0x4020616a in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #13 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #14 0x40176e1c in gtk_button_clicked () from /usr/lib/libgtk-1.2.so.0
    #15 0x4017838a in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #16 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #17 0x4020607b in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #18 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #19 0x40176d60 in gtk_button_released () from /usr/lib/libgtk-1.2.so.0
    #20 0x40177d26 in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #21 0x401d764d in gtk_marshal_BOOL__POINTER () from /usr/lib/libgtk-1.2.so.0
    #22 0x4020619f in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #23 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #24 0x40239fd9 in gtk_widget_event () from /usr/lib/libgtk-1.2.so.0
    #25 0x401d74d0 in gtk_propagate_event () from /usr/lib/libgtk-1.2.so.0
    #26 0x401d65c1 in gtk_main_do_event () from /usr/lib/libgtk-1.2.so.0
    #27 0x40065b44 in gdk_wm_protocols_filter () from /usr/lib/libgdk-1.2.so.0
    #28 0x4003de75 in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #29 0x4003e32c in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #30 0x4003e4f5 in g_main_iteration () from /usr/lib/libglib-1.2.so.0
    #31 0x401d6388 in gtk_main_iteration_do () from /usr/lib/libgtk-1.2.so.0
    #32 0x080b4d26 in TGTKWIDGETSET__WAITMESSAGE (this=0x40473014) at gtkobject.inc:1427
    #33 0x08070073 in TAPPLICATION__IDLE (this=0x4045b014) at application.inc:319
    #34 0x08070ffb in TAPPLICATION__HANDLEMESSAGE (this=0x4045b014) at application.inc:827
    #35 0x08071380 in RUNMESSAGE (parentfp=0xbffff5a4) at application.inc:933
    #36 0x080712d7 in TAPPLICATION__RUN (this=0x4045b014) at application.inc:944
    #37 0x080531c3 in main () at project1.lpr:13
    (gdb)
    

Этот Backtrace был создан при использовании этого кода: 
    
    
     30. procedure TForm1.Button1Click(Sender: TObject);
     31. var
     32.  X, Y, Z: Integer;
     33. begin
     34.   X := 0;
     35.   Y := 0;
     36.   Z := X div Y;   //   ошибка деления на ноль
     37. end;
    

## См. также

  * [Creating a Backtrace with lldb](<../en/Creating_a_Backtrace_with_lldb.md> "Creating a Backtrace with lldb")

---

_Source: [https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB/ru)_
