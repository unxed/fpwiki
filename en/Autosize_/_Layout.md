# Autosize / Layout

│ **English (en)** │  **[русский (ru)](<../../ru/Autosize_/_Layout.md> "Autosize / Layout/ru")** │  **[中文（中国大陆） (zh_CN)](</Autosize_/_Layout/zh_CN> "Autosize / Layout/zh CN")** │  **[中文（臺灣） (zh_TW)](</Autosize_/_Layout/zh_TW> "Autosize / Layout/zh TW")** │    
****

## Contents

  * 1 Overview
    * 1.1 Precedence rules
    * 1.2 Common Properties
  * 2 Fixed design
  * 3 AutoSize
    * 3.1 Lazarus vs Delphi
    * 3.2 AutoSize and resizing the control
    * 3.3 AutoSize and moving child controls
    * 3.4 AutoSize and Forms
      * 3.4.1 Force the auto sizing of a form
      * 3.4.2 Compute the size of an autosized form in advance
  * 4 Anchor Sides
    * 4.1 Anchored to Parent or Nil
      * 4.1.1 Changing the size of the parent of an anchored control
      * 4.1.2 Changing the size of an anchored control
        * 4.1.2.1 Anchored to nil
        * 4.1.2.2 Anchored to Parent
      * 4.1.3 Anchored to sibling
    * 4.2 BorderSpacing
      * 4.2.1 BorderSpacing rules
        * 4.2.1.1 Anchoring to the opposite side
        * 4.2.1.2 Anchoring to the same side
        * 4.2.1.3 Anchoring to center
        * 4.2.1.4 Example
      * 4.2.2 BorderSpacing and Align
  * 5 Align
    * 5.1 Align and BorderSpacing
    * 5.2 Align and Anchoring
    * 5.3 Align and AutoSize
    * 5.4 Align and parent AutoSize
    * 5.5 alCustom
    * 5.6 Order of controls with same Align
  * 6 Layout
    * 6.1 Rows, columns and lines
    * 6.2 Fixed space between rows and columns
    * 6.3 Enlarge
      * 6.3.1 crsScaleChilds
      * 6.3.2 crsHomogeneousChildResize
      * 6.3.3 crsHomogeneousSpaceResize
    * 6.4 Shrink
    * 6.5 Individual cells
  * 7 Custom layout with OnResize / OnChangeBounds
    * 7.1 Common mistake: Wrong OnResize event
    * 7.2 Common mistake: Width instead of ClientWidth, AdjustClientRect
  * 8 Custom Controls
    * 8.1 SetBounds, ChangeBounds, DoSetBounds
    * 8.2 AdjustClientRect
      * 8.2.1 AdjustClientRect and Align
    * 8.3 Own AutoSize
      * 8.3.1 Preferred Size
      * 8.3.2 AdjustSize
  * 9 Reduce overhead with DisableAutoSizing, EnableAutoSizing
    * 9.1 DisableAutoSizing and Form bounds
  * 10 Example: a button panel
  * 11 Scrolling
    * 11.1 Scroll Position
    * 11.2 Scrolling and AutoSize
  * 12 Docking
  * 13 Splitter
  * 14 TLabel.WordWrap
  * 15 DPI auto-adjustment and absolute layout auto-adjustment
  * 16 More details
    * 16.1 Parent.AutoSize
    * 16.2 Properties / Methods
  * 17 FAQ
    * 17.1 Why does AutoSize not work in the designer properly?
    * 17.2 Why does TForm.AutoSize not work when something changes?
    * 17.3 Do I need to call Application.ProcessMessages when creating lots of controls?
    * 17.4 When enabling Anchors at runtime the control resizes, does not use the current values. Why?
    * 17.5 Resizing stringgrid columns in form's OnResize event does not work
    * 17.6 Why is TForm.Width equal to TForm.ClientWidth?
    * 17.7 I get an infinite loop / How to debug autosizing?
      * 17.7.1 Differences to Delphi
      * 17.7.2 Overriding a LCL method and triggering a recompute
        * 17.7.2.1 TWinControl.AlignControls
      * 17.7.3 OnResize/OnChangeBounds conflict with LCL properties
      * 17.7.4 LCL interface bug, custom widget
      * 17.7.5 alCustom
  * 18 See also



## Overview

The [LCL](<../LCL.md> "LCL") can automatically alter a control's size and position so it adapts to changes in font, theme and text or other content. If you want to run your program on several platforms, or if your captions are available in several languages, your controls need to adapt correctly to their environment. The [LCL](<../LCL.md> "LCL") allows you not only to make a quick first design (moving controls around on a form using the mouse) but also later to set several key properties which will make the controls automatically adapt subsequently to changed content etc. 

  * Fixed design: this is the default when placing a control in the Designer. The control's position is fixed relative to its parent. The control's size and position (Left, Top) is fully adjustable by the programmer. You can move the control around with the mouse, and resize it freely.
  * Aligned: aligned controls fill up the remaining parental space at the top, bottom, left, or right, or fill the entire remaining space.
  * Anchored: you can anchor a control's sides (left, top, right, bottom) to its parent or to another control. Anchoring means: the [LCL](<../LCL.md> "LCL") will try to maintain the same distance from the anchor point.
  * Layout: controls can be automatically aligned in rows and columns (e.g. TRadioGroup)
  * Custom via OnResize: you can align controls yourself in code using the OnResize and OnChangeBounds events.
  * Custom controls: writing your own controls, you can override nearly every [LCL](<../LCL.md> "LCL") behaviour you want.



### Precedence rules

  1. Constraints
  2. Align
  3. Anchors
  4. ChildSizing.Layout
  5. AutoSize
  6. OnResize, OnChangeBounds - however, if you set bounds that conflict with the rules above you will create an endless loop



### Common Properties

Several important properties can be changed to configure autosizing: 

  * Left, Top, Width, Height
  * AutoSize: autosize instructs the [LCL](<../LCL.md> "LCL") to automatically resize the Width and Height of a control
  * Anchors: lets you create dependencies, for example to anchor a ComboBox to the right side of a Label.
  * Align
  * Constraints: lets you set a minimum and maximum for the Width and Height
  * BorderSpacing: lets you set the spacing between anchored controls
  * ChildSizing: lets you set the layout and spacing of child controls



The internals of the algorithms are explained here: [LCL AutoSizing](<../LCL_AutoSizing.md> "LCL AutoSizing"). 

## Fixed design

Fixed design is the default. The Anchor's property is set to [akLeft,akTop], which means the values of Left and Top are not changed by the [LCL](<../LCL.md> "LCL"). If AutoSize is false, the [LCL](<../LCL.md> "LCL") does not alter Width or Height. If AutoSize is true, the Width and Height are changed to fit the content. For example, TLabel.AutoSize defaults to true and thus changing its Caption will resize the label to accommodate the changed wording. TButton.AutoSize defaults to false, so changing a button's Caption does not resize the button. When you set Button.AutoSize to true the Button will shrink or enlarge every time the button's Caption or its font (or theme) changes. Note, this change does not always happen immediately. For example during FormCreate all autosizing is suspended. At any time you can change the "Left", "Top", "Width", or the "Height" property. 

## AutoSize

AutoSize is a boolean property found in many classes; it permits the size of a control to be adjusted automatically to accommodate differences in the text or graphic contained therein, and allows most efficient use of available space. This is a crucial mechanism to create cross platform forms. 

Normally AutoSize=true does two things for a visible control: 

  * If possible it resizes the control to the preferred size. For example a TButton's Width and Height is resized to fit the caption, while a TEdit is only resized in Height. A TEdit's Width is not autosized. You can change the Width of a TEdit. (see GetPreferredSize).
  * It moves all fixed positioned child controls by the same amount, so that the leftmost child control is moved to Left=0 (depends on BorderSpacing) and the topmost child control is moved to Top=0.



### Lazarus vs Delphi

  * Delphi's AutoSize happens only when certain properties change, for example when the Font of a TLabel is changed. The [LCL](<../LCL.md> "LCL") AutoSize is always active. Delphi allows one to change the size of a Label which has AutoSize=true, the Lazarus control does not.
  * Delphi's hidden controls are not autosized.
  * With Delphi, changing the size of a control does not resize/move anchored child controls during loading, but a constructor is normally used to make the desired effect. When using akRight,akBottom anchors, set AnchorSides and BorderSpacing to keep the distance correct.



### AutoSize and resizing the control

With AutoSize=false buttons are given a default fixed size. 

