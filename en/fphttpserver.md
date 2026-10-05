# fphttpserver

## Contents

  * 1 fphttpserver Unit
    * 1.1 Intro
    * 1.2 Properties
    * 1.3 Usage
    * 1.4 Support for HTTPS
  * 2 fphttapp & custhttpapp Units
    * 2.1 Intro
    * 2.2 Properties
    * 2.3 Usage



## fphttpserver Unit

### Intro

The _fphttpserver_ unit contains a standalone HTTP server component: _TFPHTTPServer_. It has the following features: 

  1. Specify any port you want.
  2. SSL capable. It generates a self-signed certificate if needed.
  3. Various threading modes: none, fully threaded or using a thread pool.
  4. Upgrade mechanism.
  5. Event-driven.
  6. Keeps connections (HTTP 1.1).



You can drop this component on a form and handle requests. Note that this component does not do any routing of requests, for that, use the _TFPHTTPApplication_ class (see below). 

### Properties

The following properties are available: 

Active (boolean)
    Set to true to start the server, set to false to stop the server.
Port (integer)
    Port number to listen on. Default 80.
QueueSize (integer)
    Queue size for the Accept() call.
Threaded (boolean)
    Is the server threaded or not?
    Deprecated in 3.3.1: use ThreadMode instead.
ThreadMode (tmNone, tmThread, tmThreadPool)
    Choose how to use threads to process requests. Note that keeping connections open and no threading will not work very well.
AcceptIdleTimeout (integer)
    Accepting new connections happens in a loop.
    This timeout specifies the number of milliseconds to wait before Accept times out, at which point the OnAcceptIdle is called.
    If zero, the accept call does not time out.
KeepConnections (boolean)
    Allow to keep connections (HTTP 1.1).
KeepConnectionTimeout (integer)
    Number of milliseconds to wait before closing an open (persistent) connection.
OnAllowConnect (event)
    An event handler to decide whether you want to accept a connection or not. This is called before the request is parsed.
OnRequest (event)
    An event handler to actually handle requests.
    The signature of this method is the same as for all other fcl-web request handlers: you get a request and response object.
OnRequestError (event)
    Triggered when an exception occurs during a request.
OnAcceptIdle (event)
    Triggered when the Accept loop goes idle.

### Usage

  1. Create the component in code (or drop on a form/datamodule in the Lazarus IDE).
  2. Set the port number.
  3. Create an _OnRequest_ handler to handle requests.
  4. At runtime, set the _Active_ property to _True_. This statement will not return till the server is stopped.



### Support for HTTPS

Using HTTPS is just setting a property on _TFPHTTPServer_ : 
    
    
     UseSSL := True;
    

And include one of the units that registers an SSL handler: _opensslsockets_ or _gnutlssockets_. 

You need to set your certificate details as well: 
    
    
      fServer.UseSSL := fUseSSL;
      if fUseSSL then
      begin
        fServer.CertificateData.KeyPassword := fCertificatePassword;
        fServer.CertificateData.HostName := fCertificateHostName;
        fServer.CertificateData.Certificate.FileName := fCertificateFileName;
        fServer.CertificateData.PrivateKey.FileName := fCertificatePrivateKey;
      end;
    

If you do not set the certificate details, the HTTP server component will generate a temporary certificate, which may lead to a warning being displayed in the browser. 

This works as of FPC 3.2.2. 

## fphttapp & custhttpapp Units

### Intro

The _TFPHTTPServer_ does not route requests. It only sends them through the 'OnRequest' event handler. 

To integrate with the rest of fcl-web, instead use the _THTTPApplication_ or _TFPCustomHTTPApplication_ classes. These classes are descendents of TCustomApplication and handle everything from server setup to routing requests. 

The _fphttpapp_ unit contains simply an initialized version of the _THTTPApplication_ class. 

### Properties

In addition to the TCustomWebApplication (common ancestor for all kinds of web application: CGI, FastCGI, Apache Module etc.) the following properties are available: (they mostly map to the properties of the _TFPHttpServer_ class) 

Address (string)
    Server address
Port (Word)
    TCP/IP port to listen on
QueueSize (Word)
    Max connections on queue (for Listen call)
OnAllowConnect (Event)
    Called when deciding whether to accept a connection.
Threaded (Boolean)
    Use a thread to handle a connection ?
LookupHostNames (Boolean)
    Should addresses be matched to hostnames ? (expensive)
OnAcceptIdle (Event)
    Event handler called when going Idle while waiting for a connection
AcceptIdleTimeout (integer)
    If >0, when no new connection appeared after timeout, OnAcceptIdle is called.
UseSSL (Boolean)
    Use SSL or not
HostName (String)
    Hostname to use when using SSL
CertificateData (TCertificateData)
    Access to certificate data

### Usage

The following demonstrates the use of the _TFPHTTPApplication_ class: 
    
    
    program webserver;
     
    {$mode objfpc}{$H+}
     
    uses
      {$ifdef UNIX}
      cthreads, cmem,
      {$endif} 
      fphttpapp, httpdefs, httproute;
     
    procedure route1(aReq: TRequest; aResp: TResponse);
    begin
      aResp.Content:='<html><body><h1>Route 1 The Default</h1></body></html>';
    end;
     
    procedure route2(aReq: TRequest; aResp: TResponse);
    begin
      aResp.Content:='<html><body><h1>Route 2</h1></body></html>';
    end;
     
    begin
      HTTPRouter.RegisterRoute('/', @route1);
      HTTPRouter.RegisterRoute('/2', @route2);
      Application.Port := 1999;
      Application.Threaded := true;
      Application.Initialize;
      Application.Run;
    end.

---

_Source: [https://wiki.freepascal.org/fphttpserver](https://web.archive.org/web/20240718110936/https://wiki.freepascal.org/fphttpserver)_
