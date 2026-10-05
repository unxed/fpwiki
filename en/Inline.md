# Inline

│ **English (en)** │

The [modifier](<modifier.md> "modifier") `inline` requests the [FPC](<FPC.md> "FPC") to consider copying the definition of a [routine](<Routine.md> "Routine") to the call site (“inlining”). The modifier `noinline` prevents the FPC from ever inlining a routine, even automatically (since [SVN revision 41198](<https://gitlab.com/freepascal.org/fpc/source/commit/503ea604f33b5a7dd72d7a6417f9a38774f19263>), as of 2022 only available in Trunk). 

## Contents

  * 1 use
  * 2 implications
    * 2.1 advantages
    * 2.2 disadvantages
  * 3 caveats
  * 4 application
  * 5 see also



## use

The use of `inline` routines is switched off by default. You can enable it with the `‑Si` compiler switch or the `{$inline on}` [local compiler directive](<local_compiler_directives.md> "local compiler directives"). The `inline` directive is placed after a routine’s signature at its defining point. Example: 
    
    
    function cube(const x: ALUSInt): ALUSInt; inline;
    begin
    	cube := sqr(x) * x;
    end;
    

## implications

Inlining means that a routine’s implementation exists at _multiple_ places in the final [executable file](<Executable_program.md> "Executable program"). There is no single address the program jumps to, but every time you invoke that routine there is dedicated copy in the source code. 

### advantages

  * Avoid call overhead for frequently invoked routines. This _could_ increase the speed of the program.
  * Elimination of an extra level of indirection in the case of [parameters passed by reference](<Variable_parameter.md> "Variable parameter").



### disadvantages

  * More difficult to debug: There is no extra frame on the stack indicating the subroutine.
  * Inlining requires space. You will necessarily have numerous copies of the same code at many places.
  * It is not possible to “fine tune” the use of `inline`: You cannot ask for inlining _just_ at specific places (e. g. in a loop).



## caveats

  * `inline` is a compiler _hint_. The compiler can ignore it. If the compiler warns you it has not inlined a certain code part marked as `inline`, you should remove the `inline` directive. This is not a bug; it is about code complexity.
  * Recursive routines cannot be inlined.
  * Regardless of the `inline` request _there is_ one instance the routine exists in memory like usual.



## application

If you want to optimize for speed, consider trying [`{$optimization autoInline}`](<Optimization.md> "Optimization") first, before manually adding `inline` hints. 

## see also

  * [`inline`](<https://www.freepascal.org/docs-html/ref/refsu77.html>) in the FPC Reference Guide
  * [`$INLINE`: Allow inline code](<https://www.freepascal.org/docs-html/prog/progsu36.html>) in the FPC Programmers’ Guide

---

_Source: [https://wiki.freepascal.org/Inline](https://web.archive.org/web/20250219010927/https://wiki.freepascal.org/Inline)_
