# Basic Pascal Tutorial/Chapter 1/Constants

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Constants/bg> "Basic Pascal Tutorial/Chapter 1/Constants/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Constants/de> "Basic Pascal Tutorial/Chapter 1/Constants/de")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Constants/fr> "Basic Pascal Tutorial/Chapter 1/Constants/fr")** │  **[italiano (it)](</Basic_Pascal_Tutorial/Chapter_1/Constants/it> "Basic Pascal Tutorial/Chapter 1/Constants/it")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Constants/ja> "Basic Pascal Tutorial/Chapter 1/Constants/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Constants/ko> "Basic Pascal Tutorial/Chapter 1/Constants/ko")** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants/ru")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Constants/zh_CN> "Basic Pascal Tutorial/Chapter 1/Constants/zh CN")** │    
****

[ ◄ ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types")  
---|---|---  
  
1C - Constants (author: Tao Yue, state: changed) 

Constants are referenced by identifiers, and can be assigned one value at the beginning of the program. The value stored in a constant cannot be changed. 

Constants come in three flavors: Scalars, records, and arrays. A scalar constant is a single identifier which is assigned a single value. A record constant is a single identifier holding one or more separate values in a structured form. An array constant holds multiple values. They will be explained in greater detail in separate sections below. 

A constant, whether a scalar, a record field, or an array subscript, may be used in place of a literal of the same type, or may be used anywhere a variable may be used with two exceptions. They cannot be used as the target of an assignment statement, and they cannot be passed as a VAR parameter in a call to a procedure, function, method or property. 

## Contents

  * 1 Scalar constants
  * 2 Record constants
  * 3 Array constants
    * 3.1 One-dimensional arrays
      * 3.1.1 One-dimensional array of record
    * 3.2 Two-dimensional array constant
    * 3.3 Three-dimensional array constant



## Scalar constants

Scalar constants are defined in the Const (constant) section of the program: 
    
    
    const
      Identifier1 = value;
      Identifier2 = value;
      Identifier3 = value;
    

For example, let's define implicitly some constants of various data types: strings, characters, integers, reals, and booleans. These data types will be further explained in the next section. 
    
    
    const
      Name = 'Tao Yue';
      FirstLetter = 'a';
      Year = 1997;
      pi = 3.1415926535897932;
      UsingNCSAMosaic = TRUE;
    

Note that in Pascal, single characters are enclosed in single quotes, or apostrophes (')! This contrasts with newer languages which often use or allow double quotes or Heredoc notation. Standard Pascal does not use or allow double quotes to mark characters or strings. 

Constants are useful for defining a value which is used throughout your program but may change in the future. Instead of changing every instance of the value, you can change just the constant definition. 

**Typed constants** force a constant to be of a particular data type, by defining it explicitly. 

For example, 
    
    
    const
      a : real = 12;
    

would yield an identifier **a** which contains a **real** value **12.0** instead of the integer value **12**. 

## Record constants

Record constants are created by creating a record type with one or more fields, then creating a constant that references that record type, filling the field(s) with values. 

For record constants, the general format is 

    const 

    `identifier`: `record_type` = ( `field_values` );
    `identifier` …

Where 

  * `identifier` is the name of the record constant;
  * `record_type` is the name of the record type used to describe this record constant, and
  * `field_values` is a list consisting of a field name from the `record_type` referenced earlier, a colon, and the value to be assigned to that field, separated from the next field:value pair by a semicolon.



Lets try a simple example, a complex number constant. 
    
    
    type
        complex = record
                    R,I: real;
                  end;
    const
         Pi = 3.14159267;
         C1: Complex = (R:3; I:1783.5);
         C2: Complex = (R:9.6; I:1.62);
         C3: Complex = (R:17; I:115);
         C4: Complex = (R:1.9e56; I:72.43);
         C5: Complex = (R:22.1; I:Pi);
    

Note that fields must be initialized in the same order that they are declared (or the compiler will flag an error). If a record value is missing the compiler will warn of an uninitialized value. The values assigned can be literals or may be a scalar constant. 

For how to describe a constant array of record, see the section below. 

## Array constants

Array constants work much the same as scalar constants, except multiple values can be specified. All of the values must be the same type, whether it is a number (byte, word, integer. real, etc.) or character based (char, string, ansistring, etc.) 

### One-dimensional arrays

For one-dimensional array constants the general format is: 

    const 

    `identifier`: array[`low_value` .. `high_value`] of `type` = ( `values` );
    `identifier` …

Where 

  * `identifier` is the name of the array;
  * `low_value` is the lower bound of the array;
  * `high_value` is the upper bound of the array;
  * `type` is the type of value stored in the elements of the array (char, integer, real, string, etc.) and
  * `values` is a list of values with each item in the list separated from the next item by a comma.



Here's a simple one, the alphabet: 
    
    
    const
      Alphabet: array [1..26] of char =
           ('A','B','C','D','E','F','G','H','I',
            'J','K','L','M','N','O','P','Q','R',
            'S','T','U','V','W','X','Y','Z'   );
    

There are two rules to remember. As was said before, all the values given have to be of the same type, and you have to specify as many values as there are elements in the array. While the example given above is okay, there will be other parts of the program that reference this array, such as `for` loops. etc. Instead of using the literal 26 for the size of the array, let's use a scalar constant instead: 
    
    
    const
       LetterCount = 26;
       Alphabet: array [1 .. LetterCount] of char =
           ('A','B','C','D','E','F','G','H','I',
            'J','K','L','M','N','O','P','Q','R',
            'S','T','U','V','W','X','Y','Z');
    

Now, any place you need to reference the size of the array, you can just use `LetterCount`. 

Here's a more interesting example in which several data types are used. This example, which is probably from a calendar program, has all sorts of types: char, string, and integer. 
    
    
    const
      MonthStart = 0; // could be 1 and
      MonthEnd   = 11; // 12 if desired
      DayStart   = 0; // same,could be 1 and
      DayEnd     = 6;  // 7
      DayNameCh: array [DayStart .. DayEnd] of char =
          ('S','M','T','W','H','F','A');
      DayNameShort: array [DayStart .. DayEnd] of string =
          ('Sun','Mon','Tue','Wed','Thu', 'Fri','Sat');
      DayNameLong: array [DayStart .. DayEnd] of string =
          ('Sunday', 'Monday', 'Tuesday', 'Wednesday',
           'Thursday', 'Friday', 'Saturday');
      MonthNameLong: array [MonthStart .. MonthEnd] of string =
          ('January', 'February', 'March', 'April', 'May', 'June', 'July',
           'August', 'September', 'October', 'November', 'December');
      MonthDays: array [MonthStart .. MonthEnd] of integer =
          (31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);
    

Notice that for the items indicated as type `string`, while every value had to be a string, they were not required to all be the same size. 

#### One-dimensional array of record

An array constant may consist of records. To describe them, we use the parentheses ( ) around each record. Using the `complex` type defined in the section on record constants, we can create a constant array of `complex` records, as follows: 
    
    
    type
        complex = record
            R,I: real;
        end;
    const
         Pi = 3.14159267;
         C2: complex = ( R: 96; I:1.62);
         C_Low = 1; C_High = 5;
         C: array [C_low .. C_high] of complex = (
             (R:3; I:1783.5),
             (R:96; I:1.62),
             (R:17; I:115),
             (R:1.9e56; I:72.43),
             (R:102.1; I:Pi)
         );
    

As before, values need not be literals; they can be other scalar constants. Also, values must be given in the same order as defined in the record. 

Now for a more (pun unintentional) complex example, we use an enumerated type and a set to define a record constant array: 
    
    
    type
        Month = (January, February, March, April, May,
                 June, July, August, September,
                 October, November, December);
        MonthSet = set of Month;
        EachMonth = record
             Name, Short: string;
             Count: integer;
        end;
    
    const
         Months: array [January .. December] of EachMonth = (
           (Name: 'January';   Short: 'Jan'; Count: 31),
           (Name: 'February';  Short: 'Feb'; Count: 28),
           (Name: 'March';     Short: 'Mar'; Count: 31),
           (Name: 'April';     Short: 'Apr'; Count: 30),
           (Name: 'May';       Short: 'May'; Count: 31),
           (Name: 'June';      Short: 'Jun'; Count: 30),
           (Name: 'July';      Short: 'Jul'; Count: 31),
           (Name: 'August';    Short: 'Aug'; Count: 31),
           (Name: 'September'; Short: 'Sep'; Count: 30),
           (Name: 'October';   Short: 'Oct'; Count: 31),
           (Name: 'November';  Short: 'Nov'; Count: 30),
           (Name: 'December';  Short: 'Dec'; Count: 31)
         );
    

Take notice of how the items are delimited (separated). Elements in the record are delimited by semi-colons, while commas delimit elements of the array. 

### Two-dimensional array constant

As used in the example of ` array of record`, two-dimensional arrays are described by encapsulating each of the second dimension in parentheses. To make it easier to understand, the entries in the array have been identified by number, e.g. 13 is the first row, third column, while 21 is the second row, first column. Here is the format: 
    
    
    const
       DemoArray: array [1..2, 1..3] of integer = (
               (11,12,13),
               (21,22,23)
       );
    

### Three-dimensional array constant

Three-dimensional arrays are rare, and you might never use one, but you should know how to construct a three-dimensional constant array if you need it. Again, each level of array is encapsulated by parentheses. Here is a three-dimensional constant array, and the values represent their position in the array. 
    
    
    const
       Demo3: array [1..2, 1..3, 1..4] of integer =
       (
          (
           (111,112,113,114),
           (121,122,123,124),
           (131,132,133,134)
          ),
          (
           (211,212,213,214),
           (221,222,223,224),
           (231,232,233,234)
          )
       );
    

  


[ ◄ ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Constants](https://web.archive.org/web/20250331233553/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Constants)_
