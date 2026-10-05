# Page

The [standard](<Standard_Pascal.md> "Standard Pascal") [`procedure`](<Procedure.md> "Procedure") **`page`** advances the output of a given textfile by one page. _Page_ could refer to a piece of paper if the destination is a physical printer, or could imply blanking the entire screen, the specific behavior is implementation-defined. 

## Contents

  * 1 signature
  * 2 behavior
  * 3 implementation
  * 4 notes



## signature

`Page` takes _one_ [`text` file](<Text.md> "Text") as a parameter. It determines the destination. If the destination is [`output`](<Output.md> "Output"), it can be omitted. `page;` is short for `page(output);`. 

## behavior

Provided the destination is open for writing: 

  1. If not at the _beginning_ of a line, do a [`writeLn(destination)`](<writeln.md> "writeln").
  2. Implementation-defined method of advancing to the next “page”.



## implementation

  * On IBM mainframes, advancing to the next page was achieved by putting a `1`, that is the character of the digit 1, in the first column of a line.
  * On most machines using [ASCII](<ASCII.md> "ASCII") though, it simply means emitting a form feed character, i. e. [`chr(12)`](<Chr.md> "Chr"). The [FPC](<FPC.md> "FPC") and the [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") Compiler use this method.



## notes

  * In the FPC `page` is only available in [`{$mode ISO}`](<Mode_iso.md> "Mode iso") and [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal").
  * `Page` is a regular [identifier](<Identifier.md> "Identifier") and as such can be redefined.

---

_Source: [https://wiki.freepascal.org/Page](https://web.archive.org/web/20241209230603/https://wiki.freepascal.org/Page)_
