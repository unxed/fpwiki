# fphttpclient

## Contents

  * 1 Overview
  * 2 HTTPS (TLS/SSL)
  * 3 Examples
    * 3.1 Get body of a web page via HTTP protocol
    * 3.2 Download a file via HTTP protocol
    * 3.3 Get file size from URL
    * 3.4 Upload a file using POST
    * 3.5 Form data and encoding
    * 3.6 Posting JSON
    * 3.7 Get external IP address
    * 3.8 Translate text using Google Translate
  * 4 See also
  * 5 External links



## Overview

fphttpclient is supplied with FPC as part of the [fcl-web](<fcl-web.md> "fcl-web") package, and can be used by itself as well. 

## HTTPS (TLS/SSL)

Since April 2014, the trunk/development fphttpclient supports SSL/TLS connections using the OpenSSL library (will ship with version 2.8.0 and above). This requires the OpenSSL .so/.dll/.dylib library/libraries to be installed (e.g. present in your application or system directory on Windows). 

  * If you do not use client side certificates, just specifying the proper port (e.g. 443 for https) is enough to enable TLS/SSL as long as you have [OpenSSL libraries](<https://wiki.openssl.org/index.php/Binaries>) installed (or e.g. in the application directory)
  * If you want to use e.g. a client side certificate, do something like this:


    
    
    uses ...ssockets, sslsockets..
    // Callback for setting up SSL client certificate
    procedure TSSLHelper.SSLClientCertSetup(Sender: TObject; const UseSSL: Boolean;
      out AHandler: TSocketHandler);
    begin
      AHandler := nil;
      if UseSSL and (FClientCertificate <> '') then
      begin
        // Only set up client certificate if needed.
        // If not, let normal fphttpclient flow create
        // required socket handler
        AHandler := TSSLSocketHandler.Create;
        // Example: use your own client certificate when communicating with the server:
        (AHandler as TSSLSocketHandler).Certificate.FileName := FClientCertificate;
      end;
    end;
    
    //... and in your TFPHTTPClient creation:
    myclient := TFPHTTPClient.Create(nil);
    if FClientCertificate <> '' then
      myclient.OnGetSocketHandler := @SSLClientCertSetup;
    

## Examples

Examples are included in your FPC directory: packages/fcl-web/examples/ 

Apart from those, please see below: 

### Get body of a web page via HTTP protocol
    
    
    {$mode delphi}
    uses fphttpclient;
    
    Var
      URL, S : String;
    begin
      URL := ParamStr(1);
      with TFPHttpClient.Create(Nil) do
        try
          S := Get(URL);
        finally
          Free;
        end;
      Writeln('Content: ', S);
    end.
    

If you want to write even fewer lines of code, in FPC 2.7.1 you can use the class method: 
    
    
      S := TFPCustomHTTPClient.SimpleGet('http://a_site/a_page');
    

### Download a file via HTTP protocol

Let's show you the simplest example using HTTP only. This example retrieves just an html page and writes it to the screen. 

It uses one of the class **Get** methods of TfpHttpClient. You don't need to create and free the class. There are several overloads for e.g. TStrings or TStream (file) as well: 
    
    
    program dl_fphttp_a;
    {$mode delphi}{$ifdef windows}{$apptype console}{$endif}
    uses 
      fphttpclient;
    begin  
      writeln(TFPHttpClient.SimpleGet('http://example.com')); 
    end.
    

That's all! For simple purposes this will suffice. You can even use it for HTTPS, which needs just the inclusion of one additional unit: 
    
    
    program dl_fphttp_b;
    
    {$mode delphi}{$ifdef windows}{$apptype console}{$endif}
    
    uses 
      fphttpclient,
      opensslsockets;
    
    begin  
      writeln(TFPHttpClient.SimpleGet('https://freepascal.org')); 
    end.
    

This really is all there is to do in simple scenarios, usually if you have enough control. 

But there are some caveats with this code. The foremost one being that this code does not allow for redirects. Let me demo that: 
    
    
    program dl_fphttp_redirect_error;
    {$mode delphi}{$ifdef windows}{$apptype console}{$endif}
    uses 
      sysutils, fphttpclient, opensslsockets;
    begin  
      try
        writeln(TFPHttpClient.SimpleGet('https://google.com')); 
      except on E:EHttpClient do
        writeln(e.message) else raise;
      end;
    end.
    

Because the Google URL uses redirects, there is an exception: **"Unexpected response status code: 301"**. In such a case we need more control, which I will show you in the next example: 
    
    
    program dl_fphttp_c;
    
    {$mode delphi}{$ifdef windows}{$apptype console}{$endif}
    
    uses
      classes, 
      fphttpclient,
      openssl, { This implements the procedure InitSSLInterface }
      opensslsockets;
    
    var
      Client: TFPHttpClient;
    
    begin
    
      { SSL initialization has to be done by hand here }
      InitSSLInterface;
    
      Client := TFPHttpClient.Create(nil);
      try
        { Allow redirections }
        Client.AllowRedirect := true;
        writeln(Client.Get('https://google.com/')); 
      finally
        Client.Free;
      end;
    end.
    

You see this requires slightly more code, but it is still a very small program. Well, now we go to the last example, which downloads any file and saves it to disk. You can use it as a template for your own code, it demonstrates almost everything you need. 
    
    
    program dl_fphttp_d;
    
    {$mode delphi}{$ifdef windows}{$apptype console}{$endif}
    
    uses
      sysutils, 
      classes, 
      fphttpclient, 
      openssl,
      opensslsockets;
    
    const
      Filename = 'testdownload.txt';
    
    var
      Client: TFPHttpClient;
      FS: TStream;
      SL: TStringList;
    
    begin
    
      { SSL initialization has to be done by hand here }
      InitSSLInterface;
    
      Client := TFPHttpClient.Create(nil);
      FS := TFileStream.Create(Filename,fmCreate or fmOpenWrite);
      try
        try
          { Allow redirections }
          Client.AllowRedirect := true;
          Client.Get('https://google.com/',FS); 
        except
          on E: EHttpClient do
            writeln(E.Message)
          else
            raise;
        end;
      finally
        FS.Free;
        Client.Free;
      end;
      
      { Test our file }
      if FileExists(Filename) then
      try
        SL := TStringList.Create;
        SL.LoadFromFile(Filename);
        writeln(SL.Text);
      finally
        SL.Free;
      end;      
    end.
    

### Get file size from URL

If you will use HTTPS links, you will need to use unit "opensslsockets". The following function assumes that server supports HEAD requests, only rare servers these days do not support it. 
    
    
    uses
      opensslsockets, fphttpclient;
    
    function GetRemoteFileSize(const URL: string): Int64;
    var
      SL: TStringList;
      S: string;
    begin
      SL:= TStringList.Create;
      try
        TFPHTTPClient.Head(URL, SL);
        //SL.NameValueSeparator := ':'; // Already done in TFPCustomHTTPClient.Create()
        S:= SL.Values['Content-Length'];
        Result:= StrToInt64Def(S, -1);
      finally
        FreeAndNil(SL);
      end;
    end;
    

### Upload a file using POST

Use TFPHTTPClient.FileFormPost(), which allows the caller to specify the path to a local file to be uploaded and the content of the _name_ field (see <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition#directives> for more information): 
    
    
    {$mode delphi}
    
    uses fphttpclient, classes;
    
    const
      FieldName = 'myfield'; { This is the name of the HTTP field in the form you are filling }
      FileName = 'test.txt'; { This file must exist on your computer }
       
    Var
      Respo: TStringStream;
      S : String;
    
    begin
      With TFPHttpClient.Create(Nil) do
        try
          Respo := TStringStream.Create('');
          FileFormPost('http://example.com',
                       FieldName,
                       FileName,
                       Respo);
          S := Respo.DataString;
          WriteLn('Response: ');
          WriteLn(S);
          Respo.Destroy;
        finally
          Free;
        end;
    end.
    

There is a method StreamFormPost which will upload a stream instead of directly reading a file. 

### Form data and encoding
    
    
     FormPost(const URL: string; FormData: TStrings; const Response: TStream);
    
    
    
     FormPost(const URL, FormData: string; const Response: TStream);
    

If you pass FormData as [TStrings](<TStrings.md> "TStrings") the FormPost procedure performs _EncodeURLElement_ but if you pass FormData as simple [string](<string.md> "string") to FormPost, then _EncodeURLElement_ is not performed. 

There is no difference in the behaviour if the data in FormData does not need to be encoded, but there is different behaviour for strings which include characters which are not allowed (eg '@', '=', etc). 

### Posting JSON

JSON is a popular way for machine to machine talk, and requires that appropriate headers be set. This example sends a short JSON string and gets back, in 'Response' there is some more JSON. 
    
    
    var
      Client: TFPHttpClient;
      Response: TStringStream;
      Params: string = '{"title": "Some note", "content": "Awesome stuff"}';
    begin
      Client := TFPHttpClient.Create(nil);
      Client.AddHeader('User-Agent', 'Mozilla/5.0 (compatible; fpweb)');
      Client.AddHeader('Content-Type', 'application/json; charset=UTF-8');
      Client.AddHeader('Accept', 'application/json');
      Client.AllowRedirect := true;
      Client.UserName := USER_STRING;
      Client.Password := PASSW_STRING;
      Client.RequestBody := TRawByteStringStream.Create(Params);
      Response := TStringStream.Create('');
      try
        try
          Client.Post(TheURL, Response);
          Writeln('Response Code: ' + IntToStr(Client.ResponseStatusCode)); // better be 200
        except on E: Exception do
          Writeln('Something bad happened: ' + E.Message);
        end;
      finally
        Client.RequestBody.Free;
        Client.Free;
        Response.Free;
      end;
    

Note that using Client.FormPost() will overwrite the content-type header with "application/x-www-form-urlencoded" causing a lot of confusion with a server that is expecting JSON. 

### Get external IP address

If your computer is connected to the internet via a LAN (cabled or wireless), the IP address of your network card most probably is not your external IP address. 

You can retrieve your external IP address from e.g. your router or an external site. The code below tries to get it from an external site (thanks to JoStudio on the forum for the inspiration: [[1]](<http://lazarus.freepascal.org/index.php/topic,17506.msg110917.html#msg110917>)): 
    
    
    {$mode objfpc}{$H+}
    
    uses
      Classes, SysUtils, fphttpclient, RegExpr;
    
    function GetExternalIPAddress: string;
    var
      HTTPClient: TFPHTTPClient;
      IPRegex: TRegExpr;
      RawData: string;
    begin
      try
        HTTPClient := TFPHTTPClient.Create(nil);
        IPRegex := TRegExpr.Create;
        try
          //returns something like:
          {
    <html><head><title>Current IP Check</title></head><body>Current IP Address: 44.151.191.44</body></html>        
          }
          RawData := HTTPClient.Get('http://checkip.dyndns.org');
          // adjust for expected output; we just capture the first IP address now:
          IPRegex.Expression := '\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b';
                           //or '\b(?:\d{1,3}\.){3}\d{1,3}\b'
          if IPRegex.Exec(RawData) then
            Result := IPRegex.Match[0]
          else
            Result := 'Got invalid results getting external IP address. Details:'
              + LineEnding + RawData;
        except
          on E: Exception do
          begin
            Result := 'Error retrieving external IP address: ' + E.Message;
          end;
        end;
      finally
        HTTPClient.Free;
        IPRegex.Free;
      end;
    end;
    
    begin
      Writeln('External IP address:', GetExternalIPAddress);
    end.
    

### Translate text using Google Translate

See [Using Google Translate](<Using_Google_Translate.md> "Using Google Translate")

## See also

  * [macOS NSURLConnection](<macOS_NSURLConnection.md> "macOS NSURLConnection")
  * [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession")
  * [macOS OpenSSL, LibreSSL, Secure Transport, Network Framework](<macOS_Programming_Tips.md> "macOS Programming Tips")
  * [Windows native WinInet API](<Windows_Programming_Tips.md> "Windows Programming Tips")



## External links

  * [fpHTTPClient Tutorial](<http://lazplanet.blogspot.com/2019/03/how-to-get-contents-of-url-in-2-ways.html>).

---

_Source: [https://wiki.freepascal.org/fphttpclient](https://web.archive.org/web/20240907051914/https://wiki.freepascal.org/fphttpclient)_
