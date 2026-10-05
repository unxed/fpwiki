# BGRABitmap XYZ

### Using XYZ colorspace in [BGRABitmap](<BGRABitmap.md> "BGRABitmap")

XYZ is a colorspace that stores the actual stimulation of the average eye, relative to a max luminance (Y=1) considered as white. Fluorescent colors or light sources may have greater values for X/Y/Z so in this case you would rather use TXYZABitmap floating-point format to avoid saturation. Otherwise, for most use cases, when the image represents an object reflecting daylight, TWordXYZABitmap is sufficient. 

The reference white is the color of an object that reflects all light, it depends on the illuminant. ICC recommends to use D50 as reference, which is a kind of natural daylight. While the reference white is always at Y = 1, its hue changes (X and Z values). So white is not necessary the simple value (1,1,1). This is only the case with the theoretical equal-energy illuminant (E). 

So if you are not working with a specific illuminant, you would rather avoid defining colors as XYZ values. If they are in the RGB range, you can use automatic conversion and provide color constants as usual. Otherwise a convenient way is to use L*a*b* colors. For example those colors are outside of the RGB scope: \- TLabA.New(60,-145,45) is a deep mint color (greenish cyan) \- TLabA.New(90,0,140) is a deep golden yellow 

While L*a*b* is convenient to specify colors, it is not so to merge colors and draw, so there isn't any bitmap that stores Lab values. Also it is worth noting that L*a*b* values do not have the exact same appearance depending on the illuminant. So I would rather use D50 to have the same values as ICC profiles. 

If you want to change the reference white anyway, you can use SetReferenceWhite function. ReferenceWhite2E, ReferenceWhite2D50 and ReferenceWhite2D65 are provided as global variables. 

It is also worth noting that in order to have the same values as most programs, the gamma value is now set to 2.2. This may give some gradients a different appearence, so you may want to switch back to the old 1.7 value (version 9 and before) or use your own with BGRASetGamma function. 

When saving an image in XYZ colorspace, usual formats like PNG or BMP will result in colors being clipped or distorted. You need to use TIF format so that it will be saved in Lab colorspace. This format though is limited to -128..127 values for a and b, so unfortunately not all colors can be saved. If someone has an idea for a format, please let me know. You can use the Serialize/Deserialize function, though this would be readable by most programs and you need to take into account the endianness. 

To specify how a color outside of RGB bounds is to be handled, global variables XYZToRGBOverflowMin/Max are provided. The following values are possible: 

  * xroClipToTarget: colors outside of target colorspace are converted to transparent
  * xroSaturateEachChannel: each color channel is saturated independently (hue may be lost). That's the default value
  * xroPreserveHue: hue is preserved by reducing intensity or saturation. The appearance is more realistic but it is slower.

---

_Source: [https://wiki.freepascal.org/BGRABitmap_XYZ](https://web.archive.org/web/20250418100113/https://wiki.freepascal.org/BGRABitmap_XYZ)_
