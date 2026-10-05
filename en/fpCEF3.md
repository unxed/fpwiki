# fpCEF3

[**Deutsch (de)**](</fpCEF3/de> "fpCEF3/de") | **English (en)**   
****

## Contents

  * 1 Description
  * 2 Installation
    * 2.1 Download Package
    * 2.2 Package Installation
    * 2.3 Download Framework CEF3
  * 3 Usage
    * 3.1 Minimal example
      * 3.1.1 Windows
    * 3.2 Show URL of loaded page
    * 3.3 Allow downloading of files
    * 3.4 Printing per code
    * 3.5 Creating a pdf
  * 4 Known Problems
  * 5 Further Information



## Description

[![SimpleChromium.png](https://wiki.freepascal.org/images/b/b4/SimpleChromium.png)](</File:SimpleChromium.png>)

Chromium is an open source project (from Google) to create a browser for different platforms. Among other things the browser Chrome is based on it. 

The Chromium Embedded Framework (CEF) is an open source framework for embedding Chromium into user applications. It can be used on Linux, macOS and Windows in 32 and 64 bit versions. Originally there were two versions of CEF, CEF1 and CEF3, but CEF1 is no longer maintained. 

CEF can be used in Delphi via the Delphi Chromium Embedded Framework (DCEF3). For Lazarus/Free Pascal there is the equivalent framework [fpCEF3](<https://github.com/dliw/fpCEF3>) (or alternative [CEF4Delphi](<https://github.com/salvadordf/CEF4Delphi>) with examples for Lazarus including newest versions of Chromium Embedded Framework). 

## Installation

Besides the Lazarus package, the CEF framework itself is needed. It is important to use the correct version of the CEF framework for the fpCEF release used. Further specific installation instructions can be found on Github: <https://github.com/dliw/fpCEF3>

The latest release and its corresponding CEF version can be found on [[1]](<https://github.com/dliw/fpCEF3/releases/latest>)

CEF releases can be found on <https://cef-builds.spotifycdn.com/index.html> (use the version filter or click "Show more builds" to find the release) 

### Download Package

fpCEF3 with [Git](<http://git-scm.com/>)
    
    
    <git://github.com/dliw/fpCEF3.git>
    

You can also download a zipped package: 
    
    
    [master.zip](<https://github.com/dliw/fpCEF3/archive/master.zip>)
    

### Package Installation

[![Chromium.png](https://wiki.freepascal.org/images/3/33/Chromium.png)](</File:Chromium.png>)

After downloading of package fpCEF3, it can be installed in Lazarus: 

  * Start Lazarus and open main menu _Package_ -> _Open package file (.lpk)..._
  * Select the file _cef3.lpk_ in the fpCEF3 subdirectory _Component_
  * The package editing window will open
  * Click on _Compile_ , the package will be compiled
  * Click on _Use..._ -> _Install_ and confirm the dialog window with _Yes_
  * Lazarus is now compiling and create a new tab _Chromium_ in the component palette



### Download Framework CEF3

You can now download the framework and made the libraries available for a particular project. You can download it from the following pages:  
<https://cefbuilds.com>  
<http://www.magpcss.net/cef_downloads> (or a older version from [here](<http://www.magpcss.net/cef_downloads/index.php?query=label%3ADeprecated#list>)) 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Make sure to use the correct framework version for the fpCEF version! See Installation

## Usage

### Minimal example

A minimal example with a simple browser is created. 

#### Windows

The CEF libraries are needed for loading, so it makes sense to create a new project folder, to save the project and the libraries in it together. 

  * Create a new directory for the project to a location of your choice such as _C:\SimpleBrowser_
  * Copy the complete CEF3 directory _Resources_ to _C:\SimpleBrowser_
  * Copy the contents of the CEF3 directory _Release_ in the directory _C:\SimpleBrowser_



The files (_libcef.dll_ , _natives_blob.bin_ , _icudtl.dat_ etc.) are in the same directory as the project and the directory _locales_ in _C:\SimpleBrowser\locales_.  
To prevent misunderstands, here is a picture how the file and directory structure of the Simplebrowser should look:  
  
[![FilesForSimplebrowser.png](https://wiki.freepascal.org/images/a/aa/FilesForSimplebrowser.png)](</File:FilesForSimplebrowser.png>)  
  


Now build the project with Lazarus 

  * Create a new empty [ GUI application](<Form_Tutorial.md> "Form Tutorial")
  * Save the project in the directory you specify for Windows (e.g. under _C:\SimpleBrowser_)
  * Drop a `TEditButton` on the form
  * With Object Inspector set the `Name` to `edtURL`, `Align` on `alTop` and `ButtonCaption` to `go`
  * Put a `TChromium` on the form and set the `Name` to `Chromium` and its `Align` to `alClient`
  * Create the `OnButtonClick` event handler for the TEditButton `edtURL`. Select the `OnButtonClick` in the Object Inspector tab `events` and click on the button [...]
  * Write following code (chromium should open at each click the URL in the text box):


    
    
    procedure TForm1.edtURLClick(Sender: TObject);
    begin
      Chromium.Load(UTF8Decode(edtURL.Text));
    end;
    

  * `Chromium` must be initialized at the start of the program, you can do this with the `OnCreate` event handler of the form:


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      PrjPath: ustring;
      Lang, FallbackLang: string;
    begin
      PrjPath := UTF8Decode(GetCurrentDir + PathDelim);
      CefLocalesDirPath := PrjPath + 'locales';
      GetLanguageIDs(Lang, FallbackLang);
      CefLocale := UTF8Decode(FallbackLang);
      edtURL.Text:='http://www.lazarus-ide.org/';
    end;
    

  * If a identifier wouldn't be found, you have to put some units to the uses clause. These units should be in there:


    
    
    uses
      sysutils, Forms, EditBtn, cef3lcl, cef3intf, cef3types, cef3lib, gettext;
    

  * Run your program, if all goes well it might look like:



[![SimpleChromium.png](https://wiki.freepascal.org/images/b/b4/SimpleChromium.png)](</File:SimpleChromium.png>)  
  


### Show URL of loaded page

Based on Minimal example

  * Create the `OnLoadEnd` event handler for `Chromium`
  * Write following code (URL should be updated after any browsing):


    
    
    procedure TForm1.ChromiumLoadEnd(Sender: TObject; const Browser: ICefBrowser;
      const Frame: ICefFrame; httpStatusCode: Integer);
    begin
      edtURL.Text:=UTF8Encode(Browser.MainFrame.Url);
    end;
    

### Allow downloading of files

  * Create the `OnBeforeDownload` event handler for `Chromium` and write in it:


    
    
    procedure TForm1.ChromiumBeforeDownload(Sender: TObject;
      const Browser: ICefBrowser; const downloadItem: ICefDownloadItem;
      const suggestedName: ustring; const callback: ICefBeforeDownloadCallback);
    begin
      callback.Cont(suggestedName, True);
    end;
    

### Printing per code

  * Add a `TButton` on your form and write in its `OnClick` event handler:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Chromium.Browser.Host.Print;
    end;
    

### Creating a pdf

  * Add a `TSaveDialog` on your form
  * Add a `TButton` on your form and write in its `OnClick` event handler:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      CefPdfPrintSettings: TCefPdfPrintSettings;
      EmptyCefStringUtf16: TCefStringUtf16;
    begin
      SaveDialog1.DefaultExt := 'pdf';
      SaveDialog1.Filter     := 'Portable Document Format (pdf)|*.pdf';
      if SaveDialog1.Execute then
      begin
        FillByte(EmptyCefStringUtf16, SizeOf(EmptyCefStringUtf16), 0);
    
        CefPdfPrintSettings.backgrounds_enabled := 1;
        CefPdfPrintSettings.header_footer_enabled := 0;
        CefPdfPrintSettings.header_footer_title := EmptyCefStringUtf16;
        CefPdfPrintSettings.header_footer_url := EmptyCefStringUtf16;
        CefPdfPrintSettings.landscape := 0;
        CefPdfPrintSettings.margin_bottom := 0;
        CefPdfPrintSettings.margin_left := 0;
        CefPdfPrintSettings.margin_right := 0;
        CefPdfPrintSettings.margin_top := 0;
        CefPdfPrintSettings.margin_type := PDF_PRINT_MARGIN_DEFAULT;
        CefPdfPrintSettings.page_height := 0;
        CefPdfPrintSettings.page_width := 0;
        CefPdfPrintSettings.selection_only := 0;
    
        Chromium.Browser.Host.PrintToPdf(UTF8Decode(SaveDialog1.FileName), CefPdfPrintSettings, Nil);
      end;
    end;
    

The description of the `TCefPdfPrintSettings` you will find by the declaration of the `TCefPdfPrintSettings`

## Known Problems

  * If your program doesn't start, maybe a Antivirus program prevent this.
  * If your program start but your TChromium window keeps empty, maybe a firewall block it (see [Chromium Embedded Framework for Free Pascal (fpCEF3) BRUNCH 2556](<http://forum.lazarus.freepascal.org/index.php/topic,31361.0.html>)).
  * If your file structure isn't correct (see Minimal example) and therefore some libraries wouldn't be found, you can get optical errors (see [Fpcef red borders in the browser](<http://forum.lazarus.freepascal.org/index.php/topic,30763.0.html>)).



## Further Information

  * [Thread in the German Lazarusforum](<http://www.lazarusforum.de/viewtopic.php?f=18&t=7676>)
  * [Webbrowser](<Webbrowser.md> "Webbrowser")

---

_Source: [https://wiki.freepascal.org/fpCEF3](https://web.archive.org/web/20221230081341/https://wiki.freepascal.org/fpCEF3)_
