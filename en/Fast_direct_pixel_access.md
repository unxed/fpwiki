# Fast direct pixel access

## Contents

  * 1 Introduction
  * 2 Pixel format
    * 2.1 Simple ordinal type
    * 2.2 Record
    * 2.3 Subrange
    * 2.4 Pointer
    * 2.5 Class
    * 2.6 Variant parts in record
  * 3 Bitmap structure
    * 3.1 Two dimensional dynamic array
    * 3.2 Raw dynamic memory
    * 3.3 Strict Pointer pixel access
  * 4 Pixel operation optimization
    * 4.1 Basic line algorithm
    * 4.2 Pointers
    * 4.3 Mass fill using FillDWord
    * 4.4 Inlining
    * 4.5 DMA
  * 5 Drawing bitmap on screen
  * 6 Draw methods
    * 6.1 TBitmap.Canvas.Pixels
    * 6.2 TBitmap.Canvas.Pixels with Update locking
    * 6.3 TLazIntfImage
    * 6.4 TBGRABitmap.ScanLine
    * 6.5 TBitmap.RawImage
    * 6.6 OpenGL
    * 6.7 OpenGL PBO
  * 7 See also



# Introduction

Standard graphical LCL components provides Canvas object for common drawing. But most of available graphic routines have some overhead given by universality, platform independence and safety. To achieve best drawing speed it can be useful to use specialized bitmap structures and routines. You can also use existing [Graphics libraries](<Graphics_libraries.md> "Graphics libraries"). 

Direct pixel access in libraries is generally slowed-down by more factors: 

  * Coordinate limits checking
  * Cropping
  * Facility for automatic image updating and redrawing (notifications and mass operation optimization)
  * Abstract program constructions as property, static and virtual methods, dynamic two dimensional arrays
  * Support for multiple pixel formats
  * Support for multiple platforms
  * Support for bit-level pixel size and addressing



This articles shows how to achieve a custom bitmap structure that is then copied to a TBitmap to render it on the screen. 

# Pixel format

Pixel type can be written in various ways to define its size and internal channel structure. Here you can found some known ways to define pixel type. 

## Simple ordinal type

We can use simple integer pixels which would be faster on 32-bit platform: 
    
    
    TFastBitmapPixel = Integer;
    

## Record

Or more abstract pixels with separated components: 
    
    
    TFastBitmapPixelComponents = packed record
      Blue: Byte;
      Green: Byte;
      Red: Byte;
      Alpha: Byte;
    end;
    

## Subrange

It is possible even go further to bit level and define 16-bit RGB pixel used for some LCD displays: 
    
    
    TFastBitmapPixelComponents16Bit = packed record
      Blue: 0..31; // 5 bits
      Green: 0..63; // 6 bits
      Red: 0..31; // 5 bits
    end;
    

## Pointer

Pixel can be pointer which would be useful for cases where pixel value itself is rather large or can be compressed somehow. 
    
    
    TFastBitmapPixelComponentsValue = packed record
      Blue: Word;
      Green: Word;
      Red: Word;
      Alpha: Word;
    end;
    
    TFastBitmapPixelComponents = ^TFastBitmapPixelComponentsValue;
    

## Class

Another situation is use of polymorphism of classes. But this would require calling Create and Destroy for each pixel instance and further background processing caused by allocation and deallocation memory on a heap. 
    
    
    TFastBitmapPixel = class
      procedure Clear; virtual;
    end;
    
    TFastBitmapPixelComponents = class(TFastBitmapPixel)
      Blue: Word;
      Green: Word;
      Red: Word;
      Alpha: Word;
      procedure Clear; override;
    end;
    
    TFastBitmapPixelByte = class(TFastBitmapPixel)
      Value: Byte;
      procedure Clear; override;
    end;
    

## Variant parts in record

