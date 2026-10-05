# Lazarus-ccr SourceForge repository

│ **English (en)** │

This page describes the policy for using the Lazarus Code and Component Repository on SourceForge (Lazarus CCR project). Anybody porting components to Lazarus can ask the [Lazarus CCR project admin](</User:Vincent> "User:Vincent") for write access. The Lazarus CCR Project has both SubVersion and Git repositories. If you want to host your project on GitHub instead, please see the [Lazarus-ccr GitHub organization](<Lazarus-ccr_GitHub_organization.md> "Lazarus-ccr GitHub organization") article. 

## Contents

  * 1 Read access
  * 2 Write access
  * 3 SubVersion
    * 3.1 Working with the Lazarus-CCR SVN repository
      * 3.1.1 Checking out
      * 3.1.2 Committing
    * 3.2 Directory owners
  * 4 Git
    * 4.1 Repository owners



### Read access

Everybody has read access to this SubVersion and Git repositories. You can browse the SubVersion repository at <http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/> or the Git repositories at <http://sourceforge.net/p/lazarus-ccr/_list/git>

For more information about the contents of the CCR, please see [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples")

### Write access

Unfortunately, at this moment Sourceforge doesn't support restricting access to SVN to only a subtree of the complete repository. So we will give write access to the complete SVN tree, with the assumption that SVN committers will only write to their own part of the tree. 

Please contact the admins about Git write access too. 

If you want to commit something to a part of the tree for which you are not the maintainer, please contact the maintainer before committing. 

### SubVersion

#### Working with the Lazarus-CCR SVN repository

This is a short guide mainly focusing on the URLs and sourceforge specifics. It is not an introduction to the use of SVN. 

##### Checking out

The Lazarus-CCR is located at <https://svn.code.sf.net/p/lazarus-ccr/svn>

The following command will check out the complete tree into the lazarus-ccr subdirectory of your current directory: 
    
    
    svn co https://svn.code.sf.net/p/lazarus-ccr/svn lazarus-ccr
    

##### Committing

The first time you commit something, svn will ask for your password. Use the password, which belongs to your SourceForge account. This password is stored in the svn metadata in the checked out tree and won't be asked the next time. 

  * If you include a text file named 'readme.txt' with a short summary of your code and maybe a link to it's wiki page, SourceForge will automatically display this when a user browses to your folder.



#### Directory owners

The following lists shows the directory structure and their maintainers. 

  * [applications](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/applications>)
    * [biffexplorer](<Lazarus_Application_Gallery.md> "Lazarus Application Gallery"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [cactusjukebox](</index.php?title=cactusjukebox&action=edit&redlink=1> "cactusjukebox \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [draw_test](</index.php?title=draw_test&action=edit&redlink=1> "draw test \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpbrowser](<fpbrowser.md> "fpbrowser"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [fpchess](<fpChess.md> "fpChess"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [fpsvnsync](<fpsvnsync.md> "fpsvnsync"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpvviewer](<fpvectorial.md> "fpvectorial"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [gobject-introspection](<gir2pascal.md> "gir2pascal"): [Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [idlparser](</index.php?title=idlparser&action=edit&redlink=1> "idlparser \(page does not exist\)"): [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [instantfpc](<InstantFPC.md> "InstantFPC"): [Mattias Gärtner](</User:Mattias2> "User:Mattias2")
    * [khexeditor](</index.php?title=khexeditor&action=edit&redlink=1> "khexeditor \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazclock](</index.php?title=lazclock&action=edit&redlink=1> "lazclock \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [LazEdit](<LazEdit.md> "LazEdit"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Bart](</User:Bart> "User:Bart")
    * [lazeyes](</index.php?title=lazeyes&action=edit&redlink=1> "lazeyes \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazimageeditor](<Lazarus_Image_Editor.md> "Lazarus Image Editor"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [lazspreadsheet](</index.php?title=lazspreadsheet&action=edit&redlink=1> "lazspreadsheet \(page does not exist\)"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [lazstacktrace](</index.php?title=lazstacktrace&action=edit&redlink=1> "lazstacktrace \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [pyramidtiff](<pyramidtiff.md> "pyramidtiff"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [spready](</index.php?title=spready&action=edit&redlink=1> "spready \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [tappytux](<TappyTux.md> "TappyTux"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [wikihelp](</index.php?title=wikihelp&action=edit&redlink=1> "wikihelp \(page does not exist\)"): [Christian Ulrich](</User:Christian> "User:Christian")


  * [bindings](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/bindings>)
    * [android_ndk](<Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [android_sdk](<Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [gtk3](<Gtk+3.md> "Gtk+3"): [Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [objc](</index.php?title=Objective-c_to_Pascal_bindings&action=edit&redlink=1> "Objective-c to Pascal bindings \(page does not exist\)"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat") \-- Note: Obsolete
    * [pascocoa](<PasCocoa.md> "PasCocoa"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz") \-- Note: Obsolete 
      * [parser](<ObjCParser.md> "ObjCParser"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz") \-- Note: Obsolete


  * [components](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/>)
    * [AboutComponent](<Adding_an_About_dialog_as_a_property_to_a_custom_component.md> "Adding an About dialog as a property to a custom component"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [acs](</index.php?title=acs&action=edit&redlink=1> "acs \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [beepfp](<BeepFp.md> "BeepFp"): [Wimpie Nortje](</index.php?title=User:Wimpie&action=edit&redlink=1> "User:Wimpie \(page does not exist\)")
    * [CalLite](<CalLite.md> "CalLite"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [chelper](<Chelper.md> "Chelper"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [ChemText](<ChemText.md> "ChemText"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [cmdline](<CmdLine.md> "CmdLine"): [Julian Schutsch](</User:Alexandrus> "User:Alexandrus")
    * [cmdlinecfg](<CmdLineCfg.md> "CmdLineCfg"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [ColorPalette](<ColorPalette.md> "ColorPalette"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [csvdocument](<CsvDocument.md> "CsvDocument"): [Vladimir Zhirov](</index.php?title=User:Vvzh&action=edit&redlink=1> "User:Vvzh \(page does not exist\)")
    * [epiktimer](</index.php?title=epiktimer&action=edit&redlink=1> "epiktimer \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [extrasyn](</index.php?title=extrasyn&action=edit&redlink=1> "extrasyn \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [fpexif](</index.php?title=fpexif&action=edit&redlink=1> "fpexif \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [fpsound](</index.php?title=fpsound&action=edit&redlink=1> "fpsound \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpspreadsheet](<FPSpreadsheet.md> "FPSpreadsheet"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Jose Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)"), [Reinier Olislagers](</User:BigChimp> "User:BigChimp"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [Fractions](<Fractions.md> "Fractions"): [Bart](</User:Bart> "User:Bart")
    * [freetypepascal](<Freetype.md> "Freetype"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [GeckoPort](<GeckoPort.md> "GeckoPort"): [Jose Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)"), [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [gradcontrols](<GradControls.md> "GradControls"): [Eugen Bolz](</User:EugenE> "User:EugenE")
    * [Industrial stuff](</index.php?title=Industrial_stuff&action=edit&redlink=1> "Industrial stuff \(page does not exist\)"): [Juha Manninen](</User:JuhaManninen> "User:JuhaManninen")
    * [iOS Designer](<iOS_Designer.md> "iOS Designer"): [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [iPhone Laz Extension](<iPhone_Laz_Extension.md> "iPhone Laz Extension"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [jujiboutils](<jujiboutils.md> "jujiboutils"): [Julio Jiménez Borreguero](</User:Jujibo> "User:Jujibo")
    * [jvcllaz](</index.php?title=jvcllaz&action=edit&redlink=1> "jvcllaz \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [KControls](<KControls.md> "KControls"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazbarcodes](</index.php?title=lazbarcodes&action=edit&redlink=1> "lazbarcodes \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lclextensions](</index.php?title=lclextensions&action=edit&redlink=1> "lclextensions \(page does not exist\)"): [Luiz Américo Pereira Câmara ](</User:Luizmed> "User:Luizmed")
    * [TLongTimer](<longtimer.md> "longtimer"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [manualdocker](<Manual_Docker.md> "Manual Docker"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [mbColorLib](<mbColorLib.md> "mbColorLib"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [mplayer](</index.php?title=mplayer&action=edit&redlink=1> "mplayer \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [multithreadprocs](<Parallel_procedures.md> "Parallel procedures"): [Mattias Gärtner](</User:Mattias2> "User:Mattias2")
    * [nvidia-widgets](<nvidia-widgets.md> "nvidia-widgets"): Darius Blaszyk
    * [onguard](</index.php?title=onguard&action=edit&redlink=1> "onguard \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [orpheus](<http://web.fastermac.net/~MacPgmr/OrphPort/OrphStatus.html>): [User:Phil](</index.php?title=User:Phil&action=edit&redlink=1> "User:Phil \(page does not exist\)")
    * [TPlaySound](<playsound.md> "playsound"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [TPoweredby](<Poweredby.md> "Poweredby"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [powerpdf](<PowerPDF.md> "PowerPDF"): [Jesús Reyes](</User:Jesusrmx> "User:Jesusrmx"), [Christian Ulrich](</User:Christian> "User:Christian")
    * [rgbgraphics](<LazRGBGraphics.md> "LazRGBGraphics"): [Tom Gregorovic](</User:Tombo> "User:Tombo")
    * [richmemo](<RichMemo.md> "RichMemo"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [richview](</index.php?title=richview&action=edit&redlink=1> "richview \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [rtfview](</index.php?title=RTFView&action=edit&redlink=1> "RTFView \(page does not exist\)"): [Jesús Reyes](</User:Jesusrmx> "User:Jesusrmx")
    * [rx](<RXfpc.md> "RXfpc"): Aleksy Lagunov, Andrew Ivanov
    * [TScrollText](<ScrollText.md> "ScrollText"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [smnetgradient](</index.php?title=smnetgradient&action=edit&redlink=1> "smnetgradient \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [SpkToolbar](<SpkToolbar_Package.md> "SpkToolbar Package"): [Juha Manninen](</User:JuhaManninen> "User:JuhaManninen"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [svn](<SvnClasses.md> "SvnClasses"): [Vincent Snijders](</User:Vincent> "User:Vincent")
    * [TDINotebook](<TTDINotebook.md> "TTDINotebook"): [Daniel Simões de Almeida](</User:Dopidaniel> "User:Dopidaniel")
    * [THtmlPort](<THtmlPort.md> "THtmlPort"): [User:Phil](</index.php?title=User:Phil&action=edit&redlink=1> "User:Phil \(page does not exist\)")
    * [tparadoxdataset](<TParadoxDataSet.md> "TParadoxDataSet"): [Christian Ulrich](</User:Christian> "User:Christian")
    * [tvplanit](<Turbopower_Visual_PlanIt.md> "Turbopower Visual PlanIt"): [Christian Ulrich](</User:Christian> "User:Christian"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [virtualtreeview](<VirtualTreeview.md> "VirtualTreeview"): [Christian Ulrich](</User:Christian> "User:Christian")
    * [virtualtreeview-new](<http://lazarusroad.blogspot.com/2007/02/children-of-port.html>): [Luiz Américo Pereira Câmara](</User:Luizmed> "User:Luizmed")
    * [xdev_toolkit](</index.php?title=xdev_toolkit&action=edit&redlink=1> "xdev toolkit \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [Zlibar](<Zlibar.md> "Zlibar"): [ Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [zmsql](</index.php?title=zmsql&action=edit&redlink=1> "zmsql \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")


  * [examples](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/examples/>)
    * [androidlcl](<Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [germesorders](<germesorders.md> "germesorders") : [MageSlayer](<https://sourceforge.net/users/mageslayer/>)
    * [noise](<Perlin_Noise.md> "Perlin Noise"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [process](<Executing_External_Programs.md> "Executing External Programs"): [Vincent Snijders](</User:Vincent> "User:Vincent")


  * [lclbindings](<LCL_Bindings.md> "LCL Bindings"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * [wst](<Web_Service_Toolkit.md> "Web Service Toolkit"): [Inoussa Ouedraogo](</index.php?title=User:Inoussa&action=edit&redlink=1> "User:Inoussa \(page does not exist\)")



### Git

It the Git area of Lazarus CCR you can have one repository for every project. Thus making read/write access much easier to manage. 

#### Repository owners

  * [DCPCrypt](<DCPcrypt.md> "DCPcrypt"): [Graeme Geldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys")
  * [lazarus-ccr](</index.php?title=lazarus-ccr&action=edit&redlink=1> "lazarus-ccr \(page does not exist\)"): Unknown

---

_Source: [https://wiki.freepascal.org/Using_the_Lazarus-ccr_SVN_repository](https://web.archive.org/web/20250429211628/https://wiki.freepascal.org/Using_the_Lazarus-ccr_SVN_repository)_
