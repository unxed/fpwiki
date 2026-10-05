# Namespaces

Since [FPC 3.0.0](<FPC_New_Features_3.md> "FPC New Features 3.0") unit namespaces with dots are supported. 

## Contents

  * 1 Namespaces in FPC
  * 2 Why?
  * 3 Interested Parties
  * 4 Usage Examples
    * 4.1 Suggestion 1 - "dotted unit name notation"
      * 4.1.1 Pros
      * 4.1.2 Cons
    * 4.2 Suggestion 2 - new keyword & new compiler parameter
      * 4.2.1 Example
      * 4.2.2 Pros
      * 4.2.3 Cons
    * 4.3 Suggestion 3 - directory layout
      * 4.3.1 Pros
      * 4.3.2 Cons
    * 4.4 Mapping Namespace to each search path
      * 4.4.1 Definitions / Proposals
      * 4.4.2 Adding / Creating a Namespace / SearchPath-Alias
      * 4.4.3 The "uses" clause
      * 4.4.4 References in the code
      * 4.4.5 Pros
      * 4.4.6 Cons
    * 4.5 Suggestion 123 - all together
      * 4.5.1 Proposal
      * 4.5.2 Example
      * 4.5.3 Pros
      * 4.5.4 Cons
  * 5 see also



## Namespaces in FPC

This page contains proposals and ideas, which could lead to the a better implementation of namespaces that what Delphi currently has. These ideas may or may not work, that's the whole point of this wiki page. Throw ideas around until we find something that could work. 

## Why?

Namespaces are not perfect, but they do work, and they do greatly reduce the chances for Unit name or Class name conflicts. Other languages and frameworks (Java, .NET, C++ etc) have already seen the problem and introduced namespace support. 

How visible is the unit name conflict problem? Just think, how many projects are there that use the unit names (or would like to use these common names): 
    
    
     constants.pas
     utils.pas
     etc...
    

Extremely common names, and are really good names for what they contain (they describe the purpose of the unit clearly), so why can't we use them in our projects? Namespaces will resolve such problem. 

  * To reduce the chances of conflicting unit names
  * Allow developers to use ideal names for units without the worry of conflicting unit names with the FPC compiler or other component libraries. eg: constants.pas or utils.pas or strutils.pas
  * No need for cryptic 2 or 3 letter prefixes to unit names.



## Interested Parties

  * Graeme Geldenhuys
  * Marcos Douglas



## Usage Examples

Please add any new ideas to the end of the list. 

### Suggestion 1 - "dotted unit name notation"

Use the same style as implemented in Delphi 2009 onwards. 

#### Pros

  * Extends the syntax in Pascal where units are already namespaces (disambiguating two names by prefixing the unit name)
  * Similar to the Ada approach of packages, much more robust than namespaces
  * Delphi compatible



