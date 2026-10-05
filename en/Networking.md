# Networking

│ **English (en)** │  **[русский (ru)](<../ru/Networking.md>)** │

This page contains both tutorials/code and information with regard to network programming with Lazarus as well as plain FPC. 

## Contents

  * 1 TCP/IP Protocol
    * 1.1 Basic IP socket programming and a bit more
    * 1.2 CGI/FastCGI - REST, chat, blog, web page etc.
    * 1.3 SSH/Telnet client, sending emails, downloading files, OAuthv1 examples
    * 1.4 Webserver example
    * 1.5 Download HTTP page body, web server upload using POST, get external IP address
    * 1.6 LazWebsockets
  * 2 Web Services
    * 2.1 RSS example
    * 2.2 Web Service Toolkit for FPC & Lazarus
    * 2.3 Using Google Translate
  * 3 See also
    * 3.1 macOS
    * 3.2 Windows
  * 4 External links



## TCP/IP Protocol

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Because there are multiple libraries (Synapse, lnet, fphttpclient, Indy,...) that provide networking functionality for FPC/Lazarus, many examples can be written for multiple libraries. Therefore you may see the same examples multiple times - for different libraries. Have a look at the [Brook for Free Pascal](<Brook_for_Free_Pascal.md> "Brook for Free Pascal") or [fpctwit](<fpctwit.md> "fpctwit") frameworks for examples on how to build frameworks that can use multiple network libraries

### Basic IP socket programming and a bit more

For very basic applications and first steps exploring TCP socket communication the [fcl-net](<fcl-net.md> "fcl-net") library may be the best approach. It is included in the default installation. Code samples, which also serve as documentation, can be found in the ...\fpc\\...\source\packages\fcl-net\examples subdirectory of your local Lazarus installation. Amongst others there are basic TCP socket server and client implementations, and a DNS resolver. All samples are command-line only, and should work on all platforms which provide a socket-style networking interface. 