Notable implementation of the pixel type is Graphics32 library which define pixel using composed record type with case construction to union access with different methods to the same pixel data. There four ways to access pixel data and its internal structure defined by this type: 
    
    
    TColor32 = type Cardinal;
    
    TColor32Component = (ccBlue, ccGreen, ccRed, ccAlpha);
    
    TColor32Entry = packed record
      case Integer of
        0: (B, G, R, A: Byte);
        1: (ARGB: TColor32);
        2: (Planes: array[0..3] of Byte);
        3: (Components: array[TColor32Component] of Byte);
    end;
    

# Bitmap structure

Bitmap class should provide direct pixel access given by X, Y coordinate. But some graphic operation could be further optimized by not doing coordinate calculations for every pixel and rather do pixel pointer shifting by simple memory pointer addition. Some mass operation as filling rectangular region could be optimized using Move and FillChar functions. 

## Two dimensional dynamic array

This is native way to express two dimensional array in pascal. Internal structure is implemented as pointer to array of pointers to data because dynamic array is in fact pointer to array data. Then calculation of pixel position is matter of fetching pointer for rows and add horizontal position to it. 
    
    
    interface
    
    type
      TFastBitmap = class
      private
        function GetSize: TPoint;
        procedure SetSize(const AValue: TPoint);
      public
        Pixels: array of array of TFastBitmapPixel;
        property Size: TPoint read GetSize write SetSize;
      end;
    
    implementation
    
    { TFastBitmap }
    
    function TFastBitmap.GetSize: TPoint;
    begin
      Result.X := Length(Pixels);
      if Result.X > 0 then 
        Result.Y := Length(Pixels[0])
      else 
        Result.Y := 0;
    end;
    
    procedure TFastBitmap.SetSize(const AValue: TPoint);
    begin
      SetLength(Pixels, AValue.X, AValue.Y);
    end;
    

## Raw dynamic memory

It is good to have whole bitmap in one compact memory area. Such memory block behave as video memory of video card. Position of pixels have to be calculated by using equation Y * Width + X with use of instructions for addition and multiplication. Access to pixels is pretty fast thanks to GetPixel and SetPixel methods inlining. But more instruction have to be used than in case of two dimensional dynamic array. 
    
    
    interface
    
    type
      PFastBitmapPixel = ^TFastBitmapPixel;
      TFastBitmap = class
      private
        FPixelsData: PByte;
        FSize: TPoint;
        function GetPixel(X, Y: Integer): TFastBitmapPixel; inline;
        procedure SetPixel(X, Y: Integer; const AValue: TFastBitmapPixel); inline;
        procedure SetSize(const AValue: TPoint);
      public
        constructor Create;
        destructor Destroy; override;
        property Size: TPoint read FSize write SetSize;
        property Pixels[X, Y: Integer]: TFastBitmapPixel read GetPixel write SetPixel;
      end;
    
    implementation
    
    { TFastBitmap }
    
    function TFastBitmap.GetPixel(X, Y: Integer): TFastBitmapPixel;
    begin
      Result := PFastBitmapPixel(FPixelsData + (Y * FSize.X + X) * SizeOf(TFastBitmapPixel))^;
    end;
    
    procedure TFastBitmap.SetPixel(X, Y: Integer; const AValue: TFastBitmapPixel);
    begin
      PFastBitmapPixel(FPixelsData + (Y * FSize.X + X) * SizeOf(TFastBitmapPixel))^ := AValue;
    end;
    
    procedure TFastBitmap.SetSize(const AValue: TPoint);
    begin
      if (FSize.X = AValue.X) and (FSize.Y = AValue.X) then
        Exit;
      FSize := AValue;
      FPixelsData := ReAllocMem(FPixelsData, FSize.X * FSize.Y * SizeOf(TFastBitmapPixel));
    end;
    
    constructor TFastBitmap.Create;
    begin
      Size := Point(0, 0);
    end;
    
    destructor TFastBitmap.Destroy;
    begin
      FreeMem(FPixelsData);
      inherited Destroy;
    end;
    

