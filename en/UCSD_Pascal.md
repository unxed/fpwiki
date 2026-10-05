# UCSD Pascal

│ **English (en)** │

**UCSD Pascal** was the first minicomputer and microcomputer implementation of the [Pascal](<Pascal.md> "Pascal") programming language. Developed at the University of California, San Diego, under the direction of Kenneth Bowes, it implemented a number of significant improvements to the [standard Pascal](<Standard_Pascal.md> "Standard Pascal") language, including 

  * Separate compilation of programs by use of the [unit](<Unit.md> "Unit") directive.
  * Implementation of a means to distinguish between disk files and screen files, so that interactive applications could be developed.
  * On-screen compilation including an interactive editor such that indications of errors in the program detected by the [compiler](<Compiler.md> "Compiler") could be transferred back to the editor to allow the exact line that the error occurred on to be highlighted, along with the precise error message.
  * Variable length strings, including [procedures](<Procedure.md> "Procedure") to handle them.



UCSD Pascal was implemented on 

  * Terak computer system, which used the PDP-11 processor and a bit-mapped screen similar to the original Apple Macintosh computer.
  * The Apple II with the 80-character video card.
  * The IBM-PC.
  * The Texas Instruments TI 99/4A, which used the TMS 9900 processor.



UCSD Pascal compiled to [P-Code](<P-Code.md> "P-Code") that was executed by a virtual machine (similar to Java byte-code and the JVM). The compiler and virtual machine ran on the UCSD p-System [operating system](<Operating_System.md> "Operating System") which had its own file format for its disk directories, different from any other microcomputer operating system file format at the time, as it handled filenames longer than the then-standard 6 + 3, and later 8 + 3 formats, and also could handle filenames containing one or more blanks. 

Most of the operating system was itself written in UCSD Pascal, apart from the machine-dependent parts. To make this possible, UCSD Pascal was also expanded with some features, mainly aimed at system programmers 

  * Handling of untyped data.
  * Access to untyped files, as well as the ability to read and write blocks directly on a disk.
  * Concurrent processes.



## Source code

Version I.5 of UCSD Pascal is nowadays available under a non-commercial open source license. The [source code](<Source_code.md> "Source code") of this version can be found on the [Free Pascal](<FPC.md> "FPC") ftp site at <ftp://ftp.freepascal.org/pub/fpc/attic/ucsd-pascal>

Various [Pascal](<Pascal.md> "Pascal") [Compilers](<Compiler.md> "Compiler"):  [Pascal 8000 (AAEC)](<Pascal_8000_\(AAEC\).md> "Pascal 8000 \(AAEC\)") | [Alice Pascal](<Alice_Pascal.md> "Alice Pascal") | [Apple Pascal](<Apple_Pascal.md> "Apple Pascal") | [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") | [Clascal](<Clascal.md> "Clascal") | [Delphi](<Delphi.md> "Delphi") | [Free Pascal Compiler (FPC)](<FPC.md> "FPC") | [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") | [Kylix](<Kylix.md> "Kylix") | [Lisa Pascal](<Lisa_Pascal.md> "Lisa Pascal") | [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") | [Metrowerks Pascal](</index.php?title=Metrowerks_Pascal&action=edit&redlink=1> "Metrowerks Pascal \(page does not exist\)") | [NBS Pascal](<NBS_Pascal.md> "NBS Pascal") | [OMSI Pascal](</index.php?title=OMSI_Pascal&action=edit&redlink=1> "OMSI Pascal \(page does not exist\)") | [PascalABC.net](<PascalABC.md> "PascalABC.net") | [P32](</index.php?title=P32&action=edit&redlink=1> "P32 \(page does not exist\)") | [Sibyl](<Sibyl.md> "Sibyl") | [Smart Pascal](</index.php?title=Smart_Pascal&action=edit&redlink=1> "Smart Pascal \(page does not exist\)") | [Stanford Pascal Compiler](<Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler") | [Swedish Pascal](</index.php?title=Swedish_Pascal&action=edit&redlink=1> "Swedish Pascal \(page does not exist\)") | [THINK Pascal](<THINK_Pascal.md> "THINK Pascal") | [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") | UCSD Pascal | [VAX Pascal](</index.php?title=VAX_Pascal&action=edit&redlink=1> "VAX Pascal \(page does not exist\)") | [Virtual Pascal](<Virtual_Pascal.md> "Virtual Pascal") | [winsoft PocketStudio](<winsoft_PocketStudio.md> "winsoft PocketStudio")  
---  
An extensive list of compilers was maintained at [Pascaland (Internet Archive Version)](<https://web.archive.org/web/20230529010938/https://www.pascaland.org/pascall.htm>) up to January 2018.   
  
  
****

---

_Source: [https://wiki.freepascal.org/UCSD_Pascal](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/UCSD_Pascal)_
