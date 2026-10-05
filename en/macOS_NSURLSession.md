# macOS NSURLSession

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
    * 1.1 Types of sessions
    * 1.2 Types of Tasks
    * 1.3 Asynchronicity
    * 1.4 Protocol support
    * 1.5 Creating a session
    * 1.6 Caching policies
  * 2 Retrieving data using system-provided delegates
    * 2.1 Example code
      * 2.1.1 Compilation note
  * 3 Retrieving data using a custom delegate
    * 3.1 Example code
      * 3.1.1 Compilation note
  * 4 Downloading files
    * 4.1 Example code
  * 5 Downloading files with graphical user interface feedback
    * 5.1 Example code
  * 6 See also
  * 7 External links



## Overview

The NSURLSession class and related classes provide an API for downloading content via HTTP and HTTPS. The downloading is performed asynchronously, so your application can remain responsive and handle incoming data or errors as they arrive. This class replaces the [macOS NSURLConnection](<macOS_NSURLConnection.md> "macOS NSURLConnection") class which was deprecated by Apple in macOS 10.11 (El Capitan) in 2015. NSURLSession is available from macOS 10.9 (Mavericks) onwards. 

The NSURLSession API is highly asynchronous. If you use the default, system-provided delegate, you must provide a completion handler block that returns data to your application when a transfer finishes successfully or with an error. Alternatively, if you provide your own custom delegate objects, the task objects call those delegates’ methods with data as it is received from the server (or, for file downloads, when the transfer is complete). The NSURLSession API provides status and progress properties, in addition to delivering this information to delegates. It supports cancelling, resuming, and suspending tasks, and it provides the ability to resume suspended, cancelled, or failed downloads where they left off. 

The behaviour of the tasks in a session depends on three things: 

  1. the type of session (determined by the type of configuration object used to create it),
  2. the type of task, and
  3. whether the application was in the foreground when the task was created.



### Types of sessions

The NSURLSession API supports three types of sessions, as determined by the type of configuration object used to create the session: 

  1. Default sessions behave similarly to other Foundation methods for downloading URLs. They use a persistent disk-based cache and store credentials in the user’s keychain. A default session configuration object results in a session that behaves like an NSURLConnection object in its standard configuration. Altering a session configuration obtained through the defaultSessionConfiguration method does not change the default session configuration of which it is a copy.
  2. Ephemeral sessions do not store any data to disk; all caches, credential stores, and so on are kept in RAM and tied to the session. Thus, when your application invalidates the session, they are purged automatically. Ephemeral sessions are therefore ideal if you need to implement private browsing.
  3. Background sessions are similar to default sessions, except that a separate process handles all data transfers. Background sessions have some additional limitations: 
     1. The session must provide a delegate for uploads and downloads.
     2. Only HTTP and HTTPS protocols are supported.
     3. Only upload and download tasks are supported (no data tasks).
     4. Redirects are always followed.
     5. If the background transfer is initiated while the app is in the background, the configuration object’s discretionary property is treated as being true.
     6. Background configurations cannot be reused (ie you cannot use two background session configurations using the same configuration identifier).



### Types of Tasks

An NSURLSession supports four types of tasks: 

  * data tasks,
  * download tasks,
  * upload tasks, and
  * webSocket tasks.



Data tasks send and receive data using NSData objects. Data tasks are intended for short, often interactive requests to a server. Data tasks can return data to your application one piece at a time after each piece of data is received, or all at once through a completion handler. As data tasks do not store the data to a file, they are not supported in background sessions. 

Download tasks retrieve data in the form of a file, and support background downloads. 

Upload tasks send data (usually in the form of a file), and support background uploads. 

