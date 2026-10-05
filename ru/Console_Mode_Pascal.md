# Console Mode Pascal

│ **[Deutsch (de)](</Console_Mode_Pascal/de> "Console Mode Pascal/de")** │  **[English (en)](<../en/Console_Mode_Pascal.md> "Console Mode Pascal")** │  **[español (es)](</Console_Mode_Pascal/es> "Console Mode Pascal/es")** │  **[suomi (fi)](</Console_Mode_Pascal/fi> "Console Mode Pascal/fi")** │  **[magyar (hu)](</Console_Mode_Pascal/hu> "Console Mode Pascal/hu")** │  **[日本語 (ja)](</Console_Mode_Pascal/ja> "Console Mode Pascal/ja")** │  **русский (ru)** │  **[slovenčina (sk)](</Console_Mode_Pascal/sk> "Console Mode Pascal/sk")** │    
****

## Contents

  * 1 Программирование в консольном режиме Pascal
    * 1.1 Программирование в консольном режиме без Lazarus
    * 1.2 Программирование в консольном режиме с помощью Lazarus
      * 1.2.1 Тип проекта: Console Application (Консольное приложение)
      * 1.2.2 Тип проекта: Program (Программа)
    * 1.3 Pascal scripts
  * 2 Запуск программы
    * 2.1 Компиляция
    * 2.2 Запуск в консоли
    * 2.3 Запуск в IDE
    * 2.4 Запуск в IDE с перенаправлением вывода
  * 3 Unicode (UTF8) вывод
  * 4 Примеры
    * 4.1 Выполнение команд оболочки
      * 4.1.1 Пример: обновить FPC и Lazarus
    * 4.2 Пример: корректировка fpc и lazarus
  * 5 Примечание для пользователей Windows
  * 6 См.также



## Программирование в консольном режиме Pascal

Многие из нас писали [ программы](<../en/Program.md> "Program") на [Паскале](<../en/Pascal.md> "Pascal") задолго до того, как вошли в моду [Графический интерфейс пользователя](<../en/Graphical_User_Interface.md> "Graphical User Interface") (GUI) и [Интегрированные среды разработки (IDE)](<../en/IDE.md> "IDE"). Многие другие являются новичками в программировании на Pascal, и им необходимо уметь опробовать основные инструменты языка. Третьим нужны [консольные](<../en/Command-line_interface.md> "Command-line interface") приложения или приложения [текстового режима](<../en/Text_User_Interface.md> "Text User Interface") для выполнения сложных задач управления системой. 

### Программирование в консольном режиме без Lazarus

Хотя многие предпочитают использовать [Lazarus IDE](<../en/Lazarus_IDE.md> "Lazarus IDE") для написания программ в режиме консоли, вы также можете написать их с помощью любого текстового редактора и скомпилировать их самостоятельно, вызвав [компилятор](<../en/Compiler.md> "Compiler") [FPC](<../en/FPC.md> "FPC"), например так для программы в example.pas: 
    
    
    fpc example.pas
    

Также доступен текстовый режим IDE _fp_ (немного похожий на старый [Turbo Pascal](<../en/Turbo_Pascal.md> "Turbo Pascal")).В следующих разделах мы сосредоточимся на программировании с помощью [Lazarus](<../en/Lazarus.md> "Lazarus"). 

### Программирование в консольном режиме с помощью Lazarus

Lazarus предоставляет идеальную среду для изучения Паскаля и разработки программ в текстовом режиме. Можно использовать все функции интегрированной среды разработки, включая редактор исходного кода с ее [ подсветкой синтаксиса](<../en/Syntax_highlighting.md> "Syntax highlighting"), доступом к библиотекам, сложными инструментамиы поиска и завершения кода, а также проверкой синтаксиса. Если вам не нужна [Form](<../en/TForm.md> "TForm") с ее визуальными компонентами, можно и без нее, но редактор исходного кода Lazarus по-прежнему остается отличной средой для разработки программ. Вы можете компилировать и запускать свою программу во время разработки, не выходя из редактора. 

