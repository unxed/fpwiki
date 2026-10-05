# BGRABitmap Miscellaneous types

Miscellaneous types in [BGRABitmap](<BGRABitmap.md> "BGRABitmap"). They are provided by _BGRABitmapTypes_ unit. 

## Contents

  * 1 Miscellaneous types
    * 1.1 Imported from GraphType
    * 1.2 Integer math
    * 1.3 Types provided for fonts
    * 1.4 Images and resampling



### Miscellaneous types

**const** BGRABitmapVersion = 11050800;  
---  
| Current version expressed as an integer with each part multiplied by 100  
**function** BGRABitmapVersionStr: **string** ;  
| String representation of the version, numbers separated by dots  
_TMultiFileContainer_ = BGRAMultiFileType.TMultiFileContainer;  
| Generic definition of a multifile container  
_Int32or64_ = BGRAClasses.Int32or64;  
| Signed integer value of at least 32 bits  
_UInt32or64_ = BGRAClasses.UInt32or64;  
| Unsigned integer value of at least 32 bits  
_HDC_ = {$**ifdef** BGRABITMAP_USE_LCL}LCLType.HDC{$**else**}PtrUInt{$**endif**};  
| Device context handle (using LCL if available)  
_TFloodfillMode_ = (  
| Options when doing a floodfill (also called bucket fill)  
| _fmSet_ ,  
| | Pixels that are filled are replaced  
| _fmDrawWithTransparency_ ,  
| | Pixels that are filled are drawn upon with the fill color  
| _fmLinearBlend_ ,  
| | Pixels that are filled are drawn without gamma correction upon with the fill color  
| _fmXor_ ,  
| | Pixels that are XORed with the fill color  
| _fmProgressive_);  
| | Pixels that are filled are drawn upon to the extent that the color underneath is similar to the start color. The more different the different is, the less it is drawn upon  
_TMedianOption_ = (moNone, moLowSmooth, moMediumSmooth, moHighSmooth);  
| Specifies how much smoothing is applied to the computation of the median  
_TRadialBlurType_ = (  
| Specifies the shape of a predefined blur  
| _rbNormal_ ,  
| | Gaussian-like, pixel importance decreases progressively  
| _rbDisk_ ,  
| | Disk blur, pixel importance does not decrease progressively  
| _rbCorona_ ,  
| | Pixel are considered when they are at a certain distance  
| _rbPrecise_ ,  
| | Gaussian-like, but 10 times smaller than _rbNormal_  
| _rbFast_ ,  
| | Gaussian-like but simplified to be computed faster  
| _rbBox_);  
| | Box blur, pixel importance does not decrease progressively and the pixels are included when they are in a square. This is much faster than _rbFast_ however you may get square shapes in the resulting image  
| **const** RadialBlurTypeToStr: **array**[TRadialBlurType] **of** **string** =  
| | String constants to represent TRadialBlurType values  
_TEmbossOption_ = (  
| Possible options when applying emboss filter  
| _eoTransparent_ ,  
| | Transparent output except when there borders were detected  
| _eoPreserveHue_);  
| | Preserve the original hue  
| _TEmbossOptions_ = **set** **of** TEmbossOption;  
| | Sets of emboss options  
_TBGRAImageFormat_ = (  
| List of image formats  
| _ifUnknown_ ,  
| | Unknown format  
| _ifJpeg_ ,  
| | JPEG format, opaque, lossy compression  
| _ifPng_ ,  
| | PNG format, transparency, lossless compression. Can be animated (see BGRAAnimatedGif)  
| _ifGif_ ,  
| | GIF format, single transparent color, lossless in theory but only low number of colors allowed. Can be animated (see BGRAAnimatedGif)  
| _ifBmp_ ,  
| | BMP format, transparency, no compression. Note that transparency is not supported by all BMP readers so it is recommended to avoid storing images with transparency in this format  
| _ifBmpMioMap_ ,  
| | iGO BMP (16-bit, rudimentary lossless compression)  
| _ifIco_ ,  
| | ICO format, contains different sizes of the same image  
| _ifCur_ ,  
| | CUR format, has hotspot, contains different sizes of the same image  
| _ifPcx_ ,  
| | PCX format, opaque, rudimentary lossless compression  
| _ifPaintDotNet_ ,  
| | Paint.NET format, layers, lossless compression  
| _ifLazPaint_ ,  
| | LazPaint format, layers, lossless compression  
| _ifOpenRaster_ ,  
| | OpenRaster format, layers, lossless compression  
| _ifPhoxo_ ,  
| | Phoxo format, layers  
| _ifPsd_ ,  
| | Photoshop format, layers, rudimentary lossless compression  
| _ifTarga_ ,  
| | Targa format (TGA), transparency, rudimentary lossless compression  
| _ifTiff_ ,  
| | TIFF format, limited support  
| _ifXwd_ ,  
| | X-Window capture, limited support  
| _ifXPixMap_ ,  
| | X-Pixmap, text encoded image, limited support  
| _ifPortableAnyMap_ ,  
| | text or binary encoded image, no compression, extension PBM, PGM, PPM  
| _ifSvg_ ,  
| | Scalable Vector Graphic, vectorial, read-only as raster  
| _ifWebP_ ,  
| | Lossless or lossy compression using V8 algorithm (need libwebp library)  
| _ifAvif_  
| | Lossless or lossy compression using Avif algorithm (need libavif library)  
| _DefaultBGRAImageReader_ : **array**[TBGRAImageFormat] **of** TFPCustomImageReaderClass;  
| | List of stream readers for images  
| _DefaultBGRAImageWriter_ : **array**[TBGRAImageFormat] **of** TFPCustomImageWriterClass;  
| | List of stream writers for images  
| **function** DetectFileFormat(AFilenameUTF8: **string**): TBGRAImageFormat;  
| | Detect the file format of a given file  
| **function** DetectFileFormat(AStream: TStream; ASuggestedExtensionUTF8: **string** = ''): TBGRAImageFormat;  
| | Detect the file format of a given stream. _ASuggestedExtensionUTF8_ can be provided to guess the format  
| **function** SuggestImageFormat(AFilenameOrExtensionUTF8: **string**): TBGRAImageFormat;  
| | Returns the file format that is most likely to be stored in the given filename (according to its extension)  
| **function** SuggestImageExtension(AFormat: TBGRAImageFormat): **string** ;  
| | Returns a likely image extension for the format  
| **function** CreateBGRAImageReader(AFormat: TBGRAImageFormat): TFPCustomImageReader;  
| | Create an image reader for the given format  
| **function** CreateBGRAImageWriter(AFormat: TBGRAImageFormat; AHasTransparentPixels: boolean): TFPCustomImageWriter;  
| | Create an image writer for the given format. _AHasTransparentPixels_ specifies if alpha channel must be supported  
_TBGRALoadingOption_ = (  
| Possible options when loading an image  
| _loKeepTransparentRGB_ ,  
| | Do not clear RGB channels when alpha is zero (not recommended)  
| _loBmpAutoOpaque_ ,  
| | Consider BMP to be opaque if no alpha value is provided (for compatibility)  
| _loJpegQuick_);  
| | Load JPEG quickly however with a lower quality  
| _TBGRALoadingOptions_ = **set** **of** TBGRALoadingOption;  
| | Set of options when loading  
_TTextLayout_ = BGRAGraphics.TTextLayout;  
| Text layout (vertical position)  
| _tlTop_ = BGRAGraphics.tlTop;  
| | Text aligned to the top  
| _tlCenter_ = BGRAGraphics.tlCenter;  
| | Text aligned vertically to the center  
| _tlBottom_ = BGRAGraphics.tlBottom;  
| | Text aligned to the bottom  
_TFontBidiMode_ = BGRAUnicode.TFontBidiMode;  
| Bidi-mode preference (right-to-left or left-to-right)  
| _fbmAuto_ = BGRAUnicode.fbmAuto;  
| | Automatic bidi-mode, depending on first letter type  
| _fbmLeftToRight_ = BGRAUnicode.fbmLeftToRight;  
| | Always left-to-right (but can embed another direction)  
| _fbmRightToLeft_ = BGRAUnicode.fbmRightToLeft;  
| | Always right-to-left (but can embed another direction)  
_TBidiTextAlignment_ = (  
| Alignment relative to the bidi-mode  
| _btaNatural_ ,  
| | Natural alignment: left-aligned for left-to-right and right-aligned for right-to-left text  
| _btaOpposite_ ,  
| | Opposite of natural alignment  
| _btaLeftJustify_ ,  
| | Always left-aligned  
| _btaRightJustify_ ,  
| | Always right-aligned  
| _btaCenter_);  
| | Centered  
| **function** AlignmentToBidiTextAlignment(AAlign: TAlignment; ARightToLeft: boolean): TBidiTextAlignment; **overload** ;  
| | Converts an alignment to a bidi alignement relative to a bidi-mode  
| **function** AlignmentToBidiTextAlignment(AAlign: TAlignment): TBidiTextAlignment; **overload** ;  
| | Converts an alignment to a bidi alignement independent of bidi-mode  
| **function** BidiTextAlignmentToAlignment(ABidiAlign: TBidiTextAlignment; ARightToLeft: boolean): TAlignment;  
| | Converts a bidi alignment to a classic alignement according to bidi-mode  
**function** CheckPutImageBounds(x, y, tx, ty: integer; **out** minxb, minyb, maxxb, maxyb, ignoreleft: integer; **const** cliprect: TRect): boolean;  
| Checks the bounds of an image in the given clipping rectangle  
  
#### Imported from GraphType

_TRawImageLineOrder_ = (  
---  
| Order of the lines in an image  
| _riloTopToBottom_ ,  
| | The first line in memory (line 0) is the top line  
| _riloBottomToTop_);  
| | The first line in memory (line 0) is the bottom line  
_TRawImageBitOrder_ = (  
| Order of the bits in a byte containing pixel values  
| _riboBitsInOrder_ ,  
| | The lowest bit is on the left. So with a monochrome picture, bit 0 would be pixel 0  
| _riboReversedBits_);  
| | The lowest bit is on the right. So with a momochrome picture, bit 0 would be pixel 7 (bit 1 would be pixel 6, ...)  
_TRawImageByteOrder_ = (  
| Order of the bytes in a group of byte containing pixel values  
| _riboLSBFirst_ ,  
| | Least significant byte first (little endian)  
| _riboMSBFirst_);  
| | most significant byte first (big endian)  
_TGraphicsBevelCut_ =  
| Definition of a single line 3D bevel  
| _bvNone_ ,  
| | No bevel  
| _bvLowered_ ,  
| | Shape is lowered, light is on the bottom-right corner  
| _bvRaised_ ,  
| | Shape is raised, light is on the top-left corner  
| _bvSpace_);  
| | Shape is at the same level, there is no particular lighting  
  
