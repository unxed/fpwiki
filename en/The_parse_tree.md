# The parse tree

│ **English (en)** │  **[français (fr)](</The_parse_tree/fr> "The parse tree/fr")** │    
****

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 The parse tree
    * 1.1 Architecture
    * 1.2 Node types
    * 1.3 Node structure fields (node.pas)
      * 1.3.1 TNodeFlag
      * 1.3.2 TLocalSwitches
      * 1.3.3 Additional fields
        * 1.3.3.1 AddN
        * 1.3.3.2 CallParaN
        * 1.3.3.3 AssignN
        * 1.3.3.4 LoadN
        * 1.3.3.5 CallN
        * 1.3.3.6 addrn
        * 1.3.3.7 OrdConstN
        * 1.3.3.8 RealConstN
        * 1.3.3.9 FixConstN
        * 1.3.3.10 FuncRetN
        * 1.3.3.11 SubscriptN
        * 1.3.3.12 RaiseN
        * 1.3.3.13 VecN
        * 1.3.3.14 StringConstN
        * 1.3.3.15 TypeConvN
        * 1.3.3.16 TypeN
        * 1.3.3.17 InlineN
        * 1.3.3.18 ProcInlineN
        * 1.3.3.19 SetConstN
        * 1.3.3.20 LoopN
        * 1.3.3.21 AsmN
        * 1.3.3.22 CaseN
        * 1.3.3.23 LabelN, GotoN
        * 1.3.3.24 WithN
        * 1.3.3.25 OnN
        * 1.3.3.26 ArrayConstructorN



# The parse tree

## Architecture

_(last updated for fpc version 1.0.x)_

The tree is the basis of the compiler. When the compiler parses statements and blocks of code, they are converted to a tree representation. This tree representation is actually a doubly linked list. From this tree the code generation can easily be implemented. 

Assuming that you have the following pascal syntax: 

`x := x * y + (6 shl x);`

The tree structure will be built in memory, where each circle represents an element (a node) in the tree: <http://www.pjh2.de/fpc/CompilerInternalsFigure03.png>

## Node types

_(last updated for fpc version 2.1.1, 2005-06-11)_

The following tree nodes are possible (of type TNodeTyp): 

Tree type definition  | Description   
---|---  
emptynode | No node (returns nil when loading from ppu)   
addn | Represents the + operator   
muln | Represents the * operator   
subn | Represents the - operator   
divn | Represents the div operator   
symdifn | Represents the >< operator   
modn | Represents the mod operator   
assignn | Represents the := operator (assignment)   
loadn | Represents the use of a variabele   
rangen | Represents a range (i.e. 0..9)   
ltn | Represents the < operator   
lten | Represents the <= operator   
gtn | Represents the > operator   
gten | Represents the >= operator   
equaln | Represents the = operator   
unequaln | Represents the <> operator   
inn | Represents the in operator   
orn | Represents the or operator   
xorn | Represents the xor operator   
shrn | Represents the shr operator   
shln | Represents the shl operator   
slashn | Represents the / operator   
andn | Represents the and operator   
subscriptn | Represents a field in an object or record   
derefn | Represents a pointer reference (such as the ^ operator)   
addrn | Represents the @ operator   
ordconstn | Represents an ordinal constant   
typeconvn | Represents a typecast / type conversion   
calln | Represents a routine call   
callparan | Represents a parameter passed to a routine   
realconstn | Represents a floating point constant   
unaryminusn | Represents a sign change (e.g : -)   
asmn | Represents an assembler statement node   
vecn | Represents array indexing   
pointerconstn | Represents a pointer constant   
stringconstn | Represents a string constant   
notn | Represents the not operator   
inlinen | Represents one of the internal routines (writeln,ord,etc.)   
niln | Represents the nil pointer   
errorn | Represents error in parsing this node (used for error detection and correction)   
typen | Represents a type name (i.e typeof(obj))   
setelementn | Represents set elements (i.e : [a..b], [a,b,c]) (nonconstant)   
setconstn | Represents set element constants i.e : [1..9], [1,2,3])   
blockn | Represents a block of statements   
statementn | One statement in a block of nodes   
ifn | Represents an if statement   
breakn | Represents a break statement   
continuen | Represents a continue statement   
whilerepeatn | Represents a while or repeat statement   
forn | Represents a for statement   
exitn | Represents an exit statement   
withn | Represents a with statement   
casen | Represents a case statement   
labeln | Represents a label statement   
goton | Represents a goto statement   
tryexceptn | Represents a try..except statement   
raisen | Represents a raise statement   
tryfinallyn | Represents a try..finally statement   
onn | Represents an on..do statement (in exception code)   
isn | Represents the is operator   
asn | Represents the as typecast operator   
caretn | Represents the ^ operator   
starstarn | Represents the ** operator (exponentiation)   
arrayconstructorn | Represents a construction node for [...] parsing   
arrayconstructorrangen | Range element to allow sets in array construction tree   
tempcreaten | for temps in the result/firstpass   
temprefn | references to temps   
tempdeleten | for temps in the result/firstpass   
addoptn | added for optimizations where we cannot suppress   
nothingn | NOP, Do nothing   
loadvmtaddrn | Load the address of the VMT of a class/object   
guidconstn | A GUID COM Interface constant   
rttin | Rtti information so they can be accessed in result/firstpass   
loadparentfpn | Load the framepointer of the parent for nested procedures   
  
  


