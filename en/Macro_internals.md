# Macro internals

## Implementation of macros in FPC

Compile time symbols (a k a defines), substitution macros and compile time variables, are all internally refered to as macros. 

Each macro is stored in an object of type TMacro. Macro objects belonging to the same scope are collected in a macro symbol table. When a macro is defined, a macro object is created. If it later on is undefined, it is not deleted, instead its _defined_ flag is set to false. If again defined, its _defined_ flag is set to true. 

Since several scopes of macro symbols might be in effect, their corresponding macro symbol tables are arranged in a macrosymbolstack, in a fashion in parallell with ordinary symbol tables. 

When a macro is used, the stack is searched top-down for the macro. If an existing macro is changed in any way, and the macro is not found on the top of the stack, a new macro object is created and put in the macrosymtable on the top of the stack. This is the case even if the macro is undefined, in which case the defined flag of the newly created macro object is set to false. 

Each module always has a localmacrosymtable. If its macros should be exported (happends in MODE MACPAS), it will also have a globalmacrosymtable. There is also the internalmacrosymtable, which is immutable, and is used by every module. It contains all predefined macros, both by the compiler and given on the command line. 

For a unit which do not export and import macros, the macrosymbolstack look like this: 
    
    
     <top>
     localmacrosymtable
     internalmacrosymtable
    

And for a unit which do export and import macros, it look like this (at the end of the parsing): 
    
    
     <top>
     localmacrosymtable
     globalmacrosymtable
     globalmacrosymtables for used unit 1
     globalmacrosymtables for used unit 2
     ...
     internalmacrosymtable
    

### System macros

A system macro is a macro which is defined by the compiler, it often has FPC_ prepended to it. Although it is not a good programming practice, system macros can be overridden by the user. 

Most system macros are in initialsymboltable, but those which are specific to a unit are put in the localmacrosymtable of the unit instead. Examples are FPC_<mode> and FPC_THREADING. 

System macros are not exported, even if they are available in the interface part of a unit, although if they are redefined there, they _will_ be exported. 

### Compilation of a module with regard to macros

When a unit is about to be compiled, its localmacrosymtable is created, and it is pushed on the stack (which initially contains initialmacrosymtable). At this moment we do not know if macros should be exported. 

Macros should be exported if it is a) a unit and b) it is MODE MACPAS. 

Only one $MODE <xxx> declaration is allowed when a module is parsed. This ensures that when a $MODE <xxx> is encountered, we know for sure what the mode of the file should be. So, if a $MODE MACPAS is encountered, the globalmacrosymtable is immediatelly created. 

If -Mmacpas is given on the command line, and no $MODE has been encountered when UNIT is encountered, the globalmacrosymtable is created, and further $MODE switch is forbidden. 

For other modes given on the command line, or if no mode is given, $MODE is forbidden at the point where the first pascal construct is encountered after the UNIT or PROGRAM clause. In this case, or in all cases if it is a PROGRAM, the module has only a localmacrosymtable, and none of what is described below will happen. Thus the following apply to MODE MACPAS units only. 

Between the beginning of the file and until the globalmacrosymtable is created, the localmacrossymtable is available for macros which are declared by the user or for local system macros. They will of course not be exported. 

When there is a globalmacrosymtable, user macros are declared there, but system macros are declared in localmacrosymtable (so they are not exported). If some of the macros declared in the localmacrosymtable part is overriden, this is ok, and the override is then put in globalmacrosymtable. 

At IMPLEMENTATION the stack positions of globalmacrosymtable and localmacrotable is swapped. Macros existing in the localmacrosymtable, which has been overriden in globalmacrosymtable is deleted from localmacrosymtable (otherwise it would appear for the user as the macro sudenly has changed). 

_Caveat:_ If $MODE MACPAS is given before UNIT, the macros defined between this point and UNIT is exported, if the unit is compiled as part of a program or other unit, but is not exported if the unit is compiled from the commandline (at top level). Remedy: Do not put macros before UNIT. 

_NOTE:_ In macpas, globalmacrosymtable is saved only if it contains anything.

---

_Source: [https://wiki.freepascal.org/Macro_internals](https://web.archive.org/web/20230924182801/https://wiki.freepascal.org/Macro_internals)_
