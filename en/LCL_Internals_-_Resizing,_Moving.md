# LCL Internals - Resizing, Moving

│ **English (en)** │  **[français (fr)](</LCL_Internals_-_Resizing,_Moving/fr> "LCL Internals - Resizing, Moving/fr")** │    
****

## Contents

  * 1 Overview
  * 2 How the LCL tells the interface to resize/move a handle
  * 3 How the Interface tells the LCL to resize/move a handle
    * 3.1 When the client area of a Handle was resized
    * 3.2 When a Handle of a TWinControl was resized/moved
      * 3.2.1 First the LCL interface send a LM_WINDOWPOSCHANGED message
      * 3.2.2 Then a LM_SIZE message is sent
      * 3.2.3 Then a LM_MOVE message is sent
    * 3.3 When a form is maximized, minimized or restored the interface sends a LM_SIZE message
  * 4 How the LCL gets the current size / position of a LCL interface handle
    * 4.1 LCLIntf.GetWindowSize(Handle, InterfaceWidth, InterfaceHeight)
    * 4.2 LCLIntf.GetClientRect(Handle, Result)
    * 4.3 TWSWinControlClass(WidgetSetClass).GetDefaultClientRect(Self, Left, Top, Width, Height, Result)
    * 4.4 GetWindowRelativePosition(Handle,NewLeft,NewTop)
  * 5 Constraints
  * 6 Preferred Size
  * 7 Scrolling
  * 8 Rectangles
  * 9 Miscellaneous
    * 9.1 LCLIntf.GetClientBounds(Handle,Result);



## Overview

The LCL has a lot of resizing properties (Left, Width, Anchors, Align, AnchorSide, AutoSize, Constraints, ChildSizing, ...) and hooks where applications can alter the behavior (OnResize, OnChangeBounds, ...). All these things can not be calculated in one step, so controls can move quite a lot before the final coordinates come out. To reduce flickering the LCL does not send every move/resize to the LCL interface. For example during Begin/EndAlign and csLoading no move/resize is sent to the interface. The widgetset will try to follow the advices of the LCL, but there are some cases, where it does not follow. For example forms (top level windows) are limited by the window manager policies. And TPage completely depends on the size and theme of the TNotebook. 

## How the LCL tells the interface to resize/move a handle

In TWinControl.DoSendBoundsToInterface the widget function SetBounds is called 
    
    
    TWSWinControlClass(WidgetSetClass).SetBounds(Self, Left, Top, Width, Height);
    

## How the Interface tells the LCL to resize/move a handle

### When the client area of a Handle was resized

_Note_ : If the theme changes the frame of a TGroupBox can change. This leaves the Size and Position of the TGroupBox unchanged, but the client area can change. The interface calls 
    
    
     LCLControl.InvalidateClientRectCache(false);
    

and sends a LM_SIZE message as below. 

### When a Handle of a TWinControl was resized/moved

#### First the LCL interface send a LM_WINDOWPOSCHANGED message

  * x := Left (relative to 0,0 of client area of parent)
  * y := Top
  * cx := Width
  * cy := Height



#### Then a LM_SIZE message is sent

  * Width
  * Height
  * SizeType is a bit flag containing Size_SourceIsInterface plus one of the values SIZENORMAL, SIZEICONIC, SIZEFULLSCREEN.



#### Then a LM_MOVE message is sent

  * MoveType := Move_SourceIsInterface;
  * XPos := Left (relative to 0,0 of client area of parent)
  * YPos := Top



### When a form is maximized, minimized or restored the interface sends a LM_SIZE message

  * Width
  * Height
  * SizeType is a bit flag containing Size_SourceIsInterface plus one of the values SIZENORMAL, SIZEICONIC, SIZEFULLSCREEN.



## How the LCL gets the current size / position of a LCL interface handle

### LCLIntf.GetWindowSize(Handle, InterfaceWidth, InterfaceHeight)

Returns the current width and height of a Handle. 

### LCLIntf.GetClientRect(Handle, Result)

Returns the current width and height (left and top are always 0) of the client area of a Handle (= inner area inside the border/frame of a control). 

### TWSWinControlClass(WidgetSetClass).GetDefaultClientRect(Self, Left, Top, Width, Height, Result)

This function is called first by the LCL to get the ClientRect. If it returns false, the LCL will use GetClientRect if it has already created a handle, otherwise the values loaded from the .lfm. If all this fails, the default is Rect(0,0,Width,Height). GetDefaultClientRect can be used by the LCL interface to reduce flickering by providing good values before the handle is created. 

### GetWindowRelativePosition(Handle,NewLeft,NewTop)

Returns the current Left, Top of a Handle relative to the client area (0,0) of its parent. 

## Constraints

The LCL asks the interface for constraints (min, max for width and height) with the function 
    
    
    GetControlConstraints
    

For example: Under gtk a horizontal scrollbar has a fixed height. 

## Preferred Size

To autosize a control (e.g. a TButton or a TLabel) the LCL asks the interface about the minimum size to render a control nicely: 
    
    
    TWSWinControlClass(WidgetSetClass).GetPreferredSize(Self, PreferredWidth, PreferredHeight, WithThemeSpace);
    

## Scrolling

The LCL scrolls child controls only virtually. That means, their Left,Top properties do not change. To scroll it uses the LCL interface function 
    
    
    TWSScrollingWinControlClass(WidgetSetClass).ScrollBy(Self, DeltaX, DeltaY);
    

## Rectangles

  * BoundsRect - the rectangle containing coordinates of the control within parents coordinates



    for TForm (not embedded into another control) coordinates are given in Screen position. The form non-client area (aka header, aka title) section is also NOT PART of the BoundsRect.

  * ClientsRect - the rectangle containing coordinates of the client area.



## Miscellaneous

### LCLIntf.GetClientBounds(Handle,Result);

Returns the unscrolled client rectangle relative to the top, left of the Handle. In other words it is equivalent to: 
    
    
    CurClientBounds := GetClientRect(Handle)
    OffsetRect(CurClientBounds,FrameBorderLeft,FrameBorderTop);

---

_Source: [https://wiki.freepascal.org/LCL_Internals_-_Resizing%2C_Moving](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/LCL_Internals_-_Resizing%2C_Moving)_
