# LazPaint Command line

│ **English (en)** │  **[suomi (fi)](</LazPaint_Command_line/fi> "LazPaint Command line/fi")** │  **[français (fr)](</LazPaint_Command_line/fr> "LazPaint Command line/fr")** │    
****

[ Go back](<LazPaint.md> "LazPaint")

## Contents

  * 1 Command line
    * 1.1 Line without command
    * 1.2 Line without output file
    * 1.3 Line without input file
    * 1.4 General commands
    * 1.5 Filter commands



### Command line

General use : 
    
    
    lazpaint inputfile [/command1 [/command2...]] [outputfile]
    

Or 
    
    
    lazpaint inputfile [-command1 [-command2...]] [outputfile]
    

On Linux, the command prefix is "-". 

On Windows, the command prefix can be "-" or "/". 

#### Line without command

You can omit commands to convert an image to another file format. For example : 
    
    
    lazpaint image.pdn image.png
    

will flatten and save a Paint.NET image into a PNG. 

#### Line without output file

If you do not specify an output file, the application is opened with the resulting image. For example: 
    
    
    lazpaint image.png /RotateCW
    

will open the image with a 90° rotation. 

#### Line without input file

You can also create an image out of nothing. For example : 
    
    
    lazpaint /New(800,600) /Gradient(255,255,255,255,0,0,0,255,Linear,0,0,800,600) blackwhite.png
    

will create an image with a black and white gradient and save it into blackwhite.png. 

#### General commands

Here is the list of supported commands : 

  * /HorizontalFlip : flips image horizontally
  * /VerticalFlip : flips image vertically
  * /SwapRedBlue : swaps red and blue channels
  * /RotateCW : rotates image clockwise
  * /RotateCCW : rotates image counterclockwise
  * /Resample(Width,Height: Integer) : resample image to Width x Height
  * /Opacity(Value: Byte) : applies a global factor to opacity
  * /Gradient(red1,green1,blue1,alpha1,red2,green2,blue2,alpha2: Byte; type; x1,y1,x2,y2: Single) : draws a gradient. Type can be Linear, Reflected, Diamond or Radial.
  * /New(Width,Height) : creates a new empty image of Width x Height



For example : 
    
    
    lazpaint bigimage.bmp /Resample(320,200) smallimage.png
    

will save a small copy of bigimage.bmp into smallimage.png 

#### Filter commands

You can apply filters with default values. Here is the list of filter commands: 

**Blur filters :** BlurPrecise, BlurRadial, BlurFast, BlurCorona, BlurDisk, BlurMotion, BlurCustom 

**Pixel filters :** Sharpen, Smooth, Median, ClearType, ClearTypeInverse, Emboss, Contour, Grayscale, Negative, LinearNegative, ComplementaryColor, Normalize, SmartZoom3 

**Deformation filters :** Sphere, Twirl, Cylinder, Plane 

**Rendering filters :** PerlinNoise, CyclicPerlinNoise, Clouds, CustomWater, Water, Wood, WoodVertical, Plastik, MetalFloor, Camouflage, SnowPrint, Stone, RoundStone, Marble 

For example : 
    
    
    lazpaint photo.jpg /Contour /Emboss /Normalize
    

will transform the image into a carved stone.

---

_Source: [https://wiki.freepascal.org/LazPaint_Command_line](https://web.archive.org/web/20250404084812/https://wiki.freepascal.org/LazPaint_Command_line)_
