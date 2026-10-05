# LazPaint LZP Format Single Image

This description applies for both the flat image and for the images in the layer structure of [LZP file format](<LazPaint_LZP_Format.md> "LazPaint LZP Format"). 

There are two possible compressions. Both are lossless i.e. there is no loss of quality. 

If a file is saved in this format without any header, it uses ZStream compression. But in general LazPaint uses RLE compression here. 

## Contents

  * 1 ZStream
  * 2 RLE
    * 2.1 Full RLE structure for empty images
    * 2.2 Full RLE structure for non paletted images
    * 2.3 Full RLE structure for paletted images
    * 2.4 RLE stream
    * 2.5 Simple instructions examples
    * 2.6 Packed repetitions
    * 2.7 Packed dumps



#### ZStream

The image has a defined width and height. There is a rectangular area for its content. Pixels that are outside of this area are transparent (all channels equal to zero). 

The content is compressed in any number of chunks. The uncompressed data must then be concatenated regardless of where scanlines start or end. 

The header of the ZStream image is as follows: 
    
    
    Offset  Size  Content
    ------  ----  ----------------------------
    0       4     Width
    4       4     Height
    8       4     Length of the caption (N)
    12      N     Caption (UTF8 encoded)
    12+N    4     Left bound of the content
    16+N    4     Top bound of the content
    20+N    4     Right bound of the content
    24+N    4     Bottom bound of the content
    28+N    4     Line order (0: top to bottom, 1: bottom to top)
    32+N    4     Number of chunks
    

The content contains the lines in the rectangular area, following the specified line order. Each line contains all pixels in BGRA 32-bit format, i.e. 1 byte for each channel and the order of the channels being blue, green, red, alpha. 

The chunks follow directly the header. Here is the structure for one chunk: 
    
    
    Offset  Size  Content
    ------  ----  ----------------------------
    0       4     Size of the compressed data in the chunk (M)
    4       M     Compressed data using ZStream (without ZStream header)
    

#### RLE

The header of the RLE image is as follows: 
    
    
    Offset  Size  Content
    ------  ----  ----------------------------
    0       4     Width
    4       4     Height
    8       4     Length of the caption (N)
    12      N     Caption (UTF8 encoded)
    12+N    1     Plane flags
    

The following plane flags are defined: 

\- $01: the green channel is a copy of the red channel 

\- $02: the blue channel is a copy of the red channel 

\- $04: the blue channel is a copy of the green channel 

\- $08: there is no alpha channel (the image is opaque) 

\- $10: there is a palette and colors are expressed as indices in this palette 

There is then a variable number of RLE streams. The pixels are scanned from top to bottom and from left to right. 

##### Full RLE structure for empty images

If the image is completely transparent, there are no stream for color channels. 
    
    
    Offset  Size  Content
    ------  ----  -------------------------
    0       13+N  Header
    13+N    ?     Alpha channel stream
    

The uncompressed stream for the alpha channel contains one byte for each pixel of the canvas. 0 means the pixel is transparent and 255 means opaque. Because the image is empty, it contains only zeros. 

##### Full RLE structure for non paletted images
    
    
    Offset  Size  Content
    ------  ----  -------------------------
    0       13+N  Header
    13+N    ?     Alpha channel stream (optional)
    ?       ?     Red channel stream
    ?       ?     Green channel stream (optional)
    ?       ?     Blue channel stream (optional)
    

Alpha channel is not present if $08 flag is used. Otherwise the uncompressed stream contains one byte for each pixel of the canvas. 0 means the pixel is transparent and 255 means opaque. 

The uncompressed stream for the red channel contains one byte for each **non-transparent** pixel. 

Green channel is not present if $01 flag is used. Otherwise the uncompressed stream contains one byte for each **non-transparent** pixel. 

Blue channel is not present if $02 or $04 flag is used. Otherwise the uncompressed stream contains one byte for each **non-transparent** pixel. 

##### Full RLE structure for paletted images
    
    
    Offset  Size  Content
    ------  ----  -------------------------
    0       13+N  Header
    13+N    ?     Alpha channel stream (optional)
    ?       ?     Red channel palette stream
    ?       ?     Green channel palette stream (optional)
    ?       ?     Blue channel palette stream (optional)
    ?       ?     Indexed color stream
    

Alpha channel is not present if $08 flag is used. Otherwise the uncompressed stream contains one byte for each pixel of the canvas. 0 means the pixel is transparent and 255 means opaque. 

Each channel palette stream contains a maximum of 256 uncompressed bytes. The streams may be shorter and in that case, the rest is padded with zeros. The position in the uncompressed stream is the color index. 

The uncompressed stream for the red channel contains one byte for each entry of the palette. It is padded with zeros to make 256 bytes. 

Green channel palette is not present if $01 flag is used. Otherwise the stream contains the green values for the palette. 

Blue channel palette is not present if $02 or $04 flag is used. Otherwise the stream contains the blue values for the palette. 

The uncompressed indexed color stream contains one byte for each **non-transparent** pixel, that is the index into the palette. 

##### RLE stream

The structure of one RLE stream is as follows: 
    
    
    Offset  Size  Content
    ------  ----  ----------------------------
    0       4     Size of the compressed data (P)
    4       P     Compressed data
    

The data is a series of unpacking instructions. Each element of the instruction is a one byte value. 

