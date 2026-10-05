# macOS NSURLConnection

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Creating a connection
    * 2.1 Retrieving data synchronously
      * 2.1.1 Code example
        * 2.1.1.1 Compilation note
    * 2.2 Retrieving data asynchronously using a completion handler block
      * 2.2.1 Code example
        * 2.2.1.1 Compilation note
    * 2.3 Retrieving data asynchronously using a custom delegate object
      * 2.3.1 Code example
        * 2.3.1.1 Compilation note
  * 3 See also
  * 4 External links



## Overview

An NSURLConnection object lets you load the contents of a URL by providing a URL request object. The interface for NSURLConnection is sparse, providing only the controls to start and cancel asynchronous loads of a URL request. You perform most of your configuration on the URL request object itself. 

NSURLConnection is available from macOS 10.2 onwards. As with many things Apple, be warned that many of the methods in this API were deprecated in 10.11 (El Capitan). Nonetheless, to paraphrase WWDC 2015: "Yeah, NSURLConnection is deprecated in 10.11, but it's not going away and will still work, but new features will be added to [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession")". 

## Creating a connection

The NSURLConnection class supports three ways of retrieving the content of a URL: synchronously, asynchronously using a completion handler block, and asynchronously using a custom delegate object. 

### Retrieving data synchronously

The NSURLConnection class provides support for retrieving the contents of a resource represented by an NSURLRequest object in a synchronous manner using the class method _sendSynchronousRequest:returningResponse:error:_. Using this method is not generally recommended, because it has severe limitations: 

  * Unless you are writing a command-line tool, you should add additional code to ensure that the request does not run on your application's main thread because it will block any user interaction until the data has been received.


  * Minimal support is provided for requests that require authentication.


  * There is no means of modifying the default behavior of response caching or accepting server redirects.



If the request succeeds, the contents of the request are returned as an NSData object and an NSURLResponse object for the request is returned by reference. If NSURLConnection is unable to retrieve the URL, the method returns nil and any available NSError instance by reference in the appropriate parameter. 

If the request requires authentication to make the connection, valid credentials must already be available in the NSURLCredentialStorage object or must be provided as part of the requested URL. If the credentials are not available or fail to authenticate, the URL loading system responds by sending the NSURLProtocol subclass handling the connection a _continueWithoutCredentialForAuthenticationChallenge:_ message. When a synchronous connection attempt encounters a server redirect, the redirect is always honored. Likewise, the response data is stored in the cache according to the default support provided by the protocol implementation. 

#### Code example
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework foundation}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      CocoaAll, CocoaUtils;
    
      { TForm1 }
    
    type
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}      
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      URL: NSURL;
      urlData : NSData;
      urlRequest : NSUrlRequest;
      urlResponse: NSURLResponse;
      urlConnection: NSURLConnection;
      error: NSError;
      urlBody: NSString;
    begin
    
      // create URL
      URL := NSURL.URLWithString(NSSTR(PAnsiChar('https://sentinel.sentry.org/')));
      if(Url = Nil) then
        ShowMessage('NSURL.URLWithString failed!');
    
      // create NSURLRequest
      urlRequest := NSURLRequest.requestWithURL(URL);
    
      // create NSURLConnection
      urlConnection := NSURLConnection.alloc.init;
    
      // make synchronous URL request
      urlData := urlConnection.sendSynchronousRequest_returningResponse_error(
          urlRequest,
          @urlResponse,
          @error
        );
    
      // housekeeping
      urlConnection.release;
    
      // check for failure 
      if(urlData = Nil) then
        begin
          //NSLog(NSStr('Error in urlConnection: %@'), error);
          ShowMessage('Error retrieving ' + NSSTringToString(URL.description)
            + LineEnding + NSStringToString(error.localizedDescription));
        end
      // otherwise success 
      else
        begin
          // show web page meta data
          ShowMessage(NSStringToString(urlResponse.description));
    
          // show web page content as hexadecimal
          ShowMessage(NSStringToString(urlData.description));
    
          // show web page content as HTML text
          urlBody := NSString.alloc.initWithData_encoding(urlData,NSUTF8StringEncoding);
          ShowMessage(NSStringToString(urlBody));
    
          // housekeeping
          urlBody.release;
        end;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlconnection/>). 

