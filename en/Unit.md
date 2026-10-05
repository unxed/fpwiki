# Unit

│ **[Deutsch (de)](</Unit/de> "Unit/de")** │  **English (en)** │  **[español (es)](</Unit/es> "Unit/es")** │  **[suomi (fi)](</Unit/fi> "Unit/fi")** │  **[français (fr)](</Unit/fr> "Unit/fr")** │  **[português (pt)](</Unit/pt> "Unit/pt")** │  **[русский (ru)](<../ru/Unit.md> "Unit/ru")** │    
****

A `unit` is a [source code](<Source_code.md> "Source code") file (or the [binary](<Binary.md> "Binary") compiled from that [file](</File> "File")) which was written using the [Pascal](<Pascal.md> "Pascal") programming language, and that is designed to be a single module in an [application](<Application.md> "Application") or an [object module](<Object_module.md> "Object module"). `Unit` is a [reserved word](<Reserved_word.md> "Reserved word"). It must appear before anything in the unit except comments. 

## Contents

  * 1 Purpose
  * 2 Format
    * 2.1 Detailed unit example
  * 3 Unit precedence
  * 4 See also



## Purpose

A unit may be used where functionality needs to be provided to an application program or to other units. It allows writing code that performs that functionality once and have it used in many places. This can reduce the possibility of errors and increase code reusage. 

A binary unit may be used where a unit author wishes to provide certain functionality to be used in a Pascal [program](<Program.md> "Program") but does not wish to provide the source code that performs that functionality. 

Units were also used on older versions of Pascal when it was necessary on computers with limited resources to be able to load [routines](<Routine.md> "Routine") as needed rather than keeping every routine of the [executable program](<Executable_program.md> "Executable program") in memory all of the time. 

A unit or program that needs to access the [procedures](<Procedure.md> "Procedure") and [data types](<Data_type.md> "Data type") in another unit must specify the units it needs to access them in a [`uses` statement](<Uses.md> "Uses") (but linking is done without the need to write a makefile as in C). 

A unit may also be used to declare a series of global [constants](<Const.md> "Const") or [variables](<Global_variables.md> "Global variables") for use by the entire application, without actually containing any executable code. This is similar to the `common` [keyword](<Keyword.md> "Keyword") in the [Fortran](<Fortran.md> "Fortran") programming language. 

## Format

A unit is defined with the `unit` keyword followed by the unit-identifier. The unit-identifier (in the following example the unit's name is “minimalunit”) should match the filename it is written in. A minimal working example (which does nothing) is: 
    
    
    unit minimalunit;
    interface
    	// here comes stuff that the unit publicly offers
    implementation
    	// here comes the implementation of offered stuff and
    	// optionally internal stuff (only known in the unit)
    end.
    

where the part after [`interface` corresponds](<Interface.md> "Interface") to the `public`-part of other languages and [`implementation`](<Implementation.md> "Implementation") does so to `private`. 

A more advanced basic structure is: 
    
    
    unit advancedunit;
    interface
    
    implementation
    
    initialization
    	// here may be placed code that is
    	// executed as the unit gets loaded
    
    finalization
    	// code executed at program end
    
    end.
    

The optional [`initialization`](<Initialization.md> "Initialization") and [`finalization`](<Finalization.md> "Finalization") blocks may be followed by code that is executed on program start/end. 

### Detailed unit example

A step-by-step example: 
    
    
    unit randomunit;
    // this unit does something
    
    // public  - - - - - - - - - - - - - - - - - - - - - - - - -
    interface
    
    type
    	// the type TRandomNumber gets globally known
    	// since it is included somewhere (uses-statement)
    	TRandomNumber = integer;
    
    // of course the const- and var-sections are possible, too
    
    // a list of procedure/function signatures makes
    // them usable from outside of the unit
    function getRandomNumber(): TRandomNumber;
    
    // an implementation of a function/procedure
    // must not be in the interface-part
    
    // private - - - - - - - - - - - - - - - - - - - - - - - - -
    implementation
    
    var
    	// var in private-part
    	// => only modifiable inside from this unit
    	chosenRandomNumber: TRandomNumber;
    
    function getRandomNumber(): TRandomNumber;
    begin
    	// return value
    	getRandomNumber := chosenRandomNumber;
    end;
    
    // initialization is the part executed
    // when the unit is loaded/included
    initialization
    begin
    	// choose our random number
    	chosenRandomNumber := 3;
    	// chosen by fair-dice-roll
    	// guaranteed to be random
    end;
    
    // finalization is executed at program end
    finalization
    begin
    	// this unit says 'bye' at program halt
    	writeln('bye');
    end;
    
    // initialization and finalization
    // are executed at most _once_
    // during the entire runtime of a program
    end.
    

To include a unit just use the `uses`-statement. 
    
    
    program chooseNextCandidate;
    uses
    	// include a unit
    	randomunit;
    
    begin
    	writeln('next candidate: no. ' + getRandomNumber());
    end.
    

When compiling, FPC checks this program for unit dependencies. It has to be able to find the unit “randomunit”. 

The simplest way to satisfy this is to have a unit whose name matches the file name it is written in (appending `.pas` is OK and encouraged). The unit file may be in the same directory where the program source is in or in the unit path FPC looks for units. 

## Unit precedence

When multiple units are described in a use clause, conflicts can occur with identifiers (procedures, types, functions etc.) that have the same name in multiple units. In FreePascal, the last unit “wins” and provides the code for the unit. 

If you want to achieve a different behavior, you can explicitly prepend `_unitname._ identifier` to specify the unit you want to use, or reorder the units. The former is often the clearest option. 
    
    
    unit interestCalculations;
    
    interface
    
    type
    	basisType = currency;
    
    implementation
    
    end.
    

Specifying the unit of declaration explicitly: 
    
    
    program interestCalculator;
    
    uses
    	interestCalculations;
    
    type
    	// we already loaded a declaration of "basisType"
    	// from the interestCalculations unit, but we're
    	// re-declaring it here again ("shadowing")
    	basisType = extended;
    
    var
    	// last declaration wins: originalCapital is an extended
    	originalCapital: basisType;
    	// specify a scope to use the declaration valid there
    	monthlyMargin: interestCalculations.basisType;
    
    begin
    end.
    

Note, the `interestCalculations` unit will still perform its own calculations with its own `basisType` (here [`currency`](<Currency.md> "Currency")). You can only alter (“shadow”) declarations in the current [scope](<Scope.md> "Scope") (and descending). 

## See also

  * [“Unit scope” in the “reference guide”](<https://www.freepascal.org/docs-html/ref/refsu102.html>) (FPC HTML documentation)
  * [“Compiling a unit” in the “users's guide”](<https://www.freepascal.org/docs-html/user/userse11.html>) (FPC HTML documentation)
  * [§ “Units” in the WikiBook “Pascal Programming”](<https://en.wikibooks.org/wiki/Pascal_Programming/Units>)
  * [Unit not found - How to find units](<Unit_not_found_-_How_to_find_units.md> "Unit not found - How to find units")

---

_Source: [https://wiki.freepascal.org/Unit](https://web.archive.org/web/20250324165127/https://wiki.freepascal.org/Unit)_
