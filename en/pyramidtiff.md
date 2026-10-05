# pyramidtiff

## Contents

  * 1 Overview
  * 2 Getting pyramidtif
  * 3 Usage and parameters
  * 4 Examples



# Overview

**pyramidtiff** is a command line tool which takes an image file (jpg, tif, png or bmp) and creates a multi resolution tif file with tiles. Such tif files can be used by high resolution image viewers like iipimage to quickly zoom into huge images. 

The new tif contains the original image, the image scaled to half the width and height, the image scaled to quarter width and height and so forth. Each image is saved in tiles and compressed with the Deflate (zlib) algorithm. These are basic TIFF features, so any tiff viewer can show the image. 

# Getting pyramidtif

The source is on lazarus-ccr and requires at least fpc 2.7.1: 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/applications/pyramidtiff> pyramidtiff
    

# Usage and parameters
    
    
    -c <input file>
       Check if file is a pyramid, tiled tif. 0 = yes, 1 = no.
    -i <input file>
       Input image file can be a: jpg;jpeg png bmp tif;tiff
    -o <output file>
       Output image file. It will always be a tif file, no matter what extension it has.
    --width=<tilewidth>
       In pixel. Default=256
    --height=<tileheight>
       In pixel. Default=256
    --min-size=<min size>
       Create no images with a smaller width or height than this value in pixel.
       The original image is always added.
       Default=32
    -h or --help
       Write this help
    -q or --quiet
       Be less verbose
    -v or --verbose
       Be more verbose
    -V or --version
       Write version.
    

# Examples

Convert input.jpg into output.tif: 
    
    
    pyramidtiff -i input.jpg -o output.tif
    

Check if file.tif is already a pyramid, tiled tif: 
    
    
    pyramidtiff -c file.tif
    

Execute a command if a file is a pyramid, tiled tif: 
    
    
    pyramidtiff -c file.tif && echo file.tif is a pyramid, tiled tif

---

_Source: [https://wiki.freepascal.org/pyramidtiff](https://web.archive.org/web/20250314134734/https://wiki.freepascal.org/pyramidtiff)_