Чтобы запустить программу в режиме консоли, перейдите в главное меню и выберите Project -> New Project(Проект - новый проект), а затем выберите **Program**(Программа) или **Console Application**(Консольное приложение). Среда IDE не будет генерировать все дополнительные файлы, связанные с полным графическим приложением, и не откроет окно инспектора объектов, но откроет редактор исходного кода со структурой скелетной программы и будет ожидать ввода вашего программирования. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В Windows приложение с графическим интерфейсом пользователя не имеет консоли и, следовательно, не может выполнить **writeln** или **readln**. Вы получите ошибку _File not open_(Файл не открыт). Снимите галочку в разделе _Project Options / Compiler Options / Linking / Target specific options / Win32 GUI application_(Параметры проекта / Параметры компилятора / Связывание / Частные параметры цели / Приложение Win32 GUI), чтобы создать консольное приложение. Начиная с Lazarus IDE 1.4 она находится в разделе _Project(menu) / Project Options / Compiler Options / Config and Target / Target specific options / Win32 GUI application_(Проект (меню) / Параметры проекта / Параметры компилятора / Настройка и целевая платформа / Частные параметры цели / Приложение Win32 GUI).

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Lazarus имеет некоторые функции для программ командной строки, такие как [copyfile](<../en/CopyFile.md> "CopyFile"). Чтобы использовать их, добавьте требование к проекту для LazUtils (Project / Project Inspector / Required Packages --> "Add" --> New Requirement --> choose "LazUtils" --> OK), которое не будет задействовать весь LCL. Затем добавьте соответствующий модуль в раздел uses.

#### Тип проекта: Console Application (Консольное приложение)

