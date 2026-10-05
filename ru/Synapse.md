# Synapse

│ **[English (en)](<../en/Synapse.md>)** │  **русский (ru)** │

Synapse предоставляет простой в использовании последовательный порт и синхронную библиотеку TCP/IP. 

## Contents

  * 1 Обзор
  * 2 Установка
  * 3 Поддержка и сообщения об ошибках
  * 4 Поддержка SSL/TLS
    * 4.1 Отсутствущая библиотека
  * 5 Пример веб-сервера
  * 6 Пример запроса сервера QOTD
  * 7 Отправка email
    * 7.1 Отправка вложений
  * 8 Загрузка файлов
    * 8.1 С FTP-сервера
    * 8.2 С HTTP-сервера
      * 8.2.1 Простая версия
      * 8.2.2 Расширенная версия
      * 8.2.3 Простая версия с показом прогресса загрузки
      * 8.2.4 С HTTP-сервера путем анализа URL-адресов: Sourceforge
    * 8.3 С HTTPS-сервера
  * 9 Пример программы клиента SSH/Telnet
    * 9.1 Требования
      * 9.1.1 Cryptlib
      * 9.1.2 LibSSH2
    * 9.2 Synapse libssh2 SSL plugin
    * 9.3 Класс клиента терминала
    * 9.4 Пример клиентского кода
  * 10 Интреграция OAuth с v1/Twitter/Plurk
  * 11 Другие статьи о Интернете и сетях
  * 12 См.также
  * 13 Внешние ссылки



## Обзор

Synapse предлагает [последовательный](<../en/Hardware_Access.md> "Hardware Access") порт и возможность подключения по TCP/IP. Он отличается от других библиотек тем, что вам нужно только добавить в свой код несколько файлов исходного кода Synapse Pascal; нет необходимости устанавливать пакеты и т. д. Единственное исключение - вам понадобится внешняя криптографическая библиотека, если вы хотите использовать шифрование, такое как SSL/TLS/SSH. 

См. документацию на официальном сайте (ссылка ниже) для более подробной информации. 

## Установка

Установка может быть такой же простой, как простое копирование всех файлов в каталог вашего приложения и добавление соответствующих модулей Synapse в ваш раздел _uses_. 

Более элегантный и рекомендуемый способ - это компиляция пакета laz_synapse.lpk, чтобы вы могли использовать одни и те же модули во всех своих проектах. 