Web socket tasks exchange messages over TCP and TLS, using the WebSocket protocol defined in [RFC 6455](<https://tools.ietf.org/html/rfc6455>). 

### Asynchronicity

The NSURLSession API is highly asynchronous. It returns data to your application in one of two ways, depending on the methods you call: 

  * by calling a completion handler block when a transfer finishes successfully or with an error; or
  * by calling methods on the session’s delegate as data arrives and when the transfer is complete.



In addition to delivering this information to delegates, the NSURLSession provides status and progress properties that you can query if you need to make programmatic decisions based on the current state of the task (with the caveat that its state can change at any time). 

### Protocol support

An NSURLSession natively supports the data, file, ftp, http, and https URL schemes, with transparent support for proxy servers and SOCKS gateways, as configured in the user’s system preferences. It supports HTTP/1.1 and HTTP/2 protocols. HTTP/2, described in [RFC 7540](<https://tools.ietf.org/html/rfc7540>), requires a server that supports Application-Layer Protocol Negotiation (ALPN). macOS 10.11 (El Capitan) and later use App Transport Security (ATS) for all HTTP connections made with NSURLSession. ATS requires that HTTP connections use HTTPS ([RFC 2818](<https://tools.ietf.org/html/rfc2818>)). 

### Creating a session

The NSURLSession API provides a wide range of configuration options: 

  * Private storage support for caches, cookies, credentials, and protocols in a way that is specific to a single session.
  * Authentication, tied to a specific request (task) or group of requests (session).
  * File uploads and downloads by URL, which encourages separation of the data (the file’s contents) from the metadata (the URL and settings).
  * Configuration of the maximum number of connections per host.
  * Per-resource timeouts that are triggered if an entire resource cannot be downloaded in a certain amount of time.
  * Minimum and maximum TLS version support Custom proxy dictionaries.
  * Control over cookie policies.
  * Control over HTTP pipelining behaviour.



When you instantiate a session object, you specify the following: 

  * A configuration object that governs the behaviour of that session and the tasks within it
  * Optionally, a delegate object to process incoming data as it is received and handle other events specific to the session and the tasks within it, such as server authentication, determining whether a resource load request should be converted into a download, and so on.
  * If you do not provide a delegate, the NSURLSession object uses a system-provided delegate. In this way, you can readily use NSURLSession in place of existing code that uses the _sendAsynchronousRequest:queue:completionHandler:_ convenience method on NSURLSession.



As these settings are contained in a separate configuration object, you can reuse commonly used settings in a configuration object in other sessions provided they are not background sessions. When you create a session, the session copies the configuration object and, as a result, modifications of the configuration object only affect new sessions and not existing sessions. However, you cannot reuse background session configuration objects because the behaviour of two background session objects sharing the same identifier is undefined. 

### Caching policies

Network caching reduces the number of requests that need to be made to the server, and improve the experience of using an application offline or under slow network conditions. An NSURLRequest instance specifies how the local cache is used by setting the cache policy to one of the **NSURLRequestCachePolicy** values. The default cache policy for an NSURLRequest instance is **NSURLRequestUseProtocolCachePolicy** which is protocol specific and is defined as being the best conforming policy for the protocol. 

There are a number of different NSURLRequest caching policies from which to choose: 

  * **NSURLRequestUseProtocolCachePolicy** Use the caching logic defined in the protocol implementation, if any, for a particular URL load request.
  * **NSURLRequestReloadIgnoringLocalCacheData** The URL load should be loaded only from the originating source.
  * **NSURLRequestReloadIgnoringLocalAndRemoteCacheData** Ignore local cache data, and instruct proxies and other intermediates to disregard their caches so far as the protocol allows.
  * **NSURLRequestReturnCacheDataElseLoad** Use existing cache data, regardless or age or expiration date, loading from originating source only if there is no cached data.
  * **NSURLRequestReturnCacheDataDontLoad** Use existing cache data, regardless or age or expiration date, and fail if no cached data is available.
  * **NSURLRequestReloadRevalidatingCacheData** Use cache data if the origin source can validate it; otherwise, load from the origin.



The default cache policy is **NSURLRequestUseProtocolCachePolicy** and is almost always the best choice rather than implementing your own caching or changing the cache policy of the request. When a server responds to an HTTP request, the response includes information about how long and under what conditions the response will be valid. However, for debugging I always prefer **NSURLRequestReloadIgnoringLocalCacheData** ro ensure the data isn not coming from the local cache and that a request is actually made whether it is fulfilled by the origin server or an intermediate proxy. 

## Retrieving data using system-provided delegates

The NSURLSession object does not need to have a delegate. If no delegate is assigned, a system-provided delegate is used, and you must provide a completion handler callback to obtain the data. 

This simple demonstration shows how to use NSURLSession and related classes to download and display a web page using NSURLSession as a drop-in replacement for the [_NSURLConnection sendAsynchronousRequest:queue:completionHandler:_](<macOS_NSURLConnection.md> "macOS NSURLConnection") method. Using this method, you need to provide only two pieces of code in your application: 

  1. code to create a configuration object and a session based on that object; and
  2. code to implement a completion handler routine to do something with the data after it has been fully received.



### Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$modeswitch cblocks}
    
    interface
    
    uses
      Forms,      // for the main form
      Dialogs,    // for ShowMessage
      StdCtrls,   // for the button
      SysUtils,   // for IntToStr()
      CocoaAll,   // for NSData and other Cocoa types
      CocoaUtils; // for NSStringString
    
    type
      // setup cBlock for completion handler
      tblock = reference to procedure(data: NSData; response: NSURLResponse; connectionError: NSError); cdecl; cblock;
    
      // redefine version from packages/cocoaint/src/foundation/NSURLSession.inc
      NSURLSession = objcclass external (NSObject)
      public
        class function sessionWithConfiguration(configuration: NSURLSessionConfiguration): NSURLSession; message 'sessionWithConfiguration:';
      end;
    
      NSURLSessionAsynchronousConvenience = objccategory external (NSURLSession)
        function dataTaskWithURL_completionHandler(url: NSURL; completionHandler: tBlock): NSURLSessionDataTask; message 'dataTaskWithURL:completionHandler:';
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      myCache: NSURLcache;
      webData: String;
      webResponse: String;
      webHTML: String;
      webStatusCode: Integer;
      webError: String;
      webErrorReason: String;
      didFinish: PRTLEvent;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    {$DEFINE DEBUG}
    
    // Completion handler: Executed after URL has been retrieved or retrieval fails
    procedure myCompletionHandler(data: NSData; response: NSURLResponse; connectionError: NSError);
    var
      httpResponse: NSHTTPURLResponse;
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('Completion handler called'));
    
      if (NSThread.currentThread.isMainThread) then
         NSLog(NSStr('In main thread--completion handler'))
      else
         NSLog(NSStr('Not in main thread--completion handler'));
      {$ENDIF}
    
      // if no error
      if((data.Length > 0) and (connectionError = Nil)) then
        begin
          {$IFDEF DEBUG}
          NSLog(NSStr('Data desciption: %@'),data.description);
          NSLog(NSStr('Response description: %@'),response.description);
          NSLog(NSStr('Data: %@'),NSString.alloc.initWithData_encoding(data,NSASCIIStringEncoding).autorelease);
          {$ENDIF}
          webData     :=  NSStringToString(data.description);
          {$IFDEF DEBUG}
          NSLog(NSStr('Web data' + webData));
          {$ENDIF}
          // The NSHTTPURLResponse class is a subclass of NSURLResponse
          // so we can cast an NSURLResponse as an NSHTTPURLResponse
          httpResponse := NSHTTPURLResponse(response);
          {$IFDEF DEBUG}
          // Extract status code from response header
          NSLog(NSStr('HTTP status code: %@'),NSStr(IntToStr(httpResponse.statusCode)));
          {$ENDIF}
          webStatusCode :=  httpResponse.statusCode;
          webResponse   :=  NSStringToString(response.description);
          webHTML       :=  NSSTringToString(NSString.alloc.initWithData_encoding(data,NSASCIIStringEncoding));
        end
      // o/w return error
      else
        begin
          {$IFDEF DEBUG}
          NSLog(NSStr('Error %@'), connectionError.userInfo);
          {$ENDIF}
          webError := 'Error description: ' + LineEnding +  NSStringToString(connectionError.description);
          webErrorReason := 'Error retrieving: ' + NSStringToString(connectionError.userInfo.valueForKey(NSErrorFailingUrlStringKey))
            + LineEnding + LineEnding + 'Reason: ' + NSStringToString(connectionError.localizedDescription);
        end;
    
      // notify main thread that completion handler thread has finished
      RTLEventSetEvent(didFinish);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('leaving handler'));
      {$ENDIF}
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      urlSessionConfig: NSURLSessionConfiguration = Nil;
      cachePath: NSString = Nil;
      urlSession: NSURLSession = Nil;
      URL: NSURL = Nil;
    begin
      // create event to synchronise completion handler background thread
      // with the GUI main thread
      didFinish := RTLEventCreate;
    
      // create default url session config
      urlSessionConfig := NSURLSessionConfiguration.defaultSessionConfiguration;
    
      // configure caching behavior for the default session
      cachePath := NSTemporaryDirectory.stringByAppendingPathComponent(NSStr('/nsurlsessiondemo.cache'));
    
      {$IFDEF DEBUG}
      NSLog(NSStr('Cache path: %@'), cachePath);
      {$ENDIF}
    
      myCache := NSURLCache.alloc.initWithMemoryCapacity_diskCapacity_diskPath(16384, 268435456, cachePath);
      urlSessionConfig.setURLCache(myCache);
    
      // set cache policy
      {$IFDEF DEBUG}
      urlSessionConfig.setRequestCachePolicy(NSURLRequestReloadIgnoringLocalCacheData);
      {$ELSE}
      urlSessionConfig.setRequestCachePolicy(NSURLRequestUseProtocolCachePolicy);
      {$ENDIF}
    
      // create a session for configuration
      urlSession := NSURLSession.sessionWithConfiguration(urlSessionConfig);
    
      // create NSURL
      URL := NSURL.URLWithString(NSSTR(PAnsiChar('https://sentinel.sentry.org/')));
      if(Url = Nil) then
          ShowMessage('NSURL.URLWithString failed!');
    
      // setup and execute data task
      urlSession.dataTaskWithURL_completionHandler(URL, @myCompletionHandler).resume;
    
      // wait for completion handler to finish
      RTLEventWaitFor(didFinish);
    
      // display results
      if(webErrorReason <> '') then
        begin
          ShowMessage(webError);
          ShowMessage(webErrorReason);
        end
      else
        begin
          ShowMessage('HTTP status code: ' + IntToStr(webStatusCode) + LineEnding
            + LineEnding + 'Raw data: ' + LineEnding + webData);
          ShowMessage('Response: ' + LineEnding + LineEnding + webResponse);
          ShowMessage('Web page: ' + LineEnding + LineEnding + webHTML);
        end;
    
      // housekeeping
      RTLeventdestroy(didFinish);
      myCache.release;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlsession/>). 

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



