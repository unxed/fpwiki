# Common problems when converting C header files

│ **English (en)** │

## Contents

  * 1 h2pas reports an error
    * 1.1 h2pas problem: extern "C"
    * 1.2 h2pas problem: Empty macro
    * 1.3 h2pas problem: Implicit array types
    * 1.4 h2pas problem: Makros for 0 pointers
    * 1.5 h2pas problem: function types are silently skipped
  * 2 Test compilation and add tools to beautify the output
    * 2.1 Example: MPICH2
  * 3 Common compiler errors on h2pas output
    * 3.1 Unit name contains the file path
    * 3.2 Missing include files
    * 3.3 Forward type not resolved
    * 3.4 Removing system types
    * 3.5 Empty type/var/const sections
    * 3.6 h2pas forgets to add IFDEFs for function bodies
    * 3.7 Implicit Types
    * 3.8 Array of nothing
    * 3.9 Identifier not found
      * 3.9.1 The identifier is defined in the unit, but after it is used
      * 3.9.2 The identifier is defined in another unit (another .h file)
        * 3.9.2.1 Solution 1: Add the unit to the uses section
        * 3.9.2.2 Solution 2: Merge the two include files
        * 3.9.2.3 Example: MPICH2
      * 3.9.3 The identifier is a pointer to an existing type, but h2pas forgot to add it
      * 3.9.4 The identifier is not defined anywhere
      * 3.9.5 Example: MPICH2
    * 3.10 Illegal expression
      * 3.10.1 Example: MPICH2
  * 4 Common other issues
    * 4.1 The pascal file contains a lot of unneccessary ifdef
      * 4.1.1 Hints about unneeded directives ($IFDEFs, $DEFINEs, ...)
        * 4.1.1.1 Example: MPICH2
    * 4.2 The C header files contain redefinitions
      * 4.2.1 Example: MPICH2
    * 4.3 Alias macros are always translated by h2pas to constants
      * 4.3.1 Example: MPICH2
    * 4.4 h2pas creates functions for constants
    * 4.5 h2pas creates functions for type casts
    * 4.6 Some identifiers are used before they are defined
    * 4.7 The C header files do not contain parameter names
    * 4.8 See also



# h2pas reports an error

Here is a list of common C structures, not recognized by h2pas and how to fix them: 

## h2pas problem: extern "C"

Some header files contain the C++ namespace block: 
    
    
    #if defined(__cplusplus)
    extern "C" {
    #endif
    ...
    #if defined(__cplusplus)
    }
    #endif
    

Fix: Add the **Remove C++ 'extern "C"' lines** or the **Pre H2Pas** tool to the **before h2pas** tools. 

## h2pas problem: Empty macro

Some header files contain empty macros used for further extensions: 
    
    
    #define MPI_FILE_DEFINED
    

Fix: Add the **Remove empty C macros** or the **Pre H2Pas** tool to the **before h2pas** tools. 

## h2pas problem: Implicit array types

C allows implicit arrays in parameter types. For example: 
    
    
    int MPI_Group_range_incl(MPI_Group, int, int [][3], MPI_Group *);
    

The int [][3] is an implicit type, which is not allowed under Pascal. h2pas supports adding pointer types. So, it enough to replace all [] with *. 

Fix: Add the **Replace [] with *** or the **Pre H2Pas** tool to the **before h2pas** tools. 

## h2pas problem: Makros for 0 pointers

Some header files contain typed 0 pointers: 
    
    
    #define MPI_BOTTOM      (void *)0
    

Fix: Add the **Replace macro values 0 pointer like (char *)0 with NULL** or the **Pre H2Pas** tool to the **before h2pas** tools. 

## h2pas problem: function types are silently skipped

C allows to define function types, while pascal only allows pointers to functions. For example: 
    
    
    typedef int (MPI_Comm_copy_attr_function)(MPI_Comm, int, void *, void *,
                                            void *, int *);
    

The C header never uses this definition directly, instead it always uses pointers to such types **MPI_Comm_copy_attr_function*** , for instace: 
    
    
    #define MPI_COMM_NULL_COPY_FN ((MPI_Comm_copy_attr_function*)0)
    

  
The translation can work this way: Make the function type a pointer. That means add a * in front of the definition and prepend a P and remove the * behind all references. For example: 
    
    
    typedef int (*PMPI_Comm_copy_attr_function)(MPI_Comm, int, void *, void *,
                                            void *, int *);
    #define MPI_COMM_NULL_COPY_FN ((PMPI_Comm_copy_attr_function)0)
    

Solution: Add the tool **Convert function types to pointer** or **Pre H2Pas** tool to the before h2pas tools. 

