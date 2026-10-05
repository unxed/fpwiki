# Dynamic array

│ **English (en)** │  **[español (es)](</Dynamic_array/es> "Dynamic array/es")** │  **[suomi (fi)](</Dynamic_array/fi> "Dynamic array/fi")** │  **[français (fr)](</Dynamic_array/fr> "Dynamic array/fr")** │  **[日本語 (ja)](</Dynamic_array/ja> "Dynamic array/ja")** │  **[русский (ru)](<../ru/Dynamic_array.md> "Dynamic array/ru")** │    
****

A **dynamic array** is an [array](<Array.md> "Array") whose dimensions are not known at [compile-time](<Compile_time.md> "Compile time"). The dynamic array type is not the only type providing variable-length arrays, but as of 2022 it is the only one [FPC](<FPC.md> "FPC") supports. 

## Contents

  * 1 usage
    * 1.1 concept
    * 1.2 definition
    * 1.3 sizing
    * 1.4 initializing
      * 1.4.1 constructor-like syntax
      * 1.4.2 Non-Constructor like syntax
    * 1.5 dynamic array literals
    * 1.6 Insert, Delete
    * 1.7 handling
  * 2 application
  * 3 see also



## usage

### concept

A dynamic array’s definition will only allocate space for a [pointer](<Pointer.md> "Pointer"). During [runtime](<runtime.md> "runtime"), various routines will ensure convenient usage but, most importantly, the syntax of accessing an array’s elements by placing indices in square brackets is supported by the [compiler](<Compiler.md> "Compiler"), implemented as automatic de-referencing of the pointer. 

Dynamic arrays’ indices are always non-negative integers starting at zero for the first element. It is not possible to use an enumerative type or any other ordinal type as an index. The first element is always specified by an index of `0` – this cannot be changed. 

### definition

A one-dimensional dynamic array is defined like this: 
    
    
    array of char
    

Note how no dimensions’ size is specified. This is what distinguishes the definition from a normal array. 

In order to define a multidimensional array, an array itself is specified as the base type, thus creating an “array of arrays”: 
    
    
    array of array of longInt
    

### sizing

