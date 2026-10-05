# Extended Pascal

**Extended Pascal** is the name given to the version of Pascal specified in International Organization for Standardization (ISO) standard 10206. It specifies an enhanced version of the [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") language that was specified in the ISO 7185 specification. The 214-page ISO 10206 document was published in May 1991 and not been revised since. 

Support in Free Pascal for Extended Pascal is planned, and will be via [compiler mode `extendedPascal`](<Mode_extendedpascal.md> "Mode extendedpascal"). 

## Contents

  * 1 Enhancements to Standard Pascal
  * 2 Reserved Words
  * 3 Symbols
  * 4 Free Pascal support for Extended Pascal
    * 4.1 Already supported features
    * 4.2 Conformant arrays (not yet implemented)
    * 4.3 Set improvements (not yet implemented)
    * 4.4 Generalized ordinal functions (not yet implemeted)
    * 4.5 Modules (not yet implemented)
      * 4.5.1 Required interfaces
    * 4.6 Schemata (not yet implemented)
    * 4.7 File routines
    * 4.8 Date/Time
    * 4.9 String functions
    * 4.10 Complex Numbers
    * 4.11 Number bases (not yet implemented)
    * 4.12 Exponents
    * 4.13 New constants
    * 4.14 Ensuring the proper order of Boolean expressions (not yet implemented)
    * 4.15 The value keyword (not yet implemented)
    * 4.16 Function result variables (not yet implemented)
    * 4.17 Protected parameters (not yet implemented)
    * 4.18 Type inquiry (not yet implemented)
    * 4.19 Value constructors (not yet implemented)
    * 4.20 Local variables with dynamic size (not yet implemented)
    * 4.21 Acceptable Identifiers (not yet implemented)
  * 5 External links



## Enhancements to Standard Pascal

  * Modularity and separate compilation (implemented with new keywords **module** , **import** , **export** , **only** , **qualified** and **protected**).
  * Direct access file handing (via **SeekRead** , **SeekWrite** , **SeekUpdate** and **update** procedures, and the **position** , **LastPosition** and **empty** functions as well as an _update file mode_).
  * The procedure **extend** prepares an existing file for appending data
  * The declaration parts of a program (labels, constants, types, variables, procedures, and functions) can appear in any order
  * Complex numbers



## Reserved Words

In addition to all the [reserved words of Standard Pascal](<Standard_Pascal.md> "Standard Pascal") the following (referred to as _word-symbols_ in the ISO 10206 document) have been added to Extended Pascal: 

  * and_then (short circuit version of "and"; right side not evaluated if not necessary)
  * bindable
  * export
  * import
  * module
  * only
  * or_else (short circuit version of "or"; right side not evaluated if not necessary)
  * otherwise (used with the **case** statement and _variant records_)
  * pow (exponentiation for integer, real and complex numbers to real and integer powers)
  * protected
  * qualified
  * restricted
  * value



## Symbols

In addition to all the [ symbols of Standard Pascal](<Standard_Pascal.md> "Standard Pascal") the following have been added to Extended Pascal: 

  * ** (exponentiation for integer, real and complex numbers to real and integer powers)
  * >< (computes a set symmetric difference)
  * =>



## Free Pascal support for Extended Pascal

Free Pascal has traditionally only supported the Borland dialects of Pascal, but Extended Pascal support will be available via [compiler mode](<Compiler_Mode.md> "Compiler Mode") [extendedpascal](<Mode_extendedpascal.md> "Mode extendedpascal"). [Bug 0032549](<https://bugs.freepascal.org/view.php?id=32549>) has been set up to track its progress. Below is a list of things that have been implemented (if only in unapplied patches) and things that are planned. The planned features are subject to change. 

### Already supported features

These features are already supported in Free Pascal 3.0 and some earlier versions. 

  * The declaration parts of a program (labels, constants, types, variables, procedures, and functions) can appear in any order
  * The **ReadStr** and **WriteStr** procedures.
  * The **Halt** procedure.
  * The **otherwise** clause of case statements.
  * The [**for** ... **in**](<for-in_loop.md> "for-in loop") loop
  * Array and string slices (mostly working, though assigning to them doesn't).
  * The * and >< operators of sets.
  * The ** operator (requires the Math unit)
  * Various features from ISO 7185 not supported in the Borland dialects (requires ISO mode)



### Conformant arrays (not yet implemented)

This is the standard way to pass arrays of various sizes to routines. 

### Set improvements (not yet implemented)

In addition to the * and >< operators, ISO 10206 also has a **Card** function, for retrieving the number of elements in a set. In Free Pascal, sets will still be limited to 256 elements for the foreseeable future (see [Future Plans](<https://freepascal.org/future.var>)). 

### Generalized ordinal functions (not yet implemeted)

  * **Succ** and **Pred** can take a second argument, making them essentially equivalent to Borland's **Inc** and **Dec**.



### Modules (not yet implemented)

Modules are the standard way for modular programs, covering the same domain as UCSD units. There are two types of modules: those written in a single file and those written in two files. For modules written in one file, Free Pascal will search for them as it does for units (_modulename_.{p,pp,pas}). Suggestion for those written in two files as this is not provided in the standard: the compiler will search for the implementation file normally and then search for _modulename_.imp.{p,pp,pas}. The interface is in _modulename_.int.{p,pp,pas}, unless otherwise specified by a {$INTERFACE} directive. 

  * The **import** section specifies modules to import (much like UCSD's **uses**) and can be used with the keywords **only** (to only import certain identifiers (functions, procedures, types, variables, constants)), and **qualified** (qualified identifiers; that is, _modulename_._identifier_ , rather than just _identifier_). Identifiers can be renamed on import. An example:


    
    
    program PathTest;
    
    import
      SysUtils qualified only (GetEnvironmentVariable => GetEnv);
    
    begin
      WriteLn(SysUtils.GetEnv('PATH'));
    end.
    

  * Modules must export identifers for them to be usable to programs or other modules.
  * On export, identifers can also be renamed and the **protected** keyword can be specified so that other programs and modules can't change the variable's value.
  * In the module, the **restricted** keyword can be used to make opaque types. To use the example given in the standard:


    
    
    type
      real_widget = record
        f1: integer;
        f2: real
      end;
      widget = restricted real_widget;
    

  * The **to begin do** statement specifies what happens when the module loads (equivalent to Delphi's **initialization** section).
  * The **to end do** statement specifies what happens when the module unloads (equivalent to Delphi's **finalization** section). The example in the standard uses **to begin do** to bind a text file on module load, writes to the text file during runtime, and then closes the text file in **to end do**.



Two extensions from [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") are also planned: 

  * The **all** keyword to export all identifers from a module's interface (specified identifers can be renamed).
  * [PXSC](<PXSC.md> "PXSC")-style modules, which are essentially implementation modules without an interface part. (Free Pascal already supports PXSC-style operator overloading, as does GNU Pascal.)



#### Required interfaces

The modules **StandardInput** and **StandardOutput** are equivalent to **input** and **output** in a program's header. 

### Schemata (not yet implemented)

The standard defines a schema as a collection of similar types. Essentially, though, a schema is a data type that takes one or more arguments (called discriminants). 

  * The ISO 10206 string type is a schema type.
  * The **New** procedure can be used to set a schema's discriminants at runtime.
  * The **New** procedure can also be used to set a variant record's discriminant.



### File routines

  * The **bindable** keyword allows the binding of a variable to an external resource (not implemented, and it might stay that way; GNU Pascal also doesn't allow binding non-files)
  * Indexed files (not yet implemented)
  * The **Bind** procedure to bind a variable to an external resource
  * **Unbind**
  * **Binding** returns a **BindingType** record, whose Bound field tells if the file was successfully bound
  * **Empty**
  * **Extend**
  * **SeekRead**
  * **SeekWrite**
  * **SeekUpdate**
  * **Update**
  * **Position**
  * **LastPosition** (currently, it always returns 0, as its functioning requires support of indexed files)



### Date/Time

The **GetTimeStamp** procedure fills a **TimeStamp** record with details about the current time, which can be converted to a human-readable format via the **Date** and **Time** functions. 

### String functions

  * **Substr** , equivalent to **Copy**.
  * **Index** , equivalent to **Pos**.
  * **Trim**.
  * String comparison through **EQ** , **NE** , **LT** , **GT** , **LE** , and **GE** (equal to, not equal to, less than, greater than, less than or equal to, greater than or equal to).



### Complex Numbers

  * There's a **complex** data type. In Free Pascal, it's implemented as a record with two fields, but you shouldn't count on that. Instead use the **Re** and **Im** functions to get the real and imaginary parts, respectively.
  * Use **Cmplx** to create a complex number from Cartesian arguments or **Polar** to create one from polar arguments.
  * The standard operators and functions (**abs** , **arctan** , **cos** , **exp** , **ln** , **sin** , **sqr** , and **sqrt**) are overloaded to take complex arguments.
  * The function **arg** gets the complex number's argument.



### Number bases (not yet implemented)

It's possible to use any number base from 2 to 36. 

### Exponents

The ** operator for real exponents has existed in Free Pascal for a while, but the **pow** operator for integer exponents remains unimplemented. 

### New constants

  * **maxchar** \- The highest codepoint supported by a Pascal implementation.
  * **minreal** \- The minimum real value supported.
  * **maxreal** \- The maximum real value supported.
  * **epsreal** \- The precision of reals.



### Ensuring the proper order of Boolean expressions (not yet implemented)

ISO 10206 introduces the **and_then** and **or_else** operators when you need your expression to evaluated left to right. The **and** and **or** operators, on the other hand, can evaluate the Boolean expressions in any order. These new operators are especially useful for pointer operations, where you need to know whether a pointer is assigned before trying to dereference it. 

### The value keyword (not yet implemented)

The **value** keyword specifies the default value for a variable, type, parameter, or record field. 

  * For variables and default parameters, this is equivalent to the Delphi operator =.
  * For record fields and types, there's no direct equivalent in the Borland dialects, but the [`initialize` “management operator”](<management_operators.md> "management operators") could perform the same task.



Note: Default parameters aren't defined in ISO 10206, but are in Appendix C.4 of the [OOE](<Object-Oriented_Extensions_to_Pascal.md> "Object-Oriented Extensions to Pascal") draft. 

### Function result variables (not yet implemented)

Allows to bind a function's result to a variable, sort of like Delphi's **Result**. 

### Protected parameters (not yet implemented)

The **protected** keyword can also be used with parameters to indicated that they can't be changed in the routine, giving the compiler hints to aid optimization. To use the example given in the standard: 
    
    
    procedure illustrate(a: integer; var b: integer; protected c: integer; protected var d: integer);
    begin
      a := 1; { value param }
      b := 1; { variable param }
    { c := 1; not legal }
    { d := 1; not legal }
    end;
    

### Type inquiry (not yet implemented)

Allows you to make the type of one variable or parameter the same as another variable or parameter. 

### Value constructors (not yet implemented)

This provides a cleaner alternative to Borland's typed constants (which aren't really constants at all) and gives a shortcut to constructing arrays and records at runtime. 

### Local variables with dynamic size (not yet implemented)

This should work but doesn't currently: 
    
    
    procedure p(n: integer);
    var
      v: 1..n;
    begin
      ...
    end;
    

### Acceptable Identifiers (not yet implemented)

  * An underscore (`_`) may neither begin or end an identifier.
  * Two underscores may not appear in succession, thus `Foo__bar` (_two_ underscores) is illegal.
  * Merely all-numeric [`label`s](<Label.md> "Label") are allowed and have a maximum length of four digits.



## External links

  * [ISO/IEC 10206:1991](<http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=18237>), [PDF (Archive.org copy)](<https://web.archive.org/web/20210820153846/www.pascal-central.com/docs/iso10206.pdf>): Extended Pascal standard

---

_Source: [https://wiki.freepascal.org/Extended_Pascal](https://web.archive.org/web/20250305223057/https://wiki.freepascal.org/Extended_Pascal)_
