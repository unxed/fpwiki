# macOS NSAlert

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Alert styles
  * 3 Code examples
    * 3.1 Modal error dialog with OK button
    * 3.2 Modal error sheet with OK button
    * 3.3 Modal error sheet with three buttons
    * 3.4 Suppress future alerts
  * 4 See also
  * 5 External links



## Overview

The NSAlert class allows you to specify an alert level, alert text, button titles, and a custom icon if required. The class also lets your alerts display a help button and provides ways for applications to offer help specific to an alert. The NSAlert class is available from Mac OS X Panther 10.3 onwards. An NSAlert object is intended for a single alert with a unique combination of title, buttons, and so on that is displayed on a particular condition. You should create an NSAlert object for each alert dialog, creating it only when you need to display it, and releasing it when you are done. 

## Alert styles

There are three alert styles: 

  * Critical: Use when severe consequences may result from certain user responses (for example, a "clean install" will erase all data on a volume). This style includes a caution icon badged with the application icon.


  * Warning: Use to warn the user about a current or impending event. The purpose is more than informational but not critical. This is the default alert style if none is specified. It is badged with the application icon.


  * Informational: Use to inform the user about a current or impending event. It is badged with the application icon.



Unless you change it with the _setMessageText_ method, the title is "Alert" for all three styles (the actual explanatory text is set with the _setInformativeText_ method). 

The global variable names for styles changed between macOS 10.12 (Sierra) and 10.13 (High Sierra): 

**macOS 10.13 (Sierra) onwards**

  * NSAlertStyleCritical
  * NSAlertStyleWarning
  * NSAlertStyleInformational



**macOS 10.3 (Panther) through 10.12 (Sierra)**

  * NSCriticalAlertStyle
  * NSWarningAlertStyle
  * NSInformationalAlertStyle



## Code examples

### Modal error dialog with OK button

This example demonstrates a simple modal error dialog with just error message text and an OK button to exit the dialog. Compiles with FPC 3.0.4 onwards. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,    // needed for Form1
      StdCtrls, // needed for Button1
      CocoaAll; // needed for NSAlert, NSStr
    
    type
    
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
    
    {Display error message modal dialog with OK button.}
    procedure ErrorModalDlgWithOKbutton(const ErrorMsg : NSstring);
    var
      Alert : NSAlert;
    begin
      Alert := NSAlert.alloc.init;
    
      // If you want to change the default "Alert" message to something else:
      //Alert.setMessageText(NSStr('Message Text'));
      Alert.setInformativeText(ErrorMsg);
    
      Alert.runModal;
      Alert.release;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ErrorModalDlgWithOKbutton(NSStr('Error Message Test!'));
    end;
    
    end.
    

### Modal error sheet with OK button

This example demonstrates a simple modal error sheet with just error message text and an OK button to close the sheet. To compile this example you need to use FPC 3.2.0 onwards. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,    // needed for Form1
      StdCtrls, // needed for Button1
      CocoaAll; // needed for NSAlert, NSStr
    
    type
    
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
    
    {Display error message modal dialog}
    procedure ModalErrorSheetWithOKbutton(const ErrorMsg: NSString);
    var
      Alert : NSAlert;
    begin
      Alert := NSAlert.alloc.init;
    
      Alert.setAlertStyle(NSCriticalAlertStyle);
      // If you want to change the default "Alert" message to something else:
      //Alert.setMessageText(NSStr('Message Text'));
      Alert.setInformativeText(ErrorMSg);
     
      Alert.beginSheetModalForWindow_completionHandler(NSView(Form1.Handle).window, nil);
      Alert.release;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ModalErrorSheetWithOKbutton(NSStr('Error message test'));
    end;
    
    end.
    

### Modal error sheet with three buttons

This example demonstrates a more complex modal error sheet with error message text and three possible buttons with which to close the sheet. Depending on which button you use to close the sheet, different actions can be taken -- in this example, we simply change the caption of a label to indicate which button was used to close the sheet. The magic to handle the buttons is in the completion handler block which executes after the sheet is closed. 

