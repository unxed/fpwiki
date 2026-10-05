# Qt4 binding

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

This article applies to [Qt widgetset](</Category:Qt> "Category:Qt") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Download Free Pascal Qt4 Binding
  * 2 Install Free Pascal Qt4 Binding
  * 3 Links
  * 4 How to use the Qt4 binding
    * 4.1 License
    * 4.2 Demo Programs
    * 4.3 Use the Qt Documentation
      * 4.3.1 How to create a Class
      * 4.3.2 How to call a method of a Qt Class
      * 4.3.3 How to hook a Pascal Method to a signal of a Qt Class
      * 4.3.4 C++ Multiple inheritance
  * 5 Technical information
    * 5.1 Supported Platforms
    * 5.2 Type safe Qt Class Handles
      * 5.2.1 Inheritance
      * 5.2.2 Type Safe Class Parameters
      * 5.2.3 Type Safe Method Parameters
    * 5.3 Qt Enums and QFlags
    * 5.4 Floating Point Exceptions
    * 5.5 Pascalify Qt Classes
      * 5.5.1 What is the problem
      * 5.5.2 Example of the problem
      * 5.5.3 Solution:Pascalify
  * 6 FAQ
    * 6.1 I get linker error /usr/bin/ld: cannot find -lQt4Pas



# Download Free Pascal Qt4 Binding

The binding releases are uploaded to the [FPC Qt4 Binding Home Page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). 

# Install Free Pascal Qt4 Binding

The download contains a file _README.TXT_ which contains the installation instructions. 

