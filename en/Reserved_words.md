# Reserved words

│ **[Deutsch (de)](</Reserved_words/de> "Reserved words/de")** │  **English (en)** │  **[français (fr)](</Reserved_words/fr> "Reserved words/fr")** │  **[polski (pl)](</Reserved_words/pl> "Reserved words/pl")** │  **[русский (ru)](<../ru/Reserved_words.md> "Reserved words/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</Reserved_words/zh_CN> "Reserved words/zh CN")** │    
****

## Contents

  * 1 Reserved words in Turbo Pascal
  * 2 Reserved words in Object Pascal
  * 3 Reserved words in Extended Free Pascal
  * 4 Modifiers (directives)
  * 5 Unsupported Turbo Pascal modifiers
  * 6 More functionality
  * 7 See also



The keywords of the individual [compiler modes](</Category:Modes> "Category:Modes") are summarized as follows: 

  * [Turbo Pascal mode](<Mode_TP.md> "Mode TP"): the [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") keywords are available for you to use
  * [ Delphi mode](<Mode_Delphi.md> "Mode Delphi"): the Turbo Pascal and Object Pascal keywords are available for you to use
  * [ Extended Free Pascal mode](<Mode_ObjFPC.md> "Mode ObjFPC"): the Turbo Pascal and Object Pascal keywords are available for you to use



Keywords are **reserved** words, ie they cannot be defined as [identifiers](<Identifier.md> "Identifier") by the programmer. This means that you cannot use these words to name your variables, constants, function and procedure names, class names, etc. In Pascal, in the source code, keywords are often shown in bold. 

## Reserved words in Turbo Pascal

The following keywords occur in Turbo Pascal mode: 

Keyword | Description   
---|---  
[and](<And.md> "And") | Boolean operator requiring both conditions are true for the result to be true   
[array](<Array.md> "Array") | multiple elements with the same name   
[asm](<Asm.md> "Asm") | start of code written in assembly language   
[begin](<Begin.md> "Begin") | start of a [block](<Block.md> "Block") of code   
[break](<Break.md> "Break") | exit a loop   
[case](<Case.md> "Case") | select a particular segement of code to execute based on a value   
[const](<Const.md> "Const") | declare an identifier with a fixed value, or a variable with an initialized value   
[constructor](<Constructor.md> "Constructor") | routine used to create an object   
[continue](<Continue.md> "Continue") | skips an iteration in a loop and restart execution at the beginning of the loop   
[destructor](<Destructor.md> "Destructor") | routine used to deallocate an object   
[div](<Div.md> "Div") | integer divide operator   
[do](<Do.md> "Do") | used to indicate start of a loop   
[downto](<Downto.md> "Downto") | used in a [for](<For.md> "For") loop to indicate the index variable is decremented   
[else](<Else.md> "Else") | used in [if](<If.md> "If") statement to provide an execution path when the if test fails   
[end](<End.md> "End") | end of a block of code, a record or certain other constructs   
[false](<False.md> "False") | boolean value indicating a test failed; opposite of [true](<True.md> "True"). **As of FPC 3.0.0. False is no longer a keyword**.   
[file](</File> "File") | external data structure, typically stored on disc   
[for](<For.md> "For") | loop used to increment or decrement a control variable   
[function](<Function.md> "Function") | define start of a routine that returns a result value   
[goto](<Goto.md> "Goto") | used to exit a segment of code and jump to another point   
[if](<If.md> "If") | test a condition and perform a set of instructions based on the result   
[implementation](<Implementation.md> "Implementation") | define the internal routines in [unit](<Unit.md> "Unit")  
[in](<In.md> "In") | identifies elements in a collection   
[inline](<Inline.md> "Inline") | machine code inserted directly into a routine   
[interface](<Interface.md> "Interface") | public declarations of routines in a [unit](<Unit.md> "Unit")  
[label](<Label.md> "Label") | defines the target jump point for a [goto](<Goto.md> "Goto")  
[mod](<Mod.md> "Mod") | operator used to return the remainder of an integer division   
[nil](<Nil.md> "Nil") | pointer value indicating the pointer does not contain a value   
[not](<Not.md> "Not") | boolean operator that negates the result of a test   
[object](<Object.md> "Object") | defines an object construct   
[of](<Of.md> "Of") | defines the characteristics of a variable   
[on](<On.md> "On") | defines an exception handling statement in the [Except](<Except.md> "Except") part of a [Try](<Try.md> "Try") statement   
[operator](<Operator.md> "Operator") | defines a routine used to implement an operator   
[or](<Or.md> "Or") | boolean operator which allows either of two choices to be used   
[packed](<Packed.md> "Packed") | indicates the elements of an array are to use less space (this keyword is primarily for compatibility with older programs as packing of array elements is generally automatic)   
[procedure](<Procedure.md> "Procedure") | define start of a routine that does not return a result value   
[program](<Program.md> "Program") | defines start of an application. This keyword is usually optional.   
[record](<Record.md> "Record") | group a series of variables under a single name   
[repeat](<Repeat.md> "Repeat") | loop through a section of code through an [until](<Until.md> "Until") statement as long as the result of the test is true   
[set](<Set.md> "Set") | group a collection   
[shl](<Shl.md> "Shl") | operator to shift a value to the left; equivalent to multiplying by a power of 2   
[shr](<Shr.md> "Shr") | operator to shift a value to the right; equivalent to dividing by a power of 2   
[string](<String.md> "String") | declares a variable that contains multiple characters   
[then](<Then.md> "Then") | indicates start of code in an [if](<If.md> "If") test   
[to](<To.md> "To") | indicates a [for](<For.md> "For") variable is to be incremented   
[true](<True.md> "True") | boolean value indicating a test succeeded; opposite of [False](<False.md> "False"). **As of FPC 3.0.0. True is no longer a keyword**.   
[type](<Type.md> "Type") | declares kinds of records or new classes of variables   
[unit](<Unit.md> "Unit") | separately compiled module   
[until](<Until.md> "Until") | indicates end test of a [repeat](<Repeat.md> "Repeat") statement   
[uses](<Uses.md> "Uses") | names [units](<Unit.md> "Unit") this program or unit refers to   
[var](<Var.md> "Var") | declare variables   
[while](<While.md> "While") | test a value and if true, loop through a section of code   
[with](<With.md> "With") | reference the internal variables within a record without having to refer to the record itself   
[xor](<Xor.md> "Xor") | boolean operator used to invert and [or](<Or.md> "Or") test   
  
## Reserved words in Object Pascal

Object Pascal extends the (Turbo) Pascal language with both support for dealing more easily with objects (object orientation) as well as other newer/more advanced concepts (threads, etc). 

In addition to the reserved words in Turbo Pascal, the following reserved words are available in Delphi mode as well: 

Keyword | Description   
---|---  
[as](<As.md> "As") |   
[class](<Class.md> "Class") |   
[constref](<Constref.md> "Constref") |   
[dispose](<Dispose.md> "Dispose") |   
[except](<Except.md> "Except") |   
[exit](<Exit.md> "Exit") |   
[exports](<Exports.md> "Exports") | exports symbols which will be publicly available   
[finalization](<Finalization.md> "Finalization") | introduces an optional 'finalization' part of a unit.   
[finally](<Finally.md> "Finally") | part of a try - finally - end block   
[inherited](<Inherited.md> "Inherited") | calls function/procedure from ancestor class   
[initialization](<Initialization.md> "Initialization") | introduces an optional 'initialization' part of a unit.   
[is](<Is.md> "Is") | can be used as an [operator](<Operator.md> "Operator") or a [modifier](<modifier.md> "modifier")  
[library](<Library.md> "Library") | used in a shared library unit instead of the reserved word [unit](<Unit.md> "Unit")  
[new](<New.md> "New") |   
[on](<On.md> "On") |   
[out](</index.php?title=Out&action=edit&redlink=1> "Out \(page does not exist\)") |   
[property](</Property> "Property") |   
[raise](<Raise.md> "Raise") | causes an exception   
[self](<Self.md> "Self") | reference to an instance of a class   
[threadvar](<Threadvar.md> "Threadvar") | declare global variable to be thread local   
[try](<Try.md> "Try") | part of Try .. Finally or Try .. Exception block   
  
## Reserved words in Extended Free Pascal

The reserved words in [Extended Free Pascal mode](<Mode_ObjFPC.md> "Mode ObjFPC") include: 

  * [Turbo Pascal mode](<Mode_TP.md> "Mode TP") reserved words
  * [Object Pascal mode](<Mode_Delphi.md> "Mode Delphi") reserved words  




## Modifiers (directives)

Modifiers are not strictly reserved words; however they are used in the same way as reserved words. 

See the [Free Pascal Reference Guide](<https://www.freepascal.org/docs-html/current/ref/ref.html>) for details. 

Modifiers | Description   
---|---  
[absolute](<Absolute.md> "Absolute") |   
[abstract](</index.php?title=abstract&action=edit&redlink=1> "abstract \(page does not exist\)") | an abstract class cannot be instantiated, only inherited   
[alias](<alias.md> "alias") |   
[assembler](</index.php?title=assembler&action=edit&redlink=1> "assembler \(page does not exist\)") | pure assembler routine: routine is defined by [`asm`](<Asm.md> "Asm") … [`end`](<End.md> "End")  
[cdecl](</index.php?title=cdecl&action=edit&redlink=1> "cdecl \(page does not exist\)") | C declaration modifier   
[Cppdecl](<Cppdecl.md> "Cppdecl") | C++ declaration modifier   
[default](</index.php?title=default&action=edit&redlink=1> "default \(page does not exist\)") | For indexed properties to use them without specifying the property name   
[export](</index.php?title=export&action=edit&redlink=1> "export \(page does not exist\)") |   
[external](</index.php?title=external&action=edit&redlink=1> "external \(page does not exist\)") |   
[forward](<Forward.md> "Forward") | Allow a subroutine to be used before it is declared   
[generic](</index.php?title=generic&action=edit&redlink=1> "generic \(page does not exist\)") | class creation modifier   
[index](</index.php?title=index&action=edit&redlink=1> "index \(page does not exist\)") |   
[local](<Local.md> "Local") | A function/procedure modifier only usable with Linux (for Kylix compatibility)   
[name](</index.php?title=name&action=edit&redlink=1> "name \(page does not exist\)") |   
[nostackframe](</index.php?title=nostackframe&action=edit&redlink=1> "nostackframe \(page does not exist\)") | compiler hint: omit stack frame if possible   
[oldfpccall](<oldfpccall.md> "oldfpccall") | _deprecated_ subroutine calling convention   
[override](<Override.md> "Override") | overriding of virtual functions   
[pascal](<pascal.md> "pascal") | use classic pascal calling convention   
[private](<Private.md> "Private") | private accessibility modifier, only class members can access data/functions/procedures   
[protected](<Protected.md> "Protected") | protected accessibility modifier, accessibility modifier, class members and inherited classes can access data/functions/procedures   
[public](<Public.md> "Public") | public accessibility modifier, public access to data/functions/procedures   
[published](<Published.md> "Published") | accessibility modifier, published properties are visible in IDE ar can be written to .lfm   
[read](<Read.md> "Read") | property read access   
[register](<Register.md> "Register") | define routine’s calling convention: pass first n parameters via GPRs   
[reintroduce](<Reintroduce.md> "Reintroduce") |   
[safecall](<Safecall.md> "Safecall") | subroutine calling convention   
[softfloat](</index.php?title=softfloat&action=edit&redlink=1> "softfloat \(page does not exist\)") |   
[specialize](</index.php?title=specialize&action=edit&redlink=1> "specialize \(page does not exist\)") | specialization of [generic classes](<Generics.md> "Generics")  
[stdcall](<Stdcall.md> "Stdcall") | subroutine calling convention   
[virtual](<Virtual.md> "Virtual") | describes a virtual method in OO programming   
[write](<Write.md> "Write") | property write access   
  
## Unsupported Turbo Pascal modifiers

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** These modifiers **are supported** in the [DOS](<DOS.md> "DOS") cross compiler present in the FPC development version

The reason why these modifiers are not supported is that these modifiers deal with 16 bit code for DOS. In other words, these modifiers have special meaning for 16 bit programming under DOS and Windows 3.x. 

As Free Pascal does not support 16 bit code (only 32 and 64 bit), these modifiers are irrelevant in Free Pascal code. 

[far](<Far.md> "Far") | access addresses outside of the current 64KB segment   
---|---  
[near](<Near.md> "Near") | access addresses in the current 64KB segment   
  
## More functionality

Apart from the language features provided by the reserved words/keywords mentioned above, there is a lot of functionality available for the programmer in the various libraries: 

  * [RTL](<RTL.md> "RTL"): Run-Time Library, available for all FPC and Lazarus programs
  * [FCL](<FCL.md> "FCL"): Free Component Library: a core set of libraries available for Lazarus programs and usually for FPC (FPC can be compiled without it, but that only happens on purpose for low-memory embedded systems etc)
  * FPC Packages: other packages provided by FPC
  * Lazarus components: these are Lazarus components that can be dropped on a form and often based on FCL or FPC packages
  * Lazarus utility functions: e.g. the [fileutil](<http://wiki.lazarus.freepascal.org/Category:fileutil>) unit.



Apart from the libraries provided by FPC and Lazarus, there are more libraries/components available: 

  * FPC user-supplied units: see the FPC wiki
  * Lazarus CCR: components
  * User-supplied code on the internet: see open source repositories like SourceForge and GitHub.



## See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")


  *[GPR]: general purpose register

---

_Source: [https://wiki.freepascal.org/Reserved_words](https://web.archive.org/web/20230204002430/https://wiki.freepascal.org/Reserved_words)_
