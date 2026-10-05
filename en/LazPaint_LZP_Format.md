# LazPaint LZP Format

## Contents

  * 1 LZP File format specification
    * 1.1 Global structure
    * 1.2 File header
      * 1.2.1 Compression
      * 1.2.2 Thumbnail
    * 1.3 Flat image
    * 1.4 Layer structure
      * 1.4.1 Layer entry
      * 1.4.2 Blend operations
      * 1.4.3 Computation of blend-over
    * 1.5 Directory structure



## LZP File format specification

In this document, [hexadecimal](<Hexadecimal.md> "Hexadecimal") numbers follow Pascal convention, i.e. they are prefixed with a [dollar sign](<Dollar_sign.md> "Dollar sign"). For example $10 means 16. As an exception, sequences of bytes are not prefixed. 

The default encoding for numbers is little endian. So the hexadecimal number $12345678 would be stored as the sequence of bytes 78 56 34 12. 

Numbers are 32-bit signed integers, unless stated otherwise. So -1 is stored like $FFFFFFFF. 

### Global structure

The file is composed of: 
    
    
    Offset  Content
    ------  --------------------
    0       File header
    ?       Thumbnail (optional)
    ?       [Flat image](<LazPaint_LZP_Format_Single_Image.md> "LazPaint LZP Format Single Image")
    ?       Layer structure (optional)
    

Note: some old formats do not have a header and start directly with the flat image. Image readers may discard those files. However for your information, in this case, images are always stored with ZStream compression and the size of the image is deduced from the size of the flat image. 

### File header
    
    
    Offset  Size  Name                Content
    ------  ----  ------------------  ------------------------
    0       8     _magic_               Always contains the string 'LazPaint'
    8       4     _zero1_               Contains the number 0
    12      4     _headerSize_          Size of this header
    
    16      4     _width_               Width of the image canvas
    20      4     _height_              Height of the image canvas
    24      4     _nbLayers_            Number of layers
    28      4     _previewOffset_       Offset (from the beginning of the file) of the [flattened image](<LazPaint_LZP_Format_Single_Image.md> "LazPaint LZP Format Single Image")
    
    32      4     _zero2_               Contains the number 0
    36      4     _compressionMode_     Contains flags indicating the compression and the presence of a thumbnail
    40      4     _reserved1_           Reserved for future use. Set to 0.
    44      4     _layersOffset_        Offset (from the beginning of the file) of the layer structure
    

_headerSize_ contains the size of this header. An image reader must read this to know which fields are available. It must be greater or equal to 48. 

_width_ and _height_ are the dimension of the canvas. There is no real limitation for these numbers however it is recommended to keep it under or equal to 8192. 

_nbLayers_ is the number of layers. The minimum is 1. There is no real limitation however it is recommended to keep it under or equal to 99. 

_layersOffset_ can be 0, in which case the image is flat and that there is no layer description. 

#### Compression

_compressionMode_ contains flags indicating the compression and the presence of a thumbnail. 

The low order byte indicates the compression of the flattened image. It is the value (_compressionMode_ **and** $ff): 

\- $01 indicates ZStream compression 

\- $02 indicates RLE compression 

\- any other value should be considered as an indication that the image reader cannot interpret the content of the file 

The following flags can be defined: 

\- $100 indicates that there is thumbnail in PNG format; check with (_compressionMode_ **and** $100) <> 0 

If there is a thumbnail, it is directly after the header. The reader should read _headerSize_ to have the correct location. 

The compression of the layers is defined in the layer structure. 

#### Thumbnail

Its presence is recommended however optional. Its presence is specified by the flag $100 of _compressionMode_. 

Currently, it is a PNG file embedded directly after the header. It is provided as a way to facilitate quick preview. 

PNG format is widespread so it is easy to implement a browser that would display the thumbnail. To do so, check that the header contains _magic_ and that the flag for the thumbnail is set in _compressionMode_. Then jump to the end of the header using _headerSize_. Then read the PNG file. 

There is no real limitation but the recommended size for the thumbnail is 128 along the largest dimension. The other dimension may be smaller if the image is not a square. Of course if the image is smaller than 128 then the thumbnail will be smaller than 128. 