# Test compilation and add tools to beautify the output

When h2pas runs without errors it has created a pascal file. The -i switch defines if it is a unit or an include file. The next step is test compilation. Setup up a test project that uses the new pascal source. Then use the 'Run h2pas and compile' button in the h2pas wizard. 

### Example: MPICH2

Create a new project with Project -> New Project -> Program. Save it a **mpihelloworld**. Change the code to 
    
    
    program MPIHelloWorld;
    
    {$mode objfpc}{$H+}
    {$linklib mpich}
    {$linklib rt}
    
    uses
      {$IFDEF UNIX}pthreads{$ENDIF}, MPI;
    
    begin
      MPI_Init(@argc, @argv);
      writeln('Hello, world.');
      MPI_Finalize;
    end.
    

Add the h2p/mpi.pas to the project. Project -> Add editor file to project. 

# Common compiler errors on h2pas output

Sometimes h2pas does not create valid pascal code. Here are the common problems and how to fix them. 

## Unit name contains the file path

h2pas sometimes add the whole path to the unitname. 

Fix: Add **Replace "unit filename;" with "unit name;"** or the **Post H2Pas** tool to the **After h2pas** tools. 

## Missing include files

h2pas converts #include directives to pascal {$i } directives. If you create a unit for each header file, then you can remove all include directives with the tool **Remove includes** or the **Post H2Pas**. 

## Forward type not resolved

For example: **mpi.pas(26,16) Error: Forward type not resolved "PMPI_Aint"** The error line is often a pointer type to a record: 
    
    
    PMPI_Aint = ^MPI_Aint;
    

h2pas adds PMPI_Aint to give nameless C pointers like *MPI_Aint a name. It adds the pointers to the start of the file. But pascal requires, that the forward definition is in the same **type** section. Sometimes h2pas adds a pointer type, although it already exists. 

Fix: Add the tool **Remove redefined pointer types** or the **Post H2Pas** to the **After h2pas** tools. 

## Removing system types

h2pas adds some system types, like PLongint, which are nowadays part of the system unit. 

Fix: Add **Remove type redefinitons like PLongint** or the **Post H2Pas** tool to the **After h2pas** tools. 

## Empty type/var/const sections

After the above tools removed some variables, types, constants some setions become empty, which is not allowed in pascal. 

Fix: Add **Remove empty type/var/const sections** or the **Post H2Pas** tool to the **After h2pas** tools. 

## h2pas forgets to add IFDEFs for function bodies

h2pas converts macros to functions. But if the macro was in an IFDEF block, h2pas does not add IFDEFs around the function body. 

Fix: Add the tool **Add missing h2pas IFDEFs for function bodies** or the **Post H2Pas**. Add this right in front of the tool **Reduce compiler directives in pascal file**. 

## Implicit Types

C allows implicits types in parameters. For example: _int [][3]_. Pascal does not allow this, so you must give it a name and declare a type. But h2pas does not do this automatically and adds instead the implicit type in a quasi pascal syntax. For example: 
    
    
    int some_func(int [][3]);
    

becomes 
    
    
    function some_func(_para1:array[0..2] of Plongint):longint;cdecl;external;
    

Fix: Luckily there is now a tool to remove these implicit types and add explicit types, it's called **Replace implicit types**. It is also part of the **Post H2Pas** tool. Add it to the _after_ tool list. 

## Array of nothing

Sometimes h2pas converts the C ellipsis parameter type '...' wrongly to 'array of )'. It should be 'array of const)'. 

Fix: Add the tool **Fix open arrays** or the **Post H2Pas** to the after tools. 

## Identifier not found

There are three cases: 

  * The identifier is defined in the unit, but after it is used
  * The identifier is defined in another unit (another .h file)
  * The identifier is a pointer to an existing type, but h2pas forgot to add it
  * The identifier is not defined anywhere



### The identifier is defined in the unit, but after it is used

In pascal forward defined types are only allowed in a type section. For example: 
    
    
    type
      TMyRecord = record
        Next: PMyRecord; // using the forward definition
      end;
      PMyRecord = ^TMyRecord;
    

The below is not allowed, because the forward definition is in another type section: 
    
    
    type
      TMyRecord = record
        Next: PMyRecord; // using the forward definition
      end;
    type
      PMyRecord = ^TMyRecord;
    

Solution: The code must be reordered. Add the tool fix forward definitions- 

### The identifier is defined in another unit (another .h file)

#### Solution 1: Add the unit to the uses section

