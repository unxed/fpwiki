# Cocoa Internals/Widgetset

## Contents

  * 1 Handles
    * 1.1 Handles Reference
  * 2 Focusing
    * 2.1 Need of Window
    * 2.2 Ignoring System Setting
    * 2.3 Focus Ring
  * 3 Cursors
  * 4 User Prompts and Modal Dialogs
  * 5 See Also



## Handles

THANDLE - an opaque reference value that's being generated during the .CreateHandle() call of a particular Widgetset class. THANDLE is the value that's used to interface with a control. Using the value a widgetset recognizes a particular control. 

Most of the Widget interface methods accept TWinControl as a parameter. It's assumed that the Handle property of the control would be used to back reference the actual control. The property stores the value received from the CreateHandle() call made earlier. The HandleAllocated property can be checked in order to know if handle has been previously created or not. If handle has not been created it's expected that the method can fail (returning the proper failure information. Exceptions are not expected?). 

Overall LCL Handles refer to complex controls. For example, HANDLE can refer to a Memo control that's capable of editing AND scrolling text as needed. 

Cocoa's composite engine operates controls on a lower level scope. With the Memo example, there's not a single control that does both: text editing and scrolling. Instead there are two separate controls: NSTextView and NSScrollView (both are descendants of NSView). But LCL expects only one value to be returned as a handle. 

