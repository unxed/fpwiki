# Cocoa Internals/Input

## Contents

  * 1 LCL Event Handling
  * 2 Mouse
    * 2.1 Implementation
    * 2.2 Ctrl+Left Click / Right Click
    * 2.3 Capture
    * 2.4 Scrolling Wheel
  * 3 Keyboard
    * 3.1 Virtual Key code
    * 3.2 Special Keys
    * 3.3 Major Patch
    * 3.4 Dead Keys
  * 4 See Also



## LCL Event Handling

For general "event loop" handling you might want to refer to [application event loop](<Application.md> "Cocoa Internals/Application") article. 

## Mouse

The biggest difference between Apple and LCL ideology is the way of handling MouseMove events. 

According to Apple - mouse events can easily flood the event queue and thus are disabled by default, unless a user presses and holds the button. 

Apple provides a special NSWindow mode to allow MouseMove events to come through even with buttons unreleased. However, such MouseMove events would be reported to the **focused** controls (aka FirstResponder), rather than the control immediately under the cursor. 

(MouseDown, MouseUp events are reported directly to the control under the cursor, similar to the LCL). 

Because of that, the Widgetset mouse event processing routine first checks if cursor is actually above the focused control, and if it's not, it would let Cocoa propagate the mouse event, down (from child to parent) through the controls hierarchy. 

Such propagation happens, when the NSView inherited mouseMove event is called. 

If the actual control is found, the propagation of the event is stopped. 

### Implementation

The whole set of methods _should be_ overridden for each sub-classed Objective-C control: 
    
    
        function acceptsFirstMouse(event: NSEvent): Boolean; override;
        procedure mouseDown(event: NSEvent); override;
        procedure mouseUp(event: NSEvent); override;
        procedure rightMouseDown(event: NSEvent); override;
        procedure rightMouseUp(event: NSEvent); override;
        procedure rightMouseDragged(event: NSEvent); override;
        procedure otherMouseDown(event: NSEvent); override;
        procedure otherMouseUp(event: NSEvent); override;
        procedure otherMouseDragged(event: NSEvent); override;
        procedure mouseDragged(event: NSEvent); override;
        procedure mouseMoved(event: NSEvent); override;
        procedure scrollWheel(event: NSEvent); override;
    

Even if a target LCL control doesn't publish the events (eg TScrollBar doesn't have any mouse events published), all those methods should still be overriden. The purpose is to give LCL control over that. For example, if a control is used at design time, ALL of the Cocoa default handling should be blocked. 

The typical implementation for mouseDown/mouseUp events (including "right" and "other" versions) should look like this: 
    
    
    procedure TCocoaControl.mouseDown(event: NSEvent);
    begin
      if not Assigned(callback) or not callback.MouseUpDownEvent(event) then
        inherited mouseDown(event);
    end;
    
    procedure TCocoaControl.mouseUp(event: NSEvent);
    begin
      if not Assigned(callback) or not callback.MouseUpDownEvent(event) then
        inherited mouseUp(event);
    end;
    

Such an implementation notifies the LCL first (via callback) about a mouse event. If the LCL doesn't block it (by returning "true" from MouseUpdownEvent) the event is passed further to Cocoa. Actually there's no harm in non-blocking mouseUp and always inheriting it. 

However, for some controls, when either implementing drag and drop functionality or some sort of dragging action (eg ScrollBar) mouseDown must be implemented in the following way: 
    
    
    procedure TCocoaScrollBar.mouseDown(event: NSEvent);
    begin
      if not Assigned(callback) or not callback.MouseUpDownEvent(event) then
      begin
        inherited mouseDown(event);
     
        callback.MouseUpDownEvent(event, true); // forced mouse-up event
      end;
    end;
    

The reason for that is due to the Cocoa (and Apple in general) implementation of mouse-hold button loops. Once inherited mouseDown is called, the procedure would not return until the mouse button is released or the action is cancelled. It's quite typical to do an event processing loop, until mouse-up event received. 

