# BitSizeOf

│ **[Deutsch (de)](</BitSizeOf/de> "BitSizeOf/de")** │  **English (en)** │ 

The compiler intrinsic function **`bitSizeOf`** returns the size of a datum in bits. 

## definition

`BitSizeOf` produces different results only for members of [`bitpacked`](</index.php?title=Bitpacked&action=edit&redlink=1> "Bitpacked \(page does not exist\)") structures. In all other cases the result of `bitSizeOf` is simply `8 * sizeOf(x)` (or `sizeOf(x) shl 3`). To put this relation in other words: 

[math]\displaystyle{ \texttt{sizeOf}(\texttt{x}) = \lceil \, \texttt{bitSizeOf}(\texttt{x}) \; / \; 8 \, \rceil }[/math]

## application

In some cases using `bitSizeOf` can improve your code’s readability: 
    
    
    program binaryPrint(input, output, stdErr);
    var
    	(* in FPC `integer` definition *)
    	(* depends on the selected mode *)
    	i: integer;
    begin
    	readLn(i);
    	writeLn(binStr(i, bitSizeOf(i)));
    end.
    

If your code makes presumptions about memory requirements, e. g. by using an algorithm exploiting certain properties of your `bitpacked` data type, you may want to safeguard your code with some [conditional compilation](<Conditional_compilation.md> "Conditional compilation"), may be even just for documentation purposes. 

## see also

  * [`sizeOf`](<SizeOf.md> "SizeOf")

---

_Source: [https://wiki.freepascal.org/BitSizeOf](https://web.archive.org/web/20240425041532/https://wiki.freepascal.org/BitSizeOf)_
