# PascalMagick

│ **English (en)** │  **[русский (ru)](<../ru/PascalMagick.md>)** │

## Contents

  * 1 About
    * 1.1 About ImageMagick
    * 1.2 ImageMagick Book
    * 1.3 About MagickWand
    * 1.4 About MagickCore
    * 1.5 About PascalMagick
  * 2 Screenshot
  * 3 Authors
  * 4 License
  * 5 Download
    * 5.1 Old versions
  * 6 Status
  * 7 Installation
  * 8 Demonstration program 1
  * 9 Procedures to load images to a Lazarus TBitmap with ImageMagick
  * 10 Bug Reporting/Feature Request
  * 11 Change Log
  * 12 Help



### About

#### About ImageMagick

[ImageMagick](<http://www.imagemagick.org>) is a free software suite developed to create, edit, and compose bitmap images. It supports a huge variety of formats (over 200) including formats like GIF, JPEG, JPEG-2000, PNG, PDF, PhotoCD, TIFF, and DPX. Images can be cropped, colors can be changed, various effects can be applied, images can be rotated and combined, and text, lines, polygons, ellipses and Bézier curves can be added to images and stretched and rotated. 

The suite runs on all major operating systems and it can also be used from the command line and its command line tools package is one of Linux standard packages, being distributed with many distributions. 

Features and Capabilities: 

  * Convert an image from one format to another (e.g. PNG to JPEG)
  * Resize, rotate, sharpen, color reduce, or add special effects to an image
  * Create a montage of image thumbnails
  * Create a transparent image suitable for use on the Web
  * Turn a group of images into a GIF animation sequence
  * Create a composite image by combining several separate image
  * Draw shapes or text on an image
  * Decorate an image with a border or frame
  * Describe the format and characteristics of an image



#### ImageMagick Book

[ImageMagick Tricks](<http://www.packtpub.com/imagemagick/book>) This fast paced and practical tutorial is packed with examples of photo manipulations, logo creation, animations, and complete web projects. With this book up your sleeve, you'll be creating spellbinding images from code in no time. 

#### About MagickWand

The [MagickWand API](<http://www.imagemagick.org/script/magick-wand.php>) is the recommended interface by the ImageMagick team. It is an easy to use programming interface for using the suite. 

#### About MagickCore

The MagickCore API is a low-level interface for ImageMagick. 

#### About PascalMagick

This port is a translation of the c header files for both MagickWand and Magick Core. 

### Screenshot

### Authors

[Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")

[Ángel Eduardo García](</User:Arcnor> "User:Arcnor")

### License

BSD-style and compatible with the GPL. 

You can read the Licence [here](<http://www.imagemagick.org/script/license.php>). 

### Download

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** PascalMagick is included in all recent FPC releases. There is no need to download it

#### Old versions

PascalMagick 0.4 is available here: [http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=174103&release_id=431432](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=174103&release_id=431432>)

For old FPC versions, you can download the subversion version of this project using this command: 
    
    
    svn checkout http://svn.freepascal.org/svn/fpc/trunk/packages/base/imagemagick imagemagick
    

### Status

Magick Wand headers are complete and working on both Windows and Linux. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** MagickWand probably updated their headers in an incompatible way; current ImageMagick compressiontypes are incompatible with those in FPC < 2.7.1; see [Issue #26723](<https://bugs.freepascal.org/view.php?id=26723>) which also contains a workaround. This may apply to more ImageMagick consts, enums and function

### Installation

The current version of PascalMagick works on Windows and Linux. Beta testers are necessary for the Mac OS X version. 

To start with, install ImageMagick binaries from the official website: <http://www.imagemagick.org/script/binary-releases.php>

— As the binaries version(s) there may have moved ahead of the current PascalMagick release, there is a 2022-07-13 discussion, on setup under Windows here: <https://forum.lazarus.freepascal.org/index.php?topic=59925.msg447403#msg447403>

Now make sure the PascalMagick package is installed (Instructions on the #Download section above). 

To check that everything is working, open the packages\imagemagick\examples\wanddemo.lpr project under your FPC directory. Now go to the Project --> "Compiler Options dialog" and change the "Other Units" field to point to "../magick", so it can find the PascalMagick/magick/ImageMagick.pas file. After this you can compile and run the test program. 

If the program is working, it will load the image.png image located on the same directory, resize it and then save it as a jpg called image.jpg. 

One extra step is necessary on Linux: Rename the image.PNG to image.png due to case issues. 

### Demonstration program 1
    
    
    {
      Demonstration program for the ImageMagick Library.
    
      This program was converted from C by Felipe Monteiro de Carvalho.
    
      Usage: Just execute the program. It will resize the image.png image
      on its directory to fit (106, 80) and convert it to a jpg.
      
      Dez/2005
    }
    {Version 0.1}
    program wanddemo;
    
    {$mode objfpc}{$H+}
    
    uses 
      SysUtils, magick_wand, ImageMagick;
    
    procedure ThrowWandException(wand: PMagickWand);
    var
      description: PChar;
      severity: ExceptionType;
    begin
      description := MagickGetException(wand, @severity);
      WriteLn(Format('An error ocurred. Description: %s', [description]));
      description := MagickRelinquishMemory(description);
      Abort;
    end;
    
    var
      status: MagickBooleanType;
      wand: PMagickWand;
    begin
      { Read an image. }
      
      MagickWandGenesis;
    
      wand := NewMagickWand;
      
      try
        status := MagickReadImage(wand, 'image.png');
        if (status = MagickFalse) then ThrowWandException(wand);
    
        { Turn the images into a thumbnail sequence. }
    
        MagickResetIterator(wand);
    
        while (MagickNextImage(wand) <> MagickFalse) do
         MagickResizeImage(wand, 106, 80, LanczosFilter, 1.0);
    
        { Write the image as MIFF and destroy it. }
      
        status := MagickWriteImages(wand, 'image.jpg', MagickTrue);
        if (status = MagickFalse) then ThrowWandException(wand);
    
      finally
        wand := DestroyMagickWand(wand);
    
        MagickWandTerminus;
      end;
    end.
    

### Procedures to load images to a Lazarus TBitmap with ImageMagick

Source: <http://lazarus.freepascal.org/index.php/topic,20016.msg114885.html#msg114885>
    
    
    unit magicklcl;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Graphics;
    
    procedure LoadMagickBitmap(FileName: string; Bmp: TBitmap); overload;
    procedure LoadMagickBitmap(Strm: TMemoryStream; Bmp: TBitmap); overload;
    
    implementation
    
    uses magick_wand, ImageMagick, IntfGraphics, FPimage, LazUTF8;
    
    procedure LoadMagickBitmapWand(Wand: PMagickWand; Bmp: TBitmap);
    var
      img: Pimage;
      pack: PPixelPacket;
      limg: TLazIntfImage;
      i, j, wi, he: integer;
      colo: TFPColor;
    begin
      img := GetImageFromMagickWand(wand);
      he := MagickGetImageHeight(wand);
      wi := MagickGetImageWidth(wand);
      limg := TLazIntfImage.Create(0, 0);
      try
        limg.DataDescription := GetDescriptionFromDevice(0, wi, he);
        pack := GetAuthenticPixels(img, 0, 0, wi, he, nil);
        for j := 0 to he - 1 do
          for i := 0 to wi - 1 do
          begin
            colo.red := pack^.red;
            colo.green := pack^.green;
            colo.blue := pack^.blue;
            colo.alpha := pack^.opacity;
            limg.Colors[i, j] := colo;
            Inc(pack);
          end;
        Bmp.LoadFromIntfImage(limg);
      finally
        limg.Free;
      end;
    end;
    
    procedure LoadMagickBitmap(FileName: string; Bmp: TBitmap);
    var
      wand: PMagickWand;
      status: MagickBooleanType;
      description: PChar;
      severity: ExceptionType;
    begin
      wand := NewMagickWand;
      try
        status := MagickReadImage(wand, PChar(UTF8ToSys(FileName)));
        if (status = MagickFalse) then
        begin
          description := MagickGetException(wand, @severity);
          raise Exception.Create(Format('An error ocurred. Description: %s',
            [description]));
          description := MagickRelinquishMemory(description);
        end else LoadMagickBitmapWand(wand, Bmp);
      finally
        wand := DestroyMagickWand(wand);
      end;
    end;
    
    procedure LoadMagickBitmap(Strm: TMemoryStream; Bmp: TBitmap);
    var
      wand: PMagickWand;
      status: MagickBooleanType;
      description: PChar;
      severity: ExceptionType;
    begin
      wand := NewMagickWand;
      try
        Strm.Position := 0;
        status := MagickReadImageBlob(wand, Strm.Memory, Strm.Size);
        if (status = MagickFalse) then
        begin
          description := MagickGetException(wand, @severity);
          raise Exception.Create(Format('An error ocurred. Description: %s',
            [description]));
          description := MagickRelinquishMemory(description);
        end else LoadMagickBitmapWand(wand, Bmp);
      finally
        wand := DestroyMagickWand(wand);
      end;
    end;
    
    initialization
      MagickWandGenesis;
    
    finalization;
      MagickWandTerminus;
    
    end.
    

### Bug Reporting/Feature Request

Tests are necessary to verify if the bindings work with all versions of ImageMagick. 

Please post Bug Reports / Feature Requests in the discussion page. 

### Change Log

  * Later:


  1. Please see the fpc change log as the code has moved to the FPC source code.


  * 07.11.07


  1. Added 4 missing functions on magick_image.inc on fpc subversion, but no new release


  * 12.07.06 PascalMagick version 0.4 released


  1. Last bugs fixed


  * 24.05.06 PascalMagick version 0.3 released


  1. Minor fixes on the bindings


  * 10.04.06 PascalMagick version 0.2 released


  1. Based on ImageMagic 6.2
  2. MagickWand API fully translated
  3. Added a second demonstration program


  * 27.12.05 PascalMagick version 0.1 released


  1. About 80% of the MagickWand API is translated
  2. The basic demonstration program is working well
  3. Only the very minimum necessary MagickCode headers were translated


  * 14.12.05 Began working on a pascal port for the c headers



### Help

Please send help requests to the [Lazarus Forum](<Forum.md> "Forum") or the [Lazarus mailing list](<https://lists.lazarus-ide.org/listinfo>).

---

_Source: [https://wiki.freepascal.org/PascalMagick](https://web.archive.org/web/20250321064921/https://wiki.freepascal.org/PascalMagick)_
