# Colors

│ **[Deutsch (de)](</Colors/de> "Colors/de")** │  **English (en)** │  **[español (es)](</Colors/es> "Colors/es")** │  **[suomi (fi)](</Colors/fi> "Colors/fi")** │  **[français (fr)](</Colors/fr> "Colors/fr")** │  **[日本語 (ja)](</Colors/ja> "Colors/ja")** │  **[русский (ru)](<../ru/Colors.md> "Colors/ru")** │  **[中文（中国大陆） (zh_CN)](</Colors/zh_CN> "Colors/zh CN")** │    
****

## Contents

  * 1 Overview
    * 1.1 Conversions between TColor and RGB values
    * 1.2 Convert TColor to/from string
    * 1.3 Convert TColor to/from HTML string #rrggbb
  * 2 Table of standard colors
  * 3 System colors
    * 3.1 Example: clInfoBk, clInfoText
    * 3.2 Theme changes
    * 3.3 Table of system colors
    * 3.4 Finding the rgb values of a system color
  * 4 Drawing theme elements on your custom controls



## Overview

Since FPC 3.2.0, Free Pascal defines TColor and TColorRec as well as the 140 standard Color names in the Delphi compatible System.UItypes unit, to allow non visual code to use the same type as visual code. Lazarus hasn't caught on yet, because the current Lazarus versions still support 3.0.x. 

In the [LCL](<LCL.md> "LCL") TColor is the standard color type. It is compatible with [Delphi](<Delphi.md> "Delphi")'s TColor. TColor can represent _either_ an RGB (3x8bit) value, _or_ a **system** color like **clDefault**. The LCL can also work with the fpImage system which uses the TFPColor type (which is RGBA (4x16bit), not RGB (3x8bit) like TColor). 

### Conversions between TColor and RGB values

The **Graphics** [unit](<Unit.md> "Unit") provides the following [functions](<Function.md> "Function"): 
    
    
    function ColorToRGB(Color: TColor): Longint;
    function Blue(rgb: TColor): BYTE; // does not work on system color
    function Green(rgb: TColor): BYTE; // does not work on system color
    function Red(rgb: TColor): BYTE; // does not work on system color
    function RGBToColor(R, G, B: Byte): TColor;
    procedure RedGreenBlue(rgb: TColor; out Red, Green, Blue: Byte); // does not work on system color
    function FPColorToTColor(const FPColor: TFPColor): TColor;
    function TColorToFPColor(const c: TColor): TFPColor; // does not work on system color
    function IsSysColor(AColor: TColorRef): Boolean;
    

### Convert TColor to/from string

Functions to convert strings like "25500" or "[$](<Dollar_sign.md> "Dollar sign")AA0088" or "clNavy" to TColor: 

  * StringToColor
  * StringToColorDef



To convert TColor to a nice string like "clNavy" or "$AA0002": 

  * ColorToString



### Convert TColor to/from HTML string #rrggbb

See code in [Convert color to/from HTML](<Convert_color_to/from_HTML.md> "Convert color to/from HTML"). 

## Table of standard colors

About 20 predefined color [constants](<Constant.md> "Constant") are provided, which are Delphi-compatible: 