## Retrieving data using a custom delegate

When using a custom delegate to retrieve data, the delegate must implement at least the following methods: 

  * URLSession:dataTask:didReceiveData: Tells the delegate that the data task has received some of the expected data.
  * URLSession:task:didCompleteWithError: Tells the delegate that the data task finished transferring the data.



If you don’t need the features provided by a delegate, you can use this API without providing one by passing nil when you create a session. 

### Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,      // for the main form
      Dialogs,    // for ShowMessage
      StdCtrls,   // for the button
      SysUtils,   // for IntToStr()
      CocoaAll,   // for NSData and other Cocoa types
      CocoaUtils, Classes; // for NSStringString
    
    type
      // redefine version from packages/cocoaint/src/foundation/NSURLSession.inc
      NSURLSession = objcclass external (NSObject)
      public
        class function sessionWithConfiguration(configuration: NSURLSessionConfiguration): NSURLSession;
          message 'sessionWithConfiguration:';
        class function sessionWithConfiguration_delegate_delegateQueue(configuration: NSURLSessionConfiguration; delegate: NSURLSessionDelegateProtocol; queue: NSOperationQueue): NSURLSession;
          message 'sessionWithConfiguration:delegate:delegateQueue:';
        function dataTaskWithURL(url: NSURL): NSURLSessionDataTask;
          message 'dataTaskWithURL:';
      end;
    
      { TMyDelegate }
      TMyDelegate = objcclass(NSObject, NSURLSessionDelegateProtocol)
      private
    
      public
        procedure URLSession_dataTask_didReceiveData (session: NSURLSession; dataTask: NSURLSessionDataTask; data: NSData);
          message 'URLSession:dataTask:didReceiveData:';
        procedure URLSession_task_didCompleteWithError (session: NSURLSession; task: NSURLSessionTask; error: NSError);
          message 'URLSession:task:didCompleteWithError:';
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      myCache: NSURLcache;
      responseData: NSMutableData;
      queue: NSOperationQueue;
      sessionDelegate: TMyDelegate;
      webData: NSString;
      webError: String;
      ThreadDidFinish: PRTLEvent;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    {$DEFINE DEBUG}
    
    procedure TMyDelegate.URLSession_dataTask_didReceiveData(session: NSURLSession; dataTask: NSURLSessionDataTask; data: NSData);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('didReceiveData %i'), data.length);
      NSLog(NSStr('didReceiveData %@'), NSString.alloc.initWithData_encoding(data,NSASCIIStringEncoding).autorelease);
      {$ENDIF}
    
      // Append the new data to the response variable
      responseData.appendData(data);
    end;
    
    procedure TMyDelegate.URLSession_task_didCompleteWithError(session: NSURLSession; task: NSURLSessionTask; error: NSError);
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('didCompleteWithError start'));
      NSLog(NSStr('responseData.length %i'), responseData.length);
      NSLog(NSStr('responseData %@'), NSString.alloc.initWithData_encoding(responseData,NSASCIIStringEncoding).autorelease);
      {$ENDIF}
    
      // Is there an error?
      if(responseData.length = 0) then
         begin
           NSLog(NSStr('Error %@'), error.description);
           webError := 'Error retrieving: ' + NSStringToString(error.userInfo.valueForKey(NSErrorFailingUrlStringKey))
            + LineEnding + LineEnding + 'Reason: ' + NSStringToString(error.localizedDescription);
         end
      else
        begin
          webError := '';
          webData :=  NSString.alloc.initWithData_encoding(responseData,NSASCIIStringEncoding);
        end;
    
      // notify main thread that data retrieval thread has finished
      RTLEventSetEvent(ThreadDidFinish);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('didCompleteWithError finish'));
      {$ENDIF}
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      urlSessionConfig: NSURLSessionConfiguration = Nil;
      cachePath: NSString = Nil;
      urlSession: NSURLSession = Nil;
      URL: NSURL = Nil;
    begin
      // create event to synchronise data retrieval background thread
      // with the GUI main thread
      ThreadDidFinish := RTLEventCreate;
    
      // create default url session config
      urlSessionConfig := NSURLSessionConfiguration.defaultSessionConfiguration;
    
      // create connection delegate
      sessionDelegate := TMyDelegate.alloc.init;
    
      // create responseData
      responseData := NSMutableData.alloc.init;
    
      // configure caching behavior for the default session
      cachePath := NSTemporaryDirectory.stringByAppendingPathComponent(NSStr('/nsurlsessiondemo.cache'));
    
      {$IFDEF DEBUG}
      NSLog(NSStr('Cache path: %@'), cachePath);
      {$ENDIF}
    
      myCache := NSURLCache.alloc.initWithMemoryCapacity_diskCapacity_diskPath(16384, 268435456, cachePath);
      urlSessionConfig.setURLCache(myCache);
    
      // set cache policy
      {$IFDEF DEBUG}
      urlSessionConfig.setRequestCachePolicy(NSURLRequestReloadIgnoringLocalCacheData);
      {$ELSE}
      urlSessionConfig.setRequestCachePolicy(NSURLRequestUseProtocolCachePolicy);
      {$ENDIF}
    
      // create a session for configuration
      urlSession := NSURLSession.sessionWithConfiguration_delegate_delegateQueue(urlSessionConfig, sessionDelegate, queue);
    
      // create NSURL
      URL := NSURL.URLWithString(NSSTR(PAnsiChar('https://sentinel.sentry.org/')));
      if(Url = Nil) then
          ShowMessage('NSURL.URLWithString failed!');
    
      // setup and execute data task
      urlSession.dataTaskWithURL(URL).resume;
    
      // wait for data retrieval thread to finish
      RTLEventWaitFor(ThreadDidFinish);
    
      // Show result
      if(webError <> '') then
         ShowMessage(webError)
      else
         ShowMessage(NSStringToString(webData));
    
      // housekeeping
      RTLeventdestroy(ThreadDidFinish);
      sessionDelegate.release;
      responseData.release;
      myCache.release;
      webData.release;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlsession2/>). 