## Strict Pointer pixel access

We are able eliminate some of coordinate multiplications with low level pixel access using pointers only. Then only addition(incrementation) is necessary to change current pixel position. 
    
    
    interface
    
    type
      TFastBitmap = class
      private
        FPixelsData: PByte;
        FSize: TPoint;
        procedure SetSize(const AValue: TPoint);
      public
        constructor Create;
        destructor Destroy; override;
        procedure RandomImage;
        property Size: TPoint read FSize write SetSize;
        function GetPixelAddress(X, Y: Integer): PFastBitmapPixel; inline;
        function GetPixelSize: Integer; inline;
      end;  
    
    implementation
    
    { TFastBitmap }
    
    procedure TFastBitmap.SetSize(const AValue: TPoint);
    begin
      if (FSize.X = AValue.X) and (FSize.Y = AValue.X) then Exit;
      FSize := AValue;
      FPixelsData := ReAllocMem(FPixelsData, FSize.X * FSize.Y * SizeOf(TFastBitmapPixel));
    end;
    
    constructor TFastBitmap.Create;
    begin
      Size := Point(0, 0);
    end;
    
    destructor TFastBitmap.Destroy;
    begin
      FreeData(FPixelData);
      inherited Destroy;
    end;
    
    function TFastBitmap.GetPixelAddress(X, Y: Integer): PFastBitmapPixel;
    begin
      Result := PFastBitmapPixel(FPixelsData) + Y * FSize.X + X;
    end;
    
    function TFastBitmap.GetPixelSize: Integer;
    begin
      Result := SizeOf(TFastBitmapPixel);
    end;
    

In this case drawing pixels is less readable: 
    
    
    procedure RandomImage(FastBitmap: TFastBitmap);
    var
      X, Y: Integer;
      PRow: PFastBitmapPixel;
      PPixel: PFastBitmapPixel;
    begin
      with FastBitmap do begin
        PRow := GetPixelAddress(0, Size.Y div 2);
        for Y := 0 to Size.Y - 1 do begin
          PPixel := PRow;
          for X := 0 to Size.X - 1 do begin
            PPixel^ := Random(256) or (Random(256) shl 16) or (Random(256) shl 8);
            Inc(PPixel);
          end;
          Inc(PRow, Size.X);
        end;
      end;
    end;
    

# Pixel operation optimization

## Basic line algorithm

This is naive form which is readable but with price of slower processing. 
    
    
    procedure TFastBitmap.HorizontalLine(X, Y, Length: Integer; Color: TFastBitmapPixel);
    var
      I: Integer;
    begin
      for I := 0 to Length - 1 do
        Pixels[X + I, Y] := Color;
    end;
    

## Pointers

With use of pointers we can eliminate much of pixel address addition and multiplication by Pixels property access. Only fast increment operation is performed. 
    
    
    procedure TFastBitmap.HorizontalLine(X, Y, Length: Integer; Color: TFastBitmapPixel);
    var
      I: Integer;
      P: PFastBitmapPixel;
    begin
      P := PFastBitmapPixel(FPixelData + (Y * Size.X + X) * SizeOf(TFastBitmapPixel));
      for I := 0 to Length - 1 do begin
        P^ := Color;
        Inc(P);
      end;
    end;
    

## Mass fill using FillDWord

Access using pointers and incrementation is fastest possible using conventional single operations. But most of todays CPU offer instructions for mass operations like MOVS, STOS for x86 architecture. Pixel size should be 1, 2 or 4 bytes to be able to use this optimization. 
    
    
    procedure TFastBitmap.HorizontalLine(X, Y, Length: Integer; Color: TFastBitmapPixel);
    var
      I: Integer;
      P: PFastBitmapPixel;
    begin
      P := PFastBitmapPixel(FPixelData + (Y * Size.X + X) * SizeOf(TFastBitmapPixel));
      FillDWord(P^, Length, Color);
    end;
    

## Inlining

