# Rewrite

The [`procedure`](<Procedure.md> "Procedure") [`rewrite`](<https://www.freepascal.org/docs-html/rtl/system/rewrite.html>) clears and opens a file for writing. If the file did not exist, it is created. 

## signature

  * `rewrite(var destination: text)`
  * `rewrite(var destination: file of T)` where `T` is an acceptable record type
  * `rewrite(var destination: file; recordsize: longInt = 128)` ([FPC](<FPC.md> "FPC") extension)



## behavior

After invoking `rewrite` it is guaranteed that 

  * the destination file is completely undefined (i. e. empty),
  * the writing cursor is at the beginning of the file, and
  * the file is writable and non-readable.



## see also

  * [`reset`](</index.php?title=Reset&action=edit&redlink=1> "Reset \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Rewrite](https://web.archive.org/web/20250321112920/https://wiki.freepascal.org/Rewrite)_
