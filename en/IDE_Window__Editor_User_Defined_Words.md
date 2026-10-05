# IDE Window: Editor User Defined Words

│ **English (en)** │

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "User defined markup". You can open the described dialog in IDE via:  


  * The menu: "Tools" => "Options" / "Editor options" ...
  * The source-editor pop-up menu: "Editor properties" ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 General
  * 2 Terminology
  * 3 Select, Add, Delete List
  * 4 Main Settings
    * 4.1 Name
    * 4.2 Case sensitive / Word bound at start/end
    * 4.3 list of pre-defined terms
  * 5 Key Settings
    * 5.1 Add/Remove/Toggle word or term
    * 5.2 Case sensitive / Word bound at start/end
    * 5.3 Ignore bounds for terms longer than
    * 5.4 Smart match selection bounds
    * 5.5 Add/Remove to all editors
  * 6 Colors
  * 7 Handling of duplicates and matches with overlaps
    * 7.1 Within the same list
    * 7.2 Across different lists



# General

Allows to specify several lists of "terms" that will be highlighted in all editors. The list work highlighter independent. The terms apply to any text in any editor. 

Terms can be added or removed from the lists using key-shortcuts from the editor. Those terms can either apply to all editors or just the current. A lists may be empty at start, and only be used with terms added via shortcut. 

Predefined terms (specified in this option dialog) can be temporarily disabled using the same shortcuts. 

Current Limitations
    Terms added via shortcuts are currently not saved when the IDE is closed. They are also not cleared if the project is changed.

# Terminology

Term
    A word, part of a word, sentence (several words) or any sequence of chars. _Must be a single line_.

Bounds
    The begin or end of a word. A bound can either be a space before/after the matched term, the begin or end of the line, or a "none word" char (e.g. punctuation, quotes or "=")

Note: The matching of bounds, currently does not care for spaces (or other none-word chars in the search term. " Abc." will require another none-word char on each end in order to match bounds. 

# Select, Add, Delete List

[![IDE Options User Defined 1.png](https://wiki.freepascal.org/images/c/c2/IDE_Options_User_Defined_1.png)](</File:IDE_Options_User_Defined_1.png>)

Create or remove lists. Select the list to modify. 

# Main Settings

[![IDE Options User Defined 2.png](https://wiki.freepascal.org/images/f/fc/IDE_Options_User_Defined_2.png)](</File:IDE_Options_User_Defined_2.png>)

### Name

Set/Edit a name for the list. The name is used to show the list in the drop down on the toolbar. 

### Case sensitive / Word bound at start/end

These option can be set individual for each term in the list of pre-defined terms. 

  


### list of pre-defined terms

A list of pre-defined terms, that will be highlighted. This list is active, when the IDE is started. Terms in this list can be temporarily disabled using the key-shortcuts. 

See "Handling of duplicates and matches with overlaps" for info on how duplicates are handled. Duplicates are allowed while a list is edited. They will be marked in red. Duplicates are removed, if you select another list, or close the option dialog. 

# Key Settings

[![IDE Options User Defined 3.png](https://wiki.freepascal.org/images/f/f8/IDE_Options_User_Defined_3.png)](</File:IDE_Options_User_Defined_3.png>)

### Add/Remove/Toggle word or term

Adds or remove a term. 

  * Add



    If there is no text-selection, then it will use the word at caret as the term to add, if the caret is not at a word then it will do nothing.
    If there is a selection, it will use the text of the selection. If the selection exceeds one line, then it will do nothing.
      

    Terms will be added with the case/bounds settings specified.
    However if the term to be added is in the list of pre-defined terms, then it will check if the term was previously disabled. If it was, it will be enabled again. The pre-defined term will keep it's own case/bound settings. If the pre-defined term was not disabled nothing happens. Therefore it is not possible to add a pre-defined term with new case/bounds (e.g. if pre-defined is case sensitive, but key shortcut would add without case sensitivity, the pre-defined term can not be extended to no case sensitivity)

  * Remove



    Will check if a highlight exists at the caret (does not take selection into account). If found the term will be removed from the list. Pre-defined terms will be disabled, so they remember their case/bound settings.

  * Toggle



    Will remove if possible, otherwise add

Note: you may be able to add a term, that does not exist in the document, and will not cause a highlight. This happens if you set, that bounds must be matched if adding from selection, and then select some part of a word excluding it's bounds from the selection. 

If a term is added to several lists, all highlights apply. If the lists change the same attribute, and the changes can not be mixed (e.g. both change foreground color) then the last lists settings apply. If a term is in 2 lists, that both "invert" underline, then nothing happens: "invert" twice means no change. 

### Case sensitive / Word bound at start/end

Defines how terms are added with the key shortcuts 

### Ignore bounds for terms longer than

Only applies, if any bounds are selected. If a word (at caret) or the selection is longer than specified, then the requirement to match bounds will not be applied. 

A value of 0 means indefinite length. That is bounds will always be applied as specified. 

### Smart match selection bounds

Only applies, if all of these apply: 

    \- any bounds are selected
    \- a new term is added from selection
    \- bounds are not cancelled by "length of term"

Bounds will only apply if the selection starts/ends at a boundary. This test is performed individually for each site of the term (start/end). 

Example for settings: 

    \- Both bounds are requested (checkboxes ticked)
    \- Bound-off by length is off (set to 0)
    \- Smart bounds on

Select the Text "self" in "foo myself foo". 

The end of the selection is at a boundary, so the new added term "self" will be required to match bounds at the end. But the start of selection is not at a boundary. It is in the middle of a word. Therefore (despite the checkbox for bounds a start being ticked), the new term will not be required to match bounds at start. 

"self" will be matched in "foo self foo", "foo yourself foo". But it will not be matched in "foo selfish foo". 

### Add/Remove to all editors

Keys will affect the current editor or all editors. 

# Colors

Colors to be used for highlighting. 

Currently the colors are the same for all color-schemes. 

# Handling of duplicates and matches with overlaps

## Within the same list

## Across different lists

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_User_Defined_Words](https://web.archive.org/web/20240121090204/https://wiki.freepascal.org/IDE_Window%3A_Editor_User_Defined_Words)_