### Flat image

The file always include a flattened image to make it easier for image viewers. It is in the [LZP single image format](<LazPaint_LZP_Format_Single_Image.md> "LazPaint LZP Format Single Image"). 

If there is no layer structure, the caption of this image is the name of the unique layer. If there is a layer structure, then the caption of this image is discarded. 

### Layer structure
    
    
    Offset  Size  Content
    ------  ----  -------------------------------------------------------
    0       20    Contains the string 'TBGRALayeredBitmap' followed by the bytes 1A 00
    20      4     Size S of the additional header after this field (must be at least 12)
    24      4     Number of layers N (should be identical to the number in the file header)
    28      4     Selected layer index. Equals to -1 if no layer is selected, otherwise positive from 0 to N-1
    32      4     Stack flags
    36      4     Canvas width (optional)
    40      4     Canvas height (optional)
    44      8     64-bit offset to the directory structure relative to the start of this header (optional)
    24+S          First layer (bottom one)
    

The following flags are defined: 

  * $01: the layers are blended without gamma correction
  * $02: the layers are compressed with RLE compression (otherwise they are compressed with ZStream compression)



If the canvas size is not supplied (when the total header size is less than 44), then the canvas size is the size of the first non-empty layer bitmap. 

Similarly, the [directory structure](<LazPaint_LZP_directory_structure.md> "LazPaint LZP directory structure") offset may not to be supplied or set to zero. In this case, there is none. It is optional and only necessary for vector shapes. 

The total header size of the layer structure is equal to 24 + S. Then for each layer there is a layer entry. 

The first layer entry is the bottom most layer. Then each layer is blended over the previous ones. The last layer is on top and is not hidden by any other layer. 

#### Layer entry

Header of the layer entry: 
    
    
    Offset  Size  Content
    ------  ----  -------------------------------------------------------
    0       4     Size S1 of the optional fields that follows
    4       4     Layer flags (lowest bit means that it is visible)
    8       4     Layer blend operation
    12      4     Layer horizontal offset in pixels
    16      4     Layer vertical offset in pixels
    20      4     Layer ID (if used, each layer must have a different ID in a given file)
    24      4     Layer opacity from 0 (transparent) to 65535 (opaque)
    28      4     Lower bits of size S2 of image data
    32      4     Upper bits of size S2 (optional, forming 64-bit integer)
    36      16    GUID of the original used to render the layer (optional)
    52      24    Affine matrix of the transform applied to the original
    4+S1    S2    [Image data](<LazPaint_LZP_Format_Single_Image.md> "LazPaint LZP Format Single Image")
    4+S1+S2       Next entry
    

All fields are optional so in theory, S1 can be set to zero and the image data can directly follow. However it is recommended to write the whole header. 

The default values are: visible, blend operation boTransparent, offset (0,0), undefined ID (maximum integer value), opaque, size S2 = 0, GUID = NULL. If an original is specified the affine matrix transform is mandatory. 

The size of the image data is a 64-bit integer, though the upper value is optional and by default zero. When the image size S2 <> 0, the offset to the next entry is computed using the formula 4+S1+S2. Otherwise, the data is expected to continue after the end of the image data. 

The GUID is stored here as binary (in little endian): one DWord, two Words and 8 bytes. 

The affine transform is stored as binary (in little endian): 6 single precision floats ux, uy, vx, vy, tx, ty (axes u and v and translation t). 

#### Blend operations

  * Under is the value of the channels of the layer underneath
  * Over is the value of the channels of the layer above
  * Mix is an intermediate value that is then blended over the previous layers, and that, unless specified, is computed for each RGB channel. The alpha channel is computed differently (see below table).
  * Final is the resulting value



