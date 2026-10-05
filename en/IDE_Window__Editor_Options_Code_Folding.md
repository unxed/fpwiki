# IDE Window: Editor Options Code Folding

│ **English (en)** │    
****

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Code Folding". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__Options.md> "IDE Window: Options") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 About
  * 2 Settings
    * 2.1 Code folding
    * 2.2 Language specific options
  * 3 Usage
    * 3.1 Using the Mouse
    * 3.2 Using the Context Menu
    * 3.3 Using Keyboard
  * 4 About {%Region}



# About

Code folding allows certain structure or blocks of text to be "collapsed". This will hide all but the first line of the block, and the first line will be marked with the folded-symbol "[...]" at the end of that line. 

Code folding is done by using a special area in the Gutter which displays the code folding tree. This tree has a node corresponding to each line of text at which a foldable region starts. 

  * [-] denotes the beginning of an unfolded foldable block, also termed an expanded block. Clicking on the [-] symbol will fold (collapse) this block.
  * [+] denotes the beginning of a folded foldable block, i.e. a collapsed block in which all but the first line is already folded (hidden). Clicking on the [+] symbol will unfold (expand) this block.
  * | A vertical line is shown to the left of all foldable lines except the top line of such a foldable region. The vertical line begins immediately below the [+] sign and continues to the last line of the foldable block. On the last line it displays a tiny edge.



Foldable blocks can be nested or overlapped. 

# Settings

## Code folding

The checkbox at the top enables or disables code folding globally in all source code windows. If disabled, folding functionality is removed, and the Gutter no longer displays a fold tree. 

## Language specific options

Currently code folding is only supported for source code editors which are showing Object Pascal files (*.pas, *.pp, *.lpr etc.), HTML, XML, standard text diff format files, or form files created by the Designer (usually named *.lfm or *.dfm). For the editor to know which kind of lines and blocks of Pascal code should be foldable you have to specify what you want. At first installation, the Lazarus IDE sets a few of the following as defaults, but all are optional. 

Select (tick) the Pascal sections you want to be foldable. 

Procedure
    Folds entire procedures or functions
Var/Type (local)
    Folds any var, type or const declaration block that is local to a procedure
Begin/End (procedure)
    Folds the begin-end block of a procedure or function. This is the outermost block (the one enclosing the whole procedure)
Begin/End (nested)
    Folds any other begin-end block
Repeat
    
Case
    
Try
    Folds any entire Try-finally/except-end block
Except/Finally
    Lets you fold any code between the "except" or "finally" keyword and the closing "end" of the "try" block
Asm
    
Program
    Folds the entire code up to the "end." statement. This applies to program files (.lpr, .dpr) not to units.
unit
    Folds the entire unit up to the "end."
unit section
    Folds any interface, implementation, initialization or finalization block
uses
    If your uses clause goes over more than one line, you can fold it with this option
Var/Type (global)
    Folds any var, type or const declaration block not local to a procedure/function/method
Class/Object
    
public/private
    
Record
    
Nested comments
    
{$IFDEF}
    Folds any IFDEF-ELSE-ENDIF
{%Region}
    This lets you define your own blocks, by inserting this special IDE directive into your code. It needs a matching {%Endregion} directive later at the appropriate point. IDE directives are special comments recognised by the Lazarus IDE, but ignored by the compiler. As with standard Pascal syntax the character case of "region" and "endregion" does not matter. However, to be recognised as a fold directive there can be no spaces in the directive ({%ReGiOn} works, but {% Region} does not).

# Usage

## Using the Mouse

You can click the left mouse buttons on the [+] and [-] symbols to fold/unfold code. 

If you have nested nodes a click on a [+]/[-] node will only fold the node starting on this line. Outer or inner nodes are not affected. Inner nodes that were folded will keep their original state. 

Sometimes you have more than one node starting on the same line. For Example 
    
    
     {$IFDEF WithFoo} for a := i to 10 do begin
    

