# Type information

│ **English (en)** │  **[français (fr)](</Type_information/fr> "Type information/fr")** │    
****

Back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 Type information
    * 1.1 Architecture
    * 1.2 Definition types
      * 1.2.1 base definition (TDef)
      * 1.2.2 file definition (TFileDef)
      * 1.2.3 formal definition (TFormalDef)
      * 1.2.4 forward definition (TForwardDef)
      * 1.2.5 error definition (TErrorDef)
      * 1.2.6 pointer definition (TPointerDef)
      * 1.2.7 object definition (TObjectDef)
      * 1.2.8 class reference definition (TClassRefDef)
      * 1.2.9 array definition (TArrayDef)
      * 1.2.10 record definition (TRecordDef)
      * 1.2.11 ordinal definition (TOrdDef)
      * 1.2.12 float definition (TFloatDef)
      * 1.2.13 abstract procedure definition (tabstractprocdef)
      * 1.2.14 procedural variable definition (TProcVarDef)
      * 1.2.15 procedure definition (TProcDef)
      * 1.2.16 string definition (TStringDef)
      * 1.2.17 enumeration definition (TEnumDef)
      * 1.2.18 set definition (TSetDef)
    * 1.3 Definition interface
      * 1.3.1 routines
        * 1.3.1.1 TDef.Size
        * 1.3.1.2 TDef.Alignment



# Type information

## Architecture

_(last updated for fpc version 1.0.x)_

A type declaration , which is the basis for the symbol table, since inherently everything comes down to a type after parsing is a special structure with two principal fields, which point to a symbol table entry which is the type name, and the actual definition which gives the information on other symbols in the type, the size of the type and other such information. 
    
    
    type
      TType = object
        Sym: PSym;      // Points to the symbol table of this type
        Def: PDef;      // Points to the actual definition of this type
      end;
    

## Definition types

_(last updated for fpc version 1.0.x)_

Definitions represent the type information for all possible symbols which can be encountered by the parser. The definition types are associated with symbols in the symbol table, and are used by the parsing process (among other things) to perform type checking. 

The current possible definition types are enumerated in `TDefType` and can have one of the following symbolic values: 

<http://www.pjh2.de/fpc/CompilerInternalsFigure07.png>

deftype of TDef object | Description   
---|---  
AbstractDef |   
ArrayDef | array type definition   
RecordDef | record type definition   
PointerDef | pointer type definition   
OrdDef | ordinal (numeric value) type definition   
StringDef | string type definition   
EnumDef | enumeration type definition   
ProcDef | procedure type definition   
ObjectDef | object or class type definition   
ErrorDef | error definition (empty, used for error recovery)   
FileDef | file type definition   
FormalDef |   
SetDef | set type definition   
ProcVarDef | procedure variable type definition   
FloatDef | floating point type definition   
ClassrefDef |   
ForwardDef |   
  
### base definition (TDef)

All type definitions are based on this object. Therefore all derived object all posess the fields in this object in addition to their own private fields. 
    
    
    type
      PDef = ^TDef;
      TDef = object(TSymTableEntry)
        TypeSym: PTypeSym;                    // Pointer to symbol table entry for this type
                                              // definition
        InitTable_Label: PAsmLabel;           // Label to initialization information (required
                                              // for some complex types)
        Rtti_Label: PAsmLabel;                // Label to the runtime type information.
        NextGlobal: PDef;
        PreviousGlobal: PDef;
        SaveSize: Longint;                    // Size in bytes of the data definition
        DefType: TDefType;                    // Indicates the definition type (see table ??).
        Has_InitTable: Boolean;
        Has_Rtti: Boolean;
        Is_Def_Stab_Written: TDefStabStatus;  // Can be one of the following states :
                                              // (Not_Written, written, Being_Written)
                                              // which indicates if the debug information
                                              // for this type has been defined or not.
         GlobalNb: Longint;                   // Internal stabs debug information type signature
                                              // (each type definition has a numeric
                                              // signature).
      end;
    

### file definition (TFileDef)

The file definition can occur in only some rare instances, when a `file of type` is parsed, a file definition of that type will be created. Furthermore, internally, a definition for a **Text** file type and **untyped** File type are created when the system unit is loaded. These types are always defined when compiling any unit or program. 
    
    
    type
      PFileDef = ^TFileDef;
      TFileDef = object(TDef)
        FileTyp: TFileTyp;        // Indicates what type of file definition it is 
                                  // (text, untyped or typed).
        TypedFileType: TType;     // In the case of a typed file definition, 
                                  // definition of the type of the file
      end;
    

  


