# Array

│ **English (en)** │

An **array** is a linear data structure concept that groups elements of the same type, stores them in contiguous and adjacent memory locations and provides random access to all of said elements (also known as components) by way of a linear index. 

The word `array` is a [reserved word](<Reserved_word.md> "Reserved word"). It always occurs in conjunction with the word [`of`](<Of.md> "Of"). 

## Contents

  * 1 Notion
  * 2 Usage
    * 2.1 Length
    * 2.2 Static arrays
    * 2.3 Initialization
    * 2.4 Addressing elements
  * 3 Dynamic arrays
  * 4 Application
  * 5 See also



## Notion

An `array` is a limited and arranged aggregation of elements, all having the same [data type](<Data_type.md> "Data type") which is called the “base type.” It has at least one discrete, bounded dimension and continuously enumerates all of its elements. Each element can be uniquely identified by one or more scalar values, called _indices_ , along those dimensions. 

A one-dimensional `array` resembles an **n-tuple** , as it is known in mathematics, but has the constraint of being homogenous (all elements must be of the same type). The range of all possible values such an `array` can acquire is the homogenous n-ary Cartesian product of the base type. 

A two-dimensional `array` resembles the mathematical concept of a **matrix** , except for the homogeneity restriction. 

## Usage

### Length

Originally, Pascal only provided arrays of fixed length ([Standard Pascal](<Standard_Pascal.md> "Standard Pascal")), meaning the number of elements an array consisted of had to be known at compile-time. Since this turned out to be a major constraint, and changes in computer hardware since then justified a step forward, variable-length arrays were introduced. 

[Extended Pascal](<Extended_Pascal.md> "Extended Pascal") defined the notion of “schemata” for this. [Delphi](<Delphi.md> "Delphi") introduced “dynamic arrays”. As of 2020, [FPC](<FPC.md> "FPC") only supports the latter regarding variable-length arrays, while support for “schemata” is planned. 

Depending on whether an array is intended of being capable of changing its size, its definition varies, but just marginally. For a one-dimensional static array, the type definition looks like this: 
    
    
    array[indexType] of baseType
    

A dynamic array type definition is simply relieved of its dimension specification: 
    
    
    array of baseType
    

### Static arrays

In static arrays, the ranges of all dimensions are known in advance. All dimension specifications have to be ordinal types. The following code shows some valid array definitions, all of them static. 
    
    
    program staticArrayDemo(input, output, stderr);
    
    type
    	// specifying ordinal types as index directly
    	
    	/// allows selection of a character
    	/// based on a Boolean value
    	characterChoice = array[boolean] of UCS4char;
    	
    	// enumerations
    	
    	/// enumerates Cartesian axes
    	spaceAxis = (xAxis, yAxis, zAxis);
    	/// a point in three-dimensional Euclidean space
    	locus = array[spaceAxis] of valReal;
    	/// a point in a two-dimensional Euclidean plane
    	point = array[xAxis..yAxis] of valReal;
    	
    	// integer subranges
    	
    	level = array[-24..24] of longint;
    	box = array[-1..1, -1..1, -1..1] of boolean;
    	transformationMatrix = array[0..1, 0..1] of valReal;
    begin
    end.
    