# Links

  * [Qt Documentation](<http://doc.trolltech.com/>)
  * [Pascal Qt4 binding home page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>)
  * [Pascal Qt2/E binding home page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/qtedemo.html>)
  * [Lazarus Qt4 interface](<Qt_Interface.md> "Qt Interface")
  * [Compile instructions for Mac](<Qt_Interface_Mac.md> "Qt Interface Mac")
  * [FreePascal Qt4 Binding for Maemo](<Qt4_Maemo_Binding.md> "Qt4 Maemo Binding")



# How to use the Qt4 binding

## License

Since Qt4 is also available under LGPL, there is no difference in licensing with e.g. Gtk. LGPL allows development of both free and proprietary software without any license fees or royalties. [Nokia Qt License info](<http://qt.nokia.com/products/licensing>)

## Demo Programs

The FPC Qt4 Binding is not meant to create Qt applications in FPC. But it should allow the Lazarus LCL developers to create a Qt Widget set for Lazarus. A Lazarus Qt Widget set allows Lazarus developers to create programs for the entire set of platforms supported by Qt (Windows, Linux, Linux Embedded, OS X, ...). The provided demoes on [FPC Qt4 Binding Home Page](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>) provide insight in how to use the binding for LCL/Qt development. The lazarus user may however also choose to make some direct Qt4 binding calls to achieve things not yet supported in Lazarus/LCL such as WebKit support. 

The Qt2 Embedded binding was intended to create full FPC Qt programs and therefore included an extra advanced object oriented layer of Pascal classes and interfaces on top of the Qt classes. [Qt/E demo](<http://users.pandora.be/Jan.Van.hijfte/qtforfpc/qtedemo.html>). Though this extra layer is convenient, it was chosen not to create one for Qt4 to avoid any performance penalty or code bloat for LCL/Qt. 

## Use the Qt Documentation

### How to create a Class

Look up the Qt Class you would like to use. Suppose it is [QWidget](<http://doc.trolltech.com/4.6/qwidget.html>). Some classes provide several constructors (with different parameters). The binding provides for most constructors a corresponding factory function QtClassName_create(parameters). Store the class in a variable of type QtClassName+H. The beginning of Qt4.pas provides you with a list of all supported classes. That same list also serves to provide type safe usage of the Qt class handles. The list encodes the Class hierarchy. 
    
    
     var W : QWidgetH;
     ...
     W := QWidget_create(nil,0);
    

### How to call a method of a Qt Class

Use the Nokia Qt documentaton to find out how and when to use which method. Suppose you want to use [void QWidget::setFixedHeight ( int h )](<http://doc.trolltech.com/4.5/qwidget.html#setFixedHeight>) The corresponding Pascal fuction will be ClassName+_+MethodName. So look in Qt4.pas for the definition of QWidget_setFixedHeight 
    
    
     procedure QWidget_setFixedHeight(handle: QWidgetH; h: Integer);
    

Note how the pascal function has an additional parameter: the Qt Class instance handle of your QWidget Class. Calling the setFixedHeight method will be like this 
    
    
     QWidget_ setFixedHeight(W,100);
    

### How to hook a Pascal Method to a signal of a Qt Class

Qt classes can emit signals and other classes can hook method (slots) to these signals. The binding allows to hook Pascal methods to the Qt classes. To be able to do this, an intermediate Qt class needs to be created. For every class that emits signals, that intermediate class, referred to as hook, is provided in the binding and is called ClassName_hookH. This class can connect to all signals of its corresponding class and will call the corresponding pascal method upon occurrence of the signal. 

Example: The Qt Class QAbstractButton has a [signal clicked](<http://doc.trolltech.com/4.5/qabstractbutton.html#clicked>). The hook class of the binding is QAbstractButton_hookH. 
    
    
     var
       hook : QAbstractButton_hookH;
     ...
     hook:=QAbstractButton_hook_create(AbstractButtonHandle);
     QAbstractButton_hook_hook_clicked(hook, @Clicked);
    

The definition of hook_hook_clicked shows the type of the Pascal Method to provide: 
    
    
      procedure QAbstractButton_hook_hook_clicked(handle: QAbstractButton_hookH; hook: QAbstractButton_clicked_Event);
    

That Pascal Method type is defined as: 
    
    
     type
       QAbstractButton_clicked_Event = procedure (checked: Boolean = False) of object '''cdecl''';
    

Therefore the method Clicked should be defined as: (**Take note of the cdecl calling convention**) 
    
    
     TButton = class (TObject)
     ...
        procedure Clicked(checked: Boolean);'''cdecl''';
     ...
     procedure TButton.Clicked(checked: Boolean = False);'''cdecl''';
     begin
     end;
    

### C++ Multiple inheritance

Some Qt Classes inherit from more than 1 class. QWidget inherits from QObject and QPaintDevice. When you create in C++ a QWidget instance, there are actually two instances that get created. One instance handle for QObject and one for QPaintDevice. In the binding, the returned handle will be for the first parent (QObject). To use the methods provided through inheritance by the second parent, you need to use the second instance handle. That second instance handle is obtained by using a function called FirstClass_To_SecondClass: 
    
    
     function QWidget_to_QPaintDevice(handle: QWidgetH): QPaintDeviceH;
    

In the demo program you notice how the paint event is used to paint on a QWidget. To paint, you need to create a QPainter (like canvas) from the QWidget. The Qt documentation shows that the constructor of QPainter requires a QPaintDevice. So here is an extract of the demo 
    
    
     var PaintBox : QWidgetH;
     PaintBox:=QWidget_create();
     ...
      // In the paint method
     var CV : QPainterH;
     CV:=QPainter_create(QWidget_to_QPaintDevice(PaintBox));
    

Do not use typecasting like QPaintDeviceH(PaintBox). The compiler would complain if you did this anyhow. See type safety below. 

# Technical information

## Supported Platforms

The Qt library works on different platforms such as Linux/BSD, Windows, OS X and Embedded. Depending of the platform a number of methods are available or not. 

Example functions: 

  * Windows only function: QPixmap_toWinHBITMAP
  * Linux only function: QApplication_x11EventFilter
  * macOS only function: QApplication_macEventFilter
  * Embedded only function: QApplication_qwsEventFilter



Qt achieves this with conditional defines such as Q_WS_MAC or Q_WS_QWS. There are many more Q_XXX that allow to fine tune the Qt library for many platforms. You can e.g. configure Qt to not include CUPS support with QT_NO_CUPS. The Qt library source that is contained in the Windows Qt Download or the Embedded download is pretty similar, only very platform specific include files are omitted. The key difference is that the config files are different, e.g. in qconfig.h Q_WS_QWS is enabled in the Embedded Qt Download and not in the qconfig.h from other Platform downloads 

The pascal Qt interface library also uses a single source for all platforms by using conditional defines. Not as many defines as in Qt are used, the sole aim was to cope with the different platforms: Linux, Windows, OS X and Embedded. 

This is achieved by defining for a given platform a number of QT_XXX defines and then converting the headers with moc. Moc allows the provision of defines so that the methods that are available when these defines are set, are the only ones visible. 

The different binding platform defines are: 

Platforms  Platform define | List of Qt defines   
---|---  
BINUX  | Q_WS_X11 Q_OS_LINUX Q_OS_UNIX   
MSWINDOWS  | Q_WS_WIN   
DARWIN  | Q_WS_MAC Q_OS_UNIX   
QTOPIA  | Q_WS_QWS Q_OS_LINUX Q_OS_UNIX QT_NO_SESSIONMANAGER   
  
Binux stands for BSD/Linux. 

The binding consists of a C++ part, that uses above defines to enclose platform specific methods. To compile the code on a given platform, the respective define should be set. In the V1.X versions, the platform specific compile scripts defined the appropriate platform, in the V2.X the provided qmake project automatically sets the proper platform define. 

The binding also consists of a pascal part. In that part the different defines are enabled by using the free pascal predefined defines or they just are the same. 

Note that embedded is a special platform, that is both linux and embedded. (I did not look into the recently created wince and symbian qt variant yet). But to avoid the inclusion of any missing X11 libraries, you should not define BINUX for the compilation of the library or qt4.pas. The V1.X library source package for Linux contains 2 compile scripts, one for linux (compile_lib.sh) that defines BINUX and one for Qt/Embedded (compile_lib_qtopia.sh) that defines QTOPIA and allows calling either gcc (intel qvfb) or arm-linux-gcc (arm embedded). In the V2.X bindings, you need to set QTOPIA yourself in the qmake project. 

The lazarus lcl qt interface, also uses platform specific defines in the lcl code that calls the Qt binding. To correctly compile the lcl for embedded, you need to set the define QTOPIA. The other used defines (LINUX, WINDOWS, DARWIN=macOS) are done automatically by using existing platform specific defines provided by fpc. 

  
To compile lazarus for embedded/qtopia: 
    
    
     make PREFIX=/usr clean all LCL_PLATFORM=qt OPT="-dQTOPIA"
    

<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/Lazarus_Embedded.png> last Version verified to work is 0.9.29 

## Type safe Qt Class Handles

In Qt4.pas you will see a large list of type definitions of Qt Class handles. 
    
    
     QWidgetH = class(QObjectH) end;
       QAbstractButtonH = class(QWidgetH) end;
         QPushButtonH = class(QAbstractButtonH) end;
    

This list ensures type safety and encodes the class hierarchy. 

### Inheritance

This way you can call an inherited function using a descendant class type. The extract of the list shows that QPushButton descends from QWidget, so you can call the inherited method "width" from QWidget with your QPushButton handle 
    
    
     var Btn : QPushButtonH;
     ...
     Btn := QPushButton_create(parent);
     writeln('Button Width:',QWidget_width(Btn));
    

### Type Safe Class Parameters

In the Multiple inheritance paragraph, you have seen that you need to call a convert function to convert your QWidget to a QPaintDevice. If you didn't use the convert the compiler would complain. Suppose we omit the convert function 
    
    
     
     // Do not do this !!!
     CV:=QPainter_create(PaintBox);
    

The type safety net will make the compiler complain like this: 
    
    
    demo.pas(265,30) Error: Incompatible type for arg no. 1: Got "QWidgetH", expected "QPaintDeviceH"
    

Similar if you would not use the convert function, but would just use typecast like this 
    
    
     // Do '''not''' do this !!!
     CV:=QPainter_create(QPaintDeviceH(PaintBox));
    

The compiler will complain with: 
    
    
    demo.pas(266,21) Warning: Class types "QWidgetH" and "QPaintDeviceH" are not related
    

That typecast would not change the handle. Handles are just numbers, the returned handle/number of QWidget_to_QPaintDevice is different than the PaintBox handle/number. It indirectly points to a different Virtual Method Table. 

### Type Safe Method Parameters

The Pascal methods that can be provided to the Hook classes, need to be of the correct function type. This prevents otherwise not so pleasant stack problems when the binding c++ code calls the provided pascal method. 

## Qt Enums and QFlags

Qt uses a template class QFlags<Enum> when passing combinations of an Enum to a method. This Qt Doc [WindowType](<http://doc.trolltech.com/4.5/qt.html#WindowType-enum>) defines the enum Qt::WindowType and the associated Qt Class Qt::WindowFlags. In the header file the flags are declared like this 
    
    
     Q_DECLARE_FLAGS(WindowFlags, WindowType)
    

More about this macro: [Q_DECLARE_FLAGS](<http://doc.trolltech.com/4.4/qflags.html#Q_DECLARE_FLAGS>)

Alot of methods have a QFlags Qt Class as a parameter. Normally we would need to create a instance of this class, pass it to the method and delete it ourselves in the pascal code. To avoid all this hassle the binding generator changed all methods to allow for passing a pascal cardinal type. 

Let's sample this with the following constructor [QMainWindow](<http://doc.trolltech.com/4.0/qmainwindow.html#QMainWindow>). The constructor needs the QFlags Qt::WFlags. The Qt doc shows that this is just a synonym for Qt::WindowFlags. The following Qt4.pas extract show that the enums are declared as cardinals, so that "or" combination are allowed ("or" on pascal enums is not allowed). Sometimes these enums are used as default value of a parameter in a function. As pascal does not allow typed constants in default values, the different values of the enums are declared as untyped constants. The corresponding QFlags<> type is simply redeclared as the same type as the enum. Its name and the comments allow for easy cross reference Qt4.pas and the Qt documentation. 
    
    
     type
      QtWindowType = cardinal; //  Qt::WindowType (4)
      QtWindowFlags = QtWindowType; // QFlags<>
     const
      QtWidget = 0 { $0 };
      QtWindow = 1 { $1 };
      QtDialog = 3 { $3 };
     ... etc
    

Now lets use this on QMainWindow. The following code creates a MainWindow that stays on top and has a small toolwindow title bar: 
    
    
     MainWindow:=QMainWindow_create(nil,QtTool or QtWindowStaysOnTopHint);
    

## Floating Point Exceptions

Throughout the Qt-library, Trolltech does not check for divisions by zero for floating points. No exceptions are generated because FPEs (Floating Point Exceptions) have been disabled. However when you link an FPC program to a library, FPC re-enables FPEs. To solve this the call fedisableexcept(FE_ALL_EXCEPT) was added to Qt4.pas. Otherwise several graphic painting calls would cause exceptions. 

E.g. a call to QPainter::drawLine() will ultimately lead to a call to qt_tesselate_polygon in qpaintengine_x11.cpp where a division by zero will lead to an exception in the line: 
    
    
     edge.m = (p1.y() - p2.y()) / (p1.x() - p2.x()); // line derivative
    

## Pascalify Qt Classes

### What is the problem

The binding allows you to create Qt classes by reference but not by value. Assignment of one Qt class by value to another is impossible, because the pascal compiler does not know the interior of a C++ Qt Class. As this may not be clear, let us make an example. 

### Example of the problem

In C++ you can write: 
    
    
     QColor hourColor(127, 0, 127);
     painter.setBrush(hourColor)
    

At the end of this procedure you do not have to delete the QColor instance. The binding without pascalify of QColor would force you to write: 
    
    
      var hourColor : QColorH;//*QColor
      hourColor:=QColor_create(127,0,127); //new QColor()
      brush:=QBruh_create(hourColor);
      QColor_destroy(hourColor)
    

The destroy is rather annoying for small classes like QRect and QColor. 

### Solution:Pascalify

Pascalify means the creation of a pascal record that is binary compatible with the Qt class. Qt classes without a VMT like QColor, QRect or QRectF are easy targets. If you provide a pointer to this record, the C++ compiler will not notice the difference. So now you can simply write 
    
    
      var HourColor : TQColor;// a pascal record type compatible with QColor
      QColor_fromRGB(@hourColor,127,0,127);
      brush:=QBrush_create(@hourColor);
    

Now you do not need to free the QColor so there is less chance for a memory leak. 

Note: QRect and PRect use the same trick but with some extra adjustments for two reasons: 
    
    
    * In macOS the layout of QRect is different than the layout of TRect in pascal.
    * The TRect definition of LCL is different from Qt, with respect to the inclusion of the right and bottom line.
    

The interface adjust with +1 when going to Qt or with -1 when going to Pascal. 

Internal code sample for clarity: 
    
    
      inline void copyPRectToQRect(PRect pr, QRect &qr)
      {
      #if defined DARWIN
        qr.setLeft(((QRect *)pr)->top());
        qr.setTop(((QRect *)pr)->left());
        qr.setRight(((QRect *)pr)->bottom()-1);
        qr.setBottom(((QRect *)pr)->right()-1);
      #else
        qr.setLeft(((QRect *)pr)->left());
        qr.setTop(((QRect *)pr)->top());
        qr.setRight(((QRect *)pr)->right()-1);
        qr.setBottom(((QRect *)pr)->bottom()-1);
      #endif  
      }
    

# FAQ

## I get linker error /usr/bin/ld: cannot find -lQt4Pas

The Qt4Pas comes with a library (e.g. under Linux libqt4.so) which must be installed. See installation.

---

_Source: [https://wiki.freepascal.org/Qt4_binding](https://web.archive.org/web/20250416154737/https://wiki.freepascal.org/Qt4_binding)_