The issue is that the actual mouseUp() selector would never be called, and must be emulated via a callback.MouseUpDownEvent(, true) call. 

The implementation of the mouseUp method should still remain. 

Also, this approach doesn't apply to rightMouseDown and otherMouseDown. Apple doesn't make any event loops for those. 

### Ctrl+Left Click / Right Click

Apple's classic mouse is a single button mouse. The additional actions are performed by holding the mouse button down. (One might observe the similar behavior for smartphone devices, where holding a finger on the screen for a certain period of time opens up an additional action). 

However, context menus are also part of the user interface. To open a context menu a user would need to hold the "Control" button down and click the mouse button. 

With the modern mouse device (where having 3 buttons+scroll wheel is the norm) it ended up with `Ctrl+Left` being recognised as a Right click. 

The Cocoa WidgetSet recognizes the `Ctrl+Left` click as a Context Menu request only and forwards an appropriate message (LM_CONTEXTMENU) to the control which ends in a call to the OnContextPopup event. Yet, Cocoa doesn't alternate the information about left-click in any manner. (The Carbon WidgetSet reports the Button as TRightButton, but the shift state indicates the that Left mouse button is actually pressed). 

### Capture

Only 1 handle (control) can have the mouse captured. This is typically happening on MouseDown and the capture is released on MouseUp. 

There are corresponding WidgetSet methods available: 
    
    
    GetCapture 
    SetCapture
    ReleaseCapture
    

There are a few tricks. 

  * most of the Cocoa controls are doing "mouse loop" capture themselves for the MouseDown key. (Mouse events are still propagated to the LCL as if the loop is not happening)
  * if a control is disabled by an EnableWindow() function call, the Capture MUST BE released. This is not documented in WinAPI, yet this is how it's working. (see #36491)
  * if a control is destroyed with DestoryHandle, the Capture MUST BE released as well.



The captured control Handle is stored in the **CaptureControl** (FCaptureControl) property of **TCocoaWidgetSet**. 

The code at **TLCLCommonCallback.MouseUpDownEvent** checks for the **CaptureControl** to be assigned. And if it's assigned then it would pass the mouse event to it and would prevent the Cocoa control from processing the event. 

### Scrolling Wheel

Scrolling is different between LCL (WinAPI) approach and macOS (Cocoa or Carbon) 

  * LCL - reports the HARDWARE mouse wheel scroll events (in 120-based deltas). The values ARE NOT a subject to the system configuration of "Number of Lines per scroll" (SPI_GETWHEELSCROLLLINES).
  * Cocoa - reports wheel scrolls as adjustment to lines. (how many lines should be scrolled... and it doesn't matter if there are any kind of lines in the control or what's the size of lines. Just logical lines). The amounts of lines is adjusted by "Scrolling Speed" setting (and thus can be different depending on the setting for the same physical scroll of a line).



    scrolling speed can be acquired from either Preferences dialog or via command line:
    
    
    defaults read .GlobalPreferences com.apple.scrollwheel.scaling
    

    in runtime:
    
    
    NSUserDefaults.standardUserDefaults.doubleForKey(NSSTR('com.apple.scrollwheel.scaling'));
    

Cocoa would increase "wheel" delta if the wheel is scrolled fast enough. WinAPI would continue to report 120. 

Both WinAPI and Cocoa would "accumulate" the delta, if main thread is working slow. Thus the next wheel event might bring a larger delta. 

If the hardware allows, WinAPI delta would be different than 120. Cocoa might also start report different amounts. 

## Keyboard

The KeyDown event is handled through the NSWindow sendEvent method. There's explicit code written to pass an event to the focused control (firstResponder). 

### Virtual Key code

The LCL uses the concept of Virtual Key codes. 

macOS APIs don't follow such a concept. Instead Cocoa is using "characters" for this purpose. For example: Menu Short Cuts or special Key Combinations are typically specified by characters, rather than some special codes. For each key pressed, macOS does provide a hardware key code (available through event.keyCode). The key represents a hardware code. Meaning that on a different keyboard layout the keyCode would remain the same, while character might be different. (For example for QWERTY vs Dvorak vs AZERTY layout, the same "q" key pressed, would produce the same keyCode, yet different character). 

