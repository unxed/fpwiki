# LCL Drag Drop

│ **English (en)** │  **[français (fr)](</LCL_Drag_Drop/fr> "LCL Drag Drop/fr")** │  **[русский (ru)](<../ru/LCL_Drag_Drop.md> "LCL Drag Drop/ru")** │    
****

## DoDi's Guide to Dragging, Dropping and Docking

Controls in a GUI can be grabbed with the mouse, dragged to other locations, and dropped onto other controls or onto the desktop. When the dragged control is moved to a different location, the operation is called ["docking"](<LCL_Drag_Dock.md> "LCL Drag Dock"). Otherwise the source and target control can exchange information or interact in any other way, what's called a "drag-drop" operation. 

But all that does not work by itself, an application must set up the allowed operations so that the LCL knows what to do with every control in a GUI. Sophisticated operations require further code for managing the visual feedback to the application user, and what will happen when the dragged control finally is dropped. 

For simple docking support in entire applications you can use either [EasyDockingManager](<EasyDockingManager.md> "EasyDockingManager") or [Anchor Docking](<Anchor_Docking.md> "Anchor Docking"), that reduces the required adaptations to a minimum. 

## Contents

  * 1 DoDi's Guide to Dragging, Dropping and Docking
  * 2 Drag Drop
    * 2.1 The Drag Source
    * 2.2 The Drag Object (TDragObject defined in unit controls)
    * 2.3 Events
      * 2.3.1 OnStartDrag/Dock
      * 2.3.2 OnDrag/DockOver
      * 2.3.3 OnDrag/DockDrop
      * 2.3.4 OnEndDrag/Dock
  * 3 Major Shortcomings
  * 4 Delphi
    * 4.1 Drag Drop
      * 4.1.1 The Drag Source
      * 4.1.2 The Drag Object (TDragObject defined in unit controls)
      * 4.1.3 LCL Modifications
    * 4.2 Helper Functions
      * 4.2.1 General Helpers
      * 4.2.2 Special Helpers
    * 4.3 Events
      * 4.3.1 OnStartDrag/Dock
      * 4.3.2 OnDrag/DockOver
      * 4.3.3 OnDrag/DockDrop
      * 4.3.4 OnEndDrag/Dock
    * 4.4 Docking specific Events
      * 4.4.1 OnGetSiteInfo
      * 4.4.2 OnUnDock
  * 5 LCL Implementation
    * 5.1 DragManager
    * 5.2 Drag Performers
  * 6 Delphi Message Sequence Chart or UML Sequence Diagram
  * 7 See also



## Drag Drop

This is the simple and originally only dragging action. The players are: 

  * A draggable source control in the GUI
  * A drag object, derived from TDragObject
  * Helper functions
  * Events



Now let's have a look at how these are working together. 

### The Drag Source

A control can be made draggable by setting its **DragMode** property to **dmAutomatic**. When the user presses the left mouse button over such a control, the mouse pointer changes into an dragging indicator until the user releases the button, or cancels the operation by pressing the Escape key. The indicator now follows the mouse movements and changes its shape, according to what will happen when the control is dropped in the current location. 

Dragging also can be started in code, by calling _SourceControl_.**BeginDrag**. In either case the helper procedure _DragManager.DragStart_ is called to start dragging. The **DragManager** is a global instance implementing the actual dragging. It is initialized with the default _TDragManagerDefault_ and can be replaced by the programmer (You). Depending on the control's **DragKind** a drag or a dock operation is started. The source control triggers an **OnStartDrag** event, where the programmer can provide a custom drag object. If no such object is provided, a default object (_TDragControlObject_ or _TDragDockObject_) is created. Internal data structures for visual feedback and more are initialized. 

### The Drag Object (TDragObject defined in unit controls)

A drag object receives all user input while dragging. In the default implementation it reacts by calling either DragMove or finally DragStop. 

The drag object also is responsible for the visual feedback: 

  * **GetDragCursor** returns the mouse pointer shape.
  * **GetDragImages** returns a list of drag images, attachable to the mouse pointer.
  * **ShowDragImage** and **HideDragImage** implement the visual feedback.



The **EndDrag** method notifies the source control when dragging stops. In case of a cancel'd operation it invokes _SourceControl_.**DragCanceled** , followed by _SourceControl_.**DoEndDrag** in either case, which raises an **OnEndDrag** event. 

### Events

#### OnStartDrag/Dock

This event occurs when the operation starts. The handler can provide a custom TDragObject or TDragDockObject for the operation. 

#### OnDrag/DockOver

This event notifies a possible target, when an object is dragged over it. The handler can signal to accept or reject a drop. 

The event signals not only mouse moves, but also when the mouse enters or leaves a target control. 

