# Symbol tables

│ **English (en)** │

****back to contents[FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 Symbol tables
    * 1.1 Architecture
    * 1.2 The Symbol table object
    * 1.3 Inserting symbols into a symbol table
    * 1.4 Symbol table interface
      * 1.4.1 Routines
        * 1.4.1.1 Search_a_Symtable
        * 1.4.1.2 GetSym
        * 1.4.1.3 GlobalDef
      * 1.4.2 Variables
        * 1.4.2.1 SrSym
        * 1.4.2.2 SrSymTable



# Symbol tables

## Architecture

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Last updated for FPC version 1.0.x

The symbol table contains all definitions for all symbols in the compiler. It also contains all type information for all symbols encountered during the parsing process. All symbols and definitions are streamable, and are used within PPU files to avoid recompiling everything to verify if all symbols are valid. 

There are different types of symbol tables, all of which maybe active at one time or another depending on the context of the parser. 

An architectural overview of the interaction between the symbol tables, the symbol entries and the definition entries is displayed in figure 4.1. 

As can be seen, the symbol table entries in the symbol table are done using the fast hashing algorithm with a hash dictionary. 

## The Symbol table object

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Last updated for FPC version 2.5.1

All symbol tables in the compiler are from this type of object, which contains fields for the total size of the data in the symbol table, and methods to read and write the symbol table into a stream. The start of the linked list of active symbol tables is the **symtablestack** variable. 

<http://www.pjh2.de/fpc/CompilerInternalsFigure04.png>
    
    
    type
      TSymTable = class
        name: pshortstring;               // uppercased realname
        realname  : pshortstring;         // used to generate long symbol names (like symble.name + '.' + sym.name)
        DefList   : TFPObjectList;        // list of definintions
        SymList   : TFPHashObjectList;    // list of symbols
        defowner  : TDefEntry;            // The owner definition. Valud for records, objects and enumerations.
        moduleid  : longint;              // unit index
        refcount  : smallint;             // count of references. if few objects shares the same
                                          // symbol table - they add a new reference instead of 
                                          // full copying the symbols
        currentvisibility : tvisibility;  // current visibility of symtable - used while parsing object members
                                          // to put them into symtable with the correct visibility
        currentlyoptional : boolean;      // used while parsing of objc protocol
        symtablelevel : byte;             // level of symtable, used for nested procedures
        symtabletype  : TSymtabletype;    // Indicates the type of this symbol table (2).
      end;
    

The type of possible symbol tables are shown in the following table: 

Field | Description   
---|---  
abstractsymtable | Default value when the symbol table is created and its type is not defined. Used for debugging purposes   
WithSymTable | All symbols accessed in a with statement   
StaticSymTable | Contains unit implementation or program symbols   
GlobalSymTable | Contains unit interface symbols   
ObjectSymTable | Contains all symbols within an object/class/interface/objc class and other object types statement   
RecordSymTable | Contains all symbols within a record statement   
LocalSymTable | Hold symbols for all local variables of a routine   
ParaSymTable | Holds symbols for all parameters of a routine (the actual parameter declaration symbols)   
Stt_ExceptSymTable | Contains all exception symbols defined in the except block   
exportedmacrosymtable | Holds all exported macros   
localmacrosymtable | Holds all macros currently in scope   
enumsymtable | Contains all enumeration elements symbols of an enumeration   
  
## Inserting symbols into a symbol table

_(last updated for fpc version 1.0.x)_

To add a symbol into a specific symbol table, that’s symbol table’s Insert method is called, which in turns call the Insert_In_Data method of that symbol. Insert_In_Data, depending on the symbol type, adjusts the alignment and sizes of the data and actually creates the data entry in the correct segment. 

<http://www.pjh2.de/fpc/CompilerInternalsFigure05.png>

## Symbol table interface

_(last updated for fpc version 1.0.x)_

### Routines

#### Search_a_Symtable

Declaration: | `**function** Search_a_Symtable(**const** Symbol: String; SymTableType: TSymTableType): PSym;`  
---|---  
Description: | Search for a symbol Symbol in a specified symbol table SymTableType. Returns NIL if the symbol table is not found, and also if the symbol cannot be found in the desired symbol table.   
  
#### GetSym

Declaration: | `**procedure** GetSym(**const** S: StringId; NotFoundError: Boolean);`  
---|---  
Description: | Search all the active symbol tables for the symbol s,setting the global variable SrSym to the found symbol, or to nil if the symbol was not found. notfounderror should be set to TRUE if the routine must give out an error when the symbol is not found.   
  
#### GlobalDef

Declaration: | `**function** GlobalDef(**const** S: String): PDef;`  
---|---  
Description: | Returns a pointer to the definition of the fully qualified type symbol S, or NIL if not found. Notes: It is fully qualified, in that the symbol system.byte, for example, will be fully resolved to a unit and byte type component The symbol must have a global scope, and it must be a type symbol, otherwise NIL will be returned.   
  
### Variables

#### SrSym

Declaration: | `**var** SrSym: PSym;`  
---|---  
Description: | This points to the symbol entry found, when calling getsym.   
  
#### SrSymTable

Declaration: | `**var** SrSymTable: PSymTable;`  
---|---  
Description: | This points to the symbol table of the symbol SrSym when calling GetSym.   
  
Next chapter: [Symbol entries](<Symbol_entries.md> "Symbol entries")

---

_Source: [https://wiki.freepascal.org/Symbol_tables](https://web.archive.org/web/20221126195412/https://wiki.freepascal.org/Symbol_tables)_
