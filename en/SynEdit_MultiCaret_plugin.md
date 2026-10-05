# SynEdit MultiCaret plugin

## Contents

  * 1 General
  * 2 Modes
    * 2.1 Adding Carets
    * 2.2 MultiCaret - Cancel on Caret movements
    * 2.3 MultiCaret - All follow Caret movements
  * 3 Keys
  * 4 Mouse
  * 5 Column mode selecting



# General

The unit SynPluginMultiCaret provides a plugin, that allows to use SynEdit in Multi-Caret mode. 

Animated demo (in IDE) [New IDE features since#Multi_Caret](<New_IDE_features_since.md> "New IDE features since")

Keys and Mouse can be configured on the Component. When used in the IDE this is the setup is managed by the IDE. 

# Modes

When working with multiple carets, the editor can be in different modes. 

This affects what will happen when the caret movement keys (up/down/left/right/home/end/next-or-prev word/...) are used. 

Keystrokes can be defined to activate either "Cancel on Caret movements" or "MultiCaret - All follow Caret movements". Activation can either toggle between the 2 modes, or end the "Adding Carets" mode. 

## Adding Carets

This mode is active when a caret was added (or removed) using a key-combo. Caret-movement keys will affect the main-caret. All other carets stay where they are. 

This mode is ended, when an edit action takes place (insert/delete char/...). By default the mode changes to "All follow Caret movements", But this can be configured. 

  


## MultiCaret - Cancel on Caret movements

The carets can only be used to edit the text at their current location. Any edit action (insert/delete char/...) will take place at all carets. 

Caret-movement (via up/down/left/right/... keys) will end the multi caret mode. 

## MultiCaret - All follow Caret movements

Carets can be used to edit as above. 

Caret-movement (via up/down/left/right/... keys) will move all carets. If 2 or more carets end in the same place they will be merged. 

# Keys

Keys can be defined to: 

  * Set/Remove an extra caret at the position of the main caret.
  * Cancel Multi caret mode
  * Activate "Cancel on Caret movements" or "MultiCaret - All follow Caret movements".



# Mouse

Mouse actions can be defined, to 

  * Add, Remove, Toggle an extra caret at the click position
  * Create carets from a selection (carets are set at the end of each line that is part of the selection)



# Column mode selecting

Column mode selection is usually archived with the alt key. It creates a rectangular selection, covering a text column over several lines. 

Multi caret mode can be activated automatically, if a column mode selection is made. This creates one caret in each line of the selection, on the same side of the selection as the main caret is. 

If an edit action is performed with multiple carets created by column mode selection, then by default "MultiCaret - Cancel on Caret movements" is activated. This can be configured.

---

_Source: [https://wiki.freepascal.org/SynEdit_MultiCaret_plugin](https://web.archive.org/web/20240121090230/https://wiki.freepascal.org/SynEdit_MultiCaret_plugin)_
