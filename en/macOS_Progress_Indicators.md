# macOS Progress Indicators

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Example code
  * 3 See also
  * 4 External links



## Overview

Progress indicators can be _determinate_ or _indeterminate_. A determinate indicator displays the completion percentage of a task. An indeterminate indicator shows that the application is busy without providing a visual indication of how long the task will take. 

The NSProgressIndicator class is an interface that provides visual feedback to the user about the status of an ongoing task and is part of the AppKit framework. The class is available in all versions of macOS since Mac OS X 10.0 (Cheetah). The _style_ (spinning or bar), _sizeToFit_ (resize based on style) and _isDisplayedWhenStopped_ (hide when not active) properties were introduced in Mac OS X 10.2 (Jaguar). 

## Example code

[![macos native progress indicators.png](https://wiki.freepascal.org/images/7/71/macos_native_progress_indicators.png)](</File:macos_native_progress_indicators.png>)

The code below implements the four progress indicators annotated in the above image: the determinate progress indicators bar and circle and the indeterminate progress indicators spinner and bar2. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework appkit}
    
    interface
    
    uses
      Forms, Controls, StdCtrls,
      ExtCtrls, // for timers
      CocoaAll, // for Cocoa vars etc
      MacOSAll, // for data types eg CGFloat
      CTypes;   // for data types eg CUInt
    
    type
    
      NSProgressIndicator = objcclass external (NSView)
      private
        _isBezeled: ObjCBOOL;
        _isIndeterminate: ObjCBOOL;
        _threadedAnimation: ObjCBOOL;
        _minimum: double;
        _maximum: double;
        _value: double;
        _animationIndex: cuint;
        _animationDelay: NSTimeInterval;
        _timer: id;
        _drawingWidth: CGFloat;
        _roundColor: id;
        _reserved: id;
        __progressIndicatorFlags: bitpacked record
          case byte of
            0: (_anonBitField___progressIndicatorFlags0: cuint);
            1: (
              isSpinning: 0..1;
              isVector: 0..1;
              isLocked: 0..1;
              controlTint: 0..((1 shl 3)-1);
              controlSize: 0..((1 shl 2)-1);
              style: 0..1;
              _delayedStartup: 0..1;
              hideWhenStopped: 0..1;
              revive: 0..1;
              _temporarilyBlockHeartBeating: 0..1;
              _isHidden: 0..1;
              _isHeartBeatInstalled: 0..1;
              _customRenderer: 0..1;
              _lastFrame: 0..((1 shl 8)-1);
              _isDetaching: 0..1;
              RESERVED: 0..((1 shl 7)-1);
            );
          end;
        _NSProgressIndicatorReserved1: id;
      public
        procedure setIndeterminate(newValue: ObjCBOOL); message 'setIndeterminate:';
        function isIndeterminate: ObjCBOOL; message 'isIndeterminate';
        procedure setBezeled(newValue: ObjCBOOL); message 'setBezeled:';
        function isBezeled: ObjCBOOL; message 'isBezeled';
        procedure setControlTint(newValue: NSControlTint); message 'setControlTint:';
        function controlTint: NSControlTint; message 'controlTint';
        procedure setControlSize(newValue: NSControlSize); message 'setControlSize:';
        function controlSize: NSControlSize; message 'controlSize';
        procedure setDoubleValue(newValue: double); message 'setDoubleValue:';
        function doubleValue: double; message 'doubleValue';
        procedure incrementBy (delta: double); message 'incrementBy:';
        procedure setMinValue(newValue: double); message 'setMinValue:';
        function minValue: double; message 'minValue';
        procedure setMaxValue(newValue: double); message 'setMaxValue:';
        function maxValue: double; message 'maxValue';
        procedure setUsesThreadedAnimation(newValue: ObjCBOOL); message 'setUsesThreadedAnimation:';
        function usesThreadedAnimation: ObjCBOOL; message 'usesThreadedAnimation';
        procedure startAnimation (sender: id); message 'startAnimation:';
        procedure stopAnimation (sender: id); message 'stopAnimation:';
        procedure setStyle(newValue: NSProgressIndicatorStyle); message 'setStyle:';
        function style: NSProgressIndicatorStyle; message 'style';
        procedure sizeToFit; message 'sizeToFit';
        procedure setDisplayedWhenStopped(newValue: ObjCBOOL); message 'setDisplayedWhenStopped:';
        function isDisplayedWhenStopped: ObjCBOOL; message 'isDisplayedWhenStopped';
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        SpinnerButton: TButton;
        CircleButton: TButton;
        Bar2Button: TButton;
        StopButton: TButton;
        BarTimer: TTimer;
        CircleTimer: TTimer;
        procedure Button1Click(Sender: TObject);
        procedure Bar2ButtonClick(Sender: TObject);
        procedure CircleTimerUpdate(Sender: TObject);
        procedure SpinnerButtonClick(Sender: TObject);
        procedure CircleButtonClick(Sender: TObject);
        procedure StopButtonClick(Sender: TObject);
        procedure FormActivate(Sender: TObject);
        procedure BarTimerUpdate(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      myIndicatorBar:  NSProgressIndicator;
      myCircleBar: NSProgressIndicator;
      myIndicatorBar2:  NSProgressIndicator;
      mySpinner:  NSProgressIndicator;
      myView: NSView;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // Determinate bar
    procedure TForm1.Button1Click(Sender: TObject);
    begin
       myIndicatorBar :=  NSProgressIndicator.alloc.initWithFrame(NSMakeRect(30,20,200,20)); // bar
       myIndicatorBar.setMinValue(0);
       myIndicatorBar.setMaxValue(30);
       myIndicatorBar.setStyle(0);             // 1 = circle; 0 = bar
       myIndicatorBar.setIndeterminate(False);
       myView.addSubview(myIndicatorBar);
       BarTimer.enabled := True;
    end;
    
    // Determinate circle
    procedure TForm1.CircleButtonClick(Sender: TObject);
    begin
       myCircleBar :=  NSProgressIndicator.alloc.initWithFrame(NSMakeRect(10,30,80,80)); // circle
       myCircleBar.setStyle(1);             // 1 = circle; 0 = bar
       myCircleBar.setIndeterminate(False);
       myView.addSubview(myCircleBar);
       CircleTimer.enabled := True;
    end;
    
    // Indeterminate spinner
    procedure TForm1.SpinnerButtonClick(Sender: TObject);
    begin
       mySpinner := NSProgressIndicator.alloc.initWithFrame(NSMakeRect(130,80,40,40));
       mySpinner.setIndeterminate(True);
       mySpinner.setStyle(1);                   // 1 = spinner; 0 = bouncing bar
       mySpinner.setUsesThreadedAnimation(True);
       mySpinner.setDisplayedWhenStopped(False);
       mySpinner.startAnimation(Nil);
       myView.addSubview(mySpinner);
    end;
    
    // Indeterminate bar2
    procedure TForm1.Bar2ButtonClick(Sender: TObject);
    begin
       myIndicatorBar2 := NSProgressIndicator.alloc.initWithFrame(NSMakeRect(30,130,200,20));
       myIndicatorBar2.setIndeterminate(True);
       myIndicatorBar2.setStyle(0);                   // 1 = spinner; 0 = bouncing bar
       myIndicatorBar2.setUsesThreadedAnimation(True);
       myIndicatorBar2.setDisplayedWhenStopped(False);
       myIndicatorBar2.startAnimation(Nil);
       myView.addSubview(myIndicatorBar2);
    end;
    
    // Stop all
    procedure TForm1.StopButtonClick(Sender: TObject);
    begin
       BarTimer.Enabled := False;
       CircleTimer.Enabled := False;
       mySpinner.stopAnimation(Nil);
       myCircleBar.setDoubleValue(0);
       myIndicatorBar.setDoubleValue(0);
       myIndicatorBar2.stopAnimation(Nil);
    end;
    
    // Get the form's view once for use later
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      myView := NSView(Form1.Handle);
    end;
    
    // Update bar progress
    procedure TForm1.BarTimerUpdate(Sender: TObject);
    begin
       myIndicatorBar.incrementBy(0.1);
    end;
    
    // Update circle progress
    procedure TForm1.CircleTimerUpdate(Sender: TObject);
    begin
        myCircleBar.incrementBy(0.5);
    end;
    
    finalization
    
    // Cleanup
    myIndicatorBar.release;
    myCircleBar.release;
    mySpinner.release;
    myIndicatorBar2.release;
    
    end.
    

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_progress_indicators/>). 

## See also

  * [BGRAControls BGRAFlashProgressBar control](<Sample_Graphics.md> "Sample Graphics")
  * [KControls TKPercentProgressBar control](<KControls.md> "KControls")
  * [JVCL Components JvSpecialProgress component](<JVCL_Components.md> "JVCL Components")



## External links

  * [Apple: NSProgressIndicator](<https://developer.apple.com/documentation/appkit/nsprogressindicator>)

---

_Source: [https://wiki.freepascal.org/macOS_Progress_Indicators](https://web.archive.org/web/20241209234429/https://wiki.freepascal.org/macOS_Progress_Indicators)_
