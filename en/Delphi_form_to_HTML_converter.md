# Delphi form to HTML converter

## Contents

  * 1 About
    * 1.1 Overview and Benefits
    * 1.2 Requirements and/or Compatible Software
    * 1.3 Documentation
      * 1.3.1 OVERVIEW
      * 1.3.2 SUPPORTED CONTROLS
      * 1.3.3 AT DESIGN-TIME
        * 1.3.3.1 PROPERTIES FROM THE OBJECT INSPECTOR
        * 1.3.3.2 THE ATTRIBUTES EDITOR
      * 1.3.4 AT RUN-TIME
    * 1.4 INSTALLATION
    * 1.5 WebHub VCL Developers
    * 1.6 How to install into IDE
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements



## About

_Delphi Form to HTML Converter component (F2H)_ is a Lazarus version of a new component, originally developed for Delphi 3. 

Designed for... Delphi coders who wish to re-use a Delphi form design on a web page 

It's main caracteristics are : 

  * It inputs a Delphi form (soon to support Lazarus forms)
  * It outputs HTML forms (which two variations) ... Standard HTML or WebHub Chunks
  * Originally commercial for US$99 in Delphi 3 by href.com
  * Released under the Creative Commons License - contributors can donate money if they decide to support it.
  * etc



### Overview and Benefits

If you can make a Delphi or C++Builder form, you can now create HTML pages instantly, and have them look just as sharp. Drop the Form to HTML Converter component onto your existing form, right-click, choose Browse It!, and you'll have your HTML file. 

**There are two different techniques for using the converter component**

1) Use it to export static HTML files 

In this scenario, you put the component into an existing Delphi form, set the main container, and export an HTML file. Most likely, you would then maintain that HTML page using an HTML editor such as FrontPage. This is the most likely situation for a Delphi programmer who does not use WebHub. Your main benefit is that you do not have to recreate form layouts in HTML that you have already created in Delphi. 

For gain the fullest support for Netscape 3.0 and 4.0 and for Internet Explorer 3.0 and 4.0, export a tailored page for each version of the browsers. Then change the browser family property and export again. 

2) Use it as part of an interactive web application, to create dynamic HTML pages 

In this scenario, you also put the component onto your existing Delphi form and choose your container. However, your application would use the WebHub components, and you would use the converter component to generate a WebHub style "chunk" rather than a standard HTML file. That chunk would become part of a web page within a larger application. You would then set the browser family based on the information from the current surfer's browser, and the dynamic page would always be optimized for that surfer. 

The download contains the component, ( no an installation package nor demo application, that illustrates the features of the component along with some instrumentation for evaluating the chart on a given system, yet). 

### Requirements and/or Compatible Software

Delphi or C++Builder for win32, or Lazarus Other browsers/versions: The generated HTML was designed precisely for Netscape 3/4 and Microsoft IE 3/4. If you need to support another browser brand or version, try the default Netscape setting because that will look good in most browsers. 

### Documentation

Look for f2h-readme.txt which gives you everything you need for installing and using the component. 

f2h-readme.txt (follows) 

#### OVERVIEW

TwhForm2HTML is a WYSIWYG Delphi form to HTML translator. 

Drag and drop a TwhForm2HTML component on any Delphi form and you are ready to export it in HTML. 

A custom editor gives you full control on the HTML attributes used, plus a few options. 

Exports can be done : 

  * in standard HTML or as WebHub chunks
  * at design time or dynamically at run time.



As browsers size objects quite differently, the component may address the four main browsers differently in order to keep a common look and feel despite of their different behaviours. 

Who is the ideal user ? 

  * The Delphi developer which has an HTML form to code and doesn't master HTML too much - or simply wants to save time - can use the component at *design time* only, and export the HTML code to integrate it in a static page. By letting the BrowserFamily property to bfDefault, the component chooses a mean way in sizing objects which gives an acceptable rendering with all browsers.


  * The Delphi developer which has an existing C/S Delphi application to be ported for remote access through the Web, with tens of data entry screens can do it in a breeze.



By adding the component to all the forms for which a remote access is needed and including those forms in a WebHub project, a big part of the job is got immediately. In that way of use, keeping the original look and feel of the form is a big plus of course. 