##### Compilation note

To compile the above code successfully you are going to have to add the missing _initWithData_encoding_ function to the _NSString_ class by editing `/usr/local/share/fpcsrc/fpc-[3.0.4|3.2.0|3.3.1]/packages/cocoaint/src/foundation/NSString.inc` to add the missing (highlighted) function as follows: 
    
    
    --- NSString.inc        (revision 45778)
    +++ NSString.inc        (working copy)
    @@ -105,6 +105,7 @@
         function characterAtIndex (index: NSUInteger): unichar; message 'characterAtIndex:';
         function init: instancetype; message 'init'; { NS_DESIGNATED_INITIALIZER }
         function initWithCoder (aDecoder: NSCoder): instancetype; message 'initWithCoder:'; { NS_DESIGNATED_INITIALIZER }
    +    function initWithData_encoding(data: NSData; encoding: NSStringEncoding) : instancetype; message 'initWithData:encoding:';
     
         { Adopted protocols }
         function copyWithZone (zone: NSZonePtr): id; message 'copyWithZone:';
    

and then recompile the FPC source. This has been tested with FPC 3.0.4, FPC 3.2.0 and FPC 3.3.1. Note that for FPC 3.0.4 you need to replace "instancetype" in the highlighted line with "id" 

I use the following script (for FPC 3.3.1; substitute your FPC version numbers as appropriate) to recompile FPC: 
    
    
    #!/bin/sh
    cd /usr/local/share/fpcsrc/fpc-3.3.1/
    make clean all FPC=/usr/local/lib/fpc/3.3.1/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
    make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

  * Note 1: The highlighted line(s) above should be all on one line.
  * Note 2: You will almost certainly have to use _sudo_ to execute that script successfully or face permissions problems.



### Retrieving data asynchronously using a completion handler block

The NSURLConnection class provides support for retrieving the contents of a resource represented by an NSURLRequest object in an asynchronous manner and calling a block when results are returned or when an error or timeout occurs. To do this, you call the class method _sendAsynchronousRequest:queue:completionHandler:_ , providing: 

  * the request object,
  * a completion handler block, and
  * an NSOperation queue on which that block should run.



When the request completes or an error occurs, the URL loading system calls that block with the result data or error information. If the request succeeds, the contents of the request are passed to the callback handler block as an NSData object and an NSURLResponse object for the request. If NSURLConnection is unable to retrieve the URL, an NSError object is passed as the third parameter. 

This method has two significant limitations: 

  * There is minimal support for requests that require authentication. If the request requires authentication to make the connection, valid credentials must already be available in the NSURLCredentialStorage object or must be provided as part of the requested URL. If the credentials are not available or fail to authenticate, the URL loading system responds by sending the NSURLProtocol subclass handling the connection a _continueWithoutCredentialForAuthenticationChallenge:_ message.


  * There is no means of modifying the default behaviour of response caching or accepting server redirects. When a connection attempt encounters a server redirect, the redirect is always honored. Likewise, the response data is stored in the cache according to the default support provided by the protocol implementation.



#### Code example

