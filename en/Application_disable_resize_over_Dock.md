# Application disable resize over Dock

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

On macOS it's often needed to disable app window's resize below OS Dock. But Lazarus forms always resize below it. How to disable this resize? 

1) Create TForm.OnConstrainedResize handler for your form. Note, it was not "published" in LCL until LCL 1.7. Create it by hands: 
    
    
      TfmMain = class(TForm)
        ... 
      private
        ...
        procedure FormConstrainedResize(Sender: TObject; var MinWidth, MinHeight,
          MaxWidth, MaxHeight: TConstraintSize);
        ...
      end;
    
    procedure TfmMain.FormCreate(Sender: TObject);
    begin
      ..
      Self.OnConstrainedResize:= @FormConstrainedResize; 
      ..
    end;
    

2) Write code in handler to change MaxHeight, value must be workarea height minus current form's Top. 
    
    
    procedure TfmMain.FormConstrainedResize(Sender: TObject; var MinWidth, MinHeight,
      MaxWidth, MaxHeight: TConstraintSize);
    const
      cMinHeight = 200;
    var
      RWork: TRect;
    begin
      {$ifndef darwin} exit; {$endif}
      RWork:= Screen.PrimaryMonitor.WorkareaRect;
      MaxHeight:= Max(cMinHeight, RWork.Bottom - RWork.Top - Top);
    end;
    

3) Note: handler has side effect. On dragging window by its caption, handler is called too, so window will be resized on moving it too low, near the Dock. 

## See also

  * [Bounce Application Icon in Dock](<Bounce_Application_Icon_in_Dock.md> "Bounce Application Icon in Dock")
  * [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")
  * [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu")
  * [Show Badge on Application Icon in Dock](<Show_Badge_on_Application_Icon_in_Dock.md> "Show Badge on Application Icon in Dock")

---

_Source: [https://wiki.freepascal.org/Application_disable_resize_over_Dock](https://web.archive.org/web/20240907020930/https://wiki.freepascal.org/Application_disable_resize_over_Dock)_
