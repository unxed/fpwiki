# BGRABitmap Types imported from Graphics

Back to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). 

Here is the list of types in _BGRAGraphics_ unit. They are imported from the LCL unit called [Graphics](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/index.html>). They are imported by _BGRABitmapTypes_ unit so that you don't need to add explicitely this unit to the uses clause. 

If the LCL is not present, those types are defined to provide their basic features. 

### Types imported from Graphics

_PColor_ = ^TColor;  
---  
| Pointer to a TColor value. TColor contains a color stored as RGB. The red/green/blue values range from 0 to 255. The formula to get the color value is: _color_ = _red_ \+ (_green_ **shl** 8) + (_blue_ **shl** 16) except with fpGUI where it is: _color_ = (_red_ **shl** 16) + (_green_ **shl** 8) + _blue_  
| **function** FPColorToTColor(**const** FPColor: TFPColor): TColor;  
| | Converts a TFPColor into a TColor value  
| **function** TColorToFPColor(**const** c: TColor): TFPColor;  
| | Converts a TColor into a TFPColor value  
_TGradientDirection_ = (  
| Direction of change in a gradient  
| _gdVertical_ ,  
| | Color changes vertically  
| _gdHorizontal_);  
| | Color changes horizontally  
_TAntialiasingMode_ = (  
| Antialiasing mode for a Canvas  
| _amDontCare_ ,  
| | It does not matter if there is antialiasing or not  
| _amOn_ ,  
| | Antialiasing is required (BGRACanvas provide it)  
| _amOff_);  
| | Antialiasing is disabled  
_TTextLayout_ = (tlTop, tlCenter, tlBottom);  
| Vertical position of a text  
_TTextStyle_ = **packed** **record**  
| Styles to describe how a text is drawn in a rectangle  
| _Alignment_ : TAlignment;  
| | Horizontal alignment  
| _Layout_ : TTextLayout;  
| | Vertical alignment  
| _SingleLine_ : boolean;  
| | If WordBreak is false then process #13, #10 as standard chars and perform no Line breaking  
| _Clipping_ : boolean;  
| | Clip Text to passed Rectangle  
| _ExpandTabs_ : boolean;  
| | Replace #9 by apropriate amount of spaces (default is usually 8)  
| _ShowPrefix_ : boolean;  
| | Process first single '&' per line as an underscore and draw '&&' as '&'  
| _Wordbreak_ : boolean;  
| | If line of text is too long too fit between left and right boundaries try to break into multiple lines between words. See also _EndEllipsis_  
| _Opaque_ : boolean;  
| | Fills background with current brush  
| _SystemFont_ : Boolean;  
| | Use the system font instead of canvas font  
| _RightToLeft_ : Boolean;  
| | For RightToLeft text reading (Text Direction)  
| _EndEllipsis_ : Boolean;  
| | If line of text is too long to fit between left and right boundaries truncates the text and adds "...". If Wordbreak is set as well, Workbreak will dominate  
_TFillStyle_ =  
| Option for floodfill (used in BGRACanvas)  
| _fsSurface_ ,  
| | Fill up to the color (it fills all except the specified color)  
| _fsBorder_  
| | Fill the specified color (it fills only connected pixels of this color)  
_TFillMode_ = (  
| How to handle polygons that intersect with themselves and overlapping polygons  
| _fmAlternate_ ,  
| | Each time a boundary is found, it enters or exit the filling zone  
| _fmWinding_);  
| | Adds or subtract 1 depending on the order of the points of the polygons (clockwise or counter clockwise) and fill when the result is non-zero. So, to draw a hole, you must specify the points of the hole in the opposite order  
_TFontStyle_ = (  
| Available font styles  
| _fsBold_ ,  
| | Font is bold  
| _fsItalic_ ,  
| | Font is italic  
| _fsStrikeOut_ ,  
| | An horizontal line is drawn in the middle of the text  
| _fsUnderline_);  
| | Text is underlined  
| _TFontStyles_ = **set** **of** TFontStyle;  
| | A combination of font styles  
_TFontQuality_ = (fqDefault, fqDraft, fqProof, fqNonAntialiased, fqAntialiased, fqCleartype, fqCleartypeNatural);  
| Quality to use when font is rendered by the system  
_TCanvas_ = **class**  
| A surface on which to draw  
| **procedure** Draw(x,y: integer; AImage: TGraphic);  
| | Draw an image with top-left corner at (_x_ , _y_)  
| **procedure** StretchDraw(ARect: TRect; AImage: TGraphic);  
| | Draw and stretch an image within the rectangle _ARect_  
_TGraphic_ = **class**(TPersistent)  
| A class containing any element that can be drawn within rectangular bounds  
| **procedure** LoadFromFile(**const** Filename: **string**); **virtual** ;  
| | Load the content from a given file  
| **procedure** LoadFromStream(Stream: TStream); **virtual** ; **abstract** ;  
| | Load the content from a given stream  
| **procedure** SaveToFile(**const** Filename: **string**); **virtual** ;  
| | Saves the content to a file  
| **procedure** SaveToStream(Stream: TStream); **virtual** ; **abstract** ;  
| | Saves the content into a given stream  
| **class** **function** GetFileExtensions: **string** ; **virtual** ;  
| | Returns the list of possible file extensions  
| **procedure** Clear; **virtual** ;  
| | Clears the content  
| **property** Empty: Boolean **read** GetEmpty;  
| | Returns if the content is completely empty  
| **property** Height: Integer **read** GetHeight **write** SetHeight;  
| | Returns the height of the bounding rectangle  
| **property** Width: Integer **read** GetWidth **write** SetWidth;  
| | Returns the width of the bounding rectangle  
| **property** Transparent: Boolean **read** GetTransparent **write** SetTransparent;  
| | Gets or sets if it is drawn with transparency  
_TBitmap_ = **class**(TGraphic)  
| Contains a bitmap  
| **property** Width: integer **read** GetWidth **write** SetWidth;  
| | Width of the bitmap in pixels  
| **property** Height: integer **read** GetHeight **write** SetHeight;  
| | Height of the bitmap in pixels  
**function** MulDiv(nNumber, nNumerator, nDenominator: Integer): Integer;  
| Multiply and divide the number allowing big intermediate number and rounding the result  
**function** MathRound(AValue: ValReal): Int64; **inline** ;  
| Round the number using math convention

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Types_imported_from_Graphics](https://web.archive.org/web/20250515131734/https://wiki.freepascal.org/BGRABitmap_Types_imported_from_Graphics)_
