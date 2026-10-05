# Using macOS Sheets

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

A sheet is a modal dialog that’s attached to a particular window—usually a document—and prevents further interaction with the window until the sheet is dismissed. It is the Apple recommended way to request user input before proceeding with a document-specific action, such as attaching files, exporting, saving, and printing. Sheets are also used to present information to the user before the next step, for example, a licensing agreement that requires acceptance. 

According to Apple's [Human Interface Guidelines](<https://developer.apple.com/design/human-interface-guidelines/macos/windows-and-views/sheets/>), you should only use a sheet when a window has a frame. Sheets should always emerge from a window’s frame above the body area. Here is an example: 

[![macOSSheetExample.png](https://wiki.freepascal.org/images/d/dd/macOSSheetExample.png)](</File:macOSSheetExample.png>)

The code below creates the sheet as shown in the above example (Tested working on Mojave and Catalina). 
    
    
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
    
    procedure ShowAlertSheet(FormHandle: HWND; const TitleStr, MessageStr: string);
    var
      tNSStr, mNSStr, okNSStr, fNSStr: NSString;
      theWindow : NSWindow;
      theID : id;
    begin
      theID := NSView(FormHandle).window;
      theWindow := NSView(FormHandle).window;
    
      tNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, TitleStr, kCFStringEncodingUTF8));  // title
      mNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, MessageStr, kCFStringEncodingUTF8));// message
      okNSStr:= NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, 'OK', kCFStringEncodingUTF8));      // button caption
      fNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, '%@', kCFStringEncodingUTF8));      // format
    
      NSBeginAlertSheet(tNSStr,okNSStr,nil,nil,theWindow,theID,nil,nil,nil,fNSStr, mNSStr);
    end;
    
    ...
    
    end.
    

You can replace _NSBeginAlertSheet()_ for _NSInformationalAlertSheet()_ or _NSBeginCriticalAlertSheet()_ which have different visual attributes. In particular, when using _NSBeginCriticalAlertSheet()_ , the sheet presented to the user is badged with a caution icon. Critical alerts should be used only as specified in the "Alerts" section of the UI Element Guidelines Windows chapter of [macOS Human Interface Guidelines](<https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/OSXHIGuidelines/index.html>). 

To call the **ShowAlertSheet** procedure from, for example, a menu item: 
    
    
    procedure TForm1.MenuItem3Click(Sender: TObject);
    begin
      ShowAlertSheet(Form1.Handle, 'Test', 'Message text');
    end;
    

## See also

  * [macOS Big Sur changes for developers - Sheets](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers")

---

_Source: [https://wiki.freepascal.org/Using_macOS_Sheets](https://web.archive.org/web/20240229225455/https://wiki.freepascal.org/Using_macOS_Sheets)_