### formal definition (TFormalDef)

### forward definition (TForwardDef)

The forward definition is created, when a type is declared before an actual definition exists. This is the case, when, for example `type PMyObject = TMyObject`, while `TMyObject` has yet to be defined. 
    
    
    type
      PForwardDef = ^TForwardDef;
      TForwardDef = object(TDef)
        toSymName: String;         // The symbol name for this forward declaration
                                   // (the actual real definition does not exist yet)
        ForwardPos: TFilePosInfo;  // Indicates file position where this forward
                                   // definition was declared.
      end;
    

  


### error definition (TErrorDef)

This definition is actually an empty definition entry. When the parser encounters an error when parsing a definition instead of putting nothing in the type for a symbol, it puts this entry. This avoids illegal memory accesses later in parsing. 

### pointer definition (TPointerDef)

The pointer definition is used for distinguishing between different types of pointers in the compiler, and are created at each `typename` parsing construct found. 
    
    
    type
      PPointerDef = ^TPointerDef;
      TPointerDef = object(TDef)
        Is_Far: Boolean;         // Used to indicate if this is a far pointer or not 
                                 // (this flag is cpu-specific)
        PointerType: TType;      // This indicates to what type definition this pointer
                                 // points to.
      end;
    

### object definition (TObjectDef)

The object definition is created each time an object declaration is found in the type declaration section. 
    
    
    type
      PObjectDef = ^TObjectDef;
      TObjectDef = object(TDef)
        ChildOf: PObjectDef;                 // This is a pointer to the parent object
                                             // definition. It is set to nil, if
                                             // this object definition has no parent.
        ObjName: PString;                    // This is the object name
        SymTable: PSymTable;                 // This is a pointer to the symbol
                                             // table entries within this object.
        ObjectOptions: TObjectOptions;       // The options for this object, see
                                             // the following table for the possible
                                             // options for the object.
        VMT_Offset: Longint;                 // This is the offset from the start
                                             // of the object image in memory
                                             // where the virtual method table is
                                             // located.
        Writing_Class_Record_Stab: Boolean;
      end;
    

  


Object Options(TObjectOptions) | Description   
---|---  
oo_is_class | This is a delphi styled class declaration, and not a Turbo Pascal object.   
oo_is_forward | This flag is set to indicate that the object has been declared in a type section, but there is no implementation yet.   
oo_has_virtual | This object / class contains virtual methods   
oo_has_private | This object / class contains private fields or methods   
oo_has_protected | This object / class contains protected fields or methods   
oo_has_constructor | This object / class has a constructor method   
oo_has_destructor | This object / class has a destructor method   
oo_has_vmt | This object / class has a virtual method table   
oo_has_msgstr | This object / class contains one or more message handlers   
oo_has_msgint | This object / class contains one or more message handlers   
oo_has_abstract | This object / class contains one or more abstract methods   
oo_can_have_published | the class has runtime type information, i.e. you can publish properties   
oo_cpp_class | the object/class uses an C++ compatible class layout   
oo_interface | this class is a delphi styled interface   
  
### class reference definition (TClassRefDef)

### array definition (TArrayDef)

This definition is created when an array type declaration is parsed. It contains all the information necessary for array type checking and code generation. 
    
    
    type
      PArrayDef = ^TArrayDef;
      TArrayDef = object(TDef)
        IsVariant: Boolean;
        IsConstructor : Boolean;
        RangeNr: Longint;          // Label number associated with the index values
                                   // when range checking is on
        LowRange : Longint;        // The lower index range of the array definition
        HighRange : Longint;       // The higher index range of the array definition
        ElementType : TType;       // The type information for the elements of the array
        RangeType : TType;         // The type information for the index ranges of the array
        IsArrayofConst : Boolean;
      end;
    

### record definition (TRecordDef)

The record definition entry is created each time a record type declaration is parsed. It contains the symbol table to the elements in the record. 
    
    
    type
      PRecordDef = ^TRecordDef;
      TRecordDef = object(TDef)
        SymTable: PSymTable;    // This is a pointer to the symbol table entries within
                                // this record.
      end;
    

### ordinal definition (TOrdDef)

This type definition is the one used for all ordinal values such as char, bytes and other numeric integer type values. Some of the predefined type definitions are automatically created and loaded when the compiler starts. Others are created at compile time, when declared. 
    
    
    type
      POrdDef = ^TOrdDef;
      TOrdDef = object(TDef)
        Low: Longint;        // The minimum value of this ordinal type
        High: Longint;       // The maximum value of this ordinal type
        Typ: TBaseType;      // The type of ordinal value
      end;
    

  


