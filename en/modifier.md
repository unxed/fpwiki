# modifier

│ **English (en)** │

**Modifiers** are [keywords](<Keyword.md> "Keyword") modifying the standard behavior of a [Pascal](<Pascal.md> "Pascal") language construct, although some modifiers merely serve the purpose of being a _hint_ to the compiler. 

## Contents

  * 1 declaration modifiers
    * 1.1 allocation
    * 1.2 routines
    * 1.3 hint directives
  * 2 call modifiers
    * 2.1 calling convention
    * 2.2 routine behavior hints
    * 2.3 position
    * 2.4 optimization requests
    * 2.5 other
  * 3 properties
  * 4 object-oriented programming
    * 4.1 access modifiers
    * 4.2 virtual methods
    * 4.3 classes
  * 5 miscellaneous
    * 5.1 loops
    * 5.2 generics
    * 5.3 libraries
    * 5.4 memory
    * 5.5 type helpers
  * 6 see also



## declaration modifiers

### allocation

  * [`absolute`](<Absolute.md> "Absolute")
  * [`alias`](<alias.md> "alias")
  * `cVar`
  * `export`
  * [`external`](<External.md> "External")
  * [`name`](<Name.md> "Name")
  * `public`



### routines

  * [`forward`](<Forward.md> "Forward")
  * `overload`



### hint directives

If `{$modeSwitch hintDirectives+}`, the following [hint modifiers](<Hint_Directives.md> "Hint Directives") are available too: 

  * `deprecated`
  * `experimental`
  * `platform`
  * `unimplemented`



These “modifiers” actually have no effect on the generated code, but can be promoted to errors using [`{$warn}`](<$warn.md> "$warn") (so, for example, in order to ensure `deprecated` functionality is not used in a release version). 

## call modifiers

### calling convention

  * [`cDecl`](</index.php?title=Cdecl&action=edit&redlink=1> "Cdecl \(page does not exist\)")
  * [`cppDecl`](<Cppdecl.md> "Cppdecl")
  * `interrupt`
  * [`oldFpcCall`](<oldfpccall.md> "oldfpccall")
  * [`pascal`](<pascal.md> "pascal")
  * [`register`](<Register.md> "Register")
  * [`safeCall`](<Safecall.md> "Safecall")
  * `saveRegisters`
  * [`stdCall`](<Stdcall.md> "Stdcall")
  * `varArgs` (in conjunction with `cDecl`)
  * `winApi`



### routine behavior hints

  * `IOCheck`
  * `noReturn`



### position

  * [`far`](<Far.md> "Far")
  * `far16`
  * [`local`](<Local.md> "Local")
  * [`near`](<Near.md> "Near")



### optimization requests

  * [`inline`](<Inline.md> "Inline")
  * [`noStackFrame`](</index.php?title=Nostackframe&action=edit&redlink=1> "Nostackframe \(page does not exist\)")



### other

  * `assembler`
  * `softFloat`



## properties

  * `default`
  * `index`
  * `noDefault`
  * `read`
  * `stored`
  * `write`



## object-oriented programming

### access modifiers

  * [`private`](<Private.md> "Private")
  * [`protected`](<Protected.md> "Protected")
  * `public`
  * [`published`](<Published.md> "Published")
  * `strict` (only in combination with one of the four visibility specifiers listed above)



### virtual methods

  * [`abstract`](</index.php?title=Abstract&action=edit&redlink=1> "Abstract \(page does not exist\)")
  * `dynamic`
  * [`override`](<Override.md> "Override")
  * [`reintroduce`](<Reintroduce.md> "Reintroduce")
  * [`virtual`](<Virtual.md> "Virtual")



### classes

  * `enumerator`
  * `static`
  * `implements`
  * `message`



## miscellaneous

### loops

  * [`break`](<Break.md> "Break")
  * [`continue`](<Continue.md> "Continue")



### generics

  * `generic`
  * `specialize`



### libraries

  * [`index`](</index.php?title=Index&action=edit&redlink=1> "Index \(page does not exist\)")



### memory

  * [`bitpacked`](</index.php?title=Bitpacked&action=edit&redlink=1> "Bitpacked \(page does not exist\)")
  * `unaligned`



### type helpers

  * `helper`



## see also

  * [reserved words](<Reserved_word.md> "Reserved word")

---

_Source: [https://wiki.freepascal.org/modifier](https://web.archive.org/web/20250219112959/https://wiki.freepascal.org/modifier)_
