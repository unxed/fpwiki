# lNet

│ **[English (en)](<../en/lNet.md> "lNet")** │  **русский (ru)** │ 

## Contents

  * 1 О компоненте
  * 2 Загрузка
  * 3 Использование
    * 3.1 События TLNetComponent
  * 4 Документация
  * 5 Установка визуального пакета в Lazarus
  * 6 Содержимое пакета
  * 7 Тестирование
  * 8 История изменений
  * 9 Надежность кода
  * 10 Дорожная карта
  * 11 Требования
    * 11.1 Специальное уведомление под Ubuntu
  * 12 Изменения в API
  * 13 lNet для FreeBSD, Linux, macOS и Windows
  * 14 См.также



## О компоненте

**lNet** или **Lightweight Networking Library** представляет собой набор классов и компонентов для включения управляемых событиями сетей TCP или UDP. 

Авторы: 

  * Ales Katona ([User: Almindor](</User:Almindor> "User:Almindor"))
  * Micha Nelissen



Лицензия: modified LGPL license (разрешает статическое связывание). 

## Загрузка

Существует два варианта библиотеки, которые больше полностью не синхронизируются: 

  * [lNet package](<http://sourceforge.net/projects/lazarus-ccr/files/lNet/>). Упакованы в виде утилит для использования самого проекта FPC. Они не рекомендуются для общего использования.
  * <https://lnet.wordpress.com/> (перемещен на [GitHub](<https://github.com/almindor/lnet>)).



Вы можете связаться с автором напрямую по электронной почте или перейти на канал #lnet на freenode. 

## Использование

Бросьте TLTCPComponent или TLUDPComponent на форму. Назначьте обработчики событий OnAccept, OnError, OnReceive, OnConnect и OnDisconnect. Обратите внимание, что это необязательно, но в большинстве случаев требуется для реализации функционала. 

Клиентское соединение инициализируется вызовом Connect. Аргументами являются IP-адрес/имя хоста и порт соответственно. Используется неблокирующее соединение, это означает, что вы «НЕ» знаете, что вы подключены, пока случится событие OnConnect. 

Соединение с сервером инициализируется вызовом Listen. Аргумент — порт. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** адрес — это строковое представление IP-адреса, например: '127.0.0.1' или имя хоста, например 'localhost'.

Отправка данных другой стороне осуществляется с помощью метода Send/SendMessage. 

### События TLNetComponent

  * `OnError` \- это событие вызывается всякий раз, когда возникает ошибка соединения. Если обработчик не назначен, возникает исключение с "msg". Аргументы: `msg` — содержит строковое и числовое представление сообщения об ошибке, отформатированное в собственной кодировке или UTF8 (в зависимости от свойства `ForceUTF8`). `aSocket` — это сокет, в котором произошла ошибка, ноль означает, что ошибка не связана с конкретным сокетом.


  * `OnConnect` \- это событие генерируется при успешном подключении клиента. `aSocket` — это сокет, к которому подключились. Он должен служить «отправной точкой» для клиентов.


  * `OnAccept` \- это событие вызывается всякий раз, когда на сервере принимается соединение. Если обработчик не назначен, ничего не происходит. `aSocket` содержит сокет, который принял соединение.


  * `OnDisconnect` \- это событие вызывается всякий раз, когда соединение разорвано. Если обработчик не назначен, ничего не происходит. `aSocket` содержит сокет, соединение с которым было разорвано.


  * `OnReceive` \- это событие вызывается всякий раз, когда новые данные «готовы к приему». Если обработчик не назначен, ничего не происходит, но может произойти заполнение системного буфера, что, в свою очередь, вызовет событие `OnError`. `aSocket` — сокет, через который были получены данные. Данные считываются методом `Get`/`GetMessage`.


  * `OnCanSend` \- это событие вызывается всякий раз, когда новые данные могут быть снова отправлены в сокет после сбоя вызова функции отправки и возврата 0. Его можно использовать для автоматизации отправки больших фрагментов. `aSocket` — это сокет, через который можно вызвать отправку.



## Документация

Документация по lNet частично завершена. Вы также можете использовать диаграммы классов Jesus и модулей в качестве основного справочника. 

Документация по протоколам eventer и TCP/UDP.: <http://members.chello.sk/ales/docs> ([Archived version at archive.org](<https://web.archive.org/web/20230000000000*/http://members.chello.sk/ales/docs>)}  


Обзорные схемы можно найти здесь.: <http://members.chello.sk/ales/docs/diagrams/overview.html>  
Pictures:   
[New class diagram](<http://members.chello.sk/ales/docs/diagrams/inheritance_composition.png>)  
[All classes](<http://members.chello.sk/ales/docs/diagrams/extra/allclasses.png>)  
[lNet units](<http://members.chello.sk/ales/docs/diagrams/extra/lnetunits.png>)  
[lNet units 2](<http://members.chello.sk/ales/docs/diagrams/extra/lnetunits2.png>)  


Зеркало документов и текущие заметки: <http://z505.com/cgi-bin/powtils/docs/1.6/idx.cgi?file=lnetlufdoc>

* * *

[Прим.переводчика](</User:Zoltanleo> "User:Zoltanleo"): большинство ссылок старые, потому - битые. На гитхабе есть [демки](<https://github.com/almindor/lnet/tree/master/examples>) и [документация](<https://github.com/almindor/lnet/blob/master/doc/en/doc_lnet.xml>) в виде xml-файла 

* * *

## Установка визуального пакета в Lazarus

  * запускаем Lazarus, перейдите в раздел «Компоненты/откройте файл пакета» (*.lpk).
  * открываем lnetvisual.lpk и устанавливаем
  * перезапускаем Лазарус



Если вы хотите использовать LNet в своем проекте, просто скопируйте куда-нибудь каталог _lib_ и поместите его в путь поиска модулей компилятора с параметрами -Fu и -Fi для подкаталога _lib/sys_. 

## Содержимое пакета

Пакет состоит из базовой библиотеки модулей lNet, lTelnet для протокола telnet, lFTP для протокола ftp и библиотек lNetComponents для предоставления визуальных и невизуальных компонентов для работы в сети. Начиная с версии 0.4.0 были добавлены компоненты lHTTP и lSMTP. Начиная с версии 0.5.1 в библиотеке есть поддержка MIME, в основном для создания составных сообщений для SMTP. Начиная с версии 0.6.0 lNet поддерживает SSL по модульному принципу, а протокол SMTP был расширен до полной поддержки SSL/TLS (включая согласование STARTTLS во время выполнения). Клиентская часть HTTPS также поддерживается. 

## Тестирование

Невизуальные консольные компоненты были протестированы на Win32, Win64, Linux_x86_32, Linux_x86_64, Linux_PPC, Linux_PPC_64, Linux_ARM* (2.1.4+) и FreeBSD_x86_32. Визуальные компоненты (пакеты Lazarus) были протестированы в Win32, Win64, ARM/WinCE, Linux_x86_32, Linux_x86_64 и FreeBSD_x86_32. (gtk1 и gtk2 на unix платформах) 

## История изменений

  * March 2011: A new version of lNet was just released. 0.6.5 completes SSL and IPv6 support including server-side and per-socket (for SSL) setting. SSL and IPv6 bugs were fixed as well as some remaining disconnect problems related to server bind errors and lingering sockets. DecodeURL bug introduced in 0.6.4 was also fixed.


  * May 2010: A new version of lNet was just released. 0.6.4 adds Qt4 support inside Lazarus (lazarus latest trunk needed, 0.9.30 should be 1st stable with it). There were also numerous fixes (http URL parsing, FTP problems) and one major bug fixed (WinCE compilation with FPC 2.4.0+). Grab it at SVN, packages will be uploaded ASAP.


  * February 2010: Well there’s a skip in dates. Anyways version 0.6.3 of lNet has been released, you can find it in the downloads section. There’s been various bugfixes and file renames to avoid FPC conflicts. I didn’t find the time to do the QT4 port as I wanted but it’s in the works now. Some SSL fixes were made as well.


  * May 2008: Where to begin? This is a HUGE critical bugfix release. There was about 5 big bugs fixed in SSL section, one logical oversight in Session support. OpenSSL unit was added so WinCE compiles again and SMTP and FTP got bugs fixed in regards to possible desynchronization. I'd urge everyone who uses 0.6.x to update ASAP, but some bugs are also from 0.5.x (specifically the SMTP and FTP event deadlock problem).


  * May 2008: It seems that due to an oversight in what's packaged with FPC and for which platform, lNet 0.6.x doesn't currently compile on WinCE due to missing OpenSSL unit. The unit is in fpc 2.2.0, but is not compiled for that platform yet (it is in 2.2.1+). I will add the unit to lNet and release 0.6.2 shortly, but until then, WinCE users can work around this by copying [this file](<http://svn.freepascal.org/svn/fpc/trunk/packages/openssl/src/openssl.pas>) into their lnetdir/lib directory. (same will be done in 0.6.2 so don't worry)


  * April 2008: 0.6.1 has been released today. This release fixes an ugly SSL logical bug in regards to the mandatory requirements of Certificate Authority List file and Keyfile proprties of SSLSession. They are no longer needed for normal SSL to work. A minor change in HTTP and handling of SSLSession are also present.



## Надежность кода

  * Base lNet TCP/UDP - stable
  * Telnet Client - stable*
  * FTP Client - stable*
  * SMTP Client - stable
  * HTTP Client - stable
  * HTTP Server - experimental
  * MIME streams - stable
  * SSL support (client-side) - stable
  * SSL support (server-side) - stable (0.6.5+)
  * IPv6 support - stable (0.6.5+)



* FTP и Telnet по-прежнему нуждаются в некоторых дополнительных функциях, таких как методы аутентификации. 

В Linux, когда вы начинаете использовать LNet, GDB может случайно (при компиляции) аварийно завершить работу с различными типами сообщений. Установка для отладчика значения «Автоматически» в параметрах пакетов lnetbase и lnetvisual устраняет проблему. 

## Дорожная карта

~~Зачеркнутые~~ пункты в указанном варианте уже реализованы: 

  * Стресс-тестирование [всегда]
  * ~~Addition of Telnet and FTP clients (experimental level) [0.3]~~ (done)
  * ~~Cementing of TCP/UDP API [0.3]~~ (done)


  * Добавление протокола ICMP Ping/Traceroute [na] (отменено по разным причинам*)
  * ~~Addition of SMTP, HTTP components [0.4]~~ (done)
  * ~~Stabilization of Telnet and FTP clients [0.5]~~ (done)
  * ~~Addition of MIME streams [0.5.1]~~ (done)
  * Добавление Telnet и FTP серверов (экспериментальный уровень) [???]
  * ~~Addition of SSL [0.6+]~~ (done)
  * Добавлена ​​поддержка набора виджетов Carbon и/или Cocoa [0.7+]
  * ~~Addition of Qt4 widget set support [0.6.4+]~~ (done)
  * Cleanup of SSL/HTTP (so pure HTTP doesn't depend on TLSSLSocket) [0.6.x?]



*Для PING требуются необработанные сокеты, которые доступны только root на платформах unix/linux. Было решено, что lNet не будет поддерживать это, пользователи, которым нужен PING, должны использовать программы ping и ping6 через TProcess. 

## Требования

LNet 0.6+: 

  * Lazarus 0.9.24+ and FPC 2.2.0+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 7.10). FastCGI considered broken for now.



LNet 0.5: 

  * Lazarus 0.9.22+ and FPC 2.0.4+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 6.10) and FreeBSD 5.5. FastCGI considered broken for now. **WinCE requires Lazarus 0.9.23+ to compile!**



LNet 0.4: 

  * Lazarus 0.9.18+ and FPC 2.0.4+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 6.06LTS) and FreeBSD 6.1. HTTP server has issues under high load with fastcgi usage. FastCGI works only in Linux for now.



### Специальное уведомление под Ubuntu

Некоторые «отсутствующие файлы» фактически находятся в подпапке «/net» и не обнаруживаются компилятором fpc. Просто скопируйте недостающие файлы из подпапки (lnet.pas, tomwinsock.pas, lfunc.inc...) '/usr/lib/lazarus/comComponents/lnetpackage/lnet' в '/usr/lib/lazarus/comComponents/lnetpackage'. ' (при необходимости измените пути). Затем завершите настройку, следуя процедуре установки. (от Steph12358) 

## Изменения в API

**0.1 до 0.2**

  * Были внесены некоторые изменения в API с версии 0.1 до 0.2. Это было необходимо и должно улучшить удобство использования пакетов. Я знаю, что изменения API — это всегда головная боль (PITA - аббр. pain in the ass), но с этого момента я постараюсь свести их к минимуму. Версия 0.3 будет первой бета-версией, API которой останется неизменным (возможно будет только добавление новых вещей).
  * `OnRecieve()` изменен на `OnReceive()`.
  * `Accept()` изменен на `Listen()`.
  * Аргументы всех событий изменены. `SocketNumber(int)` был изменен на `aSocket(TLSocket)` по логическим соображениям и причинам скорости.



**0.2 до 0.3**

  * Добавлены `OnCanSend` и `OnConnect`. Connect теперь не блокируется.
  * Удалены `buffersize` и `maxmsgs`. `buffersize` больше не требуется, поскольку внутренний буфер не используется.



**0.3 до 0.4**

  * Множество дополнений. Единственная «поломка» заключается в том, что `TLSocket.Port` был изменен на `TLSocket.LocalPort` и `TLSocket.PeerPort` по соображениям согласованности.



**0.4 до 0.5**

  * Мы провели некоторую очистку API, которая привела к поломке API.
  * Все классы "connection" теперь происходят от TLComponent.
  * `TLSocket` теперь имеет свойство `.Creator`, который является свойством `TLComponent`, определяющим создателя (например, `TLTcp`), поэтому теперь вы можете видеть, какой сокет в событии принадлежит какому соединению.
  * Добавлены вызовы без параметров `Connect()` и `Listen()` для всех соединений. В основном это делается для того, чтобы включить визуальный стиль Lazarus, чтобы люди могли использовать свойства `.Host` и `.Port` и неоднократно просто вызывать, например, `.Connect()`.
  * Добавлен набор `StatusSet` для компонентов SMTP и FTP. Это позволяет отслеживать определенные действия FTP или SMTP и их результаты с помощью событий `OnSuccess` и `OnFailure`. Вы устанавливаете события для просмотра в наборе и получаете результаты в одном из обратных вызовов.
  * **Изменены ВСЕ обратные вызовы, которые не использовали TLSocket в качестве отправителя, чтобы использовать его. Это означает, что все события FTP, SMTP и HTTP, которые имели стиль "Sender: TL <HigherType>", были возвращены к базовым обратным вызовам в стиле Socket!**



**0.5.4- до 0.5.5+**

  * Были внесены некоторые незначительные изменения в API, которые не должны ничего нарушить, но я перечисляю их на всякий случай.
  * Свойство `.Timeout` для событий и соединений было задано как Integer (из DWord) для поддержки -1 как «блокировки до тех пор, пока не произойдет событие».



**0.5 до 0.6**

  * Никаких изменений в публичном API, только дополнения.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В версии 0.4.0 я забыл переместить модуль lHTTPSettings из библиотеки в пример http. Он НЕ должен был быть частью библиотеки, просто примером. НЕ используйте это, это не «библиотека», это часть примера (http-сервер получит собственный проект позже) 

## lNet для FreeBSD, Linux, macOS и Windows

  * [lNet для FreeBSD, Linux, macOS и Windows](<https://github.com/trevoz/lnet>) от trevoz 
    * Множество изменений: теперь библиотека будет компилироваться с помощью FPC 3.2.2 или FPC 3.3.1.
    * Добавлен TLSv1.3.
    * Linux TLS не работает из-за проблемы с e-poll в файле `lib/sys/lepolleventer.inc`.
    * Примеры консоли компилируются (я протестировал пример SMTP, который мне и нужен).
    * Примеры визуальных компонентов не компилируются для macOS — в них нет привязок интерфейса Cocoa или даже несуществующего Carbon.



## См.также

  * [lNet examples](<../en/lNet_examples.md> "lNet examples")

---

_Source: [https://wiki.freepascal.org/lNet/ru](https://web.archive.org/web/20250426233635/https://wiki.freepascal.org/lNet/ru)_
