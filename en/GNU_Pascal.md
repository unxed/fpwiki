# GNU Pascal

│ **English (en)** │  **[français (fr)](</GNU_Pascal/fr> "GNU Pascal/fr")** │    
****

GNU Pascal (GPC) is the official [Pascal](<Pascal.md> "Pascal") compiler of the [GNU](<GNU.md> "GNU") project. It is compatible to [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") as defined in ISO 7185, and it implements "most" of the ISO 10206 [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") standard. 

Unlike [Free Pascal](<Free_Pascal.md> "Free Pascal"), GPC is not **self-hosted** , i.e. it is not able to translate and **build itself** (as the compiler is written in the C/C++ language). Rather, it acts as a **frontend** to GNU Compiler Collection (GCC), so that Pascal code is translated to C and from C to machine language. 

This has the advantage that it is instantly **portable** to any platform the GCC compiler supports. However, since GNU Pascal is not more than a frontend, it does have to adapt to every major change that is done to GCC. Therefore, new major versions are adopted only slowly. It is also incompatible with newer versions of the GNU C++ Compiler. The official site says it is only compatible with versions 2.8.1, 2.95.x, 3.2.x, 3.3.x or 3.4.x of the GNU C++ compiler. 

GPC was last updated in 2005. The last version of the GNU C++ compiler that it is compatible with, is 3.4.6, dated March, 2006. 

Free Pascal is expected to generate more efficient machine code than GPC. 

## External links

  * [FP and GNU Comparison](<http://www.freepascal.org/faq.var#FPandGNUPascal>)
  * [Official GPC site](<http://www.gnu-pascal.de>) Old and abandoned. The download links there suffer from [link-rot](<https://en.wikipedia.org/wiki/Link_rot>).
  * [A GitHub repository containing the compiler sources](<https://github.com/hebisch/gpc>). It is old (as is the compiler, it is no longer being maintained), as this repository was last updated in 2011.
  * [Another Github repository of GPC](<https://github.com/raylivesun/GNU-R-Delphi>) with additional resources listed on the README.md file

Various [Pascal](<Pascal.md> "Pascal") [Compilers](<Compiler.md> "Compiler"):  [Pascal 8000 (AAEC)](<Pascal_8000_\(AAEC\).md> "Pascal 8000 \(AAEC\)") | [Alice Pascal](<Alice_Pascal.md> "Alice Pascal") | [Apple Pascal](<Apple_Pascal.md> "Apple Pascal") | [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") | [Clascal](<Clascal.md> "Clascal") | [Delphi](<Delphi.md> "Delphi") | [Free Pascal Compiler (FPC)](<FPC.md> "FPC") | GNU Pascal | [Kylix](<Kylix.md> "Kylix") | [Lisa Pascal](<Lisa_Pascal.md> "Lisa Pascal") | [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") | [Metrowerks Pascal](</index.php?title=Metrowerks_Pascal&action=edit&redlink=1> "Metrowerks Pascal \(page does not exist\)") | [NBS Pascal](<NBS_Pascal.md> "NBS Pascal") | [OMSI Pascal](</index.php?title=OMSI_Pascal&action=edit&redlink=1> "OMSI Pascal \(page does not exist\)") | [PascalABC.net](<PascalABC.md> "PascalABC.net") | [P32](</index.php?title=P32&action=edit&redlink=1> "P32 \(page does not exist\)") | [Sibyl](<Sibyl.md> "Sibyl") | [Smart Pascal](</index.php?title=Smart_Pascal&action=edit&redlink=1> "Smart Pascal \(page does not exist\)") | [Stanford Pascal Compiler](<Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler") | [Swedish Pascal](</index.php?title=Swedish_Pascal&action=edit&redlink=1> "Swedish Pascal \(page does not exist\)") | [THINK Pascal](<THINK_Pascal.md> "THINK Pascal") | [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") | [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") | [VAX Pascal](</index.php?title=VAX_Pascal&action=edit&redlink=1> "VAX Pascal \(page does not exist\)") | [Virtual Pascal](<Virtual_Pascal.md> "Virtual Pascal") | [winsoft PocketStudio](<winsoft_PocketStudio.md> "winsoft PocketStudio")  
---  
An extensive list of compilers was maintained at [Pascaland (Internet Archive Version)](<https://web.archive.org/web/20230529010938/https://www.pascaland.org/pascall.htm>) up to January 2018.   
  
  
****

---

_Source: [https://wiki.freepascal.org/GNU_Pascal](https://web.archive.org/web/20250123181409/https://wiki.freepascal.org/GNU_Pascal)_
