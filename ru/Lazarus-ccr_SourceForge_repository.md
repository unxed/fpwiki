# Lazarus-ccr SourceForge repository

│ **[English (en)](<../en/Lazarus-ccr_SourceForge_repository.md>)** │  **русский (ru)** │

Эта страница описывает политику использования Lazarus Code and Component Repository на SourceForge (проект Lazarus CCR). Все, кто портируют компоненты на Lazarus, могут попросить [админа проекта Lazarus CCR](</User:Vincent> "User:Vincent") права доступа на запись. Проект Lazarus CCR имеет SubVersion и Git репозитории. Если же Вы хотите разместить свой проект на GitHub, смотрите, пожалуйста, статью [Lazarus-CCR GitHub organization](</index.php?title=Lazarus-CCR_GitHub_organization&action=edit&redlink=1> "Lazarus-CCR GitHub organization \(page does not exist\)"). 

## Contents

  * 1 Доступ на чтение
  * 2 Доступ на запись
  * 3 SubVersion
    * 3.1 Работа с Lazarus-CCR SVN-репозиторием
      * 3.1.1 Проверка
      * 3.1.2 Внесение изменений
    * 3.2 Владельцы каталогов
  * 4 Git
    * 4.1 Владельцы репозитория



### Доступ на чтение

Каждый имеет права доступа на чтение к этим SubVersion и Git репозиториям. Вы можете ознакомиться с SubVersion-репозиторием на <http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/> или Git-репозиторием на <http://sourceforge.net/p/lazarus-ccr/_list/git>

Об остальной информации по составу CCR, см. [Components and Code examples](<../en/Components_and_Code_examples.md> "Components and Code examples")

### Доступ на запись

К несчастью, в настоящий момент Sourceforge не поддерживает ограничение доступа к SVN лишь на поддерево всего репозитория. Поэтому мы даём доступ ко всему SVN-дереву, предполагая что SVN-коммиттеры будут писать только в свою собственную часть дерева. 

Пожалуйста, обращайтесь также к администраторам по поводу прав на запись в Git. 

Если Вы хотите коммитить что-нибудь в часть дерева которой не владеете, перед записью обратитесь, пожалуйста, к владельцу. 

### SubVersion

#### Работа с Lazarus-CCR SVN-репозиторием

Это краткое руководство, касающееся в основном URL и специфики sourceforge. Это не введение по работе с SVN. 

##### Проверка

Lazarus-CCR находится на <https://svn.code.sf.net/p/lazarus-ccr/svn>

Следующая команда проверит всё дерево в lazarus-ccr подкаталоге вашего текущего каталога: 
    
    
    svn co https://svn.code.sf.net/p/lazarus-ccr/svn lazarus-ccr
    

##### Внесение изменений

В первый раз, когда вы что-то записываете, SVN спросит ваш пароль. Используйте пароль, принадлежащий вашему SourceForge-аккаунту. Этот пароль сохранится в метаданных SVN-дерева и в следующий раз спрашиваться не будет. 

  * Если вы включите текстовый файл с именем «readme.txt» с кратким описанием вашего кода и, возможно, ссылкой на его вики-страницу, SourceForge автоматически отобразит его, когда пользователь перейдет в вашу папку.



#### Владельцы каталогов

