# Networking

│ **[English (en)](<../en/Networking.md>)** │  **русский (ru)** │


Эта страница будет началом руководства по сетевому(network) програмированию в Lazarus. Я не эксперт в сетевом программировании и я буду добавлять статьи по мере моего изучения. Я приглашаю других помочь в создании статей по сетям. Просто добавьте ссылку на следующую секцию, добавьте страницу и создайте свою собственную WiKi-статью. На этой странице будет даваться общая информация. 

  


## Contents

  * 1 Другие руководства по сетям
  * 2 Протокол TCP/IP
    * 2.1 CGI/FastCGI - REST, CRUD, чат, блог, веб-страницы и т. д.
    * 2.2 SSH/Telnet клиент, отправка почты, загрузка файлов, OAuthv1 примеры
    * 2.3 Пример веб-сервера
  * 3 Веб-сервисы (WebServices)
    * 3.1 Web Service Toolkit для FPC & Lazarus



## Другие руководства по сетям

  * [ Безопасное программирование](<../en/Secure_programming.md> "Secure programming")
  * [Sockets](<../en/Sockets.md> "Sockets") \- Компоненты TCP/IP Sockets 
  * [lNet](<../en/lNet.md> "lNet") \- Легкие сетевые компоненты (Lightweight Networking Components) 
  * [XML Tutorial](<../en/XML_Tutorial.md> "XML Tutorial") \- XML часто используется для передачи по сетям 
  * [ FPC и модули Apache](<FPC_and_Apache_Modules.md> "FPC and Apache Modules/ru")



