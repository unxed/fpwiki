# TBGRACustomBitmap and IBGRAScanner

Back to [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). 

This class and this interface are included in _BGRABitmapTypes_. 

For methods (procedure and functions), see [TBGRABitmap class](<TBGRABitmap_class.md> "TBGRABitmap class"). 

## Contents

  * 1 TBGRACustomBitmap and IBGRAScanner
    * 1.1 IBGRAScanner
    * 1.2 TBGRACustomBitmap
    * 1.3 Load and save files



### TBGRACustomBitmap and IBGRAScanner

#### IBGRAScanner

_IBGRAScanner_ = **interface**  
---  
| Interface for a scanner. A scanner is like an image, but its content has no limit and it can be calculated on the fly. It is like a infinite readonly image.   
  
Note: it must not implement reference counting even if it is an interface   
  
_TBGRACustomBitmap_ implements this interface and the content is repeated horizontally and vertically. There are also various classes in _BGRAGradientScanner_ unit that generate gradients on the fly and in _BGRATransform_ unit that provide geometrical transformations of images  
| **procedure** ScanMoveTo(X,Y: Integer);  
| | Move to the position (_X_ ,_Y_) for the next call to _ScanNextPixel_  
| **function** ScanNextPixel: TBGRAPixel;  
| | Scan the pixel at the current location and increments _X_  
| **function** ScanAt(X,Y: Single): TBGRAPixel;  
| | Scan at any location using floating point coordinates  
| **function** ScanAtInteger(X,Y: integer): TBGRAPixel;  
| | Scan at any location using integer coordinates  
| **procedure** ScanPutPixels(pdest: PBGRAPixel; count: integer; mode: TDrawMode);  
| | Copy a row of pixels from _X_ to _X_ +_count_ -1 to a specified destination _pdest_. _mode_ indicates how to combine with existing data  
| **function** IsScanPutPixelsDefined: boolean;  
| | Returns True if the function _ScanPutPixels_ is available. Otherwise you need to call _ScanNextPixel_ and combine pixels for example with _SetPixel_  
| _TScanAtFunction_ = **function** (X,Y: Single): TBGRAPixel **of** **object** ;  
| | A type of function of a scanner that returns the content at floating point coordinates  
| _TScanAtIntegerFunction_ = **function** (X,Y: Integer): TBGRAPixel **of** **object** ;  
| | A type of function of a scanner that returns the content at integer coordinates  
| _TScanNextPixelFunction_ = **function** : TBGRAPixel **of** **object** ;  
| | A type of function of a scanner that returns the next pixel  
_TBGRACustomScanner_ = **class**(IBGRAScanner)  
| Base class for implementing _IBGRAScanner_ interface  
  
#### TBGRACustomBitmap

