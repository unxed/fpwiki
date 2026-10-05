# macOS User Notifications

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 User notification unit
  * 2 How to use the unit
    * 2.1 Examples
  * 3 Preferences > Notifications
  * 4 External links



[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The NSUserNotification class was deprecated in macOS 10.14 (Mojave); the code still works in macOS 10.15 (Catalina); in macOS 11.0 (Big Sur) and in macOS 12 (Monterey), the notification daemon ignores the action and logs "error usernoted Denying message 3 from connection <LegacyConnection identifier: com.company.project1 type: Application>". The replacement appears to be the UserNotifications framework introduced in macOS 10.14. See below for Apple documentation.

## User notification unit

This is the user notification unit which can be used by an application to deliver notifications to the user: 
    
    
    unit UserNotification;
    
    interface
    
    {$ifdef Darwin}
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework Foundation}
    
    uses
      Classes,
      MacOSAll,
      CocoaUtils,
      CocoaAll;
    
    const
      NSUserNotificationActivationTypeNone = 0;
      NSUserNotificationActivationTypeContentsClicked = 1;
      NSUserNotificationActivationTypeActionButtonClicked = 2;
    
    type
      NSUserNotificationPtr = ^NSUserNotification;
      NSUserNotificationCenterPtr = ^NSUserNotificationCenter;
      NSUserNotificationCenterDelegateProtocolPtr = ^NSUserNotificationCenterDelegateProtocol;
    
      NSUserNotificationActivationType = NSInteger;
      NSUserNotificationActivationTypePtr = ^NSUserNotificationActivationType;
    
      NSUserNotification = objcclass external (NSObject)
      private
        _internal: id;
      public
        procedure setTitle(newValue: NSString); message 'setTitle:';
        function title: NSString; message 'title';
        procedure setSubtitle(newValue: NSString); message 'setSubtitle:';
        function subtitle: NSString; message 'subtitle';
        procedure setInformativeText(newValue: NSString); message 'setInformativeText:';
        function informativeText: NSString; message 'informativeText';
        procedure setActionButtonTitle(newValue: NSString); message 'setActionButtonTitle:';
        function actionButtonTitle: NSString; message 'actionButtonTitle';
        procedure setUserInfo(newValue: NSDictionary); message 'setUserInfo:';
        function userInfo: NSDictionary; message 'userInfo';
        procedure setDeliveryDate(newValue: NSDate); message 'setDeliveryDate:';
        function deliveryDate: NSDate; message 'deliveryDate';
        procedure setDeliveryTimeZone(newValue: NSTimeZone); message 'setDeliveryTimeZone:';
        function deliveryTimeZone: NSTimeZone; message 'deliveryTimeZone';
        procedure setDeliveryRepeatInterval(newValue: NSDateComponents); message 'setDeliveryRepeatInterval:';
        function deliveryRepeatInterval: NSDateComponents; message 'deliveryRepeatInterval';
        function actualDeliveryDate: NSDate; message 'actualDeliveryDate';
        function isPresented: boolean; message 'isPresented';
        function isRemote: boolean; message 'isRemote';
        procedure setSoundName(newValue: NSString); message 'setSoundName:';
        function soundName: NSString; message 'soundName';
        procedure setHasActionButton(newValue: boolean); message 'setHasActionButton:';
        function hasActionButton: boolean; message 'hasActionButton';
        function activationType: NSUserNotificationActivationType; message 'activationType';
        procedure setOtherButtonTitle(newValue: NSString); message 'setOtherButtonTitle:';
        function otherButtonTitle: NSString; message 'otherButtonTitle';
      end;
    
    
      NSUserNotificationCenter = objcclass external (NSObject)
      private
        _internal: id;
      public
        class function defaultUserNotificationCenter: NSUserNotificationCenter; message 'defaultUserNotificationCenter';
        procedure setDelegate(newValue: NSObject); message 'setDelegate:';
        function delegate: NSUserNotificationCenterDelegateProtocol; message 'delegate';
        procedure setScheduledNotifications(newValue: NSArray); message 'setScheduledNotifications:';
        function scheduledNotifications: NSArray; message 'scheduledNotifications';
        procedure scheduleNotification (notification: NSUserNotification); message 'scheduleNotification:';
        procedure removeScheduledNotification (notification: NSUserNotification); message 'removeScheduledNotification:';
        function deliveredNotifications: NSArray; message 'deliveredNotifications';
        procedure deliverNotification (notification: NSUserNotification); message 'deliverNotification:';
        procedure removeDeliveredNotification (notification: NSUserNotification); message 'removeDeliveredNotification:';
        procedure removeAllDeliveredNotifications; message 'removeAllDeliveredNotifications';
      end;
    
      NSUserNotificationCenterDelegateProtocol = objcprotocol external name 'NSUserNotificationCenterDelegate' (NSObjectProtocol)
      optional
        procedure userNotificationCenter_didDeliverNotification (center: NSUserNotificationCenter; notification: NSUserNotification); message 'userNotificationCenter:didDeliverNotification:';
        procedure userNotificationCenter_didActivateNotification (center: NSUserNotificationCenter; notification: NSUserNotification); message 'userNotificationCenter:didActivateNotification:';
        function userNotificationCenter_shouldPresentNotification (center: NSUserNotificationCenter; notification: NSUserNotification): boolean; message 'userNotificationCenter:shouldPresentNotification:';
      end;
    
      NSUserNotificationCenterAlwaysShowDelegate = objcclass(NSObject)
      public
        function shouldPresentNotification ({%H-}center: NSUserNotificationCenter; {%H-}notification: NSUserNotification): boolean; message 'userNotificationCenter:shouldPresentNotification:';
      end;
    
    
    // Procedure to call - without sound
    procedure DeliverUserNotification(const Title, Subtitle, Message: string);
    
    // Procedure to call - with sound
    procedure DeliverUserNotification(const Title, Subtitle, Message, Sound: string);
    
    
    var
      NSUserNotificationDefaultSoundName: NSString { available in 10_8, NA }; cvar; external;
    
    {$endif}
    
    implementation
    
    {$ifdef Darwin}
    
    var
      UserNotificationCenterDelegate: NSUserNotificationCenterAlwaysShowDelegate;
    
    
    function NSUserNotificationCenterAlwaysShowDelegate.shouldPresentNotification (center: NSUserNotificationCenter; notification: NSUserNotification): boolean;
    begin
      Result := True;
    end;
    
    
    procedure DeliverUserNotification(const Title, Subtitle, Message: string);
    begin
      DeliverUserNotification(Title, Subtitle, Message, CFStringToStr(CFStringRef(NSUserNotificationDefaultSoundName)));
    end;
    
    
    procedure DeliverUserNotification(const Title, Subtitle, Message, Sound: string);
    var
      Notification: NSUserNotification;
      TmpNSStr: NSString;
    
    begin
      Notification := NSUserNotification.alloc.init;
    
      try
        TmpNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, Title, kCFStringEncodingUTF8));
    
        try
          Notification.setTitle(TmpNSStr);
    
        finally
          TmpNSStr.release;
        end;
    
        TmpNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, Subtitle, kCFStringEncodingUTF8));
    
        try
          Notification.setSubtitle(TmpNSStr);
    
        finally
          TmpNSStr.release;
        end;
    
        TmpNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, Message, kCFStringEncodingUTF8));
    
        try
          Notification.setInformativeText(TmpNSStr);
    
        finally
          TmpNSStr.release;
        end;
    
        if Sound <> '' then
        begin
          TmpNSStr := NSString(CFStringCreateWithPascalString(kCFAllocatorDefault, Sound, kCFStringEncodingUTF8));
    
          try
            Notification.setSoundName(TmpNSStr);
    
          finally
            TmpNSStr.release;
          end;
        end;
    
        NSUserNotificationCenter.defaultUserNotificationCenter.deliverNotification(Notification);
    
      finally
        Notification.release;
      end;
    end;
    
    
    initialization
      UserNotificationCenterDelegate := NSUserNotificationCenterAlwaysShowDelegate.alloc.init;
      NSUserNotificationCenter.defaultUserNotificationCenter.setDelegate(UserNotificationCenterDelegate);
    
    
    finalization
      NSUserNotificationCenter.defaultUserNotificationCenter.setDelegate(nil);
      UserNotificationCenterDelegate.release;
    
    {$endif}
    
    end.
    