#### SUPPORTED CONTROLS

  * TLabel
  * TEdit
  * TMemo
  * TCheckBox
  * TRadioButton
  * TListBox
  * TComboBox
  * TButton
  * TImage
  * TGroupBox
  * TPanel
  * and any other TWinControl container.



#### AT DESIGN-TIME

##### PROPERTIES FROM THE OBJECT INSPECTOR

AlternateFonts : string = Optional string appended to the 'Face' attribute of the  tags. Attributes... = Open the editor (see below) BGColor : string = Fixes the form's background color. Accepts : 

  * [None] : no background color
  * [Same] : same background color as the Delphi form color
  * any HTML color name
  * any HTML color string in the form #RRGGBB
  * any color picked in a TColorDialog (opened by double clicking)



Browse it ! = Opens the HTML form using your default browser. BrowserFamily should be set before, according to that browser. BrowserFamily = (bfDefault, bfIE3, bfIE4, bfNS3, bfNS4). Fixes the target browser. bfDefault equals to bfIE4 because of the mean way of IE4 in sizing objects. ChunkOnly : Boolean = When exporting in standard mode if ChunkOnly is False, the component exports a whole HTML page, including a page title. Export HTML... = Opens a TSaveDialog to perform the export in standard HTML mode Export WebHub chunk... = Opens a TSaveDialog to perform the export in the WebHub mode MainContainer : TWinControl = By default, TwhForm2HTML converts the whole form. By setting MainContainer to any other container (groupbox, panel, ...), you may restrict the export to that container. SetFonts : Boolean = If True, TwhForm2HTML uses the  tag to keep the elements fonts. The controls which are concerned are : labels, radiobuttons and checkboxes 

  


##### THE ATTRIBUTES EDITOR

Opened by double clicking on the component, the editor lists the controls in a tree view on the left panel. By selecting a control in the tree, you may check / edit its attributes on the right panel. The first element in the tree (<FORM>..</FORM>) gives you control on the <FORM> tag attributes. 

The editor shows three kinds of attributes : 

  * HTML attributes
  * HTML attributes used when exporting in WebHub mode : they have the same name as the standard



attribute, prefixed by 'WH'. Example : Action <\--> WHAction. 

  * TwhForm2HTML options



Only the TwhForm2HTML options are documented here. They vary depending on the control selected : 

Options for all elements : 

  * AfterElement : string = Lets you insert any HTML code after the element
  * BeforeElement : string = Lets you insert any HTML code before the element



Options for all elements but containers and labels : 

  * ExtraAttributes = To define additionals HTML attributes (like scripts handlers



for example) under the form : 'AnAttribute1="Value1" AnAttributeN="ValueN"' 

Options for a TForm : 

  * Border : Boolean = If True, the form is rendered with a border around it
  * ShowCaption : Boolean = If True, the title bar of the form is kept
  * CaptionBGColor : string = Back color of the title bar if ShowCaption is True



Default = 'Navy' 

  * CaptionColor : string = Text color of the title bar if ShowCaption is True



Default = 'White' 

Options for a TButton : 

  * AsImage : Boolean = Specifies if the button has to be converted in an <INPUT>



tag (standard HTML button, type "submit" or "reset"), or in an <IMG> tag. 

Options for a TImage : 

  * AsButton : Boolean = If True, the TImage is translated in an <INPUT Type="image">



tag. If False, the TImage can be handled as an anchor or not. 

  * AsAnchor : Boolean = If True, you can edit the HREF link attribute.



#### AT RUN-TIME

The component can be used to generate the forms chunks on the fly. A write-only 'CGIUserAgent' property, compatible with TWebServer.CGIUserAgent must be used to inform the component of the target browser. 

Example in WebHub mode : 
    
    
    with WHForm2HTML1 do 
    begin
       CGIUserAgent := WebServer1.CGIUserAgent;   // set BrowserFamily automatically
       ExportWHChunkFName := 'aChunk.txt';	      // set the export filename
       if exportWHChunk then		      // function exportWHChunk : Boolean
          // do something with the text file
    end;
    

In place of writing the chunk in a text file, you can get the chunk string through the public property WHChunk : string read getWHChunk. 

Example in standard mode : 
    
    
    with WHForm2HTML1 do 
    begin
       CGIUserAgent := WebServer1.CGIUserAgent;   // sets BrowserFamily automatically
       ExportFName := 'aForm.HTML';	      	      // sets the export filename
       if exportHTML then		      	      // function exportHTML : Boolean
          // do something with the HTML page
    end;
    