##### Compilation note

See the previous Compilation note above. 

## Downloading files

At a high level, downloading a file is similar to retrieving data. Your application should implement the following delegate methods: 

  * **URLSession:downloadTask:didFinishDownloadingToURL** : provides your application with the URL to a temporary file where the downloaded content is stored.


  * **URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite** : provides your application with status information about the progress of the download.


  * **URLSession:downloadTask:didResumeAtOffset:expectedTotalBytes** : tells your application that its attempt to resume a previously failed download was successful.


  * **URLSession:task:didCompleteWithError** : tells your application that the download failed.

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  Before the _URLSession:task:didCompleteWithError_ method returns, it must either open the file for reading or move it to a permanent location. When this method returns, the temporary file is deleted if it still exists at its original location.  
---|---  
  
If you schedule the download in a background session, the download continues when your application is not running. If you schedule the download in a default or ephemeral session, the download must begin anew when your application is relaunched. 

While the example below only implements a few of the delegate methods, it should be simple enough for anyone to understand how to add the missing ones. The example application downloads a file to the user's Downloads folder. If the downloaded file already exists in the Downloads folder, it is removed and the just downloaded file will take its place. 

### Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,      // for the main form
      Dialogs,    // for ShowMessage
      StdCtrls,   // for the button
      SysUtils,   // for IntToStr()
      CocoaAll,   // for NSData and other Cocoa types
      CTypes;     // for cInt64
    
    type
      { TMyDlDelegate }
      TMyDlDelegate = objcclass(NSObject, NSURLSessionDelegateProtocol)
      private
    
      public
        procedure URLSession_downloadtask_didFinishDownloadingToURL(session: NSURLSession; downloadTask: NSURLSessionDownloadTask;
          location: NSURL);
          message 'URLSession:downloadTask:didFinishDownloadingToURL:';
        procedure URLSession_downloadTask_didWriteData_totalBytesWritten_totalBytesExpectedToWrite (session: NSURLSession;
          downloadTask: NSURLSessionDownloadTask; bytesWritten: cint64; totalBytesWritten: cint64; totalBytesExpectedToWrite: cint64);
          message 'URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite:';
        {procedure URLSession_downloadTask_didResumeAtOffset_expectedTotalBytes (session: NSURLSession;
          downloadTask: NSURLSessionDownloadTask; fileOffset: cint64; expectedTotalBytes: cint64);
          message 'URLSession:downloadTask:didResumeAtOffset:expectedTotalBytes:';}
      end;
    
      { TForm1 }
    
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
    
    { TForm1 }
    
    {$DEFINE DEBUG}
    
    //
    // Delegate methods
    //
    procedure TMyDlDelegate.URLSession_downloadtask_didFinishDownloadingToURL(session: NSURLSession;
      downloadTask: NSURLSessionDownloadTask; location: NSURL);
    var
      downloadsPath: NSString;
      downloadsDirectoryURL: NSURL;
      downloadURL: NSURL;
      error: NSError;
      filePath: NSString;
    begin
      downloadsPath := NSSearchPathForDirectoriesInDomains(NSDownloadsDirectory, NSUserDomainMask, True).firstObject;
      downloadsDirectoryURL := NSURL.fileURLWithPath(downloadsPath);
      downloadURL := downloadsDirectoryURL.URLByAppendingPathComponent(downloadTask.response.suggestedFilename);
      filePath := downloadsPath.stringByAppendingPathComponent(downloadTask.response.suggestedFilename);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('Session %@ Download task %@ Finished downloading to URL %@'), session, downloadTask, location);
      NSLog(NSStr('-------------------------------------------'));
      NSLog(NSStr('downloads path: %@'), downloadsPath);
      NSLog(NSStr('downloads directory URL: %@'), downloadsDirectoryURL);
      NSLog(NSStr('download URL: %@'), downloadURL);
      NSLog(NSStr('file path: %@'), filePath);
      NSLog(NSStr('-------------------------------------------'));
      {$ENDIF}
    
      // remove any old file from the destination directory
      if(NSFileManager.defaultManager.fileExistsAtPath(filePath)) then
        begin
          NSLog(NSStr('Oh no, file already exists in downloads directory! Will remove for you :-)'));
          if (NSFileManager.defaultManager.removeItemAtPath_error(filePath, @error) <> True) then
            NSLog(NSStr('Oh no, removal of %@ failed, giving up :-('), filePath);
        end;
    
      // file move error
      if(NSFileManager.defaultManager.moveItemAtURL_toURL_error(location,downloadURL, @Error) <> True) then
        NSLog(NSStr('Download error %@'), Error.localizedDescription);
    end;
    
    procedure TMyDlDelegate.URLSession_downloadTask_didWriteData_totalBytesWritten_totalBytesExpectedToWrite(session: NSURLSession;
      downloadTask: NSURLSessionDownloadTask; bytesWritten: cint64; totalBytesWritten: cint64; totalBytesExpectedToWrite: cint64);
    begin
      NSLog(NSStr('Session %@ download task %@ wrote an additional %lld bytes (total %lld bytes) out of an expected %lld bytes.'),
        session, downloadTask, bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
    end;
    
    //
    // Download button handler
    //
    procedure TForm1.Button1Click(Sender: TObject);
    var
      dlReq: NSURL = Nil;
      dlSessionConfig: NSURLSessionConfiguration = Nil;
      dlSession: NSURLSession = Nil;
      dlTask: NSURLSessionDownloadTask = Nil;
      dlcache: NSURLcache;
      dlcachePath: NSString = Nil;
      dlQueue: NSOperationQueue = Nil;
      dlDelegate: TMyDlDelegate = Nil;
    begin
      // create request
      dlReq := NSURL.URLWithString(NSStr('https://www.sentry.org/microlab/ASM_19930531.ZIP'));
      if(dlReq = Nil) then
        begin
          ShowMessage('NSURL.URLWithString failed! Aborting...');
          Exit;
        end;
    
      // create download delegate
      dlDelegate := TMyDlDelegate.alloc.init;
    
      // create background session config
      dlSessionConfig := NSURLSessionConfiguration.backgroundSessionConfigurationWithIdentifier(NSStr('dtask01'));
    
      // create a session with background session configuration
      dlSession := NSURLSession.sessionWithConfiguration_delegate_delegateQueue(dlSessionConfig, dlDelegate, dlQueue);
    
      // configure caching behavior for the session configuration
      dlCachePath := NSTemporaryDirectory.stringByAppendingPathComponent(NSStr('/nsurlsessiondemo.cache'));
      dlCache := NSURLCache.alloc.initWithMemoryCapacity_diskCapacity_diskPath(16384, 268435456, dlCachePath);
      dlSessionConfig.setURLCache(dlCache);
    
      // log cache path
      {$IFDEF DEBUG}
      NSLog(NSStr('Cache path: %@'), dlCachePath);
      {$ENDIF}
    
      // set cache policy
      {$IFDEF DEBUG}
      dlSessionConfig.setRequestCachePolicy(NSURLRequestReloadIgnoringLocalCacheData);
      {$ELSE}
      dlSessionConfig.setRequestCachePolicy(NSURLRequestUseProtocolCachePolicy);
      {$ENDIF}
    
      // setup download task
      dlTask := dlSession.downloadTaskWithURL(dlReq);
    
      // execute download task
      dlTask.resume;
    
      // housekeeping
      dlDelegate.release;
      dlCache.release;
      dlSession.finishTasksAndInvalidate;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlsession3/>). 

