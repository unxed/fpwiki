# macOS Gestures

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Usage Example
  * 3 Author
  * 4 License
  * 5 Download
  * 6 External links



## Overview

The Gestures unit provides convenient Free Pascal classes for handling macOS gestures. 

Currently only the magnification gesture is implemented, but others can be implemented easily in a similar way. 

## Usage Example

Let's assume your scalable control has a Scale property and you want to allow an end user to scale it by using the typical trackpad gesture. Here is the code: 
    
    
    uses
      Gestures;
    
    type
      TForm_Main = class(TForm)
      private
        Gesture: TMagnificationGesture;
        InitialScale: Double;
      end;
    
    procedure TForm_Main.FormCreate(Sender: TObject);
    begin
      Gesture := TMagnificationGesture.Create(Self);
      Gesture.Control := MyScalableControl;
      Gesture.OnGesture := @MagnificationGestureGesture;
    end;
    
    procedure TForm_Main.MagnificationGestureGesture(Sender: TMagnificationGesture;
      State: TGestureState; Magnification: Double);
    begin
      case State of
        gsBegan:
          InitialScale := MyScalableControl.Scale;
    
        gsChanged:
          if Magnification > 0 then
            MyScalableControl.Scale := InitialScale * (1 + Magnification)
          else
            MyScalableControl.Scale := InitialScale / (1 - Magnification);
      end;
    end;
    

## Author

Yuri Plashenkov 

## License

This package is licensed under the [MIT license](<https://github.com/plashenkov/macOS-gestures-FPC/blob/main/LICENSE.md>). 

## Download

The Gestures unit may be downloaded from [GitHub](<https://github.com/plashenkov/macOS-gestures-FPC>). 

## External links

  * [Apple: Trackpad gestures](<https://support.apple.com/en-au/HT204895>)
  * [Apple: Gestures](<https://developer.apple.com/documentation/appkit/gestures>)

---

_Source: [https://wiki.freepascal.org/macOS_Gestures](https://web.archive.org/web/20241207123343/https://wiki.freepascal.org/macOS_Gestures)_
