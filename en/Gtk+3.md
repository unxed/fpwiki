# Gtk+3

[![GTK+ logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/GTK_logo.svg/50px-GTK_logo.svg.png)](</File:GTK_logo.svg>)

This article applies to [GTK+ Widgetsets](</Category:GTK_Widgetsets> "Category:GTK Widgetsets") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Info
  * 2 SVN
  * 3 FAQ
    * 3.1 My simple program crashes immediately
    * 3.2 Where is class X
    * 3.3 Some functions are missing like g_signal_connect
    * 3.4 Where are GtkHbox and GtkVBox



# Info

Gtk+3.0 bindings are being developed in the Lazarus-ccr 

They are based on the headers from gtk+3.8.6 which can be downloaded here: 

<http://ftp.gnome.org/pub/gnome/sources/gtk+/3.8/gtk+-3.8.6.tar.xz>

# SVN

The current result can be found in the Lazarus-CCR at this svn address: 
    
    
     svn checkout <svn://svn.code.sf.net/p/lazarus-ccr/svn/bindings/gtk3/> gtk3
    

-or- 
    
    
     svn checkout <https://svn.code.sf.net/p/lazarus-ccr/svn/bindings/gtk3/> gtk3
    

Or you can browse the files [here](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/bindings/gtk3/>)

These files were generated with [gir2pascal](<gir2pascal.md> "gir2pascal") which should be updated to generate correct bindings if errors are found. 

# FAQ

## My simple program crashes immediately

You probably need to include the Math unit and put 
    
    
     SetExceptionMask([exDenormalized,exInvalidOp,exOverflow,exPrecision,exUnderflow,exZeroDivide]);
    

before gtk_init. 

## Where is class X

Some classes have been deprecated in Gtk3 and the generator skips deprecated classes and functions. 

## Some functions are missing like g_signal_connect

Many functions are actually c macros and are unavailable in the introspection files. In the case of g_signal_connect it is a macro around g_signal_connect_data. 

## Where are GtkHbox and GtkVBox

In Gtk 3 GtkHBox and GtkVBox are combined in GtkBox. In gtk_box_new you specifiy an orientation.

---

_Source: [https://wiki.freepascal.org/Gtk%2B3](https://web.archive.org/web/20250114033235/https://wiki.freepascal.org/Gtk%2B3)_
