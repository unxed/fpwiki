# FPC and Qt

│ **[English (en)](<../en/FPC_and_Qt.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
    * 1.1 Qt3
    * 1.2 Qt/Встраивание
    * 1.3 Qt4
  * 2 Пример



## Введение

Существует несколько возможностей использования Qt: 

### Qt3

[A QtC based binding by Theo](<http://www.theo.ch/kylix/Qt3pas.zip>)

[Another QtC based binding by Andreas](<http://andy.jgknet.de/oss/qt/qt3forFPC/>)

Первая направлена на Linux/Unix пользователей, вторая - для Win32. 

### Qt/Встраивание

[Qt/E binding](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/qtedemo.html>) \- порт FPC для ARM процессоров, который позволяет разрабатывать графические приложения для таких устройств, как Zaurus 

### Qt4

[Qt4 Binding](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>) \- Qt4 библиотека для FPC 

  


## Пример

Данный пример разработан для использования второго пакета доступа к Qt, описанного выше. 
    
    
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
    

Если вы знаете, как QT используется в C++, вы можете увидеть, что существует не так уж много различий.

---

_Source: [https://wiki.freepascal.org/FPC_and_Qt/ru](https://web.archive.org/web/20250418105102/https://wiki.freepascal.org/FPC_and_Qt/ru)_