Note: 

  * values (Red, Green, Blue, Alpha, Mix, Under, Over, Final) are considered to be between 0 and 255
  * GE means gamma expansion. GE(x) = Power(x/255, 2.1)*65535
  * GC means gamma compression. GC(x) = Power(x/65535, 1/2.1)*255
  * ByteSqrt means the square root. ByteSqrt(x) = Power(X/255, 0.5)*255
  * **div** stands for integer division (rounded down)
  * HSL is the standard HSL colorspace, without gamma correction
  * GSB is the corrected HSL colorspace of LazPaint. It is computed with gamma correction and the B (brightness) component correspond to the perceived lightness:


    
    
    B = (306*GE(Red) + 601*GE(Green) + 117*GE(Blue))/1024
    B between 0 and 65535
    

Here is the list of operations supported in LazPaint: 
    
    
    Value  Name               Description
    -----  -----------------  ----------------------------------------------------
    0      LinearBlend        Blend over the previous layers without gamma correction
    1      Transparent        Blend over the previous layers 
                              (with gamma correction unless it is specified otherwise in the stack flags)
    
    2      Lighten            Mix := Max(Over, Under)
    3      Screen             Mix := 255 - ((255 - Under) * (255 - Over) **shr** 8)
    4      Additive           Mix := GC(GE(Under)+GE(Over))
    5      LinearAdd          Mix := Under + Over
    6      ColorDodge         Mix := **if** Over = 255 **then** 255
                                     **else** Min(255, (Under **shl** 8) **div** (255 - Over))
    7      Divide             Mix := **if** Over = 0 **then** 255
                                     **else** Min(255, (Under **shl** 8) **div** Over)
    8      NiceGlow           Mix := **if** Under = 255 **then** 255
                                     **else** Min(255, Over*Over **div** (255 - Under))
    9      SoftLight          Mix := ((255 - Under)*Over shr 7 + Under)*Under div 255
    10     HardLight          Mix := **if** Over <= 128 **then** Under*Over **shr** 7
                                     **else** 255 - ((255 - Under)*(255 - Over) **shr** 7)
    
    11     Glow               Mix := **if** Under = 255 **then** 255
                                     **else** Min(255, Over*Over **div** (255 - Under))
    12     Reflect            Mix := **if** Over = 255 **then** 255
                                     **else** Min(255, Under*Under **div** (255 - Over))
    13     Overlay            Mix := **if** Under <= 128 **then** Under*Over **shr** 7
                                     **else** 255 - ((255 - Under)*(255 - Over) **shr** 7)
    14     DarkOverlay        Mix := **if** GE(Under) <= 32768 **then** Under*Over **shr** 7
                                     **else** GC(65535 - ((65535 - GE(Under))*(65535 - GE(Over)) **shr** 15)
    15     Darken             Mix := Min(Under, Over)
    16     Multiply           Mix := **if** Over >= 128 **then** Under*(Over+1) **shr** 8 **else** Under*Over **shr** 8
    17     ColorBurn          Mix := **if** Over = 0 **then** 0
                                     **else** Max(0, 255 - (Under **shl** 8) **div** Over)
    
    18     Difference         Mix := GC(Abs(GE(Under) - GE(Over)))
    19     LinearDifference   Mix := Abs(Under - Over)
    20     Exclusion          Mix := GC(GE(Under) + GE(Over) - (GE(Under)*GE(Over) **shr** 15)
    21     LinearExclusion    Mix := Under + Over - (Under*Over **shr** 7)
    22     Subtract           Mix := GC(Max(0, GE(Under) - GE(Over)))
    23     LinearSubtract     Mix := Max(0, Under - Over)
    24     SubtractInverse    Mix := GC(Max(0, GE(Under) - (65535 - GE(Over))))
    25     LinearSubtractInv  Mix := Max(0, Under - (255 - Over))
    
    26     Negation           Mix := **if** GE(Under) + GE(Over) <= 65535 **then** GC(GE(Under) + GE(Over))
                                     **else** GC(131071 - (GE(Under) + GE(Over)))
    27     LinearNegation     Mix := **if** Under + Over <= 255 **then** Under + Over
                                     **else** 511 - (Under + Over)
    28     Xor                Mix := Under **xor** Over
    
    29     SvgSoftLight       Mix := **if** Over <= 128 **then** Under - (((256 - 2*Over)* Under **shr** 8)*(255 - Under) **shr** 8)
                                     **else** **if** Under <= 64 **then** Under + ((2*Over - 256) * (Under*7 - (4*Under*(4*Under + 256)*(256 - Under) **shr** 16)) **shr** 8)
                                     **else** Under + ((2*Over - 255) * (ByteSqrt(Under)-Under) **shr** 8)
    30     Mask               ApplyOpacity := ((2*Over.Red + 4*Over.Green + Over.Blue)*Over.Alpha + 255*7*(255-Over.Alpha)) / (255*7)
    31     LinearMultiplySat  L := (min(Under.Red, Under.Green, Under.Blue) + max(Under.Red, Under.Green, Under.Blue)) / 2
                              Mix := **if** Over = 0 **then** L
                                     **else** (Under - L)*Over/255 + L
    32     LinearHue          Mix := HSL(Over.H, Under.S, Under.L)
    33     LinearColor        Mix := HSL(Over.H, Over.S, Under.L)
    34     LinearLightness    Mix := HSL(Under.H, Under.S, Over.L)
    35     LinearSaturation   Mix := HSL(Under.H, **if** Under.S <> 0 **then** Over.S **else** 0, Under.L)
    
    
    
    36     CorrectedHue       Mix := GSB(Over.H, Under.S, Under.L)
    37     CorrectedColor     Mix := GSB(Over.H, Over.S, Under.L)
    38     CorrectedLightness Mix := GSB(Under.H, Under.S, Over.L)
    39     CorrectedSat       Mix := GSB(Under.H, **if** Under.S <> 0 **then** Over.S **else** 0, Under.L)
    

For the mask, the color channels are unchanged and the ApplyOpacity value is used to multiply the destination alpha value: 
    
    
    Final.Alpha := Under.Alpha * ApplyOpacity / 255
    

For other blend modes, first the RGB channels are modified according to the underlying alpha value: 
    
    
    Mix := (Mix * Under.Alpha + Over * (255 - Under.Alpha)) / 255
    Mix.Alpha := Over.Alpha
    

In other words, if the layer underneath is transparent, the merged color is simply the color of the layer above. 

Then the Mix color is drawn over the underlying layer. If Mix.Alpha = 255 then the color is replace: 
    
    
     Final = Mix
    

If Mix.Alpha = 0 then the color is unchanged: 
    
    
     Final = Under
    

Otherwise, the color is blended using the regular drawing function (with or without gamma correction depending on the stack option). 

#### Computation of blend-over

Linear blend-over: 
    
    
    **var** 
      a1f, a2f, a12, a12m: DWord; //unsigned
    **begin**
      a12  := 65025 - (255 - UnderAlpha) * (255 - OverAlpha);
      a1f := UnderAlpha * (255 - OverAlpha);
      a2f := OverAlpha * 255;
      Under := (Under * a1f + Over * a2f + (a12 **shr** 1)) **div** a12; //for each channel
      UnderAlpha := (a12 + a12 **shr** 7) **shr** 8;   
    **end** ;
    

Blend-over with gamma correction: 
    
    
    **var** 
      a1f, a2f, a12, a12m: DWord; //unsigned
    **begin**
      a12  := 65025 - (255 - UnderAlpha) * (255 - OverAlpha);
      a1f := UnderAlpha * (255 - OverAlpha);
      a2f := OverAlpha * 255;
      Under := GC( (GE(Under) * a1f + GE(Over) * a2f + (a12 **shr** 1)) **div** a12 ); //for each channel
      UnderAlpha := (a12 + a12 **shr** 7) **shr** 8;   
    **end** ;
    

### Directory structure

The [directory structure](<LazPaint_LZP_directory_structure.md> "LazPaint LZP directory structure") is optional. It can contain additional information about the layered image, in particular originals used to render the layers. A reader that does not handle vectorial information can discard it. Its offset is determined by the layer stack header.

---

_Source: [https://wiki.freepascal.org/LazPaint_LZP_Format](https://web.archive.org/web/20230322191156/https://wiki.freepascal.org/LazPaint_LZP_Format)_
