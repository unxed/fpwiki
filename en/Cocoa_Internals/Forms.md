# Cocoa Internals/Forms

Forms are based on NSWindow (or NSPanel) + a special sub-class of NSView (TCocoaContentWindow) in order to provide the ability of embedding forms one into another. 

## Contents

  * 1 Cocoa WS Classes
    * 1.1 TCocoaWindowContent
    * 1.2 TCocoaWindowContentDocument
    * 1.3 TCocoaWindow
    * 1.4 TCocoaPanel
  * 2 Icons
  * 3 See Also



## Cocoa WS Classes

### TCocoaWindowContent

This is an descendant of NSScrollView (TCocoaScrollView). (since after 2.0.2, in a prior versions it was a descendant from TCocoaCustomControl). 

It follows the conventional approach, that a control HANDLE is the view that corresponds to the view's coordinates (size and placement). 

Also a Window itself should have scrollbars. 

### TCocoaWindowContentDocument

(since after 2.0.2) 

This is a descendant of TCocoaCustomControl, that's always embedded in TCocoaWindowContent. 

Note that the default setting TCocoaWindowContentDocument is to automatically resize towards the size of TCocoaWindowContent. 

### TCocoaWindow

lclOwnWindow
    the method returns the current owning window (NSWindow), if the content is not embedded into another NSView. Otherwise returns nil.

### TCocoaPanel

Currently the only LCL class that's using it is HintWindow. Though infact it could also be implemented via TCocoaWindow (just with a special flags set) 

## Icons

A form icon is based on **NSWindowDocumentIconButton** \- a special form button that is used to show the document icon. The button is automatically generated and placed by Cocoa, if NSWindow has representedURL assigned (to a non-nil value). The URL doesn't have to point to an existing location. If it points to a non-existing location a button with a generic icon would still be generated. 

If representedURL is not assigned, then the DocumentIcon button _could be_ created manually by using **[standardWindowButton:forStyleMask:](<https://developer.apple.com/documentation/appkit/nswindow/1419173-standardwindowbutton?language=objc>)** method. However, the placement of the button should also be handled manually (every time the title changes). As well as both the title and the button would not be aligned in the same manner as if they were automatically with representedURL assigned. 

Since an automatically managed representedURL icon provides a better user experience LCL assigns representedURL to the bundle URL. (The bundle is likely to exist, since the application has been launched). 

Keep in mind that Cocoa design guidelines suggest the Icon defines a type of the document opened in the Window. Where in LCL the icon typically defines the Application itself. That icon use contradiction is left to the LCL developer to resolve. 

The LCL behavior can be turned off globally for a project by enabling the **CocoaIconUse** variable: 
    
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1
      { you can add units after this }
    
      // CocoaIconUse is a global variable defined in CocoaConfig (in CocoaInt before 3.0)
      {$ifdef LCLCocoa}, CocoaConfig{$endif}
      ;
    
    {$R *.res}
    
    begin
      {$ifdef LCLCocoa}
      CocoaConfig.CocoaIconUse:=true;
      {$endif}
      RequireDerivedFormResource:=True;
      Application.Scaled:=True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Forms](https://web.archive.org/web/20240910073653/https://wiki.freepascal.org/Cocoa_Internals/Forms)_
