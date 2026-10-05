# global compiler directives

│ **[Deutsch (de)](</global_compiler_directives/de> "global compiler directives/de")** │  **[English (en)](<../en/global_compiler_directives.md> "global compiler directives")** │  **[français (fr)](</global_compiler_directives/fr> "global compiler directives/fr")** │  **русский (ru)** │    
****

Free Pascal поддерживает [директивы компилятора](<../en/Compiler_directive.md> "Compiler directive") директивы компилятора в [исходном файле](<../en/Source_code.md> "Source code"). В основном поддерживаются те же директивы, что и в Turbo Pascal, Delphi и Apple Pascal (Mac OS). Некоторые из них признаны только для совместимости и не имеют никакого эффекта. 

## Contents

  * 1 Синтаксис
  * 2 Генерация кода
  * 3 Включение данных
  * 4 Пути
  * 5 Целезависимые
    * 5.1 Только Novell NetWare
    * 5.2 Только Palm OS и Garnet OS
    * 5.3 Windows-подобные системы
    * 5.4 Разное
  * 6 Данные времени компиляции
  * 7 Игонорируемые
  * 8 См. также



## Синтаксис

Общий: 

  * [`{$mode}`](</index.php?title=$mode&action=edit&redlink=1> "$mode \(page does not exist\)") выбирает режим компилятора
  * [`{$modeSwitch}`](</index.php?title=$modeSwitch&action=edit&redlink=1> "$modeSwitch \(page does not exist\)") включает или выключает определенные функции режима



Конкретный: 

  * [`{$extendedSyntax}`](<../en/$extendedSyntax.md> "$extendedSyntax") позволяет использовать функции, как если бы они были процедурами ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): т.е. результат вызова функции не обязан присваиваться переменной)
  * [`{$pointerMath}`](</index.php?title=$pointerMath&action=edit&redlink=1> "$pointerMath \(page does not exist\)")позволяет арифметические операции с указателями (начиная с [FPC 2.6.0](<../en/User_Changes_2.6.md> "User Changes 2.6.0"))
  * [`{$openStrings}` или `{$P}`](</index.php?title=$openStrings&action=edit&redlink=1> "$openStrings \(page does not exist\)") определяет, все ли стандартные параметры типа `string` считаются параметрами открытых строк; этот параметр действует только для коротких строк(`ShortString`), но не для `ANSIString`.
  * [`{$varPropSetter}`](</index.php?title=$varPropSetter&action=edit&redlink=1> "$varPropSetter \(page does not exist\)")



## Генерация кода

  * [`{$codePage}`](<../en/$codePage.md> "$codePage") определяет, какая кодовая страница используется программой
  * [`{$E}`](</index.php?title=$E&action=edit&redlink=1> "$E \(page does not exist\)") эмулирует сопроцессор
  * [`{$extension}`](</index.php?title=$extension&action=edit&redlink=1> "$extension \(page does not exist\)") определяет суффикс имени сгенерированного [исполняемого](<../en/Executable_program.md> "Executable program") файла
  * [`{$libPrefix}`](</index.php?title=$libPrefix_and_$libSuffix&action=edit&redlink=1> "$libPrefix and $libSuffix \(page does not exist\)") определяет префикс имени файла сгенерированной библиотеки
  * [`{$libSuffix}`](</index.php?title=$libPrefix_and_$libSuffix&action=edit&redlink=1> "$libPrefix and $libSuffix \(page does not exist\)") определяет суффикс имени файла сгенерированной библиотеки
  * [`{$memory}`](</index.php?title=$memory&action=edit&redlink=1> "$memory \(page does not exist\)") определяет размер используемой памяти
  * [`{$PascalMainName}`](</index.php?title=$pascalMainName&action=edit&redlink=1> "$pascalMainName \(page does not exist\)") определяет имя точки входа
  * [`{$PIC}`](</index.php?title=$PIC&action=edit&redlink=1> "$PIC \(page does not exist\)") позволяет позиционно-независимую генерацию кода
  * [`{$smartlink}`](</index.php?title=$smartlink&action=edit&redlink=1> "$smartlink \(page does not exist\)") определяет умное связывание
  * [`{$sysCalls}`](</index.php?title=$sysCalls&action=edit&redlink=1> "$sysCalls \(page does not exist\)") определяет правила вызова системных вызовов Amiga/MorphOS



