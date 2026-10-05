# pas2js widgetsets

The ultimate goal is to use Free Pascal and Lazarus to create a webpage in a RAD manner as much as possible. There can be several approaches to this. 

  * Create an actual LCL widgetset
  * Do not use the actual LCL widgetset, but create a widgetset that uses the IDE support for a custom designer in Lazarus.
  * Do not use the actual LCL widgetset, but create one that supports Pas2JS app development in the browser.



  
Either way, a set of basic widgets that can be used in HTML are needed. 

Currently, several efforts are underway to create such a widget set. 

## Contents

  * 1 Open Source
  * 2 Other Demonstrations
  * 3 Commercial
  * 4 XIDE/XComponents
  * 5 Pas2JS Widgetset
  * 6 Navigation



### Open Source

In random order, they are: 

  * XComponents (and XIDE): [[1]](<https://github.com/Steve--W/XComponents>) (Steve Wright)
  * Web Component Library (WCL): [[2]](<https://github.com/pascaldragon/Pas2JS_Widget>) \- fork of Pas2JS Widgetset (Sven Barth)
  * (archived) Pas2JS Widgetset: a RAD Framework to develop Web Applications [[3]](<https://github.com/heliosroots/Pas2JS_Widget>) (Heliosroots)
  * Pas2js implementation of DHTMLX: [[4]](<https://github.com/cutec-chris/dhtmlx-for-pas2js>) (Christian Ulrich)



### Other Demonstrations

  * Web Widgets, bundled with pas2js: [[5]](<https://www.freepascal.org/~michael/pas2js-demos/widgets/#>)
  * projJ: [[6]](<https://github.com/pas2js/pas2js.github.io/tree/master/master/projJ>)
  * Puma: [[7]](<https://www.sc10.com.br/puma/puma_doc.html>) \- lightweight UI components for pas2js based on Bulma
  * VUE using pas2js: [[8]](<https://github.com/imperyal/pas2js-and-VUE>)



### Commercial

Most freeware tools for Lazarus Web Application lack of features and documentations. If you really need a good component for Web Application then TMS Web Core is worth to try. 

  * TMS software has created: [TMS Web Core](<https://www.tmssoftware.com/site/tmswebcoreintro.asp>)
  * Install TMS Web Core to Lazarus:<https://www.youtube.com/watch?v=YzZazLnF8Zk>
  * TMS Web Core Developer Guide: <http://www.tmssoftware.biz/Download/Manuals/TMSWEBCoreDevGuide.pdf>



  
**Another commercial alternative that is free for open-source projects is[xProject](<https://green-hill.srl>)**

This project transforms your Delphi or Lazarus project into a web app without any modifications.   
There's no need to install components in Delphi or Lazarus.   
The result is exclusively HTML, CSS, and JavaScript, and it looks and behaves exactly like the desktop application. 

### XIDE/XComponents

XIDE is a simple, stand alone, open source IDE for Free Pascal which runs in the browser (and on other platforms supported by Lazarus). 

It is a combined Client Side Run Time Library and RAD IDE intended to allow Pascal(Pas2JS) and/or Python(Pyodide) development with the minimum of installation or learning curve while also being as platform independent as possible. XComponents is the widgetset that enables XIDE. 

XIDE is intended for Prototyping, Small Group Collaboration and Agile Line of Business projects where the choice of browser can be specified. It will run on any platform that is supported by Chrome or Electron, or Lazarus(+CEF). It is not intended for the development of general-purpose public facing web sites. It may also run on other HTML5 browsers (e.g. Microsoft Edge), but this is not tested. 

XIDE projects can be deployed as a single static HTML/Javascript (or .exe) file combining the User App with the RTL and IDE Code. This can be done with the IDE disabled (for end users) or enabled (as done in the examples below, for collaborators). 

**Examples…..**
    
    
       XIDESimplePascalExample.html [[9]](<https://steve--w.github.io/XIDEPages/XIDESimplePascalExample.html>)
       XIDESimplePythonExample.html [[10]](<https://steve--w.github.io/XIDEPages/XIDESimplePythonExample.html>)
       XIDEPascalSVGAndGPUExample.html [[11]](<https://steve--w.github.io/XIDEPages/XIDEPascalSVGAndGPUExample.html>)
    

**To build from source….**
    
    
       XIDE [[12]](<https://github.com/Steve--W/XIDE>) 
       XComponents [[13]](<https://github.com/Steve--W/XComponents>)
    

**XIDE Screenshot**

    [![XIDEExampleScreenShot.jpg](https://wiki.freepascal.org/images/e/e6/XIDEExampleScreenShot.jpg)](</File:XIDEExampleScreenShot.jpg>)

### Pas2JS Widgetset

Pas2JS Widgetset is a RAD Framework to develop Web Applications like to develop Windows Applications.  


Installation Procedure:  


1\. Download from: <https://github.com/heliosroots/Pas2JS_Widget>  
or from <https://github.com/pascaldragon/Pas2JS_Widget>  


2\. Extract to **Lazarus - pas2js_designer**  


3\. Open: Lazarus - pas2js_designer - package: **pas2js_designer_package.lpk**

    [![pas2js designer-install-01.png](https://wiki.freepascal.org/images/3/36/pas2js_designer-install-01.png)](</File:pas2js_designer-install-01.png>)

4\. Click on **Use - Install**

    [![pas2js designer-install-02.png](https://wiki.freepascal.org/images/b/b0/pas2js_designer-install-02.png)](</File:pas2js_designer-install-02.png>)

5\. Click on Yes to install this package 

    [![pas2js designer-install-03.png](https://wiki.freepascal.org/images/c/c7/pas2js_designer-install-03.png)](</File:pas2js_designer-install-03.png>)

6\. You will see LCL for pas2js! 

    [![pas2js designer-install-04.png](https://wiki.freepascal.org/images/f/f9/pas2js_designer-install-04.png)](</File:pas2js_designer-install-04.png>)

## Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [Pas2JS Version Changes](<Pas2JS_Version_Changes.md> "Pas2JS Version Changes")
  * Back to [lazarus_pas2js_integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/pas2js_widgetsets](https://web.archive.org/web/20231209015020/https://wiki.freepascal.org/pas2js_widgetsets)_
