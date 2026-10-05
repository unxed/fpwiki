# gtk2

The package GTK2 is a header to the GTK 2.x libraries. The headers mostly try to be compatible to 2.6 with some 2.8 extensions. 

It is expected that the next lazarus version (0.9.28) will default to gtk2. Currently, gtk1 is still the default, though gtk2 is already in wide use. 

Note that while GTK(2) is mostly used on Unix, it is known to work on Windows too, though to our knowledge not tested regularly. 

Reportedly (bug 12359) the following libraries are required for windows: 
    
    
    libgdkglext-win32-1.0.0.dll
    libglade-2.0-0.dll
    

  


# fpu problem under windows

A documented problem [bug 12359](<http://bugs.freepascal.org/view.php?id=12359>) states that the default FPU mask of FPC under windows can be a problem for the GTK libraries. The gtk_demo example is affected by this. 

See also 

  * [fpc-devel msg](<http://www.mail-archive.com/fpc-devel@lists.freepascal.org/msg12237.html>)



This can be avoided by setting the FPU mask to $133F as first line in your program. 
    
    
     Set8087CW($133F); 
    

This might have an affect on Delphi compatibility of floating point operations though.

---

_Source: [https://wiki.freepascal.org/gtk2](https://web.archive.org/web/20210306111827/https://wiki.freepascal.org/gtk2)_
