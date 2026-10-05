# lazarus pas2js integration

│ **English (en)** │  **[русский (ru)](<../ru/lazarus_pas2js_integration.md>)** │

## Contents

  * 1 Lazarus pas2JS integration
    * 1.1 Global options
    * 1.2 Project Wizards
    * 1.3 Web Browser application
    * 1.4 Progressive Web Application
    * 1.5 Electron Web Application
    * 1.6 Atom package options
    * 1.7 VS Code extension options
    * 1.8 Project options
    * 1.9 Debug window
    * 1.10 Navigation



# Lazarus pas2JS integration

Requires **Lazarus 1.9+** and **pas2js** ([see here](<pas2js.md> "pas2js")). Installation procedure: 

1\. Click on menu Package - Install/Uninstall Packages 

    [![pas2jsdsgn install package.png](https://wiki.freepascal.org/images/b/b0/pas2jsdsgn_install_package.png)](</File:pas2jsdsgn_install_package.png>)

2\. Choose the package **pas2jsdsgn** then restart the IDE. 

    [![pas2jsdsgn install](https://upload.wikimedia.org/wikipedia/commons/e/ee/Pas2jsdsgn-install.png)](</File:Pas2jsdsgn-install.png> "pas2jsdsgn install")

The Lazarus [pas2js](<pas2js.md> "pas2js") integration has 3 parts to it: 

  * [Global options](<lazarus_pas2js_integration.md> "lazarus pas2js integration")
  * [Project wizards](<lazarus_pas2js_integration.md> "lazarus pas2js integration")
  * [A debug window](<lazarus_pas2js_integration.md> "lazarus pas2js integration")



## Global options

In the Tools/Options... dialog, the Pas2JS section allows you to set some options that affect the IDE integration. It looks like this: 

[![pas2jsdsgn globaloptions.png](https://wiki.freepascal.org/images/0/06/pas2jsdsgn_globaloptions.png)](</File:pas2jsdsgn_globaloptions.png>)

The following settings are available: 

Path of pas2js
    This is the compiler binary used when setting up a new project.  
It will be searched in the PATH if no absolute path is set. Corresponds to IDE macro _Pas2JSJS_.
    Change **Path of pas2js** to **C:\lazarus\pas2js\bin\win64-x86_64\pas2js.exe**

Path of simpleserver (Lazarus 2.3+ configure this in _Simple Web Server_)
    This is the webserver that is started when a project is run that needs a webserver.  
This is by default the simpleserver application from the FPC project. The program can be found in the FPC sources in the directory _fpcsrc\packages\fcl-web\examples\simpleserver_ and has to be compiled manually.
    You may also use any other webserver, as long as it accepts the -p option to set the path, and it serves files from the directory in which it was started.
    Change **$MakeExe(IDE,simpleserver)** to **C:\lazarus\pas2js\bin\compileserver.exe**

Port number to start allocating from
    Every time you create a new webserver project, a new port number is allocated for the webserver. (you can still edit this in the new project dialog).

Browser to use when opening HTML page (Lazarus 2.3+ configure this in _Simple Web Server_)
    The IDE will use this browser to open your HTML page.  
It will be searched in the PATH if no absolute path is set. Corresponds to IDE macro _Pas2JSBrowser_.

Node.js executable
    The IDE will use this Node.js executable to start a Node.js project.   
It will be searched in the PATH if no absolute path is set. Corresponds to IDE macro _Pas2JSNodeJS_.

Electron executable (since Lazarus 2.3)
    The IDE will use the electron.exe to start an Electron project.   
It will be searched in the PATH if no absolute path is set. Corresponds to IDE macro _Pas2JSElectron_.

Atom package template directory
    The IDE will use some files in this directory when creating an Atom package skeleton.   
There is no default for this.

VS Code extension template directory
    The IDE will use some files in this directory when creating a VS Code extension skeleton.   
There is no default for this.

## Project Wizards

The pas2js support is in the pas2jsdsgn.lpk package, which you can find in the components/pas2js directory. It registers 2 wizards in the 'New project' dialog: 

  * Web Browser Application
  * Node.js Application



[![pas2jsdsgn projects](https://upload.wikimedia.org/wikipedia/commons/1/1c/Pas2jsdsgn_projects.png)](</File:Pas2jsdsgn_projects.png> "pas2jsdsgn projects")

Besides creating an initial project source, both options will also 

  * Set up the compiler command for compiling with pas2js
  * change the 'Run' command so the 'Run without debugging' option works: it will open a project in the browser or run it with nodejs.



## Web Browser application

This wizard will ask for some options before generating a new project. The dialog is shown below. 

[![webbrowserappoptions.png](https://wiki.freepascal.org/images/f/fc/webbrowserappoptions.png)](</File:webbrowserappoptions.png>)

The following options are available: 

  * **Create initial HTML page**. This is self-explanatory,   
the IDE will generate a template HTML page which includes all that is necessary to run the pas2js code.  
The page is included in the project description, so you can open and edit it from within the IDE. 
    * **Maintain HTML page**.  
If you change the name of the project, the IDE will update the references in the HTML File.  
(all your changes will be lost)
    * **Run RTL when all page resources are fully loaded** : By default the script tag that starts the ball rolling will call `rtl.run()`.  
Checking this option changes the script, so the `rtl.run` is run in the HTML `document.onReady` event instead.  
This is necessary if your code contains startup code that references elements in the HTML.   
The elements will only be available after the onReady event.
    * **Let RTL show uncaught exceptions** : catch exceptions and show a dialog instead of normal browser behavior, which needs the developer tools to see an uncaught exception.
    * **Use Browser Console unit.**  
Checking this will simply include the browserconsole unit in the uses clause.   
This hooks into the system units writeln command: any output will be appended to the HTML.   
the generated HTML has a div with the correct ID to which the output is appended.
  * **Use Browser Application object.**  
This changes the code to use the `TBrowserApplication` object.  
This is a `TCustomApplication` descendent which offers support for query parameters etc. as if they were command-line parameters. 
    * **Run webassembly program**  

  * **Run**  
What to do on **Run**
    * **Start HTTP server on port**  
the IDE starts a browser at this port and opens a browser with the URL of the html file
    * **Use this URL to start application**  
the IDE opens a browser with this URL.
    * **Execute Run parameters**  
default Lazarus behavior



## Progressive Web Application

Since Lazarus 2.3 and pas2js 2.2.1 

The **PWA** is like a _Web Browser Application_ , except this wizard creates two projects, the **ServiceWorker.lpi _and_** _YourPWA.lpi_ , a project group, a manifest and some icons. After the options dialog it asks for the file name of the webapp program source filename (usually a *.lpr or *.pas). 

To run it you must compile both projects to create the two needed js files. The project group window helps you to switch easily and compile the projects. 

## Electron Web Application

Since Lazarus 2.3 and pas2js 2.2.1. 

See [pas2js Electron Web Application](<pas2js_Electron_Web_Application.md> "pas2js Electron Web Application")

The **Electron** web app is like a _Web Browser Application_ , except this wizard creates three projects, the **preload.lpi** , the **render.lpi** and **YourApp.lpi** , and a project group. After the options dialog it asks for the file name of the webapp program source filename (usually a *.lpr or *.pas). 

To run it you must compile all three projects to create the three needed js files. The project group window helps you to switch easily and compile all projects. 

## Atom package options

[![pas2jsatomintegration.png](https://wiki.freepascal.org/images/0/07/pas2jsatomintegration.png)](</File:pas2jsatomintegration.png>)

The New Atom Package wizard will create a template for an atom package. 

You must enter the following information: 

  * Directory (it will be created if it does not exist)
  * A description
  * A package name (must contain letters and can contain dashes)
  * A package class name. This is the name of the application class.



The following information is optional: 

  * Commands: a series of command names and command functions. They will be inserted in the application class and code will be created to register them within atom.
  * License: The license to be used. This is inserted in the package.json file.
  * Keywords: Keywords to insert in the package.json file.
  * Activation commands: keystrokes and commands that activate the package.


  * The generated code can be configured, with the following files:


    
    
     * **glue.js** contains the glue code for the Atom package. This is the file that starts the Atom package.
     * **keymap.json** contains the default JSON file with the keymap.
     * **menu.json** contains the default menu file with the menu entries.
     * **package.less** contains the default css file for your package.
     * **project.lpr** contains the default project code.
    

The location for the default files can be set in the global pas2js options dialog. 

In these files, the following placeholders will be substituted: 

  * **%PACKAGENAME%** will be replaced with the package name.
  * **%PACKAGEPROJECTNAME%** will be replaced with the project file name.
  * **%PACKAGEHANDLERINTFS%** the line containing this will be removed and replaced with the declarations of command functions;
  * **%PACKAGEHANDLERINTFS%** the line containing this will be removed and replaced with the implementations of command functions;
  * **%PACKAGEHANDLERREGS%** the line containing this will be removed and replaced with the registration of command functions in Atom or VS Code;



## VS Code extension options

[![pas2jsvscodeintegration.png](https://wiki.freepascal.org/images/a/a0/pas2jsvscodeintegration.png)](</File:pas2jsvscodeintegration.png>)

The New VS Code extension wizard will create a template for a VS Code extension. 

You must enter the following information: 

  * Directory (it will be created if it does not exist)
  * A description
  * A package name (must contain letters and can contain dashes)
  * A publisher name (can only contain letters)
  * A package class name. This is the name of the application class.



The following information is optional: 

  * Commands: a series of command names and command functions that activate the package.
  * License: The license to be used. This is inserted in the package.json file.
  * Keywords: Keywords to insert in the package.json file.
  * Contribution commands: commands that activate the package.


  * The generated code can be configured, with the following files:


    
    
     * **glue.js** contains the glue code for the Atom package. This is the file that starts the Atom package.
     * **tasks.json** contains the code to build the package in VS Code. You can add any commands you see fit.
     * **launch.json** contains the code to launch VS Code with the new package installed.
     * **project.lpr** contains the default project code.
    

The same substitutions as for the Atom package are made. 

The location for the default files can be set in the global pas2js options dialog. 

## Project options

You can change the settings chosen in the new project wizard in the Project Settings dialog. It also allows you to convert any existing (simple) project to a web application project. 

The project options are located at the bottom "Web Project (pas2js)' 

[![pas2jsprojectoptions.png](https://wiki.freepascal.org/images/a/a5/pas2jsprojectoptions.png)](</File:pas2jsprojectoptions.png>)

The checkbox at the top marks the project as web browser project. 

All other options have the same meaning as their counter parts in the 'New project' wizard. 

The ```Reset compile command``` and ```Reset run command``` set the respective commands to their default values. 

## Debug window

As you start projects from various directories, the IDE will start webservers as needed. After some time, there can be several webservers running. An overview of running webservers can be shown using the the View/Debug Windows/Pas2JS Webservers menu item. 

It shows a list of processes started by the IDE: 

[![webservers.png](https://wiki.freepascal.org/images/b/b9/webservers.png)](</File:webservers.png>)

Support for stopping/starting processes will still be added, as well as statistics for number of requests etc. 

## Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [Pas2JS Version Changes](<Pas2JS_Version_Changes.md> "Pas2JS Version Changes")

---

_Source: [https://wiki.freepascal.org/lazarus_pas2js_integration](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/lazarus_pas2js_integration)_