The compiler [procedure](<Procedure.md> "Procedure") [`setLength`](<https://www.freepascal.org/docs-html/rtl/system/setlength.html>) will change a dynamic array’s length, provided there is enough memory. 
    
    
    program setLengthDemo(input, output, stdErr);
    var
    	sieve: array of longWord;
    begin
    	setLength(sieve, 1337);
    end.
    

The procedure allocates memory for as many records of the base type as specified, plus some management data. It then [_copies_](<https://www.freepascal.org/docs-html/rtl/system/copyarray.html>) all elements of the old incarnation to the new one. New fields that did not exist before are initialized with the [`default` intrinsic](<Default.md> "Default"). 

It is important to remember that, since all elements are copied during `setLength`, it can become extremely inefficient to use it with large arrays, or inside inner loops. 

Multidimensional arrays can be resized with `setLength`, too, by specifying multiple sizes in the call, like this: 
    
    
    program multidimensionalSetLengthDemo(input, output, stdErr);
    var
    	samples: array of array of smallInt;
    begin
    	setLength(samples, 12, 64);
    end.
    

In this example, valid indices for `samples`’ first dimension are in the range `0..11`, while valid indices for its second dimension are within `0..63`. 

One quite useful fact is that, because multidimensional dynamic arrays are always “arrays of arrays”, the limitation that all dimensions must be of the same size does not apply to them. Different dimensions are implemented as arrays, and can each have their own size. For instance, 
    
    
    program binomialPotence(input, output, stdErr);
    var
    	pascalsTriangle: array of array of longWord;
    	exponent: longInt;
    	factor: longInt;
    begin
    	setLength(pascalsTriangle, 20);
    	
    	setLength(pascalsTriangle[0], 1);
    	pascalsTriangle[0][0] := 1;
    	
    	setLength(pascalsTriangle[1], 2);
    	pascalsTriangle[1][0] := 1;
    	pascalsTriangle[1][1] := 1;
    	
    	// construct values by simple addition
    	for exponent := 2 to high(pascalsTriangle) do
    	begin
    		setLength(pascalsTriangle[exponent], exponent + 1);
    		pascalsTriangle[exponent][0] := 1;
    		pascalsTriangle[exponent][exponent] := 1;
    		for factor := 1 to exponent - 1 do
    		begin
    			pascalsTriangle[exponent][factor] :=
    				pascalsTriangle[exponent - 1][factor - 1] +
    				pascalsTriangle[exponent - 1][factor];
    		end;
    	end;
    	
    	// ...
    

### initializing

#### [`constructor`](<Constructor.md> "Constructor")-like syntax

[Since FPC 3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") dynamic array types that are not anonymous are automatically equipped with a “constructor” as it might be familiar from object-oriented programming. This lets you unite `setLength` calls and a series of assignments in one statement: 
    
    
    program dynamicArrayCreateDemo(input, output, stdErr);
    type
    	truths = array of boolean;
    var
    	l: truths;
    begin
    	l := truths.create(false, true, true, false, true, false, false);
    end.
    

Of course you can nest arrays as well: 
    
    
    program nestedDynamicArrayCreateDemo(input, output, stdErr);
    type
    	truths = array of boolean;
    	pattern = array of truths;
    var
    	p: pattern;
    begin
    	p := pattern.create(
    			truths.create(false, false),
    			truths.create(true, false),
    			truths.create(true, false, false),
    			truths.create(true, true, false)
    		);
    end.
    

#### Non-Constructor like syntax

As announced <https://lists.freepascal.org/pipermail/fpc-pascal/2018-May/053892.html> its now possible to initialize dynamic array constants and variables directly - 
    
    
    const
       Test1: array of LongInt = (1, 2, 3);
    
    var
        DArray : array of string  = ('one', 'two', 'three');  // A dynamic Array
        IArray : array of integer = (1,2,3);                  // A dynamic array
    

This is available in at least FPC 3.2.2. A very minor note, the above will not work with the predefined FPC colors such as colRed and colBlue because the TFCColors are Typed Constants. I bet you needed to know that ! 

### dynamic array literals

[Since FPC 3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") it is possible to specify dynamic array literals. This works for anonymous data types as well. 
    
    
    program dynamicArrayLiteralDemo(input, output, stdErr);
    
    procedure printArray(const list: array of integer);
    var
    	i: ALUSInt;
    begin
    	for i := low(list) to high(list) do
    	begin
    		writeLn(list[i]);
    	end;
    end;
    
    begin
    	printArray([123, 456, 789]);
    end.
    

Note that the data type of the formal parameter `list` does not have a name, yet we are able to specify a dynamic `array of integer` literal as an actual parameter. 

### Insert, Delete

Dynamic Arrays have their content managed with the Insert and Delete intrinsics. 
    
    
    var
        IArray  : array of integer;                    // A dynamic array
        i : integer;
    begin
        iArray := [7];                                 // A very small array
        iArray := [7, 8, 9];                           // A bit bigger
        for i in IArray do
            writeln(i, ' ');                           // prints "7, 8, 9"
        writeln('');
        for i := 0 to 3 do
            insert(i, IArray, 0);                      // four more elements, at the start of array
        for i in IArray do
            write(i, ' ');                             // prints "3 2 1 0 7 8 9"
        delete(IArray, 0, 6);                          // back to a one element array, just "9" 
    end;
    

Used this way, the compiler generates code to increase and decrease the memory allocated for the array, while this is convenient, it may not be best for large data sets or the situations that need the fastest response. 

### handling

Keep in mind dynamic arrays are pointers. Assigning dynamic array variables to each other does not copy any payload, but just the address. This differs from static arrays’ behavior. 

If you want to duplicate data you have to use [`system.copy`](<https://www.freepascal.org/docs-html/rtl/system/copy.html>). 
    
    
    program dynamicArrayCopyDemo(input, output, stdErr);
    
    var
    	foo, bar: array of char;
    
    procedure printArrays;
    begin
    	writeLn('foo[0] = ', foo[0], '; bar[0] = ', bar[0]);
    end;
    
    begin
    	setLength(foo, 1);
    	foo[0] := 'X';
    	// copies _reference_
    	bar := foo;
    	write('     initial values: ');
    	printArrays;
    	
    	// change content _via_ _second_ reference
    	bar[0] := 'O';
    	write('changed via 2nd ref: ');
    	printArrays;
    	
    	// copy content
    	bar := copy(foo, 0, length(foo));
    	bar[0] := 'X';
    	write(' copied and changed: ');
    	printArrays;
    end.
    

Only by using `copy` both arrays can be modified independently. 

As stated above, `setLength` _copies_ data. This procedure can be used to make data unique again, despite the fact only the references, the pointers (so just addresses) have been copied (`bar := foo`). The highlighted line in the example above could be replaced by `setLength(bar, length(bar))`, though the `copy` function is preferred as it is more explanatory. 

Dynamic arrays are reference counted. Calling `setLength(myDynamicArrayVariable, 0)` virtually does [`myDynamicArrayVariable := nil`](<Nil.md> "Nil") and decreases the reference count. Only when the reference count hits zero, the memory block is released. 
    
    
    program dynamicArrayNilDemo(input, output, stdErr);
    var
    	foo, bar: array of char;
    begin
    	setLength(foo, 1);
    	foo[0] := 'X';
    	// copy _reference_, increase reference count
    	bar := foo;
    	// foo becomes nil, reference count is decreased
    	setLength(foo, 0);
    	writeLn('length(foo) = ', length(foo),
    		'; length(bar) = ', length(bar));
    	
    	// decrease reference count another time
    	bar := nil;
    	writeLn('length(foo) = ', length(foo),
    		'; length(bar) = ', length(bar));
    end.
    

Nonetheless, dynamic arrays are finalized automatically. It is not necessary to manually `setLength(…, 0)` on all your references when the program comes to end, or when leaving a scope in general. 

Without `{$rangeChecks on}` it is possible to reach beyond an array’s limits. That means when iterating over dynamic arrays, it is impossible to work without [`low`](<https://www.freepascal.org/docs-html/rtl/system/low.html>) and [`high`](<https://www.freepascal.org/docs-html/rtl/system/high.html>) to determine valid indices (the former being optional, since dynamic arrays always start at zero). Alternatively, [`for … in` loops](<for-in_loop.md> "for-in loop") can be used, if no index is required. 

Remember, [`sizeOf`](<SizeOf.md> "SizeOf") of a dynamic array evaluates to the size of a pointer. 

## application

  * there is [`system.tBoundArray`](<https://www.freepascal.org/docs-html/rtl/system/tboundarray.html>) and [`objPas.tBoundArray`](<https://www.freepascal.org/docs-html/rtl/objpas/tboundarray.html>)



## see also

  * [`string`](<String.md> "String")
  * example: [multidimensional dynamic array](<Example__Multidimensional_dynamic_array.md> "Example: Multidimensional dynamic array")
  * [Free Pascal Reference guide: “Dynamic arrays”](<https://www.freepascal.org/docs-html/ref/refsu14.html#x38-520003.3.1>)

---

_Source: [https://wiki.freepascal.org/Dynamic_array](https://web.archive.org/web/20250421032157/https://wiki.freepascal.org/Dynamic_array)_