## Downloading files with graphical user interface feedback

You may have noticed that all the user feedback in the previous example has been relegated to NSLog entries. There is a reason for this. Any attempt to provide user feedback to the graphical user interface from within a delegate method running on a secondary or background thread will cause the application to die with a log entry similar to this: 
    
    
     *** Terminating app due to uncaught exception 'NSInternalInconsistencyException',
     reason: 'NSWindow drag regions should only be invalidated on the Main Thread!'
    

The cause? Apple’s UI frameworks, and Cocoa in particular, can only be called from the main thread. This means that if you have a background threading process, it cannot interact with the user interface because all user interface objects are marked as not safe for threading. Touching the user interface from a background thread can cause all sorts of issues, including corruption of internal state, crashes, or inexplicable incorrect behaviour. 

The solution? _performSelectorOnMainThread:withObject:waitUntilDone:_. The selector you specify will be dispatched when the run loop of the main thread is in the default mode. When the run loop is in another mode, the selector will only be executed when the run loop is switched to the default mode. You can use _performSelectorOnMainThread:withObject:waitUntilDone:modes:_ to specify the mode(s) if necessary. 

The example below implements _performSelectorOnMainThread:withObject:waitUntilDone:_ in two places and uses an application-wide object (TdlStatsuObj) to update the content of a memo on the form with status information about the progress of the download. 

### Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,      // for the main form
      Dialogs,    // for ShowMessage
      StdCtrls,   // for the button
      SysUtils,   // for IntToStr()
      CocoaAll,   // for NSData and other Cocoa types
      CocoaUtils, // for NSTringToString
      CTypes;     // for cInt64
    
    type
      { TDownloadlDelegate }
      TDownloadlDelegate = objcclass(NSObject, NSURLSessionDelegateProtocol)
      private
    
      public
        procedure URLSession_downloadtask_didFinishDownloadingToURL(session: NSURLSession; downloadTask: NSURLSessionDownloadTask;
          location: NSURL);
          message 'URLSession:downloadTask:didFinishDownloadingToURL:';
        procedure URLSession_downloadTask_didWriteData_totalBytesWritten_totalBytesExpectedToWrite (session: NSURLSession;
          downloadTask: NSURLSessionDownloadTask; bytesWritten: cint64; totalBytesWritten: cint64; totalBytesExpectedToWrite: cint64);
          message 'URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite:';
        procedure updateProgress;
          message 'updateProgess';
        procedure downloadFinished;
          message 'downloadFinished';
      end;
    
      { TdlStatsuObj - application-wide object to pass to main thread for GUI status updates}
      TdlStatusObj = objcclass(NSObject)
      private
        _BytesRx: Int64;
        _TotalBytesRx: Int64;
        _ExpectedTotalBytes: Int64;
        _Location: NSString;
      public
        procedure setBytesRx(newValue: Int64);
          message 'setBytesRx:';
        function getBytesRx: Int64;
          message 'getBytesRx';
        procedure setTotalBytesRx(newValue: Int64);
          message 'setTotalBytesRx:';
        function getTotalBytesRx: Int64;
          message 'getTotalBytesRx';
        procedure setExpectedTotalBytes(newValue: Int64);
          message 'setExpectedTotalBytes:';
        function getExpectedTotalBytes: Int64;
          message 'getExpectedTotalBytes';
        procedure setLocation(newValue: NSString);
          message 'setLocation:';
        function getLocation: NSString;
          message 'getLocation';
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      dlStatusObj: TdlStatusObj;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    //{$DEFINE DEBUG}
    
    //
    // TdlStatusObj methods
    //
    procedure TdlStatusObj.setBytesRx(newValue: Int64);
    begin
      _BytesRx := newValue;
    end;
    
    function TdlStatusObj.getBytesRx: Int64;
    begin
      getBytesRx := _BytesRx;
    end;
    
    procedure TdlStatusObj.setTotalBytesRx(newValue: Int64);
    begin
      _TotalBytesRx := newValue;
    end;
    
    function TdlStatusObj.getTotalBytesRx: Int64;
    begin
      getTotalBytesRx := _TotalBytesRx;
    end;
    
    procedure TdlStatusObj.setExpectedTotalBytes(newValue: Int64);
    begin
      _ExpectedTotalBytes := newValue;
    end;
    
    function TdlStatusObj.getExpectedTotalBytes: Int64;
    begin
      getExpectedTotalBytes := _ExpectedTotalBytes;
    end;
    
    procedure TdlStatusObj.setLocation(newValue: NSString);
    begin
      _Location := newValue;
    end;
    
    function TdlStatusObj.getLocation: NSString;
    begin
      getLocation := _Location;
    end;
    
    //
    // Delegate methods
    //
    procedure TDownloadlDelegate.URLSession_downloadtask_didFinishDownloadingToURL(session: NSURLSession;
      downloadTask: NSURLSessionDownloadTask; location: NSURL);
    var
      downloadsPath: NSString;
      downloadsDirectoryURL: NSURL;
      downloadURL: NSURL;
      error: NSError;
      filePath: NSString;
      aSel: SEL;
    begin
      downloadsPath := NSSearchPathForDirectoriesInDomains(NSDownloadsDirectory, NSUserDomainMask, True).firstObject;
      downloadsDirectoryURL := NSURL.fileURLWithPath(downloadsPath);
      downloadURL := downloadsDirectoryURL.URLByAppendingPathComponent(downloadTask.response.suggestedFilename);
      filePath := downloadsPath.stringByAppendingPathComponent(downloadTask.response.suggestedFilename);
    
      {$IFDEF DEBUG}
      NSLog(NSStr('-------------------------------------------'));
      NSLog(NSStr('downloads path: %@'), downloadsPath);
      NSLog(NSStr('downloads directory URL: %@'), downloadsDirectoryURL);
      NSLog(NSStr('download URL: %@'), downloadURL);
      NSLog(NSStr('file path: %@'), filePath);
      NSLog(NSStr('-------------------------------------------'));
      NSLog(NSStr('Session %@ Download task %@ Finished downloading to URL %@'), session, downloadTask, location);
      {$ENDIF}
    
      // populate download status object
      dlStatusObj.setLocation(filePath);
    
      // update the user interface on the main thread.
      aSel := ObjCSelector(TDownloadlDelegate.downloadFinished);
      self.performSelectorOnMainThread_withObject_waitUntilDone(aSel, dlStatusObj, true);
    
      // remove any old file from the destination directory
      if(NSFileManager.defaultManager.fileExistsAtPath(filePath)) then
        begin
          {$IFDEF DEBUG}
          NSLog(NSStr('Oh no, file already exists in downloads directory! Will remove for you :-)'));
          {$ENDIF}
          if (NSFileManager.defaultManager.removeItemAtPath_error(filePath, @error) <> True) then
            begin
              {$IFDEF DEBUG}
              NSLog(NSStr('Oh no, removal of %@ failed with erro %@'), filePath, error.localizedDescription);
              {$ENDIF}
            end;
        end;
    
      // file move error
      if(NSFileManager.defaultManager.moveItemAtURL_toURL_error(location,downloadURL, @Error) <> True) then
        begin
          {$IFDEF DEBUG}
          NSLog(NSStr('Download error %@'), Error.localizedDescription);
          {$ENDIF}
        end;
    end;
    
    procedure TDownloadlDelegate.URLSession_downloadTask_didWriteData_totalBytesWritten_totalBytesExpectedToWrite(session: NSURLSession;
      downloadTask: NSURLSessionDownloadTask; bytesWritten: cint64; totalBytesWritten: cint64; totalBytesExpectedToWrite: cint64);
    var
      aSel: SEL;
    begin
      {$IFDEF DEBUG}
      NSLog(NSStr('Session %@ download task %@ wrote an additional %lld bytes (total %lld bytes) out of an expected %lld bytes.'),
        session, downloadTask, bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
      {$ENDIF}
    
      // populate status object
      dlStatusObj.setBytesRx(bytesWritten);
      dlStatusObj.setTotalBytesRx(totalBytesWritten);
      dlStatusObj.setExpectedTotalBytes(totalBytesExpectedToWrite);
    
      // update the user interface on the main thread.
      aSel := ObjCSelector(TDownloadlDelegate.updateProgress);
      self.performSelectorOnMainThread_withObject_waitUntilDone(aSel, dlStatusObj, true);
    end;
    
    procedure TDownloadlDelegate.updateProgress;
    begin
      Form1.Memo1.Append('Bytes received: ' + IntToStr(dlStatusObj.getBytesRx));
      Form1.Memo1.Append('Total bytes received: ' + IntToStr(dlStatusObj.getTotalBytesRx));
      Form1.Memo1.Append('Expected total bytes: ' + IntToStr(dlStatusObj.getExpectedTotalBytes));
      Form1.Memo1.Append('---');
    end;
    
    procedure TDownloadlDelegate.downloadFinished;
    begin
      Form1.Memo1.Append('Download of ' + NSStringToString(dlStatusObj.getLocation) + ' finished!');
    end;
    
    //
    // Download button handler
    //
    procedure TForm1.Button1Click(Sender: TObject);
    var
      dlReq: NSURL = Nil;
      dlSessionConfig: NSURLSessionConfiguration = Nil;
      dlSession: NSURLSession = Nil;
      dlTask: NSURLSessionDownloadTask = Nil;
      dlcache: NSURLcache;
      dlcachePath: NSString = Nil;
      dlQueue: NSOperationQueue = Nil;
      dlDelegate: TDownloadlDelegate = Nil;
    begin
      // create request
      dlReq := NSURL.URLWithString(NSStr('https://www.sentry.org/microlab/Microlab_Teach-In-93-Pt12.pdf'));
      if(dlReq = Nil) then
        begin
          ShowMessage('NSURL.URLWithString failed! Aborting...');
          Exit;
        end;
    
      // create download delegate
      dlDelegate := TDownloadlDelegate.alloc.init;
    
      // create background session config
      dlSessionConfig := NSURLSessionConfiguration.backgroundSessionConfigurationWithIdentifier(NSStr('dtask01'));
    
      // create a session with background session configuration
      dlSession := NSURLSession.sessionWithConfiguration_delegate_delegateQueue(dlSessionConfig, dlDelegate, dlQueue);
    
      // configure caching behaviour for the session configuration
      dlCachePath := NSTemporaryDirectory.stringByAppendingPathComponent(NSStr('/nsurlsessiondemo.cache'));
      dlCache := NSURLCache.alloc.initWithMemoryCapacity_diskCapacity_diskPath(16384, 268435456, dlCachePath);
      dlSessionConfig.setURLCache(dlCache);
    
      // log cache path
      {$IFDEF DEBUG}
      NSLog(NSStr('Cache path: %@'), dlCachePath);
      {$ENDIF}
    
      // set cache policy
      {$IFDEF DEBUG}
      dlSessionConfig.setRequestCachePolicy(NSURLRequestReloadIgnoringLocalCacheData);
      {$ELSE}
      dlSessionConfig.setRequestCachePolicy(NSURLRequestUseProtocolCachePolicy);
      {$ENDIF}
    
      // setup download task
      dlTask := dlSession.downloadTaskWithURL(dlReq);
    
      // create download status object
      dlStatusObj := TdlStatusObj.alloc.init;
    
      // execute download task
      dlTask.resume;
    
      // housekeeping
      dlDelegate.release;
      dlCache.release;
      dlSession.finishTasksAndInvalidate;
    end;
    
    end.
    

Note that this is a very minimal demonstration application. In a real application you would want to, for example, extend the application-wide object (TdlStatus) to include any error messages which are still being handled by NSlog in the above example. 

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsurlsession4/>). 

