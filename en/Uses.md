# Uses

│ **English (en)** │

The `uses` clause of a Pascal module imports exported [identifiers](<Identifier.md> "Identifier") from another module. It was introduced by [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") and virtually every modern Pascal [compiler](<Compiler.md> "Compiler"), including [FPC](<FPC.md> "FPC"), supports it, if no other mechanism is available. 

`Uses` is a [reserved word](<Reserved_word.md> "Reserved word"). 

## Contents

  * 1 usage
    * 1.1 location
    * 1.2 syntax
    * 1.3 access and shadowing
    * 1.4 loading and unloading
    * 1.5 special units
  * 2 see also



## usage

### location

Every Pascal module – i. e. [`program`](<Program.md> "Program"), `unit`, or `library` – can have at most one `uses` clause per section. It has to appear right after the section headings. Section headings are `interface` and `implementation` in a `unit`. A `program` does not have any explicit section headings, thus the `uses` clause appears immediately after the `program` header, but still after any [global compiler directives](<global_compiler_directives.md> "global compiler directives") as some may alter the `uses` clause. 

### syntax

The `uses` clause consists of the word `uses`, followed by a [comma](<Comma.md> "Comma")-separated list of module names, which is terminated by a [semicolon](</;> ";"). The modules listed in the clause have to be capable of _exporting_ identifiers, that means a `program` name may not appear in the list. Usually [`unit`](<Unit.md> "Unit") names are listed in the clause. The module name of the module, that is about to be defined, can not appear in the list. Example: 
    
    
    uses
    	math, sysUtils, baseUnix;
    

Furthermore, each identifier may be followed by [`in`](<In.md> "In") and a string literal overriding the automatic lookup mechanism. The following will look for a unit named `foo` in the file `bar.pas`: 
    
    
    uses
    	foo in 'bar.pas';
    

### access and shadowing

This makes identifiers exported by the listed modules, for units that means identifiers declared in the `interface` section, known in the current module. Either the fully-qualified identifier which is prefixed by the module name, and just the identifier’s stem can be used to refer to the same identifier. For instance, [`math.ceil`](<https://www.freepascal.org/docs-html/rtl/math/ceil.html>) as well as just `ceil` refer to the same function (unless in the current module `ceil` has been defined otherwise). 

However, two or more units listed in the `uses` clause might declare the same identifier. Then, only the identifier declared by the unit later in the list can be referred to using the short notation. Identifiers declared by earlier units in the `uses` clause can only be accessed via fully-qualified identifiers. A [`with`-clause](<With.md> "With") may alleviate this situation. 

### loading and unloading

At program start, all [`initialization`](<Initialization.md> "Initialization") statement blocks, if any, are processed in the order the units were listed in – possibly recursively. At program termination, all [`finalization`](<Finalization.md> "Finalization") are processed _in the reverse order_. 

If initialization fails, only units that have been initialized so far are finalized. The main block of a program is not executed at all. Confer also [FPC issue 0036754](<https://gitlab.com/freepascal.org/fpc/source/-/issues/0036754>). 

### special units

In FPC the [unit `system`](<System_unit.md> "System unit") is implicitly included by every program. It is wrong to explicitly list it in the `uses` clause. Inclusion of the `system` unit can be disabled by using the `‑Us` compiler switch. This switch indicates/ought to indicate, that a/the system unit is about to be compiled. 

## see also

  * [namespaces](<Namespaces.md> "Namespaces")
  * [§ About "in" keyword of the "uses" section in the _Free Pascal reference guide_](<https://www.freepascal.org/docs-html/ref/refse111.html>)
  * [§ “Unit dependencies” in the _Free Pascal reference guide_](<https://www.freepascal.org/docs-html/ref/refse114.html>)

---

_Source: [https://wiki.freepascal.org/Uses](https://web.archive.org/web/20240920204055/https://wiki.freepascal.org/Uses)_
