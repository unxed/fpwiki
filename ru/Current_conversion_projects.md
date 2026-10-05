# Current conversion projects

│ **[English (en)](<../en/Current_conversion_projects.md>)** │  **русский (ru)** │

  
Эта страница содержит список приложений и компонентов, которые сейчас находятся в стадии переноса. Если перенос завершён (или сначала вы хотите получить сведения от пользователей), компоненты могут быть перемещены в [Components and Code examples](<../en/Components_and_Code_examples.md> "Components and Code examples") и приложения в [Projects using Lazarus](<../en/Projects_using_Lazarus.md> "Projects using Lazarus"). Если создать описание страницы, для приложения или компонента можно создать ссылку для скачивания [sourceforge files area](<http://sourceforge.net/project/showfiles.php?group_id=92177>). 

## Contents

  * 1 Приложения
  * 2 Компоненты
    * 2.1 Компоненты большого экрана
    * 2.2 Indy
    * 2.3 FormStorage
    * 2.4 PowerPDF для Lazarus
    * 2.5 Контрол GUI tiOPF
  * 3 Библиотеки
    * 3.1 dxGetText
    * 3.2 GraphicEx
    * 3.3 Graphics32
  * 4 Требуемые компоненты
    * 4.1 devphp
    * 4.2 Usercontrol
    * 4.3 AutoREALM
    * 4.4 Toolbar 2000
    * 4.5 Report Manager
    * 4.6 Open XML
    * 4.7 Other applications, libraries and components
  * 5 Abandoned
    * 5.1 osFinancials



## Приложения

Нет текущих. 

## Компоненты

### Компоненты большого экрана

Уже завершено: 

  * TLCD99
  * TLCDLabel
  * TAnalogueclock



Все скомпилировано с помощью SGraph 2.4 с лицензией на ограниченное распространение модифицированного исходного кода. Автор просит соединиться с ним для получения разрешения на внесение изменений. 

Также осуществляется перенос маленького потокового рекордера с первоначальным вариантом Марка Додсона с его переписыванием. Если кто-то заинтересован в этих компонентах свяжитесь с [VlxAdmin](</User:VlxAdmin> "User:VlxAdmin")

### Indy

[Internet Direct (Indy)](<http://www.indyproject.org/>) \-- проект с открытым исходным кодом, компоненты сокетов TCP/IP обеспечивающие доступ к популярному интернет протоколу. Для более подробной информации смотрите [indy4lazarus](<http://indy4lazarus.sourceforge.net/>). 

Новые попытки предпринимает Марко ван де Вурт ( Marco van de Voort). Больше информации (статус) можно посмотреть здесь [Indy with Lazarus](<../en/Indy_with_Lazarus.md> "Indy with Lazarus")

Текущий снимок (только для хардкорщиков) смотреть [Indy9](<http://www.stack.nl/~marcov/indy9.zip>) и [Indy10](<http://www.stack.nl/~marcov/Indy10FPC.zip>)

### FormStorage

[FormStorage](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986>) этот компонент для сохранения всех выбранных свойств в xml файле. 

### PowerPDF для Lazarus

[Original PowerPDF site](<http://www.est.hi-ho.ne.jp/takeshi_kanno/powerpdf/index.html>) PowerPdf является набором LCL компонентов для визуального создания PDF документов. Используя этот компонент можно легко создавать PDF документы в Lazarus IDE. Базовая версия PowerPDF 0.9, статус 95% готовности. -[jesusrmx](</User:Jesusrmx> "User:Jesusrmx")

[Chtk](</User:Chtk> "User:Chtk") также начат порт PowerPdf для Lazarus. Результаты этих усилий были объединены с портом, сделанным jesusrmx. Пакет, который должен иметь все функциональные возможности версии Delphi, работает только [здесь](<http://iquad.nl/files/powerpdf/>). 

[Xno](</User:Xno> "User:Xno") имеется порт пакета для Lazarus. Этот код доступен [здесь](<http://xoomer.virgilio.it/xno/xnocbt.html>). 

### Контрол GUI tiOPF

[Bogusław Brandys](</index.php?title=User:Forest&action=edit&redlink=1> "User:Forest \(page does not exist\)") начал портировать tiOPF Persistent Aware ([TechInsite tiOPF site](<http://tiopf.sourceforge.net/>)) GUI элементы (контролы) для Lazarus. Текущее состояние простая компиляция и установка в IDE. Любая помощь приветствуется, особенно глубокие знания о создании компонентов для Lazarus. 

Что сделать (TODO): 

  * удалить все сообщения заголовков компонентов, заменить все привязки и размеры (контролы выглядят некрасиво)
  * устранить проблемы с AV при удалении субэлементов (контролы tiOPF GUI являются составными)
  * устранить проблемы с tiLVTreeView/tiLVListView



## Библиотеки

### dxGetText

[ Lazarus dxGetText](<../en/DxGetText.md> "DxGetText") это порт [Olivier Guilbaud](<http://sourceforge.net/users/golivier/>) из [проекта dxGetText](<http://dybdahl.dk/dxgettext/>). Сайт проекта dxGetText: "Первоначально, этот проект используется порт Windoes GetText библиотеке GNU, но проект ушёл гораздо дальше и сегодня это полная переработанная GNU Gettext библиотека с большим количеством улучшений". 

### GraphicEx

Фантастический пакет GraphicEx с <http://www.delphi-gems.com/> была адаптирована и улучшена theo. Смотрите [http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&p=17635>). 

### Graphics32

Graphics32 графическая библиотека для Delphi и Kylix/CLX. Оптимизирована для 32-bit пиксельного формата,и обеспечивает быстрые операции над пикселями и графическими примитивами. В большинстве случаев Graphics32 значительно превосходит стандартные методы TBitmap/TCanvas. 

Команда начала порт этой библиотеки к Free Pascal и Lazarus.LCL-Win32 порт практически завершена.LCL-carbon порт завершён примерно на 50%. 

Документацию по проекту можно найти здесь: [[1]](<http://graphics32.org/documentation/Docs/_Body.htm>)

## Требуемые компоненты

### devphp

[devphp](<http://sourceforge.net/projects/devphp/>) это IDE для PHP написана на Delphi/Kylix. Она получил много полезных возможностей, и было бы очень удобно, если бы она компилировалась под Lazarus. Автору не хватает времени, чтобы работать над ней, так что, вероятно, IDE является хорошим кандидатом для переноса. [Tom](</User:VlxAdmin> "User:VlxAdmin")

### Usercontrol

[Usercontrol](<http://sourceforge.net/projects/usercontrol>) Delphi (и Kylix) пакетные компоненты для использования и управления профилями, и контроля доступа. Поддерживается ADO, DBX, IBX, BDE, IBO, FIBPlus, ZeosDBO, DBISAM, MDO, MyDAC, MySQLDAC и ASTA3. Контроль доступа авто-извлечения TMenu, TActionList и элементы TActionManager. И MODULE для компонента UIB. 

### AutoREALM

AutoREALM ( <http://autorealm.sourceforge.net> ) это с открытыми исходниками (GNU) программное обеспечение для ролевых фантази карт. ПО "разработано с использованием Delphi Personal Edition™ (от Borland Inc.) и основан на простом и классическом языке TurboPascal™, AutoREALM также использует Kylix Open Edition™ для запуска на LINUX платформе.". Ну, это на самом деле не компилируется на Linux, но сделать порт на Lazarus, который также способен компилировать для Linux, Mac et.c. было бы очень приятно. Currently there is a project to port AutoREALM to C++ and then to Linux, but a Lazarus port would perhaps be easier. 

### Toolbar 2000

Toolbar 2000 ( <http://www.jrsoftware.org/tb2k.php> ) is "a set of components for Borland Delphi and C++Builder (4.0 and later) designed to mimic the look and behavior of Office 2000's menus and toolbars.". Available under either a commercial license or the GNU General Public License. 

### Report Manager

[Report Manager](<http://reportman.sourceforge.net>) Component for creating reports from database with visual editor,band support,conditional printing,evaluating saving to XLS,PDF,HTML. 

### Open XML

[Open XML](<http://www.philo.de/xml/>) is "a collection of XML and Unicode tools and components for the Delphi/Kylix™ programming language. All packages are freely available including source code." 

### Other applications, libraries and components

**Add an application, library or component that you need here**

~~Support for Paradox and Access databasing (ADO, DAO or ODBC) in at least a Win32 environment. A package named KADao implements this and is free in Delphi, maybe someone can translate this. If databasing is already implemented, maybe a way for new users to find it???[User:Micdutoit](</index.php?title=User:Micdutoit&action=edit&redlink=1> "User:Micdutoit \(page does not exist\)")~~

    Paradox dataset is supplied with Lazarus. MS Access can be accessed via [ODBCConn](<../en/ODBCConn.md> "ODBCConn") supplied with Lazarus. --[BigChimp](</User:BigChimp> "User:BigChimp") 16:43, 23 October 2014 (CEST)

* * *

I wish **JCL and JVCL** ported to lazarus. Also I need some sort of components like [Developer Express (c)](<http://www.devexpress.com/>) to completely leave Delphi and get Lazarus. I need the cxLayoutControl and all related components. Do you know any packages that are like they? 

* * *

The **MUTIS** full text search engine project is looking for help to provide a multi-plataform layer to cross-compiling to .NET (current) Win32 and Linux. I think lazarus is a better target than Kilyk. I have a start but need help to get rid fo .NET specific things. 

The project is at <http://sourceforge.net/projects/mutis> and the mailing list <http://groups.google.com.co/group/mutis-developers?lnk=li>

MUTIS is a search and indexing engine based in Lucene. Is done at 80% at API 1.4 level. I think is great have this tech on Delphi and make the project the first all native, all multiplataform, one language, in their class. 

* * *

I wish **OpenBSP** (part of GLScene) to be ported. The project of porting GLScene to Lazarus ([GLScene](<../en/GLScene.md> "GLScene")) seems not to include the also provided OpenBSP. -- [User:BrainChemistry](</User:BrainChemistry> "User:BrainChemistry"), 14 Feb 2008 

    

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

**Предупреждение:** osFinancials code uses old Delphi commercial visual components that are no longer available. Porting osFinancials itself is hard because of this.

Update 2014: meanwhile, all attempts at converting seem to have halted; there seems to be effort to migrate from Delphi-specific components. It looks like this effort has been abandoned, making osFinancials open source but fairly useless for development unless you happen to have the required Delphi version and components. Insert non-formatted text here

---

_Source: [https://wiki.freepascal.org/Current_conversion_projects/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Current_conversion_projects/ru)_
