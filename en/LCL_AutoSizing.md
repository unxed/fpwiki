# LCL AutoSizing

│ **English (en)** │  **[русский (ru)](<../ru/LCL_AutoSizing.md> "LCL AutoSizing/ru")** │    
****

## Contents

  * 1 Overview / Terminology
  * 2 Properties
  * 3 Order
    * 3.1 Overview
    * 3.2 Algorithm for Align and Anchors
    * 3.3 DoAutoSize
    * 3.4 DisableAutoSizing/EnableAutosizing
    * 3.5 Differences from Delphi
  * 4 FAQ



## Overview / Terminology

This page explains the LCL auto sizing algorithm. AutoSizing means here: Automatic resizing and repositioning of LCL controls. 

If you are merely interested in how to use the AutoSize properties, see [Autosize / Layout](<Autosize_/_Layout.md> "Autosize / Layout"). 

## Properties

The following properties defines the behavior of the LCL autosizing: 

  * Current Left,Top,Width,Height,ClientWidth,ClientHeight
  * Loaded Left,Top,Width,Height,ClientWidth,ClientHeight
  * AutoSize
  * Anchors
  * AnchorSides
  * Align
  * BorderSpacing
  * ChildSizing
  * Constraints



For Delphi compatibility the LCL supports the method AdjustClientRect which is an extension to the ChildSizing.LeftRightSpacing/TopBottomSpacing properties. 

## Order

### Overview

The autosizing works in several phases: 

  * DisableAutosizing. In the first phase the application changes properties, creates or deletes controls. The whole form is locked. No Autosizing is done, no bounds are sent to the widgetset.
  * EnableAutoSizing. The parent form DoAllAutoSize is called.
  * All needed handles are created recursively by TWinControl.DoAllAutoSize. They are not yet made visible.
  * TControl.DoAllAutoSize computes the bounds by calling DoAutoSize for all controls. It does that in a loop until no bound changes.
  * The bounds are sent to the widgetset (TWinControl.RealizeBoundsRecursive calling RealizeBounds).
  * The handles are made visible (if HandleObjectShouldBeVisible and not Showing then).



Basically the AutoSizing properties work in this order: 

  * The constraints are applied to every step.
  * First comes the Align in order alTop, alBottom, alLeft, alRight and finally alClient. For example alLeft has 3 aligned sides and one free side. The free side is handled by the below rules. BorderSpacing is applied to aligned sides. alCustom and alNone have no aligned sides.
  * Next comes ChildSizing. If ChildSizing.Layout is not cclNone then all child controls with Align=alNone, Anchors=[akLeft,akTop] and no AnchorSide controls are resized/repositioned. These child controls are called on this page "non aligned controls".
  * Next comes AutoSize. If ChildSizing.Layout is cclNone then all "non aligned controls" are moved to the top, left and the control is shrunken or enlarged to fit around all its children (not only the "non aligned controls").
  * Next comes the Anchors. Only those sides are anchored, which were not handled by Align (e.g. akRight on alLeft). If an AnchorSide control is set the side will be aligned to the side of the other control. Otherwise the default anchor rules apply, which are mostly similar to the Delphi ones.
  * every change of bounds triggers an OnResize/OnChangeBounds event, which may be used by the application to do arbitrary things.



### Algorithm for Align and Anchors

The main method is TWinControl.AlignControls. 

  1. init RemainingClientRect to ClientRect and RemainingBorderSpace to Rect(0,0,0,0)
  2. call AdjustClient to adjust RemainingClientRect
  3. apply ChildSizing.LeftRightSpacing,ChildSizing.TopBottomSpacing
  4. call DoAlign with alTop,alBottom,alLeft,alRight,alClient,alCustom,alNone 
     1. DoAlign collects child controls with this Align value and call for each such control DoPosition 
        1. DoPosition calculates the new Left,Top,Width,Height



  


### DoAutoSize

This is called by DoAllAutoSize and should not be called by the application. Applications should call AdjustSize and/or InvalidatePreferredSize. 

If it is not overriden the normal TWinControl.DoAutoSize does the following, when AutoSize=true: 

It will move "non aligned controls" - child controls with default anchors (Anchors=[akLeft,akTop],Align=alNone). It moves all of them the same amount, so their total bounding box fits left and top. The spacing of BorderSpacing and ChildSizing is considered. 

Before the move, there is space to the left and above the buttons: [![Autosize before move childs.png](https://wiki.freepascal.org/images/a/ab/Autosize_before_move_childs.png)](</File:Autosize_before_move_childs.png>)

After the move: [![Autosize after move childs.png](https://wiki.freepascal.org/images/6/61/Autosize_after_move_childs.png)](</File:Autosize_after_move_childs.png>)

Then the preferred size of the control is computed. This calculation considers child controls, Align, Anchors, Constraints, ChildSizing.Layout and the other LCL properties. 

The control is resized: [![Autosize after shrink.png](https://wiki.freepascal.org/images/3/33/Autosize_after_shrink.png)](</File:Autosize_after_shrink.png>)

  
See also: [Align and parent AutoSize](<Autosize_/_Layout.md> "Autosize / Layout")

### DisableAutoSizing/EnableAutosizing

see [here](<Autosize_/_Layout.md> "Autosize / Layout"). 

### Differences from Delphi

  * Delphi has no BorderSpacing, AnchorSides, ChildSizing.
  * Delphi does not support AutoSize and Align on all controls.
  * Delphi's AutoSize algorithm does not support nested autosized controls with the Align property. For example a Panel1 with Align=alTop in a Panel2 and both panels have AutoSize=true. Panel2 will not shrink/enlarge horizontally, which means Panel1 will not shrink/enlarge either. So, Panel1 is not autosized to the needed size for its children.



## FAQ

see [Autosize / Layout](<Autosize_/_Layout.md> "Autosize / Layout").

---

_Source: [https://wiki.freepascal.org/LCL_AutoSizing](https://web.archive.org/web/20250324173923/https://wiki.freepascal.org/LCL_AutoSizing)_