Special instructions: 
    
    
    Sequence                    Meaning
    --------------------------  ---------------------------------------------------------------
    $C1                         Reserved (the reader must stop and say the format is not supported)
    $C2                         Reserved (the reader must stop and say the format is not supported)
    $E0                         Ends of stream
    $E1                         Reserved (the reader must stop and say the format is not supported)
    $FF                         The reader must ignore and continue to read
    

Big repetitions instructions: 
    
    
    Sequence                    Meaning
    --------------------------  ---------------------------------------------------------------
    $00 _Hi_ _Lo_ _Val_               Repeats (_Hi_ *256 + _Lo_) times the value _Val_
    $80 _Val_                     Repeats the value _Val_ the same number of times that in the previous $00 instruction
    $40 _Count_ _Val_               Repeats (_Count_ +64) times the value _Val_
    $C0 _Val_                     Repeats the value _Val_ the same number of times that in the previous $40 instruction
    

Small repetitions: 
    
    
    Sequence                    Meaning
    --------------------------  ---------------------------------------------------------------
    $01..$3f _Val_                Repeats _Val_ , the number of times being the value of the instruction ($01..$3f)
    $40+n (_n_ = 1..31)           Uncompress _n_ group of packed repetition (see below)
    $60+_n_ (_n_ = 0..15)           Repeats (_n_ +1) times the value 0
    $70+_n_ (_n_ = 0..15)           Repeats (_n_ +1) times the value 255
    

Dump: 
    
    
    Sequence                    Meaning
    --------------------------  ---------------------------------------------------------------
    $81 _Count_                   The next (_Count_ +64) bytes are copied as they are
    $80+_n_ (_n_ = 2..63)           The next _n_ bytes are copied as they are
    $C0+_n_ _Val_ (_n_ = 3..31)       The next _(n-1)/2_ bytes must be unpacked to yield _n_ uncompressed bytes (see below)
                                An initial value _Val_ is provided and is put as it is in the uncompressed stream.
    $E0+_n_ (_n_ = 2..30)           The next _n/2_ bytes must be unpacked to yield _n_ uncompressed bytes (see below)
                                The last unpacked value with instruction $C0 and $E0 is used as an implicit initial value.
                                This initial value is however not put in the uncompressed stream. Its default value is $80
                                in the case there is no previous unpacking instruction.
    

##### Simple instructions examples
    
    
    07 FF 06 FE
    

repeats 7 times the value $FF then 6 times the value $FE. 

  

    
    
    63 73
    

repeat 4 times the value $00 then 4 times the value $FF. 

  

    
    
    00 01 00 23 E0
    

Stream containing $100 times the value $23. The $E0 instruction indicates the end of the stream. 

  

    
    
    00 01 00 C0 80 FF 80 C0 80 FF
    

Stream containing: 
    
    
    - $100 times the value $C0
    - $100 times the value $FF
    - $100 times the value $C0
    - $100 times the value $FF
    - the end of the stream instruction is missing
    

  

    
    
    40 03 FF C0 00
    

repeats 67 times the value $FF, then 67 times the value $00. 

  

    
    
    83 01 02 03
    

writes 01 02 03 into the output stream (a total of 3 bytes). 

  

    
    
    81 03 01 02 03 04 ... 40 41 42 43
    

writes 01 02 03 04 ... 40 41 42 43 into the output stream (a total of 67 bytes). 

##### Packed repetitions

The instructions $41..$5F (= $40 + _n_) represent a group of _n_ packed repetition. So the structure of one instruction is: 
    
    
    Instruction $40+_n_ 
    Packed repetition number 1
    Packed repetition number 2
    ...
    Packed repetition number _n_
    

One entry of packed repetition: 
    
    
    Offset  Size  Content
    -----   ----  -----------------
    0       1     Packed number P of repetitions to apply
    1       1     First byte to repeat N1 times
    2       1     2nd byte to repeat N2 times
    3       1     3rd byte to repeat N3 times
    4       1     4th byte to repeat N4 times
    
    P = N1-1 + ((N2-1) _shl_ 2) + ((N3-1) _shl_ 4) + ((N4-1) _shl_ 6)
    

Example: 
    
    
    42 FF 01 02 03 04 55 01 02 03 04
    
    is unpacked as
    
    01 01 01 01 02 02 02 02 03 03 03 03 04 04 04 04 01 01 02 02 03 03 04 04
    

##### Packed dumps

In a packed dumps, it is the values themselves that are packed. To do that, an initial value is defined. By default it is $80. Then each subsequent value is computed by adding an offset from -7 to +7. This offset is stored as a number between 1 and 15 (by adding 8 to it). 

For example: 
    
    
    20 22 25
    
    can be packed as
    
    initial value $20, then offset +2, then offset +3
    

So it would be written in the compressed stream as: 
    
    
    C3 20 AB
    

Where $A = 10 = 2 + 8 is the first offset and $B = 11 = 3 + 8 is the second offset. 

Another example: 
    
    
    00 01 02 03 04 05 ... 0E 0F (a total of 16 bytes)
    
    can be packed as
    
    D0 00 99 99 99 99 99 99 99 90 (a total of 10 bytes)
    

Note that the initial value may be omitted by using the instructions $E2..$FE.

---

_Source: [https://wiki.freepascal.org/LazPaint_LZP_Format_Single_Image](https://web.archive.org/web/20240524191145/https://wiki.freepascal.org/LazPaint_LZP_Format_Single_Image)_