Colour constant  | Meaning  | [Hexadecimal](<Hexadecimal.md> "Hexadecimal") value for use with TColor  | Example   
---|---|---|---  
clBlack  | Black  | TColor($000000);  |   
clMaroon  | Maroon  | TColor($000080);  |   
clGreen  | Green  | TColor($008000);  |   
clOlive  | Olive Green  | TColor($008080);  |   
clNavy  | Navy Blue  | TColor($800000);  |   
clPurple  | Purple  | TColor($800080);  |   
clTeal  | Teal  | TColor($808000);  |   
clGray  | Grey  | TColor($808080);  |   
clSilver  | Silver  | TColor($C0C0C0);  |   
clRed  | Red  | TColor($0000FF);  |   
clLime  | Lime Green  | TColor($00FF00);  |   
clYellow  | Yellow  | TColor($00FFFF);  |   
clBlue  | Blue  | TColor($FF0000);  |   
clFuchsia  | Fuchsia  | TColor($FF00FF);  |   
clAqua  | Aqua  | TColor($FFFF00);  |   
clLtGray  | Light Grey  | TColor($C0C0C0);  | clSilver alias   
clDkGray  | Dark Grey  | TColor($808080);  | clGray alias   
clWhite  | White  | TColor($FFFFFF);  |   
clCream  | Cream  | TColor($F0FBFF);  | Lazarus 1.2 and newer   
clMedGray  | Medium Grey  | TColor($A4A0A0);  | Lazarus 1.2 and newer   
clMoneyGreen  | Mint Green  | TColor($C0DCC0);  | Lazarus 1.2 and newer   
clSkyBlue  | Sky Blue  | TColor($F0CAA6);  | Lazarus 1.2 and newer   
  
  
In FPC 3.2.0+ defines some other constants for w3c colors in System.Uitypes. Note that in FPC 3.2.0 and 3.2.2 the red and blue bytes are exchanged. This has been corrected in FPC 3.2.4+. The following table shows the correct byte order: 

