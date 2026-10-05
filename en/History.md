# History

│ **English (en)** │  **[français (fr)](</History/fr> "History/fr")** │  **[magyar (hu)](</History/hu> "History/hu")** │  **[Bahasa Indonesia (id)](</History/id> "History/id")** │  **[한국어 (ko)](</History/ko> "History/ko")** │  **[русский (ru)](<../ru/History.md> "History/ru")** │    
****

The Lazarus project has its roots in the [Megido](<Megido.md> "Megido") project. From Google Groups and other mailing lists we can trace some information about Megido. It was a project that attempted to make an open source clone of Delphi, starting with the source code to [Sibyl](<Sibyl.md> "Sibyl") (which in turn was a clone of [Delphi](<Delphi.md> "Delphi") for OS/2, although its designer was rather restricted). Megido began in 1998, but died somewhere in 1999, owing to lack of focus, and lack of interest in a Sybil-based clone with Sybil's restrictions. 

So, three individuals (Cliff Baeseman, Shane Miller, and Michael A. Hess) began Lazarus in February 1999. Each of these three had tried to get involved with the Megido project before it dissolved. They started their own project, Lazarus, out of frustration with their earlier Megido experience. 

The next member of the team was [Marc Weustink](</User:Marc> "User:Marc"). He got involved with the project in August 1999. Marc was looking for a Linux Delphi (at that time Borland had not developed Kylix). He was also interested in Linux in general, and joined the Lazarus project for that reason. At the time Marc joined, Lazarus was not much more than an empty toolbar and some hardcoded GTK menu items. The editor was still being discussed. Marc is still a core contributor, with the debugger interface as his pet subproject. 

A year after Marc joined [Mattias Gaertner](</User:Mattias2> "User:Mattias2") got involved (September 2000). With Mattias as a member of the team the project made a significant move forward. Mattias ported synedit, and coded large parts of the codetools and the designer. With these new additions Lazarus began to look more like it does today. Three years later, Mattias added the package system and has continued to add further IDE features and enhancements. 

[Micha Nelissen](</index.php?title=User:Neli&action=edit&redlink=1> "User:Neli \(page does not exist\)") started contributing in June 2003, mainly sending patches for the win32 interface. He used Borland C++ Builder, but wanted to port code to other platforms as well. Because Borland added their own proprietary extensions to C++ to support their VCL, the chances of BCB applications ever becoming truly portable were slim. A change of language was not really a problem so after some looking around, he thought Lazarus held the most promise. At that time Lazarus was based on GTK for both win32 and Linux. On Linux it worked very well, but on win32 it was buggy. Lazarus users were increasingly asking for a native win32 interface and Micha jumped in to help write that native win32 interface. 

[Vincent Snijders](</User:Vincent> "User:Vincent") was given a link to Lazarus and FPC during the [summer of 1999](<http://lists.lazarus.freepascal.org/pipermail/lazarus/1999-July/002503.html>), when he had just bought his Linux computer and started his thesis, which involved mathematical simulations written in Delphi. He followed the project and attempted to get Lazarus running well on Windows. After graduation in 2003 he had more time for Lazarus and started to contribute patches for Lazarus. His main focus is getting Lazarus on Windows as good as Lazarus on Linux, and the [Lazarus snapshots](<Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). 

[Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat") started looking at Lazarus in 2005 when it was still at version 0.9.6. Felipe hoped to use it as a substitute for Delphi, especially on Linux. He ran into a number of problems when he first tried to use Lazarus, but since he saw huge potential there he decided to keep working with it. He believes his goal of having a fully functional cross-platform alternative to Delphi for his projects has now been achieved. He contributed initially only by reporting bugs and writing documentation, but eventually he started coding as well. Over the years he has started many new interfaces, including Qt 4, WinCE, fpGUI and Cocoa. He added the TTrayIcon component, the PasCocoa bindings, and the Windows CE Interface. He writes articles for the Toolbox Magazine and wrote significant chapters of the book: Lazarus - The Complete Guide. 

[Paul Ishenin](</User:Paul_Ishenin> "User:Paul Ishenin") became aware of Lazarus while working on FastCube 1.0 in Jan 2007. He decided to port FastCube to FPC and Lazarus, and made his first trial conversion. The process was rather easy, but a few things did not work - like the custom drag/drop cursor. At that stage the implementation of Lazarus cursors was very incomplete. Paul started to research how to implement cursors at least for the Windows platform. He used an IRC channel to discuss this with the Lazarus developers, and started to send patches to [Marc Weustink](</User:Marc> "User:Marc"). To Paul's surprise [Marc Weustink](</User:Marc> "User:Marc") did not accept his first patch verbatim because he had not paid full attention to dividing code between the LCL and the widgetsets. After Paul had implemented cursors for GTK, GTK2 and Carbon and made a few more patches, he was granted write access to svn. Later Paul participated in widgetset development for GTK2, Qt and win32, and IDE development. His main focus is to fix the remaining win32 LCL bugs. 

[Zeljan Rikalo](</User:Zeljan> "User:Zeljan") began contributing to the Qt widgetset in 2007 by sending patches to Felipe. During the summer of 2007, he got svn write access. Now he's developing Qt, GTK2 and LCL. 

[Martin Friebe](</User:Martin> "User:Martin") started his contributions to Lazarus by cleaning up and refactoring the SynEdit code. He has since added many new features such as syncro edit. Later he added multi-window views to the Source Editor and greatly improved the debugger. 

[Juha Manninen](</User:JuhaManninen> "User:JuhaManninen") joined in 2009 by first fixing bugs. The biggest contribution before getting SVN write access was the revamped Configure "Build Lazarus" dialog. After getting the SVN access he started maintaining the Delphi converter. After improving the converter he moved to other areas, mostly improving the IDE's usability and fixing more bugs. 

[Bart Broersma](</User:Bart> "User:Bart") was first contributing code for many years and joined the development team in 2011. 

# Contributors

[Roozbeh Gholizadeh](</User:Roozbeh> "User:Roozbeh") Joined in 2006 by trying to make his En2Fa program working on Pocket PC phones (and as a result SmileySMS program for Pocket PCs). En2Fa program was a phonetic writing program which translates phonetic writings in English to Persian. He started compiling and porting Win32 widgets to WinCE and made a working set of WinCE widgets. Also he made some small contributions to FPC compiler to be more compatible with WinCE architecture. 

[todo: others]

---

_Source: [https://wiki.freepascal.org/History](https://web.archive.org/web/20241209231202/https://wiki.freepascal.org/History)_