If the other unit is already using this unit, then you have a circle. A circle dependency is allowed between .h files, but not between pascal units. In this case you must move code between both units or use IFDEFs like the gtk2 bindings or use the below merge function of the h2pas wizard. 

#### Solution 2: Merge the two include files

The h2pas wizard can merge include files into one. For example first.h includes via the c include directive the file second.h. Select the second.h in the wizard and check the 'Merge file' feature. Then the source of second.h' will be appended to first.h before sending the output to h2pas. As always: This does not change the header files. 

  


#### Example: MPICH2

The type MPI_Request is defined in mpi.h and used in mpio.h. Both c header files heavily use each other, so you can not put them into two separated units. Select mpio.h and enable the 'Merge file' function. 

### The identifier is a pointer to an existing type, but h2pas forgot to add it

Solution: Add the tool **Add missing pointer types like PPPChar** or the **Post H2Pas** tool to the **after H2Pas** tools. 

### The identifier is not defined anywhere

Probably you are missing a header file or you are using one with the wrong version or it is private identifier. 

Solution: Search the header file and convert it too. If you can not find it or you don't want to translate this file, you can comment the identifier or replace it. 

### Example: MPICH2

Note: this is just a demonstration. The real solution for the MPICH2 headers is to setup the Defines correct, so that the below example vanishes automatically. 

The mpi.h file defines MPI_File as pointer of ADIOI_FileD, but ADIOI_FileD is not defined public: 
    
    
    typedef struct ADIOI_FileD *MPI_File;
    

The h2pas tool translated this to: 
    
    
    MPI_File = ^ADIOI_FileD;
    

Because ADIOI_FileD is not defined public, it is a private structure, so you can simply replace it with a _Pointer_ : Add to the 'After h2pas' tools a new tool of type 'Search and replace' with the following properties: 

Property | Value   
---|---  
Caption | Replace ADIOI_FileD with Pointer   
Name | ReplaceADIOI_FileDwithPointer   
SearchFor | ADIOI_FileD   
ReplaceWith | Pointer   
Options | [trtMatchCase,trtWholeWord]   
  
## Illegal expression

### Example: MPICH2

Compiler gives _Illegal expression_ on the following statement 
    
    
    const
       MPI_LONG_LONG = MPI_LONG_LONG_INT;
    

The reason is, that h2pas translated the MPI_LONG_LONG_INT constant to a function. 

Solution: Fix h2pas or use a trick: 

Replace the 
    
    
    #define MPI_LONG_LONG   MPI_LONG_LONG_INT
    

with 
    
    
    #define MPI_LONG_LONG      ((MPI_Datatype)0x4c000809)
    

by adding a **Search and replace** tool before h2pas. 

# Common other issues

## The pascal file contains a lot of unneccessary ifdef

h2pas translates even the C #ifdef directives. Many of them are C specific and not needed under FPC and can even make it impossible many tools on this side to explore the code. That's why there is a tool to clean up and disable or remove many unneeded IFDEFs: **Reduce compiler directives in pascal file**. Do not forget to insert the tool **Add missing h2pas IFDEFs for function bodies** in front of it. 

### Hints about unneeded directives ($IFDEFs, $DEFINEs, ...)

You can see the directives in the [Code Explorer](<IDE_Window__Code_Explorer.md> "IDE Window: Code Explorer"). 

The tool will only remove the $IFDEFs and $DEFINEs, that are unneeded on all platforms. That means for example a {$IFDEF CPU386} will not be removed - even if you are currently working on an intel 32 bit compatible machine it will not be removed. This allows to create platform independent bindings. Of course C headers often contain a lot of IFDEFs not needed under FPC (Delphi, Kylix, gpc, ...). 

For example: Many C headers are enclosed in 
    
    
    {$IFNDEF FOO}
    {$DEFINE FOO}
    ...
    {$ENDIF}
    

This trick allows to include a C header file multiple times - like the units under pascal. So, pascal does not need this. Therefore the tool _Reduce compiler directives in pascal file_ and the **Post H2Pas** tool have two properties: _Defines_ and _Undefines_. Click on the _..._ button of the property to edit them. Each line can contain a macroname, that should be (un)defined. Add the line _FOO_ to the _Undefines_. This way the **$IFNDEF FOO** is always true and the tool will remove it. The **$DEFINE FOO** is now unneeded too and will be removed too. 

#### Example: MPICH2

Add the following to the _Undefines_ property: 
    
    
     MPI_INCLUDED
     MPIO_INCLUDE
     NEEDS_MPI_FINT
     MPI_BUILD_PROFILING
     MPICH_SUPPRESS_PROTOTYPES
     MPI_Wtime
     HAVE_MPICH2G2
     FOO
     HAVE_PRAGMA_HP_SEC_DEF
    

  