Выбор этого типа проекта в Lazarus создает программу с новым классом, производным от TCustomApplication. TCustomApplication предоставляет много всяких штучек и упрощает программирование утилит командной строки. Например, проверка параметров командной строки, написание справки, проверка [ переменных окружения](<../en/Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables") и обработка [ исключений](<../en/Exceptions.md> "Exceptions"). Все программы [LCL](<../en/LCL.md> "LCL") используют это автоматически. 

#### Тип проекта: Program (Программа)

В качестве демо мы напишем очень минималистичную программу на Паскале. В среде IDE выберите тип проекта Program(Программа), и пусть среда IDE вам немного поможет. Например, когда вы добавляете еще один [модуль](<../en/Unit.md> "Unit"), IDE автоматически добавит имя модуля в раздел программы [ uses](<../en/Uses.md> "Uses"). Это поведение определяется в параметрах проекта. Таким образом, вы можете переключаться между 'Program'(Программой) и 'Custom Program'(Пользовательской программой) в любое время. 

Пример для начинающих: 
    
    
    program Project1;
    {$mode objfpc}{$H+}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes;
    begin
      WriteLn('Hello World!');
      ReadLn;
    end.
    

### Pascal scripts

Кроме того, можно писать сценарии, которые динамически компилируются с помощью [InstantFPC](<../en/InstantFPC.md> "InstantFPC"), кроссплатформенного решения для запуска (небольших) программ Pascal в виде сценариев. Шебанг 
    
    
    #!/usr/bin/env instantfpc

позволяет запускать скрипт как отдельную программу. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Хотя он используется для запуска сценариев Pascal, InstantFPC использует обычный Object Pascal, а не язык сценариев PascalScript. Таким образом, "**PascalScript** " и **Pascal script** \- это не одно и то же.

## Запуск программы

#### Компиляция

Когда вы закончите свою программу (или фрагмент программы), вы можете скомпилировать и запустить ее, выбрав Run -> Build(Выполнить -> Сборка)(или Quick Compile(Быстрая компиляция)) в главном меню или щелкнув значок зеленого треугольника («Выполнить») [![Start.png](https://wiki.freepascal.org/images/b/b8/Start.png)](</File:Start.png>) на панели кнопок. Любые сообщения компилятора (предупреждения, отчеты о ходе выполнения или сообщения об ошибках) появятся в окне сообщения, и, надеюсь, в конечном итоге будет сообщение, которое скажет 

'Project "Project1" successfully built. (Проект "Project1" успешно собран.) :). 

А где программа-то ?? !! 

Если вы еще не сохранили программу, то среда IDE поместит ее во «временный» каталог (например, /tmp в Linux, C:\temp в Windows, см. Environment Options / Files / Directory (Параметры среды / Файлы / Каталог) для собранных тестовых проектов). 

Если вы уже сохранили проект, значит, программа была создана в том же каталоге, где вы сохранили файл project1.lpi. 

#### Запуск в консоли

Вы можете запустить программу, перейдя в окно консоли (терминала), затем с помощью **cd** перейдя в каталог и введя имя программы. В Unix/Linux, если она находится в текущем каталоге, вам придется начать с `./`, поскольку программа, вероятно, не будет найдена в PATH. 

Пример для Linux/Unix, где программа хранится в /tmp: 
    
    
    cd /tmp 
    ./Project1
    

Однако может быть очень неудобно продолжать переходить из редактора Lazarus в окно терминала и обратно. К счастью, есть механизм, позволяющий открывать окно терминала из среды Lazarus. 

#### Запуск в IDE

Во внутренней консоли: показывается в окне вывода терминала. 

При перенаправлении вывода. 

В главном меню выберите Run -> Run Parameters(Выполнить -> Параметры запуска), затем установите флажок "Use launching application"(Использовать запускаемое приложение). В первый раз, когда вы сделаете это и попробуете последовательность Compile / Run (Собрать / Запустить), вы, вероятно, получите отлуп: 

'xterm: Can't execvp /usr/share/lazarus//tools/runwait.sh: Permission denied'. 

Если это произошло, вам необходимо изменить права доступа к соответствующему файлу (например, с помощью chmod +x <имя файла> или с помощью утилиты Windows для изменения разрешений); возможно, вам придется сделать это как root. После этого каждый раз, когда вы будете запускать свою программу, будет появляться окно консоли, в котором будут отображаться все ваши текстовые операции ввода-вывода (readln, Writeln и т. Д.). 

После того, как ваша программа завершит выполнение, на экране появится сообщение «Нажмите Enter». Таким образом, любой вывод, сгенерированный вашей программой, останется на экране до тех пор, пока вы не прочтете его; после того, как вы нажмете «Enter», окно консоли закроется. 

К сожалению, этот метод не позволяет использовать встроенный отладчик. 

#### Запуск в IDE с перенаправлением вывода

Если вы хотите увидеть, что написано в stdout, и также хотите использовать встроенный отладчик, stdout можно перенаправить в файл следующим кодом: 
    
    
    uses
      baseunix; 
    
    var
      OutputFile: text;
    
    begin     
      Assign(OutputFile, 'Output.txt');
      if FileExists(Filename) then begin
        Append(OutputFile);
      end
      else begin
        Rewrite(OutputFile);   { открываем файл для записи, уничтожая содержимое, если таковое имеется }
      end;
    
      ResultCode := fpdup(OutputFile, output);
    
      if ResultCode < 0 then begin
        raise Exception.CreateFmt('Обман не удался: %s', [ResultCode]);
      end;
      Close(OutputFile);
    end.
    

Затем файл output.txt можно просмотреть с помощью команды 'tail -f output.txt' или просмотреть с помощью редактора, если 'tail' недоступен в вашей [ОС](<../en/Operating_System.md> "Operating System"). 

В качестве альтернативы, используя Lazarus 0.9.31 или выше (только в Linux): в меню "View"(Вид) в разделе "Debug Windows"(Отладка Windows) есть запись для "console output"(консольный вывод) для просмотра стандартного вывода. 

## Unicode (UTF8) вывод

Если вы хотите, чтобы ваша программа в режиме консоли отображала вывод Unicode ([UTF8](<../en/UTF-8.md> "UTF-8")) в Windows Vista и более поздних версиях (а также, возможно, в более ранних версиях), вы можете использовать команду **SetConsoleOutputCP** для задания консоли набор символов UTF8. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** вам нужно будет убедиться, что шрифт консоли может отображать буквы, которые вы хотите вывести (например, греческий, кириллица, корейский).

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** вам нужно будет включить модуль Windows.

См. [Поддержка Unicode LCL](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus/ru") для получения более подробной информации о поддержке Unicode в Lazarus и FPC. 

Пример программы: 
    
    
    program uniconsole;
    
    {$mode objfpc}{$H+}
    {$APPTYPE CONSOLE}
    
    uses
      {$IFDEF UNIX}
        {$IFDEF UseCThreads}
        cthreads,
        {$ENDIF}
      {Диспетчер широких строк, необходимый для поддержки широких строк}
      cwstring,
      {$ENDIF}
      {$IFDEF WINDOWS}
      Windows, {для setconsoleoutputcp}
      {$ENDIF}
      Classes
      ;
    
    var
    UTF8TestString: string;
    
    begin
    {$IFDEF WINDOWS}
    SetConsoleOutputCP(CP_UTF8);
    {$ENDIF}
    UTF8TestString:= 'rosé, водка and ούζο';
    writeln ('plain: ' + UTF8TestString);
    {Видимо для этого нам не нужен UTF8ToConsole
    Фактически, UTF8ToConsole для меня ничего не сделал.}
    end.
    

## Примеры

Вы можете использовать редактор Lazarus, чтобы опробовать все примеры из стандартных учебников Паскаля, или можете написать свой собственный. Некоторые из наиболее полезных процедур - это процедуры для выполнения системных команд или для запуска других программ (написанных на Паскале, C или Perl, либо в сценариях оболочки или пакетных сценариях). 

### Выполнение команд оболочки
    
    
    program TryShell;
    uses 
      Classes, Unix;
    var 
      S: LongInt;
    begin
      S := fpsystem('/bin/ls -la *.p*'); //отображает .pp, .pas, .php, .png и т.д. в текущем каталоге
      WriteLn('Program exited with status : ', S)
    end.
    

  


#### Пример: обновить FPC и Lazarus

Могут быть выполнены более сложные команды. Например, если вы уже проверили репозитории SVN для FPC и Lazarus (см. [Buildfaq](</index.php?title=Buildfaq&action=edit&redlink=1> "Buildfaq \(page does not exist\)")), вы можете поддерживать свои исходные файлы FPC и Lazarus в актуальном состоянии, извлекая их из репозитория SVN с помощью следующей последовательности вызовов : 

  


### Пример: корректировка fpc и lazarus

Могут выполняться более сложные команды. Например, после посещения хранилища SVN с новейшими версиями FPC и Lazarus (см. [buildfaq](<../en/buildfaq.md> "buildfaq")) можно иметь исходные версии FPC и Lazarus новейшими, путем взятия из хранилища SVN следующей последовательностью вызовов: 
    
    
    program LazUpdate;
    uses
      Classes, Unix;
    var 
      S: LongInt;
    begin 
      S := fpsystem('cd /usr/local/src/fpc/devel/fpc        ; make clean');
      S := fpsystem('cd /usr/local/src/fpc/devel/lazarus  ; make clean');
      S := fpsystem('cd /usr/local/src/fpc/devel             ; svn update fpc       >& ~/cvscheckout.log');
      S := fpsystem('cd /usr/local/src/fpc/devel             ; svn update lazarus >& ~/cvslaz.log'        );
    end.
    

Обратите внимание, что выдача команды 
    
    
    fpsystem('cd /somedirectory');
    

с последующим 
    
    
    fpsystem ('сделать что-нибудь в этом подкаталоге');
    

не работает, потому что после каждого вызова [ функции](<../en/Function.md> "Function") **fpsystem()** выполнение программы возвращается в каталог, в котором она была запущена; поэтому нам нужно включить несколько операторов в каждую строку в наши вызовы оболочки через **fpsystem()**. [Скорее так: он всегда начинается.] 

Необязательно вводить каждую команду как отдельную строку Паскаля; можно создать файл сценария **bash** следующим образом (из [buildfaq](<../en/buildfaq.md> "buildfaq")): 
    
    
    #!/bin/sh
    cd /usr/local/src/fpc/devel
    cd fpc
    make clean
    cd ..
    cd lazarus
    make clean
    cd ..
    svn up fpc >& ~/cvscheckout.log
    svn up lazarus >& ~/cvslaz.log
    

Назовите файл **updatelaz.sh** , а затем вызовите его из программы на Паскале для его непосредственного выполнения вместо использования **bash** следующим образом: 
    
    
    (* Это может быть скомпилировано или использовано как скрипт Паскаля. *)
    program LazUpdate1;
    uses 
      Classes, Unix;
    var 
      S: LongInt; 
    begin
      S := fpsystem('updatelaz.sh')
    end.
    

## Примечание для пользователей Windows

Традиционно при написании консольных программ в Windows используется кодировка cp866. Если не принять специальных мер, то при написании консольной программы в IDE в консоли вместо русских букв будут выводиться кракозябры. Чтобы получился нормальный русский текст, в окне кода программы нужно нажать правую кнопку мыши и выбрать в контекстном меню "Параметры файла -> Кодировка -> CP866". 

## См.также

  * [Параметры командной строки и переменные среды](<../en/Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables")
  * [Выполнение внешних программ](<Executing_External_Programs.md> "Executing External Programs/ru")
  * [ Отладка консольных приложений](<../en/Debugger_Console_App.md> "Debugger Console App")
  * [InstantFPC](<../en/InstantFPC.md> "InstantFPC")

---

_Source: [https://wiki.freepascal.org/Console_Mode_Pascal/ru](https://web.archive.org/web/20250418104212/https://wiki.freepascal.org/Console_Mode_Pascal/ru)_
