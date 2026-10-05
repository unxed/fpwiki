# Developing Web Apps with Pascal

## Contents

  * 1 Overview
  * 2 XIDE/XComponents
  * 3 ExtPascal
  * 4 qxotica
  * 5 xProject F9
  * 6 See also



## Overview

This page describes various ways of integrating javascript with Free Pascal. 

  


## XIDE/XComponents

XIDE is a simple, stand alone, open source IDE for Free Pascal which runs in the browser (and on other platforms supported by Lazarus). 

It is a combined Client Side Run Time Library and RAD IDE intended to allow Pascal(Pas2JS) and/or Python(Pyodide) development with the minimum of installation or learning curve while also being as platform independent as possible. XComponents is the widgetset that enables XIDE. 

XIDE is intended for Prototyping, Small Group Collaboration and Agile Line of Business projects where the choice of browser can be specified. It will run on any platform that is supported by Chrome or Electron, or Lazarus(+CEF). It is not intended for the development of general-purpose public facing web sites. It may also run on other HTML5 browsers (e.g. Microsoft Edge), but this is not tested. 

XIDE projects can be deployed as a single static HTML/Javascript (or .exe) file combining the User App with the RTL and IDE Code. This can be done with the IDE disabled (for end users) or enabled (as done in the examples below, for collaborators). 

**Examples…..**
    
    
       XIDESimplePascalExample.html [[1]](<https://steve--w.github.io/XIDEPages/XIDESimplePascalExample.html>)
       XIDESimplePythonExample.html [[2]](<https://steve--w.github.io/XIDEPages/XIDESimplePythonExample.html>)
       XIDEPascalSVGAndGPUExample.html [[3]](<https://steve--w.github.io/XIDEPages/XIDEPascalSVGAndGPUExample.html>)
    

**To build from source….**
    
    
       XIDE [[4]](<https://github.com/Steve--W/XIDE>) 
       XComponents [[5]](<https://github.com/Steve--W/XComponents>)
    

**XIDE Screenshot**

    [![XIDEExampleScreenShot.jpg](https://wiki.freepascal.org/images/e/e6/XIDEExampleScreenShot.jpg)](</File:XIDEExampleScreenShot.jpg>)

## ExtPascal

ExtPascal is a set of Pascal components that wrap the Ext JS set of JavaScript widgets. 

<http://code.google.com/p/extpascal/>

With the optional ExtP Toolkit, you can turn Lazarus into an ExtPascal IDE. 

## qxotica

The qxotica tools provide a way to use Lazarus to create both the qooxdoo-based JavaScript client and the Pascal server backend for a Web app. 

<https://sourceforge.net/projects/qxotica/>

## xProject F9

xProject F9 enables the easy conversion of your Delphi or Lazarus applications into fully functional web applications with just one click. 

The compiler takes the source code and GUI settings and generates a web application from it that looks and functions exactly like the original program.  
Exclusively HTML, CSS, and Javascript code will be the result.  
xProject F9 is free for open source projects. 

<https://green-hill.srl/>

## See also

  * [Integrating Javascript Engines with Pascal](<Integrating_Javascript_Engines_with_Pascal.md> "Integrating Javascript Engines with Pascal")
  * [fcl-web](<fcl-web.md> "fcl-web") Framework for developing web applications. Also lets you use JSON/REST APIs.
  * [Fano Framework](<https://fanoframework.github.io>) Web application framework for modern Pascal programming language.

---

_Source: [https://wiki.freepascal.org/Developing_Web_Apps_with_Pascal](https://web.archive.org/web/20250324160016/https://wiki.freepascal.org/Developing_Web_Apps_with_Pascal)_