## Включение данных

  * [`{$debugInfo}` или `{$D}`](</index.php?title=$debugInfo&action=edit&redlink=1> "$debugInfo \(page does not exist\)") вставляет отладочную информацию GNU в сгенерированный код
  * [`{$referenceInfo}` или `{$Y}`](</index.php?title=$referenceInfo&action=edit&redlink=1> "$referenceInfo \(page does not exist\)") создает Delphi-совместимую информацию о браузере (пока поддерживается не полностью)



## Пути

  * [`{$frameworkPath}`](</index.php?title=$framework&action=edit&redlink=1> "$framework \(page does not exist\)") (для Darwin)
  * [`{$includePath}`](<../en/$include.md> "$include") определяет путь для включаемых файлов
  * [`{$libraryPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") определяет путь к файлам библиотеки
  * [`{$objectPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") задает путь для поиска объектных файлов
  * [`{$unitPath}`](</index.php?title=FPC_paths&action=edit&redlink=1> "FPC paths \(page does not exist\)") определяет путь поиска для модулей



## Целезависимые

### Только Novell NetWare

  * [`{$copyright}`](</index.php?title=$copyright&action=edit&redlink=1> "$copyright \(page does not exist\)") вставляет информацию об авторских правах
  * [`{$screenName}`](</index.php?title=$screenName&action=edit&redlink=1> "$screenName \(page does not exist\)") определяет отображаемое имя приложения
  * [`{$threadName}`](</index.php?title=$threadName&action=edit&redlink=1> "$threadName \(page does not exist\)") задает имя потока



### Только Palm OS и Garnet OS

  * `{$appID}` задает четырехсимвольный идентификатор приложения
  * `{$appName}` определяет название приложения



### Windows-подобные системы

  * [`{$imageBase}`](</index.php?title=$imageBase&action=edit&redlink=1> "$imageBase \(page does not exist\)") указывает базовое местоположение образа DLL
  * `{$minStackSize}` устанавливает минимальный размер стека для исполняемого файла
  * `{$maxStackSize}` устанавливает максимальный размер стека для исполняемого файла
  * `{$setPEFlags}` устанавливает флаги PE в Windows
  * `{$version}` задает номер версии DLL



### Разное

  * [`{$appType}`](</index.php?title=$appType&action=edit&redlink=1> "$appType \(page does not exist\)") задает тип программы (CONSOLE, GUI и т.д.)



## Данные времени компиляции

  * [`{$profile}`](</index.php?title=$profile&action=edit&redlink=1> "$profile \(page does not exist\)") эта директива включает или выключает генерацию кода профилирования.



## Игонорируемые

  * `{$description}`: введен для совместимости и с FPC 3.0.4 игнорируется
  * `{$G}` будет генерировать код 80286 с [TP](<../en/Turbo_Pascal.md> "Turbo Pascal")
  * `{$localSymbols}` или `{$L}` Этот параметр (не путать с локальной директивой связывания файла {$ L file}) распознается для совместимости с Turbo Pascal, но игнорируется.
  * `{$N}` распознается для совместимости с Turbo Pascal, но в остальном игнорируется, поскольку компилятор всегда использует сопроцессор для математических вычислений с плавающей точкой.
  * `{$O}` включает уровень 2 оптимизации. Это больше не распознается начиная с FPC 2.0.0. Используйте взамен [`{$optimization}`](</index.php?title=$optimization&action=edit&redlink=1> "$optimization \(page does not exist\)").
  * `{$weakPackageUnit}` анализируется для совместимости с Delphi, но в противном случае игнорируется. Компилятор выдаст предупреждение при его обнаружении.



## См. также

  * [Pascal basics](<../en/Pascal_basics.md> "Pascal basics")
  * [§ “global directives” in the _Free Pascal programmer’s guide_](<https://www.freepascal.org/docs-html/prog/progse3.html>)

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<../en/global_compiler_directives.md> "global compiler directives") • [local compiler directives](<../en/local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<../en/Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<../en/Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<../en/Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<../en/Platform_defines.md> "Platform defines")  
[$IF](<../en/$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/global_compiler_directives/ru](https://web.archive.org/web/20250418104558/https://wiki.freepascal.org/global_compiler_directives/ru)_
