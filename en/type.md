# Type

│ **English (en)** │

The [reserved word](<Reserved_word.md> "Reserved word") `type` is used to: 

  * start sections for user defined types, and
  * identify a new type instance when referring to another [data type](<Data_type.md> "Data type").



## Contents

  * 1 custom type definitions
  * 2 type aliases
  * 3 type clone
  * 4 see also



## custom type definitions

`type` starts a section, where the programmer may associate [identifiers](<Identifier.md> "Identifier") with new data types, especially structured data types such as [records](<Record.md> "Record"). 
    
    
    program typeDemo(input, output, stderr);
    
    type
    	atom = record
    		electrons: longword;
    		neutrons: longword;
    		protons: longword;
    	end;
    
    var
    	x: atom;
    
    begin
    	x.protons := 1; // H
    	x.neutrons := 1; // D
    	x.electrons := 1; // 0
    end.
    

## type aliases

In a `type` section aliases to already existing or previously defined data types can be declared. The following example utilizes [conditional compilation](<Conditional_compilation.md> "Conditional compilation") to alias the largest available unsigned [integer](<Integer.md> "Integer") type as `wholeNumber` (note there is already [`system.nativeUInt`](<https://www.freepascal.org/docs-html/rtl/system/nativeuint.html>) defined). 
    
    
    program typeAliasDemo(input, output, stderr);
    
    type
    	wholeNumber =
    		{$ifdef CPU64}
    			qword
    		{$else}
    			{$ifdef CPU32}
    				longword
    			{$else}
    				{$fatal whole number too small}
    			{$endif}
    		{$endif}
    		;
    
    begin
    end.
    

## type clone

In a `type` section a type identifier preceded by the word `type` actually clones the type, with its type information, but creating different types. Nontheless, these types are still _assigment compatible_ , which is a unique feature of the FPC (e. g. Delphi does not allow this assignment). 
    
    
    program typeCloneDemo(input, output, stderr);
    
    type
      wholeNumber = type qword;
    
    var
      A: qword;
      B: wholeNumber;
    
    begin
      writeLn('qword:       ', sysBackTraceStr(typeInfo(qword)));
      writeLn('wholeNumber: ', sysBackTraceStr(typeInfo(wholeNumber)));
    
      A := 3;
      B := A;
    end.
    

You want to do this, for instance in order to define a whole new set of [operators](<Operator.md> "Operator"). Otherwise the [operator definitions](<Operator_overloading.md> "Operator overloading") for the type it was cloned from would still apply. 

## see also

  * [§ “Type Aliases” in the Reference Guide](<https://www.freepascal.org/docs-html/ref/refse19.html>)

---

_Source: [https://wiki.freepascal.org/type](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/type)_