As all of an array’s elements have to be addressable, there exists a maximum limit of elements an array can hold. The [`sizeOf`](<SizeOf.md> "SizeOf") every array type has to be less than [`ptrInt`](<https://www.freepascal.org/docs-html/rtl/system/ptrint.html>)’s maximum value. 

### Initialization

It's possible to set the initial values of a static (and dynamic) array's elements when it is declared – 
    
    
    var
        SArray : array[0..2] of integer = (1,2,3);            // A static Array
        CArray : array[0..1] of TColor = (clRed, clBlue);     // A static Array
    

**See more:** [Topic: How to initialize the array (Free Pascal Lazarus Forum)](<https://forum.lazarus.freepascal.org/index.php/topic,52656.0.html>)

### Addressing elements

An array’s element is addressed by naming the array variable’s identifier, followed by a valid index value enclosed by square brackets. 
    
    
    program arrayAddressDemo(input, output, stderr);
    var
    	msg: array[0..2] of char;
    begin
    	msg[0] := 'H';
    	msg[1] := 'i';
    	msg[2] := '!';
    	writeLn(msg);
    end.
    

Multidimensional arrays’ elements can be addressed in two ways: either by [comma](<Comma.md> "Comma")-separated indices… 
    
    
    arrayVariable[firstDimensionIndex, secondDimensionIndex, thirdDimensionIndex]
    

…or by putting indices in dedicated square brackets. 
    
    
    arrayVariable[firstDimensionIndex][secondDimensionIndex][thirdDimensionIndex]
    

A third syntactically-valid option would be mixing both styles, however that is considered poor style, unless perhaps there is indication to group indices (e.g. `x`, `y` and `z` coordinates versus other indices) it is okay. Nonetheless, only the first mentioned notation is valid while defining array types. 

Note, it is very important to specify indices in the defined order, within each dimensions’ range. Consider the following program; it will compile, but fail during [run-time](<runtime.md> "runtime") due to `{$rangeChecks on}`: 
    
    
    program arrayAddressOrderDemo(input, output, stderr);
    {$rangeChecks on}
    var
    	i: integer;
    	f: array[0..1, 0..3] of boolean;
    begin
    	for i := 0 to 7 do
    	begin
    		f[0, i] := true;
    	end;
    end.
    

While the program would indeed iterate over every array’s elements, it doesn’t do so in the intended way, but rather exploits the fact that the array’s internal memory structure is just a continuous block of memory. This is bad style. A programmer working with a high-level language is not supposed to care about specific memory layouts. Cave: It is possible to tamper with other variables in this way. At any rate, a [run-time error](<runtime_error.md> "runtime error"), namely “RTE 216 general protection fault,” will occur if an attempt is made to access memory which is not within the purview of the process owner. 

When values contained in arrays are merely read (and thus the indices do not matter), a [`for … in` loop](<for-in_loop.md> "for-in loop") can be used. 

## Dynamic arrays

A [dynamic array](<Dynamic_array.md> "Dynamic array") is an approach for overcoming the limitation of knowing the sizes of all dimensions in advance. See its dedicated page for more details. 

## Application

See, for instance: 

  * [Array sort](<Array_sort.md> "Array sort")
  * [15-puzzle](<15-puzzle.md> "15-puzzle")’s and [Peg](<Peg_Solitaire_tutorial.md> "Peg Solitaire tutorial")’s game board states are stored as an array.



In the default [RTL](<RTL.md> "RTL")’s system unit, the function [`system.slice`](<https://www.freepascal.org/docs-html/rtl/system/slice.html>) returns the initial part of an array, similar to the Ruby notation `arrayVariable[0, n]`. Furthermore, there is [`system.arrayStringToPPchar`](<https://www.freepascal.org/docs-html/rtl/system/arraystringtoppchar.html>). Most [statistical routines of the RTL’s math unit](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics") accept arrays as parameters, as well as some other routines. 

## See also

  * [Type information](<Type_information.md> "Type information")
  * [Defensive programming techniques](<Defensive_programming_techniques.md> "Defensive programming techniques")
  * [Vectorization](<Vectorization.md> "Vectorization")
  * [Flexible Array Member](<Flexible_Array_Member.md> "Flexible Array Member")
  * Tutorial: [1-dimensional arrays](<Basic_Pascal_Tutorial/Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")
  * Tutorial: [Multidimensional arrays](<Basic_Pascal_Tutorial/Chapter_5/Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays")
  * Example: [Multidimensional dynamic array](<Example__Multidimensional_dynamic_array.md> "Example: Multidimensional dynamic array")
  * Article: [Why Pascal is not my favorite programming language § “The size of an array is part of its type”](<Why_Pascal_is_Not_My_Favorite_Programming_Language.md> "Why Pascal is Not My Favorite Programming Language")
  * [`matrix` unit](<https://www.freepascal.org/docs-html/rtl/matrix/index.html>)



  


navigation bar: data types  [simple data types](<simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean") [`byte`](<Byte.md> "Byte") [`cardinal`](<Cardinal.md> "Cardinal") [`char`](<Char.md> "Char") [`currency`](<Currency.md> "Currency") [`double`](<Double.md> "Double") [`dword`](</index.php?title=DWord&action=edit&redlink=1> "DWord \(page does not exist\)") [`extended`](<Extended.md> "Extended") [`int8`](<Int8.md> "Int8") [`int16`](<Int16.md> "Int16") [`int32`](<Int32.md> "Int32") [`int64`](<Int64.md> "Int64") [`integer`](<Integer.md> "Integer") [`longint`](<Longint.md> "Longint") [`real`](<Real.md> "Real") [`shortint`](<Shortint.md> "Shortint") [`single`](<Single.md> "Single") [`smallint`](<Smallint.md> "Smallint") [`pointer`](<Pointer.md> "Pointer") [`qword`](<QWord.md> "QWord") [`word`](<Word.md> "Word")  
---|---  
complex data types |  `array` [`class`](<Class.md> "Class") [`object`](<Object.md> "Object") [`record`](<Record.md> "Record") [`set`](<Set.md> "Set") [`string`](<String.md> "String") [`shortstring`](<Shortstring.md> "Shortstring")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/array](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/array)_