If code is notably smaller like SetPixel and GetPixel methods it is better to inline instructions rather than do push and pop operations on stact with execution of call and ret instruction. This optimization will be even significant if such operation is executed many times as pixel operations do. 
    
    
    procedure TFastBitmap.HorizontalLine(X, Y, Length: Integer; Color: TFastBitmapPixel); inline;
    var
      I: Integer;
      P: PFastBitmapPixel;
    begin
      P := PFastBitmapPixel(FPixelData + (Y * Size.X + X) * SizeOf(TFastBitmapPixel));
      FillDWord(P^, Length, Color);
    end;
    

## DMA

If memory block have to be copied to another memory place or device memory DMA(Direct Memory Access) can be used. CPU doesn't have to be involved in copy operations and can do further processing. This kind of optimization can be used in OpenGL for copying data to video card memory. 

# Drawing bitmap on screen

In this test let assume that we have simple bitmap structure designed as two dimensional byte array where each pixel have 256 possible colors. This could be gray image or some palette mapped image. All image manipulation will be done with custom functions with direct pixel access. Thanks to custom data structure functions could be optimized for faster block memory operations if necessary. 

To be able to display image on Form custom bitmap have to be copied to some TWinControl canvas area. Image have to be copied repeatedly if motion image is generated. Every bitmap copy in memory take some time. Then our aim is to do as low as possible copy operations and rather copy our bitmap to screen directly if possible. 

You can draw image as fast as possible in simple loop: 
    
    
    repeat
      FastBitmapToBitmap(FastBitmap, Image1.Picture.Bitmap);
      Application.ProcessMessages;
    until Terminated;
    

Or draw image for example using Timer with defined drawing interval. Even if nothing is changed on bitmap there is no need to copy bitmap to screen so RedrawPending simple flag could be used. Thanks to delayed draw execution with calling Redraw method drawing of frames could be skipped. 
    
    
    TForm1 = class(TForm)
    published
      procedure Timer1Execute(Sender: TObject);
      ...  
    public
      RedrawPending: Boolean;
      Drawing: Boolean;
      FastBitmap: TFastBitmap;
      procedure Redraw;
      ...
    end;
      
    procedure TForm1.Redraw;
    begin
      RedrawPending := True;
    end;
      
    procedure TForm1.Timer1Execute(Sender: TObject);
    begin
      if (not Drawing) and RedrawPending then 
      try
        Drawing := True;
        CustomProcessing(FastBitmap);
        FastBitmapToBitmap(FastBitmap, Image1.Picture.Bitmap);        
      finally
        RedrawPending := False;
        Drawing := False;
      end;
    end;
    

# Draw methods

  * These methods show various ways how to draw bitmap.
  * FastBitmap needs to be already initialized to desired size. Target Bitmap(Canvas) should have same size as FastBitmap. Use something like Bitmap.SetSize(FastBitmap.X, FastBitmap.Y)
  * These drawing methods don't assume exact pixel color format. They rather use abstract function called FastPixelToTColor for illustration purpose which needs to be implemented according real chosen color format.
  * These methods assume for simplicity Bitmap structure accessible by Pixels two-dimensional array usually implemented as property. For even better performance various further optimization could be used. See #Pixel operation optimization and #Strict Pointer pixel access.



## TBitmap.Canvas.Pixels

This is most straighforward but slowest method: 
    
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; Bitmap: TBitmap);
    var
      X, Y: Integer;
    begin
      for Y := 0 to FastBitmap.Size.Y - 1 do
        for X := 0 to FastBitmap.Size.X - 1 do
          Bitmap.Canvas.Pixels[X, Y] := FastPixelToTColor(FastBitmap.Pixels[X, Y]);
    end;
    

## TBitmap.Canvas.Pixels with Update locking

