# macOS Find Default Application

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Example code
  * 3 Alternative example code for URL schemes
  * 4 See also
  * 5 External links



## Overview

From time to time your application may need to determine the user's default application for opening certain content - for example, a web URL or a system file. 

Example code which demonstrates how to do this is set out below. Note that these are obviously not the only two content for which you may wish to determine the user's default application, but the same general approach will apply for those other content types. 

The alternative example code below demonstrates how to do this for various URL schemes (eg mailto:, http:, https:, sms:, tel:, etc). 

## Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    interface
    
    uses
      Classes,
      Forms,
      Dialogs,    // needed for ShowMessage()
      StdCtrls,   // needed for buttons
      CocoaAll,   // needed for Cocoa framework (origin the NeXTSTEP libraries)
      CocoaUtils; // needed for NSStringToString()
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Button2: TButton;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      contentUrl: NSUrl;
      defAppUrl: NSUrl;
      defAppName: String;
    begin
      contentUrl := NSUrl.URLWithString(NSSTR('https://sentinel.sentry.org/'));
      defAppUrl := NSWorkspace.sharedWorkspace.URLForApplicationToOpenURL(contentUrl);
    
      // Bundle ID (required)
      ShowMessage(NSStringToString(NSBundle.bundleWithURL(defAppUrl).bundleIdentifier));
    
      // Bundle Display Name (optional)
      defAppName := NSStringToString(NSBundle.bundleWithURL(defAppUrl).objectForInfoDictionaryKey(NSStr('CFBundleDisplayName').description));
      // Bundle Name (conventionally required)
      if (defAppName = '') then
        defAppName := NSStringToString(NSBundle.bundleWithURL(defAppUrl).objectForInfoDictionaryKey(NSStr('CFBundleName').description));
      // Cannot fail :-)
      if (defAppName = '') then
        defAppName := NSStringToString(defAppUrl.URLByDeletingPathExtension.lastPathComponent);
    
      // Show result
      ShowMessage(defAppName);
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    var
      contentUrl: NSUrl;
      defAppUrl: NSUrl;
      defAppName: String;
    begin
      contentUrl := NSUrl.URLWithString(NSSTR('file:///Users/trev/my_defaults.txt'));
      defAppUrl := NSWorkspace.sharedWorkspace.URLForApplicationToOpenURL(contentUrl);
    
      // Bundle ID (required)
      ShowMessage(NSStringToString(NSBundle.bundleWithURL(defAppUrl).bundleIdentifier));
    
      // Bundle Display Name (optional)
      defAppName := NSStringToString(NSBundle.bundleWithURL(defAppUrl).objectForInfoDictionaryKey(NSStr('CFBundleDisplayName').description));
      // Bundle Name (conventionally required)
      if (defAppName = '') then
        defAppName := NSStringToString(NSBundle.bundleWithURL(defAppUrl).objectForInfoDictionaryKey(NSStr('CFBundleName').description));
      // Cannot fail :-)
      if (defAppName = '') then
        defAppName := NSStringToString(defAppUrl.URLByDeletingPathExtension.lastPathComponent);
    
      // Show result
      ShowMessage(defAppName);
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_default_app/>). 

## Alternative example code for URL schemes

Note that _LSGetApplicationForURL_ was deprecated in macOS 10.10 (Yosemite) in favour of _LSCopyDefaultApplicationURLForURL_ which itself was deprecated in macOS 12 (Monterey), both functions are still working in macOS 12.0.1. There is no example code for _LSCopyDefaultApplicationURLForURL_ because the function is missing from the FPC `packages/univint/src/LSInfo.pas` file. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      SysUtils, Forms, StdCtrls,
      MacOSAll, CocoaAll, CocoaUtils;
    
    type
    
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
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      myURL: NSURL;
      URLRef: CFURLRef;
      OSStatus: Integer;
      myStr: String;
    begin
      // Create URL scheme from one of:
      // file:///
      // file://localhost/
      // mailto:
      // http:
      // https:
      // ftp:
      // sms:
      // tel:
      // facetime:
      // ... There are probably more.
      myURL := NSURL.alloc.initWithString(NSStr('mailto:'));
    
      // Check for default pplication
      OSStatus := LSGetApplicationForURL(CFURLRef(myURL), kLSRolesAll, Nil, @URLRef);
    
      // Check for error (eg application not found for URL = -10814)
      if(OSStatus <> 0) then
        begin
          Memo1.Append('LSGetApplicationForURL error status: ' + IntToStr(OSStatus));
          Exit;
        end;
    
      // Get CFString from CFURLRef, Cast to NSString, Convert to AnsiString
      myStr := NSStringToString(NSString(CFURLGetString(URLRef)));
    
      // Output to memo
      memo1.Append('LSGetApplicationForURL status: ' + IntToStr(OSStatus)
        + LineEnding + LineEnding + 'Default app: ' + myStr);
    
      // Housekeeping
      myURL.release;
    end;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_default_app2/>). 

## See also

  * [Find the default web browser](<Webbrowser.md> "Webbrowser") \- cross-platform example.



## External links

  * [Apple: NSWorkspace](<https://developer.apple.com/documentation/appkit/nsworkspace>) \- available from macOS 10.0
  * [Apple: NSURL](<https://developer.apple.com/documentation/foundation/nsurl>) \- available from macOS 10.0
  * [Apple: CFURL, CFURLRef](<https://developer.apple.com/documentation/corefoundation/cfurl?language=objc>) \- available from macOS 10.0
  * [Apple: LSGetApplicationForURL](<https://developer.apple.com/documentation/coreservices/1445210-lsgetapplicationforurl>) \- available from macOS 10.0 - deprecated in macOS 10.10
  * [Apple: LSCopyDefaultApplicationURLForURL](<https://developer.apple.com/documentation/coreservices/1448824-lscopydefaultapplicationurlforur>) \- available from macOS 10.10 - deprecated in macOS 12

---

_Source: [https://wiki.freepascal.org/macOS_Find_Default_Application](https://web.archive.org/web/20241213024752/https://wiki.freepascal.org/macOS_Find_Default_Application)_
