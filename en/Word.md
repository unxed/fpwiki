# Word

│ **[Deutsch (de)](</Word/de> "Word/de")** │  **English (en)** │  **[suomi (fi)](</Word/fi> "Word/fi")** │  **[français (fr)](</Word/fr> "Word/fr")** │  **[русский (ru)](<../ru/Word.md> "Word/ru")** │    
****

A **word** is the processor’s native data unit. Modern consumer processors have a word width of [64 bits](<64_bit.md> "64 bit"). 

## Data type

Most [run-time libraries](<RTL.md> "RTL") provide the native data type of a processor as the Pascal data type `word`. It is a subset of all whole numbers (non-negative integers) that can be represented by the processor’s natural data unit size. 

On a 64-bit architecture this means a `word` is an integer within the range [math]\displaystyle{ [0,~2^{64}-1] }[/math]. On a 32-bit architecture a `word` will be an integer in the range [math]\displaystyle{ [0,~2^{32}-1] }[/math], and so on, respectively. 

In [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") a `word` is just an alias for [`cardinal`](<Cardinal.md> "Cardinal"), which has the same properties regarding possible values. 

If a _signed_ integer having the processor’s native size is wanted, the data type [`integer`](<Integer.md> "Integer") provides this functionality. 

### FPC

For source compatibility reasons, [FPC](<FPC.md> "FPC") defines `word` in the same way as Turbo Pascal and Delphi: the subrange data type `0..65535`. The `high` value `65535` is [math]\displaystyle{ 2^{16}-1 }[/math]. Thus a [`system.word`](<https://www.freepascal.org/docs-html/rtl/system/word.html>) occupies two bytes of space. Subrange data types are stored in a quantity that serves best the goals of performance and memory efficiency. 

The processor’s native word size, as defined above, corresponds to different types depending on the purpose you want to use it for: 

  * the (as of 2022 still undocumented) [`system.ALUSint`](<https://www.freepascal.org/docs-html/rtl/system/alusint.html>) and [`system.ALUUint`](<https://www.freepascal.org/docs-html/rtl/system/aluuint.html>) types correspond to the native word size used by the processor’s ALU (arithmetic and logical unit), as defined at the beginning of this page. In general, this type should not be used in high level code. Instead, choose a data type based on the values it should be able to represent, as this is safer and more portable. It is the compiler’s job to generate optimal code.
  * [`system.CodePtrUInt`](<https://www.freepascal.org/docs-html/rtl/system/codeptruint.html>) corresponds to the size of pointers to code, such as the address of a [procedure](<Procedure.md> "Procedure"). This can be different from a pointer to data, e. g. on targets that support multiple [memory models](<DOS.md> "DOS").
  * [`system.PtrUInt`](<https://www.freepascal.org/docs-html/rtl/system/ptruint.html>) corresponds to the size of pointers to data.



On many platforms, all of these types have the same size, but it is not the case everywhere. 

In FPC a [`smallInt`](<Smallint.md> "Smallint") has the same size as a `word`, but is signed. 

  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") `word`  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Word](https://web.archive.org/web/20250425035457/https://wiki.freepascal.org/Word)_
