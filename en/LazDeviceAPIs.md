# LazDeviceAPIs

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[한국어 (ko)](</LazDeviceAPIs/ko> "LazDeviceAPIs/ko")** │    
****

LazDeviceAPIs is a LCL unit which offers an interface to various hardware devices, such as the accelerometer, GPS positioning, SMS sending, etc. It is mostly designed for smartphone/mobile platforms, but it might eventually be ported to desktops too. 

## Contents

  * 1 Introduction
  * 2 Accelerometer
    * 2.1 Android Permissions
  * 3 PositionInfo
    * 3.1 Android Permissions
  * 4 Messaging
    * 4.1 Android Permissions
  * 5 Possible Windows implementation
  * 6 Roadmap
  * 7 See Also



  


## Introduction

LazDeviceAPIs was modeled to be similar to other APIs for hardware devices from the LCL. Historically we had two hardware global objects: Mouse and Screen, respectively for the pointing device (Mouse is also utilized for touch messages) and Screen for the display. In LazDeviceAPIs there are global objects: Accelerometer, PositionInfo, Messaging, etc. 

## Accelerometer

The Accelerometer is very easy to use. Just call Accelerometer.StartReadingAccelerometerData to start listening to the accelerometer sensor and call Accelerometer.StopReadingAccelerometerData to stop listening to it: 
    
    
    uses lazdeviceapis;
    
    procedure TForm2.btnStartAccelClick(Sender: TObject);
    begin
      Accelerometer.OnSensorChanged := @HandleAccelerometerChanged;
      Accelerometer.StartReadingAccelerometerData();
    end;
    
    procedure TForm2.btnStopAccelClick(Sender: TObject);
    begin
      Accelerometer.StopReadingAccelerometerData();
    end;
    
    procedure TForm2.HandleAccelerometerChanged(Sender: TObject);
    begin
      labelSensorData.Caption := Format('X=%f Y=%f Z=%f', [Accelerometer.xaxis,
        Accelerometer.yaxis, Accelerometer.zaxis]);
      DebugLn(labelSensorData.Caption);
    end;
    

### Android Permissions

This object requires no special permissions under Android. 

## PositionInfo

### Android Permissions

Using the PositionInfo global object in Android requires adding the following permissions to the AndroidManifest.xml file of the application, as can be seen below. 
    
    
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    

## Messaging

### Android Permissions

Using the Messaging global object in Android requires adding the following permissions to the AndroidManifest.xml file of the application, as can be seen below. 
    
    
    <uses-permission android:name="android.permission.SEND_SMS" />
    

## Possible Windows implementation

Windows 7 and up has a sensors API that gives access to sensor info (e.g. GPS); see [[[1]](<http://msdn.microsoft.com/en-us/library/windows/desktop/dd318936%28v=vs.85%29.aspx>)]. Interested developers may be able to use those APIs to provide LazDeviceAPIs functionality. 

## Roadmap

LazDeviceAPIs is currently implemented only for LCL-CustomDrawn-Android 

## See Also

  * [Custom Drawn Interface/Android](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")
  * [Talk:LazDeviceAPIs](</Talk:LazDeviceAPIs> "Talk:LazDeviceAPIs")

---

_Source: [https://wiki.freepascal.org/LazDeviceAPIs](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/LazDeviceAPIs)_
