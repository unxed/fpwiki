# LCL Drag Dock

│ **English (en)** │  **[русский (ru)](<../ru/LCL_Drag_Dock.md> "LCL Drag Dock/ru")** │    
****

Controls or entire forms in a GUI can be glued together and detached again, by dragging them around with the mouse. Such docking is similar to [drag-drop](<LCL_Drag_Drop.md> "LCL Drag Drop"), but differs in some aspects. 

## Contents

  * 1 Procedure
    * 1.1 DragInit
    * 1.2 DragTo
    * 1.3 DragDone
  * 2 Docking Elements Overview
    * 2.1 Dock Source
    * 2.2 Dock Sites
      * 2.2.1 Floating Dock Sites
    * 2.3 The DockRect
    * 2.4 The DockObject
    * 2.5 Helpers
  * 3 Work Flow
    * 3.1 Prerequisites
      * 3.1.1 RegisterDockSite
      * 3.1.2 GetSiteInfo
    * 3.2 DragInit
    * 3.3 DragTo
      * 3.3.1 Delphi Woes
    * 3.4 DragDone
      * 3.4.1 Manual Dock
      * 3.4.2 Drop into DockSite
      * 3.4.3 Drop Nowhere
      * 3.4.4 Don't Drop
  * 4 Events
    * 4.1 DragInit
    * 4.2 DragTo
    * 4.3 DragDone



## Procedure

A docking operation is organized just as any other dragging operation. 

### DragInit

On start of a docking operation the visual feedback has to be initialized, as usual. 

### DragTo

On a move a couple of steps have to be taken: 

  * Whether the user doesn't want to dock at all
  * Determination of possible target sites
  * Determination of the precise drop zone
  * Visual feedback



### DragDone

On a drop, several reasons can block docking, It's hard to justify why a dock site should deny to undock the dragged control when it finally is dropped. The following cases can occur: 

  * The control is docked into the new dock site.
  * The dragged control becomes floating 
    * as is, if it was already floating, or when it can float for itself (is a TWinControl),
    * or wrapped into a new floating form.
  * The entire operation is aborted.



## Docking Elements Overview

In addition to the drag-drop players, some more players enter the scene: 

  * Dock sites
  * Dock rectangles as visual feedback
  * Dock managers



The introduction of dock managers opened a can of worms, with poor integration into the unmanaged docking model. Let's ignore ["managed" docking](</index.php?title=LCL_Managed_Docking&action=edit&redlink=1> "LCL Managed Docking \(page does not exist\)") for now, and consider only "unmanaged" docking. 

### Dock Source

A control or form can be made dockable by setting its **DragKind** property to **dkDock**. 

### Dock Sites

Docking requires special drop target zones, called dock sites. A TWinControl becomes a docking target by setting its **DockSite** property to True. 

Docked controls reside in a distinct member array, separated from other components on the container control. For proper behaviour it's highly recommended to have no other controls in a dock site initially, and populate it only with docked controls. 

#### Floating Dock Sites

Another kind of dock sites are used for undocked (floating) controls. When a control is dropped outside a dock site, and it is not a TWinControl, a new form is created for it at the drop location. When the floating control later is docked into a dock site, the floating form is destroyed. 

An application can create other forms for floating controls, so that e.g. multiple controls can be docked into and out of such a form. 

### The DockRect

As in drag-drop operations, a rectangular shape follows the mouse pointer in a docking operation. But the docking rectangle can change it's size and position dynamically, to signal the position and size of the control when it is dropped in the current place. 

When no accepting dock site could be found, or when the user denies an drop by holding down the Ctrl key, the DockRect reflects the floating size and position of the dragged control. The floating size is initialized to the original size of the dragged control, and is updated when the control is floated. Otherwise the dock site can determine the placement and size of the dropped control. 

The Delphi implementation remembers the coordinates of a drawn DockRect, in EraseDockRect, so that the old frame can be removed from the screen by an XOR paint, whenever required. 

### The DockObject

A special DockObject is used in a docking operation, derived from TDragObject. It's unclear why this class introduces new methods, instead of overriding the existing virtual methods for the different visual feedback. 

The inherited DragTarget is the target dock site in docking. The added DropOnControl and DropAlign properties indicate where and how the dragged control shall be positioned relative to an already docked control. 

### Helpers

A bunch of helper methods and fields has been introduced into the TControl and TWinControl classes, for the internal management of docked controls and the customization of the docking process. Most of these helpers play a dedicated role in the process of docking, and will be described in the work flow below. 

(placeholder for descriptions of general items) 

## Work Flow

This is the detailed description of the required actions, before, during, and after a control is docked. 

### Prerequisites

A control is made dockable by setting its DragKind to dkDock. Nothing else is required for a dock source. 

A global list holds all dock sites in an application. Dock sites are added and removed from this list, when their DockSite property changes. 

#### RegisterDockSite

Dock sites can be covered partially or entirely by other forms, but shall act as docking targets in any case. This procedure registers and unregisters controls in an application as dock sites, so that even hidden candidates can be found while dragging a dock source over the screen. 

#### GetSiteInfo