Base ordinal type (TBaseType) | Description   
---|---  
uauto | user defined ordinal type definition   
uvoid | Represents a void return value or node   
uchar | ASCII character (1 byte)   
u8bit | unsigned 8-bit value   
u16bit | unsigned 16-bit value   
u32bit | unsigned 32-bit value   
s16bit | signed 16-bit value   
s32bit | signed 32-bit value   
bool8bit | boolean 8-bit value   
bool16bit | boolean 16-bit value   
bool32bit | boolean 32-bit value   
u64bit | unsigned 64-bit value (not fully supported/tested)   
s64bit | signed 64-bit value   
uwidechar | Currently not supported and unused   
  
  


### float definition (TFloatDef)

This type definition is the one used for all floating point values such as SINGLE, DOUBLE. Some of the predefined type definitions are automatically created and loaded when the compiler starts. 
    
    
    type
      PFloatDef = ^TFloatDef;
      TFloatDef = object(TDef)
        Typ: TFloatType;       // The type of floating point value.
      end;
    

Base floating point type (TFloatType) | Description   
---|---  
s32real | IEEE Single precision floating point value   
s64real | IEEE Double precision floating point value   
s80real | Extended precision floating point value (cpu-specific, usually maps to double)   
s64comp | 63-bit signed value, using 1 bit for sign indication   
f16bit | _Unsupported_  
f32bit | _Unsupported_  
  
### abstract procedure definition (tabstractprocdef)

This is the base of all routine type definitions. This object is abstract, and is not directly used in a useful way. The derived object of this object are used for the actual parsing process. 
    
    
    type
      PAbstractProcDef = ^TAbstractProcDef;
      TAbstractProcDef = object(TDef)
        SymtableLevel: Byte;
        Fpu_Used: Byte;                     // Number of floating point registers
                                            // used in this routine
        RetType: TType;                     // Type information for the return value
                                            // (uvoid if it returns nothing)
        ProcTypeOption: TProcTypeOption;    // Indicates the type of routine it is.
        ProcCallOptions: TProcCallOptions;  // Indicates the calling convention
                                            // of the routine.
        ProcOptions: TProcOptions;          // Indicates general procedure options.
        Para: PLinkedList;                  // This is a linked list of parameters
                                            // (pparaitem list)
      end;
    