Note: Due to the use of the cblocks feature, you must use at least version 3.2.0 of the Free Pascal Compiler to compile it successfully. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$modeswitch cblocks}
    {$linkframework foundation}
    
    interface
    
    uses
      SysUtils, Forms, Controls, Dialogs, StdCtrls,
      CocoaAll, CocoaUtils;
    
    { TForm1 }
    
    type
      // setup cBlock for completion handler
      tblock = reference to procedure(response: NSURLResponse; data: NSData; connectionError: NSError); cdecl; cblock;
    
      // redefine version from packages/cocoaint/src/foundation/NSURLConnection.inc
      NSURLConnectionQueuedLoading = objccategory external (NSURLConnection)
        class procedure sendAsynchronousRequest_queue_completionHandler(request: NSURLRequest; queue: NSOperationQueue; completionHandler: tBlock); message 'sendAsynchronousRequest:queue:completionHandler:';
      end;
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    //{$DEFINE DEBUG}
    
    // Completion handler: Executed after URL has been retrieved or retrieval fails
    procedure myCompletionHandler(response: NSURLResponse; data: NSData; connectionError: NSError);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('Completion handler called'));
      {$ENDIF}
    
      // if no error
      if((data.Length > 0) and (connectionError = Nil))
        then
           begin
             ShowMessage('Data desc: ' + LineEnding + NSStringToString(data.description));
             ShowMessage('Response: ' +  LineEnding + NSStringToString(response.description));
             ShowMessage('HTML: ' +  LineEnding + NSStringToString(NSString.alloc.initWithData_encoding(data,NSUTF8StringEncoding)));
           end
      // o/w return error
      else
        begin
          ShowMessage('Data length: ' + IntToStr(data.length));
          ShowMessage('Error description: ' + LineEnding +  NSStringToString(connectionError.description));
          ShowMessage('Error retrieving: ' + NSStringToString(connectionError.userInfo.valueForKey(NSErrorFailingUrlStringKey))
            + LineEnding + LineEnding + 'Reason: ' + NSStringToString(connectionError.localizedDescription));
        end;
    end;
    
    // Retrieve web page
    procedure TForm1.Button1Click(Sender: TObject);
    var
      URL: NSURL;
      urlRequest : NSUrlRequest;
      urlConnection: NSURLConnection;
    begin
    
      // create NSURL
      URL := NSURL.URLWithString(NSSTR(PAnsiChar('https://sentinel.sentry.org/')));
      if(Url = Nil) then
          ShowMessage('NSURL.URLWithString failed!');
    
      // create NSURLRequest
      urlRequest := NSURLRequest.requestWithURL(URL);
    
      // create NSURLConnection
      urlConnection := NSURLConnection.alloc.init;
    
      // make asynchronous URL connection request
      urlConnection.sendAsynchronousRequest_queue_completionHandler
        (urlRequest, NSoperationQueue.mainQueue, @myCompletionHandler);
    
      // housekeeping
      urlConnection.release;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlconnection1/>). 

##### Compilation note

See the previous Compilation note above. 

### Retrieving data asynchronously using a custom delegate object

The NSURLConnection class provides support for retrieving the contents of a resource represented by an NSURLRequest object in an asynchronous manner using a custom delegate object that implements at least the following delegate methods: 

  * connection:didReceiveResponse:,
  * connection:didReceiveData:,
  * connection:didFailWithError:, and
  * connectionDidFinishLoading:.



I have also implemented the _connectionWillCacheResponse_ delegate method so that you can disable caching of the response which is handy when testing/debugging web page retrieval code. 

The supported delegate methods are defined in the _NSURLConnectionDelegate_ , _NSURLConnectionDownloadDelegate_ , and _NSURLConnectionDataDelegate_ protocols. 

