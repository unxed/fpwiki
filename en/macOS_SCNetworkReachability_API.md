# macOS SCNetworkReachability API

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │ 

  


  


## Overview

The `SCNetworkReachability` programming interface allows an application to determine the status of a system's current network configuration and the reachability of a target host. A remote host is considered reachable when a data packet, sent by an application into the network stack, can leave the local device. Reachability does not guarantee that the data packet will actually be received by the host. 

The SCNetworkReachability programming interface supports a synchronous and an asynchronous model. In the synchronous model, you get the reachability status by calling the _SCNetworkReachabilityGetFlags_ function. In the asynchronous model, you can schedule the SCNetworkReachability object on the run loop of a client object’s thread. The client implements a callback function to receive notifications when the reachability status of a given remote host changes. Note that these functions follow Core Foundation naming conventions. A function that has "Create" or "Copy" in its name returns a reference you must release with the CFRelease function. 

The example below implements the asynchronous model which is useful for monitoring connectivity. 

## Example code
    
    
    {
    Pascal GUI version of the Apple sample command line utility SimpleReach in C
    which demonstrates the System Configuration framework reachability APIs.
    https://developer.apple.com/library/archive/samplecode/SimpleReach/Introduction
    /Intro.html#//apple_ref/doc/uid/DTS10003238
    }
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$linkframework SystemConfiguration}
    
    interface
    
    uses
      SysUtils, Forms, StdCtrls, MacOSAll, Controls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        MonitorButton: TButton;
        QuitButton: TButton;
        HostnameEdit: TEdit;
        HostnameLabel: TLabel;
        Memo1: TMemo;
        procedure FormKeyPress(Sender: TObject; var Key: char);
        procedure MonitorButtonClick(Sender: TObject);
        procedure QuitButtonClick(Sender: TObject);
      private
    
      public
    
      end;
    
      // pocedure requires mwpascal calling convention
      type TProcedure = procedure(target: SCNetworkReachabilityRef;
        flags: SCNetworkConnectionFlags; info: pointer); mwpascal;
    
    var
      Form1: TForm1;
      callout: TProcedure;
      monitoring: Boolean = False;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // Print flag meanings
    procedure print_header;
    begin
        Form1.Memo1.Lines.Add('T' + #09 + 'kSCNetworkFlagsTransientConnection');
        Form1.Memo1.Lines.Add('R' + #09 + 'kSCNetworkFlagsReachable');
        Form1.Memo1.Lines.Add('C' + #09 + 'kSCNetworkFlagsConnectionRequired');
        Form1.Memo1.Lines.Add('A' + #09 + 'kSCNetworkFlagsConnectionAutomatic');
        Form1.Memo1.Lines.Add('I' + #09 + 'kSCNetworkFlagsInterventionRequired');
        Form1.Memo1.Lines.Add('L' + #09 + 'kSCNetworkFlagsIsLocalAddress');
        Form1.Memo1.Lines.Add('D' + #09 + 'kSCNetworkFlagsIsDirect' + LineEnding);
    end;
    
    // The procedure to print current reachability flags
    procedure print_flags(flags: SCNetworkConnectionFlags);
    var
      statusStr: String;
    begin
      statusStr := DateTimeToStr(Now) + ' : ';
    
      // The specified node name or address can be reached via a transient
      //  connection, such as PPP.
      if(flags and kSCNetworkFlagsTransientConnection > 0) then
        statusStr := statusStr + 'T'
      else
        statusStr := statusStr + '-';
    
      // The specified node name or address can be reached using the current network
      // configuration.
      if(flags and kSCNetworkFlagsReachable > 0) then
        statusStr := statusStr + 'R'
      else
        statusStr := statusStr + '-';
    
      // The specified node name or address can be reached using the current network
      // configuration, but a connection must first be established.
      // eg Returned for a dialup connection that was not currently active
      if(flags and kSCNetworkFlagsConnectionRequired > 0) then
        statusStr := statusStr + 'C'
      else
        statusStr := statusStr + '-';
    
      // The specified node name or address can be reached using the current network
      // configuration, but a connection must first be established.
      // Any traffic directed to the specified name or address will initiate the
      // connection
      if(flags and kSCNetworkFlagsConnectionAutomatic > 0) then
        statusStr := statusStr + 'A'
      else
        statusStr := statusStr + '-';
    
      // The specified node name or address can be reached using the current network
      // configuration, but a connection must first be established.
      // Returned when there is a dial-on-traffic configuration and manual
      // intervention required eg due to no dial tone, password etc.
      if(flags and kSCNetworkFlagsInterventionRequired > 0) then
        statusStr := statusStr + 'I'
      else
        statusStr := statusStr + '-';
    
      // The specified node name or address is one associated with a network
      // interface on the current system.
      if(flags and kSCNetworkFlagsIsLocalAddress > 0) then
        statusStr := statusStr + 'L'
      else
        statusStr := statusStr + '-';
    
      // Network traffic to the specified node name or address does not go through
      // a gateway, but is routed directly to one of the interfaces in the system.
      if(flags and kSCNetworkFlagsIsDirect > 0) then
        statusStr := statusStr + 'D'
      else
        statusStr := statusStr + '-';
    
      Form1.Memo1.Lines.Add(statusStr);
    end;
    
    // The procedure to be called when the reachability of the target changes.
    procedure status_callback(target: SCNetworkReachabilityRef; flags: SCNetworkConnectionFlags; info: pointer); mwpascal;
    begin
      print_flags(flags);
    end;
    
    // Start monitoring/ Stop monitoring
    procedure TForm1.MonitorButtonClick(Sender: TObject);
    var
      context: SCNetworkReachabilityContext;
      hostname: PChar;
      charArray: Array[0..254] of Char;
      count: Integer;
      status: boolean;
      target: SCNetworkReachabilityRef;
    begin
      // Action iff we have text in HostnameEdit.Text
      if(length(HostnameEdit.Text) = 0) then
        exit;
    
      // Update monitoring status
      if(monitoring = True) then
        begin
          // Remove our callback from the runloop.
          SCNetworkReachabilityUnscheduleFromRunLoop(target, CFRunLoopGetCurrent, kCFRunLoopDefaultMode);
          // Update status of button, memo, flag
          MonitorButton.Caption := 'Monitor';
          Form1.Memo1.Lines.Add('Monitoring stopped');
          monitoring := False;
          exit;
        end;
    
      // Set flag
      monitoring := True;
      MonitorButton.Caption := 'Stop';
    
      // Print flag meanings
      print_header;
    
      // Convert HostnameEdit.Text to PChar; ensure final char is null
      hostname := charArray;
      for count := 0 to 254 do
         hostname[count] := #00;
      for count := 0 to Length(HostnameEdit.Text) do
         hostname[count] := HostnameEdit.Text[count + 1];
    
      // Setup the context structure for the callback - we just use the hostname
      context.copyDescription := Nil; // Nil if unused
      context.info    := hostname;    // C pointer to user-specified block of data
      context.release := Nil;         // Nil if unused
      context.retain  := Nil;         // Nil if unused
      context.version := 0;           // Structure version always zero
    
      // Setup our callback procedure variable to be called when the reachability of
      // the target changes.
      callout := @status_callback;
    
      Try
        // Create the target from the hostname
        target := SCNetworkReachabilityCreateWithName(Nil, hostname);
    
        // Set our callback
        status := SCNetworkReachabilitySetCallback(target, callout, context);
        if(status <> true) then
          Form1.Memo1.Lines.Add('callback status err: ' + BoolToStr(status));
    
        // Install our callback on the runloop.
        status := SCNetworkReachabilityScheduleWithRunLoop(target, CFRunLoopGetCurrent, kCFRunLoopDefaultMode);
        if(status <> true) then
          Form1.Memo1.Lines.Add('schedule status err: ' + BoolToStr(status));
      Finally
        // Housekeeping
        CFRelease(target);
      End;
    end;
    
    // Allow ENTER key to trigger Monitor Button
    // Form1.KeyPreview must be set to True in code/Obj Inspector
    procedure TForm1.FormKeyPress(Sender: TObject; var Key: char);
    begin
      if(key = #13) then
        MonitorButton.Click;
    end;
    
    // Exit application
    procedure TForm1.QuitButtonClick(Sender: TObject);
    begin
      if(monitoring = True) then
        begin
          MonitorButton.Click;
          Application.ProcessMessages;
          Sleep(750);
        end;
    
      Close;
    end;
    
    end.
    

## External links

  * [Apple: SCNetworkReachabilityCreateWithName](<https://developer.apple.com/documentation/systemconfiguration/1514904-scnetworkreachabilitycreatewithn>).
  * [Apple: SCNetworkReachabilityGetFlags](<https://developer.apple.com/documentation/systemconfiguration/1514924-scnetworkreachabilitygetflags>).
  * [Apple: SCNetworkReachabilityFlags](<https://developer.apple.com/documentation/systemconfiguration/scnetworkreachabilityflags>).
  * [Apple: SCNetworkReachabilitySetCallback](<https://developer.apple.com/documentation/systemconfiguration/1514903-scnetworkreachabilitysetcallback>).
  * [Apple: SCNetworkReachabilityRef](<https://developer.apple.com/documentation/systemconfiguration/scnetworkreachabilityref>).
  * [Apple: SCNetworkReachabilityContext](<https://developer.apple.com/documentation/systemconfiguration/scnetworkreachabilitycontext>).
  * [Apple: SCNetworkReachabilityScheduleWithRunLoop](<https://developer.apple.com/documentation/systemconfiguration/1514894-scnetworkreachabilityschedulewit>).
  * [Apple: SCNetworkReachabilityUnscheduleFromRunLoop](<https://developer.apple.com/documentation/systemconfiguration/1514899-scnetworkreachabilityunschedulef>).

---

_Source: [https://wiki.freepascal.org/macOS_SCNetworkReachability_API](https://web.archive.org/web/20250122032803/https://wiki.freepascal.org/macOS_SCNetworkReachability_API)_
