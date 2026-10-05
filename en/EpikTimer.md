# EpikTimer

│ **[Deutsch (de)](</EpikTimer/de> "EpikTimer/de")** │  **English (en)** │  **[français (fr)](</EpikTimer/fr> "EpikTimer/fr")** │  **[русский (ru)](<../ru/EpikTimer.md> "EpikTimer/ru")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Project Status
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation in Lazarus IDE
  * 9 Usage
    * 9.1 EpikTimer as a profiler
  * 10 The ETDemo Application
  * 11 Console application
  * 12 See also



### About

EpikTimer is a programmer's stopwatch that is capable of measuring very short events with traceably high precision over long periods of time. It's simple to use, consumes virtually no CPU and requires only 25 bytes of ram to implement a timer instance. The component provides a single internal timer... but unlimited numbers can be declared externally and linked to a single EpikTimer component on the form. 

The download contains the component, an installation package and a demo application, that illustrates the features of the component along with some instrumentation for evaluating the clock sources on a given system. 

This component was designed for cross-platform applications and was written specifically for the Lazarus IDE and Free Pascal Compiler and includes a demo application. 

### Author

[Tom Lisjac](</User:VlxAdmin> "User:VlxAdmin")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>) (please contact the author if the LGPL doesn't work with your project licensing) 

### Download

The **source code** can be downloaded from the Git repository as follows: 
    
    
     git clone <https://github.com/graemeg/epiktimer.git>
    

If you don't want to install a Git client, Github also allows you to download the latest code as a ZIP archive. Simply go to the [Github EpikTimer](<https://github.com/graemeg/epiktimer>) url and click the "Download ZIP" button on the right. 

All **tagged releases** can be downloaded in .zip or .tar.gz format by going to this [Github Releases](<https://github.com/graemeg/epiktimer/releases>) url and select the zip or tar.gz link for whichever release you want to download. 

### Project Status

  * Production/Stable



### Change Log

  * Initially written on 24-06-2003 TL
  * Pre-release 30-06-2003 TL - Needs testing on the BSD's and Win32
  * Version 0.1 1-7-2003 TL


  1. initial beta release


  * Version 0.2 3-7-2003 TL


  1. Revised logic around hardware detection to prevent executing extended instructions if the HasCapabilityData call returns false.
  2. Removed exposed low level diagnositic functions from unit interface.
  3. Revised demo.


  * Version 0.3 15-11-2005


  1. Updated 0.2 version to make it compile on the latest Lazarus (0.9.10).
  2. Added LCL to the required packages.
  3. Changed mode to Delphi in order to compile.
  4. Changed windows timebase to use QueryPerformanceCounter, because has a much greater precision then GetSystemTime.
  5. Added changes to ensure the component compiles on Delphi 7.0
  6. Made tests on Windows and Linux
  7. Small changes to the demo to make it compile


  * Version 1.0 06-10-2006


  1. Changes for 64 bits operation
  2. Added units BaseUnix, Unix and UnixUtil, removed oldlinux (obsolete)
  3. Gettimeofday -> fpGettimeofday
  4. Changed systemsleep for 64 bits systems
  5. Some changes in timeval
  6. Tested on AMD64 (linux)



For any later changes, please see the Git commit log. 

### Dependencies / System Requirements

  * Nanosecond resolution is supported on Intel Pentium versions with a Timestamp Counter.
  * Microsecond system clock is the default timebase.



Tested on: 

  * Pentium IV 3,2Ghz on Windows and Linux with great precision (~ 3,220,000,000 ticks per second)
  * Pentium 233Mhz with Damn Small Linux. Works great with hardware clock.
  * Mobile AMD 64 Athlon 3200+ on Windows XP Home (32 bit), Lazarus 9.13
  * AMD 64 CPU, running 64 bit version of Ubuntu Linux, Lazarus 9.18.
  * Intel i7-3770K, running 64-bit version of FreeBSD 9.1, using FPC 2.6.2
  * ARM CPU using Raspberry Pi and FPC 2.6.2



### Installation in Lazarus IDE

  * In Components/Open Package File, open etpackage.lpk
  * Compile the package and close the package dialog
  * In Components -> Open Package File, open the etpackage_dsgn.lpk
  * Compile the package.
  * Click the Install button and let Lazarus rebuild
  * Component will be in the System Palette (stopwatch-ruler icon)



### Usage

Drop the component on a form. The component contains a single timer instance and parameterless calls to start, stop, elapsed and clear will implicitly reference it. 

**If the timer is named ET**
    
    
    uses epiktimer;
    
    var
      ET: TEpikTimer;
    
    procedure InitTimer;
    begin
      ET := TEpikTimer.Create(Application);
    end;
    
    procedure InstrumentedCall;
    Begin
      ET.Clear; // optional... timer is cleared at creation
      ET.Start;
      ExecuteFirstTimedSection;
      ET.Stop; // the timer is actually paused and can be restarted later
      TimedSection1:=ET.Elapsed; // store the elapsed in a global
      MakeAnUntimedOverheadCall; // not counted in the timer
      ET.Start; //resume the timer... continue accumulating ticks
      CallTimedSection2;
      TimedSection2:=ET.Elapsed; //timer keeps running... we've just sample it.
      CallTimedSection3;
      CallSomethingElse;
      TimedSection3:=ET.Elapsed; //keep counting... tap the elapsed
      CallTimedSection4;
      TimedSection4:=ET.Elapsed; //keep counting... tap the elapsed
      ET.clear // done... timer is stopped and zeroed
    end;
    

You can also create any number of timers from a single component on the form by declaring a TimerData record and passing it as a parameter to start, stop, elapsed and clear using the overloaded methods in the component. An example would be: 
    
    
    Function TimedExecution: Extended;
    Var 
      DiskAccessTime:TimerData;
    Begin
      ET.Clear(DiskAccessTimer); // Declared timers *must* be cleared before use. 
      ET.Start(DiskAccessTimer);
      ExecuteTheTimedSection;
      Result:=ET.Elapsed(DiskAccessTimer); // the timer keeps running...
      etc...
    

One particular use for EpikTimer is checking if a given amount of time has elapsed before continuing with a task or to another iteration of the same task. 
    
    
    DelayInSeconds := 8.5; // or any value
    OldTime := ET.Elapsed;
    
    while ((ET.Elapsed - OldTime) < DelayInSeconds) do
    begin
      ET.SystemSleep(10);
    end;
    
    // DelayInSeconds has elapsed and now you can continue executing the code
    

See etdemo.pas for additional examples of component usage 

#### EpikTimer as a profiler

EpikTimer can be used to profile the speed of procedures: 
    
    
    uses
      {$ifdef PROFILING}epiktimer,{$endif}
      ...
    
    type
      TMyClass = class
      private
        {$ifdef PROFILING}
        profiler: Tepiktimer;
        {$endif}
        ...
      end;
    
    implementation
    
    constructor TMyClass.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
    
      ...
    
      {$ifdef CARDROOM_PROFILING}
      profiler := TEpikTimer.Create(Self)
      {$endif}
    end;
    
    procedure TMyClass.MethodToBeProfiled;
    begin
      {$ifdef PROFILING}
      profiler.Clear;
      profiler.Start;
      {$endif}
      // do stuff
      {$ifdef PROFILING}
      OPDebugLn('TMyClass.MethodToBeProfiled ' + FloatToStr(profiler.Elapsed));
      {$endif}
    end;
    

### The ETDemo Application

The ETDemo application does not require EpikTimer to be installed in order to compile and operate. I never liked having to install a palette full of components only to find out that I didn't like any of them! :) 

**Installation**

  * Open etdemo.lpi
  * compile
  * run



### Console application

If you don't have an "Application" object instance as above, simply create the EpikTimer instance as follows. It must be manually freed when you are all done with it: 
    
    
     ET := TEpikTimer.Create(nil)
     ... your code here ...
     ET.Free;
    

### See also

  * [FPTimer included with FPC/Lazarus](<http://lazarus-ccr.sourceforge.net/docs/fcl/fptimer/index.html> "doc:fcl/fptimer/index.html")
  * [Yet another Running Timer](<Yet_another_Running_Timer.md> "Yet another Running Timer")

---

_Source: [https://wiki.freepascal.org/EpikTimer](https://web.archive.org/web/20250417160040/https://wiki.freepascal.org/EpikTimer)_
