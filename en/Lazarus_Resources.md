# Lazarus Resources

│ **[Deutsch (de)](</Lazarus_Resources/de> "Lazarus Resources/de")** │  **English (en)** │  **[español (es)](</Lazarus_Resources/es> "Lazarus Resources/es")** │  **[français (fr)](</Lazarus_Resources/fr> "Lazarus Resources/fr")** │  **[한국어 (ko)](</Lazarus_Resources/ko> "Lazarus Resources/ko")** │  **[русский (ru)](<../ru/Lazarus_Resources.md> "Lazarus Resources/ru")** │    
****

## Contents

  * 1 Introduction
  * 2 FPC resources
    * 2.1 Adding resources to your program
    * 2.2 Use the -FF parameter in FPC 3.3.1
    * 2.3 Checking you have windres
      * 2.3.1 Linux
      * 2.3.2 FreeBSD
      * 2.3.3 macOS
        * 2.3.3.1 Using binutils source
      * 2.3.4 Windows
    * 2.4 Using resources in your program
  * 3 Lazarus resources
    * 3.1 Lazarus Resource Form File
    * 3.2 Getting the raw data of an lrs resource
  * 4 See also
  * 5 External links



## Introduction

Resource files contain data which should be compiled into the executable file. That data could consist of images, string tables, version info, ... even a Windows XP manifest and forms. This includes data that the programmer can retrieve in his code (accessing them as files). Using resources can be handy if you want to distribute self-contained executables. 

Before FPC 2.4 it was not possible to use standard Delphi-compatible resource files (*.res) in Lazarus because they were Win32 specific. Please see Lazarus resources (*.lrs) below. 

Standard Delphi-compatible resources are now recommended for current FPC **(including all recent Lazarus versions)**. Please see FPC resources below. 

Alternatively, you can include resources in your application from within the Lazarus IDE by using the [IDE Resources Dialog](<IDE_Window__Project_Options.md> "IDE Window: Project Options"). 

## FPC resources