[![Autosize1.png](https://wiki.freepascal.org/images/1/1f/Autosize1.png)](</File:Autosize1.png>)

When setting AutoSize=true for each button, the buttons are enlarged (or shrunk) to fit the text and theme frame. 

[![Autosize on.png](https://wiki.freepascal.org/images/7/75/Autosize_on.png)](</File:Autosize_on.png>)

AutoSize does not shrink to the smallest possible size as you can see in the OK Button. It uses the GetPreferredSize method of the control, which calls the CalculatePreferredSize method. The default TWinControl implementation queries the widget set, which might have a preferred width or height. Each control can override the CalculatePreferredSize method. For example TImage overrides it and returns the size of the Picture. If no preferred width (height) is available the returned value is 0 and the [LCL](<../LCL.md> "LCL") will keep the Width (Height) (unless the ControlStyle flag csAutoSize0x0 is set, which is currently only used by TPanel). 

A TWinControl computes the size of all its child controls and uses that to compute its preferred size. 

When a control is anchored to both left and right its _width is fixed_. For example with _Align=alTop_ the control is anchored to left and right and follows the width of the Parent. If _Parent.AutoSize_ is true, then the _Parent_ will use the preferred width of the control to compute its own preferred width, and thus the control will be resized to its preferred width. See [Align and AutoSize](<_Layout.md> "Autosize / Layout"). If no preferred width is available, then the last set bounds are used (**BaseBounds** or **ReadBounds**). If no bounds were set, the **GetControlClassDefaultSize** method is used. Same for **Height** and anchoring to top and bottom. 

Constraints are always applied and have precedence. 

### AutoSize and moving child controls

When AutoSize=false you can place and move controls freely: 

[![Autosize on.png](https://wiki.freepascal.org/images/7/75/Autosize_on.png)](</File:Autosize_on.png>)

When AutoSize=true child controls with fixed positions are moved to fit. 

[![Autosize panel on.png](https://wiki.freepascal.org/images/d/d9/Autosize_panel_on.png)](</File:Autosize_panel_on.png>)

Both buttons on the panel were moved by the same amount to top and left, so that there is no space left to the left and top. If the buttons would have BorderSpacing>0 or the Panel.ChildSizing.LeftRightSpacing>0 the buttons would be moved so that the defined space is used. 

Only child controls with the following values are moved: 

  * Anchors=[akLeft,akRight]
  * AnchorSide[akLeft].Control=nil
  * AnchorSide[akTop].Control=nil
  * Align=alNone



The moving of the child controls depend on the propert [ChildSizing.Layout](<_Layout.md> "Autosize / Layout"). The layout is applied in the method TWinControl.AlignControls which can be overridden completely or in parts. For example TToolBar overrides ControlsAligned to position all controls with Align=alCustom and defines a flow layout where controls that do not fit are put into subsequent lines. 

Controls can disable the moving of child controls by setting the ControlStyle flag csAutoSizeKeepChildLeft and csAutoSizeKeepChildTop (since 0.9.29). 

### AutoSize and Forms

Forms without Parent are controlled by the window manager and can therefore not be placed or resized freely. Setting the size is only a recommendation and might be ignored by the window manager. For example you might set the Width of a Form to 1000 and the widget set might answer with a resize to 800. If you set the Width in OnResize you might create an endless loop. That's why the [LCL](<../LCL.md> "LCL") TForm disables AutoSize when the widget set resizes the form. 

That means AutoSize for forms stops when the user resizes the form or if the window manager does not like the bounds. For example some Linux window managers have features like magnetic sides which resizes windows in relationship to other windows. 

#### Force the auto sizing of a form

You can start/execute a new AutoSize by doing: 
    
    
    Form1.AutoSize := False;
    Form1.AutoSize := True;
    

#### Compute the size of an autosized form in advance

When placing an autosized form you might need the size of the form before showing it. Autosizing requires the handle. You can compute the size of a form before showing it with: 
    
    
    Form1.HandleNeeded;
    Form1.GetPreferredSize(PreferredWidth,PreferredHeight);
    

**Note** : The window manager and form events might change the size. The preferred size does not include the form's window borders. This is a planned feature. 

## Anchor Sides

Controls have four sides: akLeft, akTop, akRight and akBottom. Each side can be anchored to the parent or the side of another sibling (a control with the same parent). Anchoring means **keeping the distance**. The default is Anchors=[akLeft,akTop]. Vertical anchors are independent of horizontal anchors. Some properties like Align and Parent.AutoSize have higher precedence and can change the behaviour. 

### Anchored to Parent or Nil

Anchoring to Nil (the default) has almost the same effect as anchoring to the Parent. Both try to keep the distance to a side of the Parent's client area. Anchoring to Nil uses the distance of the last SetBounds call, while Anchoring to Parent uses the BorderSpacing value. 

There are four combinations of akLeft,akRight (akTop,akBottom): 

  * **akLeft, no akRight** : the control's **Left** is fixed and not changed by the [LCL](<../LCL.md> "LCL"). The right side is not anchored, so it follows the left side. That means the **Width** is kept too.
  * **akLeft and akRight** : the control's **Left** is fixed and not changed by the [LCL](<../LCL.md> "LCL"). The right side is anchored to the Parent's right side. That means if the Parent is enlarged by 100pixel then the control's **Width** is enlarged by 100pixel too.
  * **akRight, no akLeft** : the control's left side is not anchored, so it will follow its right side. The right side is anchored. That means if the parent is enlarged by 100pixel then the control is moved to the right by 100pixel.
  * **no akLeft and no akRight** : neither sides are anchored. The position of the center of the control scales with the parent. For example if the control is in the middle of the parent and the parent is enlarged by 100 pixel, then the control is moved 50pixel to the right.



#### Changing the size of the parent of an anchored control

When changing the size of a parent all anchored child controls are moved and/or resized immediately unless AutoSizing is disabled. For example AutoSizing is disabled during loading of a form and during creation of a form. 

A GroupBox with a Button: [![Anchors1.png](https://wiki.freepascal.org/images/6/60/Anchors1.png)](</File:Anchors1.png>)

When the GroupBox is enlarged the distance of the anchored side is kept: 

With akLeft, no akRight: [![Anchors akLeft.png](https://wiki.freepascal.org/images/0/05/Anchors_akLeft.png)](</File:Anchors_akLeft.png>)

With akLeft and akRight: [![Anchors akLeft akRight.png](https://wiki.freepascal.org/images/6/66/Anchors_akLeft_akRight.png)](</File:Anchors_akLeft_akRight.png>)

With akRight, no akLeft: [![Anchors akRight no akLeft.png](https://wiki.freepascal.org/images/d/d3/Anchors_akRight_no_akLeft.png)](</File:Anchors_akRight_no_akLeft.png>)

Three buttons in a groupbox: [![Anchors no akLeft no akRight small.png](https://wiki.freepascal.org/images/4/48/Anchors_no_akLeft_no_akRight_small.png)](</File:Anchors_no_akLeft_no_akRight_small.png>)

With no akLeft and no akRight their centers are scaled: [![Anchors no akLeft no akRight big.png](https://wiki.freepascal.org/images/7/7b/Anchors_no_akLeft_no_akRight_big.png)](</File:Anchors_no_akLeft_no_akRight_big.png>)

  
**Notes:**

  * Loading a form is like one big SetBounds. During loading properties are set using the values of the lfm. Keep in mind that there can be multiple lfm files because of ancestors and frames. After loading the [LCL](<../LCL.md> "LCL") enables autosizing. Anchored controls use the bounds at the end of loading. Any step in between is ignored.
  * For custom controls it is often better to set AnchorSides instead of only Anchors.



#### Changing the size of an anchored control

When changing the Width of an anchored control, for example via the Object Inspector or via code _Button1.Width:=3_ , you can see the difference between anchoring to _Parent_ and anchoring to _Nil_. Anchoring to Parent will resize and move the Button1, while anchoring to Nil will only resize. For example: 

##### Anchored to nil

[![Anchored right nil resize1.png](https://wiki.freepascal.org/images/b/bd/Anchored_right_nil_resize1.png)](</File:Anchored_right_nil_resize1.png>) A Button1 anchored [akTop,akRight], AnchorSide[akRight].Control=nil 

[![Anchored right nil resize2.png](https://wiki.freepascal.org/images/a/a0/Anchored_right_nil_resize2.png)](</File:Anchored_right_nil_resize2.png>) Setting the Width to a smaller value will shrink the button, keeping the button's _Left_ , increasing the distance of the right side. 

[![Anchored right nil resize3.png](https://wiki.freepascal.org/images/1/16/Anchored_right_nil_resize3.png)](</File:Anchored_right_nil_resize3.png>) When the Groupbox is resized the button will keep the new distance. 

**Explanation:** setting the **Width** is equivalent to calling SetBounds(**Left** ,Top,**NewWidth** ,Height). That's why the _Left_ is kept. This is Delphi compatible. 

##### Anchored to Parent

[![Anchored right parent resize1.png](https://wiki.freepascal.org/images/4/45/Anchored_right_parent_resize1.png)](</File:Anchored_right_parent_resize1.png>) A Button1 anchored [akTop,akRight], AnchorSide[akRight].Control=Button1.Parent 

[![Anchored right parent resize2.png](https://wiki.freepascal.org/images/0/04/Anchored_right_parent_resize2.png)](</File:Anchored_right_parent_resize2.png>) Setting the Width to a smaller value will shrink the button, keeping the right distance, changing the button's _Left_. 

#### Anchored to sibling

You can anchor to neighbor controls. The following example shows: 

  * you can anchor a label's left to the left of a button
  * anchor a label's top to the bottom of a button
  * anchor the center of a label to the center of a button



[![Anchorsides example1.png](https://wiki.freepascal.org/images/e/ee/Anchorsides_example1.png)](</File:Anchorsides_example1.png>)

[![Anchorside example2.png](https://wiki.freepascal.org/images/b/ba/Anchorside_example2.png)](</File:Anchorside_example2.png>)

For more details and how to setup anchors see: [Anchor Sides](<../Anchor_Sides.md> "Anchor Sides"). 

[![Anchoreditor.png](https://wiki.freepascal.org/images/6/6d/Anchoreditor.png)](</File:Anchoreditor.png>)

### BorderSpacing

The BorderSpacing properties controls the minimum amount of space around a control. The properties are: 

  * **Around** : this amount in pixel is added to Left, Top, Right, Bottom.
  * **Left** : space in pixel on the left side of the control
  * **Top** : space in pixel above the control
  * **Right** : space in pixel on the right side of the control
  * **Bottom** : space in pixel below the control
  * **InnerBorder** : this amount in pixel is added twice to the preferred width and height. Some controls override the calculation and ignore this property. An example where it works is TButton. With InnerBorder you can make a button bigger than needed.
  * **CellAlignHorizontal** : This is used in table layouts like ChildSizing.Layout=cclLeftToRightThenTopToBottom. If the control is smaller than the table cell, this property defines where to align the control: to the left ccaLeftTop, to the right ccaRightBottom or in the middle ccaCenter.
  * **CellAlignVertical** : same as CellAlignHorizontal but for vertical alignment.



#### BorderSpacing rules

  * Around is added to Left,Top,Right,Bottom borderspacing
  * The space can be even bigger, if the controls have constraints that don't allow to expand.



##### Anchoring to the opposite side

For example right side of A to left side of B. 

Both borderspacings of A and B are used. 

  * The horizontal space between two controls (LeftControl, RightControl on Parent) is the maximum of 
    * LeftControl.BorderSpacing.Right + LeftControl.BorderSpacing.Around
    * RightControl.BorderSpacing.Left + RightControl.BorderSpacing.Around
    * Parent.ChildSizing.HorizontalSpacing
  * The vertical space works analog: between two controls (TopControl, BottomControl on Parent) is the maximum of 
    * TopControl.BorderSpacing.Bottom + TopControl.BorderSpacing.Around
    * BottomControl.BorderSpacing.Top + BottomControl.BorderSpacing.Around
    * Parent.ChildSizing.VerticalSpacing



For instance if LeftControl.BorderSpacing.Right = 3 and LeftControl.BorderSpacing.Around = 4, then there is at least 7 pixel space between the two controls. If RightControl.BorderSpacing.Left = 4 and RightControl.BorderSpacing.Around = 4 then the space will be at least 8. If Parent.ChildSizing.HorizontalSpacing = 10 then the space will be at least 10. 

##### Anchoring to the same side

For example right side of A to right side of B. 

  * Only borderspacings of A is used.
  * Parent.ChildSizing.Horizontal/VerticalSpacing are not used.



##### Anchoring to center

For example center of A to center of B. 

No borderspacing is used and no Parent.ChildSizing.Horizontal/VerticalSpacing is used. 

##### Example

A common example is anchoring a Label to an Edit. 

[![BorderSpacing Anchors.png](https://wiki.freepascal.org/images/1/1e/BorderSpacing_Anchors.png)](</File:BorderSpacing_Anchors.png>)

The Label center is vertically anchored to the Edit. The Edit's left side is anchored to the right side of the Label. Both have BorderSpacing.Around=6. This results in 6 pixel space between the Label and the Edit and 6 pixel space left of the Label and 6 pixel space right of the Edit. There is 6 pixel space above and below the Edit as well. 

  * The left space between a control and its parent (Label1 on GroupBox1) is the maximum of 
    * Label1.BorderSpacing.Left + Label1.BorderSpacing.Around
    * GroupBox1.ChildSizing.LeftTopSpacing


  * The right space between a control and its parent (Edit1 on GroupBox1) is the maximum of 
    * Edit1.BorderSpacing.Right + Edit1.BorderSpacing.Around
    * GroupBox1.ChildSizing.RightBottomSpacing


  * When a control's center is anchored to another control, for example the above Label is centered vertically to the Edit1, then all spacings are ignored.


  * When a control's left side is anchored to the left side of another control (i.e. they are aligned with the left side), then the distance between both left's side is the 
    * Control1.BorderSpacing.Left + Control1.BorderSpacing.Around



Here is a more complex example: 

[![Borderspacing anchors2.png](https://wiki.freepascal.org/images/9/9c/Borderspacing_anchors2.png)](</File:Borderspacing_anchors2.png>)

The important parts of the lfm code: 
    
    
      object GroupBox1: TGroupBox
        AutoSize = True
        Caption = 'GroupBox1'
        TabOrder = 0
        object Label1: TLabel
          AnchorSideLeft.Control = GroupBox1
          AnchorSideTop.Control = Edit1
          AnchorSideTop.Side = asrCenter
          BorderSpacing.Around = 6
          Caption = 'Label1'
        end
        object Edit1: TEdit
          AnchorSideLeft.Control = Label1
          AnchorSideLeft.Side = asrBottom
          AnchorSideTop.Control = GroupBox1
          AnchorSideRight.Control = GroupBox1
          AnchorSideRight.Side = asrBottom
          Anchors = [akTop, akLeft, akRight]
          BorderSpacing.Around = 6
          TabOrder = 0
          Text = 'Edit1'
        end
        object Label2: TLabel
          AnchorSideLeft.Control = GroupBox1
          AnchorSideTop.Control = Edit1
          AnchorSideTop.Side = asrBottom
          BorderSpacing.Around = 6
          Caption = 'Label2'
        end
        object ComboBox1: TComboBox
          AnchorSideLeft.Control = Label2
          AnchorSideTop.Control = Label2
          AnchorSideTop.Side = asrBottom
          AnchorSideRight.Control = GroupBox1
          AnchorSideRight.Side = asrBottom
          Anchors = [akTop, akLeft, akRight]
          BorderSpacing.Right = 6
          TabOrder = 1
          Text = 'ComboBox1'
        end
        object CheckBox1: TCheckBox
          AnchorSideLeft.Control = GroupBox1
          AnchorSideTop.Control = ComboBox1
          AnchorSideTop.Side = asrBottom
          BorderSpacing.Around = 6
          Caption = 'CheckBox1'
          TabOrder = 2
        end
        object Label3: TLabel
          AnchorSideLeft.Control = GroupBox1
          AnchorSideTop.Control = Edit2
          AnchorSideTop.Side = asrCenter
          BorderSpacing.Around = 6
          Caption = 'Label3'
        end
        object Edit2: TEdit
          AnchorSideLeft.Control = Label3
          AnchorSideLeft.Side = asrBottom
          AnchorSideTop.Control = CheckBox1
          AnchorSideTop.Side = asrBottom
          BorderSpacing.Around = 6
          TabOrder = 3
          Text = 'Edit2'
        end
      end
    

#### BorderSpacing and Align

BorderSpacing works with Align. In the example below there is a Memo1 with Align=alTop and a Memo2 with Align=alCLient. 

  * Normally the two Memos would fill the whole GroupBox.
  * But the Memo1 has BorderSpacing.Around=10, so there 10 pixel space around Memo1.
  * The Memo2 has BorderSpacing.Top=20. The space between Memo1 and Memo2 is the maximum, which is the 20 from Memo2.
  * Memo2 has also BorderSpacing.Right=50 so there is 50 Pixel space right of Memo2.
  * The GroupBox can define default space for all its child controls via ChildSizing.LeftRightSpacing/VerticalSpacing/HorizontalSpacing. In this example it does not (all 0).



[![Borderspacing align1.png](https://wiki.freepascal.org/images/1/13/Borderspacing_align1.png)](</File:Borderspacing_align1.png>)

## Align

The Align property works pretty much like in Delphi and can be used to quickly fill an area. For example to let a TListBox fill the entire area of its Parent, set ListBox1.Align=alClient. The align values alTop, alBottom, alLeft and alRight will place controls without overlapping if possible. That means all controls with Align in alLeft,alTop,alBottom,alRight will not overlap if there is enough space. 

The algorithm works as follows: 

  * First all controls with alTop are put to the top of the client area. If there are several controls with alTop the last added/moved will be put topmost. The algorithm will try to avoid overlapping and keep the height of the control, while expanding the width to maximum. AnchorSides of the left, top and right sides are ignored. The bottom AnchorSide works normal. Borderspacing and Parent.ChildSizing spaces are considered, so you can define space around each aligned control.
  * Then all controls with alBottom are put to the bottom of the client area. Otherwise it works analog to alTop.
  * Then all controls with alLeft are put to the left of the client area between the alTop and alBottom controls. Otherwise it works analog to alTop.
  * Then all controls with alRight are put to the right of the client area between the alTop and alBottom controls. Otherwise it works analog to alTop.
  * If there is a control with alClient it will fill the remaining client.
  * If there is more than one control with alClient they will be placed at the same position, overlapping each other. Use the Visibility property to define which one is shown.



[![Autosize align.png](https://wiki.freepascal.org/images/e/e6/Autosize_align.png)](</File:Autosize_align.png>)

### Align and BorderSpacing

The space of BorderSpacing and of the parent's ChildSizing is applied to aligned controls. The memo below has Align=alClient. 

[![Autosize align borderspacing.png](https://wiki.freepascal.org/images/b/b4/Autosize_align_borderspacing.png)](</File:Autosize_align_borderspacing.png>)

### Align and Anchoring

The free side of an aligned control (e.g. the right side of a **Align=alLeft**) follows the anchoring rules. If the anchor is not set, then the control will keep its **Width**. If the anchor is set, then the **Width** will change. 

### Align and AutoSize

A control aligned with alLeft or alRight expands vertically, and will use its designed width. If _AutoSize_ is set to _true_ , then the _Width_ will be the preferred width. The button below has _Align=alRight_ and _AutoSize=true_. 

[![Autosize align autosize.png](https://wiki.freepascal.org/images/3/35/Autosize_align_autosize.png)](</File:Autosize_align_autosize.png>)

### Align and parent AutoSize

A control aligned with alClient fills the remaining space. A parent control with _AutoSize=true_ will expand or shrink to enclose its children exactly. What happens if you combine these two properties? Say you put a button with Align=alClient into a groupbox with AutoSize=true? 

[![Autosize nested1.png](https://wiki.freepascal.org/images/b/b1/Autosize_nested1.png)](</File:Autosize_nested1.png>)

The [LCL](<../LCL.md> "LCL") uses the _preferred size_ of the buttons and expands or shrinks the groupbox accordingly: 

[![Autosize nested2.png](https://wiki.freepascal.org/images/2/28/Autosize_nested2.png)](</File:Autosize_nested2.png>)

### alCustom

This Align value exists for custom AutoSize algorithms and is treated by the [LCL](<../LCL.md> "LCL") almost like alNone. Controls with alCustom are not moved by the [LCL](<../LCL.md> "LCL") but by your custom control. You have to override CalculatePreferredSize and DoAutoSize. 

### Order of controls with same Align

Controls with same Align are positioned in the following order. On alLeft the control with the lowest Left wins, for alTop the lowest Top, for alRight the biggest Left+Width, for alBottom the biggest Top+Height. If two controls have the same coordinate the one last moved (calls to SetBounds or SetParent) wins. Controls can override CreateControlAlignList to change the order or override DoAlignChildControls to handle the whole Align. 

Note: Before 0.9.29 controls at the same coordinate were put in Z order. This resulted in reordering in some cases and had to be fixed. There is one incompatibility: If you added several controls with alTop or alLeft without specifying bounds the controls are put at 0,0 and therefore the last added wins, where formerly the first won. 

Note for Delphians: Contrary to the VCL the LCL can and often does delay recomputing the layout. Delphi does the layout after every property change. Setting Align=alTop calls in the VCL a special AlignControls, where the current control has a special meaning. That's why in the VCL the order of setting properties is important. The LCL layout algorithm is independent of the chronological order of setting the properties. 

For example all changes in FormCreate are delayed and the layout is computed from the final property values. On the other hand ButtonClick has no delay (aka you have to add the Disable/EnableAutoSize yourself), so inserting alTop controls may have different results. 

If you want a specific order, use Disable/EnableAutoSize and set the "Top" properties. For example TForm.OnCreate is called with AutoSize disabled, so you can use: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // making sure Panel2 is above Panel1, even though Panel1 is set first:
      Panel1.Align:=alTop;
      Panel1.Top:=1;
      Panel2.Align:=alTop;
      Panel2.Top:=0;
    end;
    

## Layout

### Rows, columns and lines

You can align child controls in rows and columns using the ChildSizing properties. For example: 

  * ChildSizing.Layout=cclLeftToRightThenTopToBottom
  * ChildSizing.ControlsPerLine=3
  * AutoSize=false (the groupbox does not resize to fit the child controls)



[![Autosize layout lefttoright.png](https://wiki.freepascal.org/images/1/10/Autosize_layout_lefttoright.png)](</File:Autosize_layout_lefttoright.png>)

The **Layout** property defaults to cclNone. If you set **Layout** to another value, every child, that has _normal_ anchors, will be aligned. _Normal_ anchors means: 

  * Anchors=[akLeft,akTop]
  * AnchorSide[akLeft].Control=nil
  * AnchorSide[akTop].Control=nil
  * Align=alNone



The value **cclLeftToRightThenTopToBottom** will put the first child at top, left, the second to the right, and so forth. This is a **line**. The property **ControlsPerLine** defines when a new line is started. In the above example each line (row) has 3 controls. There are 12 controls, so there are 4 lines (rows) each with 3 controls (columns). If _ControlsPerLine_ is 0 it means unlimited controls per line - there would be only one row with all childs. 

You can see that the rows have different sizes, each row has the size of the biggest control and that the controls are resized to column width. There is no space between the rows. The space in the image comes from the used theme, not from the [LCL](<../LCL.md> "LCL"). 

### Fixed space between rows and columns

You can add space between with these properties: 

  * ChildSizing.VerticalSpacing - Space between rows
  * ChildSizing.HorizontalSpacing - Space between columns
  * ChildSizing.LeftRightSpacing - Space on the left and right of _all_ columns
  * ChildSizing.TopBottomSpacing - Space above and below of _all_ columns



The above example with ChildSizing.VerticalSpacing=6, ChildSizing.HorizontalSpacing=15, ChildSizing.LeftRightSpacing=30, ChildSizing.TopBottomSpacing=10, AutoSize=true 

[![Autosize layout parentspacing.png](https://wiki.freepascal.org/images/a/a9/Autosize_layout_parentspacing.png)](</File:Autosize_layout_parentspacing.png>)

Additionally you can add individual space for each control with its BorderSpacing properties. 

### Enlarge

The above example resized the GroupBox to the needed space. If your GroupBox has a fixed size or if it is not freely resizable, for instance if the GroupBox should fill the whole width of the form, then the childs should enlarge. There are several modes. The default mode _ChildSizing.EnlargeHorizontal=crsAnchorAligning_ is to not enlarge anything. The space on the right side will be unused. 

  * crsAnchorAligning - do not use the extra space
  * crsScaleChilds - multiply the width/height with the same factor
  * crsHomogeneousChildResize - add to each width/height the same amount
  * crsHomogeneousSpaceResize - add to each space between the childs the same amount



#### crsScaleChilds

ChildSizing.EnlargeHorizontal=crsScaleChilds, ChildSizing.EnlargeVertical=crsScaleChilds, AutoSize=false 

[![Autosize layout scalechilds.png](https://wiki.freepascal.org/images/b/b0/Autosize_layout_scalechilds.png)](</File:Autosize_layout_scalechilds.png>)

For example if the ClientWidth is twice as big as needed, then every child will be twice as big. 

#### crsHomogeneousChildResize

ChildSizing.EnlargeHorizontal=crsHomogeneousChildResize, ChildSizing.EnlargeVertical=crsHomogeneousChildResize, AutoSize=false 

[![Autosize layout homogeneouschildresize.png](https://wiki.freepascal.org/images/3/39/Autosize_layout_homogeneouschildresize.png)](</File:Autosize_layout_homogeneouschildresize.png>)

For example if the ClientWidth is 30 pixel bigger than needed, then every child will be 10 pixel broader. 

#### crsHomogeneousSpaceResize

ChildSizing.EnlargeHorizontal=crsHomogeneousSpaceResize, ChildSizing.EnlargeVertical=crsHomogeneousSpaceResize, AutoSize=false 

[![Autosize layout homogeneousspaceresize.png](https://wiki.freepascal.org/images/9/96/Autosize_layout_homogeneousspaceresize.png)](</File:Autosize_layout_homogeneousspaceresize.png>)

For example if the ClientWidth is 40 pixel bigger than needed, there will be 10 pixel space on the left, right and between each child. 

### Shrink

Shrinking works similarly to enlarging. You can set different modes if there is not enough space for controls. **ShrinkHorizontal** , **ShrinkVertical**. 

### Individual cells

In the above examples all controls were resized the same, each filled the whole _cell_. A _cell_ is the space in a specific row and column. Normally a control fills the whole cell space. This can be changed with the properties **BorderSpacing.CellAlignHorizontal** and **BorderSpacing.CellAlignVertical**. 

For example set the _BorderSpacing.CellAlignHorizontal_ of the fifth button to **caCenter** you will get this: 

[![Autosize layout cellhorizontal cacenter.png](https://wiki.freepascal.org/images/d/d3/Autosize_layout_cellhorizontal_cacenter.png)](</File:Autosize_layout_cellhorizontal_cacenter.png>)

There are four possible values for CellAlignHorizontal/CellAlignVertical: 

  * caFill: the child control fills the whole width (height) of the cell
  * caCenter: the child control uses its preferred width (height) and will be centered in the cell
  * caLeftTop: the child control uses its preferred width (height) and will be leftaligned in the cell
  * caRightBottom: the child control uses its preferred width (height) and will be rightaligned in the cell



[![Autosize layout cellalign.png](https://wiki.freepascal.org/images/c/c0/Autosize_layout_cellalign.png)](</File:Autosize_layout_cellalign.png>)

## Custom layout with OnResize / OnChangeBounds

Sometimes the [LCL](<../LCL.md> "LCL") layout is not sufficient. The below example shows a GroupBox1 with a ListBox1 and a Memo1. The ListBox1 should fill one third of the space, the Memo1 takes the rest. 

[![Autosize onresize.png](https://wiki.freepascal.org/images/4/4b/Autosize_onresize.png)](</File:Autosize_onresize.png>)

Whenever the GroupBox is resized the ListBox1.Width should be one third. To achieve this set the OnResize event of the GroupBox1 to: 
    
    
    procedure TForm1.GroupBox1Resize(Sender: TObject);
    begin
      ListBox1.Width := GroupBox1.ClientWidth div 3;
    end;
    

### Common mistake: Wrong OnResize event

Do not put _all_ your resizing code into the Form OnResize event. The Form OnResize event is only called when the Form was resized. The child controls (e.g. a GroupBox1) is resized later, so the GroupBox1.ClientWidth has still the old value during the FormResize event. You must use the GroupBox1.OnResize event to react to changes of GroupBox1.ClientWidth. 

### Common mistake: Width instead of ClientWidth, AdjustClientRect

The Width is the size including the frame. The ClientWidth is the inner Width without the frame. Some controls like the TPanel paints a further frame. Then you have to use AdjustClientRect. The same example, but instead of a GroupBox1 the ListBox1 is in a Panel1: 
    
    
    procedure TForm1.Panel1Resize(Sender: TObject);
    var 
      r: TRect;
    begin
      r := Panel1.ClientRect;
      Panel1.AdjustClientRect(r);
      ListBox1.Width := (r.Right - r.Left) div 3;
    end;
    

## Custom Controls

When you write your own control, you can override and fine tune many parts of the [LCL](<../LCL.md> "LCL") autosizing. 

### SetBounds, ChangeBounds, DoSetBounds

**SetBounds** is called when the properties Left, Top, Width, Height, BoundsRect is set or the user calls it directly. SetBounds updates the BaseBounds and BaseParentClientSize, which are used by anchoring to keep the distance. For example loading a Form with TMemo and the lfm contains TMemo's Left and Width, then SetBounds is called two times for the memo. When the user maximizes a window, SetBounds is called for the form, but not for the Memo, keeping the BaseBounds of the Memo. If the Memo is anchored to the right, the Width of the Memo is changed based on the BaseBounds and BaseParentClientSize. Keep in mind that the given aLeft, aTop, aWidth, aHeight might not be valid and will be changed by the [LCL](<../LCL.md> "LCL") before applied. Delphi calls SetBounds more often. SetBounds calls ChangeBounds with KeepBase=false. 

**ChangeBounds** is called whenever the position or size of the control is set, either via the properties or by the layouter of the [LCL](<../LCL.md> "LCL"). SetBounds calls internally ChangeBounds with KeepBase=false, while the [LCL](<../LCL.md> "LCL") layouter calls it with KeepBase=true. Override this for code that might change the preferred size or resizes other controls. Keep in mind that the given aLeft, aTop, aWidth, aHeight might not be valid and will be changed by the [LCL](<../LCL.md> "LCL") before applied. You can call this function. 

**DoSetBounds** is a low level function to set the private variables FLeft, FTop, FWidth, FHeight. Do not call this function, only the [LCL](<../LCL.md> "LCL") calls it. It also updates FClientWidth and FClientHeight accordingly. Override this to update the content layout of the control, for example scroll bars. As always: do not paint here, but call Invalidate and paint in OnPaint or override Paint. 

**DoAdjustClientRectChange** is called by the [LCL](<../LCL.md> "LCL") and the [LCL](<../LCL.md> "LCL") interface, when the ClientRect has changed and the Width and Height were kept. 

**WMSize** exists for Delphi/VCL compatibility. It is called by the [LCL](<../LCL.md> "LCL") interface and on every change of bounds. 

### AdjustClientRect

The method AdjustClientRect can be overriden by your custom controls and affects Align, ChildSizing.Layout and AnchorSides. It does not affect the meaning of Left, Top and does not affect normal anchoring (for example setting). 

When you want to draw your own frame, then the child controls should be aligned within these frames. For example TPanel draws a frame and reduces the client area by overriding the method **AdjustClientRect** : 
    
    
      TCustomPanel = class(TCustomControl)
      ...
      protected
        ...
        procedure AdjustClientRect(var aRect: TRect); override;
      ...
    
    procedure TCustomPanel.AdjustClientRect(var aRect: TRect);
    var
      BevelSize: Integer;
    begin
      inherited AdjustClientRect(aRect);
    
      BevelSize := BorderWidth;
      if (BevelOuter <> bvNone) then
        inc(BevelSize, BevelWidth);
      if (BevelInner <> bvNone) then
        inc(BevelSize, BevelWidth);
    
      InflateRect(aRect, -BevelSize, -BevelSize);
    end;
    

#### AdjustClientRect and Align

AdjustClientRect can be used to reduce the client area used by all autosize operations. For example TPanel uses AdjustClientRect to reduce the client area by the borderwidth: 

[![Adjustclientrect align.png](https://wiki.freepascal.org/images/c/ca/Adjustclientrect_align.png)](</File:Adjustclientrect_align.png>)

The Button1 in the screenshot was created with Align=alClient. ChildSizing.Layout is affected too. 

When anchoring to a parent via AnchorSides the AdjustClientRect is used too: 
    
    
      Button1.AnchorParallel(akTop,0,Button1.Parent);
    

The Button1's top side is anchored to the top side of the parent's client area. If the AdjustClientRect adds 3 to the Top the Button1.Top will be 3 (3 plus 0). 

### Own AutoSize

When _AutoSize_ is set to true the control should be resized to the preferred size if possible. 

#### Preferred Size

The new size is fetched by the [LCL](<../LCL.md> "LCL") via **GetPreferredSize** which calls **CalculatePreferredSize** , which can be overridden. For example let's write a **TQuadrat** , which is a TShape, but its height should equal its width: 
    
    
      TQuadrat = class(TShape)
      protected
        procedure CalculatePreferredSize(var PreferredWidth,
            PreferredHeight: integer; WithThemeSpace: Boolean); override;
      end;
    ...
    procedure TQuadrat.CalculatePreferredSize(var PreferredWidth,
      PreferredHeight: integer; WithThemeSpace: Boolean);
    begin
      PreferredHeight:=Width;
    end;
    

The method CalculatePreferredSize gets two var parameters: _PreferredWidth_ and _PreferredHeight_. They default to 0 which means: there is no preferred size, so the [LCL](<../LCL.md> "LCL") will not change the size. The above function sets _PreferredHeight_ to the current _Width_. The boolean parameter _WithThemeSpace_ is deprecated and always false. 

**Important** : CalculatePreferredSize must not change the bounds or any other value of the control that can trigger an autosize. Doing so will create a loop. 

Computing the _PreferredWidth/Height_ can be expensive. Therefore the [LCL](<../LCL.md> "LCL") caches the result until _InvalidatePreferredSize_ is called for the control. In our example the _PreferredHeight_ depends on the _Width_ , so we must invalidate when the _Width_ changes: 
    
    
      TQuadrat = class(TShape)
      protected
        ...
        procedure DoSetBounds(ALeft, ATop, AWidth, AHeight: integer); override;
      end;
    ...
    procedure TQuadrat.DoSetBounds(ALeft, ATop, AWidth, AHeight: integer);
    begin
      inherited DoSetBounds(ALeft, ATop, AWidth, AHeight);
      InvalidatePreferredSize;
      // Note: The AdjustSize can be omitted here, because the [[LCL]] does that after calling DoSetBounds.
    end;
    

The [LCL](<../LCL.md> "LCL") will automatically trigger the autosizing when the bounds have changed, so the example is complete. 

The default TWinControl implementation of CalculatePreferredSize queries the widget set, which might return a preferred width and/or height. Each control can override the CalculatePreferredSize method. For example TImage overrides it and returns the size of the Picture. If no preferred width (height) is available the returned value is 0 and the [LCL](<../LCL.md> "LCL") will keep the current Width (Height). If 0 is a valid size for your control, you must set the ControlStyle flag csAutoSize0x0 (ControlStyle:=ControlStyle+[csAutoSize0x0];). An example is the [LCL](<../LCL.md> "LCL") control TPanel. 

#### AdjustSize

When the preferred size depends on a new property, then every time the property changes the auto sizing must be triggered. For example: 
    
    
    procedure TQuadrat.SetSubTitle(const AValue: string);
    begin
      if FSubTitle = AValue then exit;
      FSubTitle := AValue;
      InvalidatePreferredSize;
      AdjustSize;
    end;
    

## Reduce overhead with DisableAutoSizing, EnableAutoSizing

Since Lazarus 0.9.29 there is a new autosizing algorithm, that reduces the overhead and allows deep nested dependencies. Up to 0.9.28 DisableAlign/EnableAlign and Disable/EnableAutoSizing worked only for one control and its direct child controls. 

Each time you change a property the [LCL](<../LCL.md> "LCL") triggers a recomputation of the layout: 
    
    
    Label1.Caption := 'A';  // first recompute
    Label2.Caption := 'B';  // second recompute
    

The recomputations will trigger events and send messages. To reduce the overhead you can suspend the autosizing: 
    
    
    DisableAutoSizing;
    try
      Label1.Caption := 'A';  // no recompute
      Label2.Caption := 'B';  // no recompute
    finally
      EnableAutoSizing; // one recompute
    end;
    

You must balance calls to Disable/EnableAutoSizing. Autosizing starts only when EnableAutoSizing is called after a corresponding (earlier) DisableAutoSizing. 

Since 0.9.29 the Disable/EnableAutoSizing works for the entire form. This means every call to DisableAutoSizing suspends autosizing for **all** controls on that form. If you write your own control you can now use the following: 
    
    
    procedure TMyRadioGroup.DoSomething;
    begin
      DisableAutoSizing;  // disables not only TMyRadioGroup, but the whole form's autosizing
      try
        // delete items ...
        // add, reorder items ...
        // change item captions ...
      finally
        EnableAutoSizing; // recompute
      end;
    end;
    

Which basically means: you do not have to care. Just call Disable/EnableAutoSizing. 

Note: this is wrong: 
    
    
    Button1.DisableAutoSizing;
    Label1.EnableAutoSizing; // wrong: every control has its own autosizing reference counter
    

### DisableAutoSizing and Form bounds

DisableAutoSizing has another useful effect under asynchronous Window Managers such as you find in Linux systems. Each time a form is resized or moved the bounds are sent to the widget set (DoSendBoundsToInterface). Even if the form is not shown the Handle is resized. The window manager often treats these bounds only as a proposal. The window manager has its own logic and often only the first bounds sent are used. The second, third or further moves may be ignored. With DisableAutoSizing you can make sure that only the final bounds are sent to the widget set making the form bounds more reliable. 

## Example: a button panel

This example combines several of the [LCL](<../LCL.md> "LCL") layout mechanisms to create a panel with three buttons: a _Help_ button to the left and _Ok_ and _Cancel_ buttons to the right. We want the panel to be at the bottom of the form filling the entire width. The buttons and the panel are autosized to fit all fonts and themes. 

Step 1: Create the panel and set its Align property to alBottom. Add three TBitBtns. 

[![Autosize example buttonpanel1.png](https://wiki.freepascal.org/images/7/7e/Autosize_example_buttonpanel1.png)](</File:Autosize_example_buttonpanel1.png>)

Set the _Kind_ properties of the BitBtns to show the glyphs. You might need to set GlyphShowMode to gsmAlways to see them on your platform. Set the BitBtn's _AutoSize_ property to _True_ , which will shrink/enlarge the buttons to fit perfectly around the glyphs and text. Depending on your theme and platform you might notice that the buttons have different heights. 

[![Autosize example buttonpanel2.png](https://wiki.freepascal.org/images/4/49/Autosize_example_buttonpanel2.png)](</File:Autosize_example_buttonpanel2.png>)

Set the _Align_ property of the help button to _alLeft_ and set the other two buttons' _Align_ property to _alRight_. This will enlarge the buttons vertically and move them to the far left/right. alLeft/alRight has no effect on the width, so the buttons use their preferred width. 

[![Autosize example buttonpanel3.png](https://wiki.freepascal.org/images/b/b6/Autosize_example_buttonpanel3.png)](</File:Autosize_example_buttonpanel3.png>)

The Panel height is still fixed. Now set the panel _AutoSize_ property to _True_. The panel now shrinks vertically to fit the tallest button. 

[![Autosize example buttonpanel4.png](https://wiki.freepascal.org/images/3/36/Autosize_example_buttonpanel4.png)](</File:Autosize_example_buttonpanel4.png>)

The _Ok_ button has a short caption, so the button is very small. Set the button's _Constraints.MinWidth_ to _75_. The button will now enlarge somewhat. 

[![Autosize example buttonpanel5.png](https://wiki.freepascal.org/images/3/32/Autosize_example_buttonpanel5.png)](</File:Autosize_example_buttonpanel5.png>)

Now add some space around the buttons. Set the panel's _ChildSizing.LeftTopSpacing/RightBottomSpacing/HorizontalSpacing_ to _6_. 

[![Autosize example buttonpanel6.png](https://wiki.freepascal.org/images/4/46/Autosize_example_buttonpanel6.png)](</File:Autosize_example_buttonpanel6.png>)

Finally clear the _Caption_ of the panel and set its _BevelOuter_ to _bvNone_. 

[![Autosize example buttonpanel7.png](https://wiki.freepascal.org/images/4/4e/Autosize_example_buttonpanel7.png)](</File:Autosize_example_buttonpanel7.png>)

## Scrolling

Some [LCL](<../LCL.md> "LCL") controls like _TScrollBox_ , _TForm_ , and _TFrame_ show scrollbars if the child controls are too big to fit the scrollbox, form or frame. They inherit this behaviour from their ancestor **TScrollingWinControl**. 

A scrolling control's **logical client area** can be bigger than the **visible client area**. The _visible client area_ is **ClientRect**. It always starts at 0,0 and its width and height is the inner area. For example in a TGroupBox it is the size of the area inside the frame. So the following is always true: 
    
    
    ClientWidth <= Width
    ClientHeight <= Height
    

The **logical client area** is defined by the method **GetLogicalClientRect**. By default it is the same as _ClientRect_. When a child control is anchored to the right side, it uses the _logical client area_. _TScrollingWinControl_ overrides this method and returns the _Range_ of the scrollbars if they are bigger than the _ClientRect_. The **Range** can be set manually or automatically with _AutoScroll=true_. An example for _AutoScroll=true_ : 

[![Autoscroll all fit1.png](https://wiki.freepascal.org/images/d/db/Autoscroll_all_fit1.png)](</File:Autoscroll_all_fit1.png>)

The upper button has a fixed Width of 200. The lower button is anchored to the right side of the panel. Therefore the child controls have a preferred width of 200. Because the panel is bigger than 200 the logical client area is bigger too and the lower button expands. 

Now the panel is shrunk, so that the ClientWidth becomes lower than 200: 

[![Autoscroll not fit1.png](https://wiki.freepascal.org/images/2/27/Autoscroll_not_fit1.png)](</File:Autoscroll_not_fit1.png>)

The preferred width is still 200, so the logical client area is now 200 and bigger than the visible client area. The lower button has now a Width of 200 and the panel shows a horizontal scrollbar. 

### Scroll Position

Changing the Position of a scrollbar does not change the Left or Top of any child control, nor does it change the logical client area, nor does it affect autosizing. The child controls are only virtually moved. 

### Scrolling and AutoSize

When AutoSize=true the [LCL](<../LCL.md> "LCL") will expand the control to accommodate all its child controls, and no scrollbars are needed. If the control cannot be expanded, then (only) is the secondary action of AutoSize executed: moving the child controls. 

## Docking

Docking uses the described methods and properties of this page, see [Docking](<../Docking.md> "Docking"). 

## Splitter

See [TSplitter](<../TSplitter.md> "TSplitter"). 

## TLabel.WordWrap

**TLabel.WordWrap** changes the behavior of the preferred size of the label. **WordWrap=true** requires that the Width of the label is fixed, for example by anchoring the left and right size of the label. The preferred height of the label is then computed by breaking the Caption into multiple lines. 

## DPI auto-adjustment and absolute layout auto-adjustment

Historically the [LCL](<../LCL.md> "LCL") has been utilized mostly to design absolute layouts, despite the huge amount of options which Lazarus offers for flexible layouts, like Align, Anchors, etc, as described on the rest of this article. On top of that, it has also historically ignored the DPI of the target and instead utilized values in pixels to measure the left, top, width and height properties of controls. For desktop platforms and Windows CE this has worked reasonably well, but with the advent of [LCL](<../LCL.md> "LCL") support for Android this could no longer be ignored. Starting in Lazarus 0.9.31 the [LCL](<../LCL.md> "LCL") can reinterprete the [LCL](<../LCL.md> "LCL") absolute layout in pixels as a flexible grid. There are multiple modes to choose from and they will allow to reinterprete the pixel values as either really absolute, or as in being adjusted for the DPI or as in being considered simply a fraction of the form size. 

It is important to note that these new rules affect only controls which are positioned without Align and with the most standard Anchors only. 

In the case where the values will be adjusted for DPI, there is a new property: TCustomForm.DesignTimeDPI which should store the DPI value of the system where the form was designed. The positioning values will be expanded when the target DPI is larger than the design time DPI or reduced otherwise. The common value for desktop DPIs is 96 and is the default value given. 

property DesignTimeDPI: Integer read FDesignTimeDPI write FDesignTimeDPI; 

The way in which the layout is adjusted can be controlled with the property TApplication.LayoutAdjustmentPolicy 
    
    
      TLayoutAdjustmentPolicy = (
        lapDefault,     // widget set dependent
        lapFixedLayout, // A fixed absolute layout in all platforms
        lapAutoAdjustWithoutHorizontalScrolling, // Smartphone platforms use this one,
                                                 // the x axis is stretched to fill the screen and
                                                 // the y is scaled to fit the DPI
        lapAutoAdjustForDPI // For desktops using High DPI, scale x and y to fit the DPI
      );
    

And the following new methods in TControl allow to force a layout-autoadjustment in a particular control and all its children or to control how particular descendants of TControl react to this: 
    
    
    TControl = class
    public
    ...
        procedure AutoAdjustLayout(AMode: TLayoutAdjustmentPolicy;
          const AFromDPI, AToDPI, AOldFormWidth, ANewFormWidth: Integer); virtual;
        function ShouldAutoAdjustLeftAndTop: Boolean; virtual;
        function ShouldAutoAdjustWidthAndHeight: Boolean; virtual;
    

LCL-CustomDrawn-Android will call AutoAdjustLayout when the screen rotates, for example. 

## More details

Many controls override _TControl.DoAutoSize_ to perform the actual auto-sizing. 

IMPORTANT: Many Delphi controls override this method and many call this method directly after setting some properties.

During handle creation not all interfaces can create complete Device Contexts which are needed to calculate things like text size.

That's why you should always call _AdjustSize_ instead of DoAutoSize.

TControl.AdjustSize calls DoAutoSize in a smart fashion.

During loading and handle creation the calls are delayed.

This method initially does the same as _TWinControl.DoAutoSize_. But since DoAutoSize is commonly overriden by descendant components, it is not useful to perform all tests, which can result in too much overhead. To reduce this the [LCL](<../LCL.md> "LCL") calls AdjustSize instead.

When setting AutoSize = true the [LCL](<../LCL.md> "LCL") autosizes the control in width and height. This is one of the most complex parts of the [LCL](<../LCL.md> "LCL"), because the result depends on nearly a hundred properties. Let's start simple: 

The [LCL](<../LCL.md> "LCL") will only autosize the _Width_ (Height) if it is free to resize. In other words - the width is not autosized if: 

  * the left and right side is anchored. You can anchor the sides with the **Anchors** property or by setting the _Align_ property to alTop, alBottom or alClient.
  * the _Width_ is bound by the _Constraints_ properties. The Contraints can also be overriden by the widget set. For example the winapi does not allow resizing the height of a combobox. And the gtk widget set does not allow resizing the width of a vertical scrollbar.



Same for _Height_. 

The new size is calculated by the protected method **TControl.CalculatePreferredSize**. This method asks the widget set for an appropriate Width and Height. For example a TButton has preferred Width and Height. A TComboBox has only a preferred Height. The preferred Width is returned as 0 and so the [LCL](<../LCL.md> "LCL") does not autosize the Width - it keeps the width unaltered. Finally a TMemo has no preferred Width or Height. Therefore AutoSize has no effect on a TMemo. 

Some controls override this method. For example the TGraphicControl descendants like TLabel have no window handle and so cannot query the widget set. They must calculate their preferred Width and Height themselves. 

The widget sets must override the GetPreferredSize method for each widget class that has a preferred size (Width or Height or both). 

### Parent.AutoSize

The above described the simple explanation. The real algorithm provides far more possibilities and is therefore far more complex. 

### Properties / Methods

  * Left
  * Top



If Parent<>nil then Left, Top are the pixel distance to the top, left pixel of the parent's client area (not scrolled). Remember the client area is always _without_ the frame and scrollbars of the parent. For Delphi users: Some VCL controls like TGroupbox define the client area as the whole control including the frame and some not - the [LCL](<../LCL.md> "LCL") is more consistent, and therefore Delphi incompatible. Left and Top can be negative or bigger than the client area. Some widget sets define a minimum/maximum somewhere around 10.000 or more. 

When the client area is scrolled the Left and Top are kept unchanged. 

During resizing/moving Left and Top are not always in sync with the coordinates of the Handle object. 

If Parent=nil then Left, Top depend on the widget set and the window manager. Till Lazarus 0.9.25 this is typically the screen coordinate of the left,top of the client area of the form. This is Delphi incompatible. It is planned to change this to the Left, Top of the window. 

  
Hint: Each time you change Left and Top the [LCL](<../LCL.md> "LCL") moves instantly and recomputes the whole layout. If you want to change Left _and_ Top use instead: 
    
    
    with Button1 do
      SetBounds(NewLeft, NewTop, Width, Height);
    

  * Width
  * Height



The Size in pixels must not be negative, and most widget sets do not allow Width=0 and/or Height=0. Some controls on some platforms define a bigger minimum constraint in Constraints.MinInterfaceWidth/Height. Instead of sizing a control to Width=0 and/or Height=0, set Visible=false or Parent=nil. During resizing/moving Width and Height are not always in sync with the size of the Handle object. 

  * BoundsRect



Same as Bounds(Left, Top, Width, Height). 

Common newbie mistake: 
    
    
    BoundsRect.Left := 3; // WRONG: common newbie mistake
    

This has no effect, because reading BoundsRect is a function. It creates a temporary TRect on the stack. The above is the same as 
    
    
    var
      r: TRect;
    begin
      r := BoundsRect; // fetch the bounds
      r.Left := 3;  // change a value on the stack
    end;  // no change
    

  * ClientRect



Left and Top are always 0,0. Width and Height are the visible size in pixels of the client area. Remember the client area is without the frame and without scrollbars. In a scrollable client area the logical client area can be bigger than the visible. 

  * ClientOrigin



Returns the screen coordinate of the topleft coordinate 0,0 of the client area. Note that this value is the position as stored in the interface and is not always in sync with the [LCL](<../LCL.md> "LCL"). When a control is moved, the [LCL](<../LCL.md> "LCL") sets the bounds to the desired position and sends a move message to the interface. It is up to the interface to handle moves instantly or queued. 

  * LCLIntf.GetClientBounds



Returns the client bounds of a control. Like ClientRect, but Left and Top are the pixel distances to the control's left, top. For example on a TGroupBox the Left, Top are the width and height of the left and top frame border. Scrolling has no effect on GetClientBounds. 

  * LCLIntf.GetWindowRect



After the call, ARect will be the control area in screen coordinates. That means, Left and Top will be the screen coordinate of the TopLeft pixel of the Handle object and Right and Bottom will be the screen coordinate of the BottomRight pixel. 

  * FBaseBoundsLock: integer



Increased/Decreased by LockBaseBounds/UnlockBaseBounds. Used to keep FBaseBounds during SetBounds calls. 

  * FBaseParentClientSize: TPoint



The Parent.ClientRect size valid for the FBaseBounds. FBaseBounds and FBaseParentClientSize are used to calculate the distance for akRight (akBottom). When the parent is resized, the [LCL](<../LCL.md> "LCL") knows what distance to keep. 

  * FBoundsRectForNewParent: TRect



When changing the Parent of a control the Handle is recreated and many things can happen. Especially for docking forms the process is too unreliable. Therefore the BoundsRect is saved. The VCL uses a similar mechanism. 

  * fLastAlignedBounds: TRect



See TControl.SetAlignedBounds for an explanation. In short: It stops some circles between interface and [LCL](<../LCL.md> "LCL") autosizing. 

  * FLastChangebounds: TRect



Used to stop calling ChangeBounds with the same coordinates. This happens very often. 

  * FLastDoChangeBounds: TRect



Used to avoid calling OnChangeBounds with the same coordinates. This reduces user defined autosizing. 

  * FLastResizeClientHeight: integer
  * FLastResizeClientWidth: integer
  * FLastResizeHeight: integer
  * FLastResizeWidth: integer



Used to avoid calling OnResize with the same coordinates. This reduces user defined autosizing. 

  * FLoadedClientSize: TPoint



During loading many things are delayed and many things are set and worse: in the wrong order. That's why SetClientWidth/SetClientHeight calls are stored and set at end of loading again. This way the [LCL](<../LCL.md> "LCL") can restore the distances (e.g. akRight) used during designing. 

  * FReadBounds: TRect



Same as FLoadedClientSize, but for SetLeft, SetTop, SetWidth, SetHeight. 

  * procedure SetBoundsRectForNewParent(const AValue: TRect);



Used to set FBoundsRectForNewParent. See above. 

  * procedure SetAlignedBounds(aLeft, aTop, aWidth, aHeight: integer); virtual;



As SetBounds but without changing the default sizes. 

  * procedure SetInitialBounds(aLeft, aTop, aWidth, aHeight: integer); virtual;



A smart version of SetBounds, reducing overhead during creation and loading. 

  * procedure UpdateBaseBounds(StoreBounds, StoreParentClientSize, UseLoadedValues: boolean); virtual;



Commit current bounds to base bounds. 

  * procedure SetClientHeight(Value: Integer);
  * procedure SetClientSize(Value: TPoint);
  * procedure SetClientWidth(Value: Integer);



Exists for Delphi compatibility too. Resizes the control, to get the wanted ClientRect size. 

  * procedure ChangeBounds(ALeft, ATop, AWidth, AHeight: integer); virtual;



This is the internal SetBounds. Applies constraints, updates base bounds, calls OnChangeBound, OnResize, locks bounds. 

  * procedure DoSetBounds(ALeft, ATop, AWidth, AHeight: integer); virtual;



This really sets the FLeft, FTop, FWidth, FHeight private variables. 

  * procedure SetBounds(aLeft, aTop, aWidth, aHeight: integer); virtual;



This is the standard procedure overriden by many Delphi controls. TWinControl overrides it too. 

  *     * ignores calls when bounds are locked
    * lock the FBoundsRealized to avoid overhead to the interface during auto sizing.



ChangeBounds is not locked this way. 

  * Function GetClientOrigin: TPoint; virtual;



Screen coordinate of Left, Top of client area. 

  * Function GetClientRect: TRect; virtual;



Size of client area. (always Left=0, Top=0) 

  * Function GetScrolledClientRect: TRect; virtual;



Visible client area in ClientRect. 

  * function GetChildsRect(Scrolled: boolean): TRect; virtual;



Returns the Client rectangle relative to the control's Left, Top. If Scrolled is true, the rectangle is moved by the current scrolling values (for an example see TScrollingWincontrol). 

  * function GetClientScrollOffset: TPoint; virtual;



Returns the scrolling offset of the client area. 

  * function GetControlOrigin: TPoint; virtual;



Returns the screen coordinate of the topleft coordinate 0,0 of the control area. (The topleft pixel of the control on the screen) Note that this value is the position as stored in the interface and is not always in sync with the [LCL](<../LCL.md> "LCL"). When a control is moved, the [LCL](<../LCL.md> "LCL") sets the bounds to the wanted position and sends a move message to the interface. It is up to the interface to handle moves instantly or queued. 

## FAQ

### Why does AutoSize not work in the designer properly?

In the designer controls can be dragged around and properties can be set in almost any order. To allow this and avoid possible conflicts, the AutoSizing is not updated on every change at design time. 

### Why does TForm.AutoSize not work when something changes?

See [AutoSize and Forms](<_Layout.md> "Autosize / Layout")

### Do I need to call Application.ProcessMessages when creating lots of controls?

Application.ProcessMessages is called by the [LCL](<../LCL.md> "LCL") automatically after every message (e.g. after every event like OnClick). Calling it yourself is only needed if the changes should become visible to the user immediately. For example: 
    
    
    procedure TFrom.Button1Click(Sender: TObject);
    begin
      // change width of a control
      Button1.Width := Button1.Width + 10;
      // apply any needed changes and repaint the button
      Application.ProcessMessages;
      // do a lot of things that takes a long time
      ...
      // after leaving the OnClick the LCL automatically processes messages
    end;
    

### When enabling Anchors at runtime the control resizes, does not use the current values. Why?

akBottom means: keep a distance to the bottom side of the parent. The distance to keep is defined by the base bounds. They are set at designtime or by runtime calls of SetBounds or UpdateBaseBounds. 

For example: A TListBox (Anchors=[akLeft,aTop]) at designtime has a bottom distance of 100 pixel. And a button to enable/disable the akBottom of the TListBox. Now start the application and press the button to enable akBottom. The 100 pixel distance will be activated, because this was the last time the programmer defined the base bounds of the TListBox. All other resizes were done by the [LCL](<../LCL.md> "LCL") and are irrelevant. The programmers base bounds rules. You can resize the form and the 100 pixel will be kept. In order to use the current bounds as base bounds use: 
    
    
    ListBox1.UpdateBaseBounds(true, true, false);
    ListBox1.Anchors := ListBox1.Anchors + [akBottom];
    

Setting Anchors does not automatically call UpdateBaseBounds, because this would destroy the ability to change properties independently. 

### Resizing stringgrid columns in form's OnResize event does not work

The Form's OnResize is triggered when the Form's Width, Height, ClientWidth or ClientHeight changes. This is per se independent of TStringGrid. Of course it often happens that both the Form and the TStringGrid are resized. This means using forms OnResize will often work, but not always. A prominent example where it always fails is when the theme is changed and you have a TStringGrid in a TGroupBox in a TForm. When the theme changed the form size is kept, so no Forms OnResize is triggered. But the TGroupBox changed, so the TStringGrid should be resized. 

Solution: Use the TStringGrid's OnResize. 

### Why is TForm.Width equal to TForm.ClientWidth?

Mattias' notes: 

"There are historical and technical reasons. 

For forms without parent the Clientwidth equals the Width, because the real Width including the frame was not available on Linux ten years ago (at least not reliable on various window managers). I didn't test, but I heard it is now possible with gtk2. The main problem is the autosizing, because this needs the frame sizes before the form is mapped to the screen. It might be, that this is only available after an event, which means that you have to wait for it, which means trouble for ShowModal. Changing this breaks compatibility with a lot of existing [LCL](<../LCL.md> "LCL") code, but for this we added the LCLVersion in the lfm files. 

There is a new compiler define **LCLRealFormBounds** In Lazarus trunk 1.7 that enables real size for a form. To use it just compile your application with LCLRealFormBounds ON. For now, only win32 widget set is supported. 

For all other controls the rules is ClientWidth<=Width. The Width is the ClientWidth plus the widget frame. The question is if the scrollbars belong to the frame. I would say yes and it was implemented that way some time ago. Apparently this has changed. See the current cursor problem on synedit." 

### I get an infinite loop / How to debug autosizing?

Here are some notes, describing what other users did wrong and how to find out: 

#### Differences to Delphi

For Delphi users: Please read: [Lazarus_vs_Delphi](<_Layout.md> "Autosize / Layout")

#### Overriding a [LCL](<../LCL.md> "LCL") method and triggering a recompute

You override a method and tell the [LCL](<../LCL.md> "LCL") to compute again, even if nothing changed. Check for AdjustSize, Realign, AlignControls, InvalidatePreferredSize. For example: 
    
    
    procedure TMainForm.SetBounds(ALeft, ATop, AWidth, AHeight: integer);
    begin
      // This will create an endless loop
      InvalidatePreferredSize;
    
      // This will create an endless loop too:
      OtherComponent.Left:=10;
      OtherComponent.Left:=20;
    
      inherited SetBounds(ALeft, ATop, AWidth, AHeight);
    end;
    

Explanation: The SetBounds method is called often, even without changing anything. For example a "Left:=30;" will do so. 

Remedy: Check for changes: 
    
    
    procedure TMainForm.SetBounds(ALeft, ATop, AWidth, AHeight: integer);
    begin
      if (Left <> ALeft) or (Top <> ATop) or (Width <> AWidth) or (Height <> AHeight) then
        InvalidatePreferredSize;
      inherited SetBounds(ALeft, ATop, AWidth, AHeight);
    end;
    

##### TWinControl.AlignControls
    
    
    procedure AlignControls(aControl: TControl; var aRect: TRect); override;
    

AlignControls moves and sizes all child controls. The [LCL](<../LCL.md> "LCL") implementation ignores controls with Align=alCustom. 

The parameter aControl: TControl is kept for VCL compatibility. The [LCL](<../LCL.md> "LCL") always passes nil. It gives aControl precedence when applying the Align property. When you have for example two controls A,B with Align=alLeft then the one with the lower Left is put left most. If both have the same Left then creation order wins. Now imagine you want to switch both controls A,B in the designer. You drag B to the left. This results in setting B.Left to 0. Now AlignControls starts and find both A.Left=0 and B.Left=0. Normally A would win. To let B win the VCL calls AlignControls(B,r). So aControl is the last moved. Contrary to the VCL the [LCL](<../LCL.md> "LCL") allows to combine multiple layout changes without recomputing on every step. The [LCL](<../LCL.md> "LCL") keeps track of the last moved controls in TWinControl.fAlignControls and applies the order in TWinControl.CreateControlAlignList. The aControl parameter is always nil. 

See TWinControl.CreateControlAlignList. 

#### OnResize/OnChangeBounds conflict with [LCL](<../LCL.md> "LCL") properties

You set bounds that bite the [LCL](<../LCL.md> "LCL") properties. For example a TLabel.AutoSize is true by default. If you set the Label1.Width in an OnResize event, the [LCL](<../LCL.md> "LCL") will recompute, resize the Label1 and call the OnResize again. Start your application in the debugger and reproduce the bug. When it enters the loop, pause the application and see the call stack. If you see one of your events or your methods start searching there. For example: 
    
    
    procedure TMainForm.FormResize(Sender: TObject);
    begin
      // if Button1 is anchored or AutoSize=true then the following might create an endless loop:
      Button1.Width:=100;
    end;
    

#### [LCL](<../LCL.md> "LCL") interface bug, custom widget

Sometimes the [LCL](<../LCL.md> "LCL") interface or your custom control has a bug and undoes the [LCL](<../LCL.md> "LCL") bounds. Compile the [LCL](<../LCL.md> "LCL") with -dVerboseIntfSizing. This will write the communication between [LCL](<../LCL.md> "LCL") and the [LCL](<../LCL.md> "LCL") interface. If a widget does not allow free resizing, it must tell the [LCL](<../LCL.md> "LCL") via the Constraints. Search for SetInterfaceConstraints in the various [LCL](<../LCL.md> "LCL") interfaces and TWSControlClass.ConstraintWidth/Height. 

#### alCustom

You use alCustom. This was only half implemented in earlier Lazarus versions, but some clever programmers used it to create some special effects. Now it is implemented and your program does not work anymore. Please see here what alCustom does: [alCustom](<_Layout.md> "Autosize / Layout"). 

## See also

  * [Docking](<../Docking.md> "Docking")
  * [High DPI](<../High_DPI.md> "High DPI")
  * [Anchor Sides](<../Anchor_Sides.md> "Anchor Sides")

---

_Source: [https://wiki.freepascal.org/Autosize_/_Layout](https://web.archive.org/web/20250320000838/https://wiki.freepascal.org/Autosize_/_Layout)_
