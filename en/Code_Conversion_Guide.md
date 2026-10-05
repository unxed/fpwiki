# Code Conversion Guide

│ **[Deutsch (de)](</Code_Conversion_Guide/de> "Code Conversion Guide/de")** │  **English (en)** │  **[español (es)](</Code_Conversion_Guide/es> "Code Conversion Guide/es")** │  **[français (fr)](</Code_Conversion_Guide/fr> "Code Conversion Guide/fr")** │  **[日本語 (ja)](</Code_Conversion_Guide/ja> "Code Conversion Guide/ja")** │  **[português (pt)](</Code_Conversion_Guide/pt> "Code Conversion Guide/pt")** │  **[русский (ru)](<../ru/Code_Conversion_Guide.md> "Code Conversion Guide/ru")** │  **[slovenčina (sk)](</Code_Conversion_Guide/sk> "Code Conversion Guide/sk")** │    
****

This page is about how to port or convert existing Delphi or Kylix code to work with the [Free Pascal](<Free_Pascal.md> "Free Pascal") Compiler and Lazarus IDE. While Lazarus and the Free Pascal Compiler have aspects in common with Delphi and Kylix, they are not clones. There are a number of library call and convention differences... and in some areas, FPC is enhanced and can be more demanding about correct syntax. Please see the [Lazarus For Delphi Users](<Lazarus_For_Delphi_Users.md> "Lazarus For Delphi Users") guide for a description of some of the functional differences. 

The purpose of this guide is to document some of the specific differences that are frequently encountered during the code conversion process when translating existing code from Delphi to Lazarus. 

This document was placed into the wiki knowledge-base area so it could be easily extended by anyone who had encountered a unique problem and would like to post it for others to be aware of. 

## Contents

  * 1 Selecting a component or library for conversion
    * 1.1 Where to find code to convert
    * 1.2 Licensing
    * 1.3 Dependencies
    * 1.4 Compiler Issues
    * 1.5 Platform and OS Issues
  * 2 Doing the conversion
    * 2.1 Setting up the Lazarus environment for a conversion project
      * 2.1.1 Create a test project
      * 2.1.2 Delphi VCL, Kylix CLX Sources in Lazarus
    * 2.2 Conversion problems and solutions
      * 2.2.1 Delphi / Kylix file equivalents in Lazarus
      * 2.2.2 Converting Delphi projects/forms/units to Lazarus
      * 2.2.3 Selecting the right compiler mode
      * 2.2.4 Cross-Platform considerations
      * 2.2.5 Useful compiler variables / defines / macros
      * 2.2.6 32bit / 64 bit support
      * 2.2.7 Finding a missing identifier
      * 2.2.8 Major unit differences between Lazarus and Delphi
      * 2.2.9 Property and method differences Delphi -> FPC/LCL
      * 2.2.10 Semantical differences
        * 2.2.10.1 Order of parameter evaluation
        * 2.2.10.2 Nested procedures/functions as procedural variables
        * 2.2.10.3 Ref count release
      * 2.2.11 Syntax differences
        * 2.2.11.1 When assigning an event handling entry point, prefix it with an "@"
        * 2.2.11.2 When calling a procedure variable use this syntax: theprocname()
        * 2.2.11.3 When accessing values in a pointer to a record you must dereference first
        * 2.2.11.4 When accessing chars of an indexed string Property of an object, it must be enclosed in parentheses
        * 2.2.11.5 You must typecast pointers to actual type when using with var or function of that type
      * 2.2.12 Resources
    * 2.3 Another method to convert a Delphi or Kylix project to Lazarus
  * 3 Getting Help
    * 3.1 Documentation
    * 3.2 Peer support
    * 3.3 Knowledge bases
  * 4 Packaging and Releasing your component
    * 4.1 Creating a Lazarus package for your component(s)
    * 4.2 Documentation
    * 4.3 Creating a Code Release Page
    * 4.4 Submitting the component
  * 5 See also



## Selecting a component or library for conversion

### Where to find code to convert

