# $Bitpacking

│ **[Deutsch (de)](</$Bitpacking/de> "$Bitpacking/de")** │  **English (en)** │    
****

  
Back to [local compiler directives](<local_compiler_directives.md> "local compiler directives"). 

  
The local compiler directive **$BITPACKING** : 

  * is used to compress records;
  * knows the switches ON and OFF;
  * tells the compiler whether it should use bitpacking or not when it encounters the [Packed](<Packed.md> "Packed") keyword for a structured type.



Example: 
    
    
    ...
    {$BITPACKING ON}
    ...
    Type
       TMyRecord = packed record
         B1, B2, B3, B4 : Boolean;
       end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** 1\. The local compiler directive **$BITPACKING** is ignored in MacPas compiler mode. In the MacPas compiler mode, all packed records are aligned to bit addresses.   


2\. The reserved word [Bitpacked](</index.php?title=Bitpacked&action=edit&redlink=1> "Bitpacked \(page does not exist\)") can force alignment to bit addresses, regardless of the $BITPACKING directive and regardless of the $MODE.

## See also

  * [Bit manipulation](<Bit_manipulation.md> "Bit manipulation")

---

_Source: [https://wiki.freepascal.org/$Bitpacking](https://web.archive.org/web/20241004025503/https://wiki.freepascal.org/$Bitpacking)_
