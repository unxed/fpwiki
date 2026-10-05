# IDE Window: Anchor Editor

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Anchor_Editor.md>)** │

  
[![Anchor Editor en.png](https://wiki.freepascal.org/images/2/28/Anchor_Editor_en.png)](</File:Anchor_Editor_en.png>)

  
The anchor editor allows to edit the AnchorSide, Anchors and BorderSpacing properties of the selected controls. 

This page descibes how the editor works. If you want to know, how these properties themselves work, see [Anchor Sides](<Anchor_Sides.md> "Anchor Sides"). 

    Hint: This window is a floating window, you can leave it open while switching between designer and anchor editor.

The 'Enabled' checkboxes correspond to the Anchors property of the control(s). The top side uses the akTop enum, the Left side the akLeft enum and so forth. 

The Sibling is the AnchorSide[xxx].Control property. You can set it to empty (nil), the parent of the control(s) or one of the siblings (controls with the same parent). 

The three speedbuttons correspond to the AnchorSides[xxx].Side property. 

The borderspacing has 5 properties: Around, Left, Top, Right and Bottom. The Around property is the center edit field. The Top is the top edit field and so forth.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Anchor_Editor](https://web.archive.org/web/20190919050918/https://wiki.freepascal.org/IDE_Window%3A_Anchor_Editor)_
