# The Power of 10

│ **English (en)** │

_Infinite loops or memory leaks in the software of a launch rocket or unmanned space probe may cause the mission to be lost. The controller of a ventilator assuming a default value for a missing read-out of a measurement may lead to the death of a critically ill patient. Two crashes of the Boeing 737 MAX with a high number of fatalities ensued from a piece of software blindly trusting the results of a single sensor that delivered invalid data._

[Software security](<Secure_programming.md> "Secure programming") is critical for every project. This applies especially to applications in certain fields like medicine, military, industry and aerospace. This is why the National Aeronautics and Space Administration (NASA) defined a set of rules called **The Power of 10** summarising basic principles on how to improve the security of code. 

The [Pascal](<Pascal.md> "Pascal") language already implements some basic measures for improved security from the ground up, including strong typing and the encouragement of safe control structures. It is advisable, however, to have some additional principles in mind, especially at the beginning of a new project. Of course, it is possible to implement these principles in later stages, too. 

The Power of 10 Rules was defined in 2006 by Gerard J. Holzmann at the JPL|NASA/JPL Laboratory for Reliable Software. Their main intention is eliminating certain C coding practices which make code difficult to review or statically analyze. These rules have been incorporated into the coding standards of multiple institutions. 

## Contents

  * 1 Rules
    * 1.1 Avoid complex flow constructs, such as goto, direct or indirect recursion
    * 1.2 All loops must have a fixed upper bound in order to prevent runaway code
    * 1.3 Avoid heap memory allocation after initialisation
    * 1.4 Restrict procedures, functions and methods to a single printed page. Typically, this means no more than about 60 lines of code per block
    * 1.5 Use a minimum of two runtime assertions per function
    * 1.6 Restrict the scope of data to the smallest possible. Therefore, local variables should be preferred over global variables
    * 1.7 Check the return value of all functions or return nil or NaN in order to indicate that the return value is useless. Additionally, each called function must check the validity of all actual parameters provided by the caller
    * 1.8 Use compiler directives or IDE directives sparingly. If possible, they should be restricted to the interface section of a unit
    * 1.9 Limit the use of pointers to a single dereference and avoid using function pointers
    * 1.10 Compile with all possible warnings active; all warnings should then be addressed before release of the software
  * 2 See also
  * 3 Examples
  * 4 References



## Rules

The ten rules are: 

  1. Avoid complex flow constructs, such as [goto](<Goto.md> "Goto"), direct or indirect [recursion](<Recursion.md> "Recursion").
  2. All loops must have a fixed upper bound in order to prevent runaway code.
  3. Avoid heap memory allocation after initialisation.
  4. Restrict [procedures](<Procedure.md> "Procedure"), [functions](<Function.md> "Function") and [methods](<Method.md> "Method") to a single printed page. Typically, this means no more than about 60 lines of code per block.
  5. Use a minimum of two runtime assertions per function.
  6. Restrict the scope of data to the smallest possible. Therefore, local [variables](<Variable.md> "Variable") should be preferred over global variables.
  7. Check the return value of all functions or return [nil](<Nil.md> "Nil") or [NaN](<NaN.md> "NaN") in order to indicate that the return value is useless. Additionally, each called function must check the validity of all actual parameters provided by the caller.
  8. Use [compiler directives](<Compiler_directive.md> "Compiler directive") or [IDE directives](<IDE_directives.md> "IDE directives") sparingly. If possible, they should be restricted to the [interface](<Interface.md> "Interface") section of a [unit](<Unit.md> "Unit").
  9. Limit the use of [pointers](<Pointer.md> "Pointer") to a single dereference and avoid using function pointers.
  10. Compile with all possible warnings active; all warnings should then be addressed before release of the software.



  


### Avoid complex flow constructs, such as goto, direct or indirect recursion

Although the **goto** construct is available in Pascal its use is not encouraged, because it makes code hard to read and even harder to predict its behaviour. Similar considerations apply to **exit**. 

Where possible even constructs with recursion should be avoided. In many cases **for** , **while** and **repeat** ... **until** loops do the same job quite well. 

