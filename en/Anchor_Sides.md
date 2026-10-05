# Anchor Sides

│ **[Deutsch (de)](</Anchor_Sides/de> "Anchor Sides/de")** │  **English (en)** │  **[français (fr)](</Anchor_Sides/fr> "Anchor Sides/fr")** │  **[日本語 (ja)](</Anchor_Sides/ja> "Anchor Sides/ja")** │  **[русский (ru)](<../ru/Anchor_Sides.md> "Anchor Sides/ru")** │    
****

There are some new [properties](</Property> "Property") and [methods](<Method.md> "Method") for automatic layout of controls. You can now setup controls to keep a certain distance to other controls, or center relative to other controls. See below for examples. 

Each of the four sides of a control (Left, Top, Right, Bottom) can now be anchored/bound to a side of another control. For example you can now anchor the left side of [TEdit](<TEdit.md> "TEdit") to the right side of a [TLabel](<TLabel.md> "TLabel"). Everytime the Label is moved or resized the Edit's left side will follow, which normally results in moving the Edit parallel to the Label. 

## Contents

  * 1 Example 1
    * 1.1 In code
    * 1.2 Notes
    * 1.3 Via the Anchor Editor
  * 2 Example 2
  * 3 Example 3
  * 4 Example 4
  * 5 New property
  * 6 New methods to easily configure common layouts
  * 7 Anchoring to invisible controls
  * 8 Circular references
  * 9 See also



## Example 1
    
    
    +--------+ +-------+
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    

### In code
    
    
    Edit1.AnchorSide[akLeft].Side := asrRight;
    Edit1.AnchorSide[akLeft].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akLeft];
    

You can define the distance with the BorderSpacing properties: 
    
    
    Edit1.BorderSpacing.Left := 10;
    

The same can be done with the method: 
    
    
    Edit1.AnchorToNeighbour(akLeft, 10, Label1);
    

### Notes

The Edit1.Left will follow Label1.Left+Label1.Width, not the other way around. That means, moving Label1 will move Edit1. But moving Edit1 will be undone by the [LCL](<LCL.md> "LCL"). If you also anchor the right side of Label1 to the left side of Edit1, you created a circle, and this can result together with some other autosize properties in a loop. This is ignored by the LCL or automagically repaired when the Parent.AutoSize is [true](<True.md> "True"). 

### Via the Anchor Editor

The anchor editor is a floating window that is available via the menu **View / Anchor Editor** or via the button on the **Anchors** property in the object inspector. 