#### Cons

  * The force rather long unit names. eg: freepascal.rtl.classes.pas
  * Unit names now contain a '.' character which doesn't make them truly valid identifiers.
  * Ambiguities could appear, but they are rather easy to overcome with a simple language rule. Object Pascal already have many language rules, this will just be a new one. See the following Mantis report for details: [Mantis report 14439](<http://bugs.freepascal.org/view.php?id=14439>)



### Suggestion 2 - new keyword & new compiler parameter

This idea works similar to Compiler Defines. They can be applied to a unit, or to a project as a whole. Add a new keyword **namespace <value>** to the Object Pascal language that gets added to the _unit_ line of a source file. Also add a new **-namespace= <value>** compiler parameter to apply a namespace to a whole project 

#### Example
    
    
       // per unit
      unit utils namespace companyxyz;
      uses
        ...
      end.
    

...or applied at compile time to all units being compiled. 
    
    
       // our sample unit - as normal, nothing new
       unit utils;
       uses
         ...
       end.
    
    
    
       $ fpc -namespace=companyxyz utils.pas constants.pas ...
    

... if a conflict occurs, then you reference the unit with the full namespace. 
    
    
         uses
            companyxyz.utils;  // because unit names must be valid identifier and thus,
                               // can't contain dots, when the compiler sees this, it knows
                               // companyxyz. is a namespace reference.
    

  


#### Pros

  * Namespaces can be applied per unit or for a whole project. The unit specific namespace takes preference compared to the namespace declared as a compiler parameter.


    
    
     unit buttons namespace fpgui;
     uses
       Classes
      ...
     end.
    

  * Namespaces can be applied via the compiler command line parameter so no need to modify each individual unit.


    
    
     fpc -namespace=fpgui buttons.pas groupbox.pas panel.pas
    

  * Least amount of intrusion to existing code.
  * Unit names still stay short
  * Unit names are still valid identifiers (unlike the "dotted" notation of Delphi).
  * If no explicit namespace is declared in the unit, or per compiler parameter, then in automatically get added to the "global" namespace, and works like FPC and Delphi prior to v2009.
  * The usage of the namespace is only required when you get a unit name conflict. All other times, simply using the unit name as-is will suffice.
  * When a unit references a unit in a uses clause without the namespace, then the following search priority is taken. 
    * "global" (or undefined) namespaces take preference. For example the RTL in FPC will not be compiled with a namespace to get higher priority. So if a project also contains a SysUtils.pas unit, the RTL's SysUtils will take preference, and the developer will have to introduce a project wide namespace.
    * Then units part of a project.
    * Then units defined in a namespace.


    
    
    // our project also has a SysUtils unit, but because RTL already has one, we
    // must define a namespace for it.
    unit SysUtils namespace myproject;
    uses
      ...
    end.
     
    unit mytools;
    uses
      SysUtils,             // from the RTL
      myproject.SysUtils,   // from this project / the myproject namespace.
      Classes;
    

#### Cons

  * In corner cases, some ambiguities could appear (if the developer doesn't understand the preferences of namespaces).
  * The name of a common RTL unit (e.g: sysutils) can be redefined. And while being fully qualified in the uses-clause "myproject.SysUtils", it could occur unqualified in the source (if only the one SysUtils was used): "SysUtils.Foo" or "SysUtils.Exception" (Exception could be redefined in myproject)



    This would be confusing to any reader (other than the original author). As reading "SysUtils" will always trigger the assumption RTL

  * how will the compiler know what file contains myprojects.sysutils ? It will have to index all files in the search path on every compiler start. The Java solution solves at least that, check all classpath roots for a fixed directory. ( for x in classpaths do check(x+'/myproject/sysutils').



### Suggestion 3 - directory layout

Similar to what JAVA does, by using the directory hierarchy to define the namespace. 

#### Pros

  * Keeps the unit names short, while still mapping the full namespace qualified name directly to a path name


  * Only need a single search path to include all packages, since each package is automatically mapped to its own subdirectory



#### Cons

  * I see lots of issues with this idea because FPC supports multiple search and library paths per project. So this is probably the crappiest idea. :-)



### Mapping Namespace to each search path

Each installed "package" (set of units) usually resides in a directory, and has it's own /lib or units folder for pre-compiled ppu. 

This directory is added to the list of search paths. 

#### Definitions / Proposals

  * Terms used:



    

  * Package: A collection of units, grouped in one directory. (Since FPC as no real packages yet)



    Examples: RTL, FCL, LCL, SynEdit, CodeTools, RxLib, ....

  * Notes:



    

  * Unit names:



    It is not encouraged to call a unit Utils. It should still be attempted to use names with a likelihood for uniqueness (such as RxUtils). The described proposal is a last resort to resolve remaining conflicts

  


#### Adding / Creating a Namespace / SearchPath-Alias

    Current Situation; Any package used in a project must be known to FPC at compile time of the project. that is archived by adding an -Fu to either the command-line or fpc.cfg file

  * Proposal 1: Extending -Fu



    Allowing to specify Namespace/SearchPath-Alias together with the Path in the -Fu (or alternative a new directive -FU)
    Since there is no char, that can not already be part of a directory name in a path, -Fu cannot easily be extended, so that an alias would always be detected.
    Using -FU would tell the compiler, that an alias is always expected. The alias always ends at the first occurence of the separator char. e.g -FU LCL : /path/to/lcl (spaces are for readability => real directive, would not have spaces)

  * Proposal 2: Adding a default namespace to the Lazarus *.lpk file (Lazarus would add it to the -FU)



    This would be set by the supplier of the package, e.g when downloading rxlib, the people who are in charg of rxlib, would have set the default to "rxlib"
    This can create conflicts in namespace,. but the user can easily change the default 

    If the user changes the default, and hiscode refers to the changed default, he must change the default on all installations where he uses this.
    Maybe the ability override defaults, in the project settings instead should be discussed? Separate discussin...

  * Proposal 3: Adding a default namespace inside one/each unit of the package (using a namespace directive, after the unit declaration)



    

    Instead of a namespace in -FU or lpk; a namespace can be defined inside the unit(s)
    Because there is no package file, this would be needed in every unit. (alternatively a unit with special name (e.g equal to the last directory name in the path ?)
    Example 

    Unit Utils; Namespace 'RxLib'; [1]
    **Cons: As there currently is no package concept, this would be required for each unit, here would be no way to ensure all units in the path have the same (a consistent) Namespace.**
    **Multiple namespaces in one packages may not be desirable .**

  * Mixing of Proposals:



    

    -Fu (1, 2) and namespace (3) can work in parallel, -FU takes priority.

  * Multiply Namespace: Needed ?


  * Project overrides for conflicting Namespaces: Todo



#### The "uses" clause

  * Proposal 1: using "in"



    

    uses Utils in RxLib; 

    Note ther e are no quotes around the namespace, this disambiguates it from a directory of the same name, present in the project folder.
    uses Utils in RxLib alias RxUtils, utils in LCL alias LCLUtils;
    uses Utils in RxLib as RxUtils, utils in LCL as LCLUtils; 

    alias (or as / to be decided) can be used, if several units of the same name exist across several namespaces, and should all be imported. 

  * If no alias is given the unit is known by it's own name
  * If an alias is given the unit is known by the alias, and the alias only


    No 2 units must resolve to the same name 

  * It may be proposed, that if 2 units Utils exists, both must be aliased. Even though, if only one was aliased, they would have different names already.



    But that might lead to unnecessary changes in a unit if a unit name is already used to distinguish types like in the following example:
    
    
    uses
      Windows, Graphics; // both have a type TBitmap
    
    (...)
    
    var
     b1: TBITMAP; // from Windows
     b2: Graphics.TBitmap; // Note: for this example I assume that the first unit "wins". If FPC handles this different, then please exchange the unit in the uses clause ^^
    

    

    

    

    Assuming there are more usages of "Graphics.TBitmap" in that unit the addition of another Graphics unit would lead to unnecessary changes if both units must be aliased:
    
    
    uses
     Windows, Graphics in LCL, Graphics in SomeGraphicsPkg as SomeGraphics;
     // Note: I just picked one of the syntaxes for this example
    

  * Proposal 2: Using "." (dot)



    

    uses RxLib.Utils;
    uses RxLib.Utils as RxUtils;

  * Proposal 3: Using alternatively "as"/"alias" syntax



    

    uses RxUtils=RxLib.Utils;
    uses RxUtils(RxLib.Utils);

#### References in the code

In the code the unit can only be referenced by it's alias (or it's own name, if it was not aliased. 

**Namespace qualifiers should not be allowed in the code**

Namespace qualifiers in the code can cause ambiguity, since a unit and a name of the same namespace can exist. See [Mantis report 14439](<http://bugs.freepascal.org/view.php?id=14439>)

Namespace qualifiers are not native pascal. Keeping their effect as low as possible (quarantined to the "uses" clause, maybe the "unit" declaration) would be desirable 

#### Pros

#### Cons

### Suggestion 123 - all together

#### Proposal

Use dotted unit names with additional keywords and mapped to directory layout. 

#### Example

E.g. the current classes unit would become freepascal.rtl.classes.pas 

  
But you can also store it in a directory freepascal/rtl/classes.pas or freepascal.rtl/classes.pas to avoid cluttering a directory with a lot of files and long names. 

When using it, you can use a new keyword to not write the full name for every unit. 

E.g. 
    
    
      uses with freepascal.rtl do begin classes, sysutils end, with lazarus.lcl do begin ...  end, ...;
    

or allow multiple uses 
    
    
      with freepascal.rtl do uses classes, syutils;
      with lazarus.lcl do uses ...
      ...
    

#### Pros

  * Combines the pros of each suggestion(see above)



#### Cons

  * uses with needs to be repeated in every unit + some extra keywords (WITH, DO BEGIN.. END). Savings not that great, unless you really need a lot of units from one namespace. But since FPC/Delphi allows multiple classes per unit, this will be not the case as often as in e.g. C# or Java.



## see also

  * [§ “Namespaces: Dotted Units” in the _Free Pascal reference guide_](<https://www.freepascal.org/docs-html/ref/refse112.html>)

---

_Source: [https://wiki.freepascal.org/Namespaces](https://web.archive.org/web/20240304152510/https://wiki.freepascal.org/Namespaces)_
