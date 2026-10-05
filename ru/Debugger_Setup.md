# Debugger Setup

│ **[English (en)](<../en/Debugger_Setup.md>)** │  **русский (ru)** │

## Contents

  * 1 Настройка IDE
  * 2 Настройки проекта
  * 3 Версия GDB
  * 4 См.также
  * 5 Внешние ссылки



Начиная с Lazarus 2.0, в среде IDE имеется [отладчик на основе LLDB для MacOS](<https://forum.lazarus.freepascal.org/index.php/topic,42869.0.html>). Многие из подписей в IDE все еще явно ссылаются на GDB, но эти опции также применимы к LLDB. 

  


## Настройка IDE

Чтобы иметь возможность отлаживать свои проекты, необходимо убедиться, что среда IDE правильно настроена. 

Эти настройки обычно не меняются. Вам нужно сделать их только один раз после установки Lazarus или если вы изменили/обновили сборку. 

Откройте диалог параметров Lazarus: 

[![Dbg setup options1.png](https://wiki.freepascal.org/images/c/ca/Dbg_setup_options1.png)](</File:Dbg_setup_options1.png>)

На рисунке показано, где можно найти диалоговое окно параметров в Lazarus 0.9.31 и выше. В предыдущих версиях запись находится в меню "Environment"(«Среда»). 

[![Dbg setup options2.png](https://wiki.freepascal.org/images/9/98/Dbg_setup_options2.png)](</File:Dbg_setup_options2.png>)

  * Убедитесь, что выбран параметр «Отладчик GNU ([GDB](<../en/GDB.md> "GDB"))».
  * Путь к gdb.exe может отличаться: 
    * В системах на основе Linux/Unix это может быть что-то вроде "/usr/bin/gdb"
    * В Windows он должен находиться в папке с именем "mingw\bin\" в каталоге, в котором установлен Lazarus.



  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): это справедливо при установке Лазаруса штатным инсталлятором, причем путь в строке Path выглядит примерно так: "$(LazarusDir)\mingw\$(TargetCPU)-$(TargetOS)\bin\gdb.exe". В случае, если вы собираете компилятор вручную, то отладчик лучше скопировать в папку с скомпилированным fpc.exe и при необходимости указать путь к нему там (gdb.exe можно добыть из исходников последнего стабильного релиза компилятора - например, [здесь](<https://sourceforge.net/projects/freepascal/files/Win32/>). Поскольку все равно стабильная версия компилятора необходима для сборки транковой). 

* * *

  
Лазарус 2.0 и выше: В Windows 64 найдите параметр «FixIncorrectStepOver» в сетке свойств и установите для него значение «включено» (true). 

  


  * На MacOS, с Lazarus 2.0 или выше 
    * Select "LLDB (with fpdebug)"(«LLDB (с поддержкой fpdebug)»)
    * Установите путь к: /usr/bin/lldb



## Настройки проекта

Для того чтобы отладить ваш проект, вы должны указать IDE собирать его по специальному пути, который предоставит дополнительную информацию, необходимую для отладчика. 

Обратите внимание: это значительно увеличит размер вашего исполняемого файла ([см. FAQ](<Lazarus_Faq.md> "Lazarus Faq/ru")). Если вы хотите собрать релизную версию своего программного продукта, вам следует отключить эти настройки (см.также [Режимы сборки](<../en/IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")) 

Необходимые настройки выполняются в диалоговом окне "Project Options"(«Параметры проекта»): 

  
[![Dbg setup project1.png](https://wiki.freepascal.org/images/9/92/Dbg_setup_project1.png)](</File:Dbg_setup_project1.png>) [![Dbg setup project2.png](https://wiki.freepascal.org/images/a/a0/Dbg_setup_project2.png)](</File:Dbg_setup_project2.png>)

  


  * Вы должны включить опцию "Generate Debug Info for GDB"(«Создать информацию отладки для GDB») 
    * В 32-битной Windows/Linux настоятельно рекомендуется использовать режим "Dwarf"
  * Если вы используете отладчик, основанный на LLDB, вы не можете использовать режим "Stabs". Вы можете выбрать любой из вариантов режима "Dwarf". Лучше всего установить режим явно, так как режим "automatic" зависит от вашей версии fpc.



  
[![Dbg setup project3.png](https://wiki.freepascal.org/images/b/b3/Dbg_setup_project3.png)](</File:Dbg_setup_project3.png>)

  * Вы **не** должны использовать любую [опцию] из следующих 
    * "Strip Symbols"
    * "Link Smart"
    * Любую оптимизацию, отличную от "Level 0" (можно использовать "Level 1", но в некоторых случаях это может вызвать проблемы)



  
[![Dbg setup project4.png](https://wiki.freepascal.org/images/3/36/Dbg_setup_project4.png)](</File:Dbg_setup_project4.png>)

## Версия GDB

GDB 7.5 требует Lazarus 1.4 или выше. 

GDB 7.7.1, похоже, хорошо работает с Lazarus 1.2.4. 

На MacOS: LLDB является частью инструментов разработчика от Apple 

## См.также

  * [Параметры отладчика](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options/ru")
  * [IDE Window: Run parameters](<../en/IDE_Window__Run_parameters.md> "IDE Window: Run parameters") Это меню также охватывает некоторые параметры, связанные с отладкой.
  * [Подсказки отладчика GDB](<../en/GDB_Debugger_Tips.md> "GDB Debugger Tips")



## Внешние ссылки

  * [Setup Video Tutorial](<http://www.youtube.com/watch?v=cf4G06k2YL8>)

---

_Source: [https://wiki.freepascal.org/Debugger_Setup/ru](https://web.archive.org/web/20200812234025/https://wiki.freepascal.org/Debugger_Setup/ru)_