[![Anchor Editor en.png](https://wiki.freepascal.org/images/2/28/Anchor_Editor_en.png)](</File:Anchor_Editor_en.png>)

## Example 2

You can anchor the Edit's top side to follow the Label's top side: 
    
    
    +--------+ +-------+
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    
    
    
    Edit1.AnchorSide[akTop].Side := asrTop;
    Edit1.AnchorSide[akTop].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akTop];
    

The same can be done with the method: 
    
    
    Edit1.AnchorParallel(akTop,0,Label1);
    

## Example 3

Centering a Label vertically to an Edit: 
    
    
               +-------+
    +--------+ |       |
    | Label1 | | Edit1 |
    +--------+ |       |
               +-------+
    
    
    
    Edit1.AnchorSide[akTop].Side := asrCenter;
    Edit1.AnchorSide[akTop].Control := Label1;
    Edit1.Anchors := Edit1.Anchors + [akTop] - [akBottom];
    

The same can be done with the method: 
    
    
    Edit1.AnchorVerticalCenterTo(Label1);
    

Obviously anchoring the bottom side of Edit1 does not make sense when centering. 

## Example 4

Using a [TSplitter](<TSplitter.md> "TSplitter") with the AnchorEditor between two panels: the left panel should behave as if it had `Align=alLeft`, the right panel should behave as if it had `Align=alClient`. 

  * Do not use the `Align` properties of the panels and of the splitter. They must be set to `alNone`. This is important for the splitter for which the default value is `alLeft`.
  * In the anchor editor do the following adjustments: 
    * Anchor the top and bottom sides of the panels and the splitter to the corresponding sides of the form.
    * Select the first panel and anchor its left side to the left side of the form and its right side to the left side of the splitter.
    * Select the second panel and anchor its right side to the right side of the form and its left side to the right side of the splitter.
    * The left and right sides of the splitter must not be anchored (as seen from the splitter itself) because this would fix the position of the splitter and make it not movable.



The same effect can be achieved by this code: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      with Panel1 do
      begin
        AnchorSideTop.Control := Self;
        AnchorSideBottom.Control := Self;
        AnchorSideBottom.Side := asrBottom;
        AnchorSideLeft.Control := Self;
        AnchorSideRight.Control := Splitter1;
        Anchors := [akLeft, akRight, akTop, akBottom];
      end;
      with Panel2 do
      begin
        AnchorSideTop.Control := Self;
        AnchorSideBottom.Control := Self;
        AnchorSideBottom.Side := asrBottom;
        AnchorSideLeft.Control := Splitter1;
        AnchorSideLeft.Side := asrBottom;
        AnchorSideRight.Control := self;
        AnchorSideRight.Side := asrBottom;
        Anchors := [akLeft, akRight, akTop, akBottom];
      end;
      with Splitter1 do
      begin
        AnchorSideTop.Control := Self;
        Splitter1.AnchorSideBottom.Control := Self;
        AnchorSideBottom.Side := asrBottom;
        Anchors := [akTop, akBottom];
      end;
    end;
    

## New property
    
    
    property AnchorSide[Kind: TAnchorKind]: TAnchorSide read GetAnchorSide;
    

This is not published in the [object inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). You can edit it in the designer via the [anchor editor](<IDE_Window__Anchor_Editor.md> "IDE Window: Anchor Editor") (Menu: View -> View anchor editor, or click on button of 'Anchors' property). 

## New methods to easily configure common layouts
    
    
    procedure AnchorToNeighbour(Side: TAnchorKind; Space: integer; Sibling: TControl);
    procedure AnchorParallel(Side: TAnchorKind; Space: integer; Sibling: TControl);
    procedure AnchorHorizontalCenterTo(Sibling: TControl);
    procedure AnchorVerticalCenterTo(Sibling: TControl);
    procedure AnchorAsAlign(TheAlign: TAlign; Space: Integer);
    

AnchorVerticalCenterTo works with Parent too. Then it will center on the client area, that means the center of the control is at _ClientHeight div 2_. 

Center anchoring is not yet fully supported when computing the size of the parent. For example when you put a label center anchored into a [Groupbox1](<TGroupBox.md> "TGroupBox") and set Groupbox1.AutoSize to true then Groupbox1 height will shrink leaving no space for the label. The solution is to center to a control that is not centered. For example center a Label1 to a [ComboBox](<TComboBox.md> "TComboBox") and use for the ComboBox the default anchors (Anchors=[akLeft,akTop]). 

## Anchoring to invisible controls

Anchoring to invisible controls works intuitively. For example: The below controls A, B, C are anchored (C.Left to B.Right and B.Left to A.Right): 
    
    
     +---+ +---+ +---+
     | A | | B | | C |
     +---+ +---+ +---+
    

If B is hidden (Visible:=[false](<False.md> "False")) then C.Left will skip B and use the right of A, resulting in 
    
    
     +---+ +---+
     | A | | C |
     +---+ +---+
    

## Circular references

You can build circular references by anchoring two sides to each other, which creates an impossible anchoring. The LCL notices that, but will not raise an exception, because it can be a temporary circle. For example: A row of buttons. Button1 is left of Button2, Button2 is left of Button3. Now the order is changed to: Button3, Button1, Button2. If the reanchoring is started with Button3, a circle is created temporarily and is fixed when the reordering is completed. The LCL detects circles and will not move the Button. Circles are not the only anomaly. 

There is one exception to this rule. If the Parent.AutoSize is true, then the LCL will automatically break circles on autosize and writes a warning via [debugln](<DebugLn.md> "DebugLn") (windows: --debug-log.txt, linux, et al: stdout). Which anchor is broken depends on the order of controls and the sides. So, temporary circles are still allowed with AutoSize=true if you enclose the change in 
    
    
    Parent.DisableAutosizing;
    try
      // change anchors, aligns, bounds...
    finally
      Parent.EnableAutosizing;
    end;
    

The AutoSize algorithm does not only break circles, but fixes Align/AnchorSide inconsistencies too. 

Eventually it would be good to add some hints in the designer or a tool to list the circles and inconsistencies. 

## See also

  * [Autosize / Layout](<Autosize_/_Layout.md> "Autosize / Layout")
  * [Example: Anchors. How to reliably align dynamically created controls under changing visibility](<Example__Anchors.md> "Example: Anchors. How to reliably align dynamically created controls under changing visibility")

---

_Source: [https://wiki.freepascal.org/Anchor_Sides](https://web.archive.org/web/20240913201316/https://wiki.freepascal.org/Anchor_Sides)_