To compile this example you need to use FPC 3.2.0 onwards. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$modeswitch cblocks}
    
    interface
    
    uses
      Forms,    // needed for Form1
      StdCtrls, // needed for Button1
      CocoaAll; // needed for NSAlert, NSstring, NSStr
    
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
      { cBlock }
    
      // setup cBlock for beginSheetModalForWindow_completionHandler method
      tBlock = reference to procedure(resultCode: NSModalResponse); cdecl; cblock;
    
      { NSAlert }
    
      NSAlert = objcclass external (NSObject)
      public
        // Need to redefine beginSheetModalForWindow_completionHandler version from
        // packages/cocoaint/src/appkit/NSAlert.inc to use our tBlock (cf OpaqueCBlock)
        procedure beginSheetModalForWindow_completionHandler(sheetWindow: NSWindow; handler: tBlock);
          message 'beginSheetModalForWindow:completionHandler:';
        // Below not redefined - you could omit the methods you do not use
        class function alertWithError (error: NSError): NSAlert; message 'alertWithError:';
        class function alertWithMessageText_defaultButton_alternateButton_otherButton_informativeTextWithFormat(message_: NSString;
          defaultButton: NSString; alternateButton: NSString; otherButton: NSString; format: NSString): NSAlert; varargs;
          message 'alertWithMessageText:defaultButton:alternateButton:otherButton:informativeTextWithFormat:';
          deprecated 'in 10_3, 10_10, "Use -init instead"';
        procedure setMessageText(newValue: NSString); message 'setMessageText:';
        function messageText: NSString; message 'messageText';
        procedure setInformativeText(newValue: NSString); message 'setInformativeText:';
        function informativeText: NSString; message 'informativeText';
        procedure setIcon(newValue: NSImage); message 'setIcon:';
        function icon: NSImage; message 'icon';
        function addButtonWithTitle (title: NSString): NSButton; message 'addButtonWithTitle:';
        function buttons: NSArray; message 'buttons';
        procedure setShowsHelp(newValue: ObjCBOOL); message 'setShowsHelp:';
        function showsHelp: ObjCBOOL; message 'showsHelp';
        procedure setHelpAnchor(newValue: NSString); message 'setHelpAnchor:';
        function helpAnchor: NSString; message 'helpAnchor';
        procedure setAlertStyle(newValue: NSAlertStyle); message 'setAlertStyle:';
        function alertStyle: NSAlertStyle; message 'alertStyle';
        procedure setDelegate(newValue: NSAlertDelegateProtocol); message 'setDelegate:';
        function delegate: NSAlertDelegateProtocol; message 'delegate';
        procedure setShowsSuppressionButton(newValue: ObjCBOOL); message 'setShowsSuppressionButton:';
        function showsSuppressionButton: ObjCBOOL; message 'showsSuppressionButton';
        function suppressionButton: NSButton; message 'suppressionButton';
        procedure setAccessoryView(newValue: NSView); message 'setAccessoryView:';
        function accessoryView: NSView; message 'accessoryView';
        procedure layout; message 'layout'; { available in 10_5 }
        function runModal: NSModalResponse; message 'runModal';
        procedure beginSheetModalForWindow_modalDelegate_didEndSelector_contextInfo (window: NSWindow;
          delegate_: id; didEndSelector: SEL; contextInfo: pointer);
          message 'beginSheetModalForWindow:modalDelegate:didEndSelector:contextInfo:';
          deprecated 'in 10_3, 10_10, "Use -beginSheetModalForWindow:completionHandler: instead"';
        function window: id; message 'window';
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { Completion handler }
    
    procedure myCompletionHandler(resultCode: NSModalResponse);
    begin
      if(resultCode = NSAlertFirstButtonReturn) then
         Form1.Label1.Caption := 'Exited with Button 1'
      else if(resultCode = NSAlertSecondButtonReturn) then
         Form1.Label1.Caption := 'Exited with Button 2'
      else if(resultCode = NSAlertThirdButtonReturn) then
         Form1.Label1.Caption := 'Exited with Button 3';
    end;
    
    { Display error message with 3 buttons as a modal sheet }
    
    procedure ModalErrorSheetWithButtons(const ErrorMsg: string; ParentWindow : NSWindow);
    var
      Alert : NSAlert;
    begin
      Alert := NSAlert.alloc.init;
    
      Alert.setAlertStyle(NSCriticalAlertStyle);
      // If you want to change the default "Alert" message to something else:
      //Alert.setMessageText(NSStr('Message Text'));
      Alert.setInformativeText(NSStr(ErrorMSg));
    
      // Note buttons are counted right to left pre-Big Sur
      // and from top to bottom with Big Sur+
      Alert.addButtonWithTitle(NSStr('Button 1'));
      Alert.addButtonWithTitle(NSStr('Button 2'));
      Alert.addButtonWithTitle(NSStr('Button 3'));
    
      Alert.beginSheetModalForWindow_completionHandler(parentWindow, @mycompletionhandler);
    
      Alert.release;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ModalErrorSheetWithButtons('Error message test', NSView(Form1.Handle).window);
    end;
    
    end.
    

### Suppress future alerts

It is possible to suppress future alerts by adding a checkbox to the alert which allows the user to opt out of receiving the same alerts when they are triggered in future . This really only makes sense for warning and informational alerts. 

My somewhat contrived example below assumes that you keep track of the user's wishes by, for example, saving the user's choice as a preference which the user can then reverse if needed. For details of how to implement preferences see [this article](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write"). 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,    // needed for Form1
      StdCtrls, // needed for Button1
      CocoaAll; // needed for NSAlert, NSstring, NSStr
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      suppressAlertFileNotSaved: Boolean;  // Whether to suppress warning file not saved
                                           // - retrieve from application preferences
    
    implementation
    
    {$R *.lfm}
    
    { Display warning message modal window }
    
    procedure ModalDlgWithOKButton(const ErrorMsg: string);
    var
      Alert : NSAlert;
    begin
      // Should this alert be suppressed?
      // - if so do nothing and exit.
      if(suppressAlertFileNotSaved) then
        exit;
    
      Alert := NSAlert.alloc.init;
    
      Alert.setAlertStyle(NSWarningAlertStyle);
      // If you want to change the default "Alert" message to something else:
      Alert.setMessageText(NSStr('Warning'));
      Alert.setInformativeText(NSStr(ErrorMSg));
      // Show checkbox to suppress future warnings
      Alert.setShowsSuppressionButton(true);
    
      Alert.runModal;
    
      // Record whether to suppress future warnings
      // - check if checkbox checked
      if(Alert.suppressionButton.state = 1) then
          suppressAlertFileNotSaved := True;
    
      Alert.release;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ModalDlgWithOKButton('You have not saved a backup of this file!');
    end;
    
    end.
    

## See also

  * [Using macOS Alert Panels](<Using_macOS_Alert_Panels.md> "Using macOS Alert Panels")



## External links

  * [Apple: AppKit Framework](<https://developer.apple.com/documentation/appkit>).
  * [Apple: NSAlert class](<https://developer.apple.com/documentation/appkit/nsalert>).

---

_Source: [https://wiki.freepascal.org/macOS_NSAlert](https://web.archive.org/web/20241213014246/https://wiki.freepascal.org/macOS_NSAlert)_