Colour constant  | Meaning  | [Hexadecimal](<Hexadecimal.md> "Hexadecimal") value for use with TColor  | Example   
---|---|---|---  
AntiqueWhite  | AntiqueWhite  | TColor($D7EBFA);  | FPC 3.2.0 and newer   
Aqua  | Aqua  | TColor($FFFF00);  | FPC 3.2.0 and newer   
Aquamarine  | Aquamarine  | TColor($D4FF7F);  | FPC 3.2.0 and newer   
Azure  | Azure  | TColor($FFFFF0);  | FPC 3.2.0 and newer   
Beige  | Beige  | TColor($DCF5F5);  | FPC 3.2.0 and newer   
Bisque  | Bisque  | TColor($C4E4FF);  | FPC 3.2.0 and newer   
Black  | Black  | TColor($000000);  | FPC 3.2.0 and newer   
BlanchedAlmond  | BlanchedAlmond  | TColor($CDEBFF);  | FPC 3.2.0 and newer   
Blue  | Blue  | TColor($FF0000);  | FPC 3.2.0 and newer   
BlueViolet  | BlueViolet  | TColor($E22B8A);  | FPC 3.2.0 and newer   
Brown  | Brown  | TColor($2A2AA5);  | FPC 3.2.0 and newer   
BurlyWood  | BurlyWood  | TColor($87B8DE);  | FPC 3.2.0 and newer   
CadetBlue  | CadetBlue  | TColor($A09E5F);  | FPC 3.2.0 and newer   
Chartreuse  | Chartreuse  | TColor($00FF7F);  | FPC 3.2.0 and newer   
Chocolate  | Chocolate  | TColor($1E69D2);  | FPC 3.2.0 and newer   
Coral  | Coral  | TColor($FF7F50);  | FPC 3.2.0 and newer   
CornflowerBlue  | CornflowerBlue  | TColor($ED9564);  | FPC 3.2.0 and newer   
Cornsilk  | Cornsilk  | TColor($DCF8FF);  | FPC 3.2.0 and newer   
Crimson  | Crimson  | TColor($3C14DC);  | FPC 3.2.0 and newer   
Cyan  | Cyan  | TColor($FFFF00);  | FPC 3.2.0 and newer   
DarkBlue  | DarkBlue  | TColor($8B0000);  | FPC 3.2.0 and newer   
DarkCyan  | DarkCyan  | TColor($8B8B00);  | FPC 3.2.0 and newer   
DarkGoldenRod  | DarkGoldenRod  | TColor($0B86B8);  | FPC 3.2.0 and newer   
DarkGray  | DarkGray  | TColor($A9A9A9);  | FPC 3.2.0 and newer   
DarkGreen  | DarkGreen  | TColor($006400);  | FPC 3.2.0 and newer   
DarkGrey  | DarkGrey  | TColor($A9A9A9);  | FPC 3.2.0 and newer   
DarkKhaki  | DarkKhaki  | TColor($6BB7BD);  | FPC 3.2.0 and newer   
DarkMagenta  | DarkMagenta  | TColor($8B008B);  | FPC 3.2.0 and newer   
DarkOliveGreen  | DarkOliveGreen  | TColor($2F6B55);  | FPC 3.2.0 and newer   
DarkOrange  | DarkOrange  | TColor($008CFF);  | FPC 3.2.0 and newer   
DarkOrchid  | DarkOrchid  | TColor($CC3299);  | FPC 3.2.0 and newer   
DarkRed  | DarkRed  | TColor($00008B);  | FPC 3.2.0 and newer   
DarkSalmon  | DarkSalmon  | TColor($7A96E9);  | FPC 3.2.0 and newer   
DarkSeaGreen  | DarkSeaGreen  | TColor($8FBC8F);  | FPC 3.2.0 and newer   
DarkSlateBlue  | DarkSlateBlue  | TColor($8B3D48);  | FPC 3.2.0 and newer   
DarkSlateGray  | DarkSlateGray  | TColor($4F4F2F);  | FPC 3.2.0 and newer   
DarkSlateGrey  | DarkSlateGrey  | TColor($4F4F2F);  | FPC 3.2.0 and newer   
DarkTurquoise  | DarkTurquoise  | TColor($D1CE00);  | FPC 3.2.0 and newer   
DarkViolet  | DarkViolet  | TColor($D30094);  | FPC 3.2.0 and newer   
DeepPink  | DeepPink  | TColor($9314FF);  | FPC 3.2.0 and newer   
DeepSkyBlue  | DeepSkyBlue  | TColor($FFBF00);  | FPC 3.2.0 and newer   
DimGray  | DimGray  | TColor($696969);  | FPC 3.2.0 and newer   
DimGrey  | DimGrey  | TColor($696969);  | FPC 3.2.0 and newer   
DodgerBlue  | DodgerBlue  | TColor($FF901E);  | FPC 3.2.0 and newer   
FireBrick  | FireBrick  | TColor($2222B2);  | FPC 3.2.0 and newer   
FloralWhite  | FloralWhite  | TColor($F0FAFF);  | FPC 3.2.0 and newer   
ForestGreen  | ForestGreen  | TColor($228B22);  | FPC 3.2.0 and newer   
Fuchsia  | Fuchsia  | TColor($FF00FF);  | FPC 3.2.0 and newer   
Gainsboro  | Gainsboro  | TColor($DCDCDC);  | FPC 3.2.0 and newer   
GhostWhite  | GhostWhite  | TColor($FFF8F8);  | FPC 3.2.0 and newer   
Gold  | Gold  | TColor($00D7FF);  | FPC 3.2.0 and newer   
GoldenRod  | GoldenRod  | TColor($20A5DA);  | FPC 3.2.0 and newer   
Gray  | Gray  | TColor($808080);  | FPC 3.2.0 and newer   
Green  | Green  | TColor($008000);  | FPC 3.2.0 and newer   
GreenYellow  | GreenYellow  | TColor($2FFFAD);  | FPC 3.2.0 and newer   
Grey  | Grey  | TColor($808080);  | FPC 3.2.0 and newer   
HoneyDew  | HoneyDew  | TColor($F0FFF0);  | FPC 3.2.0 and newer   
HotPink  | HotPink  | TColor($B469FF);  | FPC 3.2.0 and newer   
IndianRed  | IndianRed  | TColor($5C5CCD);  | FPC 3.2.0 and newer   
Indigo  | Indigo  | TColor($82004B);  | FPC 3.2.0 and newer   
Ivory  | Ivory  | TColor($F0FFFF);  | FPC 3.2.0 and newer   
Khaki  | Khaki  | TColor($8CE6F0);  | FPC 3.2.0 and newer   
Lavender  | Lavender  | TColor($FAE6E6);  | FPC 3.2.0 and newer   
LavenderBlush  | LavenderBlush  | TColor($F5F0FF);  | FPC 3.2.0 and newer   
LawnGreen  | LawnGreen  | TColor($00FC7C);  | FPC 3.2.0 and newer   
LemonChiffon  | LemonChiffon  | TColor($CDFAFF);  | FPC 3.2.0 and newer   
LightBlue  | LightBlue  | TColor($E6D8AD);  | FPC 3.2.0 and newer   
LightCoral  | LightCoral  | TColor($8080F0);  | FPC 3.2.0 and newer   
LightCyan  | LightCyan  | TColor($FFFFE0);  | FPC 3.2.0 and newer   
LightGoldenRodYellow  | LightGoldenRodYellow  | TColor($D2FAFA);  | FPC 3.2.0 and newer   
LightGray  | LightGray  | TColor($D3D3D3);  | FPC 3.2.0 and newer   
LightGreen  | LightGreen  | TColor($90EE90);  | FPC 3.2.0 and newer   
LightGrey  | LightGrey  | TColor($D3D3D3);  | FPC 3.2.0 and newer   
LightPink  | LightPink  | TColor($C1B6FF);  | FPC 3.2.0 and newer   
LightSalmon  | LightSalmon  | TColor($7AA0FF);  | FPC 3.2.0 and newer   
LightSeaGreen  | LightSeaGreen  | TColor($AAB220);  | FPC 3.2.0 and newer   
LightSkyBlue  | LightSkyBlue  | TColor($FACE87);  | FPC 3.2.0 and newer   
LightSlateGray  | LightSlateGray  | TColor($998877);  | FPC 3.2.0 and newer   
LightSlateGrey  | LightSlateGrey  | TColor($998877);  | FPC 3.2.0 and newer   
LightSteelBlue  | LightSteelBlue  | TColor($B0C4DE);  | FPC 3.2.0 and newer   
LightYellow  | LightYellow  | TColor($E0FFFF);  | FPC 3.2.0 and newer   
Lime  | Lime  | TColor($00FF00);  | FPC 3.2.0 and newer   
LimeGreen  | LimeGreen  | TColor($32CD32);  | FPC 3.2.0 and newer   
Linen  | Linen  | TColor($E6F0FA);  | FPC 3.2.0 and newer   
Magenta  | Magenta  | TColor($FF00FF);  | FPC 3.2.0 and newer   
Maroon  | Maroon  | TColor($000080);  | FPC 3.2.0 and newer   
MediumAquaMarine  | MediumAquaMarine  | TColor($AACD66);  | FPC 3.2.0 and newer   
MediumBlue  | MediumBlue  | TColor($CD0000);  | FPC 3.2.0 and newer   
MediumOrchid  | MediumOrchid  | TColor($D355BA);  | FPC 3.2.0 and newer   
MediumPurple  | MediumPurple  | TColor($DB7093);  | FPC 3.2.0 and newer   
MediumSeaGreen  | MediumSeaGreen  | TColor($71B33C);  | FPC 3.2.0 and newer   
MediumSlateBlue  | MediumSlateBlue  | TColor($EE687B);  | FPC 3.2.0 and newer   
MediumSpringGreen  | MediumSpringGreen  | TColor($9AFA00);  | FPC 3.2.0 and newer   
MediumTurquoise  | MediumTurquoise  | TColor($CCD148);  | FPC 3.2.0 and newer   
MediumVioletRed  | MediumVioletRed  | TColor($8515C7);  | FPC 3.2.0 and newer   
MidnightBlue  | MidnightBlue  | TColor($701919);  | FPC 3.2.0 and newer   
MintCream  | MintCream  | TColor($FAFFF5);  | FPC 3.2.0 and newer   
MistyRose  | MistyRose  | TColor($E1E4FF);  | FPC 3.2.0 and newer   
Moccasin  | Moccasin  | TColor($B5E4FF);  | FPC 3.2.0 and newer   
NavajoWhite  | NavajoWhite  | TColor($ADDEFF);  | FPC 3.2.0 and newer   
Navy  | Navy  | TColor($800000);  | FPC 3.2.0 and newer   
OldLace  | OldLace  | TColor($E6F5FD);  | FPC 3.2.0 and newer   
Olive  | Olive  | TColor($008080);  | FPC 3.2.0 and newer   
OliveDrab  | OliveDrab  | TColor($238E6B);  | FPC 3.2.0 and newer   
Orange  | Orange  | TColor($00A5FF);  | FPC 3.2.0 and newer   
OrangeRed  | OrangeRed  | TColor($0045FF);  | FPC 3.2.0 and newer   
Orchid  | Orchid  | TColor($D670DA);  | FPC 3.2.0 and newer   
PaleGoldenRod  | PaleGoldenRod  | TColor($AAE8EE);  | FPC 3.2.0 and newer   
PaleGreen  | PaleGreen  | TColor($98FB98);  | FPC 3.2.0 and newer   
PaleTurquoise  | PaleTurquoise  | TColor($EEEEAF);  | FPC 3.2.0 and newer   
PaleVioletRed  | PaleVioletRed  | TColor($9370DB);  | FPC 3.2.0 and newer   
PapayaWhip  | PapayaWhip  | TColor($D5EFFF);  | FPC 3.2.0 and newer   
PeachPuff  | PeachPuff  | TColor($B9DAFF);  | FPC 3.2.0 and newer   
Peru  | Peru  | TColor($3F85CD);  | FPC 3.2.0 and newer   
Pink  | Pink  | TColor($CBC0FF);  | FPC 3.2.0 and newer   
Plum  | Plum  | TColor($DDA0DD);  | FPC 3.2.0 and newer   
PowderBlue  | PowderBlue  | TColor($E6E0B0);  | FPC 3.2.0 and newer   
Purple  | Purple  | TColor($800080);  | FPC 3.2.0 and newer   
RebeccaPurple  | RebeccaPurple  | TColor($993366);  | FPC 3.2.0 and newer   
Red  | Red  | TColor($0000FF);  | FPC 3.2.0 and newer   
RosyBrown  | RosyBrown  | TColor($8F8FBC);  | FPC 3.2.0 and newer   
RoyalBlue  | RoyalBlue  | TColor($E16941);  | FPC 3.2.0 and newer   
SaddleBrown  | SaddleBrown  | TColor($13458B);  | FPC 3.2.0 and newer   
Salmon  | Salmon  | TColor($7280FA);  | FPC 3.2.0 and newer   
SandyBrown  | SandyBrown  | TColor($60A4F4);  | FPC 3.2.0 and newer   
SeaGreen  | SeaGreen  | TColor($578B2E);  | FPC 3.2.0 and newer   
SeaShell  | SeaShell  | TColor($EEF5FF);  | FPC 3.2.0 and newer   
Sienna  | Sienna  | TColor($2D52A0);  | FPC 3.2.0 and newer   
Silver  | Silver  | TColor($C0C0C0);  | FPC 3.2.0 and newer   
SkyBlue  | SkyBlue  | TColor($EBCE87);  | FPC 3.2.0 and newer   
SlateBlue  | SlateBlue  | TColor($CD5A6A);  | FPC 3.2.0 and newer   
SlateGray  | SlateGray  | TColor($908070);  | FPC 3.2.0 and newer   
SlateGrey  | SlateGrey  | TColor($908070);  | FPC 3.2.0 and newer   
Snow  | Snow  | TColor($FAFAFF);  | FPC 3.2.0 and newer   
SpringGreen  | SpringGreen  | TColor($7FFF00);  | FPC 3.2.0 and newer   
SteelBlue  | SteelBlue  | TColor($B48246);  | FPC 3.2.0 and newer   
Tan  | Tan  | TColor($8CB4D2);  | FPC 3.2.0 and newer   
Teal  | Teal  | TColor($808000);  | FPC 3.2.0 and newer   
Thistle  | Thistle  | TColor($D8BFD8);  | FPC 3.2.0 and newer   
Tomato  | Tomato  | TColor($4763FF);  | FPC 3.2.0 and newer   
Turquoise  | Turquoise  | TColor($D0E040);  | FPC 3.2.0 and newer   
Violet  | Violet  | TColor($EE82EE);  | FPC 3.2.0 and newer   
Wheat  | Wheat  | TColor($B3DEF5);  | FPC 3.2.0 and newer   
White  | White  | TColor($FFFFFF);  | FPC 3.2.0 and newer   
WhiteSmoke  | WhiteSmoke  | TColor($F5F5F5);  | FPC 3.2.0 and newer   
Yellow  | Yellow  | TColor($00FFFF);  | FPC 3.2.0 and newer   
YellowGreen  | YellowGreen  | TColor($32CD9A);  | FPC 3.2.0 and newer   
  
