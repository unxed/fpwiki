# Synapse

│ **English (en)** │  **[русский (ru)](<../ru/Synapse.md>)** │

Synapse provides an easy to use serial port and synchronous TCP/IP library. 

## Contents

  * 1 Overview
  * 2 Installation
  * 3 Support and bug reporting
  * 4 SSL/TLS support
    * 4.1 Missing library
  * 5 Web server example
  * 6 QOTD server query example
  * 7 Sending email
  * 8 Downloading files
    * 8.1 From an FTP server
    * 8.2 From an HTTP server
      * 8.2.1 Simple version
      * 8.2.2 Advanced version
      * 8.2.3 Simple version with progress
      * 8.2.4 From an HTTP server by parsing URLs: Sourceforge
    * 8.3 From an HTTPS server
  * 9 SSH/Telnet client sample program
    * 9.1 Requirements
      * 9.1.1 Cryptlib
      * 9.1.2 LibSSH2
    * 9.2 Synapse libssh2 SSL plugin
    * 9.3 Terminal client class
    * 9.4 Example client code
  * 10 OAuth v1/Twitter/Plurk integration
  * 11 Other Web and Networking Articles
  * 12 See also
  * 13 External links



## Overview

Synapse offers [serial](<Hardware_Access.md> "Hardware Access") port and TCP/IP connectivity. It differs from other libraries that you only require to add some Synapse Pascal source code files to your code; no need for installing packages etc. The only exception is that you will need an external crypto library if you want to use encryption such as SSL/TLS/SSH. 

See the documentation on the official site (link below) for more details. 

## Installation

Installation can be as simple as simply copying over all files to your application directory and adding the relevant Synapse units to your _uses_ clause. 

A more elegant and recommended way is compiling the laz_synapse.lpk package so you can use the same units in all your projects. 