## Node structure fields (node.pas)

_(last updated for fpc version 2.1.1, 2005-06-11)_
    
    
    type
      TNode = class
      public
        NodeType: TNodeType;            // type of this node
        BlockType: TBlock_Type;         // type of the current code block, general/const/type
        ExpectLoc: TCGLoc;              // expected location of the result of this node (pass1)
        Location: TLocation;            // the location of the result of this node (pass2)
        Parent: TNode;                  // the parent node of this is node
                                        // this field is set by concattolist  
        Flags: TNodeFlags;              // there are some properties about the node stored
        PpuIdx: Longint;
        RegistersInt,                   // the number of registers needed to evalute the node
        RegistersFpu,
        RegistersMm: Longint;           // must be longint !!!! 
        {$ifdef SUPPORT_MMX}
        RegistersMmx: Longint;
        {$endif SUPPORT_MMX}
        ResultType: TType;
        FileInfo: TFilePosInfo;
        LocalSwitches: TLocalSwitches;
        {$ifdef extdebug}
        MaxFirstPassCount,
        FirstPassCount: Longint;
        {$endif extdebug}
      end;
    

### TNodeFlag

_(last updated for fpc version 2.1.1, 2005-06-11)_

TNodeType  |  | Description   
---|---|---  
nf_swapable | TBinOp | operands can be swapped   
nf_swaped | TBinOp | operands are swapped   
nf_error |  | Set to TRUE if there was an error parsing this node   
nf_pass1_done | general |   
nf_write | general | Node is written to   
nf_isproperty | general | TRUE if this is a property   
nf_typedaddr | TAddrNode |   
nf_no_checkpointer | TDerefNode |   
nf_memindex | TVecNode |   
nf_memseg | TVecNode |   
nf_callunique | TVecNode |   
nf_absolute | TLoadNode |   
nf_is_self | TLoadNode |   
nf_load_self_pointer | TLoadNode |   
nf_is_currency | TAddNode |   
nf_has_pointerdiv | TAddNode |   
nf_concat_string | TAssignmentNode |   
nf_use_strconcat | TAssignmentNode |   
nf_forcevaria | TArrayConstructNode |   
nf_novariaallowed | TArrayConstructNode |   
nf_explicit | TTypeConvNode |   
nf_internal | TTypeConvNode | no warnings/hints generated   
nf_load_procvar | TTypeConvNode |   
nf_inlineconst | TInlineNode |   
nf_get_asm_position | TAsmNode |   
nf_block_with_exit | TBlockNode |   
  
### TLocalSwitches

_(last updated for fpc version 2.1.1, 2005-06-11)_

Gébnération de code 

