# macOS Form Position Save and Restore

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Code example
  * 3 See also
  * 4 External links



## Overview

Here is a quick, almost code-free method to save the form's position and size using the Mac preferences system. 

The _setFrameAutosaveName_ method sets the preferences key name used and loads any previously-saved state. As a result, it is best to call it in a Form's OnShow event handler. There is no need to do anything else and the state of the form's position and size will be saved automatically when the form closes. 

A short example may be found below. Note that you need to give the form a unique name - here I have used Form_123 

## Code example
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms,      // needed for main form
      CocoaAll;   // needed for NSView
    
    type
    
      { TForm_123 }
    
      TForm_123 = class(TForm)
        procedure FormShow(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form_123: TForm_123;
    
    implementation
    
    {$R *.lfm}
    
    { TForm_123 }
    
    procedure TForm_123.FormShow(Sender: TObject);
    begin
      NSView(Form_123.Handle).window.setFrameAutosaveName(NSSTR(ClassName));
    end;
    
    end.
    

## See also

  * [Mac Preferences Read and Write](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write")
  * [Mac Preferences and About Menu](<Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu")
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips")



## External links

  * [Apple: setFrameAutosaveName](<https://developer.apple.com/documentation/appkit/nswindow/1419509-setframeautosavename>)

---

_Source: [https://wiki.freepascal.org/macOS_Form_Position_Save_and_Restore](https://web.archive.org/web/20230131013928/https://wiki.freepascal.org/macOS_Form_Position_Save_and_Restore)_
