# BidiMode

Some languages (Arabic, Hebrew, Farsi ...) write the characters from the right to the left, and adequate support for this must be implemented on Lazarus. 

## Contents

  * 1 BidiMode
    * 1.1 bdLeftToRight
    * 1.2 bdRightToLeft
    * 1.3 bdRightToLeftNoAlign
    * 1.4 bdRightToLeftReadingOnly
  * 2 ParentBidiMode
  * 3 BidiMode for developers
  * 4 Add BidiMode support to LCL components
    * 4.1 Add BidiMode property to TControl
    * 4.2 Bidi Controls Road Map
    * 4.3 Functions and Utils useful for multi language application
  * 5 How to test RightToLeft reading
  * 6 BiDi support of the widgetsets
    * 6.1 Carbon
    * 6.2 fpGui
    * 6.3 Gtk1
    * 6.4 Gtk2
    * 6.5 Qt
    * 6.6 Windows
  * 7 References



#### BidiMode

BidiMode can have these settings: 
    
    
    bdLeftToRight
    bdRightToLeft
    bdRightToLeftNoAlign
    bdRightToLeftReadingOnly
    

##### bdLeftToRight

Default of BidiMode, which means read and order is normal "Left To Right" 

##### bdRightToLeft

Makes a control Right To Left reading text and right alignment depend a kind of the control, 

In Delphi if there is Alignment property effect by reverse taLeft to taRight and vise versa, but in Lazarus we will reverse the alignment with FlipControls function, which is more consistent with Anchors and Align properties. Also bdRightToLeft takes a Scrollbar at the left and if there is a cell or menus they must ordered from the right. 
    
    
     For Example
     in bdLeftToRight:
       Menu1, Menu2, Menu2
     in bdRightToLeft: 
       Menu3, Menu2, Menu1
    

That means not just Right Alignment; it is Right To Left Order. bdRightToLeft must effect the text reading as depend on the OS if it is supported, the sentence words take as like Cells 
    
    
     in bdLeftToRight:
       Word1 Word2 Word2
     in bdRightToLeft: 
       Word3 Word2 Word1
    

We are reading the Word1 first then Word2, We read from the Right to Left, but Draw Text is more complicated than normal Cells; in fact the function that draws the text cares about English/Latin words and draws it Left To Right, so if we mixed both languages in the same sentence, the draw function draws continual English words in Left To Right order to make it readable, but symbols like ? or ! are considered as Right To Left characters. 

For testing purpose I used English word with a symbol to test Right To Left text reading 

E.g. _Word?_

For special states or for more compatibility with Delphi there are other values (possible? used? handy?) 

##### bdRightToLeftNoAlign

It is Right to Left but not reversed (except the Alignment property), but in Lazarus worked when use FlipControls 

##### bdRightToLeftReadingOnly

It is Right to Left text reading only; scrollbar and alignment do not take effect (_??? remain left to right???_). 

### ParentBidiMode

If this is True the control inherits the BidiMode value from its parent. 

### BidiMode for developers

When you build you own controls and try to make your control support Right to Left or BidiMode, you must not access the BiDiMode property directly. Instead you must use 
    
    
    function UseRightToLeftAlignment: Boolean; virtual;
    function UseRightToLeftReading: Boolean; virtual;
    function UseRightToLeftScrollBar: Boolean;
    function IsRightToLeft: Boolean;
    

Because some controls do not need to make changes to the alignment - for example TButton it always is centered, or TMainMenu which always is right aligned if it is Right to Left. 

  
Not just reading the words and cells, everything depends on the order (positions). For an application there is a Menus and Controls that when placed on the form must have positions starting from the right. It is like looking at a LeftToRight Form in a mirror. The flow of the Form must be from Right To Left. 

[![Sample 1](https://wiki.freepascal.org/images/2/24/BidiMode_Sample_1.png)](</File:BidiMode_Sample_1.png> "Sample 1")

## Add BidiMode support to LCL components

Most OSes now support RightToLeft but there are controls that are not supported yet or there is controls made by native language (_custom drawn?_) such as (TLabel, TGrid). So we have 3 kinds of control: 

  1. Standard Controls: it is easy to enable RTL support: just add some flag to switch it to RightToLeft (TEdit, TList, TComboBox, TCheckBox).
  2. Standard Controls not supported by the OS: There is no idea just waiting the OS developers to implement it to support (TListView, TTreeView), or use native controls that already support it.
  3. Native Controls: hard work is needed to make it Support RightToLeft, or we must add new controls that already have this features (TLabel, TGrid).



### Add BidiMode property to TControl

_TBidiMode is already declared in Classes_
    
    
    property BiDiMode: TBiDiMode read FBiDiMode write SetBiDiMode stored IsBiDiModeStored;
    property ParentBiDiMode: Boolean read FParentBiDiMode write SetParentBiDiMode default True;
    

BidiMode must not be stored if ParentBidiMode = True 
    
    
    function TControl.IsBiDiModeStored: Boolean;
    begin
      Result := not ParentBiDiMode;
    end;
    

**Override BidiMode functions**
    
    
    function IsRightToLeft:Boolean; override;
    function UseRightToLeftAlignment: Boolean; override;
    function UseRightToLeftReading: Boolean; override;
    function UseRightToLeftScrollBar: Boolean; override;
    

_BiDiMode property is public and must published in every control need to RightToLeft_

### Bidi Controls Road Map

GTK2 You must build LCL with OPT=-dGTK2 OPT=-dGTK_2_8 

  


Component | win32 | gtk2 | carbon | qt | wince | cocoa   
---|---|---|---|---|---|---  
TForm  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TLabel  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TButton  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TEdit  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TListBox  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TComboBox  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TCheckBox  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TStaticText  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TGroupBox  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TRadioButton  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
Menus  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TBitBtn  | Partially Implemented  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TSpeedBtn  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TRadioGroup  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TCheckGroup  | Working  | Working  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
TGrid  | Working  | Partially Implemented  | Not Implemented  | Not Implemented  | Not Implemented  | Not Implemented   
  
### Functions and Utils useful for multi language application
    
    
     **TControl.FlipControls; virtual**
     Change Right to Left and Left to Right in this properties (Left, Align, Anchors and Alignment)
    

## How to test RightToLeft reading

Create a new form, then add a TButton or TEdit. Set the caption to **OK?**. You must add **?** or **!** or **.** to the last word without a space. Now set the BidiMode to bdRightToLeft, and you should see the ? in the left of word if it works. 

# BiDi support of the widgetsets

## Carbon

Write me. 

## fpGui

Write me. 

## Gtk1

Has almost no BiDi support. Is deprecated by Lazarus anyway, so no new development at all. 

## Gtk2

In TMemo, TEdit, TComboBox the popup menu contains the two sub menus _Input Methods_ and _Insert Unicode Control character_. Gtk2 uses for text rendering the **Pango** library, which has excellent Unicode support. 

## Qt

Write me. 

## Windows

In Windows 7 you can switch in TMemo, TEdit, TComboBox the base direction with Left-`Ctrl`+Right-`⇧ Shift` to RTL (right to left) and Right-`Ctrl`+Left-`⇧ Shift` to LTR (left to right). This also works in the IDE for most things, like in the Property Editor and the Strings Editor Dialog. 

# References

  * [Unicode Bidirectional Algorithm](<http://www.unicode.org/reports/tr9/>)
  * [Design and Implementation of a Win32 Text Editor](<http://www.catch22.net/book/export/html/14>)

---

_Source: [https://wiki.freepascal.org/BidiMode](https://web.archive.org/web/20231129032138/https://wiki.freepascal.org/BidiMode)_
