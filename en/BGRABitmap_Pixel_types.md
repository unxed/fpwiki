# BGRABitmap Pixel types

List of pixel types and functions of [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. They are provided by _BGRABitmapTypes_ unit. 

### Pixel types and functions

_PBGRAPixel_ = ^TBGRAPixel;  
---  
| Pointer for direct pixel access. Data is stored as a sequence of _TBGRAPixel_. See [BGRABitmap tutorial 4](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")  
_TBGRAPixel_ = **packed** **record** blue, green, red, alpha: byte;  
| Each pixel is a sequence of 4 bytes containing blue, green, red and alpha channel. Values range from 0 to 255, color is in sRGB colorspace. The alpha value of 0 is transparent and 255 is opaque. In the bitmap data, when the pixel is fully transparent, the RGB values are supposed to be set to zero.  
| **function** BGRA(red, green, blue, alpha: byte): TBGRAPixel; **overload** ; **inline** ;  
| | Creates a pixel with given RGBA values  
| **function** BGRA(red, green, blue: byte): TBGRAPixel; **overload** ; **inline** ;  
| | Creates a opaque pixel with given RGB values  
| **operator** = (**const** c1, c2: TBGRAPixel): boolean; **inline** ;  
| | Checks if two pixels are equal. If they are both transparent, RGB values are ignored  
| **function** GetIntensity(c: TBGRAPixel): word; **inline** ;  
| | Returns the intensity of a pixel. The intensity is the maximum value reached by any component  
| **function** SetIntensity(c: TBGRAPixel; intensity: word): TBGRAPixel;  
| | Sets the intensity of a pixel  
| **function** GetLightness(c: TBGRAPixel): word;  
| | Returns the lightness of a pixel. The lightness is the perceived brightness, 0 being black and 65535 being white  
| **function** SetLightness(c: TBGRAPixel; lightness: word): TBGRAPixel;  
| | Sets the lightness of a pixel  
| **function** ApplyLightnessFast(color: TBGRAPixel; lightness: word): TBGRAPixel; **inline** ;  
| | Sets the lightness quickly, by fading towards black if _lightness_ is less than 32768, and fading towards white if _lightness_ is more than 32768  
| **function** ApplyIntensityFast(color: TBGRAPixel; lightness: longword): TBGRAPixel;  
| | Sets the intensity quickly, by fading towards black if _lightness_ is less than 32768, and multiplying all components if _lightness_ is more than 32768. In case of saturation, it fades towards white  
| **function** CombineLightness(lightness1,lightness2: Int32or64): Int32or64;  
| | Combines two lightnesses together. A value of 32768 is neutral. The result may exceed 65535  
| **function** BGRAToGrayscale(c: TBGRAPixel): TBGRAPixel;  
| | Converts a color into grayscale  
| **function** GrayscaleToBGRA(lightness: word): TBGRAPixel;  
| | Create a gray color with the given _lightness_  
| **function** MergeBGRA(c1, c2: TBGRAPixel): TBGRAPixel; **overload** ;  
| | Merge two colors without gamma correction  
| **function** MergeBGRA(c1: TBGRAPixel; weight1: integer; c2: TBGRAPixel; weight2: integer): TBGRAPixel; **overload** ;  
| | Merge two colors without gamma correction. _weight1_ and _weight2_ indicates the weight of the color barycentre  
| **function** MergeBGRAWithGammaCorrection(c1: TBGRAPixel; weight1: byte; c2: TBGRAPixel; weight2: byte): TBGRAPixel;  
| | Merge two colors with gamma correction. _weight1_ and _weight2_ indicates the weight of the color barycentre  
| **function** ColorToBGRA(color: TColor): TBGRAPixel; **overload** ;  
| | Converts a _TColor_ value into an opaque pixel  
| **function** ColorToBGRA(color: TColor; opacity: byte): TBGRAPixel; **overload** ;  
| | Converts a _TColor_ value into a pixel with given _opacity_  
| **function** BGRAToColor(c: TBGRAPixel): TColor;  
| | Converts a pixel into a TColor value, discarding the alpha value  
| **function** FPColorToBGRA(AValue: TFPColor): TBGRAPixel;  
| | Converts a _TFPColor_ value into a pixel. Note that even if _TFPColor_ have 16-bit values, they are not considered as gamma expanded  
| **function** BGRAToFPColor(AValue: TBGRAPixel): TFPColor; **inline** ;  
| | Converts a pixel into a _TFPColor_  
| **function** BGRAWordDiff(c1, c2: TBGRAPixel): word;  
| | Computes the difference (with gamma correction) between two pixels, taking into account all dimensions, including transparency. The result ranges from 0 to 65535  
| **function** BGRADiff(c1, c2: TBGRAPixel): byte;  
| | Computes the difference (with gamma correction) between two pixels, taking into account all dimensions, including transparency. The result ranges from 0 to 255  
_ArrayOfTBGRAPixel_ = **array** **of** TBGRAPixel;  
| Array of pixels  
| **function** MergeBGRA(**const** colors: **array** **of** TBGRAPixel): TBGRAPixel; **overload** ;  
| | Merge given colors without gamma correction  
_TExpandedPixel_ = **packed** **record** red, green, blue, alpha: word;  
| Stores a gamma expanded RGB color. Values range from 0 to 65535  
| **function** GammaExpansion(c: TBGRAPixel): TExpandedPixel; **inline** ;  
| | Converts a pixel from sRGB to gamma expanded RGB  
| **function** GammaCompression(**const** ec: TExpandedPixel): TBGRAPixel; **inline** ;  
| | Converts a pixel from gamma expanded RGB to sRGB  
| **function** GammaCompression(red,green,blue,alpha: word): TBGRAPixel; **inline** ;  
| | Converts a pixel from gamma expanded RGB to sRGB  
| **function** GetIntensity(**const** c: TExpandedPixel): word; **inline** ;  
| | Returns the intensity of an gamma-expanded pixel. The intensity is the maximum value reached by any component  
| **function** SetIntensity(**const** c: TExpandedPixel; intensity: word): TExpandedPixel;  
| | Sets the intensity of a gamma-expanded pixel  
| **function** GetLightness(**const** c: TExpandedPixel): word; **inline** ;  
| | Returns the lightness of an gamma-expanded pixel. The lightness is the perceived brightness, 0 being black and 65535 being white  
| **function** SetLightness(**const** c: TExpandedPixel; lightness: word): TExpandedPixel;  
| | Sets the lightness of a gamma-expanded pixel  
| **function** SetLightness(**const** c: TExpandedPixel; lightness: word; curLightness: word): TExpandedPixel;  
| | Sets the lightness of a gamma expanded pixel, provided you already know the current value of lightness _curLightness_. It is a bit faster than the previous function  
| **function** ColorImportance(ec: TExpandedPixel): word;  
| | Returns the importance of the color. It is similar to saturation in HSL colorspace, except it is gamma corrected. A value of zero indicates a black/gray/white, and a value of 65535 indicates a bright color  
| **function** MergeBGRA(ec1, ec2: TExpandedPixel): TExpandedPixel; **overload** ;  
| | Merge two gamma expanded pixels (so taking into account gamma correction)  
| **function** ExpandedDiff(ec1, ec2: TExpandedPixel): word;  
| | Computes the difference (with gamma correction) between two pixels, taking into account all dimensions, including transparency. The result ranges from 0 to 65535  
_THSLAPixel_ = **packed** **record**  
| Pixel color defined in HSL colorspace. Values range from 0 to 65535  
| _hue_ : word;  
| | Hue of the pixel. Extremum values 0 and 65535 are red  
| _saturation_ : word;  
| | Saturation of the color. 0 is gray and 65535 is the brightest color  
| _lightness_ : word;  
| | Lightness of the color. 0 is black, 32768 is normal, and 65535 is white  
| _alpha_ : word;  
| | Opacity of the pixel. 0 is transparent and 65535 is opaque  
| **function** HSLA(hue, saturation, lightness, alpha: word): THSLAPixel; **overload** ; **inline** ;  
| | Creates a pixel with given HSLA values, where A stands for alpha  
| **function** HSLA(hue, saturation, lightness: word): THSLAPixel; **overload** ; **inline** ;  
| | Creates an opaque pixel with given HSL values  
| **function** BGRAToHSLA(c: TBGRAPixel): THSLAPixel;  
| | Converts a pixel from sRGB to HSL color space  
| **function** ExpandedToHSLA(**const** ec: TExpandedPixel): THSLAPixel;  
| | Converts a pixel from gamma expanded RGB to HSL color space  
| **function** HSLAToExpanded(**const** c: THSLAPixel): TExpandedPixel;  
| | Converts a pixel from HSL colorspace to gamma expanded RGB  
| **function** HSLAToBGRA(**const** c: THSLAPixel): TBGRAPixel;  
| | Converts a pixel from HSL colorspace to sRGB  
| **function** HueDiff(h1, h2: word): word;  
| | Computes the hue difference  
| **function** GetHue(ec: TExpandedPixel): word;  
| | Returns the hue of a gamma expanded pixel  
_TGSBAPixel_ = THSLAPixel;  
| Pixel color defined in corrected HSL colorspace. G stands for corrected hue and B stands for actual brightness. Values range from 0 to 65535  
| **function** BGRAToGSBA(c: TBGRAPixel): TGSBAPixel;  
| | Converts a pixel from sRGB to correct HSL color space  
| **function** ExpandedToGSBA(ec: TExpandedPixel): TGSBAPixel;  
| | Converts a pixel from gamma expanded RGB to correct HSL color space  
| **function** GtoH(ghue: word): word;  
| | Converts a G hue (GSBA) to a H hue (HSLA)  
| **function** HtoG(hue: word): word;  
| | Converts a H hue (HSLA) to a G hue (GSBA)  
| **function** GSBAToBGRA(c: TGSBAPixel): TBGRAPixel;  
| | Converts a pixel from corrected HSL to sRGB  
| **function** GSBAToExpanded(c: TGSBAPixel): TExpandedPixel;  
| | Converts a pixel from correct HSL to gamma expanded RGB  
| **function** GSBAToHSLA(c: TGSBAPixel): THSLAPixel;  
| | Converts a pixel from correct HSL to usual HSL  
_TColorF_ = **packed** **array**[1..4] **of** single;  
| General purpose color variable with single-precision floating point values  
| **function** ColorF(red,green,blue,alpha: single): TColorF;  
| | Creates a TColorF structure  
| **operator** \- (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Subtract each component separately  
| **operator** \+ (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Add each component separately  
| **operator** * (**const** c1, c2: TColorF): TColorF; **inline** ;  
| | Multiply each component separately  
| **operator** * (**const** c1: TColorF; factor: single): TColorF; **inline** ;  
| | Multiply each component by _factor_  
_TDrawMode_ = (  
| Possible modes when drawing a pixel over another one  
| _dmSet_ ,  
| | The pixel is replaced  
| _dmSetExceptTransparent_ ,  
| | The pixel is replaced if the pixel over has an alpha value of 255  
| _dmLinearBlend_ ,  
| | The pixel is blend over the other one according to alpha values, however no gamma correction is applied. In other words, the color space is assumed to be linear  
| _dmDrawWithTransparency_ ,  
| | The pixel is blend over the other one according to alpha values, and a gamma correction is applied. In other word, the color space is assumed to be sRGB  
| _dmXor_);  
| | Values of all channels are combined with Xor. This is useful to compute the binary difference, however it is not something that makes much sense to display on the screen  
| _dmFastBlend_ = dmLinearBlend;  
| | An alias for the linear blend, because it is faster than blending with gamma correction  
_TBlendOperation_ = (  
| Advanced blending modes. See [Paint.NET blend modes](<http://www.brighthub.com/multimedia/photography/articles/18301.aspx>) and [Formulas](<http://www.pegtop.net/delphi/articles/blendmodes/>). Blending layers has two steps. The first one is to apply the blend operations listed below, and the second is the actual merging of the colors  
| _boLinearBlend_ ,  
| | Simple blend, except that it forces a linear merge so it is equivalent to _dmLinearBlend_  
| _boTransparent_ ,  
| | Simple blend. It is equivalent to _dmLinearBlend_ or _dmDrawWithTransparency_  
| _boLighten_ , boScreen, boAdditive, boLinearAdd, boColorDodge, boDivide, boNiceGlow, boSoftLight, boHardLight,  
| | Lighting blend modes (tends to increase the luminosity)  
| _boGlow_ , boReflect, boOverlay, boDarkOverlay, boDarken, boMultiply, boColorBurn,  
| | Masking blend modes (tends to decrease the luminosity)  
| _boDifference_ , boLinearDifference, boExclusion, boLinearExclusion, boSubtract, boLinearSubtract, boSubtractInverse, boLinearSubtractInverse,  
| | Difference blend modes  
| _boNegation_ , boLinearNegation,  
| | Negation blend modes  
| _boXor_);  
| | Xor blend mode. It is sightly different from _dmXor_ because the alpha value is used like in other blends modes  
| _boGlowMask_ = boGlow;  
| | Alias to glow that express that this blend mode masks the part where the top layer is black  
| _boLinearMultiply_ = boMultiply;  
| | Alias because linear or non linear multiply modes are identical  
| _boNonLinearOverlay_ = boDarkOverlay;  
| | Alias to express that dark overlay is simply an overlay with gamma correction  
| _BlendOperationStr_ : **array**[TBlendOperation] **of** **string**  
| | String constants for blend modes  
| **function** StrToBlendOperation(str: **string**): TBlendOperation;  
| | Returns the blend mode expressed by the string  
_TChannel_ = (cRed, cGreen, cBlue, cAlpha);  
| Possible channels in a bitmap using any RGBA colorspace  
| _TChannels_ = set **of** TChannel;  
| | Combination of channels  
_TAlphaChannelPaletteOption_ = (  
| Specifies how a palette handles the alpha channel  
| _acIgnore_ ,  
| | The alpha channel is ignored. The alpha channel is considered to be stored elsewhere  
| _acTransparentEntry_ ,  
| | One entry is allocated the fully transparent color  
| _acFullChannelInPalette_);  
| | The alpha channel is fully embedded in the palette so that a color is identified by its four RGBA channels  
_TDitheringAlgorithm_ = (  
| Dithering algorithms that specifies how to handle colors that are not found in the palette  
| _daNearestNeighbor_ ,  
| | The nearest color is to be used instead  
| _daFloydSteinberg_);  
| | The nearest color may be used however another color may be used to compensate for the error, following Floyd-Steinberg algorithm

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Pixel_types](https://web.archive.org/web/20170425222443/https://wiki.freepascal.org/BGRABitmap_Pixel_types)_