#### Code example
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework foundation}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls,
      CocoaAll, CocoaUtils;
    
    type
      { TForm1 }
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
      { TMyDelegate }
      TMyDelegate = objcclass(NSObject)
      private
    
      public
        procedure connection_didReceiveResponse (connection: NSURLConnection; response: NSURLResponse);
          message 'connection:didReceiveResponse:';
        procedure connection_didReceiveData (connection: NSURLConnection; data: NSData);
          message 'connection:didReceiveData:';
        function connection_willCacheResponse (connection: NSURLConnection; cachedResponse: NSCachedURLResponse): NSCachedURLResponse;
          message 'connection:willCacheResponse:';
        procedure connectionDidFinishLoading (connection: NSURLConnection);
          message 'connectionDidFinishLoading:';
        procedure connection_didFailWithError (connection: NSURLConnection; error: NSError);
          message 'connection:didFailWithError:';
      end;
    
    //{$DEFINE DEBUG}  // Uncomment to log to Console app
    
    var
      Form1: TForm1;
      responseData: NSMutableData;
      connDelegate: TMyDelegate;
    
    implementation
    
    {$R *.lfm}
    
    procedure TMyDelegate.connection_didReceiveResponse (connection: NSURLConnection; response: NSURLResponse);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('didReceiveResponse'));
      {$ENDIF}
    
      // A response has been received, so initialize the response variable
      // so that we can append data to it in the didReceiveData method
      // Note: as this method is called each time there is a redirect,
      // reinitializing it also serves to clear it.
      responseData := NSMutableData.alloc.init;
    end;
    
    procedure TMyDelegate.connection_didReceiveData (connection: NSURLConnection; data: NSData);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('appendData'));
      {$ENDIF}
    
      // Append the new data to the response variable
      responseData.appendData(data);
    end;
    
    function TMyDelegate.connection_willCacheResponse (connection: NSURLConnection; cachedResponse: NSCachedURLResponse): NSCachedURLResponse;
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('willCacheResponse'));
      {$ENDIF}
    
      // Return nil to disable caching of the response for this connection
      Result := Nil;
    end;
    
    procedure TMyDelegate.connectionDidFinishLoading (connection: NSURLConnection);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('didFinishLoading'));
      {$ENDIF}
    
      // The request is completed and data received, so show it
      ShowMessage(NSStringtoString(NSString.alloc.initWithData_encoding(responseData,NSUTF8StringEncoding)));
    end;
    
    procedure TMyDelegate.connection_didFailWithError (connection: NSURLConnection; error: NSError);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('didFailWithError'));
      {$ENDIF}
    
      // The request failed
      ShowMessage('Request for ' + NSStringToString(error.userInfo.valueForKey(NSErrorFailingUrlStringKey)) + ' failed!' + LineEnding + LineEnding
        + 'Reason: ' + NSStringToString(error.localizedDescription));
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      URL: NSURL;
      urlRequest : NSUrlRequest;
      urlConnection: NSURLConnection;
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('create url'));
      {$ENDIF}
    
      // create URL
      URL := NSURL.URLWithString(NSSTR(PAnsiChar('https://sentinel.sentry.org/')));
      if(Url = Nil) then
        ShowMessage('NSURL.URLWithString failed!');
    
      {$IFDEF DEBUG}
      NSLog(NSStr('create request'));
      {$ENDIF}
    
      // create NSURLRequest
      urlRequest := NSURLRequest.requestWithURL(URL);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('create connection delegate'));
      {$ENDIF}
    
      // create connection delegate
      connDelegate := TMyDelegate.alloc.init;
    
      {$IFDEF DEBUG}
      NSLog(NSStr('create connection and fire request'));
      {$ENDIF}
    
      // create NSURLConnection and make request
      urlConnection := NSURLConnection.alloc.initWithRequest_delegate(urlRequest, connDelegate);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('cleanup'));
      {$ENDIF}
    
      // housekeeping cleanup
      urlConnection.release;
      connDelegate.release;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlconnection2/>). 

##### Compilation note

See the previous Compilation note above. 

## See also

  * [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession") \- the replacement for NSURLConnection.
  * [fpwebview](<fpwebview.md> "fpwebview") \- Free Pascal binding for webview, a cross-platform library that links Cocoa/WebKit on macOS.



## External links

  * [Apple: NSString](<https://developer.apple.com/documentation/foundation/nsstring>).
  * [Apple: NSURLConnection](<https://developer.apple.com/documentation/foundation/nsurlconnection>).
  * [Apple: NSURLRequest](<https://developer.apple.com/documentation/foundation/nsurlrequest>).
  * [Apple: NSData](<https://developer.apple.com/documentation/foundation/nsdata>).
  * [Apple: NSURL](<https://developer.apple.com/documentation/foundation/nsurl>).
  * [Apple: NSURLSession](<https://developer.apple.com/documentation/foundation/nsurlsession>).

---

_Source: [https://wiki.freepascal.org/macOS_NSURLConnection](https://web.archive.org/web/20250219024335/https://wiki.freepascal.org/macOS_NSURLConnection)_