In this case you may want to fold the nodes individually. 

Note: If any of the nodes on the line is folded, the default action is to unfold, and the Gutter shows a [+] sign (even though there may be more nodes that can still be folded). 

The default mouse settings allow you to change various mouse/key combinations. Note that any clicks apply only to folds starting on this line. Nested folds starting on a different line must be folded by clicking the symbol on their line. 

Left Click on [-]
    Fold **one** node starting on this line. This will fold the node which starts closest to the end of the line ("begin" in the example above)
Ctrl-Left Click on [-]
    Fold **all** nodes starting on this line.

Alt-Left Click on [-]/[+]
    Folds **one** node on this line. Each click will fold the next node traversing from the node starting closest to the end of the line to the one closest to the start of the line. This can be used to fold more than one node, because it always folds even if the gutter has a[+] sign.
Middle Click on [-] or [+]
    The same as alt-Left, but saves you having to pressing Ctrl.

Left Click on [+]
    Unfold **one** Node starting on this line. It will unfold the (folded) node closest to the start of the line. Repeated clicks unfold further nodes (if any)
Ctrl-Left Click on [+]
    Unfold **all** nodes starting on this line.

**Why are nodes folded from the end towards the start of the line?** If you have several nodes nested (such as two {$ifdef}s, or two repeat...until blocks) then the inner node must end first (because it probably has fewer lines). This inner node is the one that starts second on the line 
    
    
     Repeat {<<outer}   Repeat {<<inner}
       // do something
       Until InnerFlag = true;
     Until OuterFlag = true;
    

If you folded the outer (first) "repeat" node first, the inner one would be hidden too. 

However, this nesting is not always the case if you mix {$ifdef} {%region} and Pascal language nodes. If you need more control you can use the context menu, or see the [Advanced Mouse options](<IDE_Window__EditorMouseOptionsAdvanced.md> "IDE Window: EditorMouseOptionsAdvanced"). 

  
Notes: 

  * Mouse middle clicks are unavailable, if the [Mouse settings](<IDE_Window__Editor_Options_Mouse.md> "IDE Window: Editor Options Mouse") for the middle button are set to "ignore"
  * You can read [Advanced Mouse options](<IDE_Window__EditorMouseOptionsAdvanced.md> "IDE Window: EditorMouseOptionsAdvanced") to see how to add further mouse settings, or change existing mouse settings.



## Using the Context Menu

The Fold-Tree has a special context menu (accessed via a right mouse button click) which allows you to select any node starting on the current line to fold or unfold. 

It also shows all nodes that include the current line (all nodes that can hide the current line) and lets you fold them. 

## Using Keyboard

Bindings for the Keyboard can be changed in the Options dialog (Tools -> Options...), under Editor/"Key Mappings". There is a section "Text Folding commands". 

The defaults are 

  * Alt+⇧ Shift+\- (minus): Fold at the current line. Or if no fold node starts on this line, fold at previous line with a fold node.
  * Alt+⇧ Shift+\+ (plus): Unfold at the current line
  * Alt+⇧ Shift+0: Unfold all
  * Alt+⇧ Shift+1..9: Fold at the level corresponding with the number



    only act on Pascal fold nodes (IFDEF and REGION are ignored)
    e.g Alt+⇧ Shift+2 will fold all nodes that are inside ONE outer fold node
    Alt+⇧ Shift+1 will fold everything

# About {%Region}
    
    
     {%region}
     // some code
     {%endregion}
      
    

This directive lets you fold arbitrary blocks of code or text. It also accepts {$region}, but this will give a compiler warning. 

{%region} is not recognized if it occurs inside a comment or string, so 
    
    
     a := ' {%region} ';
    

does not start a foldable block 

You can specify 
    
    
     {%region /fold}
    

if you want the fold to start out collapsed on first loading the file.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Code_Folding](https://web.archive.org/web/20190611061525/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_Code_Folding)_
