# Dynamic array

The dynamic array type is a very nice feature of FreePascal (and Delphi) syntax. It is very similar to the [array](<Array.md> "Array") type, but allows more flexibility for the programmer since the number of elements does not need to be known until program execution. 

The declaration part is just as simple as for the [array](<Array.md> "Array") type: 
    
    
    var
      ...
      MyVariable : array of type;
      ...

The number of elements can be set or modified whenever needed during the execution of the program by inserting the statement: 
    
    
    begin
      ...
      SetLength(MyVariable, ItsNewLength);
      ...
    end

You can put as many **SetLength** statements as you want in your program in order to expand or truncate an array but you must put at least one statement before you can use the variable for the first time. 

The individual elements can be accessed as follows: 
    
    
    ...
    SetLength(MyVariable,19);
    ...
    MyVariable[18] := 123;
    ...
    MyOtherVariable := MyVariable[0];
    ...
    WriteLn('MyVariable has ', Length(MyVariable), ' elements');  {should be 19}
    ...
    WriteLn('Its range is ', Low(MyVariable), ' to ', High(MyVariable)); {should be 0 to 18}
    ...

The index of a dynamic array is ZERO based, i.e. it must be within the range from 0 to (Length-1). It is NOT possible to change this to a ONE based system. 

Actually, dynamic arrays are pointers with automatic dereferencing. They are initialized to **nil** automatically. This means, that **MyVariable** is interpreted as a pointer variable when handed over to low level routines like **fillchar** , **sizeof** , etc. but it is automatically expanded to **MyVariable^** when indexing its elements (as in **MyVariable[2]**) or when handing it over to routines that expect array types. 

From a memory management view, dynamic array variables are simple pointers. **SetLength** allocates and frees memory on the heap as needed. When used in functions or procedures only the pointer is added to the stack. When the procedure exits, the dynamic array variable is removed and the memory is made available again. In fact, the memory management procedures are inserted in the executable program and the result is totally transparent to the programmer. 

Assigning **nil** to a dynamic array variable automatically frees the memory where the pointer pointed to. It's identical to **SetLength(MyVariable, 0)**. This can have a side effect, if the pointer value is not valid for some reason (i.e., if it was read from disk where it was stored from previous program runs). To init such an invalid pointer you have to use **FillChar(MyVariable,sizeof(MyVariable), #0)**. 

Although writing to elements of dynamic arrays does **not** create a new instance of the array (no copy-on-write as it exists for Ansistrings) using **SetLength** on such arrays **does** create a copy! So if 2 dynamic array variables point to the same array (one has been assigned to the other) they do not do so after using **SetLength** on one (or both) of them. After the SetLength() call the two variables are distinct arrays whose elements are independent from each other. 

## See also

  * [Free Pascal Reference guide: Dynamic arrays](<http://www.freepascal.org/docs-html/current/ref/refsu15.html#x39-520003.3.1>)
  * [Example: Multidimensional dynamic array](<Example__Multidimensional_dynamic_array.md> "Example: Multidimensional dynamic array")
  * [Static array](<Array.md> "Array")

---

_Source: [https://wiki.freepascal.org/DYNAMIC_ARRAY](https://web.archive.org/web/20160604020606/https://wiki.freepascal.org/DYNAMIC_ARRAY)_
