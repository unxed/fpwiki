# iconvenc

The **iconvenc** [package](<Package_List.md> "Package List") is a header file for the POSIX iconv posix library on Unix. Note that currently we try to keep this header portable, so GNUisms are not allowed. A simple Pascal wrapper to do a string conversion is also added. 

## Contents

  * 1 Buffering
  * 2 Credits
  * 3 linking method
  * 4 Naming



## Buffering

While creating the header, some confusion arose over the fact that the library didn't always seem to write the last few chars. This turned out to be a buffering in the iconv lib, which isn't really spellled out in the man pages (though the below URLs do mention it cryptically). 

The lib does autoflushed if the input string is 0 terminated (IOW if the count of chars to be passed is strlen(pchar)+1 or length(ansistring)+1), but an additional iconv(nil,nil,dest,outcharcount); will flush the buffer. (see the iconvert routine) 

## Credits

Thanks to Thomas Schatzl for his help solving the autoflushing issue, also thanks for Ido Kanner for helping with the testing setup, and Micha Nelissen and Ales Katona for minor help and testing. 

## linking method

The header file allows both dynamic as static linking, with static being the default. {$define loaddynamic} to make it try multiple libraries. A function to try additional library names is also provided. 

## Naming

The unit is called iconvenc because the main function already is named iconv. 

URLS 

  * <http://www.opengroup.org/onlinepubs/009695399/functions/iconv.html>
  * <http://www.gnu.org/software/libiconv/documentation/libiconv/iconv.3.html>

---

_Source: [https://wiki.freepascal.org/iconvenc](https://web.archive.org/web/20210617051852/https://wiki.freepascal.org/iconvenc)_
