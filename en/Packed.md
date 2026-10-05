# Packed

│ **English (en)** │  **[русский (ru)](<../ru/Packed.md>)** │

The [reserved word](<Reserved_word.md> "Reserved word") `packed` tells the compiler to use as little memory as possible for a particular complex data type. Without specifying `packed`, the compiler may insert extra unused bytes between members in order to align the data on full word boundaries for faster access by the CPU. 
    
    
     1program packedDemo(input, output, stderr);
     2
     3type
     4	signedQword = record
     5		value: qword;
     6		signum: -1..1;
     7	end;
     8	
     9	signedQwordPacked = packed record
    10		value: qword;
    11		signum: -1..1;
    12	end;
    13
    14begin
    15	writeLn('      signedQword: ', sizeOf(signedQword), 'B');
    16	writeLn('signedQwordPacked: ', sizeOf(signedQwordPacked), 'B');
    17end.
    

So `packed` sacrifices some speed while reducing the memory used. 

## Circumvention

Smart ordering of record elements can mitigate the issue. In the example above `signedQword` as well `signedQwordPacked` have `signum` located at an offset of `8`. That is OK. If for any reason `value` and `signum` were listed in reverse order, `signum` would appear in both versions at an offset of `0`, but in the `packed` version `value` has an offset of `1` Byte. This might cause a potentially slower access to the `value` field. 

## See also

  * [`{$align}`](</index.php?title=sA&action=edit&redlink=1> "sA \(page does not exist\)")
  * [`{$pack}`](</index.php?title=sPack&action=edit&redlink=1> "sPack \(page does not exist\)")
  * procedures [`pack`](<https://www.freepascal.org/docs-html/rtl/system/pack.html>) and [`unpack`](<https://www.freepascal.org/docs-html/rtl/system/pack.html>)
  * [“Packing and unpacking an array” in § “Arrays” of the Reference Guide](<https://www.freepascal.org/docs-html/ref/refsu14.html>)
  * [`bitpacked`](</index.php?title=Bitpacked&action=edit&redlink=1> "Bitpacked \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Packed](https://web.archive.org/web/20230610203044/https://wiki.freepascal.org/Packed)_