The common rule is the following: the outermost bounds (aka container) control is returned as the handle. Thus in case of Memo, two NSViews are allocated: NSTextView and NSScrollView. But the outermost NSScrollView is returned as theHANDLE. All TCustomMemoWSControl methods are aware of that and treat the handle as such. For example: 

  * if paste action is required, the handle would be used as **NSScrollView** and then it's **documentview** would be used to get **NSTextView to call for** paste**.**
  * if bounds changes are required (moving, resizing) then the sizes are changed directly for the NSView referenced by HANDLE. (It's assumed that the view would automatically resize its controls. In case of TMemo, NSScrollView would adjust size of NSTextView, if wordwrapping is off).
  * Forms (TForm), that generally correspond to NSWindow, also return their handle as NSView. That view represents the root NSView of a form. However, CocoaWS API's know that and call the necessary NSWindow methods where applicable.



HandleViews are stored within Callback object that's being created for each CreateHandle() method. 

### Handles Reference

  * Window handle (HWND) is always NSView. 
    * TCustomWSForm it is content NSView.
    * Any control that has scroll bars (i.e. TCustomWSList) the it its the embedding NSScrollView (TCocoaScrollView)



The table below gives a reference of LCL controls and their implementing classes. Note that the LCL handle returned is not always the same as the implementing class. This applies to controls that have to embedded in a scrollbox. 

LCL Control  | WS Part  | Cocoa Class (NS class)  | View Returned as LCL Handle  | Notes   
---|---|---|---|---  
TScrollBar  | Std  | TCocoaScrollBar (NSScrollBar)  | _self_ |   
TGroupBox  | Std  | TCocoaGroupBox (NSBox)  | _self_ | There's also a content view created to hold all the child controls within the box   
TComboBox  | Std  | TCocoaComboBox (NSComboBox) or TCocoaReadOnlyComboBox (NSPopUpButton)  | _self_ | The actual type is defined by the style of LCL combobox (readonly vs editable)   
TListBox  | Std  | TCocoaTableView (NSTableView)  | NSScrollView  | The behavior of TListbox is defined by the callback object TLCLListBoxCallback assigned to TCocoaTableView.   
TEdit  | Std  | TCocoaTextField (NSTextField)  | _self_ | TCocoaTextField itself is readonly static field. The editing capabilities are implemented by TCocoaFieldEditor, which is driven by AppKit "editing" schema   
TMemo  | Std  | TCocoaTextView (NSTextView)  | NSScrollView  | Lines property is implemented via TCocoaMemoStrings   
TButton  | Std  | TCocoaButton (NSButton)  | _self_ |   
TCheckBox  | Std  | TCocoaButton (NSButton)  | _self_ | Checkbox is still a button but with a different Bezel   
TToggleBox  | Std  | TCocoaButton (NSButton)  | _self_ |   
TRadioButton  | Std  | TCocoaButton (NSButton)  | _self_ | Radiobutton is still a button but with a different Bezel   
TStaticText  | Std  | TCocoaTextField (NSTextField)  | _self_ | This field doesn't allow editor to be enabled   
[TTrackBar](<../TTrackBar.md> "TTrackBar") | Com  | [TCocoaSlider](<Buttons.md> "Cocoa Internals/Buttons") (NSSlider)  | _self_ |   
  
## Focusing

That has important impact of FOCUS-ing. Cocoa doesn't operate the term "focused" instead it's using terms "firstRepsonder" and "key" view/window. Generally focused control corresponds to Cocoa's "firstResponder". The NSResponder object (typically NSView) is the first object that would handle key or mouse events. However, this is not the case for composed controls. TMemo is a combination of NSScrollView and NSTextField. NSScrollView is used as a handle. If, NSTextField would be the firstRepsonder, CocoaWS needs to return NSScrollView as focused HANDLE. 

The same approach is applicable in reverse. If SetFocus is made for the HANDLE that represents NSScrollView with NSTextField inside. NSTextField should actually become Cocoa's firstResponder. 

The expected order of events is: 

  * LM_KILLFOCUS
  * LM_SETFOCUS



Note that unlike WinAPI WM_KILLFOCUS, LM_KILLFOCUS doesn't provide the information about "the next suggested" control to be focused. For controls that have some sort of editors (i.e. TTreeView with a node editor (TEdit) active) such approach is causing to switch focus to TForm (the hosting form), for the next focus control selection. 

The focus switch notification is handled in TCocoaWindow makeFirstResponder call. Due to problems with the recursive calls of changing the focus, the current order of events is: 

  * LM_SETFOCUS
  * LM_KILLFOCUS



(for Cocoa WS) 

#### Need of Window

The "firstResponder" is a property of a window. Thus a view can become a "firstResponder" only if it's within NSWindow's view hierarchy. This is important for complex controls, such as a TabControl. It manages multiple child views (each view for a tab). Only one view is visible at a time. All over views are actually DETACHED from NSWindow hierarchy. (while from LCL perspective they are still children of TForm). When switching between tabs a notification about a successful switch should only be delivered when a new view is in the Window hierarchy. 

#### Ignoring System Setting

LCL basic logic is to focus every control that is a tab stoppable (which is every TWinControl by default) user. Where in macOS there's a system setting of **Full Keyboard Access** (which is off by default). If the setting is off then macOS acts similar to what LCL does. If it's on only "keyboard input" controls are "tab-reachable" text fields (including editable combo-boxes) and lists. Buttons, scrollers are skipped from tabbing. LCL doesn't currently support limited tabbing. 

#### Focus Ring

Focus Ring is not drawn for controls with **BorderStyle** set to **bsNone**. (that allows to visually "hide" controls) 

If global variable **CocoaHideFocusNoBorder** is set to false, then the focus remains unchanged. 

## Cursors

Widgetset cursors are based on **NSCursor** class. Which very nice and convenient to use. Cocoa however, doesn't provide some of the cursors that do exist in windows/LCL. For example: diagonal resize. Diagonal cursors are generated by rotation of the horizontal cursors. 

## User Prompts and Modal Dialogs

Most of the dialogs are based on **NSModal** class. 

With the switch of rendering engine to layered drawing (effective macOS 10.14), an attempt to show a modal dialog during rendering (or any other stage of animation. I.e. "Activation") would cause an exception: 
    
    
    Application Specific Information:
    *** Terminating app due to uncaught exception 'NSGenericException', reason: '-[NSAlert runModal]
    may not be  invoked inside of transaction begin/commit pair, or inside of transaction commit 
    (usually this means it was invoked inside of a view's -drawRect: method.)'
    

However, if the error dialog is shown as a "sheet" then the error doesn't occur. 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Widgetset](https://web.archive.org/web/20240909162939/https://wiki.freepascal.org/Cocoa_Internals/Widgetset)_
