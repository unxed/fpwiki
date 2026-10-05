# Record

│ **[Deutsch (de)](</Record/de> "Record/de")** │  **English (en)** │  **[español (es)](</Record/es> "Record/es")** │  **[suomi (fi)](</Record/fi> "Record/fi")** │  **[français (fr)](</Record/fr> "Record/fr")** │  **[magyar (hu)](</Record/hu> "Record/hu")** │  **[polski (pl)](</Record/pl> "Record/pl")** │  **[português (pt)](</Record/pt> "Record/pt")** │  **[русский (ru)](<../ru/Record.md> "Record/ru")** │    
****

A `record` is a highly structured data [`type`](<Type.md> "Type") in [Pascal](<Pascal.md> "Pascal"). They are widely used in Pascal, to group data items together logically. 

While simple data structures such as [`array`s](<Array.md> "Array") or sets consist of elements all of the same type, a record can consist of a number of elements of different types, and can take on a huge complexity. Each separate part of a record is referred to as a field. 

The word `record` is a [reserved word](<Reserved_word.md> "Reserved word"). 

## Contents

  * 1 Declaration
    * 1.1 Fixed structure
    * 1.2 Variable structure
    * 1.3 Advanced record
  * 2 Addressing
    * 2.1 Fields
    * 2.2 Instances
  * 3 Constant record
  * 4 Records compared to other structured types
  * 5 See also



## Declaration

### Fixed structure
    
    
    type
    	TMember = record
    		firstname, surname : string;
    		address: array [1..3] of string;
    		phone: string;
    		birthdate: TDateTime;
    		paidCurrentSubscription: boolean
    	end;
    

### Variable structure

And even more complex structures are possible, e.g.: 
    
    
    type
    	TMaritalState = (unmarried, married, widowed, divorced);
    	TPerson = record
    		// CONSTANT PART
    		// of course records may be nested
    		name: record
    			first, middle, last: string;
    		end;
    		sex: (male, female);
    		// date of birth
    		dob: TDateTime;
    		// VARIABLE PART
    		case maritalState: TMaritalState of
    			unmarried: ( );
    			married, widowed: (marriageDate: TDateTime);
    			divorced: (marriageDateDivorced, divorceDate: TDateTime;
    				isFirstDivorce: boolean)			
    	end;
    

Note that fields of the variable part have to be in parentheses. You cannot use the same identifier multiple times, so a slightly different name has to be used for the `marriageDate` in case of _divorced_. 

The variable part shares the same memory. So `marriageDate` and `marriageDateDivorced` will have the same value regardless of `maritalState`. This behaviour is particularly practical as the following example shows: 
    
    
    type
      TSpecialWord = record
        case Byte of
          0: (Word_value: Word);                      // 7
          1: (Byte_low, Byte_high: Byte);             // 7, 0
          2: (Bits: bitpacked array [0..15] of 0..1); // 1, 1, 1, 0, 0, ...
      end;
    

This record has only a variable part and enables access to the value of the word, the individual bytes and even to the bits. An identifier is not necessarily required in the `case` clause, so this does not occupy any memory. The size of this record is two bytes. In the case of `Bits`, this is only possible if `bitpacked` is used. Note the order of the bytes, with the less significant byte (LSB) coming first. 

### Advanced record

An “advanced record” is a record with [methods](<Method.md> "Method"), [properties](</Property> "Property") and [management_operators](<management_operators.md> "management operators")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Remember to add a compiler directive `{$modeSwitch advancedRecords}` (after you have set the mode).

## Addressing

### Fields

Individual fields are accessed by placing a dot between the record name and the field name thus: 
    
    
    	a.firstname := 'George';
    	a.surname := 'Petersen';
    	a.phone := '789534';
    	a.paidCurrentSubscription := true;
    

Alternatively, the whole series of fields can be made available together using the [`with`-construct](<With.md> "With"): 
    
    
    with a do
    begin
    	firstname := 'George';
    	surname := 'Petersen';
    	phone := '789534';
    	paidCurrentSubscription := true
    end;
    

### Instances

A record is treated by the program as a single entity, and for example a whole record can be copied (provided the copy is of the same type) thus: 
    
    
    var
    	a, b: TMember;
    
    (* main program *)
    begin
    	// ... assign values to the fields in record a
    	b := a
    	// Now b holds a _copy_ of a.
    	// Do not get confused with references:
    	// a and b still point to _different_ _entities_ of TMember.
    end.
    

## Constant record
    
    
    type
    	// record definition
    	TSpecialDay = record
    		dayName: string;
    		month: integer;
    		day: integer;
    	end;
    
    const
    	// TSpecialDay constant
    	christmasDay: TSpecialDay = (
    		dayName: 'Christmas Day';
    		month: 12;
    		day: 25;
    	);
    
    // since FPC 3.2.0 you may define an constant array of records like this
    const
       SpecialDays: array of TSpecialDay = (
         ( dayName: 'Christmas Day' ; month: 12; day: 25),
         ( dayName: 'New Year''s Day'; month:  1; day:  1)
       );
    

## Records compared to other structured types

Feature  | Record | [Adv Record](<Record.md> "Record") | [Object](<Object.md> "Object") | [Class](<Class.md> "Class")  
---|---|---|---|---  
Encapsulation (combining data and methods + hiding visibility)  | No  | Yes  | Yes  | Yes   
[Inheritance](<Inherited.md> "Inherited") | No  | No  | Yes  | Yes   
Class constructor and destructor  | No  | Yes  | Yes  | Yes   
Polymorphism (virtual methods)  | No  | No  | Yes  | Yes   
Memory allocation  | Stack  | Stack  | Stack  | Heap (Only)   
  
    Setting fields to zero on allocation
| Managed Types only  | Managed Types only  | Managed Types only  | All fields   
  
    Default() function returns a constant with
| all fields zeros  | all fields zeros  | all fields zeros  | returns nil   
Operator overload (global)  | Yes  | Yes  | Yes  | Yes   
Operator overload (in type only)  | No  | Yes  | No  | No   
Type helpers  | No  | Yes  | No  | Yes   
Virtual constructors, class reference  | No  | No  | No  | Yes   
Variant part (case) as c/c++ union  | Yes  | Yes  | No  | No   
Bitpacked (really packing)  | Yes  | Yes  | No  | No   
  
Modified from <https://forum.lazarus.freepascal.org/index.php/topic,30686.30.html> (original author: ASerge). 

## See also

  * [Records](<Basic_Pascal_Tutorial/Chapter_5/Records.md> "Basic Pascal Tutorial/Chapter 5/Records"), tutorial that covers records
  * [`object`](<Object.md> "Object")
  * [`class`](<Class.md> "Class")



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  [`array`](<Array.md> "Array") [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") `record` [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Record](https://web.archive.org/web/20250421200425/https://wiki.freepascal.org/Record)_