For this particular reason, Virtual Keycodes are determined with the following approach: 

  * the character (event.characterWithoutModifiers) is examined. If it's available and it can be mapped to a known VirtualKey code - then character to VK would be used and non-numpad key (i.e. "/" on the keyboard is different than "/" on number keypad)
  * otherwise the Virtual Key code is determined based on keyCode.



It covers the most common cases. A problem remains for national characters (i.e. German [ß](<https://en.wikipedia.org/wiki/%C3%9F>)). The character itself is not within the range of VK_(a-z) codes. It's encoded as VK_OEM_xx code. However such a VK code would be different on Cocoa and Windows. 

### Special Keys

Some controls wants special keys, such as Tabs and Arrow (Cursor) keys. For example TMemo with WantsTab set to true, or TTrackBar to change thumb position. 

Both tabs and arrow keys are used for switching focus within the application itself. 

Thus, it's critical for a control to let the LCL know that it will handle those keys so that focus is not switched. 

For this purpose an additional method has been introduced: 
    
    
    lclExpectedKeys:::
    

This method accepts variable 3 parameters: 

  * wantTabs (default value false), should be set to true, if the control expects to process tab key
  * wantArrows (default value false), should be set to true, if the control expects to process arrow keys (all of them!)
  * wantAllKeys (default value true?)... not sure how it's being used.



### Major Patch

See <https://bugs.freepascal.org/view.php?id=35449> by Zoë Peterson 

Reference to <https://asciiwwdc.com/2010/sessions/145> (video: 

    HD: <https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2010__hd/session_145__key_event_handling_in_cocoa_applications.mov>)

In short, Cocoa's key handling looks like this: 

    1\. NSApplication.sendEvent (overridable) 

    2\. Local NSEvent Monitors (local event monitor can change the resulting event. NSEvent. addLocalMonitorForEventsMatchingMask)
    3\. If modifierFlags include Cmd or Ctrl, then Cocoa tries to perform KeyEquivalent first. 

    3.1 NSWindow.performKeyEquivalent (Key window) 

    NSResponder.performKeyEquivalent (called on all views recursively)
    3.2 NSWindow.performKeyEquivalent (Other "active" windows, but only for views that manage menus)
    3.3 NSMenu.performKeyEquivalent
    4\. Check registered hot keys ("app command" keys) (e.g., Cmd~ for Window cycling or Ctrl+F2 to move focus to menu)
    5\. NSWindow.sendEvent (Key window) 

    5.1 NSResponder.keyDown (firstResponder) 

    inherited calls keyDown up the responder chain until it reaches the NSWindow
    5.2 NSWindow.keyDown 

    5.2.0 <undocumented magic> but for Control+\
    5.2.1 NSWindow.performKeyEquivalent (Key Window, possibly for second time if Cmd or Ctrl)
    5.2.3 NSMenu.performKeyEquivalent (again, possibly the second time)
    5.2.4 <undocumented magic>
    5.2.5 Beep

### Dead Keys

By default Cocoa event handling ignores dead-keys (on national keyboards, eg Spanish). For the event to be ready, NSTextInputContext needs to be created and events "translated" through it. 

The context exists for any NSTextView field(s). Thus native Cocoa TMemo, TEdit or TcomboBox would not be affected. Custom controls however are affected. 

In order to "translate" a keyboard event TCocoaApplication creates its own (global) NSTextInputContext and passes all events through it. In order to recognize a dead-key combination. Turning the sequence of 
    
    
    ´ e
    

into 
    
    
    é
    

However, if a sequence is invalid, eg: 
    
    
    ´ s
    

Cocoa reports a single string of character "'s". The Cocoa Widget Set breaks the string into separate characters and reports them as standalone characters, rather than the entire string. (This matches the WinAPI.) 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Input](https://web.archive.org/web/20240910190838/https://wiki.freepascal.org/Cocoa_Internals/Input)_
