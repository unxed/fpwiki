# UniqueInstance

│ **English (en)** │

## Contents

  * 1 Description
    * 1.1 About
    * 1.2 Features
    * 1.3 How To Use (Component)
    * 1.4 How To Use (Raw)
    * 1.5 Remarks
    * 1.6 History
    * 1.7 Author
    * 1.8 License
    * 1.9 Download
  * 2 See also



## Description

### About

UniqueInstance provides an easy way to force only one instance per application running at same time. To install this component on your component palette, the easiest way is to use the Lazarus [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager"). Supports Windows and Linux (macOS prevents multiple instances by default). 

### Features

  * Easy of use: just drop a component in the main form
  * Provides a mechanism to receive the Command Line of the other instances



### How To Use (Component)

Just drop in the main form of a LCL application (it's on the System tab, look for a red full-circle with the digit "1" in the middle). 

Properties: 

  * Enabled: enables/disables the component
  * Identifier: used to provides a way to identify the application
  * UpdateInterval: the interval in milliseconds which the component will monitor messages from new instances. **Has meaning only under unix since, under win32, the message is received through the windows message loop**



Event 

  * OnOtherInstance: called when another instance is initiated. Receives the parameters passed to that instance.



### How To Use (Raw)

Add **uniqueinstanceraw** unit to your uses section. 

Call the _InstanceRunning_ function: it will return true if there's already a instance running. 

There are two variants: 

  * Without arguments: will use the executable name as identifier and will not send the command line parameters
  * With two arguments: 
    * Identifier: the identifier of the application
    * SendParameters(default = false): send the command line parameters to the already running instance, if any, before quit.



### Remarks

  * Tested with win32 (XP SP2) and Linux (Ubuntu 9.04) with fpc 2.4.2.
  * If you put two TUniqueInstance components in the same application with the same identifier your application won't load.
  * The Identifier is optional both to the function and the component. In the case it's not used, the executable name is used as an Identifier. The consequence is that if someone changes the exe name another instance will not be identified. In the other hand, if two different applications use the same Identifier one will prevent the other from launching 
    * TIP: to make sure an application will not prevent another from launching you can use a GUID as the Identifier



### History

  * 11/01/17 - Version 1.1 
    * Make compatible with fpc 3.0 and above
    * Code cleanup
  * 17/04/11 - Version 1.0 
    * Fix application being detected as running after a crash under unix
    * Fix crash when compiling with Gtk2 widgetset
    * Set default values of published properties
    * Optimizations and code clean up
    * New icon
  * 02/10/07 - Version 0.2 
    * Based in fpc 2.2.0
    * Implemented parameter reception under unix
    * General optimizations and code clean up
  * 16/12/06 - Initial release. See [my blog](<http://lazarusroad.blogspot.com/2006/12/only-one-instance.html>) to know how it began.



### Author

[Luiz Américo Pereira Câmara](</User:Luizmed> "User:Luizmed")

### License

Modified LGPL 

  


### Download

Version 1.1: [Download from GitHub](<https://github.com/blikblum/luipack/releases/tag/uniqueinstance-1.1>)

Recent development moved to GitHub repository: <https://github.com/blikblum/luipack/tree/master/uniqueinstance>

You can use SVN client: 
    
    
     svn co https://github.com/blikblum/luipack.git/trunk/uniqueinstance
    

## See also

  * [AppIsRunning](<AppIsRunning.md> "AppIsRunning")

---

_Source: [https://wiki.freepascal.org/UniqueInstance](https://web.archive.org/web/20250123161544/https://wiki.freepascal.org/UniqueInstance)_
