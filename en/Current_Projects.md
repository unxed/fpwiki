# Current Projects

### From Lazarus-ccr

(Redirected from [Current Projects](</index.php?title=Current_Projects&redirect=no> "Current Projects"))

[**Deutsch (de)**](</Current_conversion_projects/de> "Current conversion projects/de") | ****English (en)**** | [**Français (fr)**](</Current_conversion_projects/fr> "Current conversion projects/fr") | [**Bahasa Indonesia (id)**](</Current_conversion_projects/id> "Current conversion projects/id") | [**Bahasa Indonesia (id)**](</Current_conversion_projects/id> "Current conversion projects/id") | [**中文（简体）(zh_CN)**](</Current_conversion_projects/zh_CN> "Current conversion projects/zh CN") | [**正體中文 (zh_TW)**](</Current_conversion_projects/zh_TW> "Current conversion projects/zh TW")

  
This page contains a list of applications and components, that are currently being converted. If the conversion has been finished (or before if you want more user feedback), the components can be moved to [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples") and the applications to [Projects using Lazarus](<Projects_using_Lazarus.md> "Projects using Lazarus"). If a description page has been made, the applications or components can be offered for download at the [sourceforge files area](<http://sourceforge.net/project/showfiles.php?group_id=92177> "http://sourceforge.net/project/showfiles.php?group_id=92177"). 

## Contents

  * 1 Applications
    * 1.1 osFinancials
  * 2 Components
    * 2.1 Large Display Components
    * 2.2 Indy
    * 2.3 FormStorage
    * 2.4 PowerPDF for Lazarus
    * 2.5 tiOPF GUI controls
    * 2.6 TeeChart
    * 2.7 AnyDAC
  * 3 Libraries
    * 3.1 dxGetText
    * 3.2 Pascal Script
    * 3.3 GraphicEx
    * 3.4 Graphics32
  * 4 Requested Components
    * 4.1 devphp
    * 4.2 ZEOS Data Objects
    * 4.3 Usercontrol
    * 4.4 AutoREALM
    * 4.5 Toolbar 2000
    * 4.6 Report Manager
    * 4.7 Open XML
    * 4.8 Other applications, libraries and components

  
---  
  
## [[edit](</index.php?title=Current_conversion_projects&action=edit&section=1> "Edit section: Applications")] Applications

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=2> "Edit section: osFinancials")] osFinancials

The port of this open source project will not be easy, but Rome was not build in one day. The new version does allow interacting with the database through the SQL db components. I have created an example to create a plugin for osFinancials. I had some problems with the new components, but I am sure all this will disappear with time and one day I can fully compile the project in Lazarus. I do have a need for the memdataset to be able to mimic the clientdataset. This will need an XML parser (I am thinking of TJanXmlTree from Jan Verhoeven) and the dataset will need to support blobdata. I will try to see if i can implement this and use the component in Delphi and Lazarus. I will use this component to write the external links to PHP websites (like the osCommerce plugin and the new one I am making for V-Tiger). I use Clientdata set just as a memdataset in the code but I also need the part where the XML datapacket is translated to the dataset and the ability to save to this format. 

[Delphidreamer](</User:Delphidreamer> "User:Delphidreamer")

  


## [[edit](</index.php?title=Current_conversion_projects&action=edit&section=3> "Edit section: Components")] Components

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=4> "Edit section: Large Display Components")] Large Display Components

Almost finished: 

  * TLCD99 
  * TLCDLabel 
  * TAnalogueclock 



Everything compiles for SGraph 2.4 but the license for it restricts redistribution of modified source code. The author has been contacted in the hopes that this can be changed. 

I've also converted a small trend recorder variant of Mark Dodson's original but I'm thinking about rewriting it. If anyone is interested in any of these components, let me know. - [VlxAdmin](</User:VlxAdmin> "User:VlxAdmin")

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=5> "Edit section: Indy")] Indy

[Internet Direct (Indy)](<http://www.indyproject.org/> "http://www.indyproject.org/") is an open source TCP/IP socket component suite comprised of popular Internet protocols. For more info see [indy4lazarus](<http://indy4lazarus.sourceforge.net/> "http://indy4lazarus.sourceforge.net/"). 

Newer attempts are done by Marco van de Voort. For more (status) info see [Indy with Lazarus](<Indy_with_Lazarus.md> "Indy with Lazarus")

Current snapshots (for die hards only) are at [Indy9](<http://www.stack.nl/~marcov/indy9.zip> "http://www.stack.nl/~marcov/indy9.zip") and [Indy10](<http://www.stack.nl/~marcov/Indy10FPC.zip> "http://www.stack.nl/~marcov/Indy10FPC.zip")

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=6> "Edit section: FormStorage")] FormStorage