#### Integer math

**function** PositiveMod(value, cycle: Int32or64): Int32or64; **inline** ; **overload** ;  
---  
| Computes the value modulo cycle, and if the _value_ is negative, the result is still positive  
**function** Sin65536(value: word): Int32or64; **inline** ;  
| Returns an integer approximation of the sine. Value ranges from 0 to 65535, where 65536 corresponds to the next cycle  
**function** Cos65536(value: word): Int32or64; **inline** ;  
| Returns an integer approximation of the cosine. Value ranges from 0 to 65535, where 65536 corresponds to the next cycle  
**function** ByteSqrt(value: byte): byte; **inline** ;  
| Returns the square root of the given byte, considering that 255 is equal to unity  
  
#### Types provided for fonts

_TBGRAFontQuality_ = (  
---  
| Quality to be used to render text  
| _fqSystem_ ,  
| | Use the system capabilities. It is rather fast however it may be not be smoothed.  
| _fqSystemClearType_ ,  
| | Use the system capabilities to render with ClearType. This quality is of course better than _fqSystem_ however it may not be perfect.  
| _fqFineAntialiasing_ ,  
| | Garanties a high quality antialiasing.  
| _fqFineClearTypeRGB_ ,  
| | Fine antialiasing with ClearType assuming an LCD display in red/green/blue order  
| _fqFineClearTypeBGR_);  
| | Fine antialiasing with ClearType assuming an LCD display in blue/green/red order  
| _TGetFineClearTypeAutoFunc_ = **function**(): TBGRAFontQuality;  
| | Function type to detect the adequate ClearType mode  
| _fqFineClearType_ : TGetFineClearTypeAutoFunc;  
| | Provide function to detect the adequate ClearType mode  
_TFontPixelMetric_ = **record**  
| Measurements of a font  
| _Defined_ : boolean;  
| | The values have been computed  
| _Baseline_ ,  
| | Position of the baseline, where most letters lie  
| _xLine_ ,  
| | Position of the top of the small letters (x being one of them)  
| _CapLine_ ,  
| | Position of the top of the UPPERCASE letters  
| _DescentLine_ ,  
| | Position of the bottom of letters like g and p  
| _Lineheight_ : integer;  
| | Total line height including line spacing defined by the font  
_TFontPixelMetricF_ = **record**  
| Measurements of a font in floating point values  
| _Defined_ : boolean;  
| | The values have been computed  
| _Baseline_ ,  
| | Position of the baseline, where most letters lie  
| _xLine_ ,  
| | Position of the top of the small letters (x being one of them)  
| _CapLine_ ,  
| | Position of the top of the UPPERCASE letters  
| _DescentLine_ ,  
| | Position of the bottom of letters like g and p  
| _Lineheight_ : single;  
| | Total line height including line spacing defined by the font  
_TFontVerticalAnchor_ = (  
| Vertical anchoring of the font. When text is drawn, a start coordinate is necessary. Text can be positioned in different ways. This enum defines what position it is regarding the font  
| _fvaTop_ ,  
| | The top of the font. Everything will be drawn below the start coordinate.  
| _fvaCenter_ ,  
| | The center of the font  
| _fvaCapLine_ ,  
| | The top of capital letters  
| _fvaCapCenter_ ,  
| | The center of capital letters  
| _fvaXLine_ ,  
| | The top of small letters  
| _fvaXCenter_ ,  
| | The center of small letters  
| _fvaBaseline_ ,  
| | The baseline, the bottom of most letters  
| _fvaDescentLine_ ,  
| | The bottom of letters that go below the baseline  
| _fvaBottom_);  
| | The bottom of the font. Everything will be drawn above the start coordinate  
_TWordBreakHandler_ = **procedure**(**var** ABeforeUTF8, AAfterUTF8: **string**) **of** **object** ;  
| Definition of a function that handles work-break  
_TBGRATypeWriterAlignment_ = (twaTopLeft, twaTop, twaTopRight, twaLeft, twaMiddle, twaRight, twaBottomLeft, twaBottom, twaBottomRight);  
| Alignment for a typewriter, that does not have any more information than a square shape containing glyphs  
_TBGRATypeWriterOutlineMode_ = (twoPath, twoFill, twoStroke, twoFillOverStroke, twoStrokeOverFill, twoFillThenStroke, twoStrokeThenFill);  
| How a typewriter must render its content on a Canvas2d  
_TBGRACustomFontRenderer_ = **class**  
| Abstract class for all font renderers  
| _FFontEmHeightF_ : single;  
| | Specifies the height of the font without taking into account additional line spacing. A negative value means that it is the full height instead  
| **function** GetFontEmHeight: integer;  
| | Retrieves the em-height of the font  
| **procedure** SetFontEmHeight(AValue: integer);  
| | Sets the font height as em-height  
| _FontName_ : **string** ;  
| | Specifies the font to use. Unless the font renderer accept otherwise, the name is in human readable form, like 'Arial', 'Times New Roman', ...  
| _FontStyle_ : TFontStyles;  
| | Specifies the set of styles to be applied to the font. These can be fsBold, fsItalic, fsStrikeOut, fsUnderline. So the value [fsBold, fsItalic] means that the font must be bold and italic  
| _FontQuality_ : TBGRAFontQuality;  
| | Specifies the quality of rendering. Default value is fqSystem  
| _FontOrientation_ : integer;  
| | Specifies the rotation of the text, for functions that support text rotation. It is expressed in tenth of degrees, positive values going counter-clockwise  
| **function** GetFontPixelMetric: TFontPixelMetric; **virtual** ; **abstract** ;  
| | Returns measurement for the current font in pixels  
| **function** GetFontPixelMetricF: TFontPixelMetricF; **virtual** ;  
| | Returns measurement for the current font in fractional pixels  
| **function** FontExists(AName: **string**): boolean; **virtual** ; **abstract** ;  
| | Checks whether a font exists  
| **function** TextVisible(**const** AColor: TBGRAPixel): boolean; **virtual** ;  
| | Checks if any text would be visible using the specified color  
| **function** TextSize(sUTF8: **string**): TSize; **overload** ; **virtual** ; **abstract** ;  
| | Returns the total size of the string provided using the current font. Orientation is not taken into account, so that the width is horizontal  
| **function** TextSizeF(sUTF8: **string**): TPointF; **overload** ; **virtual** ;  
| | Returns the total floating point size of the string provided using the current font. Orientation is not taken into account, so that the width is horizontal  
| **function** TextSize(sUTF8: **string** ; AMaxWidth: integer; ARightToLeft: boolean): TSize; **overload** ; **virtual** ; **abstract** ;  
| | Returns the total size of the string provided given a maximum width and RTL mode, using the current font. Orientation is not taken into account, so that the width is along the text  
| **function** TextSizeF(sUTF8: **string** ; AMaxWidthF: single; ARightToLeft: boolean): TPointF; **overload** ; **virtual** ;  
| | Returns the total floating point size of the string provided given a maximum width and RTL mode, using the current font. Orientation is not taken into account, so that the width is along the text  
| **function** TextSizeAngle(sUTF8: **string** ; orientationTenthDegCCW: integer): TSize; **virtual** ;  
| | Returns the total size of the string provided using the current font, with the given orientation, along the text  
| **function** TextSizeAngleF(sUTF8: **string** ; orientationTenthDegCCW: integer): TPointF; **virtual** ;  
| | Returns the total floating-point size of the string provided using the current font, with the given orientation, along the text  
| **function** TextFitInfo(sUTF8: **string** ; AMaxWidth: integer): integer; **virtual** ; **abstract** ;  
| | Returns the number of Unicode characters that fit into the specified size  
| **function** TextFitInfoF(sUTF8: **string** ; AMaxWidthF: single): integer; **virtual** ;  
| | Returns the number of Unicode characters that fit into the specified floating-point size  
| **procedure** TextOut(ADest: TBGRACustomBitmap; x, y: single; sUTF8: **string** ; c: TBGRAPixel; align: TAlignment); **overload** ; **virtual** ; **abstract** ;  
| | Draws the UTF8 encoded string, with color _c_. If align is taLeftJustify, (_x_ , _y_) is the top-left corner. If align is taCenter, (_x_ , _y_) is at the top and middle of the text. If align is taRightJustify, (_x_ , _y_) is the top-right corner. The value of _FontOrientation_ is taken into account, so that the text may be rotated  
| **procedure** TextOut(ADest: TBGRACustomBitmap; x, y: single; sUTF8: **string** ; c: TBGRAPixel; align: TAlignment; ARightToLeft: boolean); **overload** ; **virtual** ;  
| | Same as above but with given RTL mode  
| **procedure** TextOut(ADest: TBGRACustomBitmap; x, y: single; sUTF8: **string** ; texture: IBGRAScanner; align: TAlignment); **overload** ; **virtual** ; **abstract** ;  
| | Same as above functions, except that the text is filled using texture. The value of _FontOrientation_ is taken into account, so that the text may be rotated  
| **procedure** TextOut(ADest: TBGRACustomBitmap; x, y: single; sUTF8: **string** ; texture: IBGRAScanner; align: TAlignment; ARightToLeft: boolean); **overload** ; **virtual** ;  
| | Same as above but with given RTL mode  
| **procedure** TextOutAngle(ADest: TBGRACustomBitmap; x, y: single; orientationTenthDegCCW: integer; sUTF8: **string** ; c: TBGRAPixel; align: TAlignment); **overload** ; **virtual** ; **abstract** ;  
| | Same as above, except that the orientation is specified, overriding the value of the property _FontOrientation_  
| **procedure** TextOutAngle(ADest: TBGRACustomBitmap; x, y: single; orientationTenthDegCCW: integer; sUTF8: **string** ; c: TBGRAPixel; align: TAlignment; ARightToLeft: boolean); **overload** ; **virtual** ;  
| | Same as above but with given RTL mode  
| **procedure** TextOutAngle(ADest: TBGRACustomBitmap; x, y: single; orientationTenthDegCCW: integer; sUTF8: **string** ; texture: IBGRAScanner; align: TAlignment); **overload** ; **virtual** ; **abstract** ;  
| | Same as above, except that the orientation is specified, overriding the value of the property _FontOrientation_  
| **procedure** TextOutAngle(ADest: TBGRACustomBitmap; x, y: single; orientationTenthDegCCW: integer; sUTF8: **string** ; texture: IBGRAScanner; align: TAlignment; ARightToLeft: boolean); **overload** ; **virtual** ;  
| | Same as above but with given RTL mode  
| **procedure** TextRect(ADest: TBGRACustomBitmap; ARect: TRect; x, y: integer; sUTF8: **string** ; style: TTextStyle; c: TBGRAPixel); **overload** ; **virtual** ; **abstract** ;  
| | Draw the UTF8 encoded string at the coordinate (_x_ , _y_), clipped inside the rectangle _ARect_. Additional style information is provided by the style parameter. The color _c_ is used to fill the text. No rotation is applied.  
| **procedure** TextRect(ADest: TBGRACustomBitmap; ARect: TRect; x, y: integer; sUTF8: **string** ; style: TTextStyle; texture: IBGRAScanner); **overload** ; **virtual** ; **abstract** ;  
| | Same as above except a _texture_ is used to fill the text  
| **procedure** CopyTextPathTo(ADest: IBGRAPath; x, y: single; s: **string** ; align: TAlignment); **virtual** ; //optional  
| | Copy the path for the UTF8 encoded string into _ADest_. If _align_ is _taLeftJustify_ , (_x_ , _y_) is the top-left corner. If _align_ is _taCenter_ , (_x_ , _y_) is at the top and middle of the text. If _align_ is _taRightJustify_ , (_x_ , _y_) is the top-right corner.  
| **procedure** CopyTextPathTo(ADest: IBGRAPath; x, y: single; s: **string** ; align: TAlignment; ARightToLeft: boolean); **virtual** ; //optional  
| | Same as above but with given RTL mode  
| **function** HandlesTextPath: boolean; **virtual** ;  
| | Check whether the renderer can produce text path  
| **property** FontEmHeight: integer **read** GetFontEmHeight **write** SetFontEmHeight;  
| | Font em-height as an integer  
| **property** FontEmHeightF: single **read** FFontEmHeightF **write** FFontEmHeightF;  
| | Font em-height as a single-precision floating point value  
_TBGRATextOutImproveReadabilityMode_ = (  
| Output mode for the improved renderer for readability. This is used by the font renderer based on LCL in BGRAText  
| _irMask_ ,  
| | Render the grayscale mask  
| _irNormal_ ,  
| | Render normally with provided the color or texture  
| _irClearTypeRGB_ ,  
| | Render with ClearType for RGB ordered display  
| _irClearTypeBGR_);  
| | Render with ClearType for BGR ordered display  
**function** CleanTextOutString(**const** s: **string**): **string** ;  
| Removes line ending and tab characters from a string (for a function like _TextOut_ that does not handle this). this works with UTF8 strings as well  
**function** RemoveLineEnding(**var** s: **string** ; indexByte: integer): boolean;  
| Remove the line ending at the specified position or return False. This works with UTF8 strings however the index is the byte index  
**function** RemoveLineEndingUTF8(**var** sUTF8: **string** ; indexUTF8: integer): boolean;  
| Remove the line ending at the specified position or return False. The index is the character index, that may be different from the byte index  
**procedure** BGRADefaultWordBreakHandler(**var** ABefore, AAfter: **string**);  
| Default word break handler  
  
