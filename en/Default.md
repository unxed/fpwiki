# Default

**`Default`** is 

  * a compiler intrinsic [`function`](<Function.md> "Function") returning a zeroed out value, or
  * a [keyword](<Keyword.md> "Keyword") marking a [`class`](<Class.md> "Class")’s [`array`](<Array.md> "Array") [property](</Property> "Property") as implicitly accessible.



This article describes the compiler intrinsic. See other articles for the notion in OOP. 

## Contents

  * 1 zero-idiom
    * 1.1 advantages
    * 1.2 caveats
  * 2 see also



## zero-idiom

[Since FPC 3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") `default(dataType)` returns a zero value for the specified [`dataType`](<Data_type.md> "Data type"). 
    
    
    program defaults(input, output, stdErr);
    var
    	i: integer;
    	s: string;
    	r: tOpaqueData;
    begin
    	i := default(integer);     { assigns `0` }
    	s := default(string);      { assigns `nil` or `''` (empty string) }
    	r := default(tOpaqueData); { assigns `tOpaqueData[]` }
    end.
    

### advantages

  * The most important “advantage” is that `default` can be used in `generic` definitions with template parameters.
        
        program defaultDemo(input, output, stdErr);
        {$mode objFPC}
        { --- generic thing -------------------------------------- }
        type
        	generic thing<storageType> = object
        			data: storageType;
        			constructor init;
        		end;
        constructor thing.init;
        begin
        	data := default(storageType);
        end;
        
        { === MAIN =============================================== }
        type
        	arr = array[1..10] of integer;
        var
        	x: specialize thing<tBoundArray>;
        	y: specialize thing<arr>;
        begin
        end.
        

In the context of the definition of the `generic` data type `thing` it is not yet known what `storageType` will be. Therefore, we could not possibly write a literal value such as `0` or [`nil`](<Nil.md> "Nil"). However, we _can_ use `default` to overcome this hurdle. Upon specialization the correct zero value will be inserted.
  * Furthermore, the compiler can choose a _faster_ implementation than, for instance, a respective `fillChar(myVariable, sizeOf(myVariable), chr(0))`.



### caveats

  * Despite its name, `default` is really just a synonym for zero. It _can_ be used to [assign](<Becomes.md> "Becomes") values _out of range_ :
        
        program faultyDefault(input, output, stdErr);
        {$rangeChecks on}
        type
        	typeWithoutZeroValue = -1337..-42;
        var
        	i: typeWithoutZeroValue;
        begin
        	i := -1024; { ✔ OK }
        	{i := 0; ✘ not OK }
        	i := default(typeWithoutZeroValue); { OK again }
        	writeLn(i);
        end.
        

See [FPC issue 34972](<https://gitlab.com/freepascal.org/fpc/source/-/issues/34972>).
  * `Default` cannot be applied on [`file`](</File> "File") or [`text`](<Text.md> "Text") data types and structured data types containing such.



## see also

  * [management operators](<management_operators.md> "management operators")

---

_Source: [https://wiki.freepascal.org/Default](https://web.archive.org/web/20220809173259/https://wiki.freepascal.org/Default)_
