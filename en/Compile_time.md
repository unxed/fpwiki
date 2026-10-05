# Compile time

│ **English (en)** │

**Compile time** is the duration it takes to compile a module. [Pascal](<Pascal.md> "Pascal") modules can be be compiled in a very short time. A [Hello, World](<Hello,_World.md> "Hello, World") program can be compiled in less than half a second. The [FPC](<FPC.md> "FPC") reports the time it took if the command line option `-vi` (show general information) is set. 

**Compile-time** refers to the period when a Pascal module is being compiled. Thus “Compile-time information” refers to information known by the [compiler](<Compiler.md> "Compiler") during compilation. Examples of such information available at compile-time include [compiler switches](</index.php?title=compiler_switch&action=edit&redlink=1> "compiler switch \(page does not exist\)"), values of [identifiers](<Identifier.md> "Identifier") defined as [const](<Const.md> "Const"), quoted [strings](<String.md> "String"), and the actual text of the program. 

Information which is not available until the [program](<Executable_program.md> "Executable program") is being executed is referred to as being known at [run-time](<runtime.md> "runtime"). 

Information available at compile-time is usually more efficient for the program as [initialization](<Initialization.md> "Initialization") of [constants](<Constant.md> "Constant") and value-defined [variables](<Var.md> "Var") can be done once, when the program is compiled, as opposed to doing so at runtime each time the program is started. 

## see also

  * [compile-time error](<compile-time_error.md> "compile-time error")
  * [compile-time expressions](</index.php?title=compile_time_expressions&action=edit&redlink=1> "compile time expressions \(page does not exist\)")
  * [compile-time variables](</index.php?title=compile_time_variables&action=edit&redlink=1> "compile time variables \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Compile_time](https://web.archive.org/web/20240920204054/https://wiki.freepascal.org/Compile_time)_