## System colors

### Example: clInfoBk, clInfoText

System colors are color constants with a special meaning. Their real value depends on the context and theme. They are not simple colors. For example **clInfoBk** : 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk;  // use the default background brush of a hint window
    Form1.Canvas.FillRect(10,10,50,50);
    

A hint window on MS Windows might have a white background so the above will draw white. On Linux/gtk2 it might be a metallic texture, so the above will draw the texture. If you want to put some text onto this you need a corresponding color like **clInfoText** , otherwise your text might be unreadable for the user. For example: 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk;  // use the default background brush of a hint window
    Form1.Canvas.FillRect(10,10,50,50);
    Form1.Canvas.Font.Color:=clInfoText;  // use the default text color of a hint window
    Form1.Canvas.TextOut(10,10,'Hint');
    

The system color **clInfoBk** can not be used for Pen.Color and not for Font.Color. If you do so the result is undefined and depends on the widgetset and user theme. The same for **clInfoText** : It can only be used as a Font.Color. Using it as Brush.Color may not work. At the moment all widgetsets allow to use it as Pen.Color too. 

### Theme changes

When the user switches the theme the system colors changes. A **clInfoBk** might change from white to blue or from a color to a texture. This change will happen when you allocate a new Brush handle. Keep in mind that a simple assignment Brush.Color:=clInfoBk does not allocate a Brush Handle. The Brush Handle is allocated on use. For example: 
    
    
    Form1.Canvas.Brush.Color:=clInfoBk; // this will not create a new brush handle
    Form1.Canvas.FillRect(10,10,50,50); // this will create the brush handle with the currently active theme brush for hint windows
    ...
    // if the theme changes in this moment the Brush.Handle is still allocated with the old values
    ...
    Form1.Canvas.FillRect(10,10,50,50); // this will paint with the old theme brush
    Form1.Canvas.Brush.Color:=clInfoBk; // assigning the old value will not create a new brush handle
    Form1.Canvas.FillRect(10,10,50,50); // this will paint with the old theme brush
    Form1.Canvas.Brush.Color:=clRed;    // assigning a new color, old Handle invalid
    Form1.Canvas.Brush.Color:=clInfoBk; // assigning a new color, old Handle invalid
    Form1.Canvas.FillRect(10,10,50,50); // this will create a new handle and paint with the new theme
    