_TBGRACustomBitmap_ = **class**(TFPCustomImage,IBGRAScanner)  
---  
| This is the base class for _TBGRABitmap_. It is the direct parent of _TBGRADefaultBitmap_ class, which is the parent of the diverse implementations. A bitmap can be used as a scanner using the interface _IBGRAScanner_  
| _Caption_ : **string** ;  
| | User defined caption. It does not appear on the image  
| _FillMode_ : TFillMode;  
| | Method to use when filling polygons (winding or alternate). See [BGRAGraphics](<BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics")  
| _LinearAntialiasing_ : boolean;  
| | Specifies if linear antialiasing must be used when drawing antialiased shapes  
| _ResampleFilter_ : TResampleFilter;  
| | Resample filter is used when resizing the bitmap. See [resampling types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| _ScanInterpolationFilter_ : TResampleFilter;  
| | Scan interpolation filter is used when the bitmap is used as a scanner (interface _IBGRAScanner_)  
| _ScanOffset_ : TPoint;  
| | Offset to apply when the image is scanned using _IBGRAScanner_ interface  
| **property** Width: integer **read** ;  
| | Width of the image in pixels  
| **property** Height: integer **read** ;  
| | Height of the image in pixels  
| **property** ClipRect: TRect **read** **write** ;  
| | Clipping rectangle for all drawing functions  
| **property** NbPixels: integer **read** ;  
| | Total number of pixels. It is always true that _NbPixels_ = _Width_ * _Height_  
| **property** ScanLine[y: integer]: PBGRAPixel **read** ;  
| | Returns the address of the left-most pixel of any line. The parameter y ranges from 0 to Height-1  
| **property** LineOrder: TRawImageLineOrder **read** ;  
| | Indicates the order in which lines are stored in memory. If it is equal to _riloTopToBottom_ , the first line is the top line. If it is equal to _riloBottomToTop_ , the first line is the bottom line. See [miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| **property** Data: PBGRAPixel **read** ;  
| | Provides a pointer to the first pixel in memory. Depending on the _LineOrder_ property, this can be the top-left pixel or the bottom-left pixel. There is no padding between scanlines, so the start of the next line is at the address _Data_ \+ _Width_. See [BGRABitmap tutorial 4](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")  
| **property** RefCount: integer **read** ;  
| | Number of references to this image. It is increased by the function _NewReference_ and decreased by the function _FreeReference_  
| **property** Empty: boolean **read** ;  
| | Returns True if the bitmap only contains transparent pixels or has a size of zero  
| **property** HasTransparentPixels: boolean **read** ;  
| | Returns True if there are transparent or semitransparent pixels, and so if the image would be stored with an alpha channel  
| **property** AverageColor: TColor **read** ;  
| | Average color of the image  
| **property** AveragePixel: TBGRAPixel **read** ;  
| | Average color (including alpha) of the image  
| **property** CanvasFP: TFPImageCanvas **read** ;  
| | Canvas compatible with FreePascal  
| **property** CanvasDrawModeFP: TDrawMode **read** **write** ;  
| | Draw mode to used when image is access using FreePascal functions and _Colors_ property  
| **property** Bitmap: TBitmap **read** ;  
| | Bitmap in a format compatible with the current GUI. Don't forget to call _InvalidateBitmap_ before using it if you changed something with direct pixel access (_Scanline_ and _Data_)  
| **property** Canvas: TCanvas **read** ;  
| | Canvas provided by the GUI  
| **property** CanvasOpacity: byte **read** **write** ;  
| | Opacity to apply to changes made using GUI functions, provided _CanvasAlphaCorrection_ is set to _True_  
| **property** CanvasAlphaCorrection: boolean **read** **write** ;  
| | Specifies if the alpha values must be corrected after GUI access to the bitmap  
| _JoinStyle_ : TPenJoinStyle;  
| | How to join segments. See [BGRAGraphics](<BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics")  
| _JoinMiterLimit_ : single;  
| | Limit for the extension of the segments when joining them with _pjsMiter_ join style, expressed in multiples of the width of the pen  
| **property** PenStyle: TPenStyle **read** **write** ;  
| | Pen style. See [BGRAGraphics](<BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics")  
| **property** CustomPenStyle: TBGRAPenStyle **read** **write** ;  
| | Custom pen style. See [geometric types](<BGRABitmap_Geometry_types.md> "BGRABitmap Geometry types")  
| **property** LineCap: TPenEndCap **read** **write** ;  
| | How to draw the ends of a line  
| **property** ArrowStartSize: TPointF **read** **write** ;  
| | Size of arrows at the start of the line  
| **property** ArrowEndSize: TPointF **read** **write** ;  
| | Size of arrows at the end of the line  
| **property** ArrowStartOffset: single **read** **write** ;  
| | Offset of the arrow from the start of the line  
| **property** ArrowEndOffset: single **read** **write** ;  
| | Offset of the arrow from the end of the line  
| **property** ArrowStartRepeat: integer **read** **write** ;  
| | Number of times to repeat the starting arrow  
| **property** ArrowEndRepeat: integer **read** **write** ;  
| | Number of times to repeat the ending arrow  
| _FontName_ : **string** ;  
| | Specifies the font to use. Unless the font renderer accept otherwise, the name is in human readable form, like 'Arial', 'Times New Roman', ...  
| _FontStyle_ : TFontStyles;  
| | Specifies the set of styles to be applied to the font. These can be _fsBold_ , _fsItalic_ , _fsStrikeOut_ , _fsUnderline_. So the value [_fsBold_ ,_fsItalic_] means that the font must be bold and italic. See [miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| _FontQuality_ : TBGRAFontQuality;  
| | Specifies the quality of rendering. Default value is _fqSystem_. See [miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| _FontOrientation_ : integer;  
| | Specifies the rotation of the text, for functions that support text rotation. It is expressed in tenth of degrees, positive values going counter-clockwise.  
| _FontVerticalAnchor_ : TFontVerticalAnchor;  
| | Specifies how the font is vertically aligned relative to the start coordinate. See [miscellaneous types](<BGRABitmap_Miscellaneous_types.md> "BGRABitmap Miscellaneous types")  
| **property** FontHeight: integer **read** **write** ;  
| | Specifies the height of the font in pixels without taking into account additional line spacing. A negative value means that it is the full height instead (see below)  
| **property** FontFullHeight: integer **read** **write** ;  
| | Specifies the height of the font in pixels, taking into account the additional line spacing defined for the font  
| **property** FontAntialias: Boolean **read** **write** ;  
| | Simplified property to specify the quality (see _FontQuality_)  
| **property** FontPixelMetric: TFontPixelMetric **read** ;  
| | Returns measurement for the current font in pixels  
| **property** FontRenderer: TBGRACustomFontRenderer **read** **write** ;  
| | Specifies the font renderer. When working with the LCL, by default it is an instance of _TLCLFontRenderer_ of unit _BGRAText_. Other renderers are provided in _BGRATextFX_ unit and _BGRAVectorize_ unit. Additionally, _BGRAFreeType_ provides a renderer independent from the LCL.   
  
Once you assign a renderer, it will automatically be freed when the bitmap is freed. The renderers may provide additional styling for the font, not accessible with the properties in this class   
  
See [font rendering](<BGRABitmap_tutorial_Font_rendering.md> "BGRABitmap tutorial Font rendering")  
  
#### Load and save files

| **procedure** LoadFromFile(**const** filename: **string**); **virtual** ;  
---|---  
| | Load image from a file. _filename_ is an ANSI string  
| **procedure** LoadFromFile(**const** filename:**string** ; Handler:TFPCustomImageReader); **virtual** ;  
| | Load image from a file with the specified image reader. _filename_ is an ANSI string  
| **procedure** LoadFromFileUTF8(**const** filenameUTF8: **string**); **virtual** ;  
| | Load image from a file. _filename_ is an UTF8 string  
| **procedure** LoadFromFileUTF8(**const** filenameUTF8: **string** ; AHandler: TFPCustomImageReader); **virtual** ;  
| | Load image from a file with the specified image reader. _filename_ is an UTF8 string  
| **procedure** LoadFromStream(Str: TStream); **virtual** ; **overload** ;  
| | Load image from a stream. Format is detected automatically  
| **procedure** LoadFromStream(Str: TStream; Handler: TFPCustomImageReader); **virtual** ; **overload** ;  
| | Load image from a stream. The specified image reader is used  
| **procedure** SaveToFile(**const** filename: **string**); **virtual** ; **overload** ;  
| | Save image to a file. The format is guessed from the file extension. _filename_ is an ANSI string  
| **procedure** SaveToFile(**const** filename: **string** ; Handler:TFPCustomImageWriter); **virtual** ; **overload** ;  
| | Save image to a file with the specified image writer. _filename_ is an ANSI string  
| **procedure** SaveToFileUTF8(**const** filenameUTF8: **string**); **virtual** ; **overload** ;  
| | Save image to a file. The format is guessed from the file extension. _filename_ is an ANSI string  
| **procedure** SaveToFileUTF8(**const** filenameUTF8: **string** ; Handler:TFPCustomImageWriter); **virtual** ; **overload** ;  
| | Save image to a file with the specified image writer. _filename_ is an UTF8 string  
| **procedure** SaveToStream (Str:TStream; Handler:TFPCustomImageWriter);  
| | Save image to a stream with the specified image writer  
| **procedure** SaveToStreamAs(Str: TStream; AFormat: TBGRAImageFormat); **virtual** ;  
| | Save image to a stream in the specified image format  
| **procedure** SaveToStreamAsPng(Str: TStream); **virtual** ;  
| | Save image to a stream in PNG format  
| **procedure** LoadFromDevice(DC: System.THandle); **virtual** ; **abstract** ; **overload** ;  
| | Gets the content of the specified device context  
| **procedure** LoadFromDevice(DC: System.THandle; ARect: TRect); **virtual** ; **abstract** ; **overload** ;  
| | Gets the content from the specified rectangular area of a device context  
| **procedure** TakeScreenshotOfPrimaryMonitor; **virtual** ; **abstract** ;  
| | Fills the content with a screenshot of the primary monitor  
| **procedure** TakeScreenshot(ARect: TRect); **virtual** ; **abstract** ;  
| | Fills the content with a screenshot of the specified rectangular area of the desktop (it can be from any screen)  
  
For more methods, see derived class [TBGRABitmap](<TBGRABitmap_class.md> "TBGRABitmap class")

---

_Source: [https://wiki.freepascal.org/TBGRACustomBitmap_and_IBGRAScanner](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/TBGRACustomBitmap_and_IBGRAScanner)_