[FormStorage](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986> "http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986") is a component to save all selected properties of a form in an xml file. 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=7> "Edit section: PowerPDF for Lazarus")] PowerPDF for Lazarus

[Original PowerPDF site](<http://www.est.hi-ho.ne.jp/takeshi_kanno/powerpdf/index.html> "http://www.est.hi-ho.ne.jp/takeshi_kanno/powerpdf/index.html") PowerPdf is a LCL suite of components to create PDF documents visually. With this components you can design PDF documents easily on Lazarus IDE. Based in PowerPDF version 0.9, status is 95% finished. -[jesusrmx](</User:Jesusrmx> "User:Jesusrmx")

[Chtk](</User:Chtk> "User:Chtk") also started a port of PowerPdf for Lazarus. The result of this effort has been merged with the port done by jesusrmx. A package, which should have all the functionality of the Delphi version, is availabe [here](<http://iquad.nl/files/powerpdf/> "http://iquad.nl/files/powerpdf/"). 

[Xno](</User:Xno> "User:Xno") have port some example of PowerPdf to Lazarus. This code is available [here](<http://xoomer.virgilio.it/xno/xnocbt.html> "http://xoomer.virgilio.it/xno/xnocbt.html"). 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=8> "Edit section: tiOPF GUI controls")] tiOPF GUI controls

[Bogusław Brandys](</index.php?title=User:Forest&action=edit> "User:Forest") started a port of tiOPF Persistent Aware ([TechInsite tiOPF site](<http://tiopf.sourceforge.net/> "http://tiopf.sourceforge.net/")) GUI controls for Lazarus. At current stage it simply compile and install into IDE. Any help is appreciated especially deeper knowledge about components creation for Lazarus. 

TODO: 

  * remove all message handlers from components ,replace with appropriate anchoring and sizing(controls look ugly now) 
  * fix problems with AV when deleting subcontrols (tiOPF GUI controls are composite) 
  * fix problems with tiLVTreeView/tiLVListView 



### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=9> "Edit section: TeeChart")] TeeChart

The commercially reliable and robust TeeChart component has been ported to Lazarus. Some functionality is still to be addressed, but the majority works. 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=10> "Edit section: AnyDAC")] AnyDAC

[AnyDAC](<http://www.da-soft.com/anydac/> "http://www.da-soft.com/anydac/") is a commercial data access library. It has been ported to Lazarus. AnyDAC supports Firebird, MySQL, Oracle, PostgreSQL, SQLite, Interbase, SQL Server, IBM DB2, SQL Anywhere and ODBC on Windows and Linux 32bit platforms. The MS Access and dbExpress are supported on Win32 platform only. In plans to add all drivers support on Win x64, Linux x64, MacOS 32bit and x64 platforms. 

## [[edit](</index.php?title=Current_conversion_projects&action=edit&section=11> "Edit section: Libraries")] Libraries

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=12> "Edit section: dxGetText")] dxGetText

[ Lazarus dxGetText](<DxGetText.md> "DxGetText") is a conversion by [Olivier Guilbaud](<http://sourceforge.net/users/golivier/> "http://sourceforge.net/users/golivier/") of the [dxGetText project](<http://dybdahl.dk/dxgettext/> "http://dybdahl.dk/dxgettext/"). From the dxGetText website: "Initially, this project used a Windows port of the GNU gettext library, but has made it much further and today it is a complete reimplementation of the GNU gettext library with many enhancements". 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=13> "Edit section: Pascal Script")] Pascal Script

[Pascal Script](<http://wiki.lazarus.freepascal.org/index.php/Pascal_Script> "http://wiki.lazarus.freepascal.org/index.php/Pascal_Script") is a REMObjects Pascal Script interpreter ([RemObjects Pascal Script home page](<http://www.remobjects.com/page.asp?id={9A30A672-62C8-4131-BA89-EEBBE7E302E6}> "http://www.remobjects.com/page.asp?id={9A30A672-62C8-4131-BA89-EEBBE7E302E6}")) ported to Lazarus. It works on Win32 and Linux and is 100% completed (maybe even without bugs). Some fixes have been done by Boguslaw Brandys. Additional Testers are welcome, especially under Linux. For screenshots see: [under Windows](<http://wiki.lazarus.freepascal.org/index.php/Image:Rops_windows.png> "http://wiki.lazarus.freepascal.org/index.php/Image:Rops_windows.png") and [under Linux](<http://wiki.lazarus.freepascal.org/index.php/Image:Rops_linux.png> "http://wiki.lazarus.freepascal.org/index.php/Image:Rops_linux.png")

  
Sources have been sent to the author (Carlo Kok) and are now available from RemObjects SVN. I hope it will also be available in the Lazarus installer as additional component. \--[Forest](</index.php?title=User:Forest&action=edit> "User:Forest") 12:22, 19 Oct 2005 (CEST) 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=14> "Edit section: GraphicEx")] GraphicEx

The fantastic GraphicEx package from <http://www.delphi-gems.com/> has been adapted and enhanced by theo. See [http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635> "http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635"). 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=15> "Edit section: Graphics32")] Graphics32

Graphics32 is a graphics library for Delphi and Kylix/CLX. Optimized for 32-bit pixel formats, it provides fast operations with pixels and graphic primitives. In most cases Graphics32 considerably outperforms the standard TBitmap/TCanvas methods. 

A team started the port of this library to Free Pascal and Lazarus. The lcl-win32 port is almost complete. The lcl-carbon port is about 50% finished. 

Documentation for this library can be found here: [[1]](<http://graphics32.org/documentation/Docs/_Body.htm> "http://graphics32.org/documentation/Docs/_Body.htm")

## [[edit](</index.php?title=Current_conversion_projects&action=edit&section=16> "Edit section: Requested Components")] Requested Components

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=17> "Edit section: devphp")] devphp

[devphp](<http://sourceforge.net/projects/devphp/> "http://sourceforge.net/projects/devphp/") is an IDE for PHP written in Delphi/Kylix. It's got a lot of nice features and would be very handy to have compiling under Lazarus. The author has run out of time to work on it so it would probably be a good candidate for conversion. [Tom](</User:VlxAdmin> "User:VlxAdmin")

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=18> "Edit section: ZEOS Data Objects")] ZEOS Data Objects