In place of writing the page in a text file, you can get the its string through the public property HTML : string read getHTML. 

Notice that if you create controls at run-time, TWHForm2HTML must be informed in order to take those new controls into account. To achieve that, simply call the Synchronize procedure. As shown below, it's also possible to set attributes on-the-fly. 

Example : 
    
    
    var
       attrObject : THAttrObject;
    begin
    with WHForm2HTML1 do 
    begin
       // Creation of controls on the fly
       // ...
       // ...
       synchronize;   // Now the component knows about them
       // ...
       // Now let's say that you created a TButton named 'Button1' and you
       // need to access the corresponding THButton object in order to set
       // one of its HTML attributes :
    
       attrObject := WHForm2HTML1.HMainContainer.FindHObjectByName('Button1');
       if attrObject <> nil then 
       begin
          with attrObject do 
          begin
             Attributes['AsImage'].BooleanValue := False;   // set the 'AsImage' option
             Attributes['Type'].Value := 'reset';           // set the 'Type' attribute
          end;
       end;
    end;
    

  


### INSTALLATION

TWHForm2HTML currently supports Delphi 5+, C++ Builder and Lazarus. 
    
    
    unit regf2h; 
    
    {*******************************************************}
    {                                                       }
    {       Form to HTML converter "F2H"                    }
    {       Register component to Delphi palette            }
    {                                                       }
    {       Copyright (c) 1998-2010 HREF Tools Corp.        }
    {       http://www.href.com/f2h                         }
    {                                                       }
    {       This file is licensed under a Creative Commons  }
    {       Share-Alike 3.0 License.                        }
    {       http://creativecommons.org/licenses/by-sa/3.0/  }
    {       If you use this file, please keep this notice   }
    {       intact.                                         }
    {                                                       }
    {       Developed by HREF Tools Corp. 1998-2010         }
    {       First author: Philippe Maquet                   }
    {                                                       }
    {*******************************************************}
    

  * Install component via regf2h.pas as a new component as per normal.



### WebHub VCL Developers

  * F2h is automatically installed on the WebHub palette when you build your component library or package.
  * Rebuild your component library or package to take advantage of your new code.



  


### How to install into IDE

  * Unzip the ZIP file in any folder
  * Menu: Component > Install Packages
  * Install the package for your compiler: D06 = Delphi 6, D07 = Delphi 7, D11 = Delphi 2007, D12 = Delphi 2009



The component will be installed on a palette named "WebHub". 

## Screenshot

(not currently available) -- single form component. 

## Author

Ann Lynneworth of HREF.com 
    
    
    { Developed by HREF Tools Corp. 1998-2010               }
    {       First author: Philippe Maquet                   }
    {       Redeveloped by http://pews-freeware-games.org   }
    {       Second author: Peter Eric Williams (pew)        }
    {       for Lazarus and Free Pascal                     }

## License
    
    
    {       This file is licensed under a Creative Commons  }
    {       Share-Alike 3.0 License.                        }
    {       http://creativecommons.org/licenses/by-sa/3.0/  }
    {       If you use this file, please keep this notice   }
    {       intact.                                         }

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

## Download

The latest stable release can be found on 

[https://sites.google.com/site/pewtas/form2html_lazarus.zip?attredirects=0&d=1](<https://sites.google.com/site/pewtas/form2html_lazarus.zip?attredirects=0&d=1>)

This is the new component for Lazarus "Delphi Form to HTML Converter" (originally for Delphi 3). Needs some work... please contact PEW... It should work fine with Delphi 3 forms... Help is requested to make it work for Lazarus forms and also changes documented in the enclosed HTML file re: detecting and calling default web browser in a cross-platform way. 

  


## Change Log

  * Version 1.0 _date_



##  Dependencies / System Requirements

  * None



Status: _Stable_

Issues: 

See Webbrowser / Webbrowser - Lazarus wiki.html ... code needs to be added to make it check for the default web browser and call the default web browser as per code on html file (copy of Lazarus wiki). This is to provide cross-platform call of default web browser.

---

_Source: [https://wiki.freepascal.org/Delphi_form_to_HTML_converter](https://web.archive.org/web/20250211174123/https://wiki.freepascal.org/Delphi_form_to_HTML_converter)_
