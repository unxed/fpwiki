# fpwebview

│ **English (en)** │

## Contents

  * 1 About
  * 2 Getting started
  * 3 Demo program
  * 4 License
  * 5 Download



## About

**fpwebview** is Free Pascal binding for [webview](<https://github.com/webview/webview>), the cross-platform library that links with gtk-webkit2 on Linux, Cocoa/WebKit on macOS and Edge on Windows 10 for building desktop hybrid GUI+web applications. 

Supporting shared library, dylib, DLL and import library files for x86_64 (Linux, macOS, Windows), i386 (Windows) and aarch64 (macOS) are bundled. 

## Getting started

**Linux**
    
    
    % git clone <https://github.com/PierceNg/fpwebview>
    % cd fpwebview/demo/browser_cli
    % ./linbuild.sh
    <output>
    % ./linrun.sh
    

**macOS**
    
    
    % git clone <https://github.com/PierceNg/fpwebview>
    % cd fpwebview/demo/browser_cli
    % ./macbuild.sh
    <output>
    % ./macrun.sh
    

**Windows**
    
    
    C:\> git clone <https://github.com/PierceNg/fpwebview>
    C:\> cd fpwebview\demo\browser_cli
    C:\fpwebview\demo\browser_cli> winbuild.bat
    <output>
    C:\fpwebview\demo\browser_cli> winrun.bat
    

## Demo program

Here's a simple Pascal program that uses fpwebview to implement a web browser. 
    
    
    program browser_cli;
    
    {$linklib libwebview}
    
    uses
      {$ifdef unix}cthreads,{$endif}
      math,
      webview;
    
    var
      w: PWebView;
    
    begin
      { Set math masks. libwebview throws at least one of these from somewhere deep inside. }
      SetExceptionMask([exInvalidOp, exDenormalized, exZeroDivide, exOverflow, exUnderflow, exPrecision]);
    
      writeln('Hello, webview, from Pascal!');
      w := webview_create(WebView_DevTools, nil);
      webview_set_size(w, 1024, 768, WebView_Hint_None);
      webview_set_title(w, PAnsiChar('WebView Free Pascal'));
      webview_navigate(w, PAnsiChar('https://www.freepascal.org/'));
      webview_run(w);
      webview_destroy(w);
      writeln('Goodbye, webview.');
    end.
    

## License

The code is under MIT license, with following exception: 

  * Microsoft's WebView2Loader.dll is distributed as per LICENSE.Microsoft.



## Download

[GitHub repo](<https://github.com/PierceNg/fpwebview/>)

---

_Source: [https://wiki.freepascal.org/fpwebview](https://web.archive.org/web/20260130112254/https://wiki.freepascal.org/fpwebview)_
