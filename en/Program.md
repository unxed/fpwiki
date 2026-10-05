# Program

│ **English (en)** │  **[русский (ru)](<../ru/Program.md>)** │

A **program** is either an [executable program](<Executable_program.md> "Executable program"), that is, the complete and runnable [application](<Application.md> "Application"), or it is that portion of a [Pascal](<Pascal.md> "Pascal") [Source code](<Source_code.md> "Source code") [file](</File> "File") or files that can be compiled and is not declared to be a [unit](<Unit.md> "Unit") or [library](<Library.md> "Library"). This is sometimes referred to as the main program. 

## Main program

`program` is a [reserved word](<Reserved_word.md> "Reserved word") that introduces a classical program source code file: 
    
    
    program hiWorld(input, output, stdErr);
    
    begin
    	writeLn('Hi!');
    end.
    

Meanwhile [FPC](<FPC.md> "FPC") _discards_ the program header, i. e. the first line. The output file name is determined by the source code file’s name. However, the program name _does_ become a reserved [identifier](<Identifier.md> "Identifier") (except in the ISO modes [since FPC 3.3.1/trunk revision #45757; cf. [Issue #37322](<https://bugs.freepascal.org/view.php?id=37322>)]). In the example above, e.g. attempting to define a constant named `hiWorld` would trigger a duplicate identifier [compile time](<Compile_time.md> "Compile time") error. The program name will identify the global [scope](<Scope.md> "Scope"), thus it can be used to write fully-qualified identifiers. 

The file descriptor list is completely ignored, except in [`{$mode ISO}`](<Mode_iso.md> "Mode iso"). The [`text`](<Text.md> "Text") [variables](<Variable.md> "Variable") [`input`](<https://www.freepascal.org/docs-html/rtl/system/input.html>), [`output`](<https://www.freepascal.org/docs-html/rtl/system/output.html>) and [`stderr`](<https://www.freepascal.org/docs-html/rtl/system/stderr.html>) are always opened and their names can not be changed in other modes. (cf.: [`SysInitStdIO`](<https://www.freepascal.org/docs-html/rtl/system/sysinitstdio.html>) is always called in [`rtl/linux/system.pp`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_0_4/rtl/linux/system.pp#L367-L368>)) 

Therefore with FPC the following complete source code example compiles identically as does the previous example. 
    
    
    begin
    	writeLn('Hi!');
    end.
    

If the program is syntactically correct, FPC ignores anything that comes after the final `end.`. The following will compile without problems: 
    
    
    program awesomeProgram(input, output, stdErr);
    begin
    	writeLn('Awesome!');
    end.I thank my mom, my dad, and everyone who supported me in making this program.
    

This “feature” is primarily used to supply an in-file changelog or copyright notice. 

FPC does not support multiple modules in one source code file, like some other compilers did or do. Each module source code has to reside in its own file. However, the restriction that module names have to match file names does not apply to programs. This is due to the fact that programs cannot be included by other modules, thus searching them (via their file name) is not necessary. 

## Program structure

A `program` file has to follow a certain [structure](<Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure"). 

  1. A (depending on used compiler possibly optional) program header.
  2. There can be at most one [`uses`-clause](<Uses.md> "Uses") and it has to be at the top of the program right after the program header.
  3. Exactly one [block](<Block.md> "Block") that concludes with an `end.` (note the [period](<period.md> "period")). This block may contain – in contrast to regular blocks – `resourcestring` section(s).



The exact order and number of various sections after the (optional) `uses`-clause until final the compound statement [`begin`](<Begin.md> "Begin")…[`end.`](<End.md> "End") is free of choice. 

However, there are some plausible considerations. 

  * A [`type`-section](<Type.md> "Type") comes prior any section that can use types, e.g. [`var`-sections](<Var.md> "Var") or [routine](<Routine.md> "Routine") declarations.
  * Since [`goto`](<Goto.md> "Goto") is known as the devil’s tool, a [`label`-section](<Label.md> "Label"), if any, is as close as possible to the statement-frame it is supposed to declare labels for.
  * Generally you go from general into specifics: For example a `var`-section comes in front of a [`threadVar`-section](<Threadvar.md> "Threadvar"). A [`const`-section](<Const.md> "Const") comes before a [`resourceString`-section](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)").
  * `resourceString`-sections can be either static or global, that means they should appear relatively soon after the `uses`-clause.
  * Direct usage of [global variables](<Global_variables.md> "Global variables") in routines (or even the mere possibility) is considered as bad style. Instead, declare/define your routines prior any `var`-(like)-section. (beware: play it safe and set `{$writeableConst off}`)
  * [Global compiler directives](<global_compiler_directives.md> "global compiler directives"), especially such that allow or restrict what can be written (e.g. `{$goto on}` allows the use of `goto`) or implicitly add unit dependencies like [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") should appear soon after the program header.



Taking all considerations into account the rough program structure should look like this (except for `label` and [`{$goto on}`](<sGoto.md> "sGoto") which are only mentioned for the sake of completeness): 
    
    
    program sectionDemo(input, output, stdErr);
    
    // Global compiler directives ----------------------------
    {$mode objFPC}
    {$goto on}
    
    uses
    	sysUtils;
    
    const
    	answer = 42;
    
    resourceString
    	helloWorld = 'Hello world!';
    
    type
    	primaryColor = (red, green, blue);
    
    procedure doSomething(const color: primaryColor);
    begin
    end;
    
    // M A I N -----------------------------------------------
    var
    	i: longint;
    
    threadVar
    	z: longbool;
    
    label
    	42;
    begin
    end.
    

[The example consciously ignores the possibility of “typed constants”, sticking rather to traditional concepts than unnecessarily confusing beginners.]

## See also

  * [unit](<Unit.md> "Unit")
  * [library](<Library.md> "Library")
  * [program structure](<Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") in the basic Pascal Introduction series
  * [§ “Beginning” in the _Pascal Programming_ book on Wikibooks.org](<https://en.wikibooks.org/wiki/Pascal_Programming/Beginning>)


  *[ISO]: International Organization for Standardization

---

_Source: [https://wiki.freepascal.org/Program](https://web.archive.org/web/20250421233940/https://wiki.freepascal.org/Program)_