Starting from FPC 2.4 you can use standard .rc (resource script) and .res (compiled resource) files in your project to include resources. To turn a .rc script used in the sources into a binary resource (.res file), FPC runs the appropriate external resource compiler (windres or GoRC). Therefore that resource compiler needs to be installed and present in the PATH environment variable. For more details, see: [FPC Programmer's guide, chapter 13 "Using Windows resources"](<http://www.freepascal.org/docs-html/prog/progch13.html>)

To simplify the compiling process, it is possible to use only the compiled resources in the .res files. You can precompile the resources by any available resource compiler - windres (available both on unixes and windows), GoRC (windows only), Microsoft resource compiler (rc.exe included in Visual Studio), Borland resource compiler (brcc32.exe included in Delphi, C++ Builder or Rad Studio products) or any other. 

Use 

  * {$R filename.rc} to compile a resource script and include the resulting resource file or
  * {$R filename.res} directive to include a compiled resource file



into the executable. 

FPC RTL provides both low-level functions as well as high-level classes to access resources. 

The low-level functions are: 

  * EnumResourceTypes
  * EnumResourceNames
  * EnumResourceLanguages
  * FindResource
  * FindResourceEx
  * LoadResource
  * SizeofResource
  * LockResource
  * UnlockResource
  * FreeResource



They are compatible with the Windows API functions: [[1]](<http://msdn.microsoft.com/en-us/library/ff468902\(VS.85\).aspx>)

The main class used to work with resources is TResourceStream. LCL uses it to load embedded bitmaps, icons and form streams. Look at TGraphic.LoadFromResourceID or TIcon.LoadFromResourceHandle to see how it is used inside LCL. 

### Adding resources to your program

Let's review the situation when you need to store some data inside your executable and during the program run you want to extract this data from it. 

First we need to tell the compiler what files to include in the resource. We do this in a .rc (resource script) file: mydata.rc file: 
    
    
    MYDATA         RCDATA "mydata.dat"
    

Here MYDATA is the resource name, RCDATA is the type of resource (look here <http://msdn.microsoft.com/en-us/library/ms648009(VS.85).aspx> for explanation of possible types) and "mydata.dat" is your data file. 

Let's instruct the compiler to include the resource into your project: 
    
    
    program mydata;
    
    {$R mydata.rc}
    begin
    end.
    

Behind the scenes, the FPC compiler actually instructs the resource compiler distributed with FPC to follow the .rc script to compile your data files into a binary .res resource file. The linker will then include this into the executable. Though this is transparent to the programmer, you can, if you want to, create your own .res file with e.g. the Borland resource compiler. Instead of using `{$R mydata.rc}` you'd use `{$R mydata.res}`. 

### Use the -FF parameter in FPC 3.3.1

FPC 3.3.1 can use fpcres instead of windres as RC to RES compiler. To enable it, pass the -FF parameter to the compiler. In that case you won't have to install windres. 

Edit your fpc.cfg and add lines at the end in order to enable the parameter by default 
    
    
    # Use fpcres as resource compiler
    -FF
    

### Checking you have windres

On Linux/FreeBSD/macOS, you might need to make sure you have the _windres_ resource compiler, which is part of the GNU binutils project. 

#### Linux

On Debian systems you could run (FIXME : this work for a 64B version, for 32B systems you will must probably change w64 to w32) : 
    
    
    sudo apt install mingw-w64 mingw-w64-tools
    

or, if you do not need full mingw : 
    
    
    sudo apt install binutils-mingw-w64-x86-64
    

Next 2 windres compilers will be installed (in /usr/bin) x86_64-w64-mingw32-windres and i386-w64-mingw32-windres. But FPC search "windres" (worst the path can be "Select manually"). You must edit /etc/fpc.cfg and add lines at the end (delete the old "-FC") 
    
    
    # MS Windows .rc resource compiler
    #IFDEF CPUAMD64
    -FCx86_64-w64-mingw32-windres
    #ENDIF
    #IFDEF cpui386
    -FCi386-w64-mingw32-windres
    #ENDIF
    

#### FreeBSD

Even if you install the _binutils_ package via the ports collection, the _windres_ binary is not installed - it is probably considered too Windows centric. So you have to manually add _windres_ yourself. 

When you build the _binutils_ package via ports, the _windres_ binary is actually built, but it is not installed by default. So, after building and installing the package: 

1\. From the /usr/ports/devel/binutils directory, cd work/binutils-2.xx/binutils 

2\. Copy the _windres_ binary into your /usr/local/bin/ directory, or into your $HOME/bin/ directory. 

**Note:** The _windres_ utility is hardcoded to use _gcc_ but [FreeBSD 10.0](<https://www.freebsd.org/releases/10.0R/relnotes.html#userland>) (January 2014) replaced _gcc_ with _clang_ , so you will also need to install _gcc_ from ports to be able to use _windres_ if you are using a version of FreeBSD that is not end-of-life. 

#### macOS

##### Using binutils source

As of December 2021, the latest version is binutils-2.37 but it changes regularly. Substitute the latest version in any command below. 

  * Download the latest binutils source from <https://ftp.gnu.org/gnu/binutils/>
  * Uncompress the binutils-2.37.tar.gz archive (I used _The Unarchiver_ free utility from the Mac App Store)
  * Open an Applications > Utilities > Terminal and type the following commands:


    
    
    cd binutils-2.34
    ./configure
    make
    cd binutils
    make windres
    sudo cp windres /usr/local/bin/
    cd doc
    gzip windres.1
    sudo cp windres.1.gz /usr/local/share/man/man1/
    

If you execute `windres` you will get a disappointing error message: 
    
    
    windres: Can't detect architecture.
    

To fix this you should be able to pass the `--target=mach-o-x86-64` argument to `windres` on the command line, but this still gives the same error message. My ultimate fix was to change the `windres.c` file as follows: 
    
    
    $ diff  ~/tmp/windres.c windres.c
    1100c1100,1101
    <   def_target_arch = NULL;
    ---
    >   //def_target_arch = NULL;
    >   def_target_arch = "mach-o-x86-64";
    

The above diff indicates I changed line 1100 in the `windres.c` file by commenting it out, then added a new line which assigns the correct architecture to the _def_target_arch_ variable. You then need to again: 
    
    
    make windres
    sudo cp windres /usr/local/bin/
    

For detailed help you can consult the man page by opening an Applications > Utilities > Terminal and typing the following command: 
    
    
    man windres
    

Alternatively, you can display a usage summary by opening an Applications > Utilities > Terminal and typing the following command: 
    
    
    windres --help
    

#### Windows

On Windows, windres and the gorc resource compiler should be provided by the FPC and Lazarus installers. 

### Using resources in your program

Now let's extract the stored resource into a file, e.g. mydata.dat: 
    
    
    program mydata;
    uses
      SysUtils, Classes, LCLType;
    {$R mydata.res}
    var
      S: TResourceStream;
      F: TFileStream;
    begin
      // create a resource stream which points to our resource
      S := TResourceStream.Create(HInstance, 'MYDATA', RT_RCDATA);
      // Please ensure you write the enclosing apostrophes around MYDATA, 
      // otherwise no data will be extracted.
      try
        // create a file mydata.dat in the application directory
        F := TFileStream.Create(ExtractFilePath(ParamStr(0)) + 'mydata.dat', fmCreate); 
        try
          F.CopyFrom(S, S.Size); // copy data from the resource stream to file stream
        finally
          F.Free; // destroy the file stream
        end;
      finally
        S.Free; // destroy the resource stream
      end;
    end.
    

Many classes (at least in Lazarus trunk/development version) also support loading directly from FPC resources, e.g. 
    
    
    procedure exampleproc;
    var
      Image: TImage
    begin
      Image := TImage.Create;
      Image.Picture.LoadFromResourceName(HInstance,'image'); // note that there is no need for the extension
    end;
    

## Lazarus resources

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** As mentioned above, using "normal"/Windows resources is now possible. Lazarus has also switched to this format since ver. 1.4; see [Lazarus 1.4.0 release notes](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes")

In order to use your files as Lazarus resources, they must be included as a resource file. 

You can create LRS files with [LRS explorer](<https://sourceforge.net/projects/lrsexplorer/>) or using _lazres_ with the command line. 

Lazres can be found in the "Tools" directory (C:\Lazarus\Tools\ - you may need to compile it from lazres.lpi project) of your Lazarus installation folder. 

Then you can compile Lazarus resource files (*.lrs) via the command line. The syntax for lazres is: 
    
    
    lazres <filename of resource file> <files to include (file1 file2 file3 ...)>

Example: 
    
    
    lazres mylazarusresource.lrs image.jpg
    

Alternatively you can compile and execute glazres (C:\Lazarus\Tools\glazres) for a GUI version of lazres. 

To use a Lazarus resource file in your project, include the file with the $I compiler directive in the **initialization** section of your unit. 

You can access the data in the resources directly or with the LoadFromLazarusResource method of the variables which will hold the file contents afterwards. LoadFromLazarusResource requires a string parameter that indicates which object should be loaded from the resource file. 

Example: 
    
    
    uses ...LResources...;
    
    ...
    procedure exampleproc;
    var
      Image: TImage
    begin
      Image := TImage.Create;
      Image.Picture.LoadFromLazarusResource('image'); // note that there is no need for the extension
    end;
    
    initialization
      {$I mylazarusresource.lrs}
    

This code includes the file mylazarusresource.lrs into the project. In the procedure exampleproc an icon object is created and loaded from the object "image" out of the resource. The file which was compiled into the resource was probably named image.jpg. 

Every class that is derived from [TGraphic](</index.php?title=TGraphic&action=edit&redlink=1> "TGraphic \(page does not exist\)") contains the [LoadFromLazarusResource](</index.php?title=LoadFromLazarusResource&action=edit&redlink=1> "LoadFromLazarusResource \(page does not exist\)") procedure. 

### Lazarus Resource Form File

Lazarus generates .LRS files from .LFM form files. 

When a LRS form file is missing, FPC reports the following error: `ERROR: unit1.pas(193,4) Fatal: Can't open include file "unit1.lrs"`

To solve it you can either: 

  * use lazres: c:\lazarus\tools\lazres.exe unit1.lrs unit1.lfm
  * (easier): make a trivial change in the form design, revert it and save the form; this will recreate the .lrs file without needing to run lazres.



### Getting the raw data of an lrs resource

You can retrieve the data of the resource with: 
    
    
    uses ...LResources...;
    
    ...
    procedure TForm1.FormCreate(Sender: TObject);
    var
      r: TLResource;
      Data: String;
    begin
      r:=LazarusResources.Find('datafile1');
      if r=nil then raise Exception.Create('resource datafile1 is missing');
      Data:=r.Value;
      ...do something with the data...
    end;
    

## See also

  * [FCL resource units](<fcl-res.md> "fcl-res")
  * [Lazarus Resource compiler](<lazres.md> "lazres")
  * [IDE Project Options > Resources](<IDE_Window__Project_Options.md> "IDE Window: Project Options")



## External links

  * [Free Pascal Programmer's Guide, Chapter 13 "Using Windows resources"](<https://www.freepascal.org/docs-html/prog/progch13.html>).

---

_Source: [https://wiki.freepascal.org/Lazarus_Resources](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Lazarus_Resources)_