[ZEOS Data Objects](<http://sourceforge.net/projects/zeoslib> "http://sourceforge.net/projects/zeoslib") is a set of components for accessing directly the various database backends that you might need to. MySQL, Postgres and others, on Delphi it compiles directly into the .exe, only requiring that you have the appropriate DLL installed (postgres.dll, mysql.dll). ~~It would be fabulous to get these components working under Lazarus as we can then write real database apps quickly~~. [User:MartynRanyard](</User:MartynRanyard> "User:MartynRanyard")

**Note:** this functionality is being implemented in the sqldb-components. Not as good as ZEOS yet, but worth a look. [User:Loesje](</User:Loesje> "User:Loesje")

**Note 2:** The conversion of these components has recently been completed. Take a look at [ZEOS Data Objects](<http://sourceforge.net/projects/zeoslib> "http://sourceforge.net/projects/zeoslib") and download ZEOSDBO_REWORK package from CVS. Also check this [Tutorial](<Zeos_tutorial.md> "Zeos tutorial") [Matthijs](</User:Matthijs> "User:Matthijs")

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=19> "Edit section: Usercontrol")] Usercontrol

[Usercontrol](<http://sourceforge.net/projects/usercontrol> "http://sourceforge.net/projects/usercontrol") Delphi (and Kylix) component package to user and profile management and access control. Supports ADO, DBX, IBX, BDE, IBO, FIBPlus, ZeosDBO, DBISAM, MDO, MyDAC, MySQLDAC and ASTA3. Access control auto-extract TMenu, TActionList and TActionManager items. And MODULE for UIB component. 

HOW IS THE DEVELOPMENT OF IT ??? USERCONTROL rocks... 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=20> "Edit section: AutoREALM")] AutoREALM

