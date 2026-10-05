# Android4Pascal

│ **English (en)** │ 

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Android4Pascal are the bindings which connect Pascal applications running as executables in Android with the Android Java APIs. 

## Contents

  * 1 Download
    * 1.1 Latest development version
    * 1.2 Version 0.3
    * 1.3 Version 0.2
      * 1.3.1 Android API Hello World in Pascal
        * 1.3.1.1 Compiling the example project in Linux
    * 1.4 Version 0.1
  * 2 See also



## Download

### Latest development version

  * svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/bindings/android-sdk> android-sdk
  * svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/examples/androidlcl> androidlcl



Changelog: 

  * Added a tool to automatically generate the bindings
  * Added many new APIs (don't remember all of them)



### Version 0.3

Released June 2011 

svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/systeminfo4android/> systeminfo4android 

Changelog: 

  * Added support for reading android.util.DisplayMetrics, Activity.getWindowManager, WindowManager.getDefaultDisplay, Display.getMetrics



### Version 0.2

Released on March 2011 

This version was distributed as an example project which can be obtained with the following svn command: 

svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/turbochessclock4android/> -r 230 turbochessclock4android 

#### Android API Hello World in Pascal

Here is an example Pascal application written for Android. It shows how to create controls, receive callback events and how to use the timer. 

The full directory structure can be download with this svn command: 
    
    
     svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/turbochessclock4android> turbochessclock4android
    

[![Simple Android app.png](https://wiki.freepascal.org/images/7/7c/Simple_Android_app.png)](</File:Simple_Android_app.png>)

Here is the Pascal code from this example: 
    
    
    {
      A simple Chess Clock application
    
      Author: Felipe Monteiro de Carvalho - 2011
      License: Public Domain
    }
    program turbochessclock4android;
    
    {$mode objfpc}{$H+}
    
    uses
      Classes, SysUtils, androidpipescomm, androidview, javalang,
      androidapp, androidtimer, androidutil;
    
    type
      TEventHandler = class
      public
        procedure HandleOnTimer(ASender: TObject);
        procedure buttonStartClickCallback(v: TView);
        procedure buttonMoveClickCallback(v: TView);
      end;
    
    var
      layout: TLinearLayout;
      params: TLayoutParams;
      tv, black_label, white_label: TTextView;
      scroller: TScrollView;
      btn_move, btn_start: TButton;
      tp: TTimePicker;
      WhiteTimeCount: Integer = 0;
      BlackTimeCount: Integer = 0;
      MyTimer: TAndroidTimer;
      MyEventHandler: TEventHandler;
      IsWhitePlayerMove: Boolean = True;
    
    procedure TEventHandler.buttonStartClickCallback(v: TView);
    begin
      black_label.setVisibility(VISIBLE);
      white_label.setVisibility(VISIBLE);
      btn_move.setVisibility(VISIBLE);
      //
      WhiteTimeCount := tp.getCurrentHour() * 60 * 60 + tp.getCurrentMinute() * 60;
      BlackTimeCount := WhiteTimeCount;
      //
      MyTimer.removeCallbacks();
      MyTimer.postDelayed(100);
    end;
    
    procedure TEventHandler.buttonMoveClickCallback(v: TView);
    begin
      IsWhitePlayerMove := not IsWhitePlayerMove;
    end;
    
    procedure TEventHandler.HandleOnTimer(ASender: TObject);
    var
      lSeconds, lMinutes, lHours: Integer;
    begin
      if IsWhitePlayerMove then
      begin
        lSeconds := WhiteTimeCount mod 60;
        lMinutes := (WhiteTimeCount mod (60 * 60)) div 60;
        lHours := WhiteTimeCount div (60 * 60);
        white_label.setText(Format('White %d:%d:%d', [lHours, lMinutes, lSeconds]));
        //
        if WhiteTimeCount = 0 then Exit;
        Dec(WhiteTimeCount);
      end
      else
      begin
        lSeconds := BlackTimeCount mod 60;
        lMinutes := (BlackTimeCount mod (60 * 60)) div 60;
        lHours := BlackTimeCount div (60 * 60);
        black_label.setText(Format('Black %d:%d:%d', [lHours, lMinutes, lSeconds]));
        //
        if BlackTimeCount = 0 then Exit;
        Dec(BlackTimeCount);
      end;
      //
      MyTimer.removeCallbacks();
      // Note that this has a low precision, but it enough for this simple app
      // A more precise app would keep another time count instead of just using Dec
      // at each timer call
      MyTimer.postDelayed(1000);
    end;
    
    begin
      // Here add any initialization.
      // Any initialization code will be run inside Activity.onCreate,
      // so keep it as short as possible!
      // It should mostly contain GUI initialization
      // User interface
      MyEventHandler := TEventHandler.Create;
    
      // Prepares the UI of the program
      layout := TLinearLayout.Create;
      params := TLayoutParams.Create(androidview.FILL_PARENT, androidview.FILL_PARENT);
      layout.setLayoutParams(params);
      params.Free;
      layout.setOrientation(androidview.VERTICAL);
    
      // Game UI
    
      black_label := TTextView.Create;
      black_label.setText('Black time:');
      black_label.setVisibility(GONE);
      black_label.setTextSize(COMPLEX_UNIT_PX, 40);
      layout.addView(black_label);
    
      white_label := TTextView.Create;
      white_label.setText('White time:');
      white_label.setVisibility(GONE);
      white_label.setTextSize(COMPLEX_UNIT_PX, 40);
      layout.addView(white_label);
    
      btn_move := TButton.Create;
      btn_move.setText('Move finished!');
      btn_move.setOnClickListener(@MyEventHandler.buttonMoveClickCallback);
      btn_move.setVisibility(GONE);
      layout.addView(btn_move);
    
      // Config UI
    
      tv := TTextView.Create;
      tv.setText('Please select how much to give to the players and press "Start Game":');
      layout.addView(tv);
    
      tp := TTimePicker.Create;
      tp.setIs24HourView(True);
      tp.setCurrentHour(0);
      tp.setCurrentMinute(30);
      layout.addView(tp);
    
      btn_start := TButton.Create;
      btn_start.setText('Start game!');
      btn_start.setOnClickListener(@MyEventHandler.buttonStartClickCallback);
      layout.addView(btn_start);
    
      // And also allow the user to scroll the UI if it is larger then the screen width
      // Scrolling takes place only horizontally
    
      scroller := TScrollView.Create;
      scroller.addView(layout);
    
      Activity.setContentView(scroller);
    
      MyTimer := TAndroidTimer.Create;
      MyTimer.OnTimer := @MyEventHandler.HandleOnTimer;
    
      // Now tell Java that the initialization has finished
      vAndroidPipesComm.onCreateFinished();
      // Here you can add any other initialization,
      // specially non-GUI code
    
      // Now we block our execution waiting for callbacks from Java
      vAndroidPipesComm.MessageLoop();
    end.
    

##### Compiling the example project in Linux

1> Get a working arm-linux cross-compiler which generates ARMv5 eabi with softfloat (as many phones like HTC Wildfire have no FPU) 

To do this one can download an unofficial build of FPC 2.5.1 from here: 

<http://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/> Compiled for ARMv5 eabi with softfloat. 

Or an older FPC 2.4.2 

<http://members.yline.com/~tom_at_work/fpc-2.4.2.UNOFFICIAL.arm-linux.tar> Compiled for ARMv5 eabi with softfloat. 

Or build your own. There are instructions here: [Setup_Cross_Compile_For_ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")

2> Install the Android SDK. Instructions here: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones#Using_the_Android_SDK](<Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones")

3> Install ant, for example in Mandriva Linux: 
    
    
     urpmi ant
    

4> Open the project turbochessclock/turbochessclock4android.lpi in Lazarus and build it 

5> Build the APK file in debug mode 
    
    
     ant debug
    

6> Connect your phone and make sure you can connect to it via ADB. More info here: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones#Recognition_of_devices_under_Linux](<Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones")

7> Install the APK file in your phone via ADB or whatever other method you prefer: 
    
    
     ../android-sdk-linux_x86/tools/adb install android/bin/TurboChessClock4Android-debug.apk
    

If the package is already installed you need need to do this instead: 
    
    
     ../android-sdk-linux_x86/tools/adb uninstall com.pascal.turbochessclock
     ../android-sdk-linux_x86/tools/adb install android/bin/TurboChessClock4Android-debug.apk
    

### Version 0.1

Released on January 2011 

This version was distributed as an example project which can be obtained with the following svn command: 

svn co <https://p-tools.svn.sourceforge.net/svnroot/PascalNotes4Android/> -r 230 PascalNotes4Android 

## See also

  * [Android Interface/Native Android GUI](<Android_Interface/Native_Android_GUI.md> "Android Interface/Native Android GUI")
  * [Android Programming](<Android_Programming.md> "Android Programming")

---

_Source: [https://wiki.freepascal.org/Android4Pascal](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Android4Pascal)_
