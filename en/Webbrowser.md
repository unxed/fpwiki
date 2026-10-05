# Webbrowser

│ **[Deutsch (de)](</Webbrowser/de> "Webbrowser/de")** │  **English (en)** │  **[español (es)](</Webbrowser/es> "Webbrowser/es")** │  **[русский (ru)](<../ru/Webbrowser.md> "Webbrowser/ru")** │    
****

This page summarizes various ways to display HTML pages from a Pascal program, ranging from calling an external browser to adding a component capable of rendering the html page into a form. 

## Contents

  * 1 Starting a web browser to show a page/URL
    * 1.1 OpenURL
    * 1.2 Finding the default web browser
    * 1.3 Starting the browser
  * 2 Integrate web browser component in application
    * 2.1 TurboPower Internet Pro
    * 2.2 WebView4Delphi
    * 2.3 TWebBrowser WebKit (macOS Cocoa)
    * 2.4 QT Webkit
    * 2.5 THtmlPort
    * 2.6 HtmlViewer
    * 2.7 fpCEF3
    * 2.8 CEF4Delphi
    * 2.9 GeckoPort
    * 2.10 LazActiveX
    * 2.11 LazWebkit
  * 3 See also



## Starting a web browser to show a page/URL

### OpenURL

OpenURL is simplest way of opening URLs in general. This procedure will detect the default web browser and open URL from parameter in it. It can be used for mail addresses of the form 'mailto:aname@AnAddress?subject=:::A subject line', web addresses 'http://' and files 'file:///'. 
    
    
    uses LCLIntf;
    ...
    OpenURL('http://www.lazarus.freepascal.org');
    

### Finding the default web browser

Every platform has a mechanism for its default web browser. The LCL unit **lazhelphtml** contains a class [THTMLBrowserHelpViewer](<THTMLBrowserHelpViewer.md> "THTMLBrowserHelpViewer") to start a web browser for the LCL help system. 

You can use its method FindDefaultBrowser to find the default browser and the parameter to start. For example: 
    
    
    uses 
      Classes, ..., LCLProc, LazHelpHTML;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      v: THTMLBrowserHelpViewer;
      BrowserPath, BrowserParams: string;
    begin
      v:=THTMLBrowserHelpViewer.Create(nil);
      v.FindDefaultBrowser(BrowserPath,BrowserParams);
      debugln('Browser=',BrowserPath,' Params=',BrowserParams);
      v.Free;
    end;
    

This gives for example under Ubuntu (Linux): 
    
    
    Browser=/usr/bin/xdg-open Params=%s
    

Under Windows: 
    
    
    Browser=C:\windows\system32\rundll32.exe Params=url.dll,FileProtocolHandler %s
    

Under macOS: 
    
    
    Browser=/usr/bin/open Params="%s"
    

### Starting the browser

Once you know the command line and parameters you can use [TProcessUTF8](<TProcessUTF8.md> "TProcessUTF8") to start the browser: 
    
    
    uses 
      Classes, ..., LCLProc, LazHelpHTML, UTF8Process;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      v: THTMLBrowserHelpViewer;
      BrowserPath, BrowserParams: string;
      p: LongInt;
      URL: String;
      BrowserProcess: TProcessUTF8;
    begin
      v:=THTMLBrowserHelpViewer.Create(nil);
      try
        v.FindDefaultBrowser(BrowserPath,BrowserParams);
        debugln(['Path=',BrowserPath,' Params=',BrowserParams]);
    
        URL:='http://www.lazarus.freepascal.org';
        p:=System.Pos('%s', BrowserParams);
        System.Delete(BrowserParams,p,2);
        System.Insert(URL,BrowserParams,p);
    
        // start browser
        BrowserProcess:=TProcessUTF8.Create(nil);
        try
          BrowserProcess.CommandLine:=BrowserPath+' '+BrowserParams;
          BrowserProcess.Execute;
        finally
          BrowserProcess.Free;
        end;
      finally
        v.Free;
      end;
    end;
    

## Integrate web browser component in application

### TurboPower Internet Pro