Previous method could be speeded up by update locking and thus reducing per pixel update and event signaling. 
    
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; Bitmap: TBitmap);
    var
      X, Y: Integer;
    begin
      try
        Bitmap.BeginUpdate(True);
        for Y := 0 to FastBitmap.Size.Y - 1 do
          for X := 0 to FastBitmap.Size.X - 1 do
            Bitmap.Canvas.Pixels[X, Y] := FastPixelToTColor(FastBitmap.Pixels[X, Y]);
      finally
        Bitmap.EndUpdate(False);
      end;
    end;
    

## TLazIntfImage

TLazIntfImage is a memory image. I can store transparency and 16-bit values for each channel. TBitmap is compatible with Delphi and use TColor type for pixels which do not contain alpha information. So TLazIntfImage is better suited for image processing. This component provide faster access to pixels because it is an array in memory like our TFastBitmap. 

Here we copy TFastBitmap grayscale pixels into a TLazIntfImage to convert it into a TBitmap. 
    
    
    uses
      ..., LCLType, LCLProc, LCLIntf, IntfGraphics;
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; Bitmap: TBitmap);
    var
      X, Y: Integer;
      TempIntfImage: TLazIntfImage;
    begin
      try
        TempIntfImage := Bitmap.CreateIntfImage; // Temp image could be pre-created and help by owner class to avoid new creation in each frame
        for Y := 0 to FastBitmap.Size.Y - 1 do
          for X := 0 to FastBitmap.Size.X - 1 do begin
            TempIntfImage.Colors[X, Y] := TColorToFPColor(FastPixelToTColor(FastBitmap.Pixels[X, Y]));
          end;
        Bitmap.LoadFromIntfImage(TempIntfImage);
      finally
        TempIntfImage.Free;
      end;                           
    end;
    

We can also work directly with TLazIntfImage pixels, which can well serve our purpose. To do this, create a TLazInfImage, set the pixel format and then use GetDataLineStart to access scanlines. This is still an indirect method, because a TLazIntfImage need to be copied to a TBitmap to be drawn. 

## TBGRABitmap.ScanLine

There is graphic library BGRABitmap which allow access to scan lines. Overall speed of this method is pretty good. Drawing is done directly to Canvas of some TWinControl components like TForm of TPaintBox. The pixel format is 32-bit color with alpha channel, i.e. 8-bit for each channel. 

Using TBitmap.ScanLine was a method used frequently on Delphi. But TBitmap.ScanLine is not supported by LCL. ScanLine property give access to memory starting point for each row raw data. Then direct manipulation with pixels is much faster than using Pixels property as no additional events is fired. 

We can copy our grayscale FastBitmap data to a TBGRABitmap to render it on the screen. 
    
    
    uses
      ..., BGRABitmap, BGRABitmapTypes;
    
    procedure FastBitmapToCanvas(FastBitmap: TFastBitmap; Canvas: TCanvas);
    var
      X, Y: Integer;
      P: PBGRAPixel;
      bgra: TBGRABitmap;
    begin
      bgra := TBGRABitmap.Create(FastBitmap.Size.X, FastBitmap.Size.Y);
      with FastBitmap do
        for Y := 0 to Size.Y - 1 do begin
          P := PInteger(bgra.ScanLine[Y]);
          for X := 0 to Size.X - 1 do begin
            PInteger(P)^ := FastPixelToTColor(Pixels[X, Y]) or $ff000000;
            Inc(P);
          end;
        end;
      bgra.InvalidateBitmap; // Changed by direct access
      bgra.Draw(Canvas, 0, 0, False);
      bgra.Free;
    end;
    

We can also use TBGRABitmap only. This library works if possible with device independent bitmaps of the operating system, so it is generally a direct pixel access or quasi-direct pixel access. 

