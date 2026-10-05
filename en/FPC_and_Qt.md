# FPC and Qt

│ **English (en)** │  **[русский (ru)](<../ru/FPC_and_Qt.md>)** │

## Contents

  * 1 Introduction
    * 1.1 Qt3
    * 1.2 Qt/Embedded
    * 1.3 Qt4
  * 2 How it works
  * 3 Example



## Introduction

There are a number of Qt bindings available: 

### Qt3

[A QtC based binding by Theo](<http://www.theo.ch/kylix/Qt3pas.zip>)

[Another QtC based binding by Andreas](<http://andy.jgknet.de/oss/qt/qt3forFPC/>)

The first one aims on linux/unix users while the second one is for win32. 

### Qt/Embedded

The FPC arm port and this [Qt/E binding](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/qtedemo.html>) allow graphical programming on devices such as the Zaurus 

### Qt4

More recently a small [Qt4 Binding](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>) is available. It does not depend on the obsolete QtC library. The [Qt4 binding](<Qt4_binding.md> "Qt4 binding") page will provide more information on how the binding works and how to use it. 

## How it works

FPC still can't use C++ classes natively so these interface units use the same scheme as e.g. the official C interface to Qt does: there are wrapper procedures written which export an procedural interface. This procedural interface is wrapped by an object pascal interface again to make usage easier. 

This might sound like an huge overhead but these wrapper are mainly necessary to hide name mangling differences so there shouldn't be a noticable speed decrease in real world applications. 

## Example

This piece of code is taken from the second package mentioned above. 
    
    
     var
       app: QApplicationH;
       btn: QPushButtonH;
     begin
       // create static ( interfaced handled ) QApplicationH
       app := NewQApplicationH(ArgCount, ArgValues).get;
       // due to a bug in fpc 1.9.5 the WideString helper methods with default parameter are disabled
       //btn := NewQPushButtonH('Quit', nil).get;
       btn := NewQPushButtonH(qs('Quit').get, nil, nil).get;
       btn.setGeometry(100, 100, 300, 300);
       btn.show;
       // override the virtual eventFilter method of btn
       btn.OverrideHook.eventFilter := @TTest.MyEventFilter;
       // and install the btn as it's own eventFilter
       btn.installEventFilter(btn);
       // connect Qt signal to Qt slot
       QObjectH.connect(btn, SIGNAL('clicked()'), app, SLOT('quit()'));
       ...
    

If you know how Qt is used in C++, you can see that there is not much difference.

---

_Source: [https://wiki.freepascal.org/FPC_and_Qt](https://web.archive.org/web/20250515002407/https://wiki.freepascal.org/FPC_and_Qt)_