### Table of system colors

The following table lists the system colors and their meaning. Using them outside the scope of the definition is undefined and the result depends on the widgetset and theme. For example **clDefault** is the normal background brush of the used device context. If you want to paint button elements on your own custom controls use the drawing functions of the unit **Themes**. 

Constant | LCL definition | Delphi notes | Supported Widgetsets   
---|---|---|---  
clNone | Indicates "do not paint". Using it as Control's color is undefined. The control will not get transparent. | - | all   
clDefault | Using it for Brush will use the normal background brush of the target DC (device context). 

  * On a Form's canvas a _FillRect_ will paint a rectangular area filled with the normal background of a standard window. This is whatever the widgetset and theme defines. This might be the color gray or a gradient or a picture.
  * Using clDefault on the Canvas of a TListBox will paint with the normal background, which is normally white on Windows. So in a TListBox clDefault is the same as clWindow.
  * Using it as Pen color will use the default line color for the device context.
  * Using it as Font color will use the normal text color of the device context.

| - | all   
clScrollBar | Scrollbar body | - | all   
clBackground | Desktop background color | - | all   
clActiveCaption | Active window titlebar | - | none   
clInactiveCaption | Inactive window titlebar | - | none   
clMenu | Regular menu item background color | - | none   
clWindow | The normal background brush of unselected text. Defined for controls like TEdit, TComboBox, TMemo, TListBox, TTreeView. | - | none   
clWindowFrame | Color of frame around the window | - | none   
clMenuText | The font color to use together with clMenu | - | none   
clWindowText | Font color to use together with clWindow | - | none   
clCaptionText | Active window titlebar text color | - | none   
clActiveBorder | ? | - | none   
clInactiveBorder | ? | - | none   
clAppWorkspace | MDIMain form background | - | none   
clHighlight | The brush color of selected element | - | none   
clHighlightText | Font color of selected text (to use together with clHighligh). | - | none   
clBtnFace | Button background | - | none   
clBtnShadow | Button shadow color (bottom right) used to achieve 3D effect | - | none   
clGrayText | The font color of disabled element | - | none   
clBtnText | Button font color to use together with clBtnFace | - | none   
clInactiveCaptionText | Inactive window titlebar text color | - | none   
clBtnHighlight | Button highlight color (top left) used to achieve 3D effect | - | none   
cl3DDkShadow | ? | - | none   
cl3DLight | ? | - | none   
clInfoText | Font color for hints. Use together with clInfoBk | - | all   
clInfoBk | Brush color for hints. Use together with clInfoText | - | all   
clHotLight | ? | - | none   
clGradientActiveCaption | The second color used to make gradient of active window titlebar | - | none   
clGradientInactiveCaption | The second color used to make gradient for inactive window titlebar | - | none   
clMenuHighlight | The background color of selected menu item | - | none   
clMenuBar | The Backround color of menu bar | - | none   
clForm | ? | - | none   
clColorDesktop | ? | - | none   
cl3DFace | ? | - | none   
cl3DShadow | ? | - | none   
cl3DHiLight | ? | - | none   
clBtnHiLight | Same as clBtnHighlight | - | none   
  
