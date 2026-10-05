# IDE Window: Find Unused Units

│ **[Deutsch (de)](</IDE_Window:_Find_Unused_Units/de> "IDE Window: Find Unused Units/de")** │  **English (en)** │    
****

## Contents

  * 1 IDE Dialog: Find unused units
    * 1.1 How does it work
    * 1.2 What is it good for
    * 1.3 Limitations
    * 1.4 How to reach this dialog



# IDE Dialog: Find unused units

## How does it work

The function scans all used units of the current unit (all uses sections, not recursively) and checks if any interface definition of these units is mentioned in the current unit. It does no overload checks. It also checks if the used units has initialization/finalization sections. 

The units used in the interface uses section are listed under 'Interfaces'. The units used in the implementation uses section are listed under 'Implementation'. 

You can select a set of units and let the IDE remove them from the uses sections of the current unit. 

Units with no initialization/finalization section have an icon with a book. It is safe to remove them. 

Units with an initialization/implementation section have a battery like icon. You must decide if they are needed. 

## What is it good for

The compiler normally gives a hint if a unit is not used. But some units have an initialization section. For example the 'graphics' unit of the LCL registers a table of color names. The compiler therefore marks graphics as 'used' even though you might never use the color names. This dialog will show the difference and let the developer decide. 

## Limitations

  * The function does not check if an identifier was overloaded/redefined and thus is not really used. So it can mark an unit as needed, even though it is not.
  * It does not evaluate expressions. So _Something.Items.DoSomething_ are treated as three different identifiers. So it can mark an unit as needed, even though it is not.
  * It only checks the current unit, not all units of a package or project.
  * Available since svn revision 19290 (lazarus 0.9.27).



## How to reach this dialog

This dialog is available via source editor popup menu / Refactoring / Show unused units. You can also set a shortcut via the editor options / key mapping / Codetools commands / Remove unused units. 

It is only available for pascal units.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Find_Unused_Units](https://web.archive.org/web/20240526091046/https://wiki.freepascal.org/IDE_Window%3A_Find_Unused_Units)_