AutoREALM ( <http://autorealm.sourceforge.net> ) is a free (GNU) Fantasy Role-Playing mapper software. It is "developed with Delphi Personal Edition™ (from Borland Inc.) and based on the simple and natural TurboPascal™ language, AutoREALM could be coded as well with Kylix Open Edition™ to run on LINUX platforms.". Well, it doesn't really compile on Linux, but a port to Lazarus that could also compile for Linux, Mac et.c. would be very nice. Currently there is a project to port AutoREALM to C++ and then to Linux, but a Lazarus port would perhaps be easier. 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=21> "Edit section: Toolbar 2000")] Toolbar 2000

Toolbar 2000 ( <http://www.jrsoftware.org/tb2k.php> ) is "a set of components for Borland Delphi and C++Builder (4.0 and later) designed to mimic the look and behavior of Office 2000's menus and toolbars.". Available under either a commercial license or the GNU General Public License. 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=22> "Edit section: Report Manager")] Report Manager

[Report Manager](<http://reportman.sourceforge.net> "http://reportman.sourceforge.net") Component for creating reports from database with visual editor,band support,conditional printing,evaluating saving to XLS,PDF,HTML. 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=23> "Edit section: Open XML")] Open XML 

[Open XML](<http://www.philo.de/xml/> "http://www.philo.de/xml/") is "a collection of XML and Unicode tools and components for the Delphi/Kylix™ programming language. All packages are freely available including source code." 

### [[edit](</index.php?title=Current_conversion_projects&action=edit&section=24> "Edit section: Other applications, libraries and components")] Other applications, libraries and components

**Add an application, library or component that you need here**

The Delphi IDE, but not Kylix, has a File -> Print selection which allows either the current source file or the current Form to be sent to the printer. I know there are lots of ways in both Linux and Windows to have files and screenshots printed, but it would be a convenience to be able to initiate printing direct from the IDE, with all the formatting and highlighting that you see on the screen. [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")

Support for Paradox and Access databasing (ADO, DAO or ODBC) in at least a Win32 environment. A package named KADao implements this and is free in Delphi, maybe someone can translate this. If databasing is already implemented, maybe a way for new users to find it???[User:Micdutoit](</index.php?title=User:Micdutoit&action=edit> "User:Micdutoit")

A way to interface with Python would be nice. In Delphi a package PythonForDelphi does this. Can someone translate this one? [User:Micdutoit](</index.php?title=User:Micdutoit&action=edit> "User:Micdutoit")

I'm working on "Lazapy" (Python for Lazarus), this ported version of PythonForDelphi will be available as soon as possible with demos (I've forgot the demo&screenshots at home!). I would like to Inform you that it is in a very Beta state, but I've compiled Python scripts successfuly except some few exceptions(related to dynamic linking). [User:Ghany](</index.php?title=User:Ghany&action=edit> "User:Ghany")

I am looking at Lazarus basically because I want to wrap some Python programs in Lazarus shells. Rather than start with the reinvention of the wheel, I would much rather contribute to the development of "Lazapy", no matter how "beta" it is. But where is Ghany? [User:OldAl](</User:OldAl> "User:OldAl")

I wish JCL and JVCL ported to lazarus. Also I need some sort of components like [Developer Express (c)](<http://www.devexpress.com/> "http://www.devexpress.com/") to completly leave Delphi and get Lazarus. I need the cxLayoutControl and all related components. Do you know any packages that are like they? 

**MUTIS** is looking for help to provide a multi-plataform layer to cross-compiling to .NET (current) Win32 and Linux. I think lazarus is a best target than Kilyk. I have a start but need help to get rid fo .NET specific things. 

The project is at <http://sourceforge.net/projects/mutis> and the mailing list <http://groups.google.com.co/group/mutis-developers?lnk=li>

MUTIS is a search and indexing engine based in Lucene. Is done at 80% at API 1.4 level. I think is great have this tech on Delphi and make the project the first all native, all multiplataform, one language, in their class. 

I wish OpenBSP (part of GLScene) to be ported. The project of porting GLScene to Lazarus ([GLScene](<GLScene.md> "GLScene")) seems not to include the also provided OpenBSP. -- [User:BrainChemistry](</User:BrainChemistry> "User:BrainChemistry"), 14 Feb 2008 

    

  * It is not mentioned on the wiki page but the code is included in the repo. However AFAIK nobody touched that code since it was put into that repo. (more details on [your user talk page](</User_talk:BrainChemistry> "User talk:BrainChemistry")). regards --[Crossbuilder](</User:Crossbuilder> "User:Crossbuilder") 12:41, 16 February 2008 (CET) 



**Inno Unpacker** <http://sourceforge.net/projects/innounp/> \- is the only known tool to extract Inno installers. It is highly desired to get ported version for automatic build of updates [for Wesnoth](<http://www.wesnoth.org/forum/viewtopic.php?p=284681#284681> "http://www.wesnoth.org/forum/viewtopic.php?p=284681#284681"). --[Skipass](</index.php?title=User:Skipass&action=edit> "User:Skipass") 12:14, 2 March 2008 (CET) 

DevExpress components are very big and very good. It would be nice to be translated or if there is any other substitution for them. I already have project hevily involved this component and substituting is not my first opton. Milan. 

[**Inno Setup**](<http://en.wikipedia.org/wiki/InnoSetup> "http://en.wikipedia.org/wiki/InnoSetup") is a tool used to create Lazarus installers for Windows. If Inno Setup is ported to FPC, the whole Lazarus package can be cross-compiled on Linux box. User cross-platform applications can benefit from this too. There is also thread in official [jrsoftware.innosetup.code](<http://news.jrsoftware.org/read/article.php?id=19240&group=jrsoftware.innosetup.code#19240> "http://news.jrsoftware.org/read/article.php?id=19240&group=jrsoftware.innosetup.code#19240") newsgroup about the same topic.

---

_Source: [https://wiki.freepascal.org/Current_Projects](https://web.archive.org/web/20110609083035/https://wiki.freepascal.org/Current_Projects)_