[BGRABitmap tutorial shows how to access directly to pixels](<BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4")

## TBitmap.RawImage

This method is so far fastest in comparing to previous ones but more complicated as special care have to be given to bitmap data structure. Example assume that bitmap PixelFormat is pf24bit. Accessed raw data may differs across platforms. 
    
    
    uses
      ..., GraphType;
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; Bitmap: TBitmap);
    var
      X, Y: Integer;
      PixelPtr: PInteger;
      PixelRowPtr: PInteger;
      P: TPixelFormat;
      RawImage: TRawImage;
      BytePerPixel: Integer;
    begin
      try
        Bitmap.BeginUpdate(False);
        RawImage := Bitmap.RawImage;
        PixelRowPtr := PInteger(RawImage.Data);
        BytePerPixel := RawImage.Description.BitsPerPixel div 8;
        for Y := 0 to Size.Y - 1 do begin
          PixelPtr := PixelRowPtr;
          for X := 0 to Size.X - 1 do begin
            PixelPtr^ := FastPixelToTColor(Pixels[X, Y]);
            Inc(PByte(PixelPtr), BytePerPixel);
          end;
          Inc(PByte(PixelRowPtr), RawImage.Description.BytesPerLine);
        end;
      finally
        Bitmap.EndUpdate(False);
      end;  
    end;
    

**RawImage.Description values examples on various platforms:**

Platform  | Format  | Depth  | BitsPerPixel  | BitOrder  | ByteOrder  | LineOrder  | LineEnd  | RedPrec  | RedShift  | GreenPrec  | GreenShift  | BluePrec  | BlueShift  | AlphaPrec  | AlphaShift   
---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---  
Windows | RGBA | 24 | 24 | ReverseBits | LSBFirst | TopToBottom | DWordBoundary | 8 | 16 | 8 | 8 | 8 | 0 | 0 | 0   
Windows | RGBA | 15 | 16 | ReverseBits | LSBFirst | TopToBottom | DWordBoundary | 5 | 10 | 5 | 5 | 5 | 0 | 0 | 0   
Linux GTK2 | RGBA | 24 | 32 | BitsInOrder | LSBFirst | TopToBottom | DWordBoundary | 8 | 16 | 8 | 8 | 8 | 0 | 0 | 0   
  
## OpenGL

OpenGL is mainly used for 3D complex modeling but it can be used for simple 2D accelerated graphics. We need initialized OpenGL, create one textured rectangle with screen resolution and set orthogonal view. Then we will able to fill texture by our custom converted bitmap data. But this method is not significantly faster then RawImage.Data method because all image data are copied with glTexImage2D function which mean slow copy using CPU. 
    
    
    uses
      ..., GL, OpenGLContext;
    
    var
      TextureId: GLuint;
      TextureData: Pointer; 
      OpenGLControl1: TOpenGLControl;
    
    procedure InitGL;
    begin
      glMatrixMode(GL_PROJECTION);
      glLoadIdentity;
      glOrtho(0, OpenGLControl1.Width, OpenGLControl1.Height, 0, 0, 1);
      glMatrixMode(GL_MODELVIEW);
      glLoadIdentity();
      glDisable(GL_DEPTH_TEST);
      glViewport(0, 0, OpenGLControl1.Width, OpenGLControl1.Height);
      glGenTextures(1, @TextureId);
      glTexEnvf(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_MODULATE); 
    end;
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; OpenGLControl: TOpenGLControl);
    var
      X, Y: Integer;
      P: PInteger;
      R: PInteger;
    const
      GL_CLAMP_TO_EDGE = $812F;
    begin
      glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
    
      P := OpenGLBitmap;
      with FastBitmap do
      for Y := 0 to Size.Y - 1 do begin
        R := P;
        for X := 0 to Size.X - 1 do begin
          R^  := FastPixelToTColor(Pixels[X, Y]) or $ff000000;
          Inc(R);
        end;
        Inc(P, Size.X);
      end;
    
      glLoadIdentity;
      glTranslatef(-OpenGLControl.Width div 2, -OpenGLControl.Height div 2, 0.0);
    
      glEnable(GL_TEXTURE_2D);
      glBindTexture(GL_TEXTURE_2D, TextureId);
        //glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
        //glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);
        glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
        glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
        glTexImage2D(GL_TEXTURE_2D, 0, 4, OpenGLControl.Width, OpenGLControl.Height,
          0, GL_RGBA, GL_UNSIGNED_BYTE, OpenGLBitmap);
    
      glBegin(GL_QUADS);
        glColor3ub(255, 255, 255);
        glTexCoord2f(0, 0);
        glVertex3f(0, 0, 0);
        glTexCoord2f(OpenGLControl.Width div 2, 0);
        glVertex3f(OpenGLControl.Width, 0, 0);
        glTexCoord2f(OpenGLControl.Width div 2, OpenGLControl.Height div 2);
        glVertex3f(OpenGLControl.Width, OpenGLControl.Height, 0);
        glTexCoord2f(0, OpenGLControl.Height div 2);
        glVertex3f(0, OpenGLControl.Height, 0);
      glEnd();
    
      OpenGLControl.SwapBuffers;
    end;
    

