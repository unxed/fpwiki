# OnGuard

│ **English (en)** │  **[português (pt)](</OnGuard/pt> "OnGuard/pt")** │    
****

## Contents

  * 1 About
  * 2 License
  * 3 Download
    * 3.1 Sourceforge version
    * 3.2 Alternative version
  * 4 Dependencies / System Requirements
  * 5 Status
  * 6 Support
  * 7 Issues
  * 8 Installation
  * 9 Usage
  * 10 Screenshots
  * 11 To Do / Future improvements



### About

This version of OnGuard is port of Turbo Power OnGuard. OnGuard is a library to create demo versions of your Delphi, Kylix and Free Pascal applications. Create demo versions that are time-limited, feature-limited, limited to a certain number of uses, or limited to a certain number of concurrent network users. You could also protect executable file from modification, either by hacker or malware. Most functionality has been tested to work under Windows, Linux and FreeBSD. 

It has now been designed for cross-platform applications (Windows, Linux, FreeBSD etc). Pure Object Pascal. However some features are heavy system dependant. OnGuard doesn't depend on the Lazarus LCL, so you could also use it in Command Line, Web or other GUI toolkit applications. 

OnGuard is distributed as a source-only package. The download contains the component package for Lazarus + documentation (pdf and hlp file) and probably all original examples ported to Lazarus. 

### License

[MPL 1.1](<http://www.mozilla.org/MPL/MPL-1.1.html>)

### Download

#### Sourceforge version

The Lazarus version of OnGuard is hosted on [SourceForge](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/onguard/>). Use `SVN` to get the most recent version, or download a zipped snapshot from the same site (<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/onguard/>). 

This port orginates back to 2006 and had been abandoned for quite some time. In 2023, however, it received some new updates, and now works with Laz 2.3 / FPC 3.2.2. 

#### Alternative version

Since the OnGuard package on Sourceforge was abandoned for some time, Graeme Geldenhuys had forked the code and started to maintain his own version. Please see his forum message for the reasons why he forked OnGuard: [forum message by Graeme](<http://forum.lazarus.freepascal.org/index.php/topic,23495.msg140693.html#msg140693>)

The code of the alternative version is hosted on Graeme Geldenhuys' [GitHub site](<https://github.com/graemeg/onguard>) from which it can be cloned by the usual `GIT` commands. Alternatively, you can download a zipped archive from [[1]](<https://github.com/graemeg/onguard/archive/master.zip>). 

The alternative version is also distributed by the [Online-Package-Manager](<Online_Package_Manager.md> "Online Package Manager"). 

### Dependencies / System Requirements

  * None



### Status

  * Stable



### Support

A dedicated support newsgroup has been set up for the Free Pascal port of OnGuard. Connection details are as follows: 

| Details   
---|---  
**NNTP Server** | geldenhuys.co.uk   
**Port** | 119   
**Group** | onguard.talk   
  
Any News Client (eg: Mozilla Thunderbird, XanaNews, Opera Mail etc) can be used to connect to the news group. This is by far the best option and gives you the freedom to use your preferred news client software. 

In a pinch, there is also a HTML webnews interface. This interface has some limitations (eg: attachments), but is good enough to read and reply to messages when on the go via a web browser (smartphone or desktop). To access the HTML interface, visit the following URL: [<http://geldenhuys.co.uk/webnews/>] 

### Issues

  * Needs testing to see if this version is compatible with the original OnGuard. But maybe this isn't really an issue anyway.



### Installation

  * Create the directory lazarus\components\tponguard
  * Ungzip+untar files into the directory
  * Open lazarus
  * Open the package tponguard.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install



### Usage

Read the manual. It's very detailed and contains a lot of samples. 

### Screenshots

[![](https://wiki.freepascal.org/images/c/cc/OnGuard_Code_Generation.png)](</File:OnGuard_Code_Generation.png>)

The Code Generation demo included with OnGuard. This screenshot shows a date range restriction, and a key encode modifier, unlocking the application for a specific PC only. This demo uses the fpGUI Toolkit.

### To Do / Future improvements

Here follows a list of possible things to extend or improve OnGuard with: 

  * Test network usage code (NAF file is not locked?)
  * Better machine ID (based on CPU (mostly unavailable) and HDD physical serial numbers and MAC address for example)
  * Merge it with HASP USB keys support and others (plugin system?)

---

_Source: [https://wiki.freepascal.org/OnGuard](https://web.archive.org/web/20240121085443/https://wiki.freepascal.org/OnGuard)_