### Finding the rgb values of a system color

Use the function `ColorToRGB` (in unit `Graphics` to determine the rgb components of a system color. This functions detects whether a color is a system color and, if this is true, looks up the system color in the themes color. The returned [Longint](<Longint.md> "Longint") can be understood as a normal color: 
    
    
    // This example has a TColorBox on a form and uses this OnChange handler for the TColorbox:
    procedure TForm1.ColorBox1Change(Sender: TObject);
    var
      c: TColor;
    begin
      c := ColorToRGB(ColorBox1.Selected);
      Caption := Format('R%d G%d B%d', [Red(c), Green(c), Blue(c)]);
    end;
    

## Drawing theme elements on your custom controls

The unit **Themes** provides functions to draw single elements of standard controls. For example to draw an expand sign like a [TTreeView](<TTreeView.md> "TTreeView") use the following code: 
    
    
    uses Themes;
    
    ...
    
    procedure TYourCustomControl.Paint;
    const
      PlusMinusDetail: array[Boolean {Hot}, Boolean {Expanded}] of TThemedTreeview =
      (
        (ttGlyphClosed, ttGlyphOpened),
        (ttHotGlyphClosed, ttHotGlyphOpened)
      );
    var
      Details: TThemedElementDetails;
      R: TRect;
      Collapse: boolean;
    begin
      ...
      //draw a themed expand sign.
      Details := ThemeServices.GetElementDetails(PlusMinusDetail[False, Collapse]);
      R := Rect(ALeft, ATop, ARight + 1, ABottom + 1);
      ThemeServices.DrawElement(Canvas.Handle, Details, R, nil);
      ...
    end;

---

_Source: [https://wiki.freepascal.org/Colors](https://web.archive.org/web/20250418082148/https://wiki.freepascal.org/Colors)_
