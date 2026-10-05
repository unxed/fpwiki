# cdrom

│ **English (en)** │


The cdrom package contains some routines to read the table of contents (TOC) of a CD-ROM. From this TOC a DISC ID can be computed which can be used to query a CDDB server such as the one on freecddb.org. This unit is compatible with Linux (only tested on x86) and Windows (only tested on 32-bit) 

The package consists of 2 main units, and some auxiliary units: 

  * **cdrom** : this unit contains the routines to determine which CD-ROM devices exist on the system, and to read the TOC of a cdrom in one of the devices. 
  * **discid** : this unit calculates a DISC ID based on the TOC of a CD-ROM. This disc ID can be used to send a query to a cddb database server. A routine exists which returns the exact query string for the CDDB server. 



Other than these units, there exist some auxiliary units: 

  * **lincd** a unit which contains essentially the translation of the linux kernel cdrom interface (linux/cdrom.h header file). 
  * **major** a unit containing the definitions of the major device numbers on linux. 
  * **wincd** a unit containing windows routines to enumerate cd-rom drives and read the TOC of a CDROM. It supports SPTI, IOCTL and ASPI interface calls to accomplish this task. 
  * **wnaspi32** the win32 interface to ASPI (SCSI programming interface for Windows) 
  * **scsidefs** some constant and type definitions for the SCSI layer on Windows. 



There are also 2 test programs to demonstrate the capabilities of these units: 

  * **showcds** : demonstrates the cdrom unit, shows the available CD-ROM disc drives on the system. 
  * **getdiscid** : demonstrates the cdrom and discid unit by calculating the disc ID from a cdrom in a disc drive. 



Go back to [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/cdrom](https://web.archive.org/web/20170128042142/https://wiki.freepascal.org/cdrom)_
