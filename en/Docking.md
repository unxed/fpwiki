# Docking

## Contents

  * 1 Overview
    * 1.1 Docking Managers
    * 1.2 Not only forms
    * 1.3 Splitters
    * 1.4 Drag and Drop
    * 1.5 Save/Restore
    * 1.6 Usage
    * 1.7 See also



## Overview

There exist two different flavors of docking. The Delphi compatible DragDock model is based on dedicated target controls (DockSites), into which other controls or forms can be docked by dragging them with the mouse. The Lazarus specific AnchorDocking model allows to glue forms together by other means. 

Docked forms or controls can be undocked again, of course, and a constructed layout can be stored for later restauration, like the IDE window layout. 

### Docking Managers

The central instance to determine how a control is docked to others is the **docking manager**. For example the Delphi default docking manager (TDockTree) allows to insert a dropped control relative to an already docked control. User supplied docking managers can implement other docking models and layouts, e.g. for constructing forms and dialogs, flowcharts, motherboards, electrical circuits, city plans or other diagrams. Every such docking manager provides visual feedback to the user, signaling how a dragged control will be placed into the managed DockSite. The free Delphi package _DockPanel_ uses the Align property, hidden panels and TPageControls to allow nested layouts and even page docking. In the Lazarus sources there are two docking implementations 

  * [Anchor Docking](<Anchor_Docking.md> "Anchor Docking")
  * [EasyDockingManager](<EasyDockingManager.md> "EasyDockingManager")



### Not only forms

Docking is not limited to forms in the LCL. It can dock and undock any control. When a non-windowed control is undocked, a form is automatically created and the control is put onto it. What type of form is created is defined by the function GetFloatingDockSiteClass. When a control is undocked the LCL automatically creates this class and add the control as child. 

### Splitters

Some docking managers automatically add splitters between the docked controls so the user can still resize the controls. The LCL provides TSplitter with some extended features like anchors, that Delphi does not have. This allows for very flexible layouts without hidden panels. 

### Drag and Drop

Some docking managers allows to dock forms via drag and drop. The dragging is implemented in the LCL and the docking managers can control the details. Some platforms like MS Windows sends drag events when dragging the title bar, others like Linux do not. Therefore docking managers under Linux require to add drag areas to each dockable form. These drag areas are often called _dock headers_. Dragging can start automatically via the DragKind and DragMode properties or manually via the DragManager.DragStart method. 

### Save/Restore

Some docking managers allow to save the current layout and restore it later. How this is done is totally up to the docking packages. The LCL provides no framework for save/restore layouts of multiple forms. 

Most docking packages have methods to save/restore the whole window layout of an application. That means they save all opened forms, their bounds and nested states and can restore that. Some have also a dynamic restore. For example imagine three forms docked together: 
    
    
    +-----++-----+
    |Form1||Form2|
    |     |+-----+
    |     |+-----+
    |     ||Form3|
    +-----++-----+
    

Now imagine the application is restarted and only Form1 and Form2 have been created at start. The layout is restored somehow. For example by expanding Form2's height, and requesting an instance of Form3. When no such instance is supplied immediately, it may or may not be docked into its previous place, when created later. 

### Usage

Some docking managers can only dock special form descendants, so to make a form dockable it must descend from such a class or must be put onto one. Because under Linux you must add a drag area, both existing dock managers wrap the dockable controls into docksites and add a drag area. 

### See also

  * [Anchor Docking](<Anchor_Docking.md> "Anchor Docking")
  * [Build custom dock manager](<Build_custom_dock_manager.md> "Build custom dock manager") \- Tutorial
  * [EasyDockingManager](<EasyDockingManager.md> "EasyDockingManager")
  * [DockedFormEditor](<DockedFormEditor.md> "DockedFormEditor") \- IDE implementation for form docked next to source
  * [MultiDoc](<MultiDoc.md> "MultiDoc") \- replacement for standard MDI interface
  * [Manual Docker](<Manual_Docker.md> "Manual Docker") \- this Lazarus-IDE extension allows Messages window to dock to the source editor

---

_Source: [https://wiki.freepascal.org/Docking](https://web.archive.org/web/20250523134858/https://wiki.freepascal.org/Docking)_