## See also

  * [macOS NSURLConnection](<macOS_NSURLConnection.md> "macOS NSURLConnection") deprecated in favour of macOS NSURLSession.
  * [fpwebview](<fpwebview.md> "fpwebview") \- Free Pascal binding for webview, a cross-platform library that links Cocoa/WebKit on macOS.



## External links

  * [Apple: NSString](<https://developer.apple.com/documentation/foundation/nsstring>).
  * [Apple: NSURL](<https://developer.apple.com/documentation/foundation/nsurl?language=objc>).
  * [Apple: NSURLCache](<https://developer.apple.com/documentation/foundation/nsurlcache>).
  * [Apple: NSURLSession](<https://developer.apple.com/documentation/foundation/nsurlsession>).
  * [Apple: NSURLRequest](<https://developer.apple.com/documentation/foundation/nsurlrequest>).
  * [Apple: NSURLSessionDownloadTask](<https://developer.apple.com/documentation/foundation/nsurlsessiondownloadtask>).
  * [Apple: NSURLSessionConfiguration](<https://developer.apple.com/documentation/foundation/nsurlsessionconfiguration>).
  * [Apple: NSURLResponse](<https://developer.apple.com/documentation/foundation/nsurlresponse>).
  * [Apple: NSHTTPURLResponse](<https://developer.apple.com/documentation/foundation/nshttpurlresponse>).
  * [Apple: performSelectorOnMainThread:withObject:waitUntilDone](<https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselectoronmainthread>).
  * [Apple: Thread Programming Guide - Threads and Your User Interface](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW21>).
  * [Apple: Network Framework](<https://developer.apple.com/documentation/network>) on which NSURLSession is built.
  * [Apple: NSAppTransportSecurity](<https://developer.apple.com/documentation/bundleresources/information_property_list/nsapptransportsecurity>).

---

_Source: [https://wiki.freepascal.org/macOS_NSURLSession](https://web.archive.org/web/20241212114306/https://wiki.freepascal.org/macOS_NSURLSession)_