An OnDockOver event occurs on the DockSite, not on the target control under the mouse. The handler can signal to deny an drop, and can adjust the visual feedback. The event occurs when the mouse enters or leaves a control, and with every single movement of the mouse. 

#### OnDrag/DockDrop

This event notifies the target control of the end of a drag operation. 

#### OnEndDrag/Dock

This event notifies the source control of the end of a drag operation. 

## Major Shortcomings

  * The same flaws as Delphi's VCL.
  * No LCL framework (cross-platform) support for dragging from an LCL application to any other application. For this to work, LCL needs support for the XDND protocol under X11, OLE Drag-n-Drop support under Windows and Mac Drag Manager under macOS.
  * No LCL framework (cross-platform) support for receiving drops inside an LCL application from another application (desktop, file manager, etc). The OnDragDrop event handler always expects the Source of the drop to come through as a TObject descendant which will **not** be the case when dragging an object from another application to a LCL application.



## Delphi

### Drag Drop

This is the simple and originally only dragging action. The players are: 

  * A draggable source control in the GUI
  * A drag object, derived from TDragObject
  * Helper functions
  * Events



Now let's have a look at how these are working together, based on the original Delphi implementation for clarity. The LCL implementation is somewhat different in the use of helper objects, details will be discussed later. 

#### The Drag Source

A control can be made draggable by setting its **DragMode** property to **dmAutomatic**. When the user presses the left mouse button over such a control, the mouse pointer changes into an dragging indicator until the user releases the button, or cancels the operation by pressing the Escape key. The indicator now follows the mouse movements and changes its shape, according to what will happen when the control is dropped in the current location. 

Dragging also can be started in code, by calling source.BeginDrag. In either case the helper procedure DragInitControl is called to start dragging. The source control raises an StartDrag event, where the application can provide a custom drag object. If no such object is provided, a default object is created. Internal data structures for visual feedback and more are initialized. Then DragTo is called to show that dragging has started. 

#### The Drag Object (TDragObject defined in unit controls)

A drag object receives all user input while dragging. In the default implementation it reacts by calling either DragTo or finally DragDone. 

The drag object also is responsible for the visual feedback: 

  * GetDragCursor returns the mouse pointer shape.
  * GetDragImages returns a list of drag images, to be attached to the mouse pointer.
  * ShowDragImage and HideDragImage implement the visual feedback.



The Finished method notifies the source control when dragging stops. In case of a cancel'd operation it invokes source.DragCanceled, followed by source.DoEndDrag in either case, which raises an EndDrag event. 

#### LCL Modifications

