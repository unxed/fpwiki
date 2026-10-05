# BGRABitmap Color definitions

List of color definitions and string conversion functions in [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. They are provided by [BGRABitmapTypes unit](<https://bgrabitmap.github.io/doc/BGRABitmapTypes.html>). 

### Color definitions

_BGRAPixelTransparent_ : TBGRAPixel;  
---  
| This is the value used for transparent pixels. In theory, any color with alpha = 0 is transparent, however it is recommended to use all other channels to zero as well.  
_BGRAWhite_ : TBGRAPixel;  
|  White opaque  
_BGRABlack_ : TBGRAPixel;  
|  Black opaque  
_clBlackOpaque_ = TColor($010000);  
| This color  looks just like black. It is needed for drawing black shapes using the _Canvas_ property of TBGRABitmap. This is a standard TCanvas and when drawing with pure black (_clBlack_), there is no way to know if something has been drawn or if it is transparent  
_TBGRAColorList_ = **class**  
| Contains a fixed list of colors  
| **constructor** Create;  
| | Creates an empty color list  
| **procedure** Add(Name: **string** ; **const** Color: TBGRAPixel); **overload** ;  
| | Add a color to the list  
| **procedure** Finished;  
| | Ends the color list and prevents further modifications  
| **function** IndexOf(Name: **string**): integer;  
| | Returns the index of a color with a given name  
| **function** IndexOfColor(**const** AColor: TBGRAPixel; AMaxDiff: Word = 0): integer;  
| | Returns the index of a color. Colors are considered to match if the difference is less than or equal to _AMaxDiff_  
| **property** ByName[Name: **string**]: TBGRAPixel **read** GetByName;  
| | Gets the color associated with a color name  
| **property** ByIndex[Index: integer]: TBGRAPixel **read** GetByIndex; **default** ;  
| | Gets the color at the specified index  
| **property** Name[Index: integer]: **string** **read** GetName;  
| | Gets the name of the color at the specified index  
| **property** Count: integer **read** FNbColors;  
| | Gets the number of colors  
_VGAColors_ : TBGRAColorList;  
| List of VGA colors: Black,  Gray,  Silver,  White,  Maroon,  Red,  Purple,  Fuchsia,  Green,  Lime,  Olive,  Yellow,  Navy,  Blue,  Teal,  Aqua. Shortcut constants are provided:  _VGABlack_ ,  _VGAGray_...  
_CSSColors_ : TBGRAColorList;  
| List of [web colors](<https://www.w3schools.com/cssref/css_colors.asp>). Shortcut constants are provided:  _CSSBlack_ ,  _CSSRed_...  
**function** BGRAToStr(c: TBGRAPixel; AColorList: TBGRAColorList = **nil** ;  
| Converts a TBGRAPixel value into a string, using color names provided in _AColorList_ , and considering that a color matches in the color list if its difference is within _AMaxDiff_  
**function** StrToBGRA(str: **string**): TBGRAPixel; **overload** ;  
| Converts a fully defined string into a TBGRAPixel value. Color names from _VGAColors_ and _CSSColors_ are used if there is an exact match  
**function** StrToBGRA(str: **string** ; **const** DefaultColor: TBGRAPixel): TBGRAPixel; **overload** ;  
| Converts a string into a TBGRAPixel value. If the value is not fully defined or that there is an error, _DefaultColor_ is returned. Color names from _VGAColors_ and _CSSColors_ are used if there is an exact match.  
**function** PartialStrToBGRA(str: **string** ; **const** fallbackValues: TBGRAPixel; **out** error: boolean): TBGRAPixel;  
| Converts a string into a TBGRAPixel value. If the value is not fully defined, missing channels (expressed with '?') are filled with fallbackValues. You can check if there was an error with the provided boolean. Color names from _VGAColors_ and _CSSColors_ are used if there is an exact match.  
**procedure** TryStrToBGRA(str: **string** ; **var** parsedValue: TBGRAPixel; **out** missingValues: boolean; **out** error: boolean);  
| Converts a string into a TBGRAPixel value into _parsedValue_. _parsedValue_ is not changed if some channels are missing (expressed with '?'). You can check if there was an error with the provided boolean. Color names from _VGAColors_ and _CSSColors_ are used if there is an exact match.  
  
## See also

  * [Using XYZ colorspace in BGRABitmap](<BGRABitmap_XYZ.md> "BGRABitmap XYZ")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Color_definitions](https://web.archive.org/web/20250515121935/https://wiki.freepascal.org/BGRABitmap_Color_definitions)_
