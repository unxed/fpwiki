# Font

TFont class describes a typographic font information. The [LCL](<LCL.md> "LCL") defines the following [properties](</Property> "Property"): 

## Contents

  * 1 Color
  * 2 Height
  * 3 Name
  * 4 Pitch
  * 5 Size
    * 5.1 Size to Height
  * 6 Style



### Color

Sets the [colour](<Colors.md> "Colors") of the text to be displayed. 

### Height

The height property specifies the font height in Pixels. 

However, due to Windows and Delphi compatibility, height might be described in 2 values, negative and positive. 

[![font height.png](https://wiki.freepascal.org/images/9/99/font_height.png)](</File:font_height.png>)

Negative value takes into account only the height of font characters. 

Positive value takes into account height of font characters plus additional space on top of the font. (So, it would be possible to write a line of the text right after another, without adding any extra space, and the font would not look too densed). 

Normally, you want to use Negative font height. 

### Name

The name property defines the name of the font to be used. 

### Pitch

This property determines the spacing between the letters. It may have one of the following values: 

  * _fpDefault_
  * _fpFixed_
  * _fxVariable_



### Size

Is the size of the font in Typographic points. 

In order to convert from Typographic points to Pixels one should use a device (Screen or Printer) dots per point value (DPI) 

The size is also using Positive and Negative values, but in reverse order. 

Positive value takes into account only the height of font characters. (it's a typographic standard) 

Negative value takes into account height of font characters plus additional space on top of the font. (So, it would be possible to write a line of the text right after another, without adding any extra space, and the font would not look too dense). 

Normally, you want to use Positive font size values. 

  
It might be useful to note, that User Interface is typically presenting the size in typographs points. 

[![toto](https://wiki.freepascal.org/images/a/af/fontdialog.png)](</File:fontdialog.png> "toto")

In this example, the selected font would have Font.Size=16 

#### Size to Height

Once should use the following formal to convert from Size to height 
    
    
    Font.Height := -Font.Size * DPI / 72
    

and from Height to Size 
    
    
    Font.Size := -Font.Height / DPI * 72
    

Note, the use of negative , due to reverse logic. 

### Style

Defines the style of the text to be displayed or printed.

---

_Source: [https://wiki.freepascal.org/Font](https://web.archive.org/web/20230602232407/https://wiki.freepascal.org/Font)_