TLocalSwitches  | Commutateur  | Paramètre  | Description   
---|---|---|---  
cs_check_overflow | {$Q+} | -Co | Le générateur de code devrait émettre du code de contrôle de dépassement   
cs_check_range | {$R+} | -Cr, -CR | Code generator should emit range checking code   
cs_check_object |  | -CR | Code generator should emit code to verify object method call validity   
cs_check_io | {$I+} | -Ci | Code generator should emit I/O checking code   
cs_check_stack | {$S+} | -Ct | Code generator should emit stack checking code   
cs_checkpointer |  | -gc | Code generator should emit pointer checking code   
cs_omitstackframe | N/A |  | Code generator should not emit frame_pointer setup code in entry code (don't used in the compiler)   
cs_do_assertion | {$C+} | -Sa | Code generator supports using the assert inline routine   
cs_generate_rtti | {$M+} |  | Code generator should emit runtime type information   
cs_full_boolean_eval | {$B+} |  | Boolean evalution mode   
cs_typed_const_writable | {$J+} |  | _**todo**_  
cs_allow_enum_calc |  |  | _**todo**_  
  
  
MMX 

TLocalSwitches  | Switch  | Param  | Description   
---|---|---|---  
cs_mmx | {$MMX+} |  | Code generator can use MMX commands   
cs_mmx_saturation | {$SATURATION+} |  | Code generator can use saturated operations (MMX)   
  
Parser 

TLocalSwitches  | Switch  | Param  | Description   
---|---|---|---  
cs_typed_addresses | {$T+} |  | Parser emits typed pointer using the @ operator   
cs_strict_var_strings | {$V+} |  | String types must be identical (same length) to be compatible   
cs_ansistrings | {$H+} | -Sh | Parser creates an ansistring when an unspecified String type is declared instead of the default ShortString   
  
  
MACPAS specific 

TLocalSwitches  | Switch  | Param  | Description   
---|---|---|---  
cs_external_var | {$J+} |  | _**todo**_  
cs_externally_visible | {$Z+} |  | _**todo**_  
  
### Additional fields

(last updated for fpc version 1.0.x) 

Depending on the tree type, some additional fields may be present in the tree node. This section describes these additional fields. Before accessing these additional fields, a check on the treetype should always be done to verify if not reading invalid memory ranges. 

#### AddN

Field  | Description   
---|---  
Use_StrConcat: Boolean; | _Currently unused (use for optimizations in future versions)_  
String_Typ: TStringType; | In the case where the + operator is applied on a string, this field indicates the string type.   
  
#### CallParaN

Field  | Description   
---|---  
Is_Colon_Para : Boolean; | Used for internal routines which can use optional format parameters (using colons). Is set to TRUE if this parameter was preceded by a colon (i.e : :1)   
Exact_Match_Found : Boolean; | Set to TRUE if the parameter type is exactly the same as the one expected by the routine.   
ConvLevel1Found : Boolean; | Set to TRUE if the parameter type requires a level 1 type conversion to conform to the parameter expected by the routine.   
ConvLevel2Found : Boolean; | Set to TRUE if the parameter type requires a level 2 type conversion to conform to the parameter expected by the routine.   
HighTree : pTree; |   
  
#### AssignN

Field  | Description   
---|---  
AssignTyp: TAssignTyp; | Currently unused (Used to be used for C-like assigns)   
Concat_String: Boolean; | Currently unused (use for optimizations in future versions)   
  
#### LoadN

Field  | Description   
---|---  
SymTableEntry: PSym; | Symbol table entry for this symbol   
SymTable: PSymTable; | Symbol table in which this symbol is stored   
Is_Absolute: Boolean; | set to TRUE if this variable is absolute   
Is_First: Boolean; | set to TRUE if this is the first occurrence of the load for this variable (used with the varstate variable for optimizations)   
  
#### CallN

Field  | Description   
---|---  
SymTableProcEntry: PProcSym; | Symbol table entry for this routine   
SymTableProc: PSymTable; | Symbol table associated with a call (object symbol table or routine symbol table)   
ProcDefinition: pAbstractProcDef; | Type definition for this routine   
MethodPointer: pTree; | ?????????   
No_Check: Boolean; | _Currently unused_  
Unit_Specific: Boolean; | set to TRUE if the routine is imported in a unit specific way (for example: system.writeln())   
Return_Value_Used : Boolean | set to TRUE if the routine is a function and that the return value is not used (in extended syntax parsing - $X+)   
Static_Call: Boolean; | _unused_  
  
#### addrn

Field  | Description   
---|---  
ProcVarLoad: Boolean; | Set to TRUE if this is a procedural variable call   
  
#### OrdConstN

Field  | Description   
---|---  
Value: LongInt; | The numeric value of this constant node   
  
#### RealConstN

Field  | Description   
---|---  
Value_Real: Best_Real; | The numeric value of this constant node   
Lab_Real: PAsmLabel; | The assembler label reference to this constant   
  
#### FixConstN

Field  | Description   
---|---  
Value_Fix: LongInt; | The numeric value of this constant node   
  
#### FuncRetN

Field  | Description   
---|---  
FuncRetProcInfo: Pointer; (PProcInfo) | Pointer to procedure information   
RetType: TType; | Indicates the return type of the function   
Is_First_FuncRet: Boolean; |   
  
#### SubscriptN

Field  | Description   
---|---  
vs: pVarSym; | Symbol table entry for this variable (a field of object/class/record)   
  
#### RaiseN

Field  | Description   
---|---  
FrameTree: PTree; | Exception frame tree (code in Raise statement)   
  
#### VecN

Field  | Description   
---|---  
MemIndex: Boolean; | Set to TRUE if Mem[Seg:Ofs] directive is parsed   
MemSeg: Boolean; | Set to TRUE if Mem[Seg:Ofs] directive is parsed   
CallUnique: Boolean; |   
  
#### StringConstN

Field  | Description   
---|---  
Value_Str: PChar; | The constant value of the string   
Length: LongInt; | Length of the string in bytes (or in characters???)   
Lab_Str: PAsmLabel; | The assembler label reference to this constant   
StringType: TStringType; | The string type (short, long, ansi, wide)   
  
#### TypeConvN

Field  | Description   
---|---  
ConvType: TConvertType; | Indicates the conversion type to do   
Explizit: Boolean; | set to TRUE if this was an explicit conversion (with explicit typecast, or calling one of the internal conversion routines)   
  
#### TypeN

Field  | Description   
---|---  
TypeNodeType: PDef; | The type definition for this node   
TypeNodeSym: PTypeSym; | The type symbol information   
  
#### InlineN

Field  | Description   
---|---  
InlineNumber: Byte; | Indicates the internal routine called (Cf. code generator)   
InlineConst: Boolean; | One or more of the parameters to this inline routine call contains constant values   
  
#### ProcInlineN

Inline nodes are created when a routine is declared as being inline. The routine is actually inlined when the following conditions are satisfied: 

It is called within the same module 

The appropriate compiler switch to support inline is activated 

It is a non-method routine (a standard procedure or function) 

Otherwise a normal call is made, ignoring the inline directive. In the case where a routine is inlined, all parameters, return values and local variables of the inlined routine are actually allocated in the stack space of the routine which called the inline routine. 

Field  | Description   
---|---  
InlineTree: PTree; | The complete tree for this inline procedure   
InlineProcsym: PProcSym; | Symbol table entry for this procedure   
RetOffset: LongInt; | Return offset in parent routine stack space   
Para_Offset: LongInt; | Parameter start offset in parent routine stack space   
Para_Size: LongInt; | Parameter size in the parent routine stack space   
  
#### SetConstN

Field  | Description   
---|---  
Value_Set: PConstSet; | The numeric value of this constant node   
Lab_Set: PAsmLabel; | The assembler label reference to this constant   
  
#### LoopN

Field  | Description   
---|---  
|   
  
#### AsmN

Field  | Description   
---|---  
p_Asm: PAasmOutput; | The instruction tree created by the assembler parser   
Object_Preserved: Boolean; | set to FALSE if the Self_Register was modified in the asm statement.   
  
#### CaseN

Field  | Description   
---|---  
Nodes: PCaseRecord; | Tree for each of the possible case in the case statement   
ElseBlock: PTree; | Else statement block tree   
  
#### LabelN, GotoN

Field  | Description   
---|---  
LabelNr: PAsmLabel; | Assembler label associated with this statement   
ExceptionBlock: PTree; | ?   
LabSym: PLabelSym; | Symbol table entry for this label   
  
#### WithN

Field  | Description   
---|---  
WithSymTables: PWithSymTable; |   
TableCount: LongInt; |   
WithReference: PReference; |   
IsLocal: Boolean; |   
  
#### OnN

Field  | Description   
---|---  
ExceptSymTable: PSymTable; |   
ExceptType: PObjectDef; |   
  
#### ArrayConstructorN

Field  | Description   
---|---  
CArgs: Boolean; |   
CArgSwap: Boolean; |   
ForceVaria: Boolean; |   
NoVariaAllowed: Boolean; |   
ConstructorDef: PDef; |   
  
  
Next chapter: [Symbol tables](<Symbol_tables.md> "Symbol tables")

---

_Source: [https://wiki.freepascal.org/The_parse_tree](https://web.archive.org/web/20231002151303/https://wiki.freepascal.org/The_parse_tree)_
