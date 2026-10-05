# false and true

│ **English (en)** │    
****

The [constants](<Constant.md> "Constant") `false` and `true` are used to define the false and true conditions of a [`boolean`](<Boolean.md> "Boolean") [variable](<Variable.md> "Variable"). They are [manifest constants](</index.php?title=Manifest_constant&action=edit&redlink=1> "Manifest constant \(page does not exist\)") that are defined as part of the [standard data types](<Standard_type.md> "Standard type") the [compiler](<Compiler.md> "Compiler") initially knows about. 

These constant values must be predefined by the compiler as there is no way to define them in terms of anything else. They are defined via [`compiler/psystem.pas`](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/tags/release_3_2_0/compiler/psystem.pas?view=markup#l114>) as part of the [system unit](<System_unit.md> "System unit"). 

As of [FPC](<FPC.md> "FPC") 3.0.0 `false` and `true` are **no longer** [reserved words](<Reserved_words.md> "Reserved words"). Thus the following program is valid, compiles and is “usable”: 
    
    
    program falseAndTrue(input, output, stderr);
    
    const
    	true = 42;
    
    begin
    	writeLn(true);                 // prints 42
    	//writeLn(true and false);     // does not compile
    	writeLn(system.true and false) // prints FALSE
    end.
    

## Internal value
    
    
    program falseDemo(input, output, stderr);
    
    uses
    	typInfo;
    
    begin
    	writeLn(false);                            // prints FALSE
    	
    	// enumerative actions ------------------------------------------
    	writeLn(ord(false));                       // prints 0
    	writeLn(succ(false));                      // prints TRUE
    	// next two statements generate compile errors
            // "Error: range check error while evaluating constants (-1 must be between 0 and 1)"
    	writeLn(pred(false));
            // " Error: range check error while evaluating constants (2 must be between 0 and 1)"                      
    	writeLn(succ(succ(false)));                
    	
    	// data type ----------------------------------------------------
    	writeLn(sizeOf(false));                    // prints 1
    	writeLn(bitSizeOf(false));                 // prints 8
    	writeLn(PTypeInfo(typeInfo(false))^.kind); // prints tkBool
    	writeLn(PTypeInfo(typeInfo(false))^.name); // prints Boolean
    end.
    

When [typecasting](<Typecast.md> "Typecast") or interpreting any numeric value as a boolean value, it is important to know, that _any_ non-zero value means `true` whilst only `0` (zero) is `false`. 

Confer [ISO 7185 § “Required simple-types”](<http://pascal-central.com/iso7185.html#6.4.2.2%20Required%20simple-types>). 

## Two types of True

(according to developer PascalDragon) There are two types of True in Pascal: 

  * the True for **Boolean** , **Boolean8** , **Boolean16** , **Boolean32** and **Boolean64** has the value 1, anything else (except 0 for False) is undefined.
  * for **ByteBool** , **WordBool** , **LongBool** (the type for the Windows API's **BOOL**) and **QWordBool** any value that is not 0 is True, but if you assign True to such a type it will have the value "not 0" in the appropriate bit width.



## See also

  * [Boolean](<Boolean.md> "Boolean")

---

_Source: [https://wiki.freepascal.org/False](https://web.archive.org/web/20250323134151/https://wiki.freepascal.org/False)_
