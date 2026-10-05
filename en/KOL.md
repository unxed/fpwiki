# KOL

## Contents

  * 1 Introduction
  * 2 Requirements
  * 3 Supported targets
  * 4 Example
  * 5 Tip



## Introduction

[![kolmck.png](https://wiki.freepascal.org/images/c/ca/kolmck.png)](</File:kolmck.png>)  
  
The Key Objects Library, a.k.a. KOL, is an object oriented framework created by Vladimir Kladov ([website](<http://f0460945.xsph.ru/rindex.htm>)) in 2000. It is the successor to the XCL library by the same author. The Mirror Classes Kit, a.k.a. MCK, is a set of visual components to write KOL applications in a visual design way, from within the Delphi IDE. 

The current point release is 3.23 for KOL and 3.23 for MCK. 

New versions appear with Irregular intervals, but the library is currently maintained. There is an svn repository for KOL and MCK at <https://svn.code.sf.net/p/kolmck/code/> which is usually more up to date than the point releases. 

The main reason creating this library is explained through the dissatisfaction of Vladimir and others towards ever increasing code bloat. Nobody seemed to care anymore about *efficient* coding. According to KOL users there a strong correlation between Moore's law and degrading code optimizing efficiency. 

An MCK project designed in a Delphi IDE can be recompiled with both 32 bit and 64 bit FPC 2.7.1 or later compilers without change. This way you can without having to upgrade your Delphi to a recent version: 

  * obtain a 64 bit version of a project.
  * obtain a unicode version of a Delphi project (5-2007) by simply defining -dUNICODE_CTRLS.
  * or both!



The framework centers around three basic concepts: 

  * It uses - some say old school - pascal objects, not classes.
  * Most of the widgets are basically one and the same object. Polymorphism is through constructing functions.
  * It ignores the VCL/LCL, in fact it attempts to replace them.



This has several **advantages** , some of which are: 

  * The size of your binary is very tiny, on average more than 20 times smaller than a VCL/LCL binary, but a factor 100 or more is not unusual.
  * KOL binaries (both executables and dll's) have much much faster load times and feel snappier.
  * KOL is very easy to extend once you understand its concepts. It is a fully object oriented framework.



It also has very obvious **disadvantages** , some of which are: 

  * KOL is a framework. It doesn't mix very well with classes and it can't be sensibly mixed with LCL/VCL code because you will loose the size advantage.
  * If you are not familiar with the old Object Pascal paradigm from Turbo Pascal the learning curve can be steep.
  * You will miss some functionality that has become common place, like RTTI, but most of these features are often codebloat and the reason why KOL exists in the first place.
  * Because you are often working on the same object with multiple appearances, it may be confusing that some methods or properties that you can see in the code editor do not apply.



## Requirements

  * Free Pascal compiler 2.6.4 or later for Win32
  * Free Pascal compiler 2.7.1 or later for Win64
  * [KOL 3.23 or later](<http://f0460945.xsph.ru/rindex.htm>) for win32
  * The Mirror Classes Kit a.k.a. MCK for those who want to develop using a visual designer like Lazarus or Delphi.
  * [Release preview of KOL 3.23-64](<https://yadi.sk/d/Z0uMP31_67nJY>) for KOL win32/64
  * [See the KOL CE page](<KOL-CE.md> "KOL-CE") for win-ce development and requirements.
  * Not required, but recommended is a copy of FPC's RTL and/or Delphi's RTL sources



## Supported targets

  * All 32-bit Windows: from Windows 95 to Windows 8.1.
  * All 64-bit x86_64 from Windows 2000 to Windows 8.1.
  * [Windows CE](<WinCE_port.md> "WinCE port") based PocketPC and Smartphones. More information on the [KOL CE page](<KOL-CE.md> "KOL-CE")



## Example

Let's illustrate KOL with a simple example. A form, a button and some action. You will notice that the code resembles that of a LCL/VCL application and basically: It does exactly the same! The main difference is the use of objects and pointer to objects instead of classes and the use of constructing functions instead of constructors. 

Here's the main program file: 
    
    
     program koldemo;
     {This line makes us FPC and Delphi compatible}
     {$IFDEF FPC}{$MODE DELPHI}{$H+}{$ENDIF}
     {$APPTYPE GUI}
     uses
      Kol,
      koldemo1 in 'koldemo1.pas';
     
     begin
      NewForm1( Form1, nil);
      Run(Form1.form);
     end.
    

  
And here is the main unit, again, you will notice it is very similar to LCL/VCL code:  

    
    
     unit koldemo1;
     
     interface
     uses
       Windows, Messages, Kol;
     const
      cps:KolString = 'Kol project in ' + {$IFDEF FPC}'Freepascal '{$ELSE}'Delphi '{$ENDIF} +
         {$IFDEF WIN64}'64 bits '{$ELSE}'32 bits '{$ENDIF} +
         {$IFDEF UNICODE_CTRLS}'Unicode'{$ELSE}'Ansi'{$ENDIF};
     type
     
     PForm1=^TForm1;
     TForm1=object(Tobj)
       {Note both widgets are of the same type}
       Form, Button:pControl;
     public
       {The message handler for our Button}
       procedure ButtonClick(Sender:PObj); 
     end;
     
     procedure NewForm1( var Result: PForm1; AParent: PControl );
     
     var
       Form1:pForm1;
     
     implementation
     
     procedure NewForm1( var Result: PForm1; AParent: PControl );
     begin
       New(Result,Create);
       with Result^ do
       begin
         {The appearance of a widget depends on its creating function}
         Form := NewForm(AParent,cps);
         Button := NewButton(Form,'&Click Me');
         {Attach the MessageHandler}
         Button.OnClick := ButtonClick;
         { The applet variable is the Application object
           Assigning the main form to it assigns the main message loop to the form.}
         Applet:=Form;
         Form.Add2AutoFree(Result);
       end;
     end;
     
     procedure TForm1.ButtonClick(Sender: PObj);
     begin
        ShowMessage('Ouch!');
     end;
     
     end.
    

So what's so special about it? Well, for one, compile this and look at the executable size. A similar program, Form + Button + ShowMessage compiled with Delphi 7 is **394 Kilobytes** and it gets much worse for progressive newer versions of Delphi:2267 Kilobytes XE7-32, 3672 Kilobytes XE7-64. For comparison: the KOL version compiled with D7 is **156** times smaller than the VCL version compiled with XE7. 

Compiled with PPC386 it renders an executable of 48128 bytes. Compiled like: 
    
    
    ppc386 -Mdelphi -CX -XX -Xs -Os koldemo.dpr 
    

Compiled with PPCX64 for Windows it renders 65536 bytes. Compiled like: 
    
    
    ppcx64 -Mdelphi -Rintel -CX -XX -Xs -Os koldemo.dpr
    

The above examples are for an ANSI application. To compile a UNICODE application, simply add the UNICODE_CTRLS define, like: 
    
    
    ppcx64 -Mdelphi -Rintel -dUNICODE_CTRLS -CX -XX -Xs -Os koldemo.dpr
    

Note that especially for 64 bit, Free Pascal renders consistently smaller code than its expensive cousin. Also note that 32 bit Free Pascal has a slightly bigger runtime payload than its cousin. The size difference disappears when your application becomes more complex. 

## Tip

For size, KOL defaults to an ugly system font. Here are multiple ways to remedy that: 
    
    
     {Add this code after the NewForm Function}
     {Release the font}
     Form.Font.ReleaseHandle;
     {Assign the default windows GUI font object}
     Form.Font.AssignHandle(GetStockObject(DEFAULT_GUI_FONT));
    

The second method is slightly more involved, but is the [|MSDN](<http://msdn.microsoft.com/en-us/library/windows/desktop/dd144925%28v=vs.85%29.aspx>) recommended way: 
    
    
     {Add this function}
     function GetGuiFont:TLogFont;
     var Metrics:TNonClientMetrics;
     begin
       Metrics.cbSize := SizeOf(Metrics);
       SystemParametersInfo(SPI_GETNONCLIENTMETRICS, SizeOf(Metrics), @Metrics, 0);
       Result := Metrics.lfMessageFont;
     end;
     {And call it after the form creation}
     Form.Font.LogFontStruct :=GetGuiFont;
    

Fonts in KOL are objects of type TGraphicTool. This object can also represent pens and brushes. We can also create a new GraphicTool object representing the GUI font: 
    
    
     {The KOL way to assign the GUI font}
     function GuiFont:PGraphicTool;
     var Metrics:TNonClientMetrics;
     begin
       Result := NewFont;
       Metrics.cbSize := SizeOf(Metrics);
       SystemParametersInfo(SPI_GETNONCLIENTMETRICS, SizeOf(Metrics), @Metrics, 0);
       Result.LogFontStruct := Metrics.lfMessageFont;
     end;
     {We can now assign the new font to the Form's font property}
     Form.Font.Assign(GuiFont);
    

And this is the result: [![kolscreenshot.png](https://wiki.freepascal.org/images/4/4a/kolscreenshot.png)](</File:kolscreenshot.png>)

---

_Source: [https://wiki.freepascal.org/KOL](https://web.archive.org/web/20250401004242/https://wiki.freepascal.org/KOL)_
