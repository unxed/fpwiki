# Global variables

│ **English (en)** │  **[suomi (fi)](</Global_variables/fi> "Global variables/fi")** │  **[русский (ru)](<../ru/Global_variables.md> "Global variables/ru")** │    
****

A [variable](<Variable.md> "Variable") is global if it is exported from a module. This usually refers to variables declared in a [`var` section](<Var.md> "Var")

  * prior any other nested [block](<Block.md> "Block") in a [`program`](<Program.md> "Program"), or
  * in the `interface` part of a [unit](<Unit.md> "Unit").



Global variables can be accessed from all other modules that import the exporting modules. Note, however, that a `program` can not be imported. 
    
    
    program globalVariableDemo(input, output, stdErr);
    var
    	x: integer;
    
    procedure doMagic;
    begin
    	// here, x is global to doMagic
    end;
    
    procedure foo;
    var
    	// shadow the global x
    	x: integer;
    begin
    	// here, x is local,
    	// as the top-scope x can not be accessed
    end;
    
    // MAIN //
    begin
    	// here, x is local
    end.
    

## remarks

If speed matters, global variables are/were used for frequently invoked routines, since allocating [local variables](<Local_variables.md> "Local variables") on the stack takes time. This, however, is considered bad style. Nest your variables as deep as possible, but as high as necessary. 

A [`resourceString` variable](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)") is always global. 

The [FPC](<FPC.md> "FPC") supports thread variables, too. They are sort of half-way between global and local variables. A [`threadVar` variable](</index.php?title=ThreadVar&action=edit&redlink=1> "ThreadVar \(page does not exist\)") is local to a thread. 

## see also

  * [Tutorial: Scope](<Scope.md> "Scope")
  * [singleton pattern](<Singleton_Pattern.md> "Singleton Pattern")
  * [Article “Global variable” on the English Wikipedia](<https://en.wikipedia.org/wiki/Global_variable>)

---

_Source: [https://wiki.freepascal.org/Global_variables](https://web.archive.org/web/20241213031114/https://wiki.freepascal.org/Global_variables)_
