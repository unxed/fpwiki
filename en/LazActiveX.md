# LazActiveX

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Installing
  * 3 Usage
    * 3.1 TActiveXContainer late binding
    * 3.2 TActiveXContainer early binding
    * 3.3 Creating visual ActiveX components from type libraries or objects



## Overview

The LazActiveX package contains the TActiveXContainer component and the IDE integration of the necessary tools to create ActiveX components from a type library or directly from the object (exe or dll). ActiveX is a Microsoft Windows technology and can only be used on that platform. Cross compilation to windows is possible but the typelib importer used to create the COM bindings works only on Windows (wine not tested). 

## Installing

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** LazActiveX needs FPC newer than 2.6.1. Importing type libraries only works on Windows XP or newer.

From the 'Package'/'Install/Uninstall Packages ...' window, select LazActiveX 0.1, click 'Install Selected' and then 'Save and Rebuild IDE'. Alternatively, open the LazActiveX.lpk package and click on 'Use >>'/'Install'. 

When the installation succeeds, a new 'ActiveX' tab is added to your component palette. 

[![LazActiveX-Install1.png](https://wiki.freepascal.org/images/9/91/LazActiveX-Install1.png)](</File:LazActiveX-Install1.png>)

## Usage

There are 3 different ways of using ActiveX components: 

  1. drop the TActiveXContainer on a form and assign the ActiveX ClassName to the OleClassName property. Suitable for late binding and without direct event support.
  2. import the ActiveX type library, drop the TActiveXContainer on a form, instantiate the ActiveX object and assign it to the ComServer property. Suitable for early binding and with event support.
  3. create a new component from the type library or the object and drop that new component on the form. This also uses early binding and has full event support. Event handlers can be assigned from the IDE.



### TActiveXContainer late binding

Although the theory is quite complex, late binding is the fastest way to get an ActiveX component running. 

Late binding refers to the fact that the interfacing to the object is all done at run-time. References to objects are stored as dispinterfaces in variants and the methods and properties of the objects are "discovered" at run-time. 

The compiler generates all the run-time magic for you, but the downside is that the compiler isn't able to help you in finding the names of methods or properties at design time. 

Good documentation for the ActiveX object is very important (sample code for VB can easily be translated to Pascal). All errors in method names and their parameters are run-time errors. The run-time "discovery" also adds quite some overhead. 

**Example:** Embed VLC player in a form. VLC has to be installed !! 

  * Drop a TActiveXContainer, a TButton and a TFileNameEdit on a form.
  * Resize and position TActiveXContainer to your liking and enter 'VideoLAN.VLCPlugin.2' as OleClassName, set Active to true. This will give a form that resembles the following:



[![LazActiveX-VLC-Form.png](https://wiki.freepascal.org/images/3/37/LazActiveX-VLC-Form.png)](</File:LazActiveX-VLC-Form.png>)

  * create an Onclick event for the button and enter


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      actx: variant;
      fn: widestring;
    begin
      actx := ActiveXContainer1.ComServer;
      fn := UTF8Decode('file:///'+StringReplace(FileNameEdit1.FileName, '\', '/', [rfReplaceAll]));
      // or using uriparser (add uriparser unit to uses clause)
      // fn := UTF8Decode(UriParser.FilenameToURI(FileNameEdit1.FileName));
      actx.playlist.add(fn);
      actx.playlist.play;
    end;
    

  * Run the program, select a movie and click the button to view the movie. Note that VLC and gdb seem to cause some problems (slow start-up in the best case, hangs or crashes). Run the program preferably outside the IDE.



### TActiveXContainer early binding

Early binding means that all interfaces are known to the compiler. So we need to create the pascal bindings for the ActiveX object. The importtl.exe tool in the fpc utils directory does that for you and creates coclasses and event listeners to facilitate the use of the objects in pascal. A GUI for the importtl.exe tool is integrated with the LazActiveX package and is accessible in the Tools menu: Import Type Library. 

[![LazActiveX-Importtl.png](https://wiki.freepascal.org/images/4/47/LazActiveX-Importtl.png)](</File:LazActiveX-Importtl.png>)

Check 'Convert dependent typelibs' to convert also the typelibs for the COM objects that are eventually used by the selected object. Clicking OK will start the typelib import and create new units in the IDE. The units have the following naming: LibraryName_MajorVersion_MinorVersion_TLB. Ex: Access_9_0_TLB. 

    **Delphi users** : Delphi unit naming does not use version numbers.

Instantiating an ActiveX object is done at run-time by calling the Create method for the appropriate CoClass created for the library. Some ActiveX libraries expose several objects and multiple CoClasses are created. Consult the documentation for the ActiveX library in case of doubt. Assigning the instantiated ActiveX object to the ComServer property of an TActiveXContainer on a form and setting it's active property to true is all that is needed to embed it in the application. 

**Example:** Embed VLC player in a form. VLC has to be installed !! 

\- Drop a TActiveXContainer, a TButton and a TFileNameEdit on a form. Resize and position TActiveXContainer to your liking 

\- Open 'Import Type Library' from the tools menu and select 'axvlc.dll' in your VLC installation directory. Click OK. The AXVLC_1_0_TLB unit is created. 

\- Add AXVLC_1_0_TLB to the uses clause. 

\- Create an OnClick event for the button and an OnCreate event for the form. Enter the following code: 
    
    
    uses ..., variants, ....
    
    var
      VLC:IVLCControl2;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var fn: widestring;
    begin
     fn := UTF8Decode('file:///'+StringReplace(FileNameEdit1.FileName, '\', '/', [rfReplaceAll]));
     VLC.playlist.add(fn,'',vararrayof([':aspect-ratio=16:9']));
     VLC.playlist.play;
    end; 
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      VLC:=CoVLCPlugin2.Create;
      ActiveXContainer1.ComServer:=VLC;
      ActiveXContainer1.Active:=true;
    end;
    

\- Run the program, select a movie and click the button to view the movie. 

  


The typelib importer adds also event support for ActiveX that generate them. In that case, alongside the CoClass a TEventSink descendant is defined that implements the ActiveX object and all its OnEvent properties. Use the TEventSink descendant instead of the CoClass if you want to use the events. Its name starts with TEvs followed by the class name. 

**Example:** Internet Explorer in a form with event support. 

\- Drop a TActiveXContainer, a TButton, TLabel and a TEdit on a form. Resize and position TActiveXContainer to your liking 

\- Open 'Import Type Library' from the tools menu and select 'C:\WINDOWS\system32\ieframe.dll'. Click OK. The SHDocVw_1_1_TLB unit is created. 

\- Add SHDocVw_1_1_TLB to the uses clause. 

\- Create an OnClick event for the button and an OnCreate event for the form. 

\- Create in class TForm1 the private procedure OnStatusTextChange(Sender: TObject;Text_:WideString); 

\- Enter the following code: 
    
    
    var
      Browser:TEvsWebBrowser;
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      url,onull:Olevariant;
    
    begin
      url:=Utf8decode(Edit1.Text);
      onull:=NULL;
      Browser.ComServer.Navigate2(url,onull,onull,onull,onull);
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Browser:=TEvsWebBrowser.Create(Self);
      ActiveXContainer1.ComServer:=Browser.ComServer;
      ActiveXContainer1.Active:=true;
      Browser.OnStatusTextChange:=@OnStatusTextChange;
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      Browser.Free;
    end; 
    
    procedure TForm1.OnStatusTextChange(Sender: TObject; Text_: WideString);
    begin
      Label1.Caption:=UTF8Encode(Text_);
    end;
    

\- Run the program, enter an URL in the Edit control and click the button to navigate to the URL. Label1 will show the status messages sent by the browser. 

\- To access the document object (DOM) you have to import also the typelib from C:\WINDOWS\system32\mshtml.tlb and add the created MSHTML_4_0_TLB to the uses clause. To display for example the contents of the <body> tag of the downloaded page, add a TMemo and a second TButton to the form and add the following code: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    var  Doc: IHTMLDocument2; //defined in MSHTML_4_0_TLB
    begin
      Doc := Browser.ComServer.Document as IHTMLDocument2;
      Memo1.Lines.Text := utf8encode(Doc.parentWindow.document.Get_body.innerHTML);
    end;
    

Note that CoClasses are not destroyed. As all COM objects they are reference counted and destroy themselves when the last reference goes out of scope. The TEventSink descendant has to be destroyed. 

### Creating visual ActiveX components from type libraries or objects

Instead of using the TActiveXContainer component directly the importtl tool can create TActiveXContainer descendants for every ActiveX object embedded in the type library and create a lpk package for you. The descendants have full support for events generated by the objects. To create a package with the ActiveX components open the 'Import Type Library' window in the 'Tools' menu. Select the library to import, check 'Create package' and eventually 'Convert dependent typelibs', click on 'OK' to create the package. You will be prompted for a directory to store the package. Install the new package. After the Lazarus rebuild you'll find the new components in the ActiveX tab of the component palette. 

[![LazActiveX-Install2.png](https://wiki.freepascal.org/images/2/21/LazActiveX-Install2.png)](</File:LazActiveX-Install2.png>)

**Example** : Embed VLC player in a form. VLC has to be installed !! 

\- Open 'Import Type Library' from the tools menu and select 'axvlc.dll' in your VLC installation directory. Check 'Create package'. Click OK. Select a directory to store the package. The AXVLC_1_0_TLBP package and all the needed units are created. Install the AXVLC_1_0_TLBP package and rebuild the IDE. 

\- Drop a TAxcVLCPlugin2 (from ActiveX component tab), a TButton and a TFileNameEdit on a form. 

\- Set TAxcVLCPlugin2.Active to true and resize and position TActiveXContainer to your liking. 

\- Create an Onclick event for the button and enter: 
    
    
    uses variants, URIParser;
    ...
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      AxcVLCPlugin2_1.OleServer.playlist.add(URIParser.FilenameToURI(FileNameEdit1.FileName),'',vararrayof([':aspect-ratio=16:9']));
      AxcVLCPlugin2_1.OleServer.playlist.play;
    end;
    

\- Run the program, select a movie and click the button to view the movie. 

Event handlers for the components can be created directly from the Object Inspector. (Note the VLC version 1.1.5.0 typelib advertises events but fails to establish a connection point. This appears to be a bug in VLC). 

    **Delphi users** : Delphi creates a stub for every method and property in its ActiveX components. importtl does not add this additional layer and the AxctiveX methods are called through the components OleServer property. Fe. AxcVLCPlugin2_1.OleServer.playlist.play is the equivalent to VLCPlugin21.playlist.play in Delphi.

---

_Source: [https://wiki.freepascal.org/LazActiveX](https://web.archive.org/web/20250321040514/https://wiki.freepascal.org/LazActiveX)_
