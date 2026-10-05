# Cocoa Internals/Scroll

## Contents

  * 1 Classes
    * 1.1 TCocoaScrollView
    * 1.2 TCocoaManualScrollView
    * 1.3 TCocoaManualScrollHost
    * 1.4 TCocoaScrollBar
  * 2 Mouse Wheel
  * 3 See Also



## Classes

The following classes are introduced to be used by LCL 

### TCocoaScrollView

Cocoa provides a native control, named **NSSrollView** to implement scrolling. Cocoa-WS sub-classes it with the **TCocoaScrollView** class. 

However, NSScrollView is designed to work strictly with the child control (documentView) size (similar to TScrollBox). 

Thus NSScrollView cannot be used to implement scrolling for CustomControls, where custom scroll settings could be used. (For example [SynEdit](<../TSynEdit.md> "TSynEdit") is using lines for vertical scrolls, not pixels) 

Instead, NSScrollView is used for standard controls, such as TListBox and TMemo to implement scrolling. For those controls, direct control of the scrolling Range via SetScrollInfo() might not work at all. Though setting a scroller position would still work. 

### TCocoaManualScrollView

This is an additional view that was introduced to satisfy LCL requirements (despite of it's name, there's no NSManualScrollView in Cocoa). 

The view provides a basic API to add scrollbars for a view. Showing or hiding scroll bars would adjust the content of the view accordingly (by resizing/redrawing its client rectangle). 

Changing the position of the scroll bar doesn't affect the contents of the view directly (unlike with NSScrollView). Instead a notification is sent to the LCL control and it should properly respond to it (by redrawing its contents). 

### TCocoaManualScrollHost

The is a descendant from TCocoaScrollView. The intent of this control is to embed **TCocoaManualScrollView** (only) and be able to provide a system-native border [Issue #34761](<https://bugs.freepascal.org/view.php?id=34761>). 

No other view or control is capable of drawing the border. **NSBox** control can draw the border (and it's used for TGroupBox), but BorderType property has been deprecated since macOS 10.14. So far NSScrollView is the only option. 

### TCocoaScrollBar

The control is aimed to implement TScrollBar that's a stand-alone scrollbar (not abound to LCL's controls SetScrollInfo) 

The object is currently used in TCocoaManualScrollView. 

..tbd.. 

The follow properties are used: 

  * minInc - the integer value of the minimum value of the scrolling range
  * maxInc - the integer value of the maximum value of the range
  * pageInc - the integer value for the page. (used for GetScrollInfo function).
  * largeInc - increment, if the blank scroll space is clicked. (note if ALT is held, the thumb jumps to the point of click). If it's set to zero, then pageInc value is used
  * smallInc - increment, if the scrolling arrow was clicked (this UI element was removed from macOS 10.x??)



## Mouse Wheel

NSScrollView suppresses mouse wheel (scrollWheel) events at all times. No matter, if there are any scrollers visible or not. For this particular reason **TCocoaManualScrollHost** simply passes scrollWheel to the next responder in the chain, as it's created only to provide a border for controls. 

There's no common method in NSScrollView to implement handling of LM_HSCROLL or LM_VSCROLL methods. The LCL scroll can appear for two reasons: 

  * via mouse wheel
  * using UI scrollbars



Either method should generate corresponding LM_xSCROLL messages. 

For this reason a bounds change notification is processed in TCocoaScrollView. The notification is sent whenever mouse wheel or scroll bars are used. 

However, if mouse wheel is used, the notification comes before Scrollers are updated with the new values. For this reason the notification handler, forces updates of the scrollers, prior to sending the notification to the LCL. 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Scroll](https://web.archive.org/web/20240910135749/https://wiki.freepascal.org/Cocoa_Internals/Scroll)_
