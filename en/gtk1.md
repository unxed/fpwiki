# gtk1

The package GTK1 is a header to the GTK 1.x libraries. 

## Contents

  * 1 Introduction
  * 2 Windows
  * 3 Downloading
  * 4 Using the units:



## Introduction

The [GTK+](<http://www.gtk.org/>) toolkit and its companion GDK (nowadays included into GTK) are quickly becoming (next to the [Qt toolkit](<http://www.qtsoftware.com/products/>)) a very popular X-windows programming toolkit. This makes it suitable as a base for the Lazarsu Component Library (LCL, the Lazarus equivalent of the Delphi VCL). 

The GTK1 widgetset is not actively maintained anymore. The default widgetset on *nix currently is GTK2. 

## Windows

Note that while GTK(1) is mostly used on Unix, it is known to work on Windows too, though to our knowledge not tested regularly. Reportedly ([bug 12359](<http://bugs.freepascal.org/view.php?id=12359>)) the following libraries are required for windows: 

  * libintl-1.dll
  * libgdk-0.dll
  * libgtk-0.dll
  * libglib-2.0-0.dll


  * gtkgl.dll ( required only if gtkgl is used like the gtkgldemo.pp example )



## Downloading

The gtk units are part of the Free Pascal packages, and are shipped with the official distrubution. 

The GTK libraries must be downloaded from the original [GTK+](<http://www.gtk.org/>) site. 

## Using the units:

There is an article on programming GTK by Florian Klaempfl and Michael Van Canneyt, written for the German Toolbox magazine. The original English version (in PDF format) can be viewed [here](<http://www.freepascal.org/packages/gtk/gtk-1.pdf>). The example programs source code can also be [downloaded](<http://www.freepascal.org/packages/gtk/gtkex-1.zip>). 

**Note that these examples haven't been tested with recent for a while, when you have problems, ask on the maillist**

The following is a (very) short explanation of how to use the GTK units. Most of what can be found here is already described in the articles on GTK. 

  1. In C, you only need to input gtk.h. Here you need to put gtk, and separately glib or gdk if you need them, in your uses clause.
  2. Names :
     * Pascal reserved words in types, record element names etc. have been prepended with the word 'the'. so 'label' has become 'thelabel'
     * functions have been kept with the same names.
     * for types : gdk names have been kept. Pointers to a type are defined as the type name, prepended with P. So 'GtkWidget *' becomes 'PGtkWidget'. In gtkobject, names also have been kept. In all other files, types have been prepended with T, that is, the C type 'GtkWidget' has become 'TGtkWidget'.   
This is annoying, but C is case sensitive, and Pascal is not, so there you have it...   
When translating, I've tried to stick to this scheme as closely as I could. One day, however, all will be done in a uniform manner... 
  3. Macros. Many C macros have not been translated. The typecasting macros have been dropped, since they're useless under pascal. Macros to access record members have been translated, BUT they are to be considered as READ-ONLY. So they can be used to retrieve a value, but not to store one. e.g.  
  
`function GTK WIDGET FLAGS(wid : pgtkwidget) : longint;`  
  
can be used to retrieve the widget flags, but not to set them. so things like  
  
`GTK WIDGET FLAGS(wid):=GTK WIDGET FLAGS(wid) and someflag;`  
  
will not work, since this is a function, and NOT a macro as in C. 
  4. Packed records. GCC allows you to specify members of a record in bit format. Since this is impossible in pascal, functions and procedures to get/set these elements have been made. e.g.  
  
`function width set(var a : TGtkCListColumn) : gint;`  
`procedure set width set(var a : TGtkCListColumn; width set : gint);`  
  
can be used to get or set the width in a TGtkCListColumn... in general, it's the name with ' set' appended for getting a value (set from 'a set') , and 'set ' prepended (from 'to set') and again ' set' appended. 



There is also an article on programming GTK by Florian Klaempfl and Michael Van Canneyt, written for the German Toolbox magazine. The original English version (in PDF format) can be viewed [here](<http://www.freepascal.org/packages/gtk/gtk-1.pdf>). The example programs source code can also be [downloaded](<http://www.freepascal.org/packages/gtk/gtkex-1.zip>). 

More information about programming GTK is available in the following documents: 

  * There are also some articles on programming GTK by Florian Klaempfl and Michael Van Canneyt, written for the German Toolbox magazine. The original English version (in PDF format) of these articles can be viewed here:


  *     1. [Article 1](<http://www.freepascal.org/packages/gtk/gtk-1.pdf>) and the sources of the examples can be found [here](<http://www.freepascal.org/packages/gtk/gtkex-1.zip>).
    2. [Article 2](<http://www.freepascal.org/packages/gtk/gtk-2.pdf>) and the sources of the examples can be found [here](<http://www.freepascal.org/packages/gtk/gtkex-2.zip>).
    3. [Article 3](<http://www.freepascal.org/packages/gtk/gtk-3.pdf>) and the sources of the examples can be found [here](<http://www.freepascal.org/packages/gtk/gtkex-3.zip>).
    4. [Article 4](<http://www.freepascal.org/packages/gtk/gtk-4.pdf>) and the sources of the examples can be found [here](<http://www.freepascal.org/packages/gtk/gtkex-4.zip>).
    5. [Article 5](<http://www.freepascal.org/packages/gtk/gtk-5.pdf>) and the sources of the examples can be found [here](<http://www.freepascal.org/packages/gtk/gtkex-5.zip>).
  * Mark Howe kindly donated a translation to pascal of the GTK tutorials. The result can be viewed [here](<http://www.freepascal.org/packages/gtk/tutorial/pgtk-contents.html>). The examples can be downloaded [here](<http://www.freepascal.org/packages/gtk/pgtk-examples.tar.gz>). If you want you can download the whole tutorial [Here](<http://www.freepascal.org/packages/gtk/pgtk-html.tar.gz>).

---

_Source: [https://wiki.freepascal.org/gtk1](https://web.archive.org/web/20210120075413/https://wiki.freepascal.org/gtk1)_