## How to use the unit

You can deliver notifications to the user: 

  * With the default notification sound, call: _procedure DeliverUserNotification(const Title, Subtitle, Message: string);_


  * With a custom notification sound, call: _procedure DeliverUserNotification(const Title, Subtitle, Message, Sound: string);_


  * With no notification sound, call: _procedure DeliverUserNotification(const Title, Subtitle, Message, Sound: string);_ and pass an empty Sound string.



### Examples
    
    
    ...
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      DeliverUserNotification('Title Test', 'Subtitle Test', 'Message', '');   // No sound
    end; 
    
    ...
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      DeliverUserNotification('Title Test', 'Subtitle Test', 'Message', 'Submarine'); // Custom sound from Sound Preferences
    end; 
    
    ...
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
      DeliverUserNotification('Title Test', 'Subtitle Test', 'Message'); // Default Notification Alert sound
    end; 
    
    ...
    

## Preferences > Notifications

Be aware that in this user-centric world that is macOS, the user has the ability to override many of your carefully crafted notification attributes via the Notifications Preference pane. This includes the ability to turn notifications from your application off completely by choosing "None" and unchecking all the checkboxes. 

[![macOS Prefs Notifications.png](https://wiki.freepascal.org/images/a/a2/macOS_Prefs_Notifications.png)](</File:macOS_Prefs_Notifications.png>)

  


## External links

  * [Apple: NSUserNotification](<https://developer.apple.com/documentation/foundation/nsusernotification>) \- deprecated in macOS 10.14 (Mojave).
  * [Apple: UNUserNotificationCenter](<https://developer.apple.com/documentation/usernotifications>) \- debuted in macOS 10.14 (Mojave).

---

_Source: [https://wiki.freepascal.org/macOS_User_Notifications](https://web.archive.org/web/20240910144127/https://wiki.freepascal.org/macOS_User_Notifications)_
