# TBGRABitmap class

│ **English (en)** │

  
Back to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). 

  
_TBGRABitmap_ class contains an image stored in BGRA 32-bit format. It derives from _TBGRADefaultBitmap_ which derives from [TBGRACustomBitmap](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner") from which it inherits its properties. It is defined in [BGRABitmap unit](<https://bgrabitmap.github.io/doc/BGRABitmap.html>). It implements [IBGRAScanner](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner") interface which allows it to be used as a texture (like a brush). 

## Contents

  * 1 Implementations
  * 2 Direct pixel access
  * 3 TBGRABitmap reference
    * 3.1 Reference counting
    * 3.2 Constructors
    * 3.3 Pixel functions
    * 3.4 Drawing lines and polylines (integer coordinates)
    * 3.5 Drawing lines and polylines (floating point coordinates)
    * 3.6 Rectangles (integer coordinates)
    * 3.7 Rectangles and ellipses (floating point coordinates)
  * 4 Text functions



### Implementations

_TBGRABitmap_ is a shortcut provided by [BGRABitmap unit](<https://bgrabitmap.github.io/doc/BGRABitmap.html>) for the active implementation, depending on the widgetset. It can be one of the following: 

  * _TBGRAWinBitmap_ or _TBGRAMSEguiBitmap_ on Windows
  * _TBGRAGtkBitmap_ on Gtk
  * _TBGRAQtBitmap_ when Qt widget is active
  * _TBGRAfpGUIBitmap_ when using the package for fpGUI toolkit (_bgrabitmappack4fpgui.lpk_)
  * _TBGRANoGUIBitmap_ when using the package independent of the graphic user interface (_bgrabitmappack4nogui.lpk_)
  * _TBGRADefaultBitmap_ otherwise



### Direct pixel access

The image is stored as BGRA 32-bit: 

| _PBGRAPixel_ = ^TBGRAPixel;  
---|---  
| | Pointer for direct pixel access. Data is stored as a sequence of _TBGRAPixel_. See [BGRABitmap tutorial 4](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")  
| _TBGRAPixel_ = **packed** **record** blue, green, red, alpha: byte; **end** ;  
| | Each pixel is a sequence of 4 bytes containing blue, green, red and alpha channel. Values range from 0 to 255, color is in sRGB colorspace. The alpha value of 0 is transparent and 255 is opaque  
  
You can either use _Data_ property if you want to go through all pixels, or _ScanLine_ property if you want to do something that depends on the line number. _Data_ returns the first pixel in memory, so that you can read all the pixels from there by increasing the pointer (add 1 to _PBGRAPixel_). Otherwise, you can do a loop where _y_ goes from 0 to _Height_ -1, and for each line, retrieve with _ScanLine_ the pointer of the beginning of the line. Then you can read all the pixels of the line from there by increasing the pointer. 

The following properties of _TBGRABitmap_ allow to access the data directly: 

_TBGRABitmap_ = **class**(TBGRACustomBitmap)  
---  
| **property** Width: integer **read** ;  
| | Width of the image in pixels  
| **property** Height: integer **read** ;  
| | Height of the image in pixels  
| **property** NbPixels: integer **read** ;  
| | Total number of pixels. It is always true that _NbPixels_ = _Width_ * _Height_  
| **property** ScanLine[y: integer]: PBGRAPixel **read** ;  
| | Returns the address of the left-most pixel of any line. The parameter y ranges from 0 to Height-1  
| **property** LineOrder: TRawImageLineOrder **read** ;  
| | Indicates the order in which lines are stored in memory. If it is equal to _riloTopToBottom_ , the first line is the top line. If it is equal to _riloBottomToTop_ , the first line is the bottom line. See [miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| **property** Data: PBGRAPixel **read** ;  
| | Provides a pointer to the first pixel in memory. Depending on the _LineOrder_ property, this can be the top-left pixel or the bottom-left pixel. There is no padding between scanlines, so the start of the next line is at the address _Data_ \+ _Width_. See [BGRABitmap tutorial 4](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")  
| **property** Caption: **string** **read** **write** ;  
| | Can contain a text to describe the image. It is not displayed. It is the name of the layer if this image is in a layered bitmap.  
  
### TBGRABitmap reference

_TBGRABitmap_ = **class**(TBGRACustomBitmap)  
---  
| This class is the base for all _TBGRABitmap_ classes. It implements most function to the exception from implementations specific to the widgetset  
| **property** CanvasBGRA: TBGRACanvas **read** ;  
| | Provides a canvas with opacity and antialiasing  
| **property** Canvas2D: TBGRACanvas2D **read** ;  
| | Provides a canvas with 2d transformation and similar to HTML5.  
  
For more properties, see parent class [TBGRACustomBitmap](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner")

#### Reference counting

| **function** NewReference: TBGRACustomBitmap;  
---|---  
| | Adds a reference (this reference count is not the same as the reference count of an interface, it changes only by explicit calls  
| **procedure** FreeReference;  
| | Free a reference. When the resulting reference count gets to zero, the image is freed. The initial reference count is equal to 1  
| **function** GetUnique: TBGRACustomBitmap;  
| | Returns an object with a reference count equal to 1. Duplicate this bitmap if necessary  
  
#### Constructors

| **constructor** Create(AWidth, AHeight: integer); **override** ;  
---|---  
| | Creates a new bitmap, initialize properties and bitmap data  
| **procedure** SetSize(AWidth, AHeight: integer); **override** ;  
| | Can only be called with an existing instance of _TBGRABitmap_. Sets the dimensions of an existing _TBGRABitmap_ instance.  
| **constructor** Create; **override** ;  
| | Creates an image of width and height equal to zero. In this case, _Data_ = **nil**  
| **constructor** Create(AFPImage: TFPCustomImage); **override** ;  
| | Creates an image by copying the content of a _TFPCustomImage_  
| **constructor** Create(ABitmap: TBitmap); **override** ;  
| | Creates an image by copying the content of a _TBitmap_  
| **constructor** Create(AWidth, AHeight: integer; Color: TColor); **override** ;  
| | Creates an image of dimensions _AWidth_ and _AHeight_ and fills it with the opaque color _Color_  
| **constructor** Create(AWidth, AHeight: integer; Color: TBGRAPixel); **override** ;  
| | Creates an image of dimensions _AWidth_ and _AHeight_ and fills it with _Color_  
| **constructor** Create(AFilename: **string**); **override** ;  
| | Creates an image by loading its content from the file _AFilename_. The encoding of the string is the default one for the operating system. It is recommended to use the next constructor and UTF8 encoding  
| **constructor** Create(AFilename: **string** ; AIsUtf8: boolean); **override** ;  
| | Creates an image by loading its content from the file _AFilename_. The boolean _AIsUtf8Filename_ specifies if UTF8 encoding is assumed for the filename  
| **constructor** Create(AStream: TStream); **override** ;  
| | Creates an image by loading its content from the stream _AStream_  
| **destructor** Destroy; **override** ;  
| | Free the object and all its resources  
| **function** NewBitmap(AWidth, AHeight: integer): TBGRACustomBitmap; **override** ;  
| | Can only be called from an existing instance of _TBGRABitmap_. Creates a new instance with dimensions _AWidth_ and _AHeight_ , containing transparent pixels.  
| **function** NewBitmap(AWidth, AHeight: integer; Color: TBGRAPixel): TBGRACustomBitmap; **override** ;  
| | Can only be called from an existing instance of _TBGRABitmap_. Creates a new instance with dimensions _AWidth_ and _AHeight_ , and fills it with Color  
| **function** NewBitmap(Filename: **string**): TBGRACustomBitmap; **override** ;  
| | Can only be called from an existing instance of _TBGRABitmap_. Creates a new instance with by loading its content from the file _Filename_. The encoding of the string is the default one for the operating system  
| **function** NewBitmap(Filename: **string** ; AIsUtf8: boolean): TBGRACustomBitmap; **override** ;  
| | Can only be called from an existing instance of _TBGRABitmap_. Creates a new instance with by loading its content from the file _Filename_  
| **procedure** Assign(Source: TPersistent); **override** ;  
| | Assign the content of the specified _Source_. It can be a _TBGRACustomBitmap_ or a _TFPCustomImage_  
| **procedure** Serialize(AStream: TStream); **override** ;  
| | Stores the image in the stream without compression nor header  
| **procedure** Deserialize(AStream: TStream); **override** ;  
| | Reads the image in a stream that was previously serialized  
| **class** **procedure** SerializeEmpty(AStream: TStream);  
| | Stores an empty image (of size zero)  
  
Example: 
    
    
    var bmp1, bmp2: TBGRABitmap;
    begin
      bmp1 := TBGRABitmap.Create(100,100);
      bmp2 := bmp1.NewBitmap(100,100) as TBGRABitmap;
      ...
    end;
    

See tutorial 2 on [how to load and display an image](<BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2"). 

See reference on [loading and saving files](<TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner")

#### Pixel functions

| **function** PtInClipRect(x, y: int32or64): boolean; **inline** ;  
---|---  
| | Checks if the specified point is in the clipping rectangle _ClipRect_  
| **procedure** SetPixel(x, y: int32or64; c: TColor); **override** ;  
| | Sets the pixel by replacing the content at (_x_ ,_y_) with the specified color. Alpha value is set to 255 (opaque)  
| **procedure** SetPixel(x, y: int32or64; c: TBGRAPixel); **override** ;  
| | Sets the pixel at (_x_ ,_y_) with the specified content  
| **procedure** XorPixel(x, y: int32or64; c: TBGRAPixel); **override** ;  
| | Applies a logical **xor** to the content of the pixel with the specified value. This includes the alpha channel, so if you want to preserve the opacity, provide a color _c_ with alpha channel equal to zero  
| **procedure** DrawPixel(x, y: int32or64; c: TBGRAPixel); **override** ;  
| | Draws a pixel with gamma correction at (_x_ ,_y_). Pixel is supplied in sRGB colorspace  
| **procedure** DrawPixel(x, y: int32or64; c: TBGRAPixel; ADrawMode: TDrawMode); **overload** ;  
| | Draws a pixel with the specified _ADrawMode_ at (_x_ ,_y_). Pixel is supplied in sRGB colorspace. Gamma correction may be applied depending on the draw mode  
| **procedure** DrawPixel(x, y: int32or64; ec: TExpandedPixel); **override** ;  
| | Draws a pixel with gamma correction at (_x_ ,_y_). Pixel is supplied in gamma expanded colorspace  
| **procedure** FastBlendPixel(x, y: int32or64; c: TBGRAPixel); **override** ;  
| | Draws a pixel without gamma correction at (_x_ ,_y_). Pixel is supplied in sRGB colorspace  
| **procedure** ErasePixel(x, y: int32or64; alpha: byte); **override** ;  
| | Erase the content of the pixel by reducing the value of the alpha channel. _alpha_ specifies how much to decrease. If the resulting alpha reaches zero, the content is replaced by _BGRAPixelTransparent_  
| **procedure** AlphaPixel(x, y: int32or64; alpha: byte); **override** ;  
| | Sets the alpha value at (_x_ ,_y_). If _alpha_ = 0, the pixel is replaced by _BGRAPixelTransparent_  
| **function** GetPixel(x, y: int32or64): TBGRAPixel; **override** ;  
| | Returns the content of the specified pixel. If it is out of the bounds of the picture, the result is _BGRAPixelTransparent_  
| **function** GetPixel(x, y: single; AResampleFilter: TResampleFilter = rfLinear; smoothBorder: boolean = true): TBGRAPixel; **override** ;  
| | Computes the value of the pixel at a floating point coordiante by interpolating the values of the pixels around it.   
There is a one pixel wide margin around the pixel where the pixels are still considered inside. If _smoothBorder_ is set to true, pixel fade to transparent.   
If it is more out of the bounds, the result is _BGRAPixelTransparent_.   
_AResampleFilter_ specifies how pixels must be interpolated. Accepted values are _rfBox_ , _rfLinear_ , _rfHalfCosine_ and _rfCosine_  
| **function** GetPixel256(x, y, fracX256,fracY256: int32or64; AResampleFilter: TResampleFilter = rfLinear; smoothBorder: boolean = true): TBGRAPixel; **override** ;  
| | Similar to previous _GetPixel_ function, but the fractional part of the coordinate is supplied with a number from 0 to 255. The actual coordinate is (_x_ \+ _fracX256_ /256, _y_ \+ _fracY256_ /256)  
| **function** GetPixelCycle(x, y: single; AResampleFilter: TResampleFilter = rfLinear): TBGRAPixel; **override** ;  
| | Computes the value of the pixel at a floating point coordiante by interpolating the values of the pixels around it. If the pixel is out of bounds, the image is repeated.   
_AResampleFilter_ specifies how pixels must be interpolated. Accepted values are _rfBox_ , _rfLinear_ , _rfHalfCosine_ and _rfCosine_  
| **function** GetPixelCycle256(x, y, fracX256,fracY256: int32or64; AResampleFilter: TResampleFilter = rfLinear): TBGRAPixel; **override** ;  
| | Similar to previous _GetPixel_ function, but the fractional part of the coordinate is supplied with a number from 0 to 255. The actual coordinate is (_x_ \+ _fracX256_ /256, _y_ \+ _fracY256_ /256)  
| **function** GetPixelCycle(x, y: single; AResampleFilter: TResampleFilter; repeatX: boolean; repeatY: boolean): TBGRAPixel; **override** ;  
| | Computes the value of the pixel at a floating point coordiante by interpolating the values of the pixels around it. _repeatX_ and _repeatY_ specifies if the image is to be repeated or not.   
_AResampleFilter_ specifies how pixels must be interpolated. Accepted values are _rfBox_ , _rfLinear_ , _rfHalfCosine_ and _rfCosine_  
| **function** GetPixelCycle256(x, y, fracX256,fracY256: int32or64; AResampleFilter: TResampleFilter; repeatX: boolean; repeatY: boolean): TBGRAPixel; **override** ;  
| | Similar to previous _GetPixel_ function, but the fractional part of the coordinate is supplied with a number from 0 to 255. The actual coordinate is (_x_ \+ _fracX256_ /256, _y_ \+ _fracY256_ /256)  
  
#### Drawing lines and polylines (integer coordinates)

These functions do not take into account current pen style/cap/join. See [coordinate system](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13"). 

| **procedure** SetHorizLine(x, y, x2: int32or64; c: TBGRAPixel); **override** ;  
---|---  
| | Replaces the content of the pixels at line _y_ and at columns _x_ to _x2_ included, using specified color  
| **procedure** XorHorizLine(x, y, x2: int32or64; c: TBGRAPixel); **override** ;  
| | Applies xor to the pixels at line _y_ and at columns _x_ to _x2_ included, using specified color. This includes the alpha channel, so if you want to preserve the opacity, provide a color _c_ with alpha channel equal to zero  
| **procedure** DrawHorizLine(x, y, x2: int32or64; c: TBGRAPixel); **override** ;  
| | Draws an horizontal line with gamma correction at line _y_ and at columns _x_ to _x2_ included, using specified color  
| **procedure** DrawHorizLine(x, y, x2: int32or64; ec: TExpandedPixel); **override** ;  
| | Draws an horizontal line with gamma correction at line _y_ and at columns _x_ to _x2_ included, using specified color  
| **procedure** DrawHorizLine(x, y, x2: int32or64; texture: IBGRAScanner); **overload** ;  
| | Draws an horizontal line with gamma correction at line _y_ and at columns _x_ to _x2_ included, using specified scanner to get the source colors  
| **procedure** FastBlendHorizLine(x, y, x2: int32or64; c: TBGRAPixel); **override** ;  
| | Draws an horizontal line without gamma correction at line _y_ and at columns _x_ to _x2_ included, using specified color  
| **procedure** HorizLine(x, y, x2: int32or64; texture: IBGRAScanner; ADrawMode : TDrawMode); **override** ;  
| | Draws an horizontal line at line _y_ and at columns _x_ to _x2_ included, using specified scanner and the specified _ADrawMode_  
| **procedure** HorizLine(x,y,x2: Int32or64; c: TBGRAPixel; ADrawMode: TDrawMode); **overload** ;  
| | Draws an horizontal line at line _y_ and at columns _x_ to _x2_ included, using specified color and the specified _ADrawMode_  
| **procedure** AlphaHorizLine(x, y, x2: int32or64; alpha: byte); **override** ;  
| | Replaces the alpha value of the pixels at line _y_ and at columns _x_ to _x2_ included  
| **procedure** DrawHorizLineDiff(x, y, x2: int32or64; c, compare: TBGRAPixel;  
| | Draws an horizontal line with gamma correction at line _y_ and at columns _x_ to _x2_ included, using specified color, and with a transparency that increases with the color difference with _compare_. If the difference is greater than _maxDiff_ , pixels are not changed  
| **procedure** SetVertLine(x, y, y2: int32or64; c: TBGRAPixel); **override** ;  
| | Replaces a vertical line at column _x_ and at row _y_ to _y2_  
| **procedure** XorVertLine(x, y, y2: int32or64; c: TBGRAPixel); **override** ;  
| | Xors a vertical line at column _x_ and at row _y_ to _y2_  
| **procedure** DrawVertLine(x, y, y2: int32or64; c: TBGRAPixel); **override** ;  
| | Draws a vertical line with gamma correction at column _x_ and at row _y_ to _y2_  
| **procedure** FastBlendVertLine(x, y, y2: int32or64; c: TBGRAPixel); **override** ;  
| | Draws a vertical line without gamma correction at column _x_ and at row _y_ to _y2_  
| **procedure** AlphaVertLine(x, y, y2: int32or64; alpha: byte); **override** ;  
| | Replace alpha values in a vertical line at column _x_ and at row _y_ to _y2_  
| **procedure** VertLine(x,y,y2: Int32or64; c: TBGRAPixel; ADrawMode: TDrawMode);  
| | Draws a vertical line with the specified draw mode at column _x_ and at row _y_ to _y2_  
| **procedure** DrawLine(x1, y1, x2, y2: integer; c: TBGRAPixel; DrawLastPixel: boolean; ADrawMode: TDrawMode = dmDrawWithTransparency); **override** ;  
| | Draws an aliased line from (x1,y1) to (x2,y2) using Bresenham's algorithm _c_ specifies the color. _DrawLastPixel_ specifies if (x2,y2) must be drawn. _ADrawMode_ specifies the mode to use when drawing the pixels  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: integer; c: TBGRAPixel; DrawLastPixel: boolean); **override** ;  
| | Draws an antialiased line from (x1,y1) to (x2,y2) using an improved version of Bresenham's algorithm _c_ specifies the color. _DrawLastPixel_ specifies if (x2,y2) must be drawn  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: integer; c1, c2: TBGRAPixel; dashLen: integer; DrawLastPixel: boolean); **override** ;  
| | Draws an antialiased line with two colors _c1_ and _c2_ as dashes of lenght _dashLen_  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: integer; c1, c2: TBGRAPixel; dashLen: integer; DrawLastPixel: boolean; **var** DashPos: integer); **override** ;  
| | Draws an antialiased line with two colors _c1_ and _c2_ as dashes of lenght _dashLen_. _DashPos_ can be used to specify the start dash position and to retrieve the dash position at the end of the line, in order to draw a polyline with consistent dashes  
| **procedure** EraseLine(x1, y1, x2, y2: integer; alpha: byte; DrawLastPixel: boolean); **override** ;  
| | Erases the line from (x1,y1) to (x2,y2) using Bresenham's algorithm. _alpha_ specifies how much to decrease. If _alpha_ = 0, nothing is changed and if _alpha_ = 255, all pixels become transparent. _DrawListPixel_ specifies if (x2,y2) must be changed  
| **procedure** EraseLineAntialias(x1, y1, x2, y2: integer; alpha: byte; DrawLastPixel: boolean); **override** ;  
| | Erases the line from (x1,y1) to (x2,y2) width antialiasing. _alpha_ specifies how much to decrease. If _alpha_ = 0, nothing is changed and if _alpha_ = 255, all pixels become transparent. _DrawListPixel_ specifies if (x2,y2) must be changed  
  
#### Drawing lines and polylines (floating point coordinates)

These functions use the current pen style/cap/join. The parameter _w_ specifies the width of the line and the base unit for dashes. See [coordinate system](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13"). 

| **procedure** DrawLineAntialias(x1, y1, x2, y2: single; c: TBGRAPixel; w: single); **override** ;  
---|---  
| | Draws a line from (x1,y1) to (x2,y2) using current pen style/cap/join  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: single; texture: IBGRAScanner; w: single); **override** ;  
| | Draws a line from (x1,y1) to (x2,y2) using current pen style/cap/join. _texture_ specifies the source color to use when filling the line  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: single; c: TBGRAPixel; w: single; Closed: boolean); **override** ;  
| | Draws a line from (x1,y1) to (x2,y2) using current pen style/cap/join. _Closed_ specifies if the end of the line is closed. If it is not closed, a space is left so that the next line can fit  
| **procedure** DrawLineAntialias(x1, y1, x2, y2: single; texture: IBGRAScanner; w: single; Closed: boolean); **override** ;  
| | Same as above with _texture_ specifying the source color to use when filling the line  
| **procedure** DrawPolyLineAntialias(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single); **override** ;  
| | Draws a polyline using current pen style/cap/join  
| **procedure** DrawPolyLineAntialias(**const** points: **array** **of** TPointF; texture: IBGRAScanner; w: single); **override** ;  
| | Draws a polyline using current pen style/cap/join. _texture_ specifies the source color to use when filling the line  
| **procedure** DrawPolyLineAntialias(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single; Closed: boolean); **override** ;  
| | Draws a polyline using current pen style/cap/join. _Closed_ specifies if the end of the line is closed. If it is not closed, a space is left so that the next line can fit  
| **procedure** DrawPolyLineAntialias(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single; fillcolor: TBGRAPixel); **override** ;  
| | Draws a polyline using current pen style/cap/join. _fillcolor_ specifies a color to fill the polygon formed by the points  
| **procedure** DrawPolyLineAntialiasAutocycle(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single); **override** ;  
| | Draws a polyline using current pen style/cap/join. The last point considered as a join with the first point if it has the same coordinate  
| **procedure** DrawPolygonAntialias(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single); **override** ;  
| | Draws a polygon using current pen style/cap/join. The polygon is always closed. You don't need to set the last point to be the same as the first point  
| **procedure** DrawPolygonAntialias(**const** points: **array** **of** TPointF; texture: IBGRAScanner; w: single); **override** ;  
| | Draws a polygon using current pen style/cap/join. The polygon is always closed. You don't need to set the last point to be the same as the first point  
| **procedure** DrawPolygonAntialias(**const** points: **array** **of** TPointF; c: TBGRAPixel; w: single; fillcolor: TBGRAPixel); **override** ;  
| | Draws a filled polygon using current pen style/cap/join. The polygon is always closed. You don't need to set the last point to be the same as the first point.  
| **procedure** EraseLineAntialias(x1, y1, x2, y2: single; alpha: byte; w: single); **override** ;  
| | Erases a line from (x1,y1) to (x2,y2) using current pen style/cap/join  
| **procedure** EraseLineAntialias(x1, y1, x2, y2: single; alpha: byte; w: single; Closed: boolean); **override** ;  
| | Erases a line from (x1,y1) to (x2,y2) using current pen style/cap/join. _Closed_ specifies if the end of the line is closed. If it is not closed, a space is left so that the next line can fit  
| **procedure** ErasePolyLineAntialias(**const** points: **array** **of** TPointF; alpha: byte; w: single); **override** ;  
| | Erases a polyline using current pen style/cap/join  
  
#### Rectangles (integer coordinates)

The integer coordinates of rectangles interpreted such that that the bottom/right pixels are not drawn. The width is equal to x2-x, and pixels are drawn from x to x2-1. If x = x2, then nothing is drawn. See [coordinate system](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13").   
These functions do not take into account current pen style/cap/join. They draw a continuous 1-pixel width border 

| **procedure** Rectangle(x, y, x2, y2: integer; c: TBGRAPixel; mode: TDrawMode); **override** ;  
---|---  
| | Draw a size border of a rectangle, using the specified _mode_  
| **procedure** Rectangle(x, y, x2, y2: integer; BorderColor, FillColor: TBGRAPixel; mode: TDrawMode); **override** ;  
| | Draw a filled rectangle with a border of color _BorderColor_ , using the specified _mode_  
| **procedure** FillRect(x, y, x2, y2: integer; c: TBGRAPixel; mode: TDrawMode); **override** ; **overload** ;  
| | Fills completely a rectangle, without any border, with the specified _mode_  
| **procedure** FillRect(x, y, x2, y2: integer; texture: IBGRAScanner; mode: TDrawMode); **override** ; **overload** ;  
| | Fills completely a rectangle, without any border, with the specified _texture_ and with the specified _mode_  
| **procedure** AlphaFillRect(x, y, x2, y2: integer; alpha: byte); **override** ;  
| | Sets the alpha value within the specified rectangle  
| **procedure** RoundRect(X1, Y1, X2, Y2: integer; DX, DY: integer; BorderColor, FillColor: TBGRAPixel; ADrawMode: TDrawMode = dmDrawWithTransparency); **override** ;  
| | Draws a filled round rectangle, with corners having an elliptical diameter of _DX_ and _DY_  
| **procedure** RoundRect(X1, Y1, X2, Y2: integer; DX, DY: integer; BorderColor: TBGRAPixel; ADrawMode: TDrawMode = dmDrawWithTransparency); **override** ;  
| | Draws a round rectangle, with corners having an elliptical diameter of _DX_ and _DY_  
  
#### Rectangles and ellipses (floating point coordinates)

These functions use the current pen style/cap/join. The parameter _w_ specifies the width of the line and the base unit for dashes   
The coordinates are pixel-centered, so that when filling a rectangle, if the supplied values are integers, the border will be half transparent. If you want the border to be completely filled, you can subtract/add 0.5 to the coordinates to include the remaining thin border. See [coordinate system](<BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13"). 

| **procedure** RectangleAntialias(x, y, x2, y2: single; c: TBGRAPixel; w: single; back: TBGRAPixel); **override** ;  
---|---  
| | Draws a rectangle with antialiasing and fills it with color _back_. Note that the pixel (x2,y2) is included contrary to integer coordinates  
| **procedure** RectangleAntialias(x, y, x2, y2: single; texture: IBGRAScanner; w: single); **override** ;  
| | Draws a rectangle with antialiasing. Note that the pixel (x2,y2) is included contrary to integer coordinates  
| **procedure** FillRectAntialias(x, y, x2, y2: single; c: TBGRAPixel); **override** ;  
| | Fills a rectangle with antialiasing. For example (-0.5,-0.5,0.5,0.5) fills one pixel  
| **procedure** FillRectAntialias(x, y, x2, y2: single; texture: IBGRAScanner); **override** ;  
| | Fills a rectangle with a texture  
| **procedure** EraseRectAntialias(x, y, x2, y2: single; alpha: byte); **override** ;  
| | Erases the content of a rectangle with antialiasing  
| **procedure** RoundRectAntialias(x,y,x2,y2,rx,ry: single; c: TBGRAPixel; w: single; options: TRoundRectangleOptions = []); **override** ;  
| | Draws a rounded rectangle border with antialiasing. The corners have an elliptical radius of _rx_ and _ry_. _options_ specifies how to draw the corners. See [geometry types](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types")  
| **procedure** RoundRectAntialias(x,y,x2,y2,rx,ry: single; texture: IBGRAScanner; w: single; options: TRoundRectangleOptions = []); **override** ;  
| | Draws a rounded rectangle border with the specified texture. The corners have an elliptical radius of _rx_ and _ry_. _options_ specifies how to draw the corners. See [geometry types](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types")  
| **procedure** RoundRectAntialias(x,y,x2,y2,rx,ry: single; pencolor: TBGRAPixel; w: single; fillcolor: TBGRAPixel; options: TRoundRectangleOptions = []); **override** ;  
| | Draws and fills a round rectangle  
| **procedure** RoundRectAntialias(x,y,x2,y2,rx,ry: single; penTexture: IBGRAScanner; w: single; fillTexture: IBGRAScanner; options: TRoundRectangleOptions = []); **override** ;  
| | Draws and fills a round rectangle with textures  
| **procedure** FillRoundRectAntialias(x,y,x2,y2,rx,ry: single; c: TBGRAPixel; options: TRoundRectangleOptions = []); **override** ;  
| | Fills a rounded rectangle with antialiasing. The corners have an elliptical radius of _rx_ and _ry_. _options_ specifies how to draw the corners. See [geometry types](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types")  
| **procedure** FillRoundRectAntialias(x,y,x2,y2,rx,ry: single; texture: IBGRAScanner; options: TRoundRectangleOptions = []); **override** ;  
| | Fills a rounded rectangle with a texture  
| **procedure** EraseRoundRectAntialias(x,y,x2,y2,rx,ry: single; alpha: byte; options: TRoundRectangleOptions = []); **override** ;  
| | Erases the content of a rounded rectangle with a texture  
| **procedure** EllipseAntialias(x, y, rx, ry: single; c: TBGRAPixel; w: single); **override** ;  
| | Draws an ellipse with antialising. _rx_ is the horizontal radius and _ry_ the vertical radius  
| **procedure** EllipseAntialias(x, y, rx, ry: single; texture: IBGRAScanner; w: single); **override** ;  
| | Draws an ellipse border with a _texture_  
| **procedure** EllipseAntialias(x, y, rx, ry: single; c: TBGRAPixel; w: single; back: TBGRAPixel); **override** ;  
| | Draws and fills an ellipse  
| **procedure** FillEllipseAntialias(x, y, rx, ry: single; c: TBGRAPixel); **override** ;  
| | Fills an ellipse  
| **procedure** FillEllipseAntialias(x, y, rx, ry: single; texture: IBGRAScanner); **override** ;  
| | Fills an ellipse with a _texture_  
| **procedure** FillEllipseLinearColorAntialias(x, y, rx, ry: single; outercolor, innercolor: TBGRAPixel); **override** ;  
| | Fills an ellipse with a gradient of color. _outercolor_ specifies the end color of the gradient on the border of the ellipse and _innercolor_ the end color of the gradient at the center of the ellipse  
| **procedure** EraseEllipseAntialias(x, y, rx, ry: single; alpha: byte); **override** ;  
| | Erases the content of an ellipse  
  
Additionally, there are also FloodFill() and Arc() methods. 

### Text functions
    
    
    class TBGRABitmap
      property FontName: string; read write;
      property FontStyle: TFontStyles; read write;
      property FontQuality : TBGRAFontQuality; read write;
      property FontOrientation: integer; read write;
      property FontHeight: integer; read write;
      property FontAntialias: Boolean; read write;
      property FontFullHeight: integer; read write;
      property FontPixelMetric: TFontPixelMetric;
      property FontRenderer: TBGRACustomFontRenderer; read write;
    
      procedure TextOut(x, y: single; sUTF8: string; c: TBGRAPixel; align: TAlignment);
      procedure TextOut(x, y: single; sUTF8: string; texture: IBGRAScanner; align: TAlignment);
      procedure TextOutAngle(x, y: single; orientationTenthDegCCW: integer; sUTF8: string; c: TBGRAPixel; align: TAlignment);
      procedure TextOutAngle(x, y: single; orientationTenthDegCCW: integer; sUTF8: string; texture: IBGRAScanner; align: TAlignment);
      procedure TextOut(x, y: single; sUTF8: string; c: TBGRAPixel);
      procedure TextOut(x, y: single; sUTF8: string; c: TColor);
      procedure TextOut(x, y: single; sUTF8: string; texture: IBGRAScanner);
      procedure TextRect(ARect: TRect; x, y: integer; sUTF8: string; style: TTextStyle; c: TBGRAPixel);
      procedure TextRect(ARect: TRect; x, y: integer; sUTF8: string; style: TTextStyle; texture: IBGRAScanner);
      procedure TextRect(ARect: TRect; sUTF8: string; halign: TAlignment; valign: TTextLayout; c: TBGRAPixel);
      procedure TextRect(ARect: TRect; sUTF8: string; halign: TAlignment; valign: TTextLayout; texture: IBGRAScanner);
      function TextSize(sUTF8: string): TSize;
    

See **[TBGRABitmap text functions](<TBGRABitmap_text_functions.md> "TBGRABitmap text functions")**.

---

_Source: [https://wiki.freepascal.org/TBGRABitmap_class](https://web.archive.org/web/20250515120453/https://wiki.freepascal.org/TBGRABitmap_class)_
