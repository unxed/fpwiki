# Data type

│ **English (en)** │  **[русский (ru)](<../ru/Data_type.md>)** │

## Contents

  * 1 General
  * 2 Integer types
    * 2.1 Unsigned types
    * 2.2 Signed types
  * 3 Floating-point types
  * 4 Boolean types
  * 5 Enumeration types
  * 6 Char types
    * 6.1 Char types with single byte encoding
    * 6.2 Char types with multi byte encoding
  * 7 Variant types
  * 8 Constants
  * 9 Structural types
  * 10 Subrange types
  * 11 Pointer
  * 12 Classes and objects
  * 13 See also



# General

This page provides an assortment of data types in Free Pascal. 

A **data type** is a template for a [data field](<Data_field.md> "Data field"). 

The data type of a field defines how the compiler and processor interpret it's content. 

The visibility of a [data field](<Data_field.md> "Data field") depends on the location of it's declaration. 

# Integer types

## Unsigned types

[Data fields](<Data_field.md> "Data field") of unsigned integral types can only contain **positive** integral numbers. 

  * [UInt8](<UInt8.md> "UInt8") \- Range: (0 .. 255)
  * [Byte](<Byte.md> "Byte") \- Range: (0 .. 255)
  * [UInt16](<UInt16.md> "UInt16") \- Range: (0 .. 65535)
  * [Word](<Word.md> "Word") \- Range: (0 .. 65535)
  * [NativeUInt](<NativeUInt.md> "NativeUInt") \- Range: depends on the processor type.
  * [DWord](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") \- is equivalent to Longword.
  * [Cardinal](<Cardinal.md> "Cardinal") \- is equivalent to Longword.
  * [UInt32](<UInt32.md> "UInt32") \- Range: (0 .. 4294967295)
  * [Longword](<Longword.md> "Longword") \- Range: (0 .. 4294967295)
  * [UInt64](</index.php?title=UInt64&action=edit&redlink=1> "UInt64 \(page does not exist\)") \- Range: (0 .. 18446744073709551615)
  * [QWord](<QWord.md> "QWord") \- Range: (0 .. 18446744073709551615)



## Signed types

[Data fields](<Data_field.md> "Data field") of signed integral types can contain positive **and** negative integral numbers. 

  * [Int8](<Int8.md> "Int8") \- Range: (-128 .. 127)
  * [ShortInt](<Shortint.md> "Shortint") \- Range: (-128 .. 127)
  * [Int16](<Int16.md> "Int16") \- Range: (-32768 .. 32767)
  * [SmallInt](<Smallint.md> "Smallint") \- Range: (-32768 .. 32767)
  * [Integer](<Integer.md> "Integer") \- Range: is equivalent either to Smallint or Longint (for 16 respectively 32 bit processors).
  * [Int32](<Int32.md> "Int32") \- Range: (-2147483648 .. 2147483647)
  * [NativeInt](<NativeInt.md> "NativeInt") \- Range: depends on the processor type.
  * [LongInt](<Longint.md> "Longint") \- Range: (-2147483648 .. 2147483647)
  * [Int64](<Int64.md> "Int64") \- Range: (-9223372036854775808 .. 9223372036854775807)



# Floating-point types

[Data fields](<Data_field.md> "Data field") of a floating-point type can contain: 

  1. positive **and** negative integral numbers with possible round-off errors.
  2. positive **and** negative floating-point numbers.


  * [Single](<Single.md> "Single") \- Range: (1.5E-45 .. 3.4E38)
  * [Real](<Real.md> "Real") \- Range: depends on the platform.
  * [Real48](</index.php?title=Real48&action=edit&redlink=1> "Real48 \(page does not exist\)") \- Range: 2.9E-39 .. 1.7E38
  * [Double](<Double.md> "Double") \- Range: (5.0E-324 .. 1.7E308)
  * [Extended](<Extended.md> "Extended") \- Range: depends on the platform.
  * [Comp](<Comp.md> "Comp") \- Range: (-2E64+1 .. 2E63-1)
  * [Currency](<Currency.md> "Currency") \- Range: (-922337203685477.5808 .. 922337203685477.5807)



# Boolean types

[Data fields](<Data_field.md> "Data field") of boolean type contain truth values. 

  * [Boolean](<Boolean.md> "Boolean") \- Range: (True, False), 8 Bit
  * [ByteBool](</index.php?title=Bytebool&action=edit&redlink=1> "Bytebool \(page does not exist\)") \- Range: (True, False), 8 Bit
  * [WordBool](<Wordbool.md> "Wordbool") \- Range: (True, False), 16 Bit
  * [LongBool](<Longbool.md> "Longbool") \- Range: (True, False), 32 Bit



# Enumeration types

[Data fields](<Data_field.md> "Data field") of an enumeration type are "lists" (enumerations...) of integral unsigned constants. 

  * [Enum Type](<Enum_Type.md> "Enum Type") \- Range: (integral data types)



# Char types

## Char types with single byte encoding

  * [Char](<Char.md> "Char") \- Constant length: 1 byte, representation: 1 character.
  * [ShortString](<Shortstring.md> "Shortstring") \- Maxmimum length: 255 characters.
  * [String](<String.md> "String") \- Maxmimum length: Shortstring or Ansistring (depends in the used compiler switch).
  * [PChar](<PChar.md> "PChar") \- Pointer to a null-terminated string without length restriction.
  * [AnsiString](<Ansistring.md> "Ansistring") \- No length restriction.
  * [PAnsiChar](<Pansichar.md> "Pansichar") \- Pointer to a null-terminated string without length restriction.



See the overview of the different [character and string types](<Character_and_string_types.md> "Character and string types"). 

## Char types with multi byte encoding

(The encoding with 2 or 4 bytes is system dependent.  


  * [WideChar](<WideChar.md> "WideChar") \- Constant length: 2 or 4 bytes, representation: 1 character.
  * [WideString](<Widestring.md> "Widestring") \- No length restriction.
  * [PWideChar](<Pwidechar.md> "Pwidechar") \- Pointer to a null-terminated wide string without length restriction.
  * [UnicodeChar](<Unicodechar.md> "Unicodechar") \- Constant length: 2 or 4 bytes, representation: 1 character.
  * [UnicodeString](<Unicodestring.md> "Unicodestring") \- No length restriction.
  * [PUnicodeChar](<Punicodechar.md> "Punicodechar") \- Pointer to a null-terminated Unicode string without length restriction.



See the overview of the different [character and string types](<Character_and_string_types.md> "Character and string types"). 

# Variant types

  * [Variant](<Variant.md> "Variant")
  * [Olevariant](<Olevariant.md> "Olevariant")



# Constants

  * Untyped constants 
    * [Const](<Const.md> "Const") \- Only simple data types can be used.
  * Typed constants 
    * [Const](<Const.md> "Const") \- Simple data types as well as records and arrays can be used.
  * Resource Strings 
    * [Resourcestring](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)") \- Used for localisation (not available in all compiler modes).



# Structural types

  * [Array](<Array.md> "Array") \- The size of the array depends on the type and the number of elements it contains.
  * [Record](<Record.md> "Record") \- A combination of multiple data types.
  * [Set](<Set.md> "Set") \- A set of elements of an ordinal type; the size depends on number of elements it contains.



# Subrange types

  * [subrange types](<subrange_types.md> "subrange types") are a subset of a base type.



# Pointer

  * [Pointer](<Pointer.md> "Pointer") \- The size depends on the processor type.



# Classes and objects

  * [Object](<Object.md> "Object") \- Developed under Turbo Pascal 5.5 for DOS and the precursor of class.
  * [Class](<Class.md> "Class") \- Developed under Delphi 1.0 for Windows and the successor of object.



# See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")

---

_Source: [https://wiki.freepascal.org/Data_type](https://web.archive.org/web/20250422214548/https://wiki.freepascal.org/Data_type)_
