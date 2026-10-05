# IDE UI Consolidation

Lazarus IDE has many great features aimed to let developers navigate and edit code faster and easier. 

Unfortunately, since those features were implemented by different authors over long period of time, they are distributed, with partial duplication, across various (and sometimes obscure) dialogs, menu items and windows. 

Consolidating IDE user interface would simplify feature discovery and usage. 

This page is WIP of a proposal for UI consolidation via merging some windows and dialogs. A timeline for this merge is not determined, and should not be rushed. 

When/if it is decided to merge some windows/dialogs, it is important not to loose any features or convenience of originals. 

## Contents

  * 1 Procedure List/Code Explorer
  * 2 Find unused units/Unit dependencies
  * 3 Unit List/Project Inspector
  * 4 Find in Files/Messages/Code Observer
  * 5 Watches/Local variables/Registers
  * 6 Compiler options/Project options



## Procedure List/Code Explorer

Both Procedure List and Code Explorer are intended for quick code navigation inside the current unit based on the list of structural elements of code. Procedure List can navigate to procedures/functions only, while Code Explorer has a more general approach. Procedure List has many convenience features absent in Code Explorer. 

Procedure List should be merged into Code Explorer. 

Following changes to Code Explorer are required: 

  * Add option to jump either to interface or implementation section
  * Add option to hide after the jump
  * Replace search with filtering
  * Add option to filter items starting with/containing search text
  * Add shortcut to open/activate Code Explorer and focus search box
  * Add options to show/hide and expand/collapse all kinds of nodes by default. Currently there are show/hide options for only some kinds of nodes.



## Find unused units/Unit dependencies

TODO 

## Unit List/Project Inspector

Both Unit List and Project Inspector are intended to navigate between units of current project. Project Inspector has also other features, while Unit List has a few convenience features absent in Project Inspector. 

Unit List should be merged into Project Inspector 

## Find in Files/Messages/Code Observer

TODO/Controversial 

## Watches/Local variables/Registers

All three are a lists of name/value pairs used at debug time. 

They should be merged together in a single list with appropriate filtering capabilities. 

## Compiler options/Project options

TODO

---

_Source: [https://wiki.freepascal.org/IDE_UI_Consolidation](https://web.archive.org/web/20240226045238/https://wiki.freepascal.org/IDE_UI_Consolidation)_
