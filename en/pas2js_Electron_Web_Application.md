# Electron Web Application

Since Lazarus 2.3 and pas2js 2.2.1. 

# Install Electron

First you must install **node.js**. 

  * Windows, MacOS: <https://nodejs.org/en/download/>
  * Debian, Ubuntu:


    
    
    sudo apt install nodejs npm
    

Check that node and npm work: 
    
    
    node -v
    npm -v
    

Create some directory and install electron there: 
    
    
    mkdir mynodejs
    cd mynodejs
    npm install --save-dev electron
    

This should create a sub folder _node_modules_ containing electron: 
    
    
    C:\Whatever\mynodejs\node_modules\electron\dist\electron.exe
    

Set the full path to this executable in _Lazarus Tools / Options / pas2js / Electron executable_

# Lazarus Electron Web Application

Install pas2jsdgn package. For more information see [here](<lazarus_pas2js_integration.md> "lazarus pas2js integration"). 

You get a new project type **Electron Web Application** , which is like a _Web Browser Application_ , except this wizard creates three projects, the **preload.lpi** , the **render.lpi** and **YourApp.lpi** , and a project group. 

After the options dialog it asks for the file name of the webapp program source filename (usually a *.lpr or *.pas). Choose some name, you can later rename it. 

To run it you must compile all three projects to create the three needed js files. The project group window helps you to switch easily and compile all projects.

---

_Source: [https://wiki.freepascal.org/pas2js_Electron_Web_Application](https://web.archive.org/web/20250427000823/https://wiki.freepascal.org/pas2js_Electron_Web_Application)_
