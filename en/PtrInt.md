# PtrInt

The data types **[`ptrInt`](<https://www.freepascal.org/docs-html/rtl/system/ptrint.html>)** (“Peter Int”) and **[`ptrUInt`](<https://www.freepascal.org/docs-html/rtl/system/ptruint.html>)** (“Pee true Int”) are signed and unsigned [`integer`](<Integer.md> "Integer") [data types](<Data_type.md> "Data type") respectively having the same [`sizeOf`](<SizeOf.md> "SizeOf") of a [`pointer`](<Pointer.md> "Pointer"). 

## application

  * Use `ptrUInt` if an `integer` value will eventually be [typecasted](<Typecast.md> "Typecast") to a `pointer`.
  * Regardless of the size taken up by its elements, an [`array`](<Array.md> "Array") cannot have more than `high(ptrInt)` elements. Additionally, the range type must be a subrange of `ptrInt`.[[1]](<https://www.freepascal.org/docs-html/current/user/userse62.html>)



## notes

  * `PtrInt`/`ptrUInt` are not necessarily the same size of `ALUSInt`/`ALUUInt`.
  * The introduction of `ptrInt` was a mistake. New code should not use it.
  * [`IntPtr`](<https://www.freepascal.org/docs-html/rtl/system/intptr.html>) and [`nativeInt`](<https://www.freepascal.org/docs-html/rtl/system/nativeint.html>) are aliases for `ptrInt`.
  * [`UIntPtr`](<https://www.freepascal.org/docs-html/rtl/system/uintptr.html>) and [`nativeUInt`](<https://www.freepascal.org/docs-html/rtl/system/nativeint.html>) are aliases for `ptrUInt`.
  * `PtrInt` and `ptrUInt` are redefined by the `unit` `unicodeData`.

---

_Source: [https://wiki.freepascal.org/PtrInt](https://web.archive.org/web/20250219112728/https://wiki.freepascal.org/PtrInt)_