The TWinControl.GetSiteInfo method returns an influence (catching) rectangle, associated with the dock site. This convention allows to e.g. hide empty dock sites in the GUI, without making them unreachable for dropping. The default implementation returns the visible control extent, increased by an certain amount in either direction. 

### DragInit

Docking starts automatically when a control has DragKind=dkDock and DragMode=dmAutomatic, and the left mouse button is pressed on that control. A TControl provides a BeginAutoDrag method for this case, and a BeginDrag method for an programmatical start when DragMode=dmManual. 

A DockObject is created, either by the OnDockStart handler of the source control (TControl.DoStartDock), or by the drag manager when no such object was provided. The DockRect is initialized to the control's rectangle. For a convenient position of the DockRect during moves, the offset of the mouse pointer to the DockRect is remembered. 

The input capture is moved to the control, or to it's TWinControl parent, when the control cannot receive system (Windows) messages. 

When dragging shall start immediately, DragTo is invoked. 

### DragTo

DragTo checks for an delayed start, and does nothing when the mouse move threshold was not yet reached. 

Otherwise a drag target is searched. When the target changes, dmDragLeave and dmDragEnter are sent to the target by DoDragOver, as CM_DRAG messages. 

A dmDragMove message is sent the same way and is handled in TWinControl.DockOver, which positions the DockRect and invokes the OnDockOver handler. The result indicates whether a drop will be accepted, and the visual feedback is updated accordingly. 

#### Delphi Woes

The next step in the Delphi implementation is bogus. When dropping is denied, the DockRect is reset to the floating position and extent of the dragged control, what already should have been done before. In addition a DropOnControl is searched in the dock site, and the DropAlign is determined. Neither of these values is reflected in the previously determined DockRect, so that the intended effect becomes visible at best (if ever) after the next mouse move. When no DropOnControl is found, what's very likely in an unmanaged dock site, the DockRect will cover the entire dock site - what's not very meaningful. When a DropOnControl is found, the meaning of the DropAlign is undefined; the Delphi implementation uses this value to adjust the DockRect to the upper (left...) half of the DropOnControl, but such a placement of the dropped control only can make sense, when the DropOnControl itself is shrinked accordingly. In a managed dock site the DockManager can adjust the DockRect, and later can fit the affected controls into the indicated boundaries, but he has no chance to determine a DropOnControl or DropAlign himself. Thus the meaning of DropOnControl and DropAlign is questionable, both for managed and unmanaged dock sites. 

The best solution will omit that step entirely, and let the dock site and event handlers manage the visual feedback and final drop, or leaves all that to the installed DockManager. 

### DragDone

When dragging is finished, either by a cancel or successful drop, the capture is released and the visual feedback is removed from the screen. The further procedure depends on the success of the operation. 

#### Manual Dock

The TControl.ManualDock method bypasses all user interaction, and docks the control into a dock site or makes it float (TControl.ManualFloat). If required by further processing of the request, a DockObject has to be created and initialized from the parameters. 

#### Drop into DockSite

In a first step the control is logically moved into the new dock site, unless it's re-docked within the same dock site. It is removed from its old dock site's control list and moved into the new site's list of docked controls, and its Parent is adjusted accordingly. 

Finally the control is placed within its new Parent, according to the last DockRect. In a managed dock site the DockManager can rearrange the docked controls as required. 

#### Drop Nowhere

The dragged control becomes floating. A form can float for itself, but for convenience all controls can be wrapped into an dedicated floating form container (default: FloatingDockSiteClass = TCustomDockForm). 

A CM_FLOAT message is sent to the source control. When the control is already floating, it repositions it's Parent, and everything is done. 

Otherwise the the control sets the DragTarget to a newly created floating dock site, and subsequently is docked into that site as on any other target site. 

#### Don't Drop

The source control receives a DragCanceled notification, that's all. 

## Events

The following events occur during a docking operation: 

### DragInit

The **StartDrag/Dock** handler can return a custom Drag/DockObject. 

### DragTo

When docking, the registered dock sites receive a **GetSiteInfo** event. The handlers can provide the influence rectangle, and whether a drop is acceptable. 

When the target changes, the old and new targets receive a **Drag/DockOver** event of state _dmDragEnter/Leave_. 

In all cases a **Drag/DockOver** event of state _dmDragMove_ occurs. The handler can reject an drop, and can adjust the visual feedback in the given drag/dock object. 

### DragDone

A final **Drag/DockMove** of state _dmDragLeave_ is sent to the target. This is the first event in a programmatical drop (TControl.ManualDock). The state better should be dmDragMove, but the Delphi implementation uses dmDragLeave. 

When docking, an **UnDock** event allows the source site to reject (cancel) the drop. The event handler, dock site and docking manager are asked in sequence to reject or perform the appropriate internal action. 

When the drop is not rejected, a **Drag/DockDrop** event occurs. 

In all cases the source receives an **EndDrag/Dock** event.

---

_Source: [https://wiki.freepascal.org/LCL_Drag_Dock](https://web.archive.org/web/20250217100127/https://wiki.freepascal.org/LCL_Drag_Dock)_