Страница информации по загрузке/SVN: [Страница загрузки Synapse](<http://www.ararat.cz/synapse/doku.php/download>)

## Поддержка и сообщения об ошибках

У проекта Synapse есть список рассылки, в котором предоставляется поддержка и можно отправлять исправления. 

Отчеты об ошибках также можно отправлять по списку рассылки. 

См. [страницу поддержки Synapse](<http://www.ararat.cz/synapse/doku.php/support>)

## Поддержка SSL/TLS

Вы можете использовать поддержку OpenSSL, CryptLib, StreamSecII или OpenStreamSecII SSL с Synapse. По умолчанию поддержка SSL не используется. 

Поддержка активируется путем помещения выбранного имени модуля в раздел uses вашего проекта. Вы также должны поместить файл двоичной библиотеки в путь к вашему проекту(Windows) или установить его в путях поиска библиотеки (Linux, macOS, FreeBSD). 

Synapse загружает файлы библиотеки SSL во время выполнения как динамические библиотеки. 

  * Для получения подробной информации см. [SSL/TLS Plugin Architecture](<http://www.ararat.cz/synapse/doku.php/public:howto:sslplugin>)
  * Некоторые криптографические библиотеки можно получить [здесь](<http://synapse.ararat.cz/files/crypt/>)



### Отсутствущая библиотека

В Linux вам необходимо убедиться, что требуемая динамическая библиотека присутствует/установлена ​​в вашей системе. В случае cryptlib, если библиотека отсутствует в системе, при компоновке появляется сообщение об ошибке: 
    
    
    /usr/bin/ld: cannot find -lcl
    

Аналогичное сообщение будет отображаться при использовании других динамических библиотек. 

## Пример веб-сервера

См. [примеры Webserver](<../en/Networking.md> "Networking"). 

## Пример запроса сервера QOTD

См. [пример запроса сервера Quote of the Day](<../en/QOTD.md> "QOTD"). 

## Отправка email

Статья _Michaël Van Canneyt_ об отправке электронной почты, включая вложения, с помощью Synapse: <http://www.freepascal.org/~michael/articles/lazmail/lazmail-en.pdf>

Из сообщения [на форуме](<http://forum.lazarus.freepascal.org/index.php/topic,21157.msg123501.html#msg123501>); работает, например, с Gmail: 
    
    
    {Этот код поддерживает использование шифрования TLS/SSL; при отправке на порт 25 используется простой текстовый SMTP.}
    
    uses
      ..., smtpsend,ssl_openssl; //вероятно, можно использовать и другие модули SSL.
    
    // MailData - это текст письма.
    function SendMail(
      User, Password, 
      MailFrom, MailTo, 
      SMTPHost, SMTPPort: string; 
      MailData: string): Boolean;
    var
      SMTP: TSMTPSend;
      sl:TStringList;
    begin
      Result:=False;
      SMTP:=TSMTPSend.Create;
      sl:=TStringList.Create;
      try
        sl.text:=Maildata;
        SMTP.UserName:=User;
        SMTP.Password:=Password;
        SMTP.TargetHost:=SMTPHost;
        SMTP.TargetPort:=SMTPPort;
        SMTP.AutoTLS:=true;
        if Trim(SMTPPort)<>'25' then
          SMTP.FullSSL:=true; // при отправке на порт 25 шифрование не используется
        if SMTP.Login then
        begin
          result:=SMTP.MailFrom(MailFrom, Length(MailData)) and
             SMTP.MailTo(MailTo) and
             SMTP.MailData(sl);
          SMTP.Logout;
        end;
      finally
        SMTP.Free;
        sl.Free;
      end;
    end;
    

### Отправка вложений

См. [документацию Synapse](<http://synapse.ararat.cz/doku.php/public:howto:tmimepart>). 

## Загрузка файлов

### С FTP-сервера

Учитывая URL-адрес и (путь и) имя файла, это загрузит его с FTP-сервера. В основном это обертка кода Synapse, предназначенная для облегчения загрузки при работе с произвольными файлами. Если вы точно знаете, что будете скачивать, простой вызов Synapse: 
    
    
    FtpGetFile
    

доставит вас очень далеко. 
    
    
    function DownloadFTP(URL, TargetFile: string): boolean;
    const
      FTPPort=21;
      FTPScheme='ftp://'; //Имя схемы URI для URL-адресов FTP
    var
      Host: string;
      Port: integer;
      Source: string;
      FoundPos: integer;
    begin
      // Вычеркиваем информацию о схеме:
      if LeftStr(URL, length(FTPScheme))=FTPScheme then URL:=Copy(URL, length(FTPScheme)+1, length(URL));
    
      // Грубый парсинг; мог использовать код синтаксического анализа URI в пакетах FPC ...
      FoundPos:=pos('/', URL);
      Host:=LeftStr(URL, FoundPos-1);
      Source:=Copy(URL, FoundPos+1, Length(URL));
    
      //Проверка номера портов:
      FoundPos:=pos(':', Host);
      Port:=FTPPort;
      if FoundPos>0 then
      begin
        Host:=LeftStr(Host, FoundPos-1);
        Port:=StrToIntDef(Copy(Host, FoundPos+1, Length(Host)),21);
      end;
      Result:=FtpGetFile(Host, IntToStr(Port), Source, TargetFile, 'anonymous', 'fpc@example.com');
      if result=false then writeln('DownloadFTP: error downloading '+URL+'. Details: host: '+Host+'; port: '+Inttostr(Port)+'; remote path: '+Source+' to '+TargetFile);
    end;
    

Пример получения списка файлов по заданному пути 
    
    
    //Используем модуль ftpsend
    
    function FtpGetDir(const IP, Port, Path, User, Pass: string; DirList: TStringList): Boolean;
    var
      i: Integer;
      s: string;
    begin
      Result := False;
      with TFTPSend.Create do
      try
        Username := User;
        Password := Pass;
        TargetHost := IP;
        TargetPort := Port;
        if not Login then
          Exit;
        Result := List(Path, False);
        for i := 0 to FtpList.Count -1 do
        begin
          s := FTPList[i].FileName;
          DirList.Add(s);
        end;
        Logout;
      finally
        Free;
      end;
    end;
    

### С HTTP-сервера

Учитывая URL-адрес и (путь и) имя файла, это загрузит его с HTTP-сервера. Обратите внимание, что этот код проверяет код состояния HTTP (например, 200, 404), чтобы узнать, является ли документ, который мы получили обратно с сервера, желаемым файлом или же страницей с ошибкой. 

#### Простая версия
    
    
    ...
    uses httpsend,
    ...
    function DownloadHTTP(URL, TargetFile: string): Boolean;
    var
      HTTPGetResult: Boolean;
      HTTPSender: THTTPSend;
    begin
      Result := False;
      HTTPSender := THTTPSend.Create;
      try
        HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
        if (HTTPSender.ResultCode >= 100) and (HTTPSender.ResultCode<=299) then begin
          HTTPSender.Document.SaveToFile(TargetFile);
          Result := True;
        end; 
      finally
        HTTPSender.Free;
      end;
    end;
    

#### Расширенная версия
    
    
    ...
    uses httpsend
    ...
    function DownloadHTTP(URL, TargetFile: string): Boolean;
    // Грузим файл; при необходимости повторяем попытку.
    // Можно использовать Synapse HttpGetBinary, но это не работает
    // с кодами результата (т.е. он успешно загружает документ с ошибкой 404)
    const
      MaxRetries = 3;
    var
      HTTPGetResult: Boolean;
      HTTPSender: THTTPSend;
      RetryAttempt: Integer;
    begin
      Result := False;
      RetryAttempt := 1;
      HTTPSender := THTTPSend.Create;
      try
        try
          // Пробуем получить файл
          HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
          while (HTTPGetResult = False) and (RetryAttempt < MaxRetries) do
          begin
            Sleep(500 * RetryAttempt);
            HTTPSender.Clear;
            HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
            RetryAttempt := RetryAttempt + 1;
          end;
          // Если у нас есть ответ от сервера, проверяем, не
          // был ли файл уже отправлен нам.
          case HTTPSender.Resultcode of
            100..299:
              begin
                HTTPSender.Document.SaveToFile(TargetFile);
                Result := True;
              end; //информирование, успешно
            300..399: Result := False; // перенаправление. Не реализовано, но могло бы быть.
            400..499: Result := False; // ошибка клиента; 404 не найдено и т.д.
            500..599: Result := False; // внутренняя ошибка сервера
            else Result := False; // неизвестный код
          end;
        except
          // Нас не интересует причина этой ошибки; загрузка не удалась.
          Result := False;
        end;
      finally
        HTTPSender.Free;
      end;
    end;
    

#### Простая версия с показом прогресса загрузки

В следующем примере показано, как получить информацию о ходе загрузки по HTTP, а также размер файла. Размер файла извлекается из информации заголовка. 
    
    
    unit uhttpdownloader;
    
    // Необходимо изменить это. По умолчанию это - {$mode objfpc}{$H+}, и не работает.
    {$mode Delphi}
    
    interface
    
    uses
      Classes, SysUtils, httpsend, blcksock, typinfo;
    
    type
      IProgress = interface
        procedure ProgressNotification(Text: String; CurrentProgress : integer; MaxProgress : integer);
      end;
    
    type
      { THttpDownloader }
    
      THttpDownloader = class
      public
        function DownloadHTTP(URL, TargetFile: string; ProgressMonitor : IProgress): Boolean;
      private
        Bytes : Integer;
        MaxBytes : Integer;
        HTTPSender: THTTPSend;
        ProgressMonitor : IProgress;
        procedure Status(Sender: TObject; Reason: THookSocketReason; const Value: String);
        function GetSizeFromHeader(Header: String):integer;
      end;
    
    implementation
    
    function THttpDownloader.DownloadHTTP(URL, TargetFile: string; ProgressMonitor : IProgress): Boolean;
    var
      HTTPGetResult: Boolean;
    begin
      Result := False;
      Bytes:= 0;
      MaxBytes:= -1;
      Self.ProgressMonitor:= ProgressMonitor;
    
      HTTPSender := THTTPSend.Create;
      try
        HTTPSender.Sock.OnStatus:= Status;
        HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
        if (HTTPSender.ResultCode >= 100) and (HTTPSender.ResultCode<=299) then begin
          HTTPSender.Document.SaveToFile(TargetFile);
          Result := True;
        end;
      finally
        HTTPSender.Free;
      end;
    end;
    
    procedure THttpDownloader.Status(Sender: TObject; Reason: THookSocketReason; const Value: String);
    var
      V, currentHeader: String;
      i: integer;
    begin
      //try to get filesize from headers
      if (MaxBytes = -1) then
      begin
        for i:= 0 to HTTPSender.Headers.Count - 1 do
        begin
          currentHeader:= HTTPSender.Headers[i];
          MaxBytes:= GetSizeFromHeader(currentHeader);
          if MaxBytes <> -1 then break;
        end;
      end;
    
      V := GetEnumName(TypeInfo(THookSocketReason), Integer(Reason)) + ' ' + Value;
    
      if Reason = THookSocketReason.HR_ReadCount then
      begin
        Bytes:= Bytes + StrToInt(Value);
        ProgressMonitor.ProgressNotification(V, Bytes, MaxBytes);
      end;
    end;
    
    function THttpDownloader.GetSizeFromHeader(Header: String): integer;
    var
      item : TStringList;
    begin
      Result:= -1;
    
      if Pos('Content-Length:', Header) <> 0 then
      begin
        item:= TStringList.Create();
        item.Delimiter:= ':';
        item.StrictDelimiter:=true;
        item.DelimitedText:=Header;
        if item.Count = 2 then
        begin
          Result:= StrToInt(Trim(item[1]));
        end;
      end;
    end;
    
    end.
    

Что мы здесь делаем? 

Прежде всего, мы смотрим на заголовки, чтобы узнать размер файла. Надо подождать и проверить, есть ли заголовок. Первые события не содержат Content-Length: information. 

Найдя, мы извлекаем эту информацию. Здесь появляется несколько событий, на которые вы можете реагировать. Но мы проверяем только THookSocketReason.HR_ReadCount в этом примере. 

HR_ReadCount предоставляет нам информацию о том, сколько байтов было прочитано с момента последнего события. 

Затем прогресс передается в пользовательский интерфейс: 
    
    
    procedure TMainForm.ProgressNotification(Text: String; CurrentProgress: integer; MaxProgress: integer);
    begin
      if (MaxProgress <> -1) then
      begin
        ProgressBar.Max:= MaxProgress;
      end;
    
      ProgressBar.Position:= CurrentProgress;
      memoStatus.Lines.Add(Text);
      Application.ProcessMessages;
    end;
    

Итак, окончательно основной модуль будет таким: 
    
    
    unit uMain;
    
    {$mode Delphi}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls, ComCtrls, httpsend, blcksock, typinfo,
      uhttpdownloader;
    
    type
    
      { TMainForm }
    
      TMainForm = class(TForm, IProgress)
        btnStartDownload: TButton;
        edtUrl: TEdit;
        labelUrl: TLabel;
        memoStatus: TMemo;
        ProgressBar: TProgressBar;
        SaveDialog: TSaveDialog;
        procedure btnStartDownloadClick(Sender: TObject);
      private
        { private declarations }
        function GetFileNameFromURL(url: String):string;
      public
        { public declarations }
        procedure ProgressNotification(Text: String; CurrentProgress : integer; MaxProgress : integer);
      end;
    
    var
      MainForm: TMainForm;
    
    implementation
    
    {$R *.lfm}
    
    { TMainForm }
    
    procedure TMainForm.btnStartDownloadClick(Sender: TObject);
    var
      fileName: String;
      downloader: THttpDownloader;
      success: boolean;
    begin
      fileName:= GetFileNameFromURL(edtUrl.Text);
      SaveDialog.FileName:=fileName;
      if (SaveDialog.Execute) then
      begin
        memoStatus.Lines.Clear;
        ProgressBar.Position:=0;
        downloader:= THttpDownloader.Create();
        success:= downloader.DownloadHTTP(edtUrl.Text, SaveDialog.FileName, Self);
    
        ProgressBar.Position:=0;
        if Success then
          memoStatus.Lines.Add('Download successful')
        else
          memoStatus.Lines.Add('Error during download');
    
      end;
    end;
    
    function TMainForm.GetFileNameFromURL(url: String): string;
    var i, l : integer;
        fileName, current : String;
    begin
      fileName:= '';
      l:= Length(url);
      for i:= l downto 0 do begin
        current:= url[i];
        if current <> '/' then
        begin
          fileName:= current + fileName;
        end else begin
          Result:= fileName;
          break;
        end;
      end;
    end;
    
    procedure TMainForm.ProgressNotification(Text: String; CurrentProgress: integer; MaxProgress: integer);
    begin
      if (MaxProgress <> -1) then ProgressBar.Max:= MaxProgress;
      ProgressBar.Position:= CurrentProgress;
      memoStatus.Lines.Add(Text);
      Application.ProcessMessages;
    end;
    
    end.
    

Ссылка: <https://andydunkel.net/2015/09/09/lazarus_synapse_progress/>

#### С HTTP-сервера путем анализа URL-адресов: Sourceforge

См. статью [Загрузка с SourceForge](<../en/Download_from_SourceForge.md> "Download from SourceForge") для примера загрузки с sourceforge.net. 

### С HTTPS-сервера

Это похоже на загрузку с HTTP-сервера. Кроме того, вам необходимо [ активировать поддержку SSL/TLS](<Synapse.md> "Synapse/ru") и получить двоичный файл(ы) для необходимой библиотеки. Затем вы можете использовать ту же функцию DownloadHTTP для загрузки файла с URL-адреса, начинающегося с **https://**. 

## Пример программы клиента SSH/Telnet

Ниже вы найдете модуль, который позволяет использовать клиентские функции telnet/SSH, использующий модуль synapse `tlntsend.pas`. Пример программы показывает, как это использовать. Другой, более простой способ проиллюстрирован _Leonardo Ramé_ на [[1]](<http://leonardorame.blogspot.com/2010/01/synapse-based-ssh-client.html>). Его пример не может использовать telnet и отправляет только одну команду. 

### Требования

Помимо исходников Synapse (которых вам понадобится всего пара), если вы хотите использовать функциональность SSH, вам понадобится библиотека шифрования, которую использует Synapse. Если вы используете только Telnet, вам это не нужно. 

Есть 2 варианта: 

  * Библиотека Cryptlib. Преимущество: стабильность. По-видимому, можно использовать закрытые ключи, но они имеют некоторый формат, который широко не поддерживается.
  * Библиотека LibSSH2. Привязки Pascal все еще находятся в разработке, но вы можете использовать файл с вашим закрытым ключом (в формате OpenSSH) для аутентификации.



#### Cryptlib

  * В Windows загрузите двоичную версию библиотеки DLL cryptlib (CL32.DLL) и поместите ее в исходный каталог. Если вы компилируете в другой каталог или распространяете свою программу, вам также необходимо будет распространить DLL.
  * В Linux и OSX установите cryptlib через диспетчер пакетов/другими способами. При распространении приложения отметьте cryptlib как требование в вашем пакете .deb/.rpm/любой.



Вам также потребуются привязки (cryptlib.pas), присутствующие в исходном дистрибутиве cryptlib. 

Версии двоичного файла cryptlib и привязок должны совпадать. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Кажется, что cryptlib не подходит для подключения к машинам с Linux, хотя AIX работает. Вместо этого используйте SSH2.

#### LibSSH2

  * В Windows загрузите двоичную версию библиотеки libssh2 (LIBSSH2.DLL) и поместите ее в исходный каталог. Если вы компилируете в другой каталог или распространяете свою программу, вам также необходимо будет распространить DLL.
  * В Linux и macOS установите libssh2 через диспетчер пакетов/другими способами. При распространении вашего приложения: 
    * Linux: отметьте libssh2 как требование в вашем .deb/.rpm/любом пакете.
    * macOS: включите libssh2 в каталог ресурсов [Application Bundle](<../en/Application_Bundle.md> "Application Bundle").



Вам также понадобится ssl_libssh2.pas (см. ниже) и привязки: (libssh2.pas, см. [это сообщение на форуме ](<http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465>)). Двоичный файл libssh2 и привязки должны совпадать. 

### Synapse libssh2 SSL plugin

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** плагин не доработан.
    
    
    {
      ssl_libssh2.pas version 0.2
    
      Плагин поддержки SSH2 (черновик) для библиотеки Synapse (http://www.ararat.cz/synapse) от LibSSH2 (http://libssh2.org)
      Требуется: интерфейс libssh2 pascal - http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465 и
      libssh2.dll с OpenSSL.
    
      (С) Alexey Suhinin http://x-alexey.narod.ru
    }
    
    {$IFDEF FPC}
      {$MODE DELPHI}
    {$ENDIF}
    {$H+}
    
    unit ssl_libssh2;
    
    interface
    
    uses
      SysUtils,
      blcksock, synsock,
      libssh2;
    
    type
      {: @abstract(класс, реализующий плагин CryptLib SSL/SSH.)
       Экземпляр этого класса будет создан для каждого @link(TTCPBlockSocket).
       Вам не нужно создавать экземпляр этого класса, все делает сам Synapse!} 
      TSSLLibSSH2 = class(TCustomSSL)
      protected
        FSession: PLIBSSH2_SESSION;
        FChannel: PLIBSSH2_CHANNEL;
        function SSHCheck(Value: integer): Boolean;
        function DeInit: Boolean;
      public
        {:См. @inherited}
        constructor Create(const Value: TTCPBlockSocket); override;
        destructor Destroy; override;
        function Connect: boolean; override;
        function LibName: String; override;
        function Shutdown: boolean; override;
        {:See @inherited}
        function BiShutdown: boolean; override;
        {:See @inherited}
        function SendBuffer(Buffer: TMemory; Len: Integer): Integer; override;
        {:See @inherited}
        function RecvBuffer(Buffer: TMemory; Len: Integer): Integer; override;
        {:See @inherited}
        function WaitingData: Integer; override;
        {:See @inherited}
        function GetSSLVersion: string; override;
      published
      end;
    
    implementation
    
    {==============================================================================}
    function TSSLLibSSH2.SSHCheck(Value: integer): Boolean;
    var
      PLastError: PAnsiChar;
      ErrMsgLen: Integer;
    begin
      Result := true;
      FLastError := 0;
      FLastErrorDesc := '';
      if Value<0 then
      begin
        FLastError := libssh2_session_last_error(FSession, PLastError, ErrMsglen, 0);
        FLastErrorDesc := PLastError;
        Result := false;
      end;
    end;
    
    
    function TSSLLibSSH2.DeInit: Boolean;
    begin
      if Assigned(FChannel) then
      begin
        libssh2_channel_free(FChannel);
        FChannel := nil;
      end;
      if Assigned(FSession) then
      begin
        libssh2_session_disconnect(FSession,'Goodbye');
        libssh2_session_free(FSession);
        FSession := nil;
      end;
      FSSLEnabled := False;
      Result := true;
    end;
    
    constructor TSSLLibSSH2.Create(const Value: TTCPBlockSocket);
    begin
      inherited Create(Value);
      FSession := nil;
      FChannel := nil;
    end;
    
    destructor TSSLLibSSH2.Destroy;
    begin
      DeInit;
      inherited Destroy;
    end;
    
    function TSSLLibSSH2.Connect: boolean;
    begin
      Result := False;
      if SSLEnabled then DeInit;
      if (FSocket.Socket <> INVALID_SOCKET) and (FSocket.SSL.SSLType = LT_SSHv2) then
        begin
          FSession := libssh2_session_init();
          if not Assigned(FSession) then
          begin
            FLastError := -999;
            FLastErrorDesc := 'Cannot initialize SSH session';
            exit;
          end;
          if not SSHCheck(libssh2_session_startup(FSession, FSocket.Socket)) then
            exit;
          if (FSocket.SSL.PrivateKeyFile<>'') then
          begin
            if (not SSHCheck(libssh2_userauth_publickey_fromfile(FSession, PChar(FSocket.SSL.Username), nil, PChar(FSocket.SSL.PrivateKeyFile), PChar(FSocket.SSL.KeyPassword)))) then
              exit;
          end
          else
          if (FSocket.SSL.Username<>'') and (FSocket.SSL.Password<>'') then
          begin
            if (not SSHCheck(libssh2_userauth_password(FSession, PChar(FSocket.SSL.Username), PChar(FSocket.SSL.Password)))) then
              exit;
          end;
          FChannel := libssh2_channel_open_session(FSession);
          if not assigned(FChannel) then
          begin
            SSHCheck(-1); // получаем ошибку
            if FLastError = 0 then
            begin
              FLastError := -999; // неизвестная ошибка
              FLastErrorDesc := 'Cannot open session';
            end;
            exit;
          end;
          if not SSHCheck(libssh2_channel_request_pty(FChannel, 'vanilla')) then
            exit;
          if not SSHCheck(libssh2_channel_shell(FChannel)) then
            exit;
          FSSLEnabled := True;
          Result := True;
        end;
    end;
    
    function TSSLLibSSH2.LibName: String;
    begin
      Result := 'ssl_libssh2';
    end;
    
    function TSSLLibSSH2.Shutdown: boolean;
    begin
      Result := DeInit;
    end;
    
    
    function TSSLLibSSH2.BiShutdown: boolean;
    begin
      Result := DeInit;
    end;
    
    function TSSLLibSSH2.SendBuffer(Buffer: TMemory; Len: Integer): Integer;
    begin
      Result:=libssh2_channel_write(FChannel, PChar(Buffer), Len);
      SSHCheck(Result);
    end;
    
    function TSSLLibSSH2.RecvBuffer(Buffer: TMemory; Len: Integer): Integer;
    begin
      result:=libssh2_channel_read(FChannel, PChar(Buffer), Len);
      SSHCheck(Result);
    end;
    
    function TSSLLibSSH2.WaitingData: Integer;
    begin
      if libssh2_poll_channel_read(FChannel, Result) <> 1 then Result := 0;
    end;
    
    function TSSLLibSSH2.GetSSLVersion: string;
    begin
      Result:=libssh2_version(0);
    end;
    
    initialization
      if libssh2_init(0)=0 then
    				SSLImplementation := TSSLLibSSH2;
    
    finalization
      libssh2_exit;
    
    end.
    

### Класс клиента терминала

Модуль telnetsshclient.pas, представленный ниже, охватывает модуль Synapse tlntsend.pas и абстрагирует вход в систему, отправку команд, получение вывода и выход из системы. 

Если вам нужен только telnet-клиент и вы можете жить без поддержки SSH, закомментируйте {$DEFINE HAS_SSH_SUPPORT} ниже, чтобы вам не нужна была библиотека libssh2. 

Этот модуль было слегка протестировано на сервере Linux ssh/telnet. Дополнительные тесты приветствуются. 
    
    
    unit telnetsshclient;
     
    { Обертка для библиотек Synapse и SSL (libssh2 + libssl
    используется прямо сейчас)
    Загрузите скомпилированную dll Windows, например,
    http://alxdm.dyndns-at-work.com:808/files/windll_libssh2.zip
    Загрузите файлы интерфейса FreePascal:
    http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465
    
    Это устройство позволяет пользователю отправлять команды Telnet или SSH и получать результат
    Спасибо Leonardo Rame
    http://leonardorame.blogspot.com/2010/01/synapse-based-ssh-client.html
    и Ludo Brands.
     
    Написано Reinier Olislagers 2011.
    Изменено для libssh2 Алексеем Сухининым 2012.
    
    Лицензия кода:
    * MIT
    * LGPLv2 или новее (с исключением статического связывания FreePascal)
    * GPLv2 или новее
    по вашему выбору.
    Бесплатное использование разрешено, но, пожалуйста, не предъявляйте иск и не обвиняйте меня.
    
    Использует другие библиотеки/компоненты; могут применяться разные лицензии, которые также могут влиять на комбинированную/скомпилированную работу.
    }
     
    {$mode objfpc}{$H+}
    {$DEFINE HAS_SSH_SUPPORT} //закомментируйте, если требуется только поддержка telnet
    {$DEFINE LIBSSH2}
     
    interface
     
    uses
      Classes, SysUtils,
      tlntsend
      {$IFDEF HAS_SSH_SUPPORT}
      {ssl - или на самом деле ssh - библиотеки, требуемые tlntsend}
        {$IFDEF LIBSSH2}
          ssl_libssh2
        {$ELSE}
          ssl_cryptlib
      {$ENDIF}
      {$ENDIF HAS_SSH_SUPPORT}  ;
     
    type
      TProtocolType = (Telnet, SSH); //Различные способы подключения
      TServerType = (Unix, Windows); //окончания строк, в основном
      { TelnetSSHClient }
     
      { TTelnetSSHClient }
     
      TTelnetSSHClient = class(TTelnetSend)
      protected
        FConnected: boolean;
        FOutputPosition: integer; //Отслеживает позицию в выходном потоке
        FProtocolType: TProtocolType;
        FServerLineEnding: string; //зависит от FServerType
        FServerType: TServerType;
        FWelcomeMessage, FTelnetLoginPrompt, FTelnetPasswordPrompt: string;
        procedure SetPrivateKeyFile(Value: string);
        function GetPrivateKeyFile: string;
        { В зависимости от протокола и типа сервера установите ожидаемое окончание строки на стороне сервера.}
        procedure DetermineLineEnding;
        { Устанавливает порт, если он явно не задан. Использует тип протокола: SSH или telnet}
        procedure DeterminePort;
        function GetSessionLog: string;
        procedure ProtocolTypeChange(Value: TProtocolType);
        function ReceiveData: string; //Может использоваться для получения приветственного сообщения и т.д.
        procedure SendData(Data: string);
        procedure ServerTypeChange(Value: TServerType);
      public
        {Весь вывод, созданный в течение всего сеанса до настоящего момента}
        property AllOutput: string read GetSessionLog;
        {Истинно, если есть подключение к серверу}
        property Connected: boolean read FConnected;
        {Имя или IP-адрес хоста для подключения}
        property HostName: string read FTargetHost write FTargetHost;
        {Порт на хосте, используемый для подключения. Если оставить 0, он будет определяться типом протокола (22 для SSH, 23 для Telnet)}
        property Port: String read FTargetPort write FTargetPort;
        {Расположение файла закрытого ключа.}
        property PrivateKeyFile: string read GetPrivateKeyFile write SetPrivateKeyFile;
        {Telnet login prompt}
        property TelnetLoginPrompt: string read FTelnetLoginPrompt write FTelnetLoginPrompt;
        {Подсказка входа в Telnet}
        property TelnetPasswordPrompt: string read FTelnetPasswordPrompt write FTelnetPasswordPrompt;
        {Имя пользователя, используемое при подключении}
        property UserName: string read FUserName write FUserName;
        {Пароль, используемый при подключении. Используется как кодовая фраза, если используется PrivateKey}
        property Password: string read FPassword write FPassword;
        {Должны ли мы общаться с сервером через Telnet или SSH? По умолчанию SSH.}
        property ProtocolType: TProtocolType read FProtocolType write ProtocolTypeChange;
        {Сервер Windows или Unix/Linux? Влияет на окончания строк. По умолчанию Unix. ПРИМЕЧАНИЕ: непроверено}
        property Servertype: TServerType read FServerType write ServerTypeChange;
        {Первоначальное сообщение отображается при входе в систему}
        property WelcomeMessage: string read FWelcomeMessage;
        {Подключение/вход на сервер. Требуется, чтобы все параметры аутентификации, протокола и имени хоста/порта были правильными
        Возвращает описательный результат. Затем вы можете использовать свойство Connected.}
        function Connect: string;
        {Если подключено, выйти с сервера}
        procedure Disconnect;
        {Отправляет команду на сервер и получает результат}
        function CommandResult(Command: string): string; //Отправьте команду и получите результат
        constructor Create;
        destructor Destroy; override;
      end;
     
    implementation
     
     
    { TelnetSSHClient }
    procedure TTelnetSSHClient.SetPrivateKeyFile(value: string);
    begin
      Sock.SSL.PrivateKeyFile := value;
    end;
    
    function TTelnetSSHClient.GetPrivateKeyFile: string;
    begin
      Result := Sock.SSL.PrivateKeyFile;
    end;
    
    procedure TTelnetSSHClient.DetermineLineEnding;
    begin
      case FProtocolType of
        SSH:
        begin
          if FServerType = Unix then
            FServerLineEnding := #10 //Unix
          else
            FServerLineEnding := #13 + #10; //windows
        end;
        Telnet:
        begin
          if FServerType = Unix then
            FServerLineEnding := #10 //Unix
          else
            FServerLineEnding := #13 + #10; //windows
        end;
        else
          raise Exception.Create('Unknown protocol type');
      end;
    end;
     
    procedure Ttelnetsshclient.DeterminePort;
    begin
      if FTargetPort = '' then
        //Задаем порт по умолчанию для протокола
      begin
        case FProtocolType of
          Telnet: FTargetPort := '23';
          SSH: FTargetPort := '22';
          else
            raise Exception.Create('Unknown protocol type.');
        end;
     
      end;
    end;
     
    procedure TTelnetSSHClient.ServerTypeChange(Value: Tservertype);
    begin
      FServerType := Value;
      DetermineLineEnding;
    end;
     
    function TTelnetSSHClient.Connect: string;
    var
      Received: string;
    begin
      result:='Unknown error while connecting';
      FOutputPosition := 1; //Первый символ в потоке вывода
      FWelcomeMessage := '';
      //Просто чтобы убедиться:
      DetermineLineEnding;
      DeterminePort;
      if FTargetPort='0' then
      begin
       result:='Port may not be 0.';
       exit; //выскакиваем из функции
      end;
      case FProtocolType of
        Telnet:
        begin
          try
            if Login then
              begin
                FConnected := True;
                result:='Connected to telnet server.';
              end
            else
              if Sock.LastError<>0 then raise Exception.Create(Sock.LastErrorDesc);
          except
            on E: Exception do
            begin
              FConnected:=false;
              result:='Error connecting to telnet server '+FTargetHost+':'+
              FTargetPort+' as user ' + FUserName +
              '. Technical details: '+E.Message;
            end;
          end;
        end;
        SSH:
        begin
          {$IFNDEF HAS_SSH_SUPPORT}
          raise Exception.Create(
            'SSH support has not been compiled into the telnetsshclient library.');
          {$ENDIF HAS_SSH_SUPPORT}
          try
            if (PrivateKeyFile <> '') and (FPassword <> '') then
              Sock.SSL.KeyPassword:=FPassword;
            if SSHLogin then
              begin
                FConnected := True;
                result:='Connected to SSH server.';
              end
            else
              begin
                if Sock.LastError<>0 then raise Exception.Create(Sock.LastErrorDesc);
                if Sock.SSL.LastError<0 then raise Exception.Create(Sock.SSL.LastErrorDesc);
              end;
          except
            on E: Exception do
            begin
              FConnected:=false;
              result:='Error connecting to SSH server '+FTargetHost+':'+
              FTargetPort+' as user ' + FUserName +
              '. Technical details: '+E.Message;
            end;
          end;
        end;
        else
          raise Exception.Create('Unknown protocol type');
      end;
      if FConnected = True then
      begin
        FWelcomeMessage := ReceiveData;
        if FProtocolType=Telnet then
        begin
          //К сожалению, нам придется извлекать логин самостоятельно
          //Надеюсь, это применимо ко всем типам серверов.
          if (AnsiPos(AnsiLowerCase(FTelnetLoginPrompt),AnsiLowerCase(FWelcomeMessage))>0) then
          begin
            SendData(UserName);
          end;
          Received:=ReceiveData;
          if (AnsiPos(AnsiLowerCase(FTelnetPasswordPrompt),AnsiLowerCase(Received))>0) then
          begin
            SendData(Password);
          end;
          //Получаем дополнительное приветственное сообщение/подсказку дня
          FWelcomeMessage:=FWelcomeMessage+LineEnding+ReceiveData;
        end;
      end;
    end;
     
    procedure TTelnetSSHClient.Disconnect;
    begin
      Logout;
      FConnected := False;
    end;
     
    function TTelnetSSHClient.ReceiveData: string;
    begin
      Result := '';
      while Sock.CanRead(1000) or (Sock.WaitingData > 0) do
      begin
        Sock.RecvPacket(1000);
        Result := Result + Copy(SessionLog, FOutputPosition,
          Length(SessionLog));
        FOutputPosition := Length(SessionLog) + 1;
      end;
    end;
     
    procedure Ttelnetsshclient.SendData(Data: String);
    begin
      Data := Data + FServerLineEnding; //Может быть Linux, может быть Windows
      Send(Data);
    end;
     
    function TTelnetSSHClient.GetSessionLog: string;
    begin
      // Получает полный вывод на данный момент
      Result := SessionLog;
    end;
     
    procedure TTelnetSSHClient.ProtocolTypeChange(Value: Tprotocoltype);
    begin
      FProtocolType := Value;
      //Автоматическое определение порта и символов конца строки, если необходимо
      DeterminePort;
      DetermineLineEnding;
    end;
     
    function TTelnetSSHClient.CommandResult(Command: string): string;
    begin
      Result := '';
      if FConnected then
      begin
        SendData(Command);
        Result := ReceiveData; //получает слишком много
      end
      else
      begin
        //возбуждаем исключение
        Result := '';
        raise Exception.Create('Can only run command when connected');
      end;
    end;
     
    constructor TTelnetSSHClient.Create;
    begin
      inherited;
      FConnected := False;
      FProtocolType := SSH; //также может быть telnet
      FServerType := Unix; //Probably a safe default.
      FTelnetLoginPrompt := 'login:';
      FTelnetPasswordPrompt := 'password:';
      DetermineLineEnding;
      DeterminePort;
    end;
     
    destructor TTelnetSSHClient.Destroy;
    begin
      if FConnected then
        Disconnect;
      inherited Destroy;
    end;
     
    end.
    

### Пример клиентского кода

Чтобы использовать только что созданный класс TTelnetSSHClient, вы можете использовать этот пример приложения, sshtest.lpr. Обратите внимание, что он должен быть скомпилирован Lazarus, так как ему нужны компоненты LCL для работы с Synapse: 
    
    
    program sshtest;
    
    {Программа тестирования для telnetsshclient
    
    Написано Reinier Olislagers 2011.
    Изменено для libssh2 Алексеем Сухининым 2012.
    
    Лицензия кода:
    * MIT
    * LGPLv2 или новее (с исключением статического связывания FreePascal)
    * GPLv2 или новее
    по вашему выбору.
    Бесплатное использование разрешено, но, пожалуйста, не предъявляйте претензии и не обвиняйте меня.
    
    Использует другие библиотеки/компоненты; могут применяться разные лицензии, которые также могут влиять на комбинированную/скомпилированную работу.
    
    Запустите: sshtest serverIPorhostname [PrivateKeyFile]
    }
    {$mode objfpc}{$H+}
    {$APPTYPE CONSOLE}
    
    uses
      telnetsshclient;
    var
      comm: TTelnetSSHClient;
      Command: string;
    begin
      writeln('Starting.');
      comm:=TTelnetSSHClient.Create;
      comm.HostName:= ParamStr(1); //Первый аргумент в командной строке
      if comm.HostName='' then
      begin
        writeln('Please specify hostname on command line.');
        halt(1);
      end;
    
      comm.PrivateKeyFile := ParamStr(2);
    
      comm.TargetPort:='0'; //автоматическое определение на основе типа протокола
      comm.UserName:='root'; //измените с соответствии с своей ситуацией
      comm.Password:='password'; //измените с соответствии с своей ситуацией
      comm.ProtocolType:=SSH; //Telnet или SSH
      writeln(comm.Connect); //Показать результат подключения
      if comm.Connected then
      begin
        writeln('Server: ' + comm.HostName + ':'+comm.TargetPort+', user: '+comm.UserName);
        writeln('Welcome message:');
        writeln(comm.WelcomeMessage);
        Command:='ls -al';
        writeln('*** Sending ' + Command);
        writeln('*** Begin result****');
        writeln(comm.CommandResult(Command));
        writeln('*** End result****');
        writeln('');
        writeln('');
        Command:='df -h';
        writeln('*** Sending ' + Command);
        writeln('*** Begin result****');
        writeln(comm.CommandResult(Command));
        writeln('*** End result****');
        writeln('');
        writeln('');
        writeln('All output:');
        writeln('*** Begin result****');
        writeln(comm.AllOutput);
        writeln('*** End result****');
        comm.Disconnect;
      end
      else
      begin
        writeln('Connection to ' +
          comm.HostName + ':' +
          comm.TargetPort + ' failed.');
      end;
      comm.Free;
    end.
    

## Интреграция OAuth с v1/Twitter/Plurk

Библиотека OAuth v1, написанная на FPC, которая использует Synapse (и готова для других сетевых библиотек, таких как lnet) доступна [здесь](<https://bitbucket.org/mararosas/fpctwit/src/default/>). FPCTwit также содержит примеры клиентских программ FPC twitter и plurk и Twitter-клиент Lazarus. 

## Другие статьи о Интернете и сетях

  * [Web Development Portal](<../en/Portal_Web_Development.md> "Portal:Web Development")
  * [Networking](<../en/Networking.md> "Networking")
  * [Networking libraries](<../en/Networking_libraries.md> "Networking libraries") \- сравнение различных сетевых библиотек
  * [Brook Framework](<../en/Brook_Framework.md> "Brook Framework") \- Идеальный фреймворк Free Pascal для ваших веб-приложений. Это чистый Паскаль. Вам не нужно оставлять предпочитаемый язык программирования.
  * [Sockets](<../en/Sockets.md> "Sockets") \- Компоненты сокетов TCP/IP
  * [fcl-net](<../en/fcl-net.md> "fcl-net") \- Сетевая библиотека, поставляемая с FPC
  * [lNet](<../en/lNet.md> "lNet") \- Легковесные сетевые компоненты
  * [Synapse](<../en/Synapse.md> "Synapse") \- Последовательный порт и синхронная библиотека TCP/IP
  * [XML Tutorial](<../en/XML_Tutorial.md> "XML Tutorial") -XML, часто использующиеся в сетевых коммуникациях
  * [FPC and Apache Modules](<../en/FPC_and_Apache_Modules.md> "FPC and Apache Modules")
  * [fcl-web](<../en/fcl-web.md> "fcl-web") \- Также известная как fpWeb, это библиотека для разработки веб-приложений, которые можно развернуть как модули cgi, fastcgi или apache.
  * [ Безопасное программирование](<../en/Secure_programming.md> "Secure programming")
  * [Internet Tools](<../en/Internet_Tools.md> "Internet Tools") \- Обертка для HTTP-компонентов Synapse/wininet/Android, упрощающая https и перенаправления, и механизм XPath/XQuery/CSS Selector/JSONiq для обработки загруженных страниц.



## См.также

  * [Download from SourceForge](<../en/Download_from_SourceForge.md> "Download from SourceForge") Пример, который использует Synapse для загрузки с HTTP-сервера, который перенаправляет.
  * [Visual Synapse](<https://sourceforge.net/projects/visualsynapse/>) обертки компонентов для многих частей последовательной и сетевой библиотеки Synapse (TvsComPort, TvsWebClient, TvsSniffer, TvsHTTPServer, TvsFTPServer, TvsAuthentication, TvsVisualDNS, TvsVisualHTTP, TvsVisualDUP, TvsVisualHTTP, TvsVisualDUP, TvsViscksVisualTvs
  * [Компонент TCP/IP на базе Synapse + небольшое демо приложение](<https://forum.lazarus.freepascal.org/index.php/topic,48677.0.html>)



## Внешние ссылки

  * [Официальный сайт](<http://www.ararat.cz/synapse/>)
  * [Официальная документация](<http://synapse.ararat.cz/doc/help/>)
  * [Пользователь malcome создал форк, чтобы быстрее улучшить Synapse](<http://lazarus.freepascal.org/index.php/topic,16032.msg87066.html#msg87066>); этот форк расположен в [Google code site for Synapse4Lazarus](<http://code.google.com/p/synapse4lazarus/>)

---

_Source: [https://wiki.freepascal.org/Synapse/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Synapse/ru)_