## Протокол TCP/IP

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Так как существуют несколько библиотек, которые обеспечивают функционирование сети в FPC/Lazarus(Synapse, lnet, fphttpclient, Indy,...), многие примеры могут быть использованы с различными библиотеками. Посетите [[1]](<http://brookframework.org>) или [[2]](<https://bitbucket.org/reiniero/fpctwit>), что бы узнать, как использовать различные сетевые библиотеки.

### CGI/FastCGI - REST, CRUD, чат, блог, веб-страницы и т. д.

Эти функции могут быть использованы с [fcl-web](<../en/fcl-web.md> "fcl-web"). Они также построены в рамках [Brook Framework](<../en/Brook_Framework.md> "Brook Framework"). 

### SSH/Telnet клиент, отправка почты, загрузка файлов, OAuthv1 примеры

Смотрите на странице [Synapse](<../en/Synapse.md> "Synapse"). 

### Пример веб-сервера

Ниже находится пример http-сервера, написанный в Synapse и оттестированный в Mac OS X, после изменения исходников synapse для использования константы $20000 как MSG_NOSIGNAL, потому что эта константа не существует в sockets unit в Mac OS X. В примере используются компоненты Ararat Synapse, которые можно получить [здесь](<http://synapse.ararat.cz/doku.php/download>). 
    
    
    {
      The Micro Pascal WebServer
     
      This is a very simple example webserver implemented with the Synapse library.
     
      It works with blocking sockets and a single thread, so it
      can only handle one request at a given time.
     
      It will write the headers that it receives from the browser
      to the standard output.
     
      It serves a fixed webpage for the / URI
      For any other URI it will return 504 not found
    }
    program upserver;
     
    {$ifdef fpc}
      {$mode delphi}
    {$endif}
     
    {$apptype console}
     
    uses
      Classes, blcksock, sockets, Synautil, SysUtils;
     
    {@@
      Attends a connection. Reads the headers and gives an
      appropriate response
    }
    procedure AttendConnection(ASocket: TTCPBlockSocket);
    var
      timeout: integer;
      s: string;
      method, uri, protocol: string;
      OutputDataString: string;
      ResultCode: integer;
    begin
      timeout := 120000;
     
      WriteLn('Received headers+document from browser:');
     
      //read request line
      s := ASocket.RecvString(timeout);
      WriteLn(s);
      method := fetch(s, ' ');
      uri := fetch(s, ' ');
      protocol := fetch(s, ' ');
     
      //read request headers
      repeat
        s := ASocket.RecvString(Timeout);
        WriteLn(s);
      until s = '';
     
      // Now write the document to the output stream
     
      if uri = '/' then
      begin
        // Write the output document to the stream
        OutputDataString :=
          '<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"'
          + ' "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">' + CRLF
          + '<html><h1>Teste</h1></html>' + CRLF;
     
        // Write the headers back to the client
        ASocket.SendString('HTTP/1.0 200' + CRLF);
        ASocket.SendString('Content-type: Text/Html' + CRLF);
        ASocket.SendString('Content-length: ' + IntTostr(Length(OutputDataString)) + CRLF);
        ASocket.SendString('Connection: close' + CRLF);
        ASocket.SendString('Date: ' + Rfc822DateTime(now) + CRLF);
        ASocket.SendString('Server: Servidor do Felipe usando Synapse' + CRLF);
        ASocket.SendString('' + CRLF);
     
      //  if ASocket.lasterror <> 0 then HandleError;
     
        // Write the document back to the browser
        ASocket.SendString(OutputDataString);
      end
      else
        ASocket.SendString('HTTP/1.0 504' + CRLF);
    end;
     
    var
      ListenerSocket, ConnectionSocket: TTCPBlockSocket;
    begin
      ListenerSocket := TTCPBlockSocket.Create;
      ConnectionSocket := TTCPBlockSocket.Create;
     
      ListenerSocket.CreateSocket;
      ListenerSocket.setLinger(true,10);
      ListenerSocket.bind('0.0.0.0','1500');
      ListenerSocket.listen;
     
      repeat
        if ListenerSocket.canread(1000) then
        begin
          ConnectionSocket.Socket := ListenerSocket.accept;
          WriteLn('Attending Connection. Error code (0=Success): ', ConnectionSocket.lasterror);
          AttendConnection(ConnectionSocket);
        end;
      until false;
     
      ListenerSocket.Free;
      ConnectionSocket.Free;
    end.

## Веб-сервисы (WebServices)

Согласно [W3C](<http://www.w3.org/>) "веб-сервис" - это программная система, разработанная для поддержки взаимодействия комппьютер-компьютер по сети. Он имеет интерфейс, описанный в машинном формате как, например, [WSDL](<http://ru.wikipedia.org/wiki/WSDL>). Другие системы взаимодействуют веб-сервисом, способом, описанным их интерфейсом, используя сообщения, которые могут быть заключены в [SOAP](<http://ru.wikipedia.org/wiki/SOAP>) упаковку или претворять собой [REST](<http://ru.wikipedia.org/wiki/REST>) подход. Эти сообщения обычно передаются используя HTTP и являются содержимым XML в соединении с другим веб-стандартом. Программные приложения, написанные на разных языках программирования и запускаемые на разных платформах могут использовать веб-сервисы для обмена данными по компьютерным сетям, таким как интернет, методом, напоминающим меж-процессное(inter-process) взаимодействие внутри одного компьютера. Эта возможность компьютерного взаимодействия (например Windows и Linux приложений) обязана своим существованием использованию открытых стандартов. [OASIS](<http://ru.wikipedia.org/wiki/OASIS>) и [W3C(wiki-ссылка)](<http://ru.wikipedia.org/wiki/W3C>) \- это главные комитеты, отвечающие за архитектуру и стандартизацию веб-сервисов. Для того, чтобы улучшать возможности взаимодействия между реализациями веб-сервисов, организация WS-I создает серии описаний для дальнейшего определения участвующих стандартов. 

### Web Service Toolkit для FPC & Lazarus

[Web Service Toolkit](<../en/Web_Service_Toolkit.md> "Web Service Toolkit") \- это web services package для FPC и Lazarus. 

* * *

[XML Tutorial](<../en/XML_Tutorial.md> "XML Tutorial")

---

_Source: [https://wiki.freepascal.org/Networking/ru](https://web.archive.org/web/20180719000758/https://wiki.freepascal.org/Networking/ru)_
