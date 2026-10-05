# Symbol entries

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 Symbol entries
    * 1.1 Architecture
    * 1.2 Symbol entry types
      * 1.2.1 Base symbol type (TSym)
      * 1.2.2 label symbol (TLabelSym)
      * 1.2.3 unit symbol (TUnitSym)
      * 1.2.4 macro symbol (TMacroSym)
      * 1.2.5 error symbol (TErrorSym)
      * 1.2.6 procedure symbol (TProcSym)
      * 1.2.7 type symbol (TTypeSym)
      * 1.2.8 variable symbol (TVarSym)
      * 1.2.9 property symbol (TPropertySym)
      * 1.2.10 return value of function symbol
      * 1.2.11 absolute declared symbol (TAbsoluteSym)
      * 1.2.12 typed constant symbol
      * 1.2.13 constant symbol (TConstSym)
      * 1.2.14 enumeration symbol
      * 1.2.15 program symbol
      * 1.2.16 sys symbol
    * 1.3 Symbol interface



# Symbol entries

## Architecture

_(last updated for fpc version 1.0.x)_

There are different possible types of symbols, each one having different fields then the others. Each symbol type has a specific signature to indicate what kind of entry it is. Each entry in the symbol table is actually one of the symbol entries described in the following sections. The relationship between a symbol entry, a type definition, and the type name symbol entry is shown in the following figure: 

<http://www.pjh2.de/fpc/CompilerInternalsFigure06.png>

## Symbol entry types

_(last updated for fpc version 1.0.x)_

### Base symbol type (TSym)

All entries in the symbol table are derived from this base object which contains information on the symbol type as well as information on the owner of this symbol entry. 
    
    
    type
      PSym = ^TSym;
      TSym = object(TSymTableEntry)
        SymOptions: TSymOptions;  // Indicate the access scope of the symbol
        FileInfo: TFilePosInfo;
        Refs: Longint;            // Indicates how many times this label is refered in the
                                  // parsed code (is only used with variable and assembler
                                  // label symbols).
        LastRef: PRef;
        DefRef: PRef;
        LastWritten: PRef;
        RefCount: Longint;        // Browser information indicating the reference count
        Typ: TSymTyp;             // Indicates the symbol type
        IsStabWritten: Boolean;   // Set to TRUE if the stabs debugging information has
                                  // been written for this symbol.
      end;