There is a LOT of code available on the net that can be converted for use with FPC and Lazarus. Here is a [Page Of Code Sites](<Page_Of_Code_Sites.md> "Page Of Code Sites") that is just a start. Please add to it if you know of any other good locations. TurboPower Software has recently released their entire commercial offering under the MPL. A list of available packages can be found [here](<http://sourceforge.net/users/tpsfadmin/>). 

To avoid duplicating effort, packages that have already been converted are listed on the [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples") page. If you've converted a package or are working on one, please add a note on the [Current conversion projects](<Current_conversion_projects.md> "Current conversion projects") page. 

### Licensing

Licenses for existing code range from freeware/public domain to restrictive versions that prohibit modification, re-distribution and commercial use. Before converting any package, it's a good idea to examine its licensing and make sure it's going to be compatible with Lazarus and the Free Pascal Compiler. License selection is especially important with components since dropping one on a form can impose an unwanted or incompatible license on an entire application. 

When converting components, please respect the wishes of the original author and retain all copyright and licensing headers along with email addresses and url's. It's courteous and often useful to inform the author that their component is being converted... especially if the component is under a restrictive license. New interest in an old or forgotten component can sometimes inspire authors to revise their original and overly restrictive licensing. 

In general, Public Domain (freeware), and the LGPL/MPL are the the most flexible for distributing components. For more information, the [Open Source Definition](<http://www.opensource.org/docs/definition.php>) is a good place to start. There are also several comparisons available to help clarify how the various types of licenses work and what impact they'll have on code they're linked to. Search for "open source license comparison" 

### Dependencies

Another step before starting to work on a conversion is to verify that the code doesn't have deep dependancies on other packages that might not be available or represent a considerable conversion challenge. Some freeware offerings are bound to or extend proprietary packages that are frequently no longer available or come with inappropriate licenses. 

### Compiler Issues

See: 

  * [Known problems](<http://www.freepascal.org/probs.html>)
  * [Unfixed bugs](<https://gitlab.com/freepascal.org/fpc/source/-/issues>)



### Platform and OS Issues

Lazarus and the Free Pascal Compiler are cross-platform and cross-architecture development tools. In contrast, most existing Delphi code was specifically designed to run on an Intel processor under Win32. If your candidate component has a lot of Win32 specific code, it might be wise to try and find a less platform dependant alternative. But don't let this stop you... it's genuinely amazing what the LCL supports! 

## Doing the conversion

### Setting up the Lazarus environment for a conversion project

#### Create a test project

  * Place code to be converted into a subdirectory (ie: convertdir)
  * Bring up Lazarus
  * File->Save All to the convertdir subdirectory. Meaningful names for the Project and default unit are optional.
  * Open the "main" unit to be converted in convertdir
  * Add it to the project: Project->Add Active Unit to Project
  * Run Tools->Quick Syntax Check or Run Build All to get started.



#### Delphi VCL, Kylix CLX Sources in Lazarus

When converting Delphi/Kylix sources, it is often useful to do a find declaration to see, what a specific function is doing. The Lazarus IDE can parse the Delphi/Kylix sources. To do this it needs some searchpaths and compiler settings. You can easily setup this in Environment->CodeTools Defines Editor->Insert Template 

### Conversion problems and solutions

#### Delphi / Kylix file equivalents in Lazarus

Delphi / Kylix | Description | Lazarus | Description   
---|---|---|---  
.pas .dfm/.xfm, .dcu/.dpu, .dpr(main project file), .res, .dof/.kof \--- \--- \---  | Delphi Source File, Delphi Form, Delphi Compiled Unit, Delphi Project, Windows Resource File, Delphi Option File  | .pas, .pp .lfm, .o, .lpr \--- \--- .lrs, .lpi(main project file), .ppu  | Pascal unit file, Form data file, Compiled unit file, Project file, Resource file, Project options file, Lazarus resource file, Lazarus project information file, FPC unit description file   
  
So a Delphi .dcu file roughly corresponds to a FPC .o and .ppu together in one file. The .PPU is mostly the header(interface) part, the .o mostly the implementation part. Main exceptions are inlined functions, that in to-be-inlined form are in the .ppu. 

#### Converting Delphi projects/forms/units to Lazarus

Use the Delphi converter in new Lazarus version to convert automatically. It does most of the things explained later on this page. More information here: [Delphi Converter in Lazarus](<Delphi_Converter_in_Lazarus.md> "Delphi Converter in Lazarus")

The following text explains the differences between Delphi and Lazarus and how to convert manually. 

#### Selecting the right compiler mode

The [Free Pascal](<Free_Pascal.md> "Free Pascal") Compiler supports 5 different pascal modes. For example TP for turbo pascal, lets you compile turbo pascal units. There is also a DELPHI compatibility mode that can be set to make existing code easier to convert. Lazarus prefers the OBJFPC mode, which almost equals the DELPHI mode, but is less ambiguous than the Delphi syntax. Here are the important points: 

The mode can be selected at command line or at the start of the source. Using the command line has the advantage, that you don't need to change the source, but the disadvantage, that others must be told. 

Most Delphi units can be compiled by the [Free Pascal](<Free_Pascal.md> "Free Pascal") compiler by adding: 
    
    
    {$IFDEF FPC}
      {$MODE DELPHI}
    {$ENDIF}
    

right after the unit name. 

For more details about [Free Pascal](<Free_Pascal.md> "Free Pascal") modes see the [Free Pascal Documentation](<http://www.freepascal.org/docs-html/prog/progap4.html#progse62.html>)

#### Cross-Platform considerations

  * Inline assembler is always a problem because it will bind the code to the Intel architecture. Some developers do algorithm prototypes in Pascal and ifdef the their optimized assembler. Fortunately TurboPower did this in numerous places with their code. If this is the case with the package you're converting, throw the switch back to Pascal.
  * Don't reference specific memory location like the BIOS data area. Find out what the code needs and try to find a cross platform alternative.
  * Don't do processor specific tricks (like using the Intel TSC) without enclosing your code in an ifdef for the platform the code needs... and providing an alternative for environments that don't have the hardware capability.
  * If you need some OS specific code, then you can use IFDEFs. See below for a list of macros.



#### Useful compiler variables / defines / macros

To write code, that behaves on different systems differently, you can use the 

{$IFDEF Name}

directives. 

  * {$IfDef FPC}




This variable is defined, when using the FPC compiler. Useful to write code, that works with FPC and Delphi. For compatibility reasons FPC defines the DELPHI macro in mode Delphi. So you can **not use** {$IFNDEF DELPHI}**.**

  * {$IfDef FPC_OBJFPC}




This variable is defined when using mode OBJFPC and can be a substitute for {$IFNDEF Delphi} for e.g. the @ differences 

  * {$IfDef LCL}




This variable is defined, when using the LCL package. Useful to write code, that works with the LCL and Delphi. 

  * {$IfDef LCLGtk}

, 

{$IfDef LCLWin32}

, 

{$IfDef LCLQt}

, ...



This variable is defined, when using the LCL package and the specific [widgetset](<Widgetset.md> "Widgetset") is currently used. Useful to write code, that works with the LCL on a specific platform. 

  * {$IfDef Unix} // for Linux, BSD, macOS, Solaris

, 

{$IfDef Win32}

, 

{$IfDef Linux}

// for Linux, 

{$IfDef Darwin}

// macOS, 

{$IfDef WinCE}

// for WinCE,...



Defined by FPC for the current Target OS. Delphi defines "Linux", "Win32" and "MSWindows". [Free Pascal](<Free_Pascal.md> "Free Pascal") runs on much more platforms and so it is recommended to use the more general items. For example "Unix" is defined for Linux, FreeBSD, NetBSD and OpenBSD, where Lazarus already runs. Use for code that should work with Delphi and FPC: 
    
    
    {$IFDEF Linux}
      {$DEFINE Unix}
    {$ENDIF}
    

to work around this for Kylix. 

  * {$IfDef ENDIAN_BIG}




This macro is defined on big endian processors such as the PowerPC as used, a.o., in older Apple computers. Their byte order is the reverse of that of Intel compatible processors. 

  * {$IfDef ENDIAN_BIG}




For more details see the [Free Pascal Documentation](<http://www.freepascal.org/docs-html/prog/prog.html#QQ2-23-21>). 

#### 32bit / 64 bit support

Pointers under 64bit need 8 bytes instead of 4 on 32bit. The 'Integer' type remains for compatibility 32bit. This means you can no longer typecast pointers into integers and back. FPC defines two new types: PtrInt and PtrUInt. PtrInt is a 32bit signed integer on 32 bit platforms and a 64bit signed integer on 64bit platforms. The same for PtrUInt, but _unsigned_ integer instead. Use for code that should work with Delphi and FPC: 
    
    
    {$IFNDEF FPC}
    type
      PtrInt = integer;
      PtrUInt = cardinal;
    {$ENDIF}
    

Replace all **integer(SomePointerOrObject)** with **PtrInt(SomePointerOrObject)**. 

For more information see [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide"). 

#### Finding a missing identifier

There are differences in how the LCL is organized when compared to the Delphi VCL. If you get a "not found" compiler error about a major class or identifier, the chances are good that it's in a different unit. A complete cross reference can be found by grep'ing lazarus/docs/xml or the lcl subdirectory. 

For example the commonly used tbutton typically throws an error in Delphi code because it's located in a unit named buttons.pp. The following command finds the correct unit very quickly (in the lazarus source directory): 
    
    
    grep -in ' tbutton =' lcl/*
    

  


#### Major unit differences between Lazarus and Delphi

**Please add to this topic!**

  * Windows->Interfaces, LCLIntf, LCLType, LCLProc, ...)



As the LCL is not windows specific, the code that is in the Delphi Windows unit for directly accessing the Win32 API is abstracted into separate interfaces, which can be accessed from the LCLIntf unit. Keep in mind, that Lazarus does not emulate win32, so many functions are missing and some do not work as their win32 counterparts. These functions only exist for Delphi compatibility and should only be used for quick & dirty porting. LCL also breaks out many of the types, so often LCLType is required. LCLProc also contains a few functions which can be useful for lower level handling such as "FreeThenNil" as is in Delphi 5 and higher, "DeleteAmpersands" to remove additional ampersands from a string for controls(& vs && etc). The Interfaces unit needs to be included in the .lpr file to initialize the appropriate widgetset. 

  * Messages->LMessages



TControl Messages for win32 event callbacks of the format WM_CALLBACK and the structs associated with them are often found in the Messages unit in Delphi. In the LCL these types of messages and there structs are usually found in LMessages, usually with name changes of WM to LM, so for instance WM_MOUSEENTER becomes LM_MOUSEENTER, and TWMMouse becomes TLMMouse. 

  * Graphics, Controls->GraphType, GraphMath, Graphics, Controls



To simplify some things and break complexity of circles between units, a few types have been abstracted into a shared unit called GraphType, which includes things, which in Delphi are located in Graphics or Controls, for instance the bvNone etc of panels. So sometimes you have to include it. Also a unit which, although incompatible with Delphi, adds other useful functionality is GraphMath, which adds a TFloatPoint for precision, misc routines for dealing with beziers, lines, and arcs, as well as some operator overloading for use with TPoints and TRect, such as for instance Point1 := Point2 + Point3, and comparing two rects like if (rect1 = rect2) then ... 

  * Mask->MaskEdit



For more intelligent naming considerations, the unit for TMaskEdit is called [MaskEdit|] instead of the slightly more nebulous Mask as in many versions of Delphi. 

  * StdCtrls->StdCtrls,Buttons



In many version of Delphi TButton is located in StdCtrls, while TSpeedButton and TBitBtn are in Buttons. For consistency and simplicity the LCL puts all button types in Buttons, which can occasionally break code conversion, so it is always a good idea to include. 

#### Property and method differences Delphi -> FPC/LCL

  * TBitmap contains a canvas in the LCL
  * [Lazarus_Faq#Why are TForm.ClientWidth.2FClientHeight the same as TForm.Width.2FHeight](<Lazarus_Faq.md> "Lazarus Faq")



#### Semantical differences

##### Order of parameter evaluation

Delphi guarantees that all parameters are evaluated from left to right. FPC makes no such guarantee, and can evaluate parameters in any order it wants in order to generate optimal code. 

##### Nested procedures/functions as procedural variables

Delphi passes the framepointer of the parent procedure always on the stack, and has the caller remove it again. This means that as long as you do not access variables from a parent procedure, you can pass the address of a nested function to another function which then can call it like any other procedural variable. 

FPC on the other hand always passes the framepointer of the parent procedure as a hidden first parameter according to the current calling convention, which means that if you call a nested procedure like a regular procedural variable, all parameters will be "shifted" one position. 

In short, do not call nested procedures via procedural variables. 

##### Ref count release

Documented behaviour in both Delphi and FPC is that temporary variables stay allocated till after the statement of last use. (so if an interface is used in a parameter declaration, it is earliest released after that whole procedure/method call statement has finished). 

However in practice FPC often indeed decreases the refcount the very next statement, while Delphi tends to wait till the end of procedures. Or, in very large procedures, till the end of a large block. 

Both are valid choices though, and fixating this behaviour would hem in the codegeneration and optimizer too much, and it won't changed. 

This can occasionally cause apparently working Delphi code that mixes object and interface references to fail, and is a typical case of seemingly working code not being correct and depending on implementation choices. The Delphi documentation doesn't make any such guarantees. 

#### Syntax differences

**Please add to this topic!**

Because of the inherent strictness in FPC, some syntax changes are necessary, even though 

{$Mode Delphi}

does allow more laziness like Delphi does. For this reason complying as much with the syntax rules of 

{$Mode ObjFPC}

as possible is highly recommended, even when the codebase is still going to be shared between Delphi and the LCL. Some of these are simply better coding practices, and sometimes because Delphi mode is not entirely accurate, or in a few instances Delphi acceptible code does not function as expected with FPC, even though it might compile. To that end even though not all such are strictly required, the following list of changes should be considered mandatory : 

  


##### When assigning an event handling entry point, prefix it with an "@"

For instance, you might assign a button [callback](<callback.md> "callback") manually. 
    
    
    With the Delphi code compiled under Lazarus You will get the error message
    "Wrong number of parameters specified for call to "SomeFunction" "
    

Delphi | OBJFPC | both   
---|---|---  
      
    
    begin
      if not Assigned(MyButton.OnClick) then 
        MyButton.OnClick:= SomeFunction;
        //@ not required
        //more code...
    end;
    

| 
    
    
    begin
      if not Assigned(MyButton.OnClick) then
        MyButton.OnClick:= @SomeFunction;
        //@ IS required
        //more code...
    end;
    

| 
    
    
    begin
      if not Assigned(MyButton.OnClick) then
        MyButton.OnClick:= {$IFDEF FPC_OBJFPC}@{$ENDIF}SomeFunction;
        //@ is only required under mode OBJFPC - but not allowed under Delphi
        //more code...
    end;
      
  
Note that this behavior of the compiler can also be influenced using the CLASSICPROCVARS modeswitch. 

##### When calling a procedure variable use this syntax: theprocname()

In Delphi there is no difference between a function result and a variable, however there is in FPC, so to call a function, even if it has no parameters, you must append parenthesis. 
    
    
    With the Delphi code compiled under Lazarus You will get the error message
    "Incompatible types: got "<procedure variable type of function:DWord;StdCall>" expected "LongWord" "
    

Delphi | OBJFPC | both   
---|---|---  
      
    
    With (SomeObject) do 
    begin
      If Assigned(OnMyCallback) then
        OnMyCallback;
        //parenthesis not required
    end;
    

| 
    
    
    With (SomeObject) do 
    begin
      If Assigned(OnMyCallback) then
        OnMyCallback();
        //parenthesis required
    end;
    

| 
    
    
    Simply use the OBJFPC way, with brackets:
    
        OnMyCallback();
    
    Delphi accepts this, too.
      
  
##### When accessing values in a pointer to a record you must dereference first

In Delphi it is not required to de-reference a pointer to a record to access values within it, it can, in fact, be treated just like the record itself, or any other object. In FPC it must be first de-referenced. 
    
    
    With the Delphi code compiled under Lazarus You will get the following messages :
    "Error: Illegal qualifier"
    "Hint: may be pointer dereference is missing"
    

Delphi | OBJFPC | both   
---|---|---  
      
    
    Function GetSomeValue(ARecord: PMyRecord):Integer;
    begin
      If Assigned(ARecord) then
        Result:=ARecord.SomeValue
      else
        Result:=0;
    end;
    

| 
    
    
    Function GetSomeValue(ARecord: PMyRecord):Integer;
    begin
      If Assigned(ARecord) then
        Result:=ARecord^.SomeValue
      else
        Result:=0;
    end;
    

| 
    
    
    Simply use the OBJFPC way, with ^ :
    
        Result:=ARecord^.SomeValue
    
    Delphi accepts this, too.
      
  
Note that the behavior of the compiler with regards to (auto) de-referencing pointers can also be influenced using the AUTODEREF modeswitch. 

##### When accessing chars of an indexed string Property of an object, it must be enclosed in parentheses

With Delphi it is possible to treat a Property exactly like some other const or var, e.g. even up to accessing individual chars of a string directly, while this is not always possible in FPC, specifically for indexed properties. Instead it must be enclosed in parentheses, to make distinct. While this may not always hold true it is probably a good practice to consider anyway. For example 

Delphi | OBJFPC   
---|---  
      
    
    Type TSomeComponent=class(TComponent)
      //More code...
    Published
    Property MyString:String index 3 read GetMyString;
    //More code...
    End;
    
    var
      MyChar:char;
    begin
      If Length(MyString)>2 then
        //no parenthesis needed
        MyChar:= MyString[3];
        //More code...
    end;
    

| 
    
    
    Type TSomeComponent=class(TComponent)
    //More code...
    Published
    Property MyString:String index 3 read GetMyString;
    //More code...
    End;
    
    var
      MyChar:char;
    begin
      If Length(MyString)>2 then
        //parenthesis sometimes needed
        MyChar:= (MyString)[3];
        //More code...
    end;
      
  
##### You must typecast pointers to actual type when using with var or function of that type

Sometimes in Delphi you will have a null pointer variable representing an object. While it might seem a complex situation, it is oddly quite common especially in large component packs as a method of preventing too many circular includes between objects in different units. In Delphi it is then possible to send this null pointer to a function expecting that object, without bothering to typecast to actual type, in fpc you must typecast. 

For example - 

Delphi | OBJFPC   
---|---  
      
    
    Unit 1
      Type 
        TSomeObject=class(TComponent)
          //More code...
        End;
    
      Procedure DoSomething(Value: TSomeObject);
      Function GetSomeObject: TSomeObject;
    
    Unit 2
      Type 
        TSomeComponent=class(TComponent)
        //More code...
        Published SomeObject: Pointer;
        //More code...
      End;
    
    Application
    var 
      MyComponent: TSomeComponent;
    begin
      MyComponent.SomeObject:=GetSomeObject;
      //More code...
      DoSomething(MyComponent.SomeObject);
    end;
    

| 
    
    
    Unit 1
      Type 
        TSomeObject=class(TComponent)
       //More code...
      End;
    
      Procedure DoSomething(Value: TSomeObject);
      Function GetSomeObject: TSomeObject;
    
    Unit 2
      Type TSomeComponent=class(TComponent)
        //More code...
        Published SomeObject: Pointer;
        //More code...
      End;
    
    Application
    var 
      MyComponent: TSomeComponent;
    begin
      MyComponent.SomeObject:=Pointer(GetSomeObject);
      //More code...
      DoSomething(TSomeObject(MyComponent.SomeObject));
    end;
      
  
#### Resources

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The section below may not be necessary. Any recent version of Lazarus directly supports .res files. The Windows Lazarus installer includes a resource compiler which supports .rc files. On *nix, please make sure windres is installed so you can use .rc files. See <http://wiki.lazarus.freepascal.org/Lazarus_Resources#FPC_resources>

Delphi resource files are win32 specific and not compatible with Lazarus, so you'll have to recreate and compile them using the lazres tool. Lazres can be found in the lazarus/tools subdirectory. If you've downloaded the Lazarus sources, you'll need to compile it first. 

  * cd lazarus/tools
  * make install



To add a resource to your application: 

  * lazres myresource.lrs mypix.xpm anotherpix.xpm
  * Add the LResources unit to your Uses clause
  * Include the .lrs file you created under the initialization block



Example: 
    
    
    function TForm1.LoadGlyph(const GlyphName: String): TBitMap;
    begin
      Result:= TPixmap.Create;
      Result.LoadFromLazarusResource(GlyphName);
    end;
    //More code...
    begin
      Speedbutton1.glyph:= LoadGlyph('mypix');
      //More code...
    end;
    
    initialization
    {$I unit1.lrs}
    {$I myresource.lrs}
    end.
    

### Another method to convert a Delphi or Kylix project to Lazarus

  * Rename or copy all .dfm or .xfm files to .lfm (Very old Delphi versions do not produce a text-based .dfm file. The convert utility, if present in the \bin folder can be used to covert the .dfm first))
  * Rename or copy .dpr file to .lpr
  * Make all necessary changes to .lpr file:


  1. Add {$mode delphi}{$H+} or {$mode objfpc}{H+} directives
  2. Add 'Interfaces' to uses clause
  3. Comment out or delete {$R *.res} or directive if needed


  * Make necessary changes to all .pas unit files:


  1. Add {$mode delphi}{$H+} or {$mode objfpc}{H+} directives
  2. Add 'LResources' - to do: check if this is still needed for any vaguely new Lazarus version
  3. If the form has buttons, add 'Buttons' to uses clause
  4. Comment out or delete {$R *.dfm} or {$R *.xfm} directive
  5. Add 'Initialization' section at the end of each unit file, and add {$I unitname.lrs} directive in it


  * Select Project->New Project from file
  * Select the .lpr file
  * In the 'Create a new project' window, choose 'Application'
  * Build project and make further necessary corrections to get proper compilation. At this point the .lpi file is generated automaticaly. You may get 'Error reading Form' messages, click on 'Continue Loading' if you do.
  * Save all, and you have a Lazarus project :-)



## Getting Help

If you encounter a problem during conversion that you just can't solve, there are a wide variety of places to get help. 

### Documentation  
  
---  
For pure Object Pascal and FPC issues  | The best place to start is the Free Pascal [Documentation](<http://www.freepascal.org/docs-html/>) by Michaël Van Canneyt and Florian Klämpfl.   
For more Lazarus oriented problems  | The Lazarus Project Documentation in the Lazarus-CCR Knowledgebase [Main Page](<Main_Page.md> "Main Page") is the next place to look.   
  
### Peer support  
  
Mailing lists  | You can post a question on any of the [mailing lists for the Free Pascal Compiler](<http://www.freepascal.org/maillist.html>) or the [FPC/Lazarus forums](<http://forum.lazarus.freepascal.org/index.php?action=forum>) where a lot of experts are subscribed.   
If you have access to IRC:  | On irc.freenode.net: [#fpc](<irc://irc.freenode.net/fpc>) for FPC, or [#lazarus-ide](<irc://irc.freenode.net/lazarus-ide>) for Lazarus   
  
### Knowledge bases

There are some outstanding search and knowledge bases online that can also be a great help for learning new techniques and solving problems: 

  * Tamarack Associates operates a fast [search](<http://codenewsfast.com/>) engine specifically for the Borland usenet archives.
  * Mer Systems Inc. provides a similar search [engine](<http://www.mers.com/searchsite.html>).
  * Another outstanding source of information along with a sitewide [search](<http://www.efg2.com/Lab/search.htm>) capability is Earl F. Glynn's Computer Lab and Reference [Library](<http://www.efg2.com/>).



## Packaging and Releasing your component

### Creating a Lazarus package for your component(s)

Creating a package makes installing the code you've converted a much easier process... especially if you're providing more than one component. Mattias Gärtner has written an overview of [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages") that should be read before beginning this process. 

### Documentation

The purpose of this site and the wiki format is to make the generation of professional documentation an easy and quick process. The wiki also makes it possible to see the results of your posting immediately and make any changes you'd like in real time. 

Using the Lazarus-CCR wiki to create nice looking documentation is very easy. If you've never used wiki markup before, you can get familiar with it in the [Sand Box](<Sand_Box.md> "Sand Box") practice area. 

### Creating a Code Release Page

The Code Release Page contains vital information about your component that a potential downloader will need to know, such as license, intended platform, status (alpha, beta, stable...), where to download it, who wrote it, is support available... etc. 

The following procedure will let you create a Code Release Page with your browser: 

  * Go to the [Release new component](<Release_new_component.md> "Release new component")
  * Type the name of your component in the textbox and click on _Create Article_
  * Fill in the template and hit save.
  * Edit the [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples") page and add the link to your newly created page to the "Released Components" section. Save the modified page.



### Submitting the component

If you're a release technician on the project, upload your component to the SourceForge File Release System and add it to the list of release packages. Otherwise send it to one of the project administrators ([Tom Lisjac](</User:Tom> "User:Tom") or [Vincent Snijders](</User:Vincent> "User:Vincent")) and we'll add it to the repository. Before we upload it to SourceForge, you have to create a _Code Release Page_ to describe your component. You can use the [Release new component](<Release_new_component.md> "Release new component") page, to start creating such a page. 

If you think you need to continue to develop on the component, we can also put it into SVN so you'll continue to have access to it. If we get tired from committing your patches, we will give you write access to the SVN, so you can commit your patches yourself. For details see [Using the Lazarus-ccr SVN repository](<Using_the_Lazarus-ccr_SVN_repository.md> "Using the Lazarus-ccr SVN repository"). 

## See also

  * [Lazarus For Delphi Users](<Lazarus_For_Delphi_Users.md> "Lazarus For Delphi Users")
  * [Delphi language features missing from the Free Pascal Compiler](<Delphi_language_features_missing_from_the_Free_Pascal_Compiler.md> "Delphi language features missing from the Free Pascal Compiler")

---

_Source: [https://wiki.freepascal.org/Code_Conversion_Guide](https://web.archive.org/web/20240906212425/https://wiki.freepascal.org/Code_Conversion_Guide)_
