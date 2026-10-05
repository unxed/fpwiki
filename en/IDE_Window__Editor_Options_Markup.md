# IDE Window: Editor Options Markup

│ **English (en)** │

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Markup and Matches". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

  


## Contents

  * 1 Markup and Matches
    * 1.1 Current Word
    * 1.2 Bracket Highlight
    * 1.3 Extended Pascal Keywords



# Markup and Matches

### Current Word

There are a couple of options to control the current-word-markup feature. They are on the "Automatic feature" page of the editor option (plus color settings on the color page) 

Note: If you have a block selected, the selected block will be used as term to highlight, rather than the word under caret. Some settings are only for block, others only for current-word 

1) You can change the time after which highlighting kicks in. You can also disable the Timer completely, in which case you can still toggle Highlighting by Keyboard 

2) Toggle by Keyboard (default Alt-M) You can toggle the highlight to any chosen word, or selection by pressing Alt-M while at the word, or while having selected the desired block. Pressing Alt-M again will set the "fixed" word to the word at your new location. To untoggle, press Alt-M twice at the same location (The exact behaviour is: If your current selected block, or (if no block selected) the current word under cursor is the same term as the word you toggled-fixed, this will clear the fixed word. Otherwise the fixed word will be set) 

3) You can ignore Keywords. This is a hardcoded list of Keywords, and will ignore them even if they are used in a context where they would not be seen as Keyword. The list comes from the highlighter, so this only works together with the highlighter. 

4) for current-word only: You can choose to only highlight exact matches(match word boundaries) instead of any occurrence including occurrences in other words. By Default this applies to words shorter than 3 chars. 

For example: Set option to true and field to 3. 

If the word at caret is the 3 letter word 'laz' then all 'laz' will be marked, but not 'lazarus' or 'blaz'. 

If the word at caret is the 4 letter word 'laza' then all words including 'laza' will be marked, such as 'lazarus'. 

  
5) blocks only: by default leading/trailing spaces are ignored/stripped from the search term 

To disable the feature completely: Set all colors (FG,. BG and Frame) to default (check the tickbox); make sure no Styles (bold, italic) are selected. 

This will be recognized, and the Editor is clever enough and will not even trigger the search for other words. 

### Bracket Highlight

If the cursor is behind or in front of a bracket, highlight matching bracket 

### Extended Pascal Keywords

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Markup](https://web.archive.org/web/20230603065131/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Markup)_
