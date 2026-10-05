# Creating LCL Control From Libraries

## Contents

  * 1 About
  * 2 Examples
    * 2.1 Usage example 1 (library)
    * 2.2 Usage example 2 (library)
    * 2.3 Usage example 3 (application)
  * 3 Technical details
    * 3.1 Passing strings
    * 3.2 How does it work?
    * 3.3 What type of LCL controls?
  * 4 FAQ/Troubleshoot
    * 4.1 I get "class not found" errors
    * 4.2 Where do I download the sources?
  * 5 Copyright
  * 6 License



## About

Due to several problems, LCL components and ansistrings cannot be passed between a library (dll/so/dynlib) and an application (exe/app). 

This interface provides a simple API between the library and the application to create LCL UI controls application-side, however controlled by dummies library-side. 

## Examples

### Usage example 1 (library)
    
    
    procedure init; stdcall;
    var myBtn:TRHandle;
    begin
      myBtn:=RCreate(StrToRstr('TButton'));
      RSetStrProp(myBtn,StrToRstr('Caption'),StrToRstr('Hello world!'));
    end;
    
    exports init;
    

### Usage example 2 (library)
    
    
    procedure init(RApplication,RParent:TRHandle); stdcall;
    var winCtrl:TRWinControl;
    begin
      winCtrl:=TRWinControl.Create(RApplication);
      winCtrl.Left:=20;
      winCtrl.Top:=10;
      winCtrl.Width:=100;
      winCtrl.Height:=24;
      winCtrl.Parent:=RParent;
    end;
    
    exports init;
    

### Usage example 3 (application)
    
    
    type
      TDllInit1=procedure(intr:TRWinControlInterface); stdcall;
      TDllInit2=procedure(intr:TRWinControlInterface; Appl,Parnt:TRHandle); stdcall;
    
    var
      lib:TLibHandle;
      init1:TDllInit1;
      init2:TDllInit2;
    begin
      RegisterClass(TButton);
      RegisterClass(TWinControl);
    
      // library example 1
      lib:=LoadLibrary('library1.'+SharedSuffix);
      init1:=TDllInit(GetProcAddress(lib,'init'));
      init1(FillRWinControlInterface);
      FreeLibrary(lib);
    
      // library example 2
      lib:=LoadLibrary('library2.'+SharedSuffix);
      init2:=TDllInit(GetProcAddress(lib,'init'));
      init2(FillRWinControlInterface,TRHandle(Application),TRHandle(Panel1));
      FreeLibrary(lib);
    end.
    

## Technical details

### Passing strings

Strings cannot be usually passed between a library and an application. Since this system requires passing strings, a solution has been included: 

  * You can either use simple PChars 
    * Fast but not suitable for binary data (especially containing null characters)
  * Or use the binary string system 
    * This probably is slower then just using PChars but it allows passing any type of data up to 2Gb.



To make it easy to switch, just type this switch 
    
    
    {$DEFINE USE_BIN_STR}
    

on the very top of your "rwincontrol" unit. If this switch is set, binary strings support is enabled. If not, the more conventional PChars are used. 

### How does it work?

A set of functions in the application are passed to the library as pointers in a record (TRWinControlInterface). Each time a library needs to change or create a control these functions are called. They basically make use of RTTI to provide this functionality. 

### What type of LCL controls?

Actually, it's not just LCL but any other class you want. You just have to register your class application-side and provide a suitable wrapper library-side (though it is not required). To register a class, put something like the following code in the initialization section of your program: 
    
    
    RegisterClass(TMyClass);
    

## FAQ/Troubleshoot

#### I get "class not found" errors

You have to register that class application-side. See #What type of LCL controls?

#### Where do I download the sources?

[View Source Code](<https://www.dropbox.com/sh/fanban54tb31p5e/AACSIXObpVfAIsinz0Sbbzkma?dl=0>) or [Download Zip](<https://www.dropbox.com/sh/fanban54tb31p5e/AACSIXObpVfAIsinz0Sbbzkma?dl=1>)

## Copyright

Devised and created by Christian Sciberras, with the help of the following people from #lazarus-ide IRC (alphabetical): 

  * Andreas '\pub\bash0r' Schneider
  * Dmitry 'skalogryz' Boyarintsev
  * Marc 'giantm' Weustink
  * Vincent 'fpcfan' Snijders



## License

© 2009 Covac Software. You may copy, store, modify and sell this software/source as long as you keep copyright comments in the source code intact.

---

_Source: [https://wiki.freepascal.org/Creating_LCL_Control_From_Libraries](https://web.archive.org/web/20240308084928/https://wiki.freepascal.org/Creating_LCL_Control_From_Libraries)_
