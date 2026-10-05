# $extendedSyntax

│ **English (en)** │

  
****The[global compiler directive](<global_compiler_directives.md> "global compiler directives") `{$extendedSyntax on}` turns on additional syntax. The [FPC](<FPC.md> "FPC") has this by default _on_. The short notation is `{$X+}`/`{$X‑}`. 

## Contents

  * 1 Affected syntax
  * 2 Notes
  * 3 Comparative remarks
  * 4 See also



## Affected syntax

  * [Functions](<Function.md> "Function") can be called as if they were [procedures](<Procedure.md> "Procedure"). The function result is discarded. This is potentially harmful if, for example, the function allocated new memory space and returned a pointer to it. Nonetheless, _managed data types_ are insusceptible to leakage. Implementing a [management operator](<management_operators.md> "management operators") can turn any `record` into a managed data type.
  * Integer _arithmetic_ expressions are allowed on [pointers](<Pointer.md> "Pointer"). The directive [`{$pointerMath}`](</index.php?title=$pointerMath&action=edit&redlink=1> "$pointerMath \(page does not exist\)") had to be _on_ for that _during_ the respective pointer type’s definition.
  * Pointers become _ordered_ and can be compared using [`<`](<Less_than.md> "Less than"),[`>`](<Greater_than.md> "Greater than"), `<=` and `>=`. _Typed_ pointers have to correspond to each other. The [`=`](<Equal.md> "Equal") and [`<>`](<Not_equal.md> "Not equal") comparisons work _regardless_ of the `{$extendedSyntax}` state.



## Notes

  * If you have `{$extendedSyntax off}`, you can still do pointer arithmetic with routines from other units if they have been compiled with `{$extendedSyntax on}`, for example [`inc` and `dec`](<Inc_and_Dec.md> "Inc and Dec"):
        
        program pointerMathDemo(input, output, stdErr);
        {$extendedSyntax off}
        var
        	p: pChar;
        begin
        	p := nil;
        	inc(p, 42); { no problem }
        end.
        

Similarly, unusual comparison operations may be accessible with foreign routines.
  * Built-in functions can never be called as if they were procedures
        
        program discardFunctionResultDemo;
        {$extendedSyntax on}
        begin
        	pi; { discardFunctionResultDemo.pas(4,4) Error: Illegal expression }
        end.
        

even though a custom [`pi` `function`](<https://www.freepascal.org/docs-html/rtl/system/pi.html>) doing exactly the same would be acceptable.
  * Although with `{$extendedSyntax on}` pointers become ordered, they do _not_ become _ordinal data types_ ; the standard functions [`ord`](<Ord.md> "Ord"), `succ` and `pred` are still not applicable on pointers.



## Comparative remarks

  * [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") does not define any of those “extensions”.



## See also

  * [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")

---

_Source: [https://wiki.freepascal.org/$extendedSyntax](https://web.archive.org/web/20250317120322/https://wiki.freepascal.org/$extendedSyntax)_
