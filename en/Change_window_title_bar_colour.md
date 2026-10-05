# Change window title bar colour

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

For macOS 10.10 and later, the following code may be used to change the colour of a window's title bar. 
    
    
        {$modeswitch objectivec2}
         
        interface
         
        uses
          CocoaAll,
          ...;
    
        const
          NSWindowTitleVisible = 0;
          NSWindowTitleHidden = 1;
         
        type
          NSWindowTitleVisibility = NSInteger;
         
          NSWindowGlam = objccategory external (NSWindow)
            function titleVisibility: NSWindowTitleVisibility; message 'titleVisibility';
            procedure setTitleVisibility(AVisibility: NSWindowTitleVisibility); message 'setTitleVisibility:';
            function titlebarAppearsTransparent: Boolean; message 'titlebarAppearsTransparent';
            procedure setTitlebarAppearsTransparent(AFlag: Boolean); message 'setTitlebarAppearsTransparent:';
          end;
         
        { TForm1 }
         
        procedure TForm1.FormShow(Sender: TObject);
        var
          w :NSWindow;
        begin
          w := NSView(Self.Handle).window;
          w.setTitlebarAppearsTransparent(true);
          w.setTitleVisibility( NSWindowTitleHidden);
         
          w.setBackgroundColor( NSColor.greenColor ); // <-- not required, and can changed to any color
       
        ...
    
        end;
    

# See also

  * [macOS Portal](<Portal_Mac.md> "Portal:Mac")
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface")

---

_Source: [https://wiki.freepascal.org/Change_window_title_bar_colour](https://web.archive.org/web/20220707181138/https://wiki.freepascal.org/Change_window_title_bar_colour)_
