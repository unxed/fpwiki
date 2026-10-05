# Enum type

│ **English (en)** │

An **enumeration** is a custom [data type](<Data_type.md> "Data type") in [Pascal](<Pascal.md> "Pascal"). It is a discrete value that can be referred to by a unique [identifier](<Identifier.md> "Identifier"). 

## Contents

  * 1 Definition
    * 1.1 Basic
    * 1.2 Indices
      * 1.2.1 Override
      * 1.2.2 Properties
    * 1.3 Prefixing
  * 2 Size
  * 3 Application
    * 3.1 Out-of-the-box functionality
    * 3.2 Masks
  * 4 Comparative remarks
  * 5 See also



## Definition

### Basic

An enumeration is defined by surrounding a non-empty [comma](<Comma.md> "Comma")-separated list of identifiers that were not yet defined in the current [scope](<Scope.md> "Scope") with parentheses: 
    
    
    program enumerationDemo(input, output, stdErr);
    type
    	fruit = (apple, banana, citrus);
    

Thus, a variable of the [type](<Type.md> "Type") `fruit` may assume the values `apple`, `banana`, or `citrus`. 
    
    
    var
    	snack: fruit;
    begin
    	snack := apple;
    end.
    

### Indices

Every member of an enumerated type has an ordinal value which can be obtained using the [standard function `ord`](<Ord.md> "Ord"). The first member of any enumeration has the ordinal value zero, and every following member is enumerated with consecutive numbers. 

#### Override

In some cases, however, it would be quite useful if the ordinal values were different. The [FPC](<FPC.md> "FPC") allows [Delphi](<Delphi.md> "Delphi")-style explicit definition of indices by following member identifiers with an [equal sign](<=.md> "=") and an [integer](<Integer.md> "Integer") value: 
    
    
    type
    	sign = (negative = -1, none = 0, positive = 1);
    

It is not necessary to define _all_ members with an explicit ordinal value, this example _just happens_ to define all of them. If the ordinal value is unspecified, the automatic enumeration mechanism applies again. 

#### Properties

  * An enumeration data type definition is _ascending_ if the difference in ordinal value to every right-hand member is ≥ 1. Currently, non-ascending definitions merely [emit a note](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_2_2/compiler/ptype.pas#L1700-1706>). A `set` literal using two enumeration values having the _same_ ordinal value, albeit represented by _different_ symbols, will not be accepted.
  * An enumeration data type is _contiguous_ if the ordinal value of every (named) member has an absolute difference of _one_ to its neighbors. Using a `set` constructor on a non-contiguous enumeration data type may produce unexpected results.
  * An enumeration data type is _normal_ if it is ascending and contiguous and the first member has an ordinal value of (strictly speaking) _zero_ , or, in the broad sense (as implemented by the FPC), ≤ 0. The compiler intrinsic `typeInfo` is _only_ available for _those_ enumeration data types.



### Prefixing

FPC prefixes all members of enumeration type definitions in the `type` section with their corresponding type name. By default, the short name _without_ a prefix is inserted into the symbol table as an _alias_ in order to meet Pascal standards. 

[Since version 2.6.0 FPC](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") supports disabling member identifiers from being aliased by using the `{$scopedEnums on}` [local compiler directive](<local_compiler_directives.md> "local compiler directives"), thus requiring users to always specify the type name as a prefix: 
    
    
    program scopedEnumerationDemo(input, output, stdErr);
    type
    	{$scopedEnums on}
    	logLevel = (error, warning, info, debug);
    var
    	verbosity: logLevel;
    begin
    	verbosity := logLevel.debug;
    end.
    

This setting only affects definition of _new_ enumeration data types. If the compiler switch `{$scopedEnums …}` was _on_ during definition of a enumeration type, you have to precede any values of that enumeration type with the data type’s name and a dot (in the example above this means `logLevel.`). 

This is useful if names are likely to clash with other identifiers, for example [`sysUtils.tUseBoolStrs`](<https://www.freepascal.org/docs-html/rtl/sysutils/tuseboolstrs.html>) is defined in that way (with `{$scopedEnums on}`), or if isolate identifiers would be meaningless or confusing like in [`objPas.tEndian`](<https://www.freepascal.org/docs-html/rtl/objpas/tendian.html>). Usage introduces some incompatibility to other compilers though. 

## Size

The size an enumeration type occupies depends on 

  1. the _minimum enumeration size_ compiler configuration, and
  2. the range, that is `ord(high(enumeration)) - ord(low(enumeration))`.



The local compiler directive `{$minEnumSize n}` determines the minimum size in [Bytes](<Byte.md> "Byte") of an enumeration data type. `{$minEnumSize n}` is (for Delphi compatibility) an alias for `{$packEnum n}`/`{$Z}`. 

  * `{$minEnumSize 1}`, `{$packEnum 1}`, `{$Z1}` or `{$Z‑}` instructs the compiler to use just one Byte (at least) for the enumeration type in question.
  * `{$minEnumSize 2}`, `{$packEnum 2}` and `{$Z2}`: 2 Bytes minimum.
  * `{$minEnumSize 4}`, `{$packEnum 4}`, `{$Z4}` or `{$Z+}`: use at least 4 Bytes.



Furthermore, `{$minEnumSize normal}`, `{$minEnumSize default}` and `{$minEnumSize 0}` restores the default value. In [`{$mode TP}`](<Mode_TP.md> "Mode TP") the default value is `1` whereas in all other modes it is `4`. Such a “large” minimum size aids quick read/write access. 

It stands to reason that an enumeration data type has to occupy at least as much space as all discrete values require to be stored. When there are 28 (256) or fewer elements to be stored, the compiler may store a variable of this kind as a single Byte. A range of 216 elements will be stored in two Bytes if a single Byte is insufficient. More than 216 members will be stored in four Bytes. These rules may, of course, be superseded by the previously mentioned `{$minEnumSize}` setting. 

## Application

### Out-of-the-box functionality

  * The pre-defined data type [`Boolean`](<Boolean.md> "Boolean") is an enumeration data type.
  * The functions `low` and `high` will give the lowest or highest available _named_ item of an enumeration data type.
  * The standard functions `pred` and `succ` return the predecessor and successor of a value. In non-Delphi modes this functionality is disabled if the enumeration data type is non-contiguous. Note that range checks may trigger [run-time errors](<runtime_error.md> "runtime error").



### Masks

Enumeration data types are particularly useful for implementing “masks”: By defining a [`set of …`](<Set.md> "Set") you have a neat data type fulfilling certain nice properties. 

## Comparative remarks

In some programming languages enumeration type definitions become “synonyms” to certain integer values. In Pascal, however, this is not the case. You may not mix a symbol referring to a member of an enumeration with other integer values. You will have to utilize `ord` or an [explicit typecast](<Typecast.md> "Typecast") to the enumeration data type. 

## See also

  * [Enumerated types](<Basic_Pascal_Tutorial/Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") in the [Basic Pascal Tutorial](<Basic_Pascal_Tutorial.md> "Basic Pascal Tutorial") series

---

_Source: [https://wiki.freepascal.org/Enum_type](https://web.archive.org/web/20250512092913/https://wiki.freepascal.org/Enum_type)_
