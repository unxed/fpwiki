# CGI Web Programming

│ **[English (en)](<../en/CGI_Web_Programming.md>)** │  **русский (ru)** │

Эта статья содержит основные сведения об CGI программировании. Обратите внимание, что использование специализированных сред разработок, будет гораздо продуктивным. 

## Contents

  * 1 Общие сведения
  * 2 Об использовании FreePascal для CGI
  * 3 Простой пример
  * 4 Простой счетчик посещений
  * 5 Отладка CGI программ
  * 6 Существующие средства разработки для CGI приложений



## Общие сведения

[Общий интерфейс шлюза - **CGI** (Common Gateway Interface)](<http://ru.wikipedia.org/wiki/Common_Gateway_Interface>) в общих чертах работает следующим образом: 

У нас есть два компьютера: 

  * Компьютер-сервер, на этом компьютере будут храниться файлы (веб-страницы, изображения и Cgi-скрипты);
  * Компьютер-клиент, этот компьютер имеет браузер для запроса файлов, хранящихся на сервере.



Вы можете использовать Cgi-скрипты только, если ваш сервер обладает поддержкой CGI. После размещения ваших файлов CGI, вы можете получить доступ к ним по ссылке вида: **http://www.ваш_сервер.зона[:порт]/ваш_логин/cgi-bin/имя_cgi_программы.exe**

**Примечания:**

  * Файл с CGI программой может не иметь расширение **.exe**. Чаще всего расширение у таких программ .cgi это настраивается на веб-сервере.
  * CGI программа должна быть откомпилирована для платформы сервера (Linux, Windows...)



## Об использовании FreePascal для CGI

CGI программа, написанная на Free Pascal, является обычной программой, как и любая другая. В отличии от CGI программы, написанной на Perl, Free Pascal CGI не интерпретируется. 

Для тестирования CGI, необходимо установить на ваш компьютер "'HTTP Сервер"' с поддержкой CGI, например [Apache](<http://ru.wikipedia.org/wiki/Apache_HTTP_Server>). 

## Простой пример

Ниже приведён небольшой пример простой CGI программы, которая осуществляет: 

  1. Установку cookie
  2. Вывод content-type (т.е. отмечает текст для передачи по HTTP)
  3. Чтение файлов Cookie
  4. Чтение данных формы переданных методом GET
  5. Чтение данных формы переданных методом POST



  

    
    
    program mini;
    
    uses dos;
    
    var
      a:string;
      c:char;
    begin
    // устанавливаем cookie (должно быть выведено до передачи content-type)
    // не забудьте изменить срок окончания
    
    writeln('Set-cookie:widget=value; path=/; expires= Mon, 21-Mar-2005  18:37:00 GMT');
    
      // указываем тип содержимого, как html
      writeln('Content-Type:text/html',#10#13);
    
      // получаем cookies
      a:= GetEnv('HTTP_COOKIE');
      writeln('cookies:',a);
      
      // данные, переданные методом GET
      a:='';
      a:= GetEnv('QUERY_STRING');
      writeln('GET: ',a);
    	
      // данные, переданные методом POST
      a:='';
      while not eof(input) do
      begin
         read(c);
         a:= a+c;
      end;	 
      writeln('POST: ',a);
    end.
    

## Простой счетчик посещений

Приложение "'hitcounter"' представляет собой простой счетчик посещений для вашего веб-сайта. 

Исходный код находится здесь: [[1]](<http://modelbuilder.svn.sourceforge.net/viewvc/modelbuilder/html/cgi/>)

  


## Отладка CGI программ

Так как для запуска CGI приложения используется другой процесс, а сама программа ни чего не выводит на экран, её отладка более трудоёмка, чем обычных приложений. 

Одно из решений состоит в запуске из CGI-приложения отладчика:  
При использовании в программе, ниже приведенного кода, сначала, как часть инициализации, запустится терминал (windows:cmd, linux:xterm), а затем отладчик (gdb) для вызывающего процесса. 

Функция "sleep" выполняет прерывание программы, для инициализации gdb. 

Данный пример был протестирован на: 

  * Windows XP32 с запущеным apache 2.2 (не работает для более свежих версий Windows, см. <http://forum.lazarus.freepascal.org/index.php/topic,21931.msg128849.html#msg128849>)
  * Ubuntu 10.04 x64 c Apache 2.2
  * Debian unstable x64 c Apache 2 (Feb 2013)



Пример модуля, использующий отладку. 
    
    
    unit SelfDebug;
    
    {
    Инструкция:
    -Подключить модуль в любом месте программы.
    -Измените константу DEBUGGER на имя используемого вами отладчика (gdb, ddd,gdbtui...)
    -Для Windows, если программа для отладки запущена в качестве службы (т.е. CGI приложение запущено
    из Apache выполняемого как служба) убедитесь, что служба настроена как
    «взаимодействовать с рабочим столом». Если так не сделать, будет запущен отладчик
    без консоли. Он прервёт программу и зависнет, ожидая ввода. 
    - Для Linux: если программа для отладки, запущена другим пользователем (напр. CGI-приложением),
    запустите "xhost +" в терминале, чтобы все пользователи могли подключиться к xserver.
     Если необходимо, измените DISPLAY, чтобы соответствовать вашей переменной окружения DISPLAY
    }
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils,Process;
    
    implementation
    
    const
      {$ifdef windows}
      DEBUGGER='C:\lazarus\mingw\bin\7.3\gdb.exe';
      {$endif}
      {$ifdef linux}
      DEBUGGER='gdb';
      DISPLAY=':0.0';
      {$endif}
      MSWAIT=2000;
    
    var
      AProcess: TProcess;
    
    initialization
    AProcess := TProcess.Create(nil);
    {$ifdef windows}
    AProcess.CommandLine := format('cmd /C START "Debugging %s" /WAIT "%s" "%s" %d"',[paramstr(0),debugger,paramstr(0),GetProcessID]);
    {$endif}
    {$ifdef linux}
    AProcess.CommandLine := format('xterm -display %s -T "Debugging %s" -e "%s" "%s" %d',[DISPLAY,paramstr(0),DEBUGGER,paramstr(0),GetProcessID]);
    {$endif}
    AProcess.Execute;
    sleep(MSWAIT);
    finalization
    AProcess.Free;
    end.
    

Исходник: <http://www.hu.freepascal.org/lists/fpc-pascal/2011-October/030752.html>

Так же отладку можно реализовать через ведение журнала программы. 

## Существующие средства разработки для CGI приложений

  * [Brook Framework](<../en/Brook_Framework.md> "Brook Framework")
  * [fcl-web](<../en/fcl-web.md> "fcl-web")
  * [Powtils](<http://z505.com/powtils/idx.shtml>)
  * [ExtPascal](<http://extpascal.googlecode.com>).

---

_Source: [https://wiki.freepascal.org/CGI_Web_Programming/ru](https://web.archive.org/web/20250418105350/https://wiki.freepascal.org/CGI_Web_Programming/ru)_