### All loops must have a fixed upper bound in order to prevent runaway code

This rule is very similar to the first one. Fixed bounds help to obviate the use of control structures like **goto** or **exit**. 

### Avoid heap memory allocation after initialisation

Mismatched pairs of allocation and disposing of memory blocks in the heap is a common source of crashes or resource leaks. Additionally, this technique usually requires the use of pointers or handles and should therefore be avoided as far as possible (see rule #9). 

### Restrict procedures, functions and methods to a single printed page. Typically, this means no more than about 60 lines of code per block

Shorter code blocks have three advantages: 

  * They are easier to understand.
  * They are less prone to be overcharged.
  * In many cases shortening blocks prevents duplication of code.



### Use a minimum of two runtime assertions per function

Assertions are a sort of automatic seat belt for your code. Their effect is similar to that of adhering to rule #7. 

### Restrict the scope of data to the smallest possible. Therefore, local variables should be preferred over global variables

Globals can be inadvertently changed by bugs in any location of the code. This danger can be significantly reduced by restricting their scope to a local context. Likewise, declaring fields in classes as protected or private can help improving security. 

### Check the return value of all functions or return nil or NaN in order to indicate that the return value is useless. Additionally, each called function must check the validity of all actual parameters provided by the caller

Don't blindly trust existing code, even if it is written by yourself. It is always a good idea to check values for plausibility. Vice versa, the checking on the site of the calling function may be supported by explicitly indicating invalid results. 

### Use compiler directives or IDE directives sparingly. If possible, they should be restricted to the interface section of a unit

Directives and conditionals add a separate layer to your code, which follows a kind of logic that is different to that of the main algorithm. This may be a major source of unpredictability. Similar considerations may also apply to the use of macros. 

### Limit the use of pointers to a single dereference and avoid using function pointers

Pointer arithmetic is complex and often confusing. A dysbalance between allocating and freeing may cause crashes and leaks. Additionally, like global variables, memory blocks referenced by pointera are prone to be accidentally damaged by bugs in any place of your code. 

### Compile with all possible warnings active; all warnings should then be addressed before release of the software

If using FPC from the command line especially the -B, -C and -g options may be helpful to generate valuable information. In Lazarus this can be [specified](<Turn_warnings_and_hints_on_or_off.md> "Turn warnings and hints on or off") in the options dialog on a global level. Additionally, it is possible to control this behaviour via IDE macros on a local level. This, of course, is an exception to rule #8. 

## See also

  * [The Power of Proper Planning and Practices](<The_Power_of_Proper_Planning_and_Practices.md> "The Power of Proper Planning and Practices")
  * [Secure programming](<Secure_programming.md> "Secure programming")



## Examples

  * On June 4th, 1996 the _Ariane 5_ heavy-lift launch rocket failed on her maiden flight due to an integer overflow and an unhandled exception. This caused the failure of the _Cluster_ magnetosphere research mission and resulted in the loss of more than US$370 million. This could have been avoided by obeying rule #7 (and probably #10).
  * Between 1985 and 1987 three patients were killed and three additional subjects were severely injured, when the computer-controlled _Therac-25_ radiation therapy machine provided massive overdoses of radiation. Causes included reusing poorly understood code for an older model and an arithmetic overflow of a flag variable. Rules #4, #5 and #7 would have prevented this.



## References

  1. GJ Holzmann: [The Power of 10: Rules for Developing Safety-Critical Code](<http://web.eecs.umich.edu/~imarkov/10rules.pdf>) DOI: [10.1002/9781119174240.ch10](<https://doi.org/10.1002/9781119174240.ch10>)
  2. R Giorato: [The Power of 10 — NASA’s Rules for Coding](<https://medium.com/better-programming/the-power-of-10-nasas-rules-for-coding-43ae1764f73d>). Medium, October 2nd, 2019

---

_Source: [https://wiki.freepascal.org/The_Power_of_10](https://web.archive.org/web/20250324174025/https://wiki.freepascal.org/The_Power_of_10)_
