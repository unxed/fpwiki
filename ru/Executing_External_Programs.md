# Executing External Programs

│ **[English (en)](<../en/Executing_External_Programs.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение: сравнение
  * 2 (Process.)RunCommand
  * 3 SysUtils.ExecuteProcess
  * 4 MS Windows : CreateProcess, ShellExecute и WinExec
    * 4.1 Использование ShellExecuteEx с повышенными правами (администратора)
  * 5 Unix fpsystem, fpexecve и shell
  * 6 TProcess
    * 6.1 Простейший пример
    * 6.2 Простой пример
    * 6.3 Усовершенствованный пример (но пока не правильный)
    * 6.4 Чтение больших объемов вывода
    * 6.5 Использование ввода и вывода TProcess
    * 6.6 Некоторые подсказки при использовании TProcess
    * 6.7 Показ комплекта приложений на переднем плане в macOS
    * 6.8 Запуск отдельной программы
    * 6.9 Пример "общения" с процессом aspell
    * 6.10 Замена операторов командной оболочки, таких как "| < >"
      * 6.10.1 Почему использование специальных операторов для перенаправления вывода не работает
    * 6.11 Как перенаправить вывод с помощью TProcess
      * 6.11.1 Заметки
    * 6.12 Перенаправление ввода и вывода и запуск под Root
    * 6.13 Использование fdisk с sudo в Linux
    * 6.14 Параметры, содержащие пробелы (замена кавычек оболочки Shell)
  * 7 Альтернативные решения с помощью LCLIntf
    * 7.1 Открытие документа в приложении по умолчанию
    * 7.2 Открытие web-страницы в web-браузере по умолчанию
  * 8 См. также



## Введение: сравнение

В библиотеках RTL, FCL, LCL есть разные способы выполнить внешнюю программу. 

Метод  | Библиотека  | Платформы  | Одной строкой?  | Особенности   
---|---|---|---|---  
[ExecuteProcess](<Executing_External_Programs.md> "Executing External Programs/ru") | RTL  | кроссплатформенный  | Да  | Очень ограничен, синхронен   
[ShellExecute](<Executing_External_Programs.md> "Executing External Programs/ru") | WinAPI  | Только MS Windows  | Да  | Много. Может запускать программы с правами администратора   
[fpsystem, fpexecve](<Executing_External_Programs.md> "Executing External Programs/ru") | Unix  | Unix only  |  |   
[TProcess](<Executing_External_Programs.md> "Executing External Programs/ru") | FCL  | кроссплатформенный  | Нет  | Все   
[RunCommand](<Executing_External_Programs.md> "Executing External Programs/ru") | FCL  | кроссплатформенный **Требуется FPC 2.6.2+** | Да  | Покрывает стандартное использование TProcess   
[OpenDocument](<Executing_External_Programs.md> "Executing External Programs/ru") | LCL  | кроссплатформенный  | Да  | Только открывает документ. Документ будет открыт стандартной программой   
  
Если вы использовали **ShellExecute** и/или **WinExec** в Delphi, то вы можете начать использовать TProcess как альтернативу в FPC/Lazarus (это верно и в случае использования Lazarus на Linux, потому что TProcess является кроссплатформенным компонентом). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** FPC/Lazarus поддерживает **ShellExecute** и **WinExec** , но только в среде Win32. Если вы пишете кросс-платформенную программу, то лучшим путем будет использование TProcess!

## (Process.)RunCommand

  * [RunCommand](<http://www.freepascal.org/docs-html/fcl/process/runcommand.html>) документация
  * [RunCommandInDir](<http://www.freepascal.org/docs-html/fcl/process/runcommandindir.html>) докуметация



В FPC 2.6.2, некоторые вспомогательные функции для TProcess были добавлены в модуль process основанный на обертке использованной в [fpcup](<../en/Projects_using_Lazarus.md> "Projects using Lazarus"). Эти функции могут быть для базового и среднего уровня использования и могот захватить вывод как одной строки, так и _огромного количества_ выводимой информации. 

Простой пример: 
    
    
    uses Process;
    ...
    var s : ansistring;
    ...
    if RunCommand('/bin/bash',['-c','echo $PATH'],s) then
       writeln(s);
    

Перегруженный вариант функции может возвращать код выхода программы. RunCommandInDir запускает программу в заданной папке. 
    
    
    function RunCommandIndir(const curdir:string;const exename:string;const commands:array of string;var outputstring:string; var exitstatus:integer): integer;
    function RunCommandIndir(const curdir:string;const exename:string;const commands:array of string;var outputstring:string): boolean;
    function RunCommand(const exename:string;const commands:array of string;var outputstring:string): boolean;
    

## SysUtils.ExecuteProcess

Простейший путь, если вам не нужно общение с процессом - это просто использовать вызов 
    
    
      SysUtils.ExecuteProcess('/full/path/to/binary',['arg1','arg2']);
    

При использовании этого метода, приложение "повисает" до тех пор, пока вызванная программа не завершится. Это может быть полезно если пользователь должен сделать что-то перед использованием вашего приложения. Если вы хотите обойти это ограничение, то более полезный кроссплатформенный способ - это **TProcess** , или если ваша целевая платформа только Windows, то используйте **ShellExecute**. 

## MS Windows : CreateProcess, ShellExecute и WinExec

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** FPC/Lazarus поддерживают **CreateProcess** , **ShellExecute** и/или **WinExec** , только на Win32/64. Если ваша программа кроссплатформенная, используйте **RunCommand** или **TProcess**.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** WinExec использует 16-битные вызовы и давно устарел. Новые версии FPC при его использовании генерируют предупреждение.

**ShellExecute** \- стандартная функция MS Windows (ShellApi.h) с хорошей [документацией на MSDN](<http://msdn.microsoft.com/en-us/library/windows/desktop/bb762153\(v=vs.85\).aspx>) (читайте документацию, если вам кажется, что функция ненадежна). 
    
    
    uses ..., ShellApi;
    
    // Обычный текст (возвращаемые ошибки игнорируются) :
    if ShellExecute(0,nil, PChar('"C:\my dir\prog.exe"'),PChar('"C:\somepath\some_doc.ext"'),nil,1) = 0 then;
    
    // Выполняем Batch файл:
    if ShellExecute(0,nil, PChar('cmd'),PChar('/c mybatch.bat'),nil,1) = 0 then;
    
    // Открываем командную строку в заданной папке:
    if ShellExecute(0,nil, PChar('cmd'),PChar('/k cd \path'),nil,1) = 0 then;
    
    // Открываем веб-страницу стандартным браузером с помощью'start' (через скрытую командную строку) :
    if ShellExecute(0,nil, PChar('cmd'),PChar('/c start www.lazarus.freepascal.org/'),nil,0) =0 then;
    
    // или полезную команду:
    procedure RunShellExecute(const prog,params:string);
    begin
      //  ( Handle, nil/'open'/'edit'/'find'/'explore'/'print',   // 'open' не всегда требуется 
      //      path+prog, params, working folder,
      //        0=hide / 1=SW_SHOWNORMAL / 3=max / 7=min)   // for SW_ constants : uses ... Windows ...
      if ShellExecute(0,'open',PChar(prog),PChar(params),PChar(extractfilepath(prog)),1) >32 then; //успех
      // если возвращается число в диапазоне 0..32, то значит ошибка
    end;
    

Также есть WideChar функции - ShellExecuteExW , ShellExecuteExA в режиме AnsiChar. 

В качестве fMask можно ипользовать SEE_MASK_DOENVSUBST, SEE_MASK_FLAG_NO_UI или SEE_MASK_NOCLOSEPROCESS и тд Если в Delphi вы используете ShellExecute для открытия документов, например документ Word или ссылки, то присмотритесь к open* (OpenURL и т.д.) модуле lclintf. 

### Использование ShellExecuteEx с повышенными правами (администратора)

Если вы хотите использовать программу с повышенными (правами администратора) правами, используйте **runas** как альтернативу ShellExecuteEx: 
    
    
    uses ShellApi, ...;
    
    function RunAsAdmin(const Handle: Hwnd; const Path, Params: string): Boolean;
    var
      sei: TShellExecuteInfoA;
    begin
      FillChar(sei, SizeOf(sei), 0);
      sei.cbSize := SizeOf(sei);
      sei.Wnd := Handle;
      sei.fMask := SEE_MASK_FLAG_DDEWAIT or SEE_MASK_FLAG_NO_UI;
      sei.lpVerb := 'runas';
      sei.lpFile := PAnsiChar(Path);
      sei.lpParameters := PAnsiChar(Params);
      sei.nShow := SW_SHOWNORMAL;
      Result := ShellExecuteExA(@sei);
    end;
    
    procedure TFormMain.RunAddOrRemoveApplication;
    begin
      // Пример, открывающий панель управления с повышенными правами
      RunAsAdmin(FormMain.Handle, 'rundll32.exe shell32.dll,Control_RunDLL appwiz.cpl', '');
    end;
    

## Unix fpsystem, fpexecve и shell

Эти функции являются платформозависимыми. 

  * [справка по fpsystem](<http://www.freepascal.org/docs-html/rtl/unix/fpsystem.html>)
  * [справка по fpexecve](<http://www.freepascal.org/docs-html/rtl/baseunix/fpexecve.html>)
  * [справка по shell](<http://www.freepascal.org/docs-html/rtl/unix/shell.html>)



Учтите, что **Unix.Shell** версии 1.0.x устарел и удален из trunk. Используйте fpsystem. 

## TProcess

Вы можете использовать TProcess для запуска внешних программ. Самыми полезными вещами при этом будут: 

  * Платформонезависимость
  * Способность читать из stdout и писать в stdin.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** TProcess не оболочка! И не терминал! Вы не можете напрямую исполнять скрипты или перенаправлять вывод используя такие операторы, как "

Важно: Вы должны определять полный путь к исполняемому файлу. Например '/bin/cp' вместо 'cp'. Если программа находится где-либо в переменной PATH, то вы можете использовать функцию [FindDefaultExecutablePath](<https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/finddefaultexecutablepath.html>) из модуля LCL [FileUtil](<https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/index.html>). 

### Простейший пример

Многие типичные случаи были подготовлены в функциях [Runcommand](<../en/Executing_External_Programs.md> "Executing External Programs"). Прежде, чем начать копировать и вставлять приведенные ниже примеры, сначала проверьте их. 

### Простой пример
    
    
     // Демо-программа, показывающая, как можно запустить
     // внешнюю программу
     program launchprogram;
     
     // Подключаем модули с требуемыми
     // нам процедурами и функциями.
     uses 
       Classes, SysUtils, Process;
     
     // Опишем переменную "AProcess" 
     // типа "TProcess"
     var 
       AProcess: TProcess;
     
     // Здесь наша программа начинается
     begin
       // Создаем объект  TProcess и
       // присваиваем его переменной AProcess.
       AProcess := TProcess.Create(nil);
     
       // Сообщаем новому AProcess, что это за команда для выполнения.
       // Давайте использовать компилятор Free Pascal (версия i386)
       AProcess.Executable:= 'ppc386';
    
       // Передаеv -h вместе с ppc386, чтобы фактически выполнить 'ppc386 -h':
       AProcess.Parameters.Add('-h');
     
       // Мы определим опцию, когда программа 
       // будет запущена. Эта опция гарантирует, что наша программа 
       // не продолжит работу, пока программа, которую мы запустили, 
       // не перестанет работать. vvvvvvvvvvvvvv
       AProcess.Options := AProcess.Options + [poWaitOnExit];
     
       // Теперь пусть Process запустит программу
       AProcess.Execute;
     
       // Пока ppc386 не прекратит работу, мы досюда не дойдем
       AProcess.Free;   
     end.
    

Вот оно! Теперь вы научились запускать внешнюю программу изнутри вашей собственной. 

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): на момент правки статьи (fpc 3.0.4) свойство _CommandLine_ (как и _ApplicationName_) согласно [документации ](<https://www.freepascal.org/docs-html/current/fcl/process/tprocess.html>) объявлены **deprecated**. Вместо них рекомендуется использовать свойства [Parameters](<https://www.freepascal.org/docs-html/current/fcl/process/tprocess.parameters.html>) и [Executable](<https://www.freepascal.org/docs-html/current/fcl/process/tprocess.executable.html>) соответственно. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** для пользователей Windows - если путь к исполняемому файлу включает символы пробела, табуляции или перевода строки, то он должен заключаться в двойные кавычки (например, "c:\Program Files\Git\bin\git.exe"). Это же правило справедливо в случае, если такой путь указывается в аргументах командной строки (в свойстве Parameters).

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** строковый агрумент, добавляемый в Parameters, не должен содержать пробелы.
    
    
    ...
    // к примеру, один из аргументов командной строки должен быть '-user sysdba'
    //тогда он должен быть записан так
    AProcess.Parameters.Add('-user');
    AProcess.Parameters.Add('sysdba');
    ...
    

* * *

### Усовершенствованный пример (но пока не правильный)

Это все замечательно, но как я могу получить вывод программы, которую я запустил? Что ж, давайте немного расширим наш пример и сделаем так: **этот пример прост, поэтому вы можете извлечь из него уроки. Пожалуйста, не используйте этот пример в рабочем коде, а используйте код из раздела** [Чтение больших объемов вывода](<Executing_External_Programs.md> "Executing External Programs/ru")
    
    
    // Это 
    // ДЕФЕКТНАЯ
    // демонстрационная программа, которая показывает
    // как запустить внешнюю программу
    // и читать из ее вывода.
    program launchprogram;
     
    // Здесь мы включаем файлы, которые имеют полезные функции
    // и процедуры, и которые нам понадобятся.
    uses 
      Classes, SysUtils, Process;
     
    // Это определение переменной «AProcess», как переменной
    // типа "TProcess".
    // Также еще мы добавляем TStringList для хранения
    // данных, читаемых из вывода программ.
    var 
      AProcess: TProcess;
      AStringList: TStringList;
    
    // Это место, где наша программа начинает работать
    begin
      // Теперь мы создадим объект Process и 
      // назначим его переменной var AProcess.
      AProcess := TProcess.Create(nil);
     
      // Сообщим новому AProcess, что это за команда для выполнения.
      AProcess.Executable := '/usr/bin/ppc386'; 
      AProcess.Parameters.Add('-h'); 
    
      // Мы определим опцию, когда программа
      // выполняется. Эта опция гарантирует, что наш AProcess
      // не будет продолжаться до тех пор, пока запущенная нами программа
      // остановится. Также теперь мы сообщим AProcess, что
      // мы хотим прочитать вывод файла.
      AProcess.Options := AProcess.Options + [poWaitOnExit, poUsePipes];
     
      // Теперь, когда AProcess знает, что это за командная строка, его можно запустить.
      AProcess.Execute;
      
      // После завершения AProcess, остальная часть программы будет выполнена.
     
      // Теперь читаем вывод программы, который мы только что запихали в TStringList.
      AStringList := TStringList.Create;
      AStringList.LoadFromStream(AProcess.Output);
       
      // Сохраняем выходные данные в файл и очищаем TStringList.
      AStringList.SaveToFile('output.txt');
      AStringList.Free;
     
      // Теперь, когда выходные данные процесса обработаны, его можно освободить.
      AProcess.Free;   
    end.
    

### Чтение больших объемов вывода

В предыдущем примере мы ждали завершения запущенной программы. После этого мы считывали все, что программа записала в выходной поток. Но ведь может оказаться и так, что программа выведет много данных, канал заполнится и вывод остановится, при этом запустившая программа ждет завершения запущенной программы, которая в свою очередь не может завершить работу, пока не выведет все данные. Возникает коллизия, dead-lock. 

Поэтому следующий пример не будет использовать опцию poWaitOnExit и будет читать данные при запущенной программе. Вывод записывается в поток, который можно позже использовать для чтения его содержимого в TStringList. 

Если вы хотите считывать выходные данные из внешнего процесса, вы должны адаптировать этот код для повседневного использования. 
    
    
     program LargeOutputDemo;
    
    {$mode objfpc}{$H+}
     
     uses
       Classes, Process, SysUtils; // Process - модуль, содержащий TProcess
    
    const
      BUF_SIZE = 2048; // Размер буфера для чтения выходных данных блоками
    var
      AProcess     : TProcess;
      OutputStream : TStream;
      BytesRead    : longint;
      Buffer       : array[1..BUF_SIZE] of byte;
    
    begin
      // Установки процесса; в качестве примера используется рекурсивный поиск в каталоге,
      // потому что это обычно приводит к большому количеству данных.
      AProcess := TProcess.Create(nil);
    
      //Команды для Windows и *nix отличаются, поэтому используется директива $IFDEFs
      {$IFDEF Windows}
        // В Windows команду dir нельзя использовать напрямую, потому что она является встроенной
        // командой оболочки. Поэтому нужны cmd.exe и дополнительные параметры
        AProcess.Executable := 'c:\windows\system32\cmd.exe';
        AProcess.Parameters.Add('/c');
        AProcess.Parameters.Add('dir /s c:\windows');
      {$ENDIF Windows}
    
      {$IFDEF Unix}
        AProcess.Executable := '/bin/ls';
        AProcess.Parameters.Add('--recursive');
        AProcess.Parameters.Add('--all');
        AProcess.Parameters.Add('-l');
      {$ENDIF Unix}
    
      // Необходимо использовать poUsePipes - параметр TProcess, чтобы можно было получить выходные данные.
      // Параметр TProcess'а poWaitOnExit не может быть использован, потому что он заблокирует
      // эту программу до окончания вызванного процесса, препятствуя чтению выходных данных процесса.
      AProcess.Options := [poUsePipes];
    
      //Запуск процесса (выполнение команды dir/ls)
      AProcess.Execute;
    
      // Создаем объект потока для хранения сгенерированного вывода. Вывод может
      // также являться файловым потоком для непосредственного сохранения выводимых данных на диск.
      OutputStream := TMemoryStream.Create;
    
      // Все сгенерированные выходные данные из AProcess читаются в цикле, пока больше не останется данных
      repeat
        // Получаем новые данные из процесса до максимального размера выделенного буфера.
        // Обратите внимание, что все вызовы read (...) будут блокироваться, кроме последнего, который возвращает 0 (ноль).
        BytesRead := AProcess.Output.Read(Buffer, BUF_SIZE);
    
        // Добавляем байты, которые были прочитаны в поток для последующего использования
        OutputStream.Write(Buffer, BytesRead)
    
      until BytesRead = 0;  // Останавливаемся, если больше нет данных
    
      // Процесс завершен, поэтому его можно очистить
      AProcess.Free;
    
      //Теперь, когда все данные прочитаны, их можно использовать; например, чтобы сохранить их в файл на диске
      with TFileStream.Create('output.txt', fmCreate) do
      begin
        OutputStream.Position := 0; // Требуется, чтобы убедиться, что все данные копируются с самого начала
        CopyFrom(OutputStream, OutputStream.Size);
        Free
      end;
    
      // Или данные могут быть показаны на экране
      with TStringList.Create do
      begin
        OutputStream.Position := 0; // Требуется, чтобы убедиться, что все данные копируются с самого начала
        LoadFromStream(OutputStream);
        writeln(Text);
        writeln('--- Номер строки = ', Count, '----');
        Free
      end;
    
      // Очищаем
      OutputStream.Free;
    end.
    

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): можно выводить данные сразу в какой-нибудь memoOutput на основе ([решения](<https://forum.lazarus.freepascal.org/index.php/topic,17315.msg95228.html#msg95228>), предложенного пользователем [KpjComp](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=47822>)): 
    
    
    const
      BUF_SIZE = 1024 * 64; // Размер буфера для чтения выходных данных, можно читать блоками по 64k
    var
      Buffer: string = '';
      BytesRead: LongInt = 0;   
    begin
    ...
      //Запуск процесса (выполнение команды dir/ls)
      AProcess.Execute;
      
      //можно дать чуть-чуть времени на отправку данных через AProcess.Output 
      sleep(500);
      
      repeat
        memoOutput.Lines.BeginUpdate;
        try
          SetLength(Buffer,BUF_SIZE);
          BytesRead := AProcess.Output.Read(Buffer[1],Length(Buffer));
          if BytesRead > 0 then
          begin
            SetLength(Buffer,BytesRead);
            memoOutput.Append(Buffer);
          end;
        finally
          memoOutput.Lines.EndUpdate;
          memoOutput.SelStart := UTF8Length(memoOutput.Text);
        end;
        Sleep(50);
        Application.ProcessMessages;
      until BytesRead = 0;
    

* * *

Обратите внимание, что вышеперечисленное также может быть достигнуто с использованием RunCommand: 
    
    
    var s: string;
    ...
    RunCommand('c:\windows\system32\cmd.exe', ['/c', 'dir /s c:\windows'], s);
    

### Использование ввода и вывода TProcess

Смотри демо пример на [Lazarus-CCR SVN](<https://sourceforge.net/p/lazarus-ccr/svn/913/tree/examples/process>). 

### Некоторые подсказки при использовании TProcess

Если вы создаете кроссплатформенную программу, вы можете изменять командную строку применительно к каждой ОС использую директивы "{$IFDEF}" и "{$ENDIF}". 

Например: 
    
    
     {...}
       AProcess:TProcess.Create(nil)
       {$IFDEF WIN32}
       AProcess.CommandLine := 'calc.exe'; // Windows калькулятор
       {$ENDIF}
       {$IFDEF LINUX}
       AProcess.CommandLine := 'kcalc'; // KDE калькулятор
       {$ENDIF}
       AProcess.Execute; // как альтернативу, вы можете использовать AProcess.Active:=True
     {...}
    

### Показ комплекта приложений на переднем плане в macOS

Вы можете запустить **application bundle**(пакет приложения) через TProcess, вызвав исполняемый файл внутри пакета. Например: 
    
    
     AProcess.Executable:='/Applications/iCal.app/Contents/MacOS/iCal';
    

Это запустит _Calendar_ , но окно будет позади текущего приложения. Чтобы получить приложение на переднем плане, вы можете использовать утилиту **open** с параметром **-n** : 
    
    
     AProcess.Executable:='/usr/bin/open';
     AProcess.Parameters.Add('-n');
     AProcess.Parameters.Add('-a'); //необязательно: скрыть терминал - аналогично опции Windows poNoConsole
     AProcess.Parameters.Add('/Application/iCal.app');
    

Если вашему приложению нужны параметры, вы можете передать **open** параметр **\--args** , после чего все параметры будут переданы приложению: 
    
    
     AProcess.Parameters.Add('--args');
     AProcess.Parameters.Add('argument1');
     AProcess.Parameters.Add('argument2');
    

### Запуск отдельной программы

Обычно программа, запускаемая вашим приложением, является дочерним процессом и уничтожается, когда ваше приложение уничтожается. Если вы хотите запустить автономную программу, которая продолжает работать [после завершения вашего приложения], вы можете использовать следующее: 
    
    
    var
      Process: TProcess;
      I: Integer;
    begin
      Process := TProcess.Create(nil);
      try
        Process.InheritHandles := False;
        Process.Options := [];
        Process.ShowWindow := swoShow;
    
        // Копируем переменные среды по умолчанию, включая переменную DISPLAY, для работы приложения с графическим интерфейсом
        for I := 1 to GetEnvironmentVariableCount do
          Process.Environment.Add(GetEnvironmentString(I));
    
        Process.Executable := '/usr/bin/gedit';  
        Process.Execute;
      finally
        Process.Free;
      end;
    end;
    

### Пример "общения" с процессом aspell

Внутри исходного кода [pasdoc](<https://github.com/pasdoc/pasdoc/wiki>) вы можете найти два модуля, которые выполняют проверку орфографии, "общаясь" с запущенным процессом aspell через каналы: 

  * Модуль [PasDoc_ProcessLineTalk.pas](<https://github.com/pasdoc/pasdoc/blob/master/source/component/PasDoc_ProcessLineTalk.pas>) реализует класс TProcessLineTalk, потомок TProcess, который может быть легко использован для общения с любым процессом построчно.


  * Модуль [PasDoc_Aspell.pas](<https://github.com/pasdoc/pasdoc/blob/master/source/component/PasDoc_Aspell.pas>) реализует класс TAspellProcess, который выполняет проверку орфографии с использованием базового экземпляра TProcessLineTalk для выполнения aspell и связи с запущенным процессом aspell.



Оба модуля довольно независимы от остальных исходников pasdoc, поэтому они могут служить реальными примерами использования TProcess для запуска и связи по каналам с другой программой. 

### Замена операторов командной оболочки, таких как "| < >"

Иногда у вас есть желание выполнить более сложную команду, которая передает свои данные другой команде или файлу. Что-то вроде 
    
    
    ShellExecute('firstcommand.exe | secondcommand.exe');
    

или 
    
    
    ShellExecute('dir > output.txt');
    

Запуск таких команд с TProcess не будет работать. То есть: 
    
    
    // так работать не будет
    Process.CommandLine := 'firstcommand.exe | secondcommand.exe'; 
    Process.Execute;
    

#### Почему использование специальных операторов для перенаправления вывода не работает

TProcess - это не оболочка, а процесс. Это не два процесса, а всего один. Можно перенаправить вывод так, как вы этого пожелаете. См. [следующий раздел](<Executing_External_Programs.md> "Executing External Programs/ru"). 

### Как перенаправить вывод с помощью TProcess

Вы можете перенаправлять вывод команды другой команде, используя экземпляр TProcess для **каждой** команды. 

Вот пример, который объясняет, как перенаправить вывод одного процесса другому. Чтобы перенаправить вывод процесса в файл/поток, см. пример [чтение больших объемов вывода](<Executing_External_Programs.md> "Executing External Programs/ru"). 

Вы можете не только перенаправлять «обычный» вывод (также известный как stdout), но также можете перенаправить вывод ошибок (stderr), если вы укажете опцию poStderrToOutPut, как видно в настройках для второго процесса. 
    
    
    program Project1;
      
    uses
      Classes, sysutils, process;
      
    var
      FirstProcess,
      SecondProcess: TProcess;
      Buffer: array[0..127] of char;
      ReadCount: Integer;
      ReadSize: Integer;
    begin
      FirstProcess  := TProcess.Create(nil);
      SecondProcess := TProcess.Create(nil);
     
      FirstProcess.Options     := [poUsePipes]; 
      FirstProcess.Executable  := 'pwd'; 
      
      SecondProcess.Options    := [poUsePipes,poStderrToOutPut];
      SecondProcess.Executable := 'grep'; 
      SecondProcess.Parameters.Add(DirectorySeparator+ ' -'); 
      // это было бы аналогично "pwd | grep / -"
      
      FirstProcess.Execute;
      SecondProcess.Execute;
      
      while FirstProcess.Running or (FirstProcess.Output.NumBytesAvailable > 0) do
      begin
        if FirstProcess.Output.NumBytesAvailable > 0 then
        begin
          // убеждаемся, что мы не читаем больше данных, чем мы выделили
          // для этого места в буфере
          ReadSize := FirstProcess.Output.NumBytesAvailable;
          if ReadSize > SizeOf(Buffer) then
            ReadSize := SizeOf(Buffer);
          // теперь считываем вывод в буфер
          ReadCount := FirstProcess.Output.Read(Buffer[0], ReadSize);
          // и пишем буфер во второй процесс
          SecondProcess.Input.Write(Buffer[0], ReadCount);
      
          // если SecondProcess записывает много данных в свой Output, то 
          // мы должны прочитать эти данные здесь, чтобы предотвратить блокировку
          // см. предыдущий пример «Чтение больших объемов вывода»
        end;
      end;
      // Закрываем вход на SecondProcess,
      // поэтому он заканчивает обработку своих данных
      SecondProcess.CloseInput;
     
      // и дожидаемся его завершения 
      //  будьте осторожны с тем, какую команду вы запускаете, потому что она может не завершиться, 
      // когда ее вход закроется, следующая строка может зациклиться навечно
      while SecondProcess.Running do
        Sleep(1);
      // вот и все! остальная часть программы просто в качестве примера
      // это немного «полезно»
    
      // мы будем повторно использовать буфер для вывода SecondProcess'а
      // вывод в *эту* программу stdout
      WriteLn('Grep output Start:');
      ReadSize := SecondProcess.Output.NumBytesAvailable;
      if ReadSize > SizeOf(Buffer) then
        ReadSize := SizeOf(Buffer);
      if ReadSize > 0 then
      begin
        ReadCount := SecondProcess.Output.Read(Buffer, ReadSize);
        WriteLn(Copy(Buffer,0, ReadCount));
      end
      else
        WriteLn('grep did not find what we searched for. ', SecondProcess.ExitStatus);
      WriteLn('Grep output Finish:');
      
      // освобождаем объекты нашего процесса
      FirstProcess.Free;
      SecondProcess.Free;
    end.
    

Вот и все. Теперь вы можете перенаправлять вывод из одной программы в другую. 

#### Заметки

Этот пример может показаться излишним, поскольку можно запускать «сложные» команды, используя оболочку с TProcess, например: 
    
    
    Process.Commandline := 'sh -c "pwd | grep / -"';
    

Но наш пример более кроссплатформенный, так как он не нуждается в модификации для работы в Windows или Linux и т.д. "sh" может существовать или не существовать на вашей платформе и обычно доступен только на платформах *nix. Кроме того, у нас больше гибкости в нашем примере, так как вы можете читать и записывать из/в ввода, вывода и stderr каждого процесса в отдельности, что может быть очень выгодно для вашего проекта. 

### Перенаправление ввода и вывода и запуск под Root

Распространенная проблема в Unix(macOS) и Linux заключается в том, что вы хотите выполнить какую-либо программу под учетной записью root (или, в более общем случае, под другой учетной записью пользователя). В качестве примера можно привести команду _ping_. 

Если вы можете использовать sudo для этого, вы можете приспособить следующий пример, адаптированный из примера, опубликованного andyman на форуме ([[1]](<http://lazarus.freepascal.org/index.php/topic,14479.0.html>)). Этот пример запускает `ls` в каталоге `/root`, но, конечно, может быть адаптирован. 

**Лучший способ** сделать это - использовать пакет policykit, который должен быть доступен во всех последних версиях Linux. Подробности смотрите в [ветке форума.](<http://lazarus.freepascal.org/index.php/topic,14479.0.html>)

Большие части этого кода похожи на предыдущий пример, но он также показывает, как перенаправить stdout и stderr процесса, вызываемого отдельно, в stdout и stderr нашего собственного кода. 
    
    
    program rootls;
    
    {Демонстрирует использование TProcess, перенаправляющего stdout/stderr в наш stdout/stderr,
    вызов sudo в Linux/OS и обеспечивающего ввод в stdin}
    
    {$mode objfpc}{$H+}
    
    uses
      Classes,
      Math, {for min}
      Process;
    
      procedure RunsLsRoot;
      var
        Proc: TProcess;
        CharBuffer: array [0..511] of char;
        ReadCount: integer;
        ExitCode: integer;
        SudoPassword: string;
      begin
        WriteLn('Пожалуйста, введите пароль sudo:');
        Readln(SudoPassword);
        ExitCode := -1; //Начнем с неудачного вывода, посмотрим позже, работает ли это
        Proc := TProcess.Create(nil); //Создаем новый процесс
        try
          Proc.Options := [poUsePipes, poStderrToOutPut]; //Используем pipes для перенаправления программных stdin, stdout, stderr
          Proc.CommandLine := 'sudo -S ls /root'; //Запускаем ls /root как root с помощью sudo
          // -S заставляет sudo читать пароль из stdin
          Proc.Execute; //Запускаем его. sudo теперь, вероятно, попросит пароль
    
          // пишем пароль в stdin sudo программы:
          SudoPassword := SudoPassword + LineEnding;
          Proc.Input.Write(SudoPassword[1], Length(SudoPassword));
          SudoPassword := '%*'; // короткая строка, надеюсь, это немного зашифрует память; примечание: использование PChars более надежно
          SudoPassword := ''; // и сделает программу немного более безопасной от слежки?!?
    
          // основной цикл для чтения вывода из stdout и stderr sudo
          while Proc.Running or (Proc.Output.NumBytesAvailable > 0) or
            (Proc.Stderr.NumBytesAvailable > 0) do
          begin
            // читаем stdout и пишем в наш stdout
            while Proc.Output.NumBytesAvailable > 0 do
            begin
              ReadCount := Min(512, Proc.Output.NumBytesAvailable); //Читаем до буфера, не более
              Proc.Output.Read(CharBuffer, ReadCount);
              Write(StdOut, Copy(CharBuffer, 0, ReadCount));
            end;
            // читаем stderr и пишем в наш stderr
            while Proc.Stderr.NumBytesAvailable > 0 do
            begin
              ReadCount := Min(512, Proc.Stderr.NumBytesAvailable); //Читаем до буфера, не более
              Proc.Stderr.Read(CharBuffer, ReadCount);
              Write(StdErr, Copy(CharBuffer, 0, ReadCount));
            end;
          end;
          ExitCode := Proc.ExitStatus;
        finally
          Proc.Free;
          Halt(ExitCode);
        end;
      end;
    
    begin
      RunsLsRoot;
    end.
    

Другие мысли: Без сомнения, было бы целесообразно проверить, запрашивает ли sudo пароль. Это можно последовательно проверить, установив переменную окружения SUDO_PROMPT в ту, которую мы наблюдаем при чтении стандартного вывода TProcess, чтобы избежать проблемы, связанной с тем, что приглашение будет различным для разных локалей. Установка переменной среды приводит к тому, что значения по умолчанию очищаются (наследуются от нашего процесса), поэтому мы должны при необходимости скопировать окружение из нашей программы. 

### Использование fdisk с sudo в Linux

В следующем примере показано, как запустить fdisk на компьютере с Linux с помощью команды sudo для получения прав root. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** это только пример, и он не рассчитан на большой результат
    
    
    program getpartitioninfo;
    
    {Первоначально пример предоставлен пользователем wjackson153 с форума Lazarus. Пожалуйста, свяжитесь с ним в случае возникновения вопросов, замечаний и т.д. Пример преобразован для простоты понимания/краткости из фрагмента кода Lazarus в программу FPC пользователем BigChimp}
    
    Uses
      Classes, SysUtils, FileUtil, Process;
    
    var
      hprocess: TProcess;
      sPass: String;
      OutputLines: TStringList;
    
    begin  
      sPass := 'yoursudopasswordhere'; // Вы должны изменить это на свой собственный пароль sudo
      OutputLines:=TStringList.Create; //... было бы неплохо убедиться с помощью блока try..finally, 
      // что OutputLines освобожден ... То же самое для hProcess.
         
      // Следующий пример откроет fdisk в фоновом режиме и даст нам информацию о разделе
      // Поскольку fdisk требует повышенных привилегий, нам нужно 
      // передать наш пароль в качестве параметра sudo с помощью параметра -S,
      // поэтому он будет ждать, пока наша программа отправит наш пароль в sudo приложение
      hProcess := TProcess.Create(nil);
      // В Linux/Unix/macOS нам нужно указать полный путь к нашему исполняемому файлу:
      hProcess.Executable := '/bin/sh';
      // Теперь мы добавляем все параметры в командную строку:
      hprocess.Parameters.Add('-c');
      // Здесь мы передаем пароль в команду sudo, которая затем выполняет fdisk -l: 
      hprocess.Parameters.add('echo ' + sPass  + ' | sudo -S fdisk -l');
      // Запускаем асинхронно (дожидаемся завершения процесса) и используем pipes, чтобы мы могли прочитать output pipe
      hProcess.Options := hProcess.Options + [poWaitOnExit, poUsePipes];
      // Теперь запускаем:
      hProcess.Execute;
    
      // hProcess должен запустить внешний исполняемый файл (потому что мы используем poWaitOnExit).
      // Теперь вы можете обработать output процесса (стандартный output и стандартный error), например:
      OutputLines.Add('stdout:');
      OutputLines.LoadFromStream(hprocess.Output);
      OutputLines.Add('stderr:');
      OutputLines.LoadFromStream(hProcess.Stderr);
      // Показываем output на экране:
      writeln(OutputLines.Text);
    
      // Очищаем, чтобы избежать утечек памяти:
      hProcess.Free;
      OutputLines.Free;
      
      //Ниже приведены некоторые примеры, как вы видите, мы можем передавать недопустимые символы, как если бы это было сделано из терминала
      //Даже, если вы где-то читали, что это невозможно, я уверяю вас, что при помощи этого метода вы сможете :)
    
      //hprocess.Parameters.Add('ping -c 1 www.google.com');
      //hprocess.Parameters.Add('ifconfig wlan0 | grep ' +  QuotedStr('inet addr:') + ' | cut -d: -f2');
    
      //Использование QuotedStr() не является обязательным требованием, хотя делает его более читабельным кодом;
      //Вы можете использовать двойные кавычки и добиться того же эффекта
    
      //hprocess.Parameters.Add('glxinfo | grep direct');   
    
      // Этот метод также можно использовать для установки приложений из вашего хранилища:
    
      //hprocess.Parameters.add('echo ' + sPass  + ' | sudo -S apt-get install -y pkg-name'); 
    
     end.
    

### Параметры, содержащие пробелы (замена кавычек оболочки Shell)

В оболочке Linux можно записать в кавычки следующие аргументы: 
    
    
    gdb --batch --eval-command="info symbol 0x0000DDDD" myprogram
    

И GDB получит 3 аргумента (в дополнение к первому аргументу, который является полным путем к исполняемому файлу): 

  1. \--batch
  2. \--eval-command=info symbol 0x0000DDDD
  3. полный путь к myprogram



TProcess также может передавать параметры, содержащие пробелы, но использует другой стиль квотирования. Вместо того, чтобы заключать в кавычки только часть параметра, заключите в кавычки все из них. Вот так: 
    
    
    TProcess.CommandLine := '/usr/bin/gdb --batch "--eval-command=info symbol 0x0000DDDD" /home/me/myprogram';
    

И также не забудьте передать только полный путь. 

Смотрите также эту дискуссию об этом: [здесь](<http://bugs.freepascal.org/view.php?id=14446>)

## Альтернативные решения с помощью LCLIntf

Иногда вам не нужно явно вызывать внешнюю программу, чтобы получить необходимую вам функциональность. Вместо того, чтобы открывать приложение и указывать документ для него, просто попросите ОС открыть документ, позволив ей использовать приложение по умолчанию, связанное с этим типом файла. Ниже приведены некоторые примеры. 

### Открытие документа в приложении по умолчанию

В некоторых ситуациях вам нужно открыть какой-либо документ/файл с использованием приложения по умолчанию, а не выполнить определенную программу. Это зависит от запущенной операционной системы. Lazarus предоставляет платформ-независимую процедуру **OpenDocument** , которая будет делать это за вас. Ваше приложение продолжит работу, не дожидаясь закрытия процесса документа. 
    
    
    uses LCLIntf;
    ...
    OpenDocument('manual.pdf');  
    ...
    

  * [справка по OpenDocument](<../en/opendocument.md> "opendocument")



### Открытие web-страницы в web-браузере по умолчанию

Просто передайте требуемый URL, _http://_ в начале строки является необязательным при определенных условиях. Кроме того, передача имени файла дает те же результаты, что и OpenDocument() 
    
    
    uses LCLIntf;
    ...
    OpenURL('www.lazarus.freepascal.org/');
    

См. также: 

  * [справка по OpenURL](<../en/OpenURL.md> "OpenURL")



Кроме того, вы можете использовать **TProcess** как в данном примере: 
    
    
    uses Process;
    
    procedure OpenWebPage(URL: string);
    // Очевидно, вам нужно передать свой URL внутри кавычек "", например, "www.lazarus.freepascal.org"
    var
      Browser, Params: string;
    begin
      FindDefaultBrowser(Browser, Params);
      with TProcess.Create(nil) do
      try
        Executable := Browser;
        Params:=Format(Params, [URL]);
        Params:=copy(Params,2,length(Params)-2); // удаляем "", новая версия TProcess.Parameters делает это сама
        Parameters.Add(Params);
        Options := [poNoConsole];
        Execute;
      finally
        Free;
      end;
    end;
    

## См. также

  * [Документация по TProcess](<http://lazarus-ccr.sourceforge.net/docs/fcl/process/tprocess.html>)
  * [OpenDocument](<../en/opendocument.md> "opendocument")
  * [OpenURL](<../en/OpenURL.md> "OpenURL")
  * [TProcessUTF8](<TProcessUTF8.md> "TProcessUTF8/ru")
  * [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage/ru")
  * [Webbrowser](<Webbrowser.md> "Webbrowser/ru")
  * [Запуск чужой программы из своей](<http://freepascal.ru/article/freepascal/20191121080000/>) \- статья на русскоязычном freepascal-ресурсе

---

_Source: [https://wiki.freepascal.org/Executing_External_Programs/ru](https://web.archive.org/web/20250418094730/https://wiki.freepascal.org/Executing_External_Programs/ru)_
