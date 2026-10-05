# move

The [`procedure`](<Procedure.md> "Procedure") **[`Move`](<https://www.freepascal.org/docs-html/rtl/system/move.html>)** (originated from [Borland Pascal](<Borland_Pascal.md> "Borland Pascal")) _copies_ data from one location in memory to another. Unlike the name suggests, no data is “lost” at the source. Its signature reads: 
    
    
    procedure Move(const source; var destination; count: sizeInt)
    

Note that `source` and `destination` do not have a data type, thus specifying an [Identifier](<Identifier.md> "Identifier") _without_ (necessarily) applying the [address‑operator](<@.md> "@") is sufficient. 

## Application

`Move` can be considered a very “low-level” routine. It was originally developed to copy `char` values _within_ the _same_ [`string`](<String.md> "String"), but has ever since been (ab‑)used for other purposes, too: 

  * If Pascal’s strong data type system imposes (too many) restrictions that need to be circumvented for hardware-close programming and [typecasting](<Typecast.md> "Typecast") does not resolve the task’s problem or is too cumbersome.
  * For circumventing Pascal’s restrictions in a very “hacky” nature: E. g. _copying_ a _[`file`](</File> "File") variable_ (there are good reasons you cannot simply [`:=`](<Becomes.md> "Becomes") to a `file`).
  * For performance reasons: Copying _huge_ blocks of memory with `move` _could_ be faster than an equivalent [`for`‑loop](<For.md> "For"). For instance the [x86‑64 implementation](<https://gitlab.com/freepascal.org/fpc/source/blob/release_3_2_0/rtl/x86_64/x86_64.inc#L75-351>) takes advantage of architecture-specific circumstances.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Using `move` often makes your code very unportable since it often makes assumptions about endianness, internal structures or similar things.

A quick demo should not be missing. Nevertheless, this example is _bad_ in nature. A plain `:=` assignment would have been sufficient. Note, using `move` possibly prevents the compiler from performing certain [optimizations](<Optimization.md> "Optimization"). 
    
    
    program moveDemo(input, output, stdErr);
    var
    	x, y: integer;
    begin
    	x := 42;
    	move(x, y, sizeOf(x));
    	writeLn(y);
    end.
    

## See also

  * [`moveChar0`](<https://www.freepascal.org/docs-html/rtl/system/movechar0.html>) \- `move` until first `chr(0)` but _at most_ `count` `char` values.
  * [`copy`](<https://www.freepascal.org/docs-html/rtl/system/copy.html>) \- creates a copy of data on the heap and returns a pointer to it.

---

_Source: [https://wiki.freepascal.org/move](https://web.archive.org/web/20250219120928/https://wiki.freepascal.org/move)_