Add the following to the _Defines_ property: 
    
    
     MPICH2
     MPICH_NEW_MPIIO
    

## The C header files contain redefinitions

This is ok for C compilers, because a type is the same if its definition is the same. But for Pascal each type is a different type. 

Solution: Add the tool **Remove redefinitions in pascal unit** or the **Post H2Pas** to the after h2pas tools. As the name implies, it only works on units, not on include files. And it expect at least a valid pascal syntax. That's why it is important to add this tool **after** the tools that fix the pascal syntax like _Replace "unit filename;" with "unit name;"_ , _Remove empty type/var/const sections_ , _Remove includes_ , _Replace implicit types_ and _Fix open arrays_. 

### Example: MPICH2

The mpi.h contains several redefinitions like MPIIMPL_HAVE_MPI_TYPE_CREATE_DARRAY. They can all be fixed with this tool. 

## Alias macros are always translated by h2pas to constants

h2pas converts macros like 
    
    
    #define A B
    

to 
    
    
    const A = B;
    

which is almost always correct. But some c header contain aliases for types, variables and functions too. 

### Example: MPICH2

The mpi.h contains the macro 
    
    
    #define MPI_LONG_LONG      MPI_LONG_LONG_INT
    

h2pas converts this simply to 
    
    
    const
       MPI_LONG_LONG = MPI_LONG_LONG_INT;
    

but MPI_LONG_LONG_INT is a function. 

  
Solution: Add the tool **Fixes section type of alias definitions in pascal unit** or the **Pre H2Pas** tool to the **Before H2Pas** tools. 

Note: This tool can also fix alias to external functions. For example: 
    
    
    function ExternalFunc1(_para1:longint; _para2:pointer):longint;cdecl;external name 'ExternalFunc1';
    const
      ExternalFuncAlias1 = ExternalFunc1;// should be replaced with full declaration
    

## h2pas creates functions for constants

For example: 
    
    
    function MPI_2COMPLEX : MPI_Datatype;
    begin
      MPI_2COMPLEX:=MPI_Datatype(MPI_DATATYPE_NULL);
    end;
    

All used identifiers are types and constants, so this function can be replaced with a simple constant. This is checked and done by the tool **Replace simple functions with constants** or the **Post H2Pas**. 

## h2pas creates functions for type casts

For example: 
    
    
    function PMPI_Win_f2c(win : longint) : MPI_Win;
    begin
       PMPI_Win_f2c:=MPI_Win(win);
    end;
    

This function can be replaced with a simple type, resulting in a type cast. This is checked and done by the tool **Replace simple functions with type casts** or the **Post H2Pas** tool. 

## Some identifiers are used before they are defined

C allows much more forward definitions than pascal, so the definitions must be topologically sorted. For example: 
    
    
    type
      PMPI_Datatype  = ^MPI_Datatype;
      MPI_Datatype = longint;
    

The pointer PMPI_Datatype must be moved to the same type section as MPI_Datatype. 

Solution: Add the **Fix forward definitions by reordering** or the **Pre H2Pas** tool to the **Before H2Pas** tools. The tool should be put **after** the tools fixing redefinitions and types. At the moment the tool only moves pointer types. ToDo: reorder some more definitions like constants. 


## The C header files do not contain parameter names

For example there is an ugly function without parameter names: 
    
    
    int MPI_Intercomm_create(MPI_Comm, int, MPI_Comm, int, int, MPI_Comm * );
    

The parameter names are defined in the corresponding .c file: 
    
    
    int MPI_Intercomm_create(MPI_Comm local_comm, int local_leader, 
                             MPI_Comm peer_comm, int remote_leader, int tag, 
                             MPI_Comm *newintercomm)
    

Solution: There is no solution yet. Except manual editing or searching for other .h files. Often such .h files were auto generated and there are some nicer header files somewhere on the internet. 

Proposal: Write a clever tool, that searches functions with missing parameter names and the corresponding functions in the .c files with parameter names and improve the .h file. 

## See also

  * [Creating bindings for C libraries](<Creating_bindings_for_C_libraries.md> "Creating bindings for C libraries")
  * [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")
  * [Pascal for C++ users](<Pascal_for_C++_users.md> "Pascal for C++ users")
  * [SWIG](<SWIG.md> "SWIG")

---

_Source: [https://wiki.freepascal.org/Common_problems_when_converting_C_header_files](https://web.archive.org/web/20250516150909/https://wiki.freepascal.org/Common_problems_when_converting_C_header_files)_
