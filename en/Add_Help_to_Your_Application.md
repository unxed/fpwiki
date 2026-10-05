# Add Help to Your Application

│ **[Deutsch (de)](</Add_Help_to_Your_Application/de> "Add Help to Your Application/de")** │  **English (en)** │  **[español (es)](</Add_Help_to_Your_Application/es> "Add Help to Your Application/es")** │  **[français (fr)](</Add_Help_to_Your_Application/fr> "Add Help to Your Application/fr")** │  **[русский (ru)](<../ru/Add_Help_to_Your_Application.md> "Add Help to Your Application/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</Add_Help_to_Your_Application/zh_CN> "Add Help to Your Application/zh CN")** │    
****

The LCL comes with a help system, and allows you to **create help for your own applications**. 

## Contents

  * 1 Quick Start
  * 2 Help Basics
  * 3 CHM
    * 3.1 Using LHelp
  * 4 HTML
    * 4.1 Setup HTML help for your application
    * 4.2 Creating a help entry
  * 5 INF (using fpGUI's DocView help viewer)
  * 6 See also



## Quick Start

Open the example in examples/helphtml/. 

This project demonstrates how to use the HTML help components. Just drop them on a form of your project. Setup the paths and create some HTML pages. Then give each control of your application a HelpKeyword. 

See the CHM help section if you want to use CHM help files instead of HTML help files. 

There is a very nice simple demo (chmdemo.zip) here: <http://forum.lazarus.freepascal.org/index.php/topic,38487.msg261731.html#msg261731>

## Help Basics

The LCL help mainly consists of two parts: Help databases and help viewers. A Help Database contains the mapping from the keywords (ID, node, message, pascal, ...) to the help page (or help web site or...). The Help Viewer is invoked by the Help Database to show the help to the user. 

  * A [THelpDatabase](</index.php?title=THelpDatabase&action=edit&redlink=1> "THelpDatabase \(page does not exist\)") manages content. It can be a collection of HTML pages or fpdoc XML files or a CHM file or a database or whatever.
  * A [THelpViewer](</index.php?title=THelpViewer&action=edit&redlink=1> "THelpViewer \(page does not exist\)") is a component that shows help content. For example a viewer for the mime type text/html can start a web browser.



When help is requested, the LCL queries each registered [THelpDatabase](</index.php?title=THelpDatabase&action=edit&redlink=1> "THelpDatabase \(page does not exist\)") and each database can return a list of entries. If several entries are returned, the LCL asks the user to choose an entry. Then the LCL asks the database to show the help for the entry. The database extracts the help content and asks the LCL for a viewer that supports the mime type of the content. Finally, the viewer shows the help content. 

## CHM

Context-sensitive CHM application help can be used from Lazarus 1.0 and later. 

A demonstration program is included that shows how to include context-sensitive help using CHM and the [lhelp](<lhelp.md> "lhelp") CHM viewer (the same one that is used for IDE help by default). Please see ${lazarusdir}/components/chmhelp/democontrol/. 

You can write your own CHM files, e.g. with the now ancient Microsoft HTML Workshop or with the new Lazarus chmmaker tools in $(lazarusdir)/tools/chmmaker You can use a [TCHMHelpDatabase](<TCHMHelpDatabase.md> "TCHMHelpDatabase") control like the [THTMLHelpDatabase](<THTMLHelpDatabase.md> "THTMLHelpDatabase") control described below. 

The advantages of using the CHM system are a smaller, self contained help file instead of multiple files. On the other hand, not every system has a CHM viewer installed by default, so you might want to include lhelp, a CHM viewer written in Pascal and included with the Lazarus sources (components/chmhelp/lhelp/lhelp.lpi). 

### Using LHelp

  * Drop a **`TCHMHelpDatabase`** on your form.
  * Set its **`FileName`** property to the path to the chm file, and set **`AutoRegister`** to `true`.
  * In **`KeywordPrefix`** specify the path to the html files contained in the chm file as seen from the chm root. Example: If the html help files during chm creation are in the subfolder _html_ the `KeywordPrefix` is `html` (without trailing path delimiter!).
  * Put a **`TLHelpConnector`** on your form.
  * Set **`LHelpPath`** to the name and location of the LHelp executable (this can be absolute, or relative to the application directory)
  * Set **`AutoRegister`** to `true`.


    
    
    // Example with both chm file and LHelp.exe in Application folder.
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      if FileExists(ChangeFileExt(Application.ExeName, '.chm')) then
      begin
        CHMHelpDatabase1.Filename := ChangeFileExt(Application.ExeName, '.chm');
        CHMHelpDatabase1.KeywordPrefix := 'html';
        CHMHelpDatabase1.AutoRegister:=True;
    
        LHelpConnector1.LHelpPath := IncludeTrailingBackSlash(ExtractFileDir(Application.ExeName)) + 'lhelp.exe';
        LHelpConnector1.AutoRegister:=True;
      end
      else
        mnuHelpMain.Enabled := False;
    end; 
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if mnuHelpMain.Enabled then  
        ShowHelpOrErrorForKeyword('', 'html/main.html');
        //ShowTableOfContents;  (Not implemented for TCHMHelpDatabase)
    end;
    

When LHelp is invoked during execution of your project it is started by _Inter-Process Communication_ (IPC) in a separate process which normally does not terminate when you application is closed. If you want to close LHelp with your application you should write an `OnDestroy` event handler to send an `mrClose` command to LHelp via IPC: 
    
    
    uses
      LHelpControl;  // for "mrClose"
         
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      if (LHelpConnector1.Connection <> nil) and LHelpConnector1.Connection.ServerRunning then
        LHelpConnector1.Connection.RunMiscCommand(LHelpControl.mrClose);
    end;
    

## HTML

The LCL provides two components to use HTML files for help: [THTMLHelpDatabase](<THTMLHelpDatabase.md> "THTMLHelpDatabase") and [THTMLBrowserHelpViewer](<THTMLBrowserHelpViewer.md> "THTMLBrowserHelpViewer"). To see the HTML help, see the lazarus example examples/helphtml/htmlhelp1.lpi. 

### Setup HTML help for your application

[![](https://wiki.freepascal.org/images/7/7e/LazHtmlHelp.jpg)](</File:LazHtmlHelp.jpg>)

[](</File:LazHtmlHelp.jpg> "Enlarge")

Lazarus help items

Adding HTML help to your application is easy: 

  * Put a **THTMLHelpDatabase** on a form.
  * Set **AutoRegister** to true.
  * Set **KeywordPrefix** to **html/**. It means all keywords must start with the string _html/_.
  * Set **BaseURL** to **file://yourhelp/**. This will search the HTML files in the sub folder _yourhelp_. You can specify full paths like _file:///usr/lib/yourhelp/_ or an URL like _<http://www.yoursite.com/>_.


  * Put a **THTMLBrowserHelpViewer** on the form. This component can start the user's default browser.
  * Set **AutoRegister** to true.



### Creating a help entry

  * Now create the subfolder _yourhelp_ and create an HTML page _yourhelp/edit1.html_. In case of a website, the help page should be accessible as _<http://www.yoursite.com/edit1.html>_
  * Put a TEdit on a form
  * Set **HelpType** to _htKeyword_
  * Set **HelpKeyword** to _html/edit1.html_



When running the program you can focus the edit control and press `F1` to invoke the help. Under macOS the help key sequence is `Cmd-?` (or `Cmd+Shift+?` depending on your keyboard layout). 

Note: Some window managers, widget set combinations do not pass `F1` to the LCL. 

## INF (using fpGUI's DocView help viewer)

See the message and example project included in the Lazarus Forums. [[1]](<http://forum.lazarus.freepascal.org/index.php/topic,27864.msg173887.html#msg173887>)

It shows a fully working example of an LCL application using [fpGUI](<fpGUI.md> "fpGUI")'s [DocView help viewer](<http://fpgui.sourceforge.net/screenshots_apps.shtml>). It shows context sensitive help and general help. 

For example: 

  * Set focus to a specific control and press `F1`. It will show the help topic for that specific control.
  * Click the Help button and it will show the help topic for the dialog/form.
  * Select the "Help -> Show Help" menu item and it will show the general application help and display the first topic in the help file.



## See also

  * [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")

---

_Source: [https://wiki.freepascal.org/Add_Help_to_Your_Application](https://web.archive.org/web/20240121085724/https://wiki.freepascal.org/Add_Help_to_Your_Application)_