В следующих списках показаны структура каталогов и лица, отвечающие за них. 

  * [applications](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/applications>)
    * [biffexplorer](<../en/Lazarus_Application_Gallery.md> "Lazarus Application Gallery"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [cactusjukebox](</index.php?title=cactusjukebox&action=edit&redlink=1> "cactusjukebox \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [draw_test](</index.php?title=draw_test&action=edit&redlink=1> "draw test \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpbrowser](<../en/fpbrowser.md> "fpbrowser"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [fpchess](<../en/fpChess.md> "fpChess"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [fpsvnsync](<../en/fpsvnsync.md> "fpsvnsync"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpvviewer](<../en/fpvectorial.md> "fpvectorial"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [gobject-introspection](<../en/gir2pascal.md> "gir2pascal"): [Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [idlparser](</index.php?title=idlparser&action=edit&redlink=1> "idlparser \(page does not exist\)"): [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [instantfpc](<../en/InstantFPC.md> "InstantFPC"): [Mattias Gärtner](</User:Mattias2> "User:Mattias2")
    * [khexeditor](</index.php?title=khexeditor&action=edit&redlink=1> "khexeditor \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazclock](</index.php?title=lazclock&action=edit&redlink=1> "lazclock \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [LazEdit](<../en/LazEdit.md> "LazEdit"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Bart](</User:Bart> "User:Bart")
    * [lazeyes](</index.php?title=lazeyes&action=edit&redlink=1> "lazeyes \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazimageeditor](<../en/Lazarus_Image_Editor.md> "Lazarus Image Editor"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [lazspreadsheet](</index.php?title=lazspreadsheet&action=edit&redlink=1> "lazspreadsheet \(page does not exist\)"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [lazstacktrace](</index.php?title=lazstacktrace&action=edit&redlink=1> "lazstacktrace \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [pyramidtiff](<../en/pyramidtiff.md> "pyramidtiff"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [spready](</index.php?title=spready&action=edit&redlink=1> "spready \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [tappytux](<../en/TappyTux.md> "TappyTux"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [wikihelp](</index.php?title=wikihelp&action=edit&redlink=1> "wikihelp \(page does not exist\)"): [Christian Ulrich](</User:Christian> "User:Christian")


  * [bindings](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/bindings>)
    * [android_ndk](<../en/Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [android_sdk](<../en/Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [gtk3](<../en/Gtk+3.md> "Gtk+3"): [Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [objc](</index.php?title=Objective-c_to_Pascal_bindings&action=edit&redlink=1> "Objective-c to Pascal bindings \(page does not exist\)"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat") \-- Note: Obsolete
    * [pascocoa](<../en/PasCocoa.md> "PasCocoa"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz") \-- Note: Obsolete 
      * [parser](<../en/ObjCParser.md> "ObjCParser"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz") \-- Note: Obsolete


  * [components](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/>)
    * [AboutComponent](<../en/Adding_an_About_dialog_as_a_property_to_a_custom_component.md> "Adding an About dialog as a property to a custom component"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [acs](</index.php?title=acs&action=edit&redlink=1> "acs \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [beepfp](<../en/BeepFp.md> "BeepFp"): [Wimpie Nortje](</index.php?title=User:Wimpie&action=edit&redlink=1> "User:Wimpie \(page does not exist\)")
    * [CalLite](<../en/CalLite.md> "CalLite"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [chelper](<../en/Chelper.md> "Chelper"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [ChemText](<../en/ChemText.md> "ChemText"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [cmdline](<../en/CmdLine.md> "CmdLine"): [Julian Schutsch](</User:Alexandrus> "User:Alexandrus")
    * [cmdlinecfg](<../en/CmdLineCfg.md> "CmdLineCfg"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [ColorPalette](<../en/ColorPalette.md> "ColorPalette"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [csvdocument](<../en/CsvDocument.md> "CsvDocument"): [Vladimir Zhirov](</index.php?title=User:Vvzh&action=edit&redlink=1> "User:Vvzh \(page does not exist\)")
    * [epiktimer](</index.php?title=epiktimer&action=edit&redlink=1> "epiktimer \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [extrasyn](</index.php?title=extrasyn&action=edit&redlink=1> "extrasyn \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [fpexif](</index.php?title=fpexif&action=edit&redlink=1> "fpexif \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [fpsound](</index.php?title=fpsound&action=edit&redlink=1> "fpsound \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [fpspreadsheet](<../en/FPSpreadsheet.md> "FPSpreadsheet"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat"), [Jose Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)"), [Reinier Olislagers](</User:BigChimp> "User:BigChimp"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [Fractions](<../en/Fractions.md> "Fractions"): [Bart](</User:Bart> "User:Bart")
    * [freetypepascal](<../en/Freetype.md> "Freetype"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [GeckoPort](<../en/GeckoPort.md> "GeckoPort"): [Jose Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)"), [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [gradcontrols](<../en/GradControls.md> "GradControls"): [Eugen Bolz](</User:EugenE> "User:EugenE")
    * [Industrial stuff](</index.php?title=Industrial_stuff&action=edit&redlink=1> "Industrial stuff \(page does not exist\)"): [Juha Manninen](</User:JuhaManninen> "User:JuhaManninen")
    * [iOS Designer](<../en/iOS_Designer.md> "iOS Designer"): [Joost van der Sluis](</User:Loesje> "User:Loesje")
    * [iPhone Laz Extension](<../en/iPhone_Laz_Extension.md> "iPhone Laz Extension"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [jujiboutils](<../en/jujiboutils.md> "jujiboutils"): [Julio Jiménez Borreguero](</User:Jujibo> "User:Jujibo")
    * [jvcllaz](</index.php?title=jvcllaz&action=edit&redlink=1> "jvcllaz \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [KControls](<../en/KControls.md> "KControls"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lazbarcodes](</index.php?title=lazbarcodes&action=edit&redlink=1> "lazbarcodes \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [lclextensions](</index.php?title=lclextensions&action=edit&redlink=1> "lclextensions \(page does not exist\)"): [Luiz Américo Pereira Câmara ](</User:Luizmed> "User:Luizmed")
    * [TLongTimer](<../en/longtimer.md> "longtimer"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [manualdocker](<../en/Manual_Docker.md> "Manual Docker"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [mbColorLib](<../en/mbColorLib.md> "mbColorLib"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [mplayer](</index.php?title=mplayer&action=edit&redlink=1> "mplayer \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [multithreadprocs](<../en/Parallel_procedures.md> "Parallel procedures"): [Mattias Gärtner](</User:Mattias2> "User:Mattias2")
    * [nvidia-widgets](<../en/nvidia-widgets.md> "nvidia-widgets"): Darius Blaszyk
    * [onguard](</index.php?title=onguard&action=edit&redlink=1> "onguard \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [orpheus](<http://web.fastermac.net/~MacPgmr/OrphPort/OrphStatus.html>): [User:Phil](</index.php?title=User:Phil&action=edit&redlink=1> "User:Phil \(page does not exist\)")
    * [TPlaySound](<../en/playsound.md> "playsound"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [TPoweredby](<../en/Poweredby.md> "Poweredby"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [powerpdf](<../en/PowerPDF.md> "PowerPDF"): [Jesús Reyes](</User:Jesusrmx> "User:Jesusrmx"), [Christian Ulrich](</User:Christian> "User:Christian")
    * [rgbgraphics](<../en/LazRGBGraphics.md> "LazRGBGraphics"): [Tom Gregorovic](</User:Tombo> "User:Tombo")
    * [richmemo](<../en/RichMemo.md> "RichMemo"): [Dmitry Boyarintsev](</User:Skalogryz> "User:Skalogryz")
    * [richview](</index.php?title=richview&action=edit&redlink=1> "richview \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [rtfview](</index.php?title=RTFView&action=edit&redlink=1> "RTFView \(page does not exist\)"): [Jesús Reyes](</User:Jesusrmx> "User:Jesusrmx")
    * [rx](<../en/RXfpc.md> "RXfpc"): Aleksy Lagunov, Andrew Ivanov
    * [TScrollText](<../en/ScrollText.md> "ScrollText"): [Gordon Bamber](</User:Minesadorada> "User:Minesadorada")
    * [smnetgradient](</index.php?title=smnetgradient&action=edit&redlink=1> "smnetgradient \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [SpkToolbar](</index.php?title=SpkToolbar_Package&action=edit&redlink=1> "SpkToolbar Package \(page does not exist\)"): [Juha Manninen](</User:JuhaManninen> "User:JuhaManninen"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [svn](<../en/SvnClasses.md> "SvnClasses"): [Vincent Snijders](</User:Vincent> "User:Vincent")
    * [TDINotebook](<../en/TTDINotebook.md> "TTDINotebook"): [Daniel Simões de Almeida](</User:Dopidaniel> "User:Dopidaniel")
    * [THtmlPort](<../en/THtmlPort.md> "THtmlPort"): [User:Phil](</index.php?title=User:Phil&action=edit&redlink=1> "User:Phil \(page does not exist\)")
    * [tparadoxdataset](<../en/TParadoxDataSet.md> "TParadoxDataSet"): [Christian Ulrich](</User:Christian> "User:Christian")
    * [tvplanit](<../en/Turbopower_Visual_PlanIt.md> "Turbopower Visual PlanIt"): [Christian Ulrich](</User:Christian> "User:Christian"), [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")
    * [virtualtreeview](<../en/VirtualTreeview.md> "VirtualTreeview"): [Christian Ulrich](</User:Christian> "User:Christian")
    * [virtualtreeview-new](<http://lazarusroad.blogspot.com/2007/02/children-of-port.html>): [Luiz Américo Pereira Câmara](</User:Luizmed> "User:Luizmed")
    * [xdev_toolkit](</index.php?title=xdev_toolkit&action=edit&redlink=1> "xdev toolkit \(page does not exist\)"): [User:???](</index.php?title=User:%3F%3F%3F&action=edit&redlink=1> "User:??? \(page does not exist\)")
    * [Zlibar](<../en/Zlibar.md> "Zlibar"): [ Andrew Haines](</User:AndrewH> "User:AndrewH")
    * [zmsql](</index.php?title=zmsql&action=edit&redlink=1> "zmsql \(page does not exist\)"): [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")


  * [examples](<http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/examples/>)
    * [androidlcl](<../en/Android_Interface.md> "Android Interface"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [germesorders](<../en/germesorders.md> "germesorders") : [MageSlayer](<https://sourceforge.net/users/mageslayer/>)
    * [noise](<../en/Perlin_Noise.md> "Perlin Noise"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
    * [process](<../en/Executing_External_Programs.md> "Executing External Programs"): [Vincent Snijders](</User:Vincent> "User:Vincent")


  * [lclbindings](<../en/LCL_Bindings.md> "LCL Bindings"): [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * [wst](<../en/Web_Service_Toolkit.md> "Web Service Toolkit"): [Inoussa Ouedraogo](</index.php?title=User:Inoussa&action=edit&redlink=1> "User:Inoussa \(page does not exist\)")



### Git

В Git-области для Lazarus CCR вы можете иметь один репозиторий на каждый проект. В этом случае управление доступом для чтения/записи становится намного проще. 

#### Владельцы репозитория

  * [DCPCrypt](<../en/DCPcrypt.md> "DCPcrypt"): [Graeme Geldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys")
  * [lazarus-ccr](</index.php?title=lazarus-ccr&action=edit&redlink=1> "lazarus-ccr \(page does not exist\)"): Unknown

---

_Source: [https://wiki.freepascal.org/Lazarus-ccr_SourceForge_repository/ru](https://web.archive.org/web/20231001084619/https://wiki.freepascal.org/Lazarus-ccr_SourceForge_repository/ru)_
