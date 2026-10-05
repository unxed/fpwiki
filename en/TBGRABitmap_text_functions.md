# TBGRABitmap text functions

Here are the text functions the [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class") class. 

## Contents

  * 1 Font name and style
  * 2 Font quality
  * 3 Drawing
  * 4 Measuring



### Font name and style
    
    
      property FontName: string; read write;

Specifies the font to use. Unless the font renderer accept otherwise, the name is in human readable form, like 'Arial', 'Times New Roman', ... 
    
    
      property FontStyle: TFontStyles; read write;

Specifies the set of styles to be applied to the font. These can be _fsBold_ , _fsItalic_ , _fsStrikeOut_ , _fsUnderline_. So the value _[fsBold,fsItalic]_ means that the font must be bold and italic. 
    
    
      property FontOrientation: integer; read write;

Specifies the rotation of the text, for functions that support text rotation. It is expressed in tenth of degrees, positive values going counter-clockwise. 
    
    
      property FontHeight: integer; read write;

Specifies the height of the font without taking into account additional line spacing. A negative value means that it is the full height instead (see below). 
    
    
      property FontFullHeight: integer; read write;

Specifies the height of the font, taking into account the additional line spacing defined for the font. 

Note: additional styles may be specified with the font renderer (see below). 

### Font quality
    
    
      property FontQuality : TBGRAFontQuality; read write;

Specifies the quality of rendering. The following values are possible : 

\- _fqSystem_ : use system rendering. It is fast however it may be not be smoothed. 

\- _fqSystemClearType_ : use system rendering with ClearType. This quality is of course better than _fqSystem_ however it may not be much smoother. 

\- _fqFineAntialiasing_ : garanties a high quality antialiasing. This is slower. 

\- _fqFineClearTypeRGB_ : garanties a high quality antialiasing with ClearType. The order of the color in the LCD screen is supposed to be un red/green/blue order. 

\- _fqFineClearTypeBGR_ : same as above, except the color of the LCD screen is supposed to be in blue/green/red order. 
    
    
      property FontAntialias: Boolean; read write;

Simplified version to specify the quality. 
    
    
      property FontRenderer: TBGRACustomFontRenderer; read write;

Specifies the font renderer. By default it is an instance of _TLCLFontRenderer_ of unit _BGRAText_. Other renderers are provided in _BGRATextFX_ unit and _BGRAVectorize_ unit. Once you assign a renderer, it will automatically be freed. The renderers may provide additional styling for the font. See **[font rendering](<BGRABitmap_tutorial_Font_rendering.md> "BGRABitmap tutorial Font rendering")**. 

### Drawing
    
    
      procedure TextOut(x, y: single; sUTF8: string; c: TBGRAPixel; align: TAlignment);

Draws the UTF8 encoded string, with color _c_. If _align_ is taLeftJustify, (_x_ ,_y_) is the top-left corner. If _align_ is taCenter, (_x_ ,_y_) is at the top and middle of the text. If _align_ is taRightJustify, (_x_ ,_y_) is the top-right corner. The value of _FontOrientation_ is taken into account, so that the text may be rotated. 
    
    
      procedure TextOut(x, y: single; sUTF8: string; texture: IBGRAScanner; align: TAlignment);

Same as above functions, except that the text is filled using _texture_. The value of _FontOrientation_ is taken into account, so that the text may be rotated. 
    
    
      procedure TextOutAngle(x, y: single; orientationTenthDegCCW: integer; sUTF8: string; c: TBGRAPixel; align: TAlignment);
      procedure TextOutAngle(x, y: single; orientationTenthDegCCW: integer; sUTF8: string; texture: IBGRAScanner; align: TAlignment);

Same as above, except that the orientation is specified, overriding the value of the property _FontOrientation_. 
    
    
      procedure TextOut(x, y: single; sUTF8: string; c: TBGRAPixel);
      procedure TextOut(x, y: single; sUTF8: string; c: TColor);
      procedure TextOut(x, y: single; sUTF8: string; texture: IBGRAScanner);

Draw the UTF8 encoded string, (_x_ ,_y_) being the top-left corner. The color _c_ or _texture_ is used to fill the text. The value of _FontOrientation_ is taken into account, so that the text may be rotated. 
    
    
      procedure TextRect(ARect: TRect; x, y: integer; sUTF8: string; style: TTextStyle; c: TBGRAPixel);
      procedure TextRect(ARect: TRect; x, y: integer; sUTF8: string; style: TTextStyle; texture: IBGRAScanner);

Draw the UTF8 encoded string at the coordinate (_x_ ,_y_), clipped inside the rectangle _ARect_. Additional style information is provided by the _style_ parameter. The color _c_ or _texture_ is used to fill the text. No rotation is applied. 
    
    
        procedure TextRect(ARect: TRect; sUTF8: string; halign: TAlignment; valign: TTextLayout; c: TBGRAPixel);
      procedure TextRect(ARect: TRect; sUTF8: string; halign: TAlignment; valign: TTextLayout; texture: IBGRAScanner);

Draw the UTF8 encoded string in the rectangle _ARect_. Text is wrapped if necessary. The position depends on the specified horizontal alignment _halign_ and vertical alignement _valign_. The color _c_ or _texture_ is used to fill the text. No rotation is applied. 

See **[text functions tutorial](<BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12")**. 

### Measuring
    
    
      function TextSize(sUTF8: string): TSize;

Returns the total size of the string provided using the current font. Orientation is not taken into account, so that the width is along the text. 
    
    
      property FontPixelMetric: TFontPixelMetric;

Returns measurement for the current font in pixels.

---

_Source: [https://wiki.freepascal.org/TBGRABitmap_text_functions](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/TBGRABitmap_text_functions)_
