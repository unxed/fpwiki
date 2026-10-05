# PPUDump

│ **English (en)** │  **[français (fr)](</PPUDump/fr> "PPUDump/fr")** │    
****

From [FreePascal User's Manual](<ftp://ftp.freepascal.org/pub/fpc/docs-pdf/user.pdf>): 

ppudump is a program, which shows the contents of a Free Pascal unit. It is distributed with the compiler. You can just issue the following command 
    
    
     ppudump [options] foo.ppu
    

to display the contents of the foo.ppu unit. You can specify multiple files on the command line. 

The options can be used to change the verbosity of the display. By default, all available information is displayed. You can set the verbosity level using the -Vxxx option. Here, xxx is a combination of the following letters: 

h: show header info. 

i: show interface information. 

m: show implementation information. 

d: show only (interface) definitions. 

s: show only (interface) symbols. 

b: show browser info. 

a: show everything (default if no -V option is present). 

  


# example output

ppudump unit1.ppu 
    
    
    PPU-Analyser Version 3.1.1
    Copyright (c) 1998-2013 by the Free Pascal Development Team
    
    Analyzing unit1.ppu (v195)
    
    Header
    -------
    Compiler version        : 3.1.1
    Target processor        : x86_64
    Target operating system : Linux-x86-64
    Unit flags              : init, static_linked, little_endian, has_debug_info, dwarf_debug_info, local_symtable
    FileSize (w/o header)   : 97855
    Checksum                : 52269C9E
    Interface Checksum      : 8B95EA98
    Indirect Checksum       : DA0A4D1C
    Definitions stored      : 312
    Symbols stored          : 960
    
    Interface section
    ------------------
    Module Name: Unit1
    
    Source file 1 : unit1.pas 2017/10/19 12:04:48
    Source file 2 : unit1.lrs 2017/10/19 11:26:00
    Uses unit: System (Crc: 0071642B, IntfcCrc: 675CDF65, IndCrc: 3EF9017F)
    Uses unit: objpas (Crc: 0C0DE648, IntfcCrc: 8F0C59F0, IndCrc: 09D16E16)
    Uses unit: Classes (Crc: 2BD4311F, IntfcCrc: FEC1B15D, IndCrc: A2A74441)
    Uses unit: sysutils (Crc: 673DB7A8, IntfcCrc: FB3278AD, IndCrc: 029F2F57)
    Uses unit: FileUtil (Crc: DDDF8F2E, IntfcCrc: 91946678, IndCrc: 773AB234)
    Uses unit: LResources (Crc: B1B5CBCA, IntfcCrc: BD6404C3, IndCrc: 07FAEA28)
    Uses unit: Forms (Crc: 7BD93F9B, IntfcCrc: 4CED5DD1, IndCrc: 5876762D)                                    
    Uses unit: Controls (Crc: B19F0092, IntfcCrc: 748FCEC1, IndCrc: 7E2DABAF)                                 
    Uses unit: Graphics (Crc: 047DF96C, IntfcCrc: 7F769B7A, IndCrc: 25206CE8)                                 
    Uses unit: Dialogs (Crc: F3CCBFDB, IntfcCrc: BA984012, IndCrc: 64641653)                                  
    Uses unit: StdCtrls (Crc: C7C6F1A3, IntfcCrc: 320098AF, IndCrc: D6382504)                                 
    Uses unit: uCmdBox (Crc: 63BFCCE9, IntfcCrc: FA038A54, IndCrc: 7A21D263)                                  
    Uses unit: GifAnim (Crc: 75FA8DA5, IntfcCrc: EB25FB6B, IndCrc: 65C55C81)                                  
    Link unit object file: unit1.o (static )                                                                  
    DerefMapsize: 15
    DerefMap[0] = SYSTEM
    DerefMap[1] = FORMS
    DerefMap[2] = CONTROLS
    DerefMap[3] = CLASSES
    DerefMap[4] = STDCTRLS
    DerefMap[5] = LCLCLASSES
    DerefMap[6] = WSLCLCLASSES
    DerefMap[7] = TYPES
    DerefMap[8] = LCLTYPE
    DerefMap[9] = LMESSAGES
    DerefMap[10] = GRAPHTYPE
    DerefMap[11] = MENUS
    DerefMap[12] = GRAPHICS
    DerefMap[13] = OBJPAS
    DerefMap[14] = LRESOURCES
    Derefdata length: 18605
    
    Interface symtable
    ----------------------
    Symtable options: none
    
    Interface definitions
    ----------------------
    ** Definition Id 0 **
    Object/Class definition
          Type symbol : (0) SymId 2
           DefOptions : 
            DefStates : VMT Written, RTTITable Used, InitTable Used, RTTITable Written, InitTable Written, Dwarf DbgInfo Written
        Name of Class : TForm1
       Import lib/pkg : 
              Options : HasVirtual, HasPrivate, HasProtected, HasConstructor, HasDestructor, HasVMT, CanHavePublished
                 Type : class
          Helper Type : none
        External name : 
             DataSize : 1904
          PaddingSize : 0
           FieldAlign : 0
          RecordAlign : 8
       RecordAlignMin : 0
           VmtField : (8) Unit 0, SymId 4244
       Ancestor Class : (17) Unit 1, DefId 1093
     Abstract methods : 0
      VMT entries: 307
        (129) Unit 1, DefId 864
          Visibility: public
        (138) Unit 5, DefId 10
          Visibility: public
        (147) Unit 0, DefId 749
          Visibility: public
        (156) Unit 3, DefId 1586
          Visibility: public
    ----
    

... lots of info

---

_Source: [https://wiki.freepascal.org/PPUDump](https://web.archive.org/web/20250122121821/https://wiki.freepascal.org/PPUDump)_
