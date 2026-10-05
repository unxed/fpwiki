# Device Contexts and GDI objects in the LCL interfaces

## Contents

  * 1 Warning and overview
  * 2 Why are TFont, TPen, TBrush faster than accessing the LCL interfaces directly?
  * 3 Device Context
  * 4 GetDC
  * 5 ReleaseDC
  * 6 CreateFontIndirectEx
  * 7 SelectObject
  * 8 DeleteObject
  * 9 SaveDC
  * 10 RestoreDC



## Warning and overview

This page is about the graphic Handle objects of the LCL interfaces: **Device contexts** (alias DC, canvas handles) and **GDI objects** (alias font handles, pen handles, brush handles, bitmap handles, palette handles, region handles). 

The TCanvas, TFont, TBrush, TPen use them too and handle all the bookkeeping automatically. So, it is better and often faster to use the LCL classes instead of the LCL interface handles. 

The LCL interfaces work all different here, especially if you use these functions not like TCanvas, TFont, TPen, TBrush and TBitmap do. If you miss a function, please ask first on the mailing list, before trying these functions. 

## Why are TFont, TPen, TBrush faster than accessing the LCL interfaces directly?

  * They use resource caches. This means if you allocate a font with the same attributes a second time, it will reuse the handle. These caches work global. So, even if you free a TFont, the handle is still in the cache and can be used if you create a TFont with the same attributes.
  * The LCL interfaces are tested and optimized for these objects.



Probably you will only get more speed by accessing directly the interfaces (the gtk, the winapi, the qt, ...). 

The only reason to access the LCL interfaces is to quickly port some 'optimized' Delphi code. 

## Device Context

Normally a Device Context (DC) is a handle (=LCL interface object) to draw on a TWinControl handle. A TCanvas.Handle is a DC. 

## GetDC

For delphi compatibility the parameter 0 creates a DC for the current screen with some default values. Otherwise it creates a DC for the given TWinControl handle. The LCL interface provides default GDI objects (font, pen, brush, region, palette and bitmap). The current GDI objects can be changed with SelectObject and should be restored before releasing the DC, either with SelectObject or RestoreDC. Otherwise the DeleteObject can fail, which can create mem leaks. 

## ReleaseDC

This frees the DC. It is allowed to release a DC, where the default GDI objects have not been restored. The LCL interface will now free the default GDI objects of the DC. If the application selected the default GDI objects of a DC1 in another DC2 and frees DC1 - which is a bug in the application - the behavior depends on the LCL interface. The winapi interface uses global stock objects and will ignore the error. The gtk interface will raise an exception. 

## CreateFontIndirectEx

## SelectObject

This replaces a GDI object in a DC and returns the old GDI object. You can select a GDI object in any number of DC at the same time. The application should unselect the GDI object before calling DeleteObject. For example: 
    
    
    FontHandle1:=CreateFontIndirect...;
    OldFontHandle:=HFONT(SelectObject(aDC,FontHandle1));
    ... some text operations ...
    SelectObject(aDC,OldFontHandle);
    DeleteObject(FontHandle1);
    

  


## DeleteObject

This frees a GDI object. If a GDI object is freed, that is still selected in a DC, it fails and returns 0. This means every LCL interface must bookkeep if a GDI object is selected in a DC. 

## SaveDC

This saves the current state of a DC to an internal stack. The references to the GDI objects are stored, not their values. See below RestoreDC. 

## RestoreDC

This loads a state from the internal DC stack. It undoes all SelectObject calls. Although the winapi supports restoring several states at once, most LCL interfaces will not support this. 

Example: 
    
    
    Font:=CreateFontIndirect...;
    DCIndex:=SaveDC(DC);
    SelectObject(DC,Font);
    RestoreDC(DCIndex);
    DeleteObject(Font);

---

_Source: [https://wiki.freepascal.org/Device_Contexts_and_GDI_objects_in_the_LCL_interfaces](https://web.archive.org/web/20240913182132/https://wiki.freepascal.org/Device_Contexts_and_GDI_objects_in_the_LCL_interfaces)_
