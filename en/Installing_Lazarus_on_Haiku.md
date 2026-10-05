# Installing Lazarus on Haiku

│ **English (en)** │  **[español (es)](</Installing_Lazarus_on_Haiku/es> "Installing Lazarus on Haiku/es")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Work in progress ...

Lazarus requires Qt under Haiku. Qt is not installed by default under Haiku. You need to install package available from [this site](<http://qt-haiku.ru/>). Currently, there is no binary package to install Lazarus. You will have to compile Lazarus from sources. 

## Contents

  * 1 Installing from sources
    * 1.1 Building Qt under Haiku
    * 1.2 Building Qt4 binding for Free Pascal under Haiku
    * 1.3 Building Lazarus under Haiku
      * 1.3.1 Required tools
      * 1.3.2 Getting sources
      * 1.3.3 Compiling Lazarus



## Installing from sources

### Building Qt under Haiku

Instructions are available there : [http://qt-haiku.ru/index.php?option=com_content&view=article&id=53&Itemid=67](<http://qt-haiku.ru/index.php?option=com_content&view=article&id=53&Itemid=67>)

Building Qt is not required. You can just download lastest binaries at [http://www.qt-haiku.ru/index.php?option=com_rokdownloads&view=folder&Itemid=60](<http://www.qt-haiku.ru/index.php?option=com_rokdownloads&view=folder&Itemid=60>) and uncompress them to /boot/. 

In order to compile the Qt4 binding below, you should get the development package. 

### Building Qt4 binding for Free Pascal under Haiku

Download the source from <http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>. The current binding is "V2.5 (Qt 4.5.X, compatible till Qt 4.8.X)". Choose the entry "Library source" under Linux X11. 

  
You need to add QMAKE_CXXFLAGS to Qt4Pas.pro before build libQt4Pas: 

QMAKE_CXXFLAGS += -mstackrealign -mincoming-stack-boundary=2 

  


On previous versions (at least 2.1), it was necessary to edit some files before compiling the package under Haiku : 
    
    
    In Qt4Pas.pro, change the supported version according to the version of Qt you have downloaded (It also works with Qt 4.8.0).
    Then, add ":!Haiku" at line 50 inside Qt4Pas.pro :
     unix:!embedded:!mac**:!haiku** :PLATFORM = BINUX
    

Finally, run : 
    
    
    qmake
    make
    make install
    

You should have libQt4Pas.so in /boot/common/lib/. The last step is to add a link to this library in /boot/develop/abi/c86/gcc4/lib. 

More information about this binding is available here : [Qt4 binding](<Qt4_binding.md> "Qt4 binding")

### Building Lazarus under Haiku

#### Required tools

fpc 2.4.0 for Haiku is not suitable to build Lazarus. Some bugs were fixed while porting Lazarus to Haiku. 

The 2.4 fix branch or trunk should compile Lazarus. 

Current official version (2.6.0) is able to compile Lazarus. Binaries for Haiku are available at : <http://freepascal.org/down/i386/haiku.var>. 

#### Getting sources

As usual : 
    
    
    svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus
    

#### Compiling Lazarus

Once you have a good enough compiler and the source tree, just run at the root of the source tree 

Make sure gcc 4 is the default compiler. 

Under Haiku with package management : 
    
    
    setarch x86
    

On older versions of Haiku, use : 
    
    
     setgcc gcc4
    

then : 
    
    
    make LCL_PLATFORM=qt

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_Haiku](https://web.archive.org/web/20250312232746/https://wiki.freepascal.org/Installing_Lazarus_on_Haiku)_
