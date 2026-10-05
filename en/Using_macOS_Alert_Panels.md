# Using macOS Alert Panels

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Example code
  * 3 See also
  * 4 External links



## Overview

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The NSRunAlertPanel functions first appeared in Mac OS X 10.0 (Cheetah) and were deprecated in OS X 10.11 (El Capitan). They have been superseded by the [NSAlert](<macOS_NSAlert.md> "macOS NSAlert") class available from Mac OS X 10.3 (Panther) which can display modal dialogs or sheets.

A panel is a special kind of window, typically serving an auxiliary function in an application. An alert panel is a type of built-in dialog for alerting the user to something and retrieving any choice the user made (eg clicking an OK or Cancel button). The function _NSRunAlertPanel_ is declared as: 
    
    
    NSInteger NSRunAlertPanel(NSString *title, NSString *msgFormat, NSString *defaultButton, NSString *alternateButton, NSString *otherButton, ...);
    

The function creates and runs an alert panel (or dialog) with the title of title and buttons with titles of defaultButton, alternateButton, and otherButton. The optional alert message specified by msgFormat should be a static value that can consist of variable values using a format string (list any necessary arguments for this formatted string at the end of the function’s argument list), an unformatted string, or an empty string, but not nil. Note that passing an untrusted string in msgFormat can lead to security issues. The panel is in a modal event loop. 

## Example code

The example code below creates an alert panel with three buttons: Cancel, Abort and OK (Tested working on Mojave, Catalina and Big Sur). Cancel returns 0; Abort returns -1 and OK returns 1. The status is displayed in the TForms's caption property. 
    
    
    unit unit1;
    
    {$modeswitch objectivec1}
    
    interface
    
    uses
      ...
      MacOSAll, CocoaAll, LCLType,
      ...
      ;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
      ...
    
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    ...
    
    function ShowAlertPanel(const alertTitle, alertMessage, defButton, altButton, othButton: String) :Integer;
    var
      tNSStr, mNSStr, defButNSStr, altButNSStr, othButNSStr: NSString;
    begin
      tNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, alertTitle, kCFStringEncodingUTF8));
      mNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, alertMessage, kCFStringEncodingUTF8));
      defButNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, defButton, kCFStringEncodingUTF8));
      altButNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, altButton, kCFStringEncodingUTF8));
      othButNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, othButton, kCFStringEncodingUTF8));
      Result := NSRunAlertPanel(tNSStr, mNSStr, defButNSStr, altButNSStr, othButNSStr);
    end;
    
    ...
    

You can replace _NSRunAlertPanel()_ with _NSRunInformationalAlertPanel()_ or _NSRunCriticalAlertPanel()_ which have different visual attributes. In particular, when using _NSRunCriticalAlertPanel()_ , the panel presented to the user is badged with a caution icon. Critical alerts should be used only as specified in the "Alerts" section of the UI Element Guidelines Windows chapter of [macOS Human Interface Guidelines](<https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/OSXHIGuidelines/index.html>). 

To call the **ShowAlertPanel** function from, for example, a menu item: 
    
    
    procedure TForm1.MenuItem1Click(Sender: TObject);
    var
      status: Integer;
    begin
      status := ShowAlertPanel('Title', 'Message text', 'OK', 'Cancel', 'Abort');
      Caption := 'Status: ' + IntToStr(status);
    end;
    

## See also

  * [macOS NSAlert](<macOS_NSAlert.md> "macOS NSAlert")



## External links

  * [Apple: NSRunAlertPane](<https://developer.apple.com/documentation/appkit/1540934-nsrunalertpanel>).

---

_Source: [https://wiki.freepascal.org/Using_macOS_Alert_Panels](https://web.archive.org/web/20230203004747/https://wiki.freepascal.org/Using_macOS_Alert_Panels)_