Lazarus provides the package TurboPowerIPro (lazarus/components/turbopower_ipro/turbopoweripro.lpk) with the following features: 

  * It contains a control to put onto a form. When you install the package in the IDE, you get some new components in the palette, so you can drop them onto a form just like any LCL control.
  * It is written completely in Pascal and therefore works on all platforms out of the box without any extra installation.
  * You have the full control, what files/urls are opened.
  * It does not have all the features of a full webbrowser. No multimedia stuff, javascript or flash. This must be implemented by you.



There are some examples in lazarus/components/turbopower_ipro/examples demonstrating how to open a local html file with some images, hyperlinks and history buttons. See the [IPro tab](<IPro_tab.md> "IPro tab") on the [Component Palette](<Component_Palette.md> "Component Palette"). 

### WebView4Delphi

[WebView4Delphi](<WebView4Delphi.md> "WebView4Delphi") allows to embed Chromium-based web browsers in your Delphi or Lazarus applications using the WebView2 runtime. It uses many of the tricks from CEF4Delphi and you will notice many similarities if you used it. 

There are a few things pending like the "windowless mode". 

### TWebBrowser WebKit (macOS Cocoa)

TWebBrowser control is implemented only for the LCL Cocoa widgetset for macOS by wrapping the WebView class that's part of every Mac's WebKit framework. Note that by default macOS blocks calls to insecure HTTP sites. You can relax this restriction by setting the _NSAllowsArbitraryLoads_ key in your application's [Info.plist](<macOS_property_list_files.md> "macOS property list files") file, but for testing you might stick with HTTPS sites. This control does not add any bulk to your app and does not require installation of any additional libraries. Everything it needs is already on your Mac and is accessible via the FPC CocoaAll unit. [Source](<https://macpgmr.github.io/MacXPlatform/lclwebbrowser-src.zip>). 

### QT Webkit

When using the Qt widgetset, you can use the Qt Webkit to embed a browser into a LCL form. Lazarus demo is available on [FPC Qt4 Binding Web Page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). 

### THtmlPort

[THtmlPort](<THtmlPort.md> "THtmlPort") is a Lazarus/Free Pascal version of Dave Baldwin's HTML Components, including THtmlViewer, TFrameViewer and TFrameBrowser. 

### HtmlViewer

The [HtmlViewer](<https://github.com/BerndGabriel/HtmlViewer>) component for Delphi and Lazarus, set consists of: 

  * THtmlViewer
  * TFrameViewer
  * TFrameBrowser



### fpCEF3

[fpCEF3](<fpCEF3.md> "fpCEF3") is a Chromium Embedded Framework for Free Pascal. 

### CEF4Delphi

[CEF4Delphi](<CEF4Delphi.md> "CEF4Delphi") is updated Chromium Embedded Framework, which supports Lazarus. 

### GeckoPort

[GeckoPort](<GeckoPort.md> "GeckoPort") is a Lazarus/Free Pascal version of Takanori Ito's Gecko SDK for Delphi, including the TGeckoBrowser component. 

### LazActiveX

[LazActiveX](<LazActiveX.md> "LazActiveX") allows to import MS Internet Explorer type library. 

### LazWebkit

[LazWebkit](<http://sourceforge.net/p/lazwebkit/wiki/Home/>) is a project focused to provide browser and composer web content features for Lazarus IDE based projects. Currently it's implemented only for GTK2 widgetset. 

## See also

  * [Executing External Programs](<Executing_External_Programs.md> "Executing External Programs")
  * [fphttpclient](<fphttpclient.md> "fphttpclient") Part of fcl-web that can be used stand-alone to retrieve HTTP-Content
  * [fpbrowser](<fpbrowser.md> "fpbrowser") web browser written in Lazarus
  * [fpwebview](<fpwebview.md> "fpwebview") cross-platform library for Linux, macOS and Windows for building desktop applications using Pascal and web technology
  * [Using Windows native WinInet for web retrieval](<Windows_Programming_Tips.md> "Windows Programming Tips")
  * [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession") \- the replacement for [NSURLConnection](<macOS_NSURLConnection.md> "macOS NSURLConnection") for web retrieval.

---

_Source: [https://wiki.freepascal.org/Webbrowser](https://web.archive.org/web/20250422002818/https://wiki.freepascal.org/Webbrowser)_
