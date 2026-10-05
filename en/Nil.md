# Nil

│ **English (en)** │  **[русский (ru)](<../ru/Nil.md>)** │

The [reserved word](<Reserved_word.md> "Reserved word") `nil` represents the special [value](<Constant.md> "Constant") of a pointer [variable](<Variable.md> "Variable") not pointing anywhere in particular. In [FPC](<FPC.md> "FPC") it is implemented as `pointer(0)` (the numeric value `0`), however the programmer is not supposed to use this fact. In other programming languages, e. g. in C, you write `null`. The terms “null pointer” and “nil pointer” are used interchangeably, even among [Pascal](<Standard_Pascal.md> "Standard Pascal") programmers. 

There are two popular explanations of `nil`’s etymology. One says, `nil` is short for the Latin word “nihil” meaning “nothing”. The other suggests, `NIL` is an English acronym standing for “not in list.” Maybe, since the German word „Null“ stands for the digit “zero”, and in order to avoid confusion, or to distinguish between the concept and value, the word `nil` was chosen. At any rate, this does not have any implications while programming. 

## Assignment compatibility

`nil` can be of course [assigned](<Becomes.md> "Becomes") to a [`pointer`](<Pointer.md> "Pointer") variable, but also to other types, which are in fact pointers, but their usage is more convenient. For instance [dynamic arrays](<Dynamic_array.md> "Dynamic array") or [classes](<Class.md> "Class"): 
    
    
    program nilDemo(input, output, stderr);
    var
    	loc: pointer;
    	chk: array of boolean;
    	msg: PChar;
    	prc: TProcedure;
    	obj: TObject;
    begin
    	// point to nothing
    	loc := nil;
    	// clears dynamic array
    	chk := nil;
    	// empty string
    	msg := nil;
    	// procedural variable not referencing any procedure
    	prc := nil;
    	// loses reference to object
    	obj := nil;
    end.
    

Note, assigning `nil` to a dynamic array is virtually equivalent to the [procedure](<Procedure.md> "Procedure") invocation `setLength(dynamicArrayVariable, 0)`. The values of the array are lost, if the reference count of `dynamicArrayVariable` has hit zero. However, there is no comparable mechanism for other types, e. g. assigning `nil` to a `class` or `pointer` variable _will not_ release (i. e. de-allocate) the memory that is possibly been occupied by the referenced structure. 

## Application

In Pascal you usually do not write expressions like `pointerVariable =: nil` but use more explanatory [identifiers](<Identifier.md> "Identifier"). The [function](<Function.md> "Function") [`system.assigned`](<Assigned.md> "Assigned") will be replaced by the exact same expression, but conceals the fact a variable is (_implemented_ as) a pointer. So its usage is optional. 

The [routine](<Routine.md> "Routine") [`SysUtils.FreeAndNil`](<FreeAndNil.md> "FreeAndNil") will call a class’s `free` routine and assign `nil` to the handed pointer (variable of type `class`). Although it is a good idea, to clear pointers which do not point to valid objects anymore, this can make debugging more difficult since there is no pointer available pointing to the address at which a certain object used to be. 

## See also

  * [`^`](<^.md> "^")
  * [`system.returnNilIfGrowHeapFails`](<https://www.freepascal.org/docs-html/rtl/system/returnnilifgrowheapfails.html>)
  * `{$objectChecks on}`
  * [nullable types](<Nullable_types.md> "Nullable types")

---

_Source: [https://wiki.freepascal.org/Nil](https://web.archive.org/web/20250324174256/https://wiki.freepascal.org/Nil)_
