# TortoiseSvn

## Contents

  * 1 Usage
  * 2 Troubleshooting
    * 2.1 Temporarily add svn directory to path
    * 2.2 Permanently add svn directory to path



## Usage

TortoiseSVN integrates itself with Windows Explorer. After installation, all commands are available from inside Windows Explorer through right-clicking on a directory or file. 

Check out a working copy using TortoiseSVN, go to Run and paste the command below: 
    
    
    tsvn:https://svn.freepascal.org/svn/lazarus/trunk
    

To get Lazarus for the first time: 

  * Open Windows Explorer
  * Create a directory where you want to put the files from the SVN server
  * Right click on the newly created directory
  * Select _SVN Checkout..._ on the popup menu



[![TortoiseSVN Main Menu.png](https://wiki.freepascal.org/images/2/27/TortoiseSVN_Main_Menu.png)](</File:TortoiseSVN_Main_Menu.png>)

  * In the next dialog you enter the URL for Lazarus server source:


    
    
    https://svn.freepascal.org/svn/lazarus/trunk
    

  * Watch how TortoiseSVN imports the file tree from the server. This can take some time to complete.



  
To update Lazarus sources: 

  * Open Windows Explorer
  * Right click on the source directory
  * Select _SVN Update_ on the popup menu



[![TortoiseSVN Main Menu Update.png](https://wiki.freepascal.org/images/6/65/TortoiseSVN_Main_Menu_Update.png)](</File:TortoiseSVN_Main_Menu_Update.png>)

## Troubleshooting

TortoiseSvn includes a svn command line version, too. If you get an error 

  * svn command not found



or similar then most probably you do not have svn.exe in your PATH environment variable. The following steps should fix this problem: 

### Temporarily add svn directory to path

  1. Locate svn.exe using the Find Files feature of your Start menu.
  2. Once you have located svn.exe you need to add the directory it is in to your PATH.



As an example: From the command prompt type: 
    
    
    set PATH=%PATH%;"C:\Program Files\TortoiseSVN\"
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The directory containing svn.exe might not be the same on your computer as the one used in this document. It is used here only as an example.

### Permanently add svn directory to path

While this section is optional, it saves you from changing the path all the time. You can add this directory to your PATH by: 

  1. Right-Clicking on "My Computer"
  2. Select "Properties"
  3. Choose the "Advanced" tab.
  4. Click the "Environment Variables" button.
  5. Locate the "Path" line in "System Variables" and add: ;"C:\Program Files\TortoiseSVN\" to the end.



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Your directory containing svn.exe might not be the same on your computer as the one used in this document. It is used here only as an example.

---

_Source: [https://wiki.freepascal.org/TortoiseSvn](https://web.archive.org/web/20210727052035/https://wiki.freepascal.org/TortoiseSvn)_
