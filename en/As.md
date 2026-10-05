# As

│ **[Deutsch (de)](</As/de> "As/de")** │  **English (en)** │  **[español (es)](</As/es> "As/es")** │  **[suomi (fi)](</As/fi> "As/fi")** │  **[français (fr)](</As/fr> "As/fr")** │    
****

The [operator](<Operator.md> "Operator") `as` performs a conditional [typecast](<Typecast.md> "Typecast"). The word `as` is a [reserved word](<Reserved_word.md> "Reserved word") in [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") and [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC"). 

## operation

`as` requires a [`class`](<Class.md> "Class") or (COM) interface as the first argument and a class or interface reference as the second. The expression `child as super` is equivalent to the expression and statement: 
    
    
    	super(child)
    	if not assigned(child) and_then not child is super then
    	begin
    		raise exception.create(sErrInvalidTypecast);
    	end;
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Typecasting a [`nil` pointer](<Nil.md> "Nil") will not raise an exception.

However, trying to de-reference `nil` by attempting to access an attribute or method _will_ cause a [RTE](<runtime_error.md> "runtime error"). 

## application

`as` ensures a typecast is legit. 

`as` is one of the operators that can not be [overloaded](<Operator_overloading.md> "Operator overloading"). 

## see also

  * [`is`](<Is.md> "Is")

---

_Source: [https://wiki.freepascal.org/As](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/As)_