The current LCL implementation has removed input processing from the drag object, so that the application can no more customize the UI. And the LCL implementation is far from being okay :-( 

Also the drag object is bypassed in the presentation of the visual feedback. 

### Helper Functions

Helper functions primarily allow for access to protected properties and methods of the controls and other classes, from outside the Controls unit. They also can perform common tasks, and can encapsulate platform specific and other implementation details. 

#### General Helpers

**DragTo** (Delphi only, not present in LCL) determines the current drag target, sends notifications to the involved controls, and manages the visual feedback. It also handles the delayed start of a dragging operation, i.e. can wait for the mouse moving away far enough from the starting point. 

**DragDone** (Delphi only, not present in LCL) terminates dragging, either with a drop or cancel of the operation. 

**CancelDrag** (Delphi only, do not confuse with the LCL global procedure CancelDrag) is protected against false calls, otherwise it defers further actions to DragDone. 

#### Special Helpers

Some more public helper procedures exist, which are undocumented in Delphi and are not required for customized dragging operations. 

**DragFindWindow** searches for a drag target window (platform specific). 

**DragInit** initializes the internal management, like mouse capture, and the visual feedback. 

**DragInitControl** creates the drag object, then calls DragInit. 

**SetCaptureControl** handles the mouse capture. 

### Events

#### OnStartDrag/Dock

This event occurs when the operation starts. The handler can provide a custom DockObject for the operation. 

#### OnDrag/DockOver

This event notifies a possible target, when an object is dragged over it. The handler can signal to accept or reject an drop. 

The event signals not only mouse moves, but also when the mouse enters or leaves a target control. 

An OnDockOver event occurs on the DockSite, not on the target control under the mouse. The handler can signal to deny an drop, and can adjust the visual feedback. The event occurs when the mouse enters or leaves a control, and with every single movement of the mouse. 

#### OnDrag/DockDrop

This event notifies the target control of the end of a drag operation. 

#### OnEndDrag/Dock

This event notifies the source control of the end of a drag operation. 

### Docking specific Events

(this section should be moved to docking topic) 

#### OnGetSiteInfo

This event occurs during the search for a qualifying dock site. Multiple dock sites can overlap each other, reside in overlapping windows, or can be invisible. Every candidate must supply an influence rectangle (catching area), and can signal to reject an drop. The drag manager selects the most appropriate dock site as the target for subsequent OnDockOver events. 

#### OnUnDock

This event occurs when the dragged control has to be removed from its host dock site. 

It's unclear what should happen when the handler denies to undock the control. 

## LCL Implementation

The Delphi implementation of dragging has many flaws, in detail with regards to docking. Until now nobody seems to understand the details of the Delphi dragging model, most code was modeled after the VCL and consequently implements exactly the same misbehaviour. This section, and more important the docking specific page, shall shed some light on the dragging model in general and the severe design and implementation flaws in the Delphi implementation. 

The current LCL implementation uses a DragManager object and drag/dock performers instead of helper functions. While the Delphi model must be provided for compatibility reasons, the introduction of a DragManager class and object (singleton) allows for the implementation of an alternative drag manager, with improved behaviour, and allows an application to select the most appropriate manager. 

### DragManager

This object handles the captured input, provides helper methods, and stores dragging and docking parameters. 

It's questionable whether the drag object shall not be allowed to handle user input. It makes the system safer, but disallows applications to extend the user interface for specific dragging operations. 

The helper methods require different calls, but this is harmless as long as they are only called from LCL components. 

More critical is the storage of dragging parameters in the DragManager object, which vanish when a different drag manager is installed. This is fatal in case of the list of registered dock sites :-( 

### Drag Performers

The internal use of different classes for dragging and docking is okay in so far, as the procedures differ significantly between drag-drop and drag-dock operations. Nonetheless common actions should use common code, e.g. inherited from the common base class. 

## Delphi Message Sequence Chart or UML Sequence Diagram

Provided by Tom Verhoeff 

Something along the lines (this is for Delphi; view with monospaced font): 
    
    
                      Main            Drag          (Optional)        Drag
      User            Event          Source           Drag           Target
     (Mouse)          Loop           Control          Object         Control
        =               =               =                               =
        |               |               |                               |
       1|--MouseDown--->|               |                               |
        |              2#--OnMouseDown->|                               |
        |               |              3#                               |
        |              4#<-BeginDrag()--#                               |
        |               #               #                               |
        |               |               |                               |
        |              5#--OnStartDrag->|                               |
        |               #              6#----Create---->=               |
        |               |               #               |               |
       7|--MouseMove--->|               |               |               |
        |              8#-----------------------------------OnDragOver->|
        |               #               |               |              9#
        |               #               |               |               #
        |               |               |               |               |
      10|---MouseUp---->|               |               |               |
        |             11#-----------------------------------OnDragDrop->|
        |               #               |               |             12#
        |               #               |               |               #
        |               #---OnEndDrag-->|               |               |
        |               |             13#----Destroy--->=               |
        |               |               |                               |
    

  
The DragObject is optional. There could be multiple candidate targets. The Main Event Loop determines which event to send to which object, depending on the user action, the cursor location, and the current state of the GUI. 

4 somehow triggers 5 (there is a delay depending on the Boolean parameter Immediate to BeginDrag). 

6 determines the location of the MouseDown and may use this to decide on what gets dragged (through GetCursorPos, ScreenToClient, MouseToCell). It can call SetDragImage to visualize a thing being dragged. 

The chain 7, 8, 9 can occur repeatedly; 9 indicates acceptability of a drop at this target via the Boolean var parameter Accepted. 

12 is called only if the immediately preceding OnDragOver of this target returned Accepted = True; i.e. Accepted is a precondition of 12. 

13 receives the parameter Target to know where the drop was accepted; if Target = nil, then the drop was cancelled by the user (MouseUp at an unacceptable location), and OnDragDrop was not performed; if Target <> nil, then OnDragDrop was already performed at the Target. The work associated with an actual drop is thus divided over the OnDragDrop (for actions at the target) and OnEndDrag (for actions at the source). Since a Drag Object needs to be destroyed, independent of whether the drop was successful or canceled, it is best to call Free (or Destroy) only in the OnEndDrag handler. 

Only one drag can be active at any moment; it either ends in a drop or is canceled. 

There are various other things to be included, but this is the fundamental part. (Maybe also see [Delphi-Central](<http://www.delphi-central.com/drag.aspx>) or ["Implementing drag-and-drop in controls"](<http://www.podgoretsky.com/ftp/Docs/Delphi/D5/dg/controls.html>).) 

I hope this is helpful. Tom 

## See also

  * [Drag and Drop sample](<Drag_and_Drop_sample.md> "Drag and Drop sample")

---

_Source: [https://wiki.freepascal.org/LCL_Drag_Drop](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/LCL_Drag_Drop)_
