# IDE Window: EditorMouseOptionsAdvanced

│ **English (en)** │

  * This page is part of [IDE_Window:_Editor_Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options")


  * You can get this dialog via:



    

    Menu / Environment / Editor options / Mouse
    Source editor / popup menu / Editor properties ...

  


## Contents

  * 1 The Context Tree
  * 2 The Click Configuration
    * 2.1 The Button
    * 2.2 The Click Type
    * 2.3 The Click Direction
    * 2.4 Restricting the mouse-up action
    * 2.5 The Modifier Keys
    * 2.6 The Priority
    * 2.7 Opt(ion) field
  * 3 The Action / Behaviour
    * 3.1 No Action Command
    * 3.2 Selection Commands
      * 3.2.1 Block Selection Commands
      * 3.2.2 Item Selection Commands
      * 3.2.3 Drag Selection Command
      * 3.2.4 Quick copy Selection Command
    * 3.3 Context Popup Menu
    * 3.4 Fold Commands
      * 3.4.1 Fold Commands
      * 3.4.2 Unfold Commands
      * 3.4.3 Fold Popup Menu
    * 3.5 Source Link Command
    * 3.6 Breakpoint Command
    * 3.7 IDE-Command
  * 4 The "Other Actions using the same button" Table
  * 5 Current Limitations
  * 6 Pitfalls
    * 6.1 Mixing Down and Up Clicks
    * 6.2 Double to Quad Clicks
  * 7 Useful Examples
    * 7.1 Mouse "Move" versus "Click" on Gutter to select Lines / toggle action
    * 7.2 Line Numbers in Gutter to select Lines
    * 7.3 Fold by clicking on Keyword ("begin")
    * 7.4 Fold current block by clicking on vertical fold line
    * 7.5 Extend "Source Links" to jump Begin/End-pairs
    * 7.6 Change History Jumps for 3 button Mouse (follow back the "Source Link" trail)
      * 7.6.1 With a 3 Button Mouse
    * 7.7 Move to procedure body



# The Context Tree

The context tree is the Treeview just left to the Overview Grid. It currently has the entries: 

  * **Text**
    * **Selection**
  * **Gutter**
    * **Fold** **Tree**
      * **Collapsed** **[+]**
      * **Expanded** **[-]**
    * **Line** **Numbers**



You can define the behaviour of the Mouse-Buttons, dependent on the Context. E.g. clicking "selected text" may have a different action, than clicking unselected text. 

If a click has no Entry in a specific context, it will use the content's parent(s) as fallback. E.g. If you click on "selected text", the configuration of the **Selection** node is searched first. If it has no action for your click, the parent node **Text** is searched. (By default this is used to assign "drag selected text" to the left button) 

The **Gutter** and **Text** node are top level nodes, that do not have fall-backs. But the other nodes do fall back on their parents. 

See also The Priority and Fall-Through in the The Action / Behaviour section. 

# The Click Configuration

When you click a mouse button, the following items are checked to find the desired behaviour/action. 

## The Button

There are individual settings for the **Left** , **Right** and **Middle** mouse Button 

  * Extra Mouse Buttons (Button 4 and 5) work only on Windows



## 

The Click Type

There are individual settings for **Single** , **Double** , **Triple** or **Quad** clicks. 

Please make sure you read the section on Double to Quad Clicks

There also is a setting **Any** , which means the action is executed for any of the clicks. That would mean that a double click would execute the action twice. Or if the first part of the double-click changed the state of the object, the 2nd part would apply to the new state. 

  * Example:



    The left button on the code fold part is configured as **Any**. Each click will toggle between folded and unfolded. Therefore a double click will revert the action of the first.
    Without the **Any** setting, if you accidentally folded the wrong node, you would have to wait before you could click again to unfold it. (Because a quick 2nd click would count as double click, which wouldn't be matched by **Single**)

Notes: 

  * Not all of them are supported on all platforms
  * On Windows **Triple** and **Quad** only work for the left button.



## 

The Click Direction

A mouse click consists of a mouse-**down** and a mouse-**up**. 

Actions can be configured to happen on either of those 2 events. 

You can assign 2 different actions to the 2 events of the same click. They will be executed in the order of the events. However keep in mind that such a configuration may limit the action of the mouse-**down** event. 

Notes: 

  * Some events require the correct Click Direction to work properly: 
    * Context-Menu: On some WidgetSets this will only work reliable on mouse-**up** events
    * Start Selection, Drag Selection: Only works on **down** events, since it relies on the following mouse-movement-while-button-down. (It has a build in "end" at mouse button up)
  * Selecting and Dragging (only if actually done by moving the mouse) capture the mouse-up event and prevent any other Action on the up event.  
This can be used to select/drag if mouse is moved, otherwise fall back to an action configured on mouse up



## 

Restricting the mouse-up action

If an action is set to mouse-up, then it can be restricted to execute only, if the down click was on the same area. 

The following test can be performed: 

  * Match action pos of mouse down:



    This does check if a click at the mouse-down location would trigger the same action.
    In cases like fold, where the [+] on different lines all return the fold-action, yet should be treated as different, this needs to be combined with the "action line". For a click on the current selection of text on the other hand, all lines might be treated the same.

  * Match action line of mouse down:



    The down click occurred on the same line of text

  * Match action button of mouse down:



    The down last click was the same button (no other button has been pressed in the meantime)

  * Match action modifiers of mouse down:



    The down click was done using the same modifier keys

  * Search all action of mouse down:



    If the click is on an element that overrides the actions for the below "context" #The_Context_Tree, check if the action would have been on any of the outer contexts.

  * Continue with next mouse up action:



    If the up click is not executed, search default actions and outer context for an alternative action to be executed. The default if this is not given, is to perform no action for this mouse-up.

## 

The Modifier Keys

Mouse behaviour can be modified by holding the **Shift** , **Alt** or **Ctrl** key (or combinations of them) 

At the time of the click-**down** or **up** the state of all three keys is checked. An action can be defined to: 

  * require one, some or all of the modifier keys pressed (Checkbox ticked)
  * require one, some or all of the modifier keys *not* pressed (Checkbox not ticked)
  * ignore the state of one, some or all of the modifier keys (Checkbox greyed)



For a given combination of Button, Click-Type, Click-Direction there must not be any ambiguity for the modifier keys. Example 

Bad / Ambiguous
    

  * Left, Single, down Click with: Shift required pressed; Alt and Ctrl ignored
  * Left, Single, down Click with: Ctrl required pressed; Alt and Shift ignored



    Now if you do this click, with both of Shift and Ctrl pressed, it would be unclear which of the 2 settings to use
Correct
    

  * Left, Single, down Click with: Shift required pressed; Ctrl required un-pressed; Alt ignored
  * Left, Single, down Click with: Ctrl required pressed; Shift required un-pressed; Alt ignored



If you whish Shift to be preferred (the shift action should be taken, if both modifier keys are pressed) then you must at an extra setting: 

  * Left, Single, down Click with: Ctrl required pressed; Shift required pressed; Alt ignored



  


Default Fallback
    

There is one exception from this rule. If all modifier keys are set to be ignored, the entry becomes the fall-back for the button, click-type, click-dir configuration. This is, if no configuration is found which has at least one modifier key required either pressed or un-pressed, then the fallback will be used. 

Note
    

The Modifier Combinations used to detect the action on mouse down (or up) has nothing to do with the modifier Key, that certain actions use during there mouse-move phase.  
e.g. Dragging uses "ctrl" to copy the dragged text instead of moving it. However the use of "ctrl" for copy indication is during mouse move, so you can set start dragging to rely on any other Modifier key. It is however recommended to set **Ctrl** to greyed, so you can start dragging with the key pressed or un-pressed.  
This is different to using "alt" (or others) for column mode selection: Here the decision is made only at the time of click. Later changes of the "alt" key during mouse-move do not change the selection mode. 

## 

The Priority

It is possible to configure several equal (or overlapping / ambiguous) Clicks in the same Context. The Configuration with the highest Priority (lowest number (0 = top)) wins. 

This can be used for 2 purposes: 

Resolve Ambiguity
    

  1. Left, Single, down Click, **priority** **0** with: Shift, Ctrl required pressed; Alt ignored
  2. Left, Single, down Click, **priority** **1** with: Shift required pressed; Ctrl, Alt ignored



Normally those two would conflict, because if you click with shift and ctrl then both will match. However due to the priority this can be resolved: 

  * Only Shift pressed: Only Config 2 (priority 1) does match. Config 2 is taken.
  * Shift and Ctrl pressed: Both Config do match. Config 1 has higher priority (0), Config 1 is taken



  


Define Actions for Command Fall-through
    

Sometimes a Command can not be executed at the given location. If the Command notices this, it will pass the Click on. The Fall-Through first looks for further Commands in the same Context, but with lower priority. If none are found it will search parent context too. 

  * Not all Commands/Actions support Fall-through
  * "Default Fallback" (modifier Keys) will act like any other entry in the Priority. If A click is matched by a "specific configuration" at priority 2 (low) and a "default fallback" at priority 1 (higher), then it will first be given to the "default fallback", and only if this supports and trigger Fall-through the event may go to the "specific config"
  * See Useful Examples: Extend "Source Links" to jump Begin/End-pairs



## 

Opt(ion) field

This numeric option applies to certain settings only: 

Wheel scroll: 

  * speed lines => amount of lines
  * other speed => percent



and possibly a few others. 

# 

The Action / Behaviour

Each mouse button event as defined above, can trigger some action. The same action can be assigned to more than one event. (IF 2 events have the same behaviour, then there modifier keys are not checked for ambiguity) 

Move Caret checkbox
    With each action you can define, if you whish the caret to be moved. However this only additionally enforces the caret move. If an action implicitly moves the caret, then this will happen, even if you uncheck the this box. (e.g. Selecting a block always moves the caret)

Command options
    Some commands take additional params to refine there behaviour

Implicit mouse up action
    Some events that occur on mouse **down** events implicitly assign a mouse **up** action for the current click.   
An implicit Mouse-Up event may or may not cancel the action/behaviour normally assigned to the mouse up.   
E.g. dragging implicitly drops (inserts) the text when the button is released, it cancel whatever action may have been assigned to the mouse up event.

Fall-Through
    

Some Commands can not always be executed. For Example "Source Link Command" can only be executed, if above a link-able identifier. In cases where a command can not be executed, it may trigger a Fall-Through. This means the Click will behave as if the configuration had not existed, and consequently a lower priority followed by the parent context will be searched.   
Note: Not all commands support this, even if they are not executed. 

## No Action Command

There are 2 uses for this: 

Move Caret
    With "Move Caret" it will set the caret to the mouse location. This will unset an existing selection. It also prevents a new selection from being marked by moving the mouse.   
If it occurs on **down** then the caret will be set to the location the mouse was pressed, never mind where you release the button
Prevent fallback to parent node
    In Context-Subnodes this can prevent a click from executing the parent node(s) action.   
E.g. this is assigned to the FoldTree. It catches clicks that are neither on a [+] or [-] node. Otherwise they fall through to the normal Gutter and toggle a breakpoint.

  


Fall-Through
    Never

## Selection Commands

### Block Selection Commands

There are 3 Selection commands, for the 3 selection modes supported by SynEdit: **Normal** , **Column** or **Full-Lines**. 

Note: All of them move the caret implicitly. 

Each of those can be configured in 2 different _Modes_ : 

Begin
    Starts a new selection . If an old selection exists, it will be disregarded. The new Selections starts at the current mouse location.
Continue
    If an old selection exists, then it will be extend.  
If there was no selection, then a new selection is created, which will start at the position the caret had before the event occurred, and range to the click position.

Those events should be assigned to mouse **down** events, in which case the selection will resize with any following mouse-movement until the mouse button is released. (**Implicit mouse up action**) 

"Begin-Mode" on mouse **up** does nothing, but setting the caret. (Except currently for line mode, which selects the line).  
"Continue-Mode" on mouse **up** will extend the selection 

  


Fall-Through
    Never

### Item Selection Commands

Allow to select the Word, Line or Paragraph at the mouse position. Once selected the selection will not extend, if the mouse moves. 

"Line" supports 2 modes, either including leading/trailing spaces of the line or not. 

Note: All of them move the caret implicitly. 

  


Fall-Through
    Never

### Drag Selection Command

Drags (Copies or moves) the current selection to a new position, following mouse movement. It will be pasted there if the button is released. (**Implicit mouse up action**) 

Independent of the modifier keys pressed at the time of the event occurring (or the configured mod, keys), it will check for the **ctrl** key at the time the dragged text is inserted. If Ctrl is pressed the text will be copied, otherwise moved. 

Note: moves the caret implicitly. 

  


Fall-Through
    If no text is selected. Or if the selection can not be dragged (Column or Line mode selection)

### Quick copy Selection Command

Inserts a copy of the currently selected text at the mouse location. If no text is selected, inserts the content of the clipboard. 

Note: moves the caret implicitly. 

  


Fall-Through
    If no text is selected and the clipboard is empty

## Context Popup Menu

Note: GTK2: PopUp-Menu is recommended for use with ButtonUp only 

  


Fall-Through
    Never

## Fold Commands

Fold commands are commonly assigned to the fold-tree-nodes, but can also be assigned to other context, like the text area. 

The fold-tree has 3 context-nodes 

Fold Tree
    Any area of the fold-tree, this can be [+], [-], |, or empty 

Collapsed
    The [+] only. This means the line has at least one collapsed section. It may have a mix of collapsed/expanded sections
Expanded
    The [-] only. This means the line has at at only expanded sections (at least one)

Note, that if you configure a specific click (e.g. shift-left-down) for either [+] or [-] or both, then most likely you should add the same click with "no action" (or other default) to the parent "Fold Tree" node. Otherwise such a click if not on the [+]/[-] node will fall through to the gutter, and may trigger an action there. 

### Fold Commands

By default only acts on nodes starting on the line at the mouse location. 

The following modes are available: 

One
    Folds the "most inner" unfolded node starting on this line. This is the node that starts at the right most place in the line. Any node already folded is ignored, so if any unfolded node exists, one and exactly one node will be folded.  
This is not necessarily the shortest fold-node (in amount of lines folded), as $region and begin folds can overlap.
All
    Folds all Nodes that start on this line
At Caret
    This only works if configured for the "Text" context. It folds the node which starts under the current mouse location. This allows you to click on a "begin" (or other keyword) to fold it. (Useful if more than one node starts at the line).
Current
    Folds the most inner node containing the current mouse location (the gutter is treated as "at the start of line"). It will search for the first fold-starting keyword (e.g "begin") in front of the current mouse location, whic has a block enclosing the mouse location (end is past mouse location). This may be above the current line.  
If assigned to the gutter (or fold tree) this is always above the current line, unless the line starts with a keyword (no spaces before)

  


Fall-Through
    (subject to change) Only in "At Caret" mode, if no fold starts under the mouse location

### Unfold Commands

Only acts on nodes starting on the line at the mouse location. 

The following modes are available: 

One
    Folds the "most outer" folded node starting on this line. This is the node that starts at the left most place in the line.
All
    Unfolds all nodes that start at the current line

Fall-Through
    Never (subject to change)

### Fold Popup Menu

Note: GTK2: PopUp-Menu is recommended for use with ButtonUp only 

A pop up menu allowing you to see and change folds enclosing the current line 

  


Fall-Through
    Never

## Source Link Command

Trigger jump to implementation. 

The "underline" argument controls, if the curremt link under the mouse is highlighted (by an underline) while the modifier keys are pressed. 

If you assign this command to a click with no modifier keys needed, you would get constant highlights (because the expected modifiers (none) are pressed). In this case you cn switch it off. 

  


Fall-Through
    If no linkable identifier is below the mouse

## Breakpoint Command

Toggle breakpoint on this line 

  


Fall-Through
    Never

## IDE-Command

Select from the same commands that are available via the Keyboard Config. 

If the chosen Command depends on the current caret location, then it is recommended to tick "Move Caret". 

  


Fall-Through
    Never

  


# The "Other Actions using the same button" Table

This table can help you identifying other clicks, that may be executed together with the current click. It lists potential other clicks. The listed clicks are **not** necessarily executed together. 

The list includes clicks from the parent(s) contexts, but not any child contexts. 

The clicks are usually searched in the order indicated in the first column. You need to check: 

  * Clicks in the other direction (up/down)
  * if the Modifier Keys match or could match
  * A click could fall through
  * A click with lesser count (single, double, ...) exists



Usually there are up to two Actions executed. The first matching one from the up and one the first matching one from the down clicks part of the list. Further Actions can happen due to fall through. 

So if your item (highlighted) is not first in the list, or there are actions in the other click-dir (up/down) check carefully, if this matches your intention. 

# Current Limitations

  * On Windows **Triple** and **Quad** pressing work for the left button only.
  * Extra Mouse Buttons (Button 4 and 5) work on Windows and Linux only
  * GTK2: PopUp-Menu is recommended for use with Button-Up only
  * OS-Depended: Some clicks may be used by the Window-Manager, and do not reach the IDE at all



# Pitfalls

## Mixing Down and Up Clicks

A mouse-down action does **not** prevent the mouse up action. This may lead to unexpected additional actions being executed  
Consequential a click-up may be preceeded by unexpected click-down actions. 

  * The mouse-up / mouse-down can be in a parent context node
  * To avoid search in parent nodes, define a "no action" event in the current node



## Double to Quad Clicks

  * On Windows **Triple** and **Quad** only work for the left button.



If you assign actions to double and triple click, you should aware that they always trigger the lesser clicks too. 

This is: Doing a Triple-Click will first do a Single-Click (down and up) with the same Modifier Keys (none, shift, alt, ...), then a double click, then the triple. (At the time of the first click, SynEdit does not know there will be more clicks) 

  * The single click can be in a parent context node



  
You should therefore take care of the actions assigned to those clicks. 

  * With the default (select, select word, select line, select paragraph) this is no problem. Selecting a Line (Triple) is not disturbed by selecting a Word (Double) first.
  * If However you would assign MouseLink to the Single (or Double), and select Word to the Double (or Tripple, one more than the other), this would not work. Because by the time you do your last click, SynEdit already followed the Link (and will not undo this). In fact, if SynEdit followed a Link to an other File, then a new SynEdit became active, and would see your last click as a single click.
  * This is not a bug. This is the correct behaviour



# Useful Examples

Some of the examples have become default, since they were listed here 

## Mouse "Move" versus "Click" on Gutter to select Lines / toggle action

Left click followed by mouse move will do selection, while a Click without move will toggle breakpoints/fold-nodes. 

You can also choose to apply this only to the fold part of the gutter. 

  


Context Node
    Gutter
Configuration
    Left - Any - Down - All modifiers greyed
Action
    Selection - Mode: Begin

  
**Optional:**

    

Context Node
    Gutter
Configuration
    Left - Any - Down - Shift required pressed; Ctrl, Alt required unpressed
Action
    Selection - Mode: Continue

  


Context Node
    Gutter
Configuration
    Left - Any - **Up** \- All modifiers greyed
Action
    Toggle Breakpoint

  


Context Node
    Gutter => Line Numbers
No Entries
    

  
_Remove the Entry_

Context Node
    ~~Gutter~~
Configuration
    ~~Left - Any - Down - All modifiers greyed~~
Action
    ~~No Action~~

If you choose to apply this only to the fold part of the gutter, then instead of removing the above entry set it to "Action: Selection Mode Begin" 

  
_Change_

Context Node
    Gutter => Fold Tree => Collapsed  
Fold Tree => Expanded
Configuration
    Change all to include the checkbox "Act on Mouse Up"

  


## Line Numbers in Gutter to select Lines

You can set the Line number part of the gutter to select text (full lines) instead of toggling a breakpoint. The Rest of the gutter (left to the line numbers) will still do breakpoints. 

Context Node
    Gutter => Line Numbers
Configuration
    Left - Any - Down - All modifiers greyed
Action
    Line Selection - Mode: Begin

Setting Click to "Any" instead of "Single" means that double-clicks do not accidentally toggle breakpoints. 

## Fold by clicking on Keyword ("begin")

Instead of folding a block by clicking on the [-] symbol in the gutter, you can fold a block by clicking on it's start keyword (e.g. "begin" or "$region"). this is useful if more than one block starts on the same line. 

Context Node
    Text
Configuration
    Middle or Right - Single - Down - Ctrl or Alt
Action
    Fold Code - Nodes: At Caret

## Fold current block by clicking on vertical fold line

You can set a click on the vertical line ("|" below the [-] sign in the fold tree) to fold the enclosing node. This may be useful if the [-] is outside the visible area. 

Context Node
    Gutter => Fold Tree
Configuration
    Left - Any - Down - All modifiers greyed
Action
    Fold Code - Nodes: Current Node

This replaces the default "No Action" Entry. The default entry (as will the substitute) prevents Fold-Tree-Gutter clicks to toggle breakpoints. 

## Extend "Source Links" to jump Begin/End-pairs

The Source-Link command is only useful for certain identifiers. But a very similar command exist "Find block other End" (IDE-Command). This can be set as a Fall-Through. In this case the same Click can trigger either of the 2 commands, depending on being above a link, or a begin end pair. 

Since IDE-Commands do not support Fall-Through to a further Action thye must be the last in the order of priorities. The "Source Link Command" does support Fall-Through, so it can be tried first. 

Keep the "Source Link Command" as per default: 

Context Node
    Text
Configuration
    Left - Single - Up - Ctrl required pressed; Shift, Alt required unpressed
Priority
    0 (highest)
Action
    Source Link - Underline: yes

Add a new entry 

Context Node
    Text
Configuration
    Left - Single - Up - Ctrl required pressed; Shift, Alt required unpressed
Priority
    1 (lower)
Action
    IDE-Command - Find block other end

Note: While links will be underlined, Begin/End pairs will not be underlined (but can be clicked) 

## Change History Jumps for 3 button Mouse (follow back the "Source Link" trail)

This is usually assigned to ctrl-h. Source-Links and similar actions keep a history of points visited. You can navigate this history backward and forward. 

By default Lazarus uses the 2 extra Buttons of a 5 button mouse to navigate this jump history. 

(This works only if the mouse is over the text. If you want them to work just anywhere including the Gutter space, you need to add a copy in the Gutter context too. There is currently no context that catches Gutter and Text) 

Steps to change them for a 3 button mouse: 

  * select the "text" node from the context-tree (this includes selection, there is no need to add extra config for selection)
  * Find the 2 entries (look for "extra1", "Extra2" in the button column
  * Double-click each, to edit
  * ctrl-right (or ctrl-middle or alt-right) click the "capture" field. **Ensure you check "act on mouse up"**
  * save changes



That's it 

  


### With a 3 Button Mouse

You may assign this to something like ctrl-right mouse. 

**If you use the right button, then you must use it on button-up. Otherwise you will also get the context-menu**

Context Node
    Text
Configuration
    Right - Single - up - Ctrl Required pressed, Others either greyed, or required un-pressed
Action
    Ide Command - Jump back

## Move to procedure body

See this [[post in the forum](<http://forum.lazarus.freepascal.org/index.php/topic,12672.msg108498.html#msg108498>)]

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_EditorMouseOptionsAdvanced](https://web.archive.org/web/20250327123841/https://wiki.freepascal.org/IDE_Window%3A_EditorMouseOptionsAdvanced)_