Synapse download / Wiki info page: [Download and Wiki](<https://github.com/geby/synapse>)

## Support and bug reporting

The Synapse project has a mailing list where support is given and patches can be submitted. 

Bug reports can also be mailed to the mailing list. 

See the [Synapse support page](<http://www.ararat.cz/synapse/doku.php/support>) and some well presented [on line help](<http://synapse.ararat.cz/doc/help/>)

## SSL/TLS support

You can use OpenSSL, CryptLib, StreamSecII or OpenStreamSecII SSL support with Synapse. By default, no SSL support is used. 

The support is activated by putting the chosen unit name in the uses section in your project. You also have to put the binary library file in your project path (Windows), or install it into your library search path (Linux, macOS, FreeBSD). 

Synapse loads SSL library files in runtime as dynamic libraries. 

  * For detailed information refer to [SSL/TLS Plugin Architecture](<http://www.ararat.cz/synapse/doku.php/public:howto:sslplugin>)
  * Some crypt libraries can be obtained from: <http://synapse.ararat.cz/files/crypt/>



### Missing library

On Linux you need to make sure the required dynamic library is present/installed on your system. In case of cryptlib if the library is not present on the system, an error message appears during linking: 
    
    
    /usr/bin/ld: cannot find -lcl
    

A similar message will be displayed when using other dynamic libraries. 

## Web server example

See the [Webserver example](<Networking.md> "Networking"). 

## QOTD server query example

See the [Quote of the Day server query example](<QOTD.md> "QOTD"). 

## Sending email

Refer to the [Synapse - Email Examples](<Synapse_-_Email_Examples.md> "Synapse - Email Examples") article. 

## Downloading files

### From an FTP server

Given an URL and a (path and) file name, this will download it from an FTP server. It's mostly a wrapper around the Synapse code meant to make downloading easier when handling arbitrary files. If you know exactly what you're going to download where, just a call to Synapse: 
    
    
    FtpGetFile
    

will get you very far. 
    
    
    function DownloadFTP(URL, TargetFile: string): boolean;
    const
      FTPPort=21;
      FTPScheme='ftp://'; //URI scheme name for FTP URLs
    var
      Host: string;
      Port: integer;
      Source: string;
      FoundPos: integer;
    begin
      // Strip out scheme info:
      if LeftStr(URL, length(FTPScheme))=FTPScheme then URL:=Copy(URL, length(FTPScheme)+1, length(URL));
    
      // Crude parsing; could have used URI parsing code in FPC packages...
      FoundPos:=pos('/', URL);
      Host:=LeftStr(URL, FoundPos-1);
      Source:=Copy(URL, FoundPos+1, Length(URL));
    
      //Check for port numbers:
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
    

Example to get list of files in given path 
    
    
    //Use ftpsend unit
    
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
    

### From an HTTP server

Given an URL and a (path and) file name, this will download it from an HTTP server. Note that this code checks the HTTP status code (like 200, 404) to see if the document we got back from the server is the desired file or an error page. 

#### Simple version
    
    
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
    

#### Advanced version
    
    
    ...
    uses httpsend
    ...
    function DownloadHTTP(URL, TargetFile: string): Boolean;
    // Download file; retry if necessary.
    // Could use Synapse HttpGetBinary, but that doesn't deal
    // with result codes (i.e. it happily downloads a 404 error document)
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
          // Try to get the file
          HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
          while (HTTPGetResult = False) and (RetryAttempt < MaxRetries) do
          begin
            Sleep(500 * RetryAttempt);
            HTTPSender.Clear;
            HTTPGetResult := HTTPSender.HTTPMethod('GET', URL);
            RetryAttempt := RetryAttempt + 1;
          end;
          // If we have an answer from the server, check if the file
          // was sent to us.
          case HTTPSender.Resultcode of
            100..299:
              begin
                HTTPSender.Document.SaveToFile(TargetFile);
                Result := True;
              end; //informational, success
            300..399: Result := False; // redirection. Not implemented, but could be.
            400..499: Result := False; // client error; 404 not found etc
            500..599: Result := False; // internal server error
            else Result := False; // unknown code
          end;
        except
          // We don't care for the reason for this error; the download failed.
          Result := False;
        end;
      finally
        HTTPSender.Free;
      end;
    end;
    

#### Simple version with progress

The following example shows how to get progress information from the HTTP download, as well as the file size. The file size is retrieved from the header information. 
    
    
    unit uhttpdownloader;
    
    // Essential to change this. The default is {$mode objfpc}{$H+} and does not work.
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
    

What are we doing here? 

First of all we look into the headers to get the file size. We have to wait and check if the header is there. The first events do not contain the Content-Length: information. 

Once found, we extract that information. There are several events popping up here, which you can react to. But we only check for THookSocketReason.HR_ReadCount in that example. 

"HR_ReadCount" provides us with the information how many bytes where read since the last event. 

The progress is then reported to the UI: 
    
    
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
    

So, the final main unit will be: 
    
    
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
    

Reference: <https://andydunkel.net/2015/09/09/lazarus_synapse_progress/>

#### From an HTTP server by parsing URLs: Sourceforge

Please see [Download from SourceForge](<Download_from_SourceForge.md> "Download from SourceForge") for an example of downloading from sourceforge.net. 

### From an HTTPS server

This is similar to downloading from an HTTP server. In addition you need to [activate SSL/TLS support](<Synapse.md> "Synapse") and obtain the binary file(s) for the needed library. Then you can use the same DownloadHTTP function for downloading a file from a URL starting with **https://**. 

## SSH/Telnet client sample program

Below you will find a unit that allows you to use telnet/SSH client functionality that uses the synapse tlntsend.pas unit. An example program shows how to use this. A different, simpler way is illustrated by Leonardo Ramé at [[1]](<http://leonardorame.blogspot.com/2010/01/synapse-based-ssh-client.html>). His example cannot use telnet and only sends one command, though. 

### Requirements

Apart from the Synapse sources (of which you only need a couple), if you want to use SSH functionality, you will need an encryption library that Synapse uses. If you only use Telnet, you don't need this. 

There are 2 choices: 

  * Cryptlib library. Advantage: stable. Apparently able to use private keys but these are in some format that is not widely supported.
  * LibSSH2 library. Pascal bindings still in development, but you can use a file with your private key (in OpenSSH format) to authenticate.



#### Cryptlib

  * On Windows, download a binary version of the cryptlib DLL (CL32.DLL) and put it in your source directory. If you compile to a different directory or distribute your program, you will need to distribute the DLL as well.
  * On Linux and OSX, install cryptlib via your package manager/other means. When distributing your application, mark cryptlib as a requirement in your .deb/.rpm/whatever package.



You will also need the bindings (cryptlib.pas), present in the source distribution of cryptlib. 

The versions of the cryptlib binary and the bindings must match. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** It seems that cryptlib is not suitable to connect to linux machines, though AIX works. Use SSH2 instead.

#### LibSSH2

  * On Windows, download a binary version of the libssh2 DLL (LIBSSH2.DLL) and put it in your source directory. If you compile to a different directory or distribute your program, you will need to distribute the DLL as well.
  * On Linux and macOS, install libssh2 via your package manager/other means. When distributing your application: 
    * Linux: mark libssh2 as a requirement in your .deb/.rpm/whatever package.
    * macOS: include libssh2 in your [Application Bundle](<Application_Bundle.md> "Application Bundle")'s Resources directory.



You will also need ssl_libssh2.pas (see below) and the bindings: (libssh2.pas, see [this forum post](<http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465>)). The libssh2 binary and the bindings must match. 

### Synapse libssh2 SSL plugin

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** plugin is not completed.
    
    
    {
      ssl_libssh2.pas version 0.2
    
      SSH2 support (draft) plugin for Synapse Library (http://www.ararat.cz/synapse) by LibSSH2 (http://libssh2.org)
      Requires: libssh2 pascal interface - http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465 and
      libssh2.dll with OpenSSL.
    
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
      {:@abstract(class implementing CryptLib SSL/SSH plugin.)
       Instance of this class will be created for each @link(TTCPBlockSocket).
       You not need to create instance of this class, all is done by Synapse itself!}
      TSSLLibSSH2 = class(TCustomSSL)
      protected
        FSession: PLIBSSH2_SESSION;
        FChannel: PLIBSSH2_CHANNEL;
        function SSHCheck(Value: integer): Boolean;
        function DeInit: Boolean;
      public
        {:See @inherited}
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
            SSHCheck(-1); // get error
            if FLastError = 0 then
            begin
              FLastError := -999; // unknown error
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
    

### Terminal client class

The telnetsshclient.pas unit below wraps around the Synapse tlntsend.pas unit and abstracts logging in, sending commands and receiving output and logging out. 

If you only need a telnet client and can live without SSH support, comment out {$DEFINE HAS_SSH_SUPPORT} below so you don't need to have the libssh2 dll. 

This unit has been lightly tested on a Linux ssh/telnet server. Additional tests welcome. 
    
    
    unit telnetsshclient;
     
    { Wrapper around Synapse libraries and SSL library (libssh2+libssl
    is used right now)
    Download compiled Windows dll from e.g.
    http://alxdm.dyndns-at-work.com:808/files/windll_libssh2.zip
    Download FreePascal interface files:
    http://www.lazarus.freepascal.org/index.php/topic,15935.msg86465.html#msg86465
    
    This unit allows the user to send Telnet or SSH commands and get the output
    Thanks to Leonardo Rame
    http://leonardorame.blogspot.com/2010/01/synapse-based-ssh-client.html
    and Ludo Brands.
     
    Written by Reinier Olislagers 2011.
    Modified for libssh2 by Alexey Suhinin 2012.
    
    License of code:
    * MIT
    * LGPLv2 or later (with FreePascal static linking exception)
    * GPLv2 or later
    according to your choice.
    Free use allowed but please don't sue or blame me.
     
    Uses other libraries/components; different licenses may apply that also can influence the combined/compiled work.
    }
     
    {$mode objfpc}{$H+}
    {$DEFINE HAS_SSH_SUPPORT} //comment out if only telnet support required
    {$DEFINE LIBSSH2}
     
    interface
     
    uses
      Classes, SysUtils,
      tlntsend
      {$IFDEF HAS_SSH_SUPPORT}
      {ssl - or actually ssh - libs required by tlntsend}
        {$IFDEF LIBSSH2}
          ssl_libssh2
        {$ELSE}
          ssl_cryptlib
      {$ENDIF}
      {$ENDIF HAS_SSH_SUPPORT}  ;
     
    type
      TProtocolType = (Telnet, SSH); //Different means of connecting
      TServerType = (Unix, Windows); //line endings, mostly
      { TelnetSSHClient }
     
      { TTelnetSSHClient }
     
      TTelnetSSHClient = class(TTelnetSend)
      protected
        FConnected: boolean;
        FOutputPosition: integer; //Keeps track of position in output stream
        FProtocolType: TProtocolType;
        FServerLineEnding: string; //depends on FServerType
        FServerType: TServerType;
        FWelcomeMessage, FTelnetLoginPrompt, FTelnetPasswordPrompt: string;
        procedure SetPrivateKeyFile(Value: string);
        function GetPrivateKeyFile: string;
        { Based on protocol and servertype, set expected serverside line ending}
        procedure DetermineLineEnding;
        { Sets port if no explicit port set. Uses protocol type: SSH or telnet}
        procedure DeterminePort;
        function GetSessionLog: string;
        procedure ProtocolTypeChange(Value: TProtocolType);
        function ReceiveData: string; //Can be used to get welcome message etc.
        procedure SendData(Data: string);
        procedure ServerTypeChange(Value: TServerType);
      public
        {All output generated during the entire session up to now}
        property AllOutput: string read GetSessionLog;
        {True if connected to server}
        property Connected: boolean read FConnected;
        {Name or IP address of host to connect to}
        property HostName: string read FTargetHost write FTargetHost;
        {Port on host used for connection. If left as 0, it will be determined by protocol type (22 for SSH, 23 for Telnet}
        property Port: String read FTargetPort write FTargetPort;
        {Location of private key file.}
        property PrivateKeyFile: string read GetPrivateKeyFile write SetPrivateKeyFile;
        {Telnet login prompt}
        property TelnetLoginPrompt: string read FTelnetLoginPrompt write FTelnetLoginPrompt;
        {Telnet password prompt}
        property TelnetPasswordPrompt: string read FTelnetPasswordPrompt write FTelnetPasswordPrompt;
        {Username used when connecting}
        property UserName: string read FUserName write FUserName;
        {Password used when connecting. Used as passphrase if PrivateKey is used}
        property Password: string read FPassword write FPassword;
        {Should we talk Telnet or SSH to the server? Defaults to SSH.}
        property ProtocolType: TProtocolType read FProtocolType write ProtocolTypeChange;
        {Windows or Unix/Linux server? Has effect on line endings. Defaults to Unix. NOTE: untested}
        property Servertype: TServerType read FServerType write ServerTypeChange;
        {Initial message displayed on logon}
        property WelcomeMessage: string read FWelcomeMessage;
        {Connect/logon to server. Requires that all authentication, protocol and hostname/port options are correct
        Returns descriptive result. You can then use the Connected property.}
        function Connect: string;
        {If connected, logoff from server}
        procedure Disconnect;
        {Send command to server and receive result}
        function CommandResult(Command: string): string; //Send command and get results
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
        //Set default port for protocol
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
      FOutputPosition := 1; //First character in output stream
      FWelcomeMessage := '';
      //Just to make sure:
      DetermineLineEnding;
      DeterminePort;
      if FTargetPort='0' then
      begin
       result:='Port may not be 0.';
       exit; //jump out of function
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
          //Unfortunately, we'll have to extract login ourselves
          //Hope it applies to all server types.
          if (AnsiPos(AnsiLowerCase(FTelnetLoginPrompt),AnsiLowerCase(FWelcomeMessage))>0) then
          begin
            SendData(UserName);
          end;
          Received:=ReceiveData;
          if (AnsiPos(AnsiLowerCase(FTelnetPasswordPrompt),AnsiLowerCase(Received))>0) then
          begin
            SendData(Password);
          end;
          //Receive additional welcome message/message of the day
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
      Data := Data + FServerLineEnding; //Could be linux, could be Windows
      Send(Data);
    end;
     
    function TTelnetSSHClient.GetSessionLog: string;
    begin
      // Gets complete output up to now
      Result := SessionLog;
    end;
     
    procedure TTelnetSSHClient.ProtocolTypeChange(Value: Tprotocoltype);
    begin
      FProtocolType := Value;
      //Auto-determine port and line ending, if necessary
      DeterminePort;
      DetermineLineEnding;
    end;
     
    function TTelnetSSHClient.CommandResult(Command: string): string;
    begin
      Result := '';
      if FConnected then
      begin
        SendData(Command);
        Result := ReceiveData; //gets too much
      end
      else
      begin
        //raise exception
        Result := '';
        raise Exception.Create('Can only run command when connected');
      end;
    end;
     
    constructor TTelnetSSHClient.Create;
    begin
      inherited;
      FConnected := False;
      FProtocolType := SSH; //Could be telnet, too
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
    

### Example client code

To use the TTelnetSSHClient class we just made, you can use this example application, sshtest.lpr. Note that it needs to be compiled by Lazarus as it needs the LCL components to work with Synapse: 
    
    
    program sshtest;
    
    {Test program for telnetsshclient
    
    Written by Reinier Olislagers 2011.
    Modified for libssh2 by Alexey Suhinin 2012.
    
    License of code:
    * MIT
    * LGPLv2 or later (with FreePascal static linking exception)
    * GPLv2 or later
    according to your choice.
    Free use allowed but please don't sue or blame me.
    
    Uses other libraries/components; different licenses may apply that also can influence the combined/compiled work.
    
    Run: sshtest <serverIPorhostname> [PrivateKeyFile]
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
      comm.HostName:= ParamStr(1); //First argument on command line
      if comm.HostName='' then
      begin
        writeln('Please specify hostname on command line.');
        halt(1);
      end;
    
      comm.PrivateKeyFile := ParamStr(2);
    
      comm.TargetPort:='0'; //auto determine based on protocoltype
      comm.UserName:='root'; //change to your situation
      comm.Password:='password'; //change to your situation
      comm.ProtocolType:=SSH; //Telnet or SSH
      writeln(comm.Connect); //Show result of connection
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
    

## OAuth v1/Twitter/Plurk integration

An OAuth v1 library written in FPC that uses Synapse (and is ready for other network libraries like lNet) is available in [fpctwit](<fpctwit.md> "fpctwit"). FPCtwit also contains FPC Twitter and Plurk example client programs and a Lazarus Twitter client. 

## Other Web and Networking Articles

  * [Web Development Portal](<Portal_Web_Development.md> "Portal:Web Development")
  * [Networking](<Networking.md> "Networking")
  * [Networking libraries](<Networking_libraries.md> "Networking libraries") \- comparison of various networking libraries
  * [Brook Framework](<Brook_Framework.md> "Brook Framework") \- The perfect Free Pascal framework for your web applications. It's pure Pascal. You don't need to leave your preferred programming language.
  * [Sockets](<Sockets.md> "Sockets") \- TCP/IP Sockets components
  * [fcl-net](<fcl-net.md> "fcl-net") \- Networking library supplied with FPC
  * [lNet](<lNet.md> "lNet") \- Lightweight Networking Components
  * [XML Tutorial](<XML_Tutorial.md> "XML Tutorial") \- XML is often utilized on network communications
  * [FPC and Apache Modules](<FPC_and_Apache_Modules.md> "FPC and Apache Modules")
  * [fcl-web](<fcl-web.md> "fcl-web") \- Also known as fpWeb, this is a library to develop web applications which can be deployed as cgi, fastcgi or apache modules.
  * [ Secure Programming](<Secure_programming.md> "Secure programming")
  * [Internet Tools](<Internet_Tools.md> "Internet Tools") \- A wrapper around Synapse/wininet/Android's http components simplifying https and redirections, and a XPath/XQuery/CSS Selector/JSONiq engine to process the downloaded pages



## See also

  * [Download from SourceForge](<Download_from_SourceForge.md> "Download from SourceForge") Example that uses Synapse to download from an HTTP server that redirects.
  * [Visual Synapse](<https://sourceforge.net/projects/visualsynapse/>) component wrappers for many parts of Synapse serial and networking library (TvsComPort, TvsWebClient, TvsSniffer, TvsHTTPServer, TvsFTPServer, TvsAuthentication, TvsVisualDNS, TvsVisualHTTP, TvsVisualDUP, TvsVisualTCP, TvsVisualICMP, TvsSocksProxyInfo, TvsIPHelper, TvsSendMail and TvsSynPing).
  * [TCP/IP component based on Synapse + a small demo application](<https://forum.lazarus.freepascal.org/index.php/topic,48677.0.html>)



## External links

  * [Official site](<http://www.ararat.cz/synapse/>)
  * [Official documentation](<http://synapse.ararat.cz/doc/help/>)
  * [Download and Wiki](<https://github.com/geby/synapse/wiki>)

---

_Source: [https://wiki.freepascal.org/Synapse](https://web.archive.org/web/20250428214836/https://wiki.freepascal.org/Synapse)_
