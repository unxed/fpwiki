# Current conversion projects

│ **[Deutsch (de)](</Current_conversion_projects/de> "Current conversion projects/de")** │  **English (en)** │  **[français (fr)](</Current_conversion_projects/fr> "Current conversion projects/fr")** │  **[Bahasa Indonesia (id)](</Current_conversion_projects/id> "Current conversion projects/id")** │  **[한국어 (ko)](</Current_conversion_projects/ko> "Current conversion projects/ko")** │  **[русский (ru)](<../ru/Current_conversion_projects.md> "Current conversion projects/ru")** │  **[中文（中国大陆） (zh_CN)](</Current_conversion_projects/zh_CN> "Current conversion projects/zh CN")** │  **[中文（臺灣） (zh_TW)](</Current_conversion_projects/zh_TW> "Current conversion projects/zh TW")** │    
****

  
This page contains a list of applications and components, that are currently being converted. If the conversion has been finished (or before if you want more user feedback), the components can be moved to [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples") and the applications to [Projects using Lazarus](<Projects_using_Lazarus.md> "Projects using Lazarus"). If a description page has been made, the applications or components can be offered for download at the [sourceforge files area](<http://sourceforge.net/project/showfiles.php?group_id=92177>). 

## Contents

  * 1 Applications
  * 2 Components
    * 2.1 Large Display Components
    * 2.2 Indy
    * 2.3 FormStorage
    * 2.4 PowerPDF for Lazarus
    * 2.5 tiOPF GUI controls
    * 2.6 TurboPower FlashFiler
  * 3 Libraries
    * 3.1 dxGetText
    * 3.2 GraphicEx
    * 3.3 Graphics32
  * 4 Requested Components
    * 4.1 devphp
    * 4.2 Usercontrol
    * 4.3 AutoREALM
    * 4.4 Toolbar 2000
    * 4.5 Report Manager
    * 4.6 Open XML
    * 4.7 Other applications, libraries and components
  * 5 Abandoned
    * 5.1 osFinancials



## Applications

None currently. 

## Components

### Large Display Components

Almost finished: 

  * TLCD99
  * TLCDLabel
  * TAnalogueclock



Everything compiles for SGraph 2.4 but the license for it restricts redistribution of modified source code. The author has been contacted in the hopes that this can be changed. 

I've also converted a small trend recorder variant of Mark Dodson's original but I'm thinking about rewriting it. If anyone is interested in any of these components, let me know. - [VlxAdmin](</User:VlxAdmin> "User:VlxAdmin")

### Indy

[Internet Direct (Indy)](<http://www.indyproject.org/>) is an open source TCP/IP socket component suite comprised of popular Internet protocols. For more info see [indy4lazarus](<http://indy4lazarus.sourceforge.net/>). 

Newer attempts are done by Marco van de Voort. For more (status) info see [Indy with Lazarus](<Indy_with_Lazarus.md> "Indy with Lazarus")

Current snapshots (for die hards only) are at [Indy9](<http://www.stack.nl/~marcov/indy9.zip>) and [Indy10](<http://www.stack.nl/~marcov/Indy10FPC.zip>)

### FormStorage

[FormStorage](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986>) is a component to save all selected properties of a form in an xml file. 

### PowerPDF for Lazarus

[Original PowerPDF site](<http://www.est.hi-ho.ne.jp/takeshi_kanno/powerpdf/index.html>) PowerPdf is a LCL suite of components to create PDF documents visually. With this components you can design PDF documents easily on Lazarus IDE. Based in PowerPDF version 0.9, status is 95% finished. -[jesusrmx](</User:Jesusrmx> "User:Jesusrmx")

[Chtk](</User:Chtk> "User:Chtk") also started a port of PowerPdf for Lazarus. The result of this effort has been merged with the port done by jesusrmx. A package, which should have all the functionality of the Delphi version, is availabe [here](<http://iquad.nl/files/powerpdf/>). 

[Xno](</User:Xno> "User:Xno") have port some example of PowerPdf to Lazarus. This code is available [here](<http://xoomer.virgilio.it/xno/xnocbt.html>). 

### tiOPF GUI controls

[Bogusław Brandys](</index.php?title=User:Forest&action=edit&redlink=1> "User:Forest \(page does not exist\)") started a port of tiOPF Persistent Aware ([TechInsite tiOPF site](<http://tiopf.sourceforge.net/>)) GUI controls for Lazarus. At current stage it simply compile and install into IDE. Any help is appreciated especially deeper knowledge about components creation for Lazarus. 

TODO: 

  * remove all message handlers from components ,replace with appropriate anchoring and sizing(controls look ugly now)
  * fix problems with AV when deleting subcontrols (tiOPF GUI controls are composite)
  * fix problems with tiLVTreeView/tiLVListView



### TurboPower FlashFiler

Converted for Windows target. [FlashFiler](<FlashFiler.md> "FlashFiler")

## Libraries

### dxGetText

[ Lazarus dxGetText](<DxGetText.md> "DxGetText") is a conversion by [Olivier Guilbaud](<http://sourceforge.net/users/golivier/>) of the [dxGetText project](<http://dybdahl.dk/dxgettext/>). From the dxGetText website: "Initially, this project used a Windows port of the GNU gettext library, but has made it much further and today it is a complete reimplementation of the GNU gettext library with many enhancements". 

### GraphicEx

The fantastic GraphicEx package from <http://www.delphi-gems.com/> has been adapted and enhanced by theo. See ~~[http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635>)~~. 

  * See also: <https://github.com/Wormnest/graphicex>



### Graphics32

Graphics32 is a graphics library for Delphi and Kylix/CLX. Optimized for 32-bit pixel formats, it provides fast operations with pixels and graphic primitives. In most cases Graphics32 considerably outperforms the standard TBitmap/TCanvas methods. 

A team started the port of this library to Free Pascal and Lazarus. The lcl-win32 port is almost complete. The lcl-carbon port is about 50% finished. 

Documentation for this library can be found here: [[1]](<http://graphics32.org/documentation/Docs/_Body.htm>)

## Requested Components

### devphp

[devphp](<http://sourceforge.net/projects/devphp/>) is an IDE for PHP written in Delphi/Kylix. It's got a lot of nice features and would be very handy to have compiling under Lazarus. The author has run out of time to work on it so it would probably be a good candidate for conversion. [Tom](</User:VlxAdmin> "User:VlxAdmin")

### Usercontrol

[Usercontrol](<http://sourceforge.net/projects/usercontrol>) Delphi (and Kylix) component package to user and profile management and access control. Supports ADO, DBX, IBX, BDE, IBO, FIBPlus, ZeosDBO, DBISAM, MDO, MyDAC, MySQLDAC and ASTA3. Access control auto-extract TMenu, TActionList and TActionManager items. And MODULE for UIB component. 

### AutoREALM

AutoREALM ( <http://autorealm.sourceforge.net> ) is a free (GNU) Fantasy Role-Playing mapper software. It is "developed with Delphi Personal Edition™ (from Borland Inc.) and based on the simple and natural TurboPascal™ language, AutoREALM could be coded as well with Kylix Open Edition™ to run on LINUX platforms.". Well, it doesn't really compile on Linux, but a port to Lazarus that could also compile for Linux, Mac et.c. would be very nice. Currently there is a project to port AutoREALM to C++ and then to Linux, but a Lazarus port would perhaps be easier. 

### Toolbar 2000

Toolbar 2000 ( <http://www.jrsoftware.org/tb2k.php> ) is "a set of components for Borland Delphi and C++Builder (4.0 and later) designed to mimic the look and behavior of Office 2000's menus and toolbars.". Available under either a commercial license or the GNU General Public License. 

### Report Manager

[Report Manager](<http://reportman.sourceforge.net>) Component for creating reports from database with visual editor,band support,conditional printing,evaluating saving to XLS,PDF,HTML. 

### Open XML

[Open XML](<http://www.philo.de/xml/>) is "a collection of XML and Unicode tools and components for the Delphi/Kylix™ programming language. All packages are freely available including source code." 

### Other applications, libraries and components

**Add an application, library or component that you need here**

* * *

Delphi 10 TListBoxGroupHeader for TListBox; it needs to be able to be a parent for a TComboBox. Contact: [trev](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=64003>) 20190617 

* * *

Existing TDBLookupComboBox, but enhanced by a IsFiltered property ([Screenshot example](<https://www.drupal.org/files/project-images/select2element_screen.jpg>)). If true, the control should reduce its list of visible (and hence selectable) values in the dropdown list by filtering the DataField by the current DBLookupComboBox while editing. This means, onChange (resulting in DBLookupComboBox.Text=someString) automatically reduces the list to 'SELECT lookup_field FROM lookup_table WHERE lookup_field LIKE %'+someString+'%'. This would greatly enhance the usability of Lazarus made database applications. Similar projects "elsewhere": [[2]](<https://www.freelancer.com/projects/Delphi/Custom-Delphi-TDBLookupComboBox/>) [[3]](<https://www.devexpress.com/Support/Center/Question/Details/B196085/filtering-dataset-of-tcxdblookupcombobox>) [[4]](<https://www.drupal.org/sandbox/jauzah/1748434>)

* * *

~~Support for Paradox and Access databasing (ADO, DAO or ODBC) in at least a Win32 environment. A package named KADao implements this and is free in Delphi, maybe someone can translate this. If databasing is already implemented, maybe a way for new users to find it???[User:Micdutoit](</index.php?title=User:Micdutoit&action=edit&redlink=1> "User:Micdutoit \(page does not exist\)")~~

    Paradox dataset is supplied with Lazarus. MS Access can be accessed via [ODBCConn](<ODBCConn.md> "ODBCConn") supplied with Lazarus. --[BigChimp](</User:BigChimp> "User:BigChimp") 16:43, 23 October 2014 (CEST)

* * *

I wish **JCL and JVCL** ported to lazarus. Also I need some sort of components like [Developer Express (c)](<http://www.devexpress.com/>) to completely leave Delphi and get Lazarus. I need the cxLayoutControl and all related components. Do you know any packages that are like they? 

* * *

The **MUTIS** full text search engine project is looking for help to provide a multi-plataform layer to cross-compiling to .NET (current) Win32 and Linux. I think lazarus is a better target than Kilyk. I have a start but need help to get rid fo .NET specific things. 

The project is at <http://sourceforge.net/projects/mutis> and the mailing list <http://groups.google.com.co/group/mutis-developers?lnk=li>

MUTIS is a search and indexing engine based in Lucene. Is done at 80% at API 1.4 level. I think is great have this tech on Delphi and make the project the first all native, all multiplataform, one language, in their class. 

* * *

I wish **OpenBSP** (part of GLScene) to be ported. The project of porting GLScene to Lazarus ([GLScene](<GLScene.md> "GLScene")) seems not to include the also provided OpenBSP. -- [User:BrainChemistry](</User:BrainChemistry> "User:BrainChemistry"), 14 Feb 2008 

    

  * It is not mentioned on the wiki page but the code is included in the repo. However AFAIK nobody touched that code since it was put into that repo. (more details on [your user talk page](</User_talk:BrainChemistry> "User talk:BrainChemistry")). regards --[Crossbuilder](</User:Crossbuilder> "User:Crossbuilder") 12:41, 16 February 2008 (CET)



* * *

**Inno Unpacker** <http://sourceforge.net/projects/innounp/> \- is the only known tool to extract Inno installers. It is highly desired to get ported version for automatic build of updates [for Wesnoth](<http://www.wesnoth.org/forum/viewtopic.php?p=284681#284681>). --[Skipass](</index.php?title=User:Skipass&action=edit&redlink=1> "User:Skipass \(page does not exist\)") 12:14, 2 March 2008 (CET) 

* * *

**DevExpress** components are very big and very good. It would be nice to be translated or if there is any other substitution for them. I already have project heavily involved this component and substituting is not my first option. Milan. 

* * *

[**Inno Setup**](<http://en.wikipedia.org/wiki/InnoSetup>) is a tool used to create Lazarus installers for Windows. If Inno Setup is ported to FPC, the whole Lazarus package can be cross-compiled on Linux box. User cross-platform applications can benefit from this too. There is also thread in official [jrsoftware.innosetup.code](<http://news.jrsoftware.org/read/article.php?id=19240&group=jrsoftware.innosetup.code#19240>) newsgroup about the same topic. 

## Abandoned

These conversions were attempted but have been abandoned or stalled: 

### osFinancials

The port of this open source project will not be easy, but Rome was not built in one day. The new version does allow interacting with the database through the SQL db components. I have created an example to create a plugin for osFinancials. I had some problems with the new components, but I am sure all this will disappear with time and one day I can fully compile the project in Lazarus. I do have a need for the memdataset to be able to mimic the clientdataset. This will need an XML parser (I am thinking of TJanXmlTree from Jan Verhoeven) and the dataset will need to support blobdata. I will try to see if i can implement this and use the component in Delphi and Lazarus. I will use this component to write the external links to PHP websites (like the osCommerce plugin and the new one I am making for V-Tiger). I use Clientdata set just as a memdataset in the code but I also need the part where the XML datapacket is translated to the dataset and the ability to save to this format. 

[Delphidreamer](</User:Delphidreamer> "User:Delphidreamer")

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** osFinancials code uses old Delphi commercial visual components that are no longer available. Porting osFinancials itself is hard because of this.

Update 2014: meanwhile, all attempts at converting seem to have halted; there seems to be effort to migrate from Delphi-specific components. It looks like this effort has been abandoned, making osFinancials open source but fairly useless for development unless you happen to have the required Delphi version and components.

---

_Source: [https://wiki.freepascal.org/Current_conversion_projects](https://web.archive.org/web/20250131051450/https://wiki.freepascal.org/Current_conversion_projects)_