Procedure options (TProcTypeOption) | Description   
---|---  
poType_ProgInit | Routine is the program entry point (defined as 'main' in the compiler).   
poType_UnitInit | Routine is the unit initialization code (defined as unitname_init in the compiler   
poType_UnitFinalize | Routine is the unit exit code (defined as unitname_finalize in the compiler)   
poType_Constructor | Routine is an object or class constructor   
poType_Destructor | Routine is an object or class destructor   
poType_Operator | Procedure is an operator   
  
  


call options (TProcCallOptions) | Description   
---|---  
pocall_clearstack | The routine caller clears the stack upon return   
pocall_leftright | Send parameters to routine from left to right   
pocall_cdecl | Passing parameters is done using the GCC alignment scheme, passing parameter values is directly copied into the stack space   
pocall_register | unused (Send parameters via registers)   
pocall_stdcall | Passing parameters is done using GCC alignment scheme, standard GCC registers are saved   
pocall_safecall | Standard GCC registers are saved   
pocall_palmsssyscall | This is a special syscall macro for embedded system   
pocall_system | unused   
pocall_inline | Routine is an inline assembler macro (not a true call)   
pocall_internproc | System unit code generator helper routine   
pocall_internconst | System unit code generator helper macro routine   
  
  


routine options (TProcOptions) | Description   
---|---  
po_classmethod | This is a class method   
po_virtualmethod | This is a virtual method   
po_abstractmethod | This is an abstract method   
po_staticmethod | This is a static method   
po_overridingmethod | This is an overriden method (with po_virtual flag usually)   
po_methodpointer | This is a method pointer (not a normal routine pointer)   
po_containsself | self is passed explicitly as a parameter to the method   
po_interrupt | This routine is an interrupt handler   
po_iocheck | IO checking should be done after a call to the procedure   
po_assembler | The routine is in assembler   
po_msgstr | method for string message handling   
po_msgint | method for int message handling   
po_exports | Routine has export directive   
po_external | Routine is external (in other object or lib)   
po_savestdregs | Routine entry should save all registers used by GCC   
po_saveregisters | Routine entry should save all registers   
po_overload | Routine is declared as being overloaded   
  
### procedural variable definition (TProcVarDef)

This definition is created when a procedure variable type is declared. It gives information on the type of a procedure, and is used when assigning and directly calling a routine through a pointer. 
    
    
    type
      PProcVarDef = ^TProcVarDef;
      TProcVarDef = object(TAbstractProcDef)
      end;
    

### procedure definition (TProcDef)

When a procedure head is parsed, the definition of the routine is created. Thereafter, other fields containing information on the definition of the routine are populated as required. 
    
    
    type
      PProcDef = ^TProcDef;
      TProcDef = object(TAbstractProcDef)
        ForwardDef: Boolean;               // TRUE if this is a forward definition
        InterfaceDef: Boolean;
        ExtNumber: Longint;
        MessageInf: TMessageInf;
        NextOverloaded: PProcDef;
        FileInfo: TFilePosInfo;            // Position in source code for the declaration of
                                           // this routine. Used for error management.
        Localst: PSymTable;                // The local variables symbol table
        Parast: PSymTable;                 // The parameter symbol table
        ProcSym: PProcSym;                 // Points to owner of this definition
        LastRef: PRef;
        DefRef: PRef;
        CrossRef: PRef;
        LastWritten: PRef;
        RefCount: Longint;
        _Class: ProbjectDef;
        Code: Pointer;                     // The actual code for the routine 
                                           // (only for inlined routines)
        UsedRegisters: TRegisterSet;       // The set of registers used in this routine
        HasForward: Boolean;
        Count: Boolean;
        Is_Used: Boolean;
      end;
    

### string definition (TStringDef)

This definition represents all string types as well as derived types. Some of the default string type definitions are loaded when the compiler starts up. Others are created at compile time as they are declared with a specific length type. 
    
    
    type
      PStringDef = ^TStringDef;
      TStringDef = object(TDef)
        String_Typ: TStringType;   // Indicates the string type definition
        Len: Longint;              // This is the maximum length which can have the string
      end;
    

String type (TStringType) | Description   
---|---  
st_default | Depends on current compiler switches, can either be a st_ShortString or st_AnsiString   
st_shortstring | short string (length byte followed by actual ASCII characters (1 byte/char))   
st_longstring | long string (length longint followed by actual ASCII characters (1 byte/char))   
st_ansistring | long string garbage collected (pointer to a length, reference count followed by actual ASCII characters (1 byte/char))   
st_widestring | long string garbage collected (pointer to a length, reference count followed by actual unicode characters (1 word/char (utf16)))   
  
  


### enumeration definition (TEnumDef)

An enumeration definition is created each time an enumeration is declared and parsed. Each element in the enumeration will be added to the enumeration symtable and if $SCOPEDENUMS directive is off then to the unit symtable also. 
    
    
    type
      tenumdef = class(tstoreddef)
        has_jumps: Boolean;   // True if enum has jumps between elements
        minval: aint;         // Value of the first element in the enumeration
        maxval: aint;         // Value of the last element in the enumeration
        BaseDef: tenumdef;    // In the case where the enumeration is a 
                              // subrange of another enumeration or a unique 
                              // enumeration copied from the another, this gives 
                              // information on the base range of the elements
       symtable: TSymtable;   // Enumeration symbol table - stores enumeration elements
      end;
    

### set definition (TSetDef)

This definition is created when a set type construct is parsed (set of declaration). 
    
    
    type
      PSetDef = ^TSetDef;
      TSetDef = object(TDef)
        SetType: TSetType; Indicates the storage type of the set.
        ElementType: TType; Points the type definition and symbol
          table to the elements in the set.
      end;
    

set type (TSetType) | Description   
---|---  
NormSet | Normal set of up to 256 elements (32 byte storage space required)   
SmallSet | Small set of up to 32 elements (4 byte storage space)   
VarSet | Variable number of element set (storage size is dependent on number of elements) (currently unused and unsupported)   
  
  


## Definition interface

_(last updated for fpc version 1.0.x)_

### routines

#### TDef.Size

Declaration: | `**function** TDef.Size: Longint;`  
---|---  
Description: | This method returns the true size of the memory space required in bytes for this type definition (after alignment considerations).   
  
  


#### TDef.Alignment

Declaration: | `**function** TDef.Alignment: Longint;`  
---|---  
Description: | This method returns the alignment of the data for complex types such as records and objects, otherwise returns 0 or 1 (no alignment).   
  
  
Next chapter: [The parser](<The_parser.md> "The parser")

---

_Source: [https://wiki.freepascal.org/Type_information](https://web.archive.org/web/20250310165603/https://wiki.freepascal.org/Type_information)_
