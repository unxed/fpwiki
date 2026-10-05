# SplashAbout

## Contents

  * 1 SplashAbout
    * 1.1 Property page screenshot
    * 1.2 Purpose
    * 1.3 Features
    * 1.4 How to use in your code
    * 1.5 License
    * 1.6 Download
    * 1.7 Installation
    * 1.8 Compatability
    * 1.9 Version
    * 1.10 Support
    * 1.11 See Also



## SplashAbout

_SplashAbout_ is a Lazarus visual component. It installs on the 'Additional' component palette. 

  * Author: minesadorada@charcodelvalle.com
  * New version: 1.4.3 - June 2014



#### Property page screenshot

[![splashabout properties143.png](https://wiki.freepascal.org/images/c/c1/splashabout_properties143.png)](</File:splashabout_properties143.png>)

#### Purpose

  * If you are fed up of coding splash and about dialogs for minor apps - this component is the easy way to give your apps a good appearance with minimum effort and easy individualisation. 
    * When starting a new application, just drop the component onto your form, and in the Form.Create() method, call SplashAbout1.ShowSplash. This will display a splash dialog. Setting a few easy properties will personalise your splash screen.
    * In your Help/About handler, call SplashAbout1.ShowAbout. This will display an 'About' dialog for your application. The 'About' dialog can show buttons to display a 'Licence' dialog and/or a 'Credits' dialog (with scrolling text)
  * All dialogs are constructed 'on-the-fly' and destroyed when closed. Thus few extra resources are added to your application.
  * For really minimal resource use, the component can optionally use only external files (deployed with your executable)



#### Features

  * Optional shaped splash screen (not Linux) Some built-in shapes or use your own.
  * Splash screen can be a jpg with transparent mask (see example app included)
  * Can automatically display application version numbers
  * Can automatically display LCL/FPC build information
  * Can display standard GPL,LGPL,MIT or ModifiedGPL licenses with personalised text
  * Able to display splash until ApplicationIdle event (default is fixed number of seconds)
  * Able to display scrolling Credits window (Like the Lazarus IDE 'About' dialog)
  * Optional pre-splash 'Powered By Lazarus' graphic



#### How to use in your code

  * Drop a SplashAbout component onto your main form (From the 'Additional' components palette)
  * In the Form.Create event


    
    
       procedure TForm1.FormCreate(Sender: TObject);
       begin
           SplashAbout1.ShowSplash;
       end;
    

  * In your 'About' button click event


    
    
       procedure TForm1.Button1Click(Sender: TObject);
       begin
           SplashAbout1.ShowAbout;
       end; 
    

  * There are lots of properties to tweak - see the example app included in the archive for ideas
  * See the [SplashAbout/Advanced_Use](<SplashAbout/Advanced_Use.md> "SplashAbout/Advanced Use") page for detailed information



#### License

SplashAbout is issued under an LGPL license. Use it as you wish; improve it as you will. (but please give credit) 

#### Download

Click to download: [splashabout zipped files](<http://www.charcodelvalle.com/splashabout/splashabout_component.zip>)

Full source code, readme and example apps (Windows and Linux) are included in the archive. 

#### Installation

  * Create a new folder in your lazarus/components folder, and unzip into it
  * Install via the Lazarus IDE. (Open splashabout.lpk, then compile and install)
  * The new component will be installed in the 'Additional' component palette.



#### Compatability

  * Lazarus 0.9x/1.x/fpc 2.x
  * Windows and Linux
  * Tested on Windows 7 (32-bit and 64-bit builds) and Linux 32-bit



#### Version

Version 1.4.3 

#### Support

[Email the author](<mailto:minesadorada@charcodelvalle.com>) with any queries 

### See Also

  * [All SplashAbout properties explained in detail](<SplashAbout/Advanced_Use.md> "SplashAbout/Advanced Use")
  * [Standalone Scrolling Text component](<ScrollText.md> "ScrollText")
  * [Components and code examples](<Components_and_Code_examples.md> "Components and Code examples")

---

_Source: [https://wiki.freepascal.org/SplashAbout](https://web.archive.org/web/20241202163214/https://wiki.freepascal.org/SplashAbout)_