#### Images and resampling

_TResampleMode_ = (  
---  
| How the resample is to be computed  
| _rmSimpleStretch_ ,  
| | Low quality resample by repeating pixels, stretching them  
| _rmFineResample_);  
| | Use resample filters. This gives high quality resampling however this the proportion changes slightly because the first and last pixel are considered to occupy only half a unit as they are considered as the border of the picture (pixel-centered coordinates)  
_TResampleFilter_ = (  
| List of resample filter to be used with _rmFineResample_  
| _rfBox_ ,  
| | Equivalent of simple stretch with high quality and pixel-centered coordinates  
| _rfLinear_ ,  
| | Linear interpolation giving slow transition between pixels  
| _rfHalfCosine_ ,  
| | Mix of _rfLinear_ and _rfCosine_ giving medium speed stransition between pixels  
| _rfCosine_ ,  
| | Cosine-like interpolation giving fast transition between pixels  
| _rfBicubic_ ,  
| | Simple bi-cubic filter (blurry)  
| _rfMitchell_ ,  
| | Mitchell filter, good for downsizing interpolation  
| _rfSpline_ ,  
| | Spline filter, good for upsizing interpolation, however slightly blurry  
| _rfLanczos2_ ,  
| | Lanczos with radius 2, blur is corrected  
| _rfLanczos3_ ,  
| | Lanczos with radius 3, high contrast  
| _rfLanczos4_ ,  
| | Lanczos with radius 4, high contrast  
| _rfBestQuality_);  
| | Best quality using rfMitchell or rfSpline  
| _ResampleFilterStr_ : **array**[TResampleFilter] **of** **string** =  
| | List of strings to represent resample filters  
| **function** StrToResampleFilter(str: **string**): TResampleFilter;  
| | Gives the sample filter represented by a string  
_TQuickImageInfo_ = **record**  
| Image information from superficial analysis  
| _Width_ ,  
| | Width in pixels  
| _Height_ ,  
| | Height in pixels  
| _ColorDepth_ ,  
| | Bitdepth for colors (1, 2, 4, 8 for images with palette/grayscale, 16, 24 or 48 if each channel is present)  
| _AlphaDepth_ : integer;  
| | Bitdepth for alpha (0 if no alpha channel, 1 if bit mask, 8 or 16 if alpha channel)  
_TBGRAImageReader_ = **class**(TFPCustomImageReader)  
| Bitmap reader with additional features  
| **function** GetQuickInfo(AStream: TStream): TQuickImageInfo; **virtual** ; **abstract** ;  
| | Return bitmap information (size, bit depth)  
| **function** GetBitmapDraft(AStream: TStream; AMaxWidth, AMaxHeight: integer; **out** AOriginalWidth,AOriginalHeight: integer): TBGRACustomBitmap; **virtual** ; **abstract** ;  
| | Return a draft of the bitmap, the ratio may change compared to the original width and height (useful to make thumbnails)  
_TBGRACustomWriterPNG_ = **class**(TFPCustomImageWriter)  
| Generic definition for a PNG writer with alpha option  
| **function** GetUseAlpha: boolean; **virtual** ; **abstract** ;  
| | Gets whether or not to use the alpha channel  
| **procedure** SetUseAlpha(AValue: boolean); **virtual** ; **abstract** ;  
| | Sets whether or not to use the alpha channel  
| **property** UseAlpha : boolean **read** GetUseAlpha **write** SetUseAlpha;  
| | Whether or not to use the alpha channel  
**operator** =(**const** AGuid1, AGuid2: TGuid): boolean;  
| Check whether to GUID are equal  
_TBGRAResourceManager_ = **class**  
| Generic class for embedded resource management  
| **var** BGRAResource : TBGRAResourceManager;  
| | Provides a resource manager   
**function** ResourceFile(AFilename: **string**): **string** ;  
| Return the full path for a resource file on the disk. On Windows and Linux, it can be next to the binary but on MacOS, it can be outside of the application bundle when debugging

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Miscellaneous_types](https://web.archive.org/web/20250515122733/https://wiki.freepascal.org/BGRABitmap_Miscellaneous_types)_
