# Qt Contest 2008

The Lazarus Qt interface was submited as an entry to the Qt Contest 2008 form Qt Centre. This page contains instructions to build Lazarus with the Qt interface and also about using it. 

Lazarus svn: 
    
    
    svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus

Instructions: 

To correctly build and run Lazarus for Qt it is necessary to have installed: Free Pascal Compiler >= 2.2.2, The Free Pascal Source code, GNU binutils, Qt >= 4.3 and the Pascal bindings for Qt. 

Free Pascal 2.2.2 for your platform can be downloaded here: 

<http://www.freepascal.org/download.var>

The Free Pascal source code can be downloaded here (search for fpc sources for your platform): 

<http://snapshots.lazarus.shikami.org/lazarus/>

The Library containing the Pascal bindings for Qt can be downloaded here (download the library binary for Qt 4.3): 

<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>

After all those items are installed, lazarus for Qt can be compiled with: 
    
    
    make LCL_PLATFORM=qt

And it can be started with: 
    
    
    ./lazarus-ide

With Lazarus you should be able to quickly develop GUI application, because it posesses an advanced visual designed, which is well integrated with the Lazarus Component Library, a cross-platform GUI development library. This library is able to use several underlying libraries to do the actual drawing and managing of widgets, and in this version Qt >= 4.3 is used, both for Lazarus and for the GUI applications that it generates. Lazarus has advanced code factoring facilities which speed development considerably, and the Free Pascal compiler is very powerful, being able to develop any kind of applications, from operating systems to database applications. 

## See also

  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- Extra instructions for running Lazarus with Qt 
  * [Lazarus Documentation](<Lazarus_Documentation.md> "Lazarus Documentation") \- Extra instructions for using Lazarus

---

_Source: [https://wiki.freepascal.org/Qt_Contest_2008](https://web.archive.org/web/20190609024135/https://wiki.freepascal.org/Qt_Contest_2008)_