## OpenGL PBO

This method use asynchronous DMA transfer to copy texture data thus CPU is free to do further computations. It also eliminate one additional copy operation which is done by glTexImage2D in previous method. Method require GL_ARB_pixel_buffer_object extension. 

  * [OpenGL Pixel Buffer Object (PBO)](<http://www.songho.ca/opengl/gl_pbo.html>)


    
    
    uses
      ..., GL, OpenGLContext;
    
    var
      TextureId: GLuint;
      TextureData: Pointer; 
      OpenGLControl1: TOpenGLControl;
      pboIds: array[0..1] of GLuint;
    
    procedure InitGL;
    var
      DataSize: Integer;
    begin
      glMatrixMode(GL_PROJECTION);
      glLoadIdentity;
      glOrtho(0, OpenGLControl1.Width, OpenGLControl1.Height, 0, 0, 1);
      glMatrixMode(GL_MODELVIEW);
      glLoadIdentity();
      glDisable(GL_DEPTH_TEST);
      glViewport(0, 0, OpenGLControl1.Width, OpenGLControl1.Height);
      glGenTextures(1, @TextureId);
      glTexEnvf(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_MODULATE); 
    
      OpenGLControl1.MakeCurrent;
      DataSize := OpenGLControl1.Width * OpenGLControl1.Height * SizeOf(Integer);
      if Load_GL_ARB_vertex_buffer_object then begin
        glGenBuffersARB(2, @pboIds);
        glBindBufferARB(GL_PIXEL_PACK_BUFFER_ARB, pboIds[0]);
        glBufferDataARB(GL_PIXEL_PACK_BUFFER_ARB, DataSize, Pointer(0), GL_STREAM_READ_ARB);
        glBindBufferARB(GL_PIXEL_PACK_BUFFER_ARB, pboIds[1]);
        glBufferDataARB(GL_PIXEL_PACK_BUFFER_ARB, DataSize, Pointer(0), GL_STREAM_READ_ARB);
    
      end else raise Exception.Create('GL_ARB_pixel_buffer_object not supported');
    
      glEnable(GL_TEXTURE_2D);
      glBindTexture(GL_TEXTURE_2D, TextureId);
        //glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
        //glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);
        glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
        glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
        glTexImage2D(GL_TEXTURE_2D, 0, 4, OpenGLControl1.Width, OpenGLControl1.Height,
          0, GL_RGBA, GL_UNSIGNED_BYTE, OpenGLBitmap);
    end;
    
    function FastBitmapToBitmap(FastBitmap: TFastBitmap; OpenGLControl: TOpenGLControl);
    var
      X, Y: Integer;
      P: PInteger;
      R: PInteger;
      Ptr: ^GLubyte;
      TextureShift: TPoint;
      TextureShift2: TPoint;
    const
      GL_CLAMP_TO_EDGE = $812F;
    begin
      // "index" is used to read pixels from framebuffer to a PBO
      // "nextIndex" is used to update pixels in the other PBO
      Index := (Index + 1) mod 2;
      NextIndex := (Index + 1) mod 2;
    
      glLoadIdentity;
    
      // bind the texture and PBO
      glBindTexture(GL_TEXTURE_2D, TextureId);
      glBindBufferARB(GL_PIXEL_UNPACK_BUFFER_ARB, pboIds[index]);
    
      // copy pixels from PBO to texture object
      // Use offset instead of ponter.
      glTexSubImage2D(GL_TEXTURE_2D, 0, 0, 0, OpenGLControl.Width, OpenGLControl.Height,
        GL_BGRA, GL_UNSIGNED_BYTE, Pointer(0));
    
    
      // bind PBO to update texture source
      glBindBufferARB(GL_PIXEL_UNPACK_BUFFER_ARB, pboIds[nextIndex]);
    
      // Note that glMapBufferARB() causes sync issue.
      // If GPU is working with this buffer, glMapBufferARB() will wait(stall)
      // until GPU to finish its job. To avoid waiting (idle), you can call
      // first glBufferDataARB() with NULL pointer before glMapBufferARB().
      // If you do that, the previous data in PBO will be discarded and
      // glMapBufferARB() returns a new allocated pointer immediately
      // even if GPU is still working with the previous data.
      glBufferDataARB(GL_PIXEL_UNPACK_BUFFER_ARB, OpenGLControl.Width * OpenGLControl.Height * SizeOf(Integer), Pointer(0), GL_STREAM_DRAW_ARB);
    
      // map the buffer object into client's memory
      ptr := glMapBufferARB(GL_PIXEL_UNPACK_BUFFER_ARB, GL_WRITE_ONLY_ARB);
      if Assigned(ptr) then begin
        // update data directly on the mapped buffer
        P := PInteger(Ptr);
        with FastBitmap do
        for Y := 0 to Size.Y - 2 do begin
          R := P;
          for X := 0 to Size.X - 1 do begin
            R^ := NoSwapBRComponent(FastPixelToTColor(Pixels[X, Y]) or $ff000000);
            Inc(R);
          end;
          Inc(P, Size.X);
        end;
        glUnmapBufferARB(GL_PIXEL_PACK_BUFFER_ARB);
      end;
    
      // it is good idea to release PBOs with ID 0 after use.
      // Once bound with 0, all pixel operations are back to normal ways.
      glBindBufferARB(GL_PIXEL_UNPACK_BUFFER_ARB, 0);
    
      glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
      glTranslatef(-OpenGLControl.Width / 2, -OpenGLControl.Height / 2, 0.0);
      glBindTexture(GL_TEXTURE_2D, TextureId);
    
      glBegin(GL_QUADS);
        glColor3ub(255, 255, 255);
        glTexCoord2f(0, 0);
        glVertex3f(0, TextureShift2.Y, 0);
        glTexCoord2f(OpenGLControl.Width div 2, 0);
        glVertex3f(OpenGLControl.Width, 0, 0);
        glTexCoord2f(OpenGLControl.Width div 2, OpenGLControl.Height div 2);
        glVertex3f(OpenGLControl.Width, OpenGLControl.Height, 0);
        glTexCoord2f(0, OpenGLControl.Height div 2);
        glVertex3f(0, OpenGLControl.Height, 0);
      glEnd();                   
    
      OpenGLControl.SwapBuffers;
    end;
    

# See also

  * [Developing with Graphics](<Developing_with_Graphics.md> "Developing with Graphics") especially the Direct pixel access section
  * [Accessing the Interfaces directly](<Accessing_the_Interfaces_directly.md> "Accessing the Interfaces directly")
  * [GraphicTest](<GraphicTest.md> "GraphicTest") \- speed comparison of various direct draw methods

---

_Source: [https://wiki.freepascal.org/Fast_direct_pixel_access](https://web.archive.org/web/20211019105608/https://wiki.freepascal.org/Fast_direct_pixel_access)_
