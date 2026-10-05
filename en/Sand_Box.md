# Sand Box

│ **English (en)** │  **[русский (ru)](<../ru/Sand_Box.md>)** │

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** You may find the narrated video below on How To Edit a Wiki Page useful.

[Narrated Video](<https://www.youtube.com/watch?v=C7yXx3YbcNI>)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Please do not delete this or anything above it. Thanks!

  


  


## Contents

  * 1 Append
    * 1.1 Sayonara
  * 2 Append 2
  * 3 External link
  * 4 An image
  * 5 a text
  * 6 new section test
  * 7 Table markup (wrong)
  * 8 Wiki table markup (correct)
  * 9 Bullets
  * 10 Numbering
  * 11 indenting first line
  * 12 Humm
  * 13 Float Like A Butterfly
    * 13.1 New section?
    * 13.2 Tutorial links
  * 14 Clipboard commands with selections



## Append
    
    
     Program Test;
     Uses Crt;
    
     Var I : Integer;
    
     Begin
       For I := 0 to 12 do
         WriteLn('Test ',I:2);
     End.
    

  


### Sayonara

Test O 

How to use an internal link: indent first line. 

## Append 2

Pascal syntax highlighting example: 
    
    
     // this is the same code as above
    
     Program Test;
     Uses Crt;
    
     Var I : Integer;
    
     Begin
       For I := 0 to 12 do
         WriteLn('Test ',I:2);
     End.
    

Terminal code (BASH) example: 
    
    
     // BASH example
     $ ls myfiles*
     $ mkdir newDir
     $ clear
    

## External link

External link: [link title](<http://www.example.com>)

another link: [link title](<http://www.example.com>)

Protocol-relative link: [Chapter 5](<//www.freepascal.org/docs-html/current/ref/refch5.html>)

## An image

[![Lazaruscomponenttab 01.png](https://wiki.freepascal.org/images/a/a0/Lazaruscomponenttab_01.png)](</File:Lazaruscomponenttab_01.png>)

This text should be placed near the picture, does it? zxvzxcv zxvc xcv xzcv zxcv zxvc zxvc zxcv zxcv zxcv zxcv xcv xcv xcv xcv zxcv zxcv zxcv xcv zxcv zxcv aaaaaaaaaaaaa 

**And this line were is going?** and this line? 

## a text

This is the text 

## new section test

## Table markup (wrong)

**Header 1** | **Header 2** | **Header 3** | **Header 4**  
---|---|---|---  
Item 1,1 | Item 1,2 | Item 1,3 | Item 1,4  
Item 2,1 | Item 2,2 | Item 2,3 | Item 2,4  
  
## Wiki table markup (correct)

Table 2  Header 1 | Header 2 | Header 3 | Header 4   
---|---|---|---  
Item 1,1 | Item 1,2 | Item 1,3 | Item 1,4   
Item 2,1 | Item 2,2 | Item 2,3 | Item 2,4   
Shopping List   
---  
Bread & Butter  | Pie  | Buns  | Danish  | Croissant   
Cheese  | Ice cream  | Butter  | Yogurt   
  
## Bullets

  * One
  * Two 
    * Tree



## Numbering

  1. First
  2. Second
  3. Third



_Nested Numbering_

  1. First 
     1. first.one
     2. first.two
     3. first.three
  2. Second 
     1. second.one
     2. second.two
     3. second.three



_Mixed Numbering_

  1. First 
     * first.one
     * first.two
     * first.three
  2. Second 
     * second.one
     * second.two
     * second.three



## indenting first line

Please note that all contributions to Lazarus-ccr are considered to be released under the GNU Free Documentation License (see Lazarus-ccr:Copyrights for details). If you don't want your writing to be edited mercilessly and redistributed at will, then don't submit it here. 

    You are also promising us that you wrote this yourself, or copied it from a public domain or similar free resource. DO NOT SUBMIT COPYRIGHTED WORK WITHOUT PERMISSION!

TEST 

  
**Another** _Test:_ _**lets go crazy**_

I want to create a boxed text: 
    
    
    This is in a **box** , or so _I hope_.
    

This is outside the box, no? And this is a reference to my incomplete [Structural Analyis](<http://www/pcug.org.au/~akabaila/StructuralAnalysis/>) book ThisIsMyShura 
    
    
    Weird: this is in the box as well?
    

  * _And a bullet in italics?_



non formatted... 

## Humm

{{#categorytree:Foo}} <categorytree>

# Float Like A Butterfly

## New section?

Let's see... ok 

grusskarte 

if Graphviz was installed, this would draw a pretty graph... 

<graphviz> digraph G { 
    
    
     rankdir=LR; node [fontsize = "8" shape = "rectangle"]; 
     "Node 1" -> "Node 2";
    

} </graphviz>

This is a test to see, if I can edit a page after the Bad-Behavior update. [fpcfan](</User:Vincent> "User:Vincent") 22:43, 7 December 2007 (CET) 

## Tutorial links

30_Second_Quick_Wiki_Tutorial 30 Second Quick Wiki Tutorial scam link removed 

  * [Using LazAutoUpdate CodeSnippet](<LazAutoUpdater.md> "LazAutoUpdater")



Use this syntax to include a page within another page: {{:LazAutoUpdater}} 

# Clipboard commands with selections

Clipboard-related commands work with carets, both with selections and without them. Some details about this: 

Command | Behaviour, when there're no selections | Behaviour, when at last one selection present  
---|---|---  
Copy to clipboard | Copies entire lines, containing carets. (Ignores multiple carets on same line.) | Copies only selections text. (Ignores carets without selections.)  
Paste from clipboard | First, selections are cleared (deleted). Then, command pastes text into each caret position. Special case is when clipboard lines count equals to carets count - in this case, first line is inserted at first caret, second line is inserted at 2nd caret, etc.

---

_Source: [https://wiki.freepascal.org/Sand_Box](https://web.archive.org/web/20250404223944/https://wiki.freepascal.org/Sand_Box)_
