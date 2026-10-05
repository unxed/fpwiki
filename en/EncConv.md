# EncConv

## About

**EncConv** is a package which gives unit EncConv, to convert text to/from UTF8. It was based on LazUtils' unit **LConvEncoding** , but API was changed and simplified. The list of supported encodings is the same as in LConvEncoding. 

Author: Alexey Torgashin 

License: MPL 2.0 

## Changes since LConvEncoding

  * LConvEncoding has string constants to identify encodings, and it checks for those strings on each encoding operation, which is little slow. EncConv has Enum to identify encodings. And it has array to access encoding/decoding functions, which is based on mentioned Enum. That is optimization.


    
    
    type
      TEncConvId = (
        eidUTF8,
        eidUTF8BOM,
        eidUCS2LE,
        eidUCS2BE,
    
        eidCP1250,
        eidCP1251,
        eidCP1252,
        eidCP1253,
        eidCP1254,
        eidCP1255,
        eidCP1256,
        eidCP1257,
        eidCP1258,
    
        eidCP437,
        eidCP850,
        eidCP852,
        eidCP861,
        eidCP865,
        eidCP866,
        eidCP874,
    
        {$IFnDEF encconv_noasian}
        eidCP932,
        eidCP936,
        eidCP949,
        eidCP950,
        {$ENDIF}
    
        eidISO1,
        eidISO2,
        eidISO3,
        eidISO4,
        eidISO5,
        eidISO7,
        eidISO9,
        eidISO10,
        eidISO13,
        eidISO14,
        eidISO15,
        eidISO16,
    
        eidCPMac,
        eidKOI8R,
        eidKOI8U,
        eidKOI8RU
        );
    

  * EncConv doesn't give the parameter to apply encoding to FPC String variable (this is feature of FPC 3). EncConv treats strings as UTF-8 strings (so after a conversion UTF-8->nnnnn is performed, string may not contain valid UTF-8 characters). It also means that OS encodings API (iconv on Unix, etc) is not used in conversions.


  * Main functions in EncConv unit are:


    
    
    function EncConvFindEncoding(const s: string): TEncConvId;
    
    function EncConvertFromUTF8(const S: string; Enc: TEncConvId): string;
    function EncConvertToUTF8(const S: string; Enc: TEncConvId): string;
    
    function EncConvGetANSI: TEncConvId;
    function EncConvGetOEM: TEncConvId;
    

  * String names of ISO-8859-x encodings were changed a little: 'iso88591' -> 'iso-8859-1' etc.



## Download

GitHub: <https://github.com/Alexey-T/EncConv>

---

_Source: [https://wiki.freepascal.org/EncConv](https://web.archive.org/web/20250417153149/https://wiki.freepascal.org/EncConv)_