The next advanced library, and the most basic one providing GUI support, is [lNet](<lNet.md> "lNet") (note: the first letter is a lowercase l, for "lightweight"). If you are using the [Lazarus IDE](<Lazarus_Tutorial.md> "Lazarus Tutorial"), you can install the latest version via the Online Package Manager (OPM). Otherwise, and for access to the latest version as well as to documentations and sample code files, see the component GIT repository hosted on [GitHub](<https://github.com/almindor/lnet>). A very good place to start exploring is the tcpudp demo contained in the ...\examples\visual directory. It shows a lightweight example of how to program an event-driven IP socket client and server, both blended together in one GUI program (use the "Host" button to switch between server and client mode). It also lets you play with some useful options like switching the transport protocols between TCP, UDP, IPV4 and IPV6 and turning on/off basic support for SSL. 

Both fcl-net and lNet encapsulate the platform dependent socket libraries to provide a cross-platform interface for all platforms supported by FPC/Lazarus. The code samples contain very basic applications like Telnet, FTP, SMTP and even some very basic HTTP servers. They do not cover advanced protocols requiring authentication and/or encryption beyond basic SSL, nor do they have support for the many subprotocols and features required to build full fledged web programs. This is where more extensives libraries like fcl-web, Synapse or Brook Framework shine - see below. 

### CGI/FastCGI - REST, chat, blog, web page etc.

These functionalities can be used with [fcl-web](<fcl-web.md> "fcl-web"). They are also built into the Brook framework. Please have a look at [this page](<http://silvioprog.github.io/brookframework>). <https://github.com/silvioprog/brookframework>

### SSH/Telnet client, sending emails, downloading files, OAuthv1 examples

See the [Synapse](<Synapse.md> "Synapse") page. 

### Webserver example

Given below is an example http server written with [Synapse](<Synapse.md> "Synapse") and tested in Win XP and macOS, after changing the Synapse source to use a fixed constant $20000 as MSG_NOSIGNAL, because this constant isn't present in the sockets unit in macOS. _Note: please send your bug reports to the Synapse project - they are quite responsive - so everybody profits from improvements. To do: check if this modification is still needed for current (Oct 2012) Synapse_
    
    
    {
      The Micro Pascal WebServer
    
      This is a very simple example webserver implemented with the Synapse library.
    
      It works with blocking sockets and a single thread, so it
      can only handle one request at a given time.
    
      It will write the headers that it receives from the browser
      to the standard output.
    
      It serves a fixed webpage for the / URI
      For any other URI it will return 404 not found
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
        ASocket.SendString('HTTP/1.0 404' + CRLF);
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
          ConnectionSocket.CloseSocket;
        end;
      until false;
    
      ListenerSocket.Free;
      ConnectionSocket.Free;
    end.
    

### Download HTTP page body, web server upload using POST, get external IP address

These examples (using the FPC/Lazarus built in fphttpclient) can be found at [fphttpclient#Examples](<fphttpclient.md> "fphttpclient")

### LazWebsockets

A WebSocket is a persistent connection between a client and server. WebSockets provide a bidirectional, full-duplex communications channel that operates over HTTP through a single TCP/IP socket connection. At its core, the WebSocket protocol facilitates message passing between a client and server. [LazWebsockets](<https://github.com/Warfley/LazWebsockets>) provides a small Websocket server and client implementation written for the FPC and Lazarus. It is based on the FCL ssockets unit and therefore independent of any additional dependencies. It can be easily built using only the FPC without Lazarus. There is a chat server and chat client example demo. 

## Web Services

According to the [W3C](<http://www.w3.org/>) a Web service is a software system designed to support interoperable machine-to-machine interaction over a network. It has an interface that is described in a machine-processable format such as WSDL. Other systems interact with the Web service in a manner prescribed by its interface using messages, which may be enclosed in a SOAP envelope, or follow a REST approach. These messages are typically conveyed using HTTP, and are normally comprised of XML in conjunction with other Web-related standards. Software applications written in various programming languages and running on various platforms can use web services to exchange data over computer networks like the Internet in a manner similar to inter-process communication on a single computer. This interoperability (e.g., between Windows and Linux applications) is due to the use of open standards. OASIS and the W3C are the primary committees responsible for the architecture and standardization of web services. To improve interoperability between web service implementations, the WS-I organisation has been developing a series of profiles to further define the standards involved. 

### RSS example

For an RSS example, see this article [RSS Unit](<RSS.md> "RSS"). 

### Web Service Toolkit for FPC & Lazarus

[Web Service Toolkit](<Web_Service_Toolkit.md> "Web Service Toolkit") is a web services package for FPC and Lazarus. 

### Using Google Translate

For an example of using the Google translation service, see the article [Using Google Translate](<Using_Google_Translate.md> "Using Google Translate") which employs the [fphttpclient](<fphttpclient.md> "fphttpclient") component from the [fcl-web](<fcl-web.md> "fcl-web") package. 

## See also

  * [Web Development Portal](<Portal_Web_Development.md> "Portal:Web Development")
  * [Brook for Free Pascal](<Brook_for_Free_Pascal.md> "Brook for Free Pascal") \- The perfect Free Pascal framework for your web applications. It's pure Pascal. You don't need to leave your preferred programming language.
  * [fcl-net](<fcl-net.md> "fcl-net") \- Networking library supplied with FPC
  * [fcl-web](<fcl-web.md> "fcl-web") \- Also known as fpWeb, this is a library to develop web applications which can be deployed as cgi, fastcgi or apache modules.
  * [fpbrowser](<fpbrowser.md> "fpbrowser") \- web browser written in FPC + Lazarus.
  * [Indy_with_Lazarus](<Indy_with_Lazarus.md> "Indy with Lazarus") \- Networking Components； How to install
  * [Internet Tools](<Internet_Tools.md> "Internet Tools") \- A wrapper around Synapse/WinInet/Android's HTTP components simplifying HTTPS and redirections, and a XPath/XQuery/CSS Selector/JSONiq engine to process the downloaded pages
  * [FPC and Apache Modules](<FPC_and_Apache_Modules.md> "FPC and Apache Modules")
  * [lNet](<lNet.md> "lNet") \- Lightweight Networking Components
  * [Networking libraries](<Networking_libraries.md> "Networking libraries") \- comparison of various networking libraries
  * [OpenURL](<OpenURL.md> "OpenURL") \- opens a URL with the default/registered/preferred web browser as specified by the operating system.
  * [ Secure Programming](<Secure_programming.md> "Secure programming")
  * [Sockets](<Sockets.md> "Sockets") \- TCP/IP Sockets components
  * [Synapse](<Synapse.md> "Synapse") \- Serial port and synchronous TCP/IP Library
  * [XML Tutorial](<XML_Tutorial.md> "XML Tutorial") \- XML is often utilized on network communications



### macOS

  * [macOS NSURLConnection](<macOS_NSURLConnection.md> "macOS NSURLConnection") This API lets you load the contents of a URL via HTTP and HTTPS by providing a URL request object.
  * [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession") This class and related classes provide an API for downloading content via HTTP and HTTPS.
  * [macOS open command](<macOS_Open_Sesame.md> "macOS Open Sesame") Opens a URL in the default web browser.



### Windows

  * [WinInet](<Windows_Programming_Tips.md> "Windows Programming Tips") (Windows Internet) API enables your application to interact with FTP and HTTP protocols to access Internet resources.



## External links

  * [Fano Framework](<https://fanoframework.github.io>) \- Web application framework written in Free Pascal.
  * [FastPlaz](<http://www.fastplaz.com>) \- Fast Web Framework for pascal. More features like theme/templating, simple Model, session, mailer, etc.

---

_Source: [https://wiki.freepascal.org/Networking](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/Networking)_