TSymTyp | Description   
---|---  
AbstractSym | This is a special abstract symbol (this should never occur)   
VarSym | This symbol is a variable declaration in the var section, or a var parameter.   
TypeSym | This symbol is a type name   
ProcSym | This symbol is a routine or method name   
UnitSym | This symbol is a unit name   
ProgramSym | This symbol is the main program name   
ConstSym | This symbol is a constant   
EnumSym | This symbol is an enumeration symbol (an element in an enumeration)   
TypedConstSym | This symbol is pre-initialized variable (pascal typed constant)   
ErrorSym | This symbol is created for error generation   
SysSym | This symbol represents an inlined system unit routine   
LabelSym | This symbol represents a label in a label pascal declaration   
AbsoluteSym | This symbol represents an the symbol following an absolute variable declaration   
PropertySym | This symbol is a property name   
FuncRetSym | This symbol is the name of the return value for functions   
MacroSym | This symbol is a macro symbol (just like #define in C)   
  
### label symbol (TLabelSym)

The label symbol table entry is only created when a pascal label is declared via the label keyword. The object has the following fields which are available for use publicly: 
    
    
    type
      PLabelSym = ^TLabelSym;
      TLabelSym = object(TSym)
        Used: Boolean;                 // Set to TRUE if this pascal label is used using a goto
                                       // or in an assembler statement
        Defined: Boolean;              // Set to TRUE if this label has been declared
        Lab: PAsmLabel;                // Points to the actual assembler label structure which
                                       // will be emitted by the code generator
        Code: Pointer;
      end;

  


### unit symbol (TUnitSym)

The unit symbol is created and added to the symbol table each time that the uses clause is parsed and a unit name is found, it is also used when compiling a unit, with the first entry in that symbol table being the unit name being compiled. The unit symbol entry is actual part of a linked list which is used in the unit symbol table. 
    
    
    type
      PUnitSym = ^TUnitSym;
      TUnitSym = object(TSym)
        UnitSymTable: PUnitSymTable;  // Pointer to the global symbol table for that
                                      // unit, containing entries for each public?
                                      // symbol in that unit
        PrevSym: PUnitSym;            // Pointer to previous entry in the linked list
      end;

### macro symbol (TMacroSym)

The macro synbols are used in the preprocessor for conditional compilation statements. There is one such entry created for each $define directive, it contains the value of the define (stored as a string). 
    
    
    type
      PMacroSym = ^TMacroSym;
      TMacroSym = object(TSym)
        Defined: Boolean;              // TRUE if the symbol has been defined with a 
                                       // $define directive, or false if it has been 
                                       // undefined with a $undef directive
        Defined_At_Startup: Boolean;   // TRUE if the symbol is a system wide define
        Is_Used: Boolean;              // TRUE if the define has been used such as in 
                                       // a $ifdef directive.
        BufText: PChar;                // The actual string text of the define
        BufLength: Longint;            // The actual string length of the define
      end;

### error symbol (TErrorSym)

This symbol is actually an empty symbol table entry. When the parser encounters an error when parsing a symbol, instead of putting nothing in the symbol table, it puts this symbol entry. This avoids illegal memory accesses later in parsing. 

### procedure symbol (TProcSym)

The procedure symbol is created each time a routine is defined in the code. This can be either a forward definition or the actual implementation of the routine. After creation, the symbol is added into the appropriate symbol table stack. 
    
    
    type
      PProcSym = ^TProcSym;
      TProcSym = object(TSym)
        Is_Global: Boolean;           // Set if the routine is exported by the unit
        Definition: PProcDef;         // Procedure definition, including parameter 
                                      // information and return values
      end;

### type symbol (TTypeSym)

The type symbol is created each time a new type declaration is done, the current symbol table stack is then inserted with this symbol. Furthermore, each time the compiler compiles a module, the default base types are initialized and added into the symbol table (psystem.pas) The type symbol contains the name of a type, as well as a pointer to its type definition. 
    
    
    type
      PTypeSym = ^TTypeSym;
      TTypeSym = object(TSym)
        ResType: TType;               // Contains base type information as well as the type
                                      // definition
      end;

### variable symbol (TVarSym)

Variable declarations, as well as parameters which are passed onto routines are declared as variable symbol types. Access information, as well as type information and optimization information are stored in this symbol type. 
    
    
    type
      PVarSym = ^TVarSym;
      TVarSym = object(TSym)
        Reg: TRegister;              // If the value is a register variable, the reg field will
                                     // be different then R_NO
        VarSpez: TVarSpez;           // Indicates the variable type (parameters only) (Cf. 32).
        Address: Longint;            // In the case where the variable is a routine parameter,
                                     // this indicates the positive offset from theframe_pointer 
                                     // to access this variable. In the caseof a local variable,
                                     // this field indicates the negative offset from the 
                                     // frame_pointer. to access this variable.
        LocalVarSym: PVarSym;
        VarType: TType;              // Contains base type information as well as the type
                                     // definition
        VarOptions: TVarOptions;     // Flags for this variable (Cf. 31)
        VarState: TVarState;         // Indicates the state of the variable, if it’s used or
                                     // declared
      end;

  


TVarOptions | Description   
---|---  
vo_regable | The variable can be put into a hardware general purpose register   
vo_is_c_var | The variable is imported from a C module   
vo_is_external | The variable is declared external   
vo_is_Dll_var | The variable is a shared library variable   
vo_is_thread_var | The variable is declared as being thread safe   
vo_fpuregable | The variable can be put into a hardware floating point register   
vo_is_local_copy |   
vo_is_const | unused and useless   
vo_is_exported | The variable is declared as exported in a dynamic link library   
  
  


TVarSpez | Description   
---|---  
vs_value | This is a value parameter   
vs_const | This is a constant parameter, property or array   
vs_var | This is a variable parameter   
  
### property symbol (TPropertySym)
    
    
    type
      PPropertySym = ^TPropertySym;
      TPropertySym = object(TSym)
        PropOptions: TPropertyOptions;    // ???
        PropType: TType;                  // Indicates the type of the property
        PropOverriden: PPropertySym;      // ???
        IndexType: TType;
        Index: Longint;                   // ????
        Default: Longint;                 // ???
        ReadAccess: PSymList;             // ???
        WriteAccess: PSymList;            // ???
        StoredAccess: PSymList;           // ???
      end;

### return value of function symbol

### absolute declared symbol (TAbsoluteSym)

This symbol represents a variable declared with the `absolute` keyword. The address of the `TVarSym` object holds the address of the variable in the case of an absolute address variable. 

The possible types of absolute symbols, are from an external object reference, an absolute address (for certain targets only), or on top of another declared variable. 
    
    
    type
      PAbsoluteSym = ^TAbsoluteSym;
      TAbsoluteSym = object(TVarSym)
        AbsTyp: TAbsoluteTyp;         // Indicates the type of absolute symbol it is 
        AbsSeg: Boolean;              // ???
        Ref: PSym;                    // In case abstyp is tovar, this field indicates the
                                      // symbol which is overlaid with this symbol. Otherwise
                                      // this field is unused.
        AsmName: PString;             // In case abstyp is toasm, this field indicates label
                                      // name for the variable.
      end;

TAbsoluteTyp | Description   
---|---  
toVar | The symbol will be declared on top of another symbol (variable or typed constant)   
toAsm | The variable is imported from an external module   
toAddr | The variable is declared as being at an absolute address   
  
### typed constant symbol

### constant symbol (TConstSym)

This symbol type will contain all constants defined and encountered during the parsing. The values of the constants are also set in this symbol type entry. 
    
    
    type
      PConstSym = ^TConstSym;
      TConstSym = object(TSym)
        ConstType: TType;       // Type information for this constant (?).
        ConstTyp: TConstTyp;    // Indicates the type of the constant
        ResStrIndex: Longint;   // If this is a resource string constant, it indicates the
                                // index in the resource table
        Value: Longint;         // In certain cases, contains the value of the constant
        Len: Longint;
      end;

### enumeration symbol

### program symbol

The program symbol type (`TProgramSym`) is used to store the name of the program, which is declared using `program` in the pascal source. This symbol type is currently unused in FreePascal. 

### sys symbol

The `TSysSym` symbol type is used to load indexes into the symbol table of the internal routines which are inlined directly by the compiler. It has a single field, which is the index of the inline routine. 

## Symbol interface

Next chapter: [Type information](<Type_information.md> "Type information")

---

_Source: [https://wiki.freepascal.org/Symbol_entries](https://web.archive.org/web/20190821113412/https://wiki.freepascal.org/Symbol_entries)_
