# nvidia-widgets

## Contents

  * 1 General
  * 2 License
  * 3 Widgets
  * 4 Context and backend
  * 5 Documentation
  * 6 SVN
  * 7 Todo list
  * 8 About IMGUI
  * 9 More information



## General

This is the FPC port of the nvidia-widget set ([Internet Archive Link](<https://web.archive.org/web/20160811073009/https://code.google.com/p/nvidia-widgets/downloads/detail?name=nvidia-widgets-1.0.0.tar.gz>)). This widgetset is lightweight and extremely useful for projects that do not want to add a dependency to some large widget set e.g. GTK2 or QT. 

Originally ported by Darius Blaszyk in June 2011. The code works out of the box with FPC and does not need any 3rd party utilities. 

[![NvWidget MacOS 386.png](https://wiki.freepascal.org/images/c/c9/NvWidget_MacOS_386.png)](</File:NvWidget_MacOS_386.png>) [![NvWidgets WinXP32.png](https://wiki.freepascal.org/images/a/a5/NvWidgets_WinXP32.png)](</File:NvWidgets_WinXP32.png>)

## License

The original code was released under the MIT license. This code, as it's derived work, will therefore also be released under MIT license. As far as I understand GPL easily mixes with MIT, so if your project is GPL you can use it as you would with a GPL library. 

## Widgets

This widgetset comes with a layoutmanager. This means that you don't need to specify screen coordinates when you define the widgets, but you specify how you want to layout them and then just add the widgets as you like. You can embed multiple layout panels in each other to build complex layouts. 

The most important widgets are implemented, but more are allways welcome; 

  * Label
  * Button
  * Checkbutton
  * Radiobutton
  * Horizontal slider
  * Listbox
  * Combobox
  * Line edit
  * Panel
  * Textureview



## Context and backend

This widgetset is very modular, which is expressed by the fact that both the context as the backend are separated into it's own classes. Currently the GLut context is implemented and for the backend OpenGL obviously. Adding a new context or backend is very easy. All you need to do is implement the UIContext or UIPainter classes. 

## Documentation

The widgetset library is documentated and online available from [here](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/nvidia-widgets/docs/index.html>). Though not 100% complete it does describe the library to some extent. Patches to improve documentation are welcomed. There's also a [CHM](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/nvidia-widgets/docs/nvwidgets.chm>) version available. 

## SVN
    
    
     svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/nvidia-widgets> nvidia-widgets
    

or ViewCVS interface 
    
    
    <http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/nvidia-widgets/>
    

## Todo list

There is a number of todo items, but here are some important ones; 

  * unicode support
  * ~~fix freetype support + create a base class and add glutfont support back again~~
  * ~~implement fpmake and a config.inc file where you can conditionally compile the desired painter and context~~
  * ~~split the base painter and context~~
  * ~~re-organise the sources to be more logical~~
  * fix panel widget (on folding it crashes)
  * improve fpdoc documentation of library
  * cleanup API (eg unused parameters, move the get_WIDGET_ functions to the base object)
  * implement scrolling panels
  * implement SDL context class
  * implement TnvApplication class that handles all context signals



## About IMGUI

This widgetset is an immediate mode graphical user interface (IMGUI). This means that this widgetset does not retain widget information outside the paint loop. The advantage of this is that it is very lightweight and easily expandible. 

## More information

IMGUI : <https://web.archive.org/web/20140110175254/https://mollyrocket.com/forums/viewforum.php?f=10>

---

_Source: [https://wiki.freepascal.org/nvidia-widgets](https://web.archive.org/web/20250114033200/https://wiki.freepascal.org/nvidia-widgets)_
