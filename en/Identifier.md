# Identifier

│ **English (en)** │  **[русский (ru)](<../ru/Identifier.md>)** │

An **identifier** is a symbol used to unambiguously denote an object. Such objects could be for instance 

  * modules such as a [`program`](<Program.md> "Program") or [`unit`](<Unit.md> "Unit"),
  * [constants](<Const.md> "Const") or [variables](<Variable.md> "Variable"),
  * [routines](<Routine.md> "Routine") and [labels](<Label.md> "Label"), or
  * [types](<Type.md> "Type").



  


## Contents

  * 1 Declaration
  * 2 Example
  * 3 Significant characters
  * 4 Choosing good identifiers
  * 5 See also



## Declaration

In [Pascal](<Pascal.md> "Pascal") an identifier has to be [declared](<Declaration.md> "Declaration") prior its first use. Depending on the object’s nature the identifier declaration is crafted in one manner or another, but the general scheme [math]\displaystyle{ (\text{identifier}, \text{object}) }[/math] stays the same (2-tuple [that means, it is ordered]). 

An identifier has to consist of at least one [ASCII](<ASCII.md> "ASCII") letter or underscore. After the first character ASCII digits may follow, too. In the case of `label`s, pure numeric identifiers are allowed, too. 

In Pascal identifiers are _case-insensitive_. `Foo`, `fOO`, and `FoO` identify the very same object. This rule applies only for Pascal code, though. Inside inline-assembler statements [`asm` … `end`](<Asm.md> "Asm") identifiers are case-sensitive and may have to adhere to different structures. 

Identifiers must not be [reserved words](<Reserved_word.md> "Reserved word"). However, since [FPC](<FPC.md> "FPC") retroactively declared some identifiers as reserved words, it introduced the [`&`-escape](<&.md> "&") to allow their usage as identifiers anyway. Note, that the set of reserved words depends on the active [compiler compatibility mode](<Compiler_Mode.md> "Compiler Mode"). 

Identifiers are only valid in the [scope](<Scope.md> "Scope") they were declared in. In other [blocks](<Block.md> "Block") a symbol may have a different meaning or hide an already declared identifier. 

## Example
    
    
    program identifierDemo(input, output, stdErr);
    
    const
    	answer = 42;
    
    var
    	x: integer;
    
    procedure foo;
    begin
    	x := answer;
    end;
    
    begin
    	foo;
    end.
    

This program contains the identifiers: 

  * `identifierDemo`
  * `input`
  * `output`
  * `stdErr`
  * `answer`
  * `x`
  * `integer`
  * `foo`



## Significant characters

FPC considers up to 127 characters of an identifier as significant. A character being significant means, that only the first [math]\displaystyle{ n }[/math] characters are actually considered while determining whether one object or another is being referred to. 

It is allowed to write identifiers _longer_ than 127 characters. However, if two objects are/ought to be referred by two identifiers that start off with the same 127 characters, the 128th and beyond characters are ignored. The compiler will fail with a duplicate declaration compile-time error. 

Due to internal reasons, all words are capped to 255 characters, though. (This limit does not concern string literals, which are – mathematically speaking – objects.) 

Historically, there were [compilers](<Compiler.md> "Compiler") that considered only the first eight characters as significant. This evoked very cryptic identifiers. 

## Choosing good identifiers

There are some considerations in picking a “good” identifier: 

  * Pascal’s strong typing system inherently “stores” an associated type with every identifier. Duplicating this information in writing is discouraged. If an identifier’s nature changes during development progress, it had to be changed throughout the entire code base in order to reflect the change. Thus, write what an identifier _means_ or is good for, not what its underlying structure is.
  * Keep all identifiers in one (natural) language. Unless you are writing a program for educational purposes, English is de facto _the_ lingua franca in programming.
  * Do not use FPC’s entire width of permissible significant characters.
  * Do not omit vowels or use other shortening strategies. It tends to produce unreadable code.
  * Avoid using the same words as identifiers in multiple places, unless necessary or guaranteed to be free of conflicts, or intentional but _documented_.



## See also

  * [`with`](<With.md> "With")
  * [§ “Identifiers” in the _Free Pascal reference guide_](<https://www.freepascal.org/docs-html/ref/refse4.html>)
  * [Article “Identifiers (computer languages)” in the _English Wikipedia_](<https://en.wikipedia.org/wiki/Identifier_\(computer_languages\)>)

---

_Source: [https://wiki.freepascal.org/Identifier](https://web.archive.org/web/20240920204158/https://wiki.freepascal.org/Identifier)_
