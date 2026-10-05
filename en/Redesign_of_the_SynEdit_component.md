# Redesign of the SynEdit component

For more info on SynEdit go to: [SynEdit](<SynEdit.md> "SynEdit")

* * *

│ **English (en)** │  **[español (es)](</Redesign_of_the_SynEdit_component/es> "Redesign of the SynEdit component/es")** │    
****

## Contents

  * 1 Introduction
  * 2 Class Tree
  * 3 Work in Process
  * 4 Older Content
    * 4.1 Related Classes
      * 4.1.1 Gutter
      * 4.1.2 Grid Painters
      * 4.1.3 Grid Contents
  * 5 See also



# Introduction

This is an attempt to restructure the monolithic TCustomSynEdit class into more general base classes, which can be shared amongst various editor or viewer components. Such an approach will allow to encapsulate the basic functionality, so that derived classes cannot break that vital functionality. 

The goal of the redesign is a set of classes, useful in creating [source code editors](<source_code_editor.md> "source code editor") and other browsers or editors. 

# Class Tree

  * **CustomSynViewer** (TODO)



    

  * **TextBuffer** (Partly done, currently TSynEditStringList)



    Holds the entire text. Eventually it should be possible that more than one SynEdit/SynViewer share the same TextBuffer. The TextBufffer provides the necessary callback hooks so all SynEdits can follow any changes
    SynEdit does not allow direct access to the TextBuffer. For the public SynEdit.Lines property a special wrapper is in place controlling access.
    The TextBuffer supports methods for Editing and viewing.

  * **TextViews**


    Act as a wrapper to the TextBuffer. They allow SynEdit/SynView to see the content of the Textbuffer in certain ways (such as Folded, with expanded Tabs information or with trailing spaces). 

    

  * some of them are:



    TSynEditStringTrimmingList
    SynEditStringDoubleWidthChars
    TSynEditStringTabExpander
    TSynEditFoldedView 

    Not yet fully integrated
    TODO: Integrate Highlighter Access into the TextViews

  * **Highlighter**


    Provides Highlight information, such as pascal or html specific highlighting. ([more](<SynEdit_Highlighter.md> "SynEdit Highlighter")) 

  * **Markup**


    Provides additional highlight info, such as current selection, matching brackets... 

  * **ViewPort** (TODO)



    Defines the visible rectangle of the Text. (Screen content). This is given through TopLine, LeftChar, CharsInWindow, LinesInWindow.
    Translation of Screen to TextBuffer can be handled here. All Scrolling can be handled here. In a way this represents the ScreenGrid. 

  * **TextGrid** (TODO)
  * **Gutter**



    

  * **GutterPart**



  * **HighlightTokenPrinter** (TODO)



    Combines Text, Highlight, Markup, ... and sends it to the ViewPort/TextGrid

  * **CustomSynEdit**



    Implements edit actions

  * **IDESynEdit** (TODO)



    Instantiates a SynEdit, with a specific set of Views and Markup modules

# Work in Process

Plugins
    The structure and integration of plug-ins needs some review, and is entirely subject to review. Applies to Syncron/TemplateEdit and/or to older SynPlugins (Code-Completion, ...)

# Older Content

Below is some older content. It may no longer reflect the actual process. 

I just came across the RichView control, that already is based on a general scrolling window component RVScroll. This component will become the root of my redesign. 

Once the scrollbar details are encapsulated, we can derive a basic grid component. According to our goal, of an [source code editor](<source_code_editor.md> "source code editor"), the cells in this grid later shall contain characters, of an monospaced font. Unlike the TCustomGrid component, our grid is only a logical organization of the drawing surface. Instead of fixed rows or columns, a gutter can be shown to the left of the grid. 

The next level will implement the character grid class, with a demo implementation of an simple text file viewer. The handling of fonts (determination of the cell size), a caret, and lines of arbitrary length will be implemented in this class. 

In the next higher levels changes to the text are implemented, and finally the additional Lazarus specific functionality. 

## Related Classes

Much specific functionality can be implemented in helper classes, so that the editor component can be customized by replacing the SynEdit helpers with other objects. One obvious helper is dedicated to the gutter display, another one to the display of the lines in the character grid, and the text lines are provided and managed by a third helper. 

### Gutter

A gutter pane to the left of the grid can be used to display additional information. Where SynEdit displays line numbers and markers, some other application may display e.g. a table of contents in the gutter. 

### Grid Painters

The basic grid delegates painting to the gutter object and a virtual PaintGrid method. In the case of a character grid, the PaintGrid method organizes the display of entire grid rows, calling another virtual PaintRow method for the actual painting. The PaintRow method again can call a PaintCell method, or it delegates the display of the text in the line to an text painter object. 

### Grid Contents

The contents of a character grid typically are stored in some TStrings object, accessible as strings by line/row numbers. For proper display, all tabs must be expanded into spaces, what suggests an alternative method to retrieve the display strings from the string list. This method also can provide Unicode strings, in order to simplify the column index calculations. Syntax highlighting requires a synchronization between the parser, the highlighting painter, and the list of strings. When wordwrap or block folding can affect the number of visible lines, a translation between display rows and line numbers (string indices) has to be implemented in a dedicated content class. Such specialized content classes should be used only together with appropriate control classes. 

When multiple source files shall be presented in a tabbed editor window (notebook?), the contents of the control should be easily exchangable. When also the same file can be shown in multiple edit controls, at the same time, the actual caret position and further context information must be stored in the content objects, apart from the common text and file information. Changes to an shared text must be reported to all active edit/content objects. Such features require a proper separation of the configuration and context information, into control settings, display content state, and physical text source state. 

# See also

  * [SynEdit](<SynEdit.md> "SynEdit")
  * [SynEdit Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter")

---

_Source: [https://wiki.freepascal.org/Redesign_of_the_SynEdit_component](https://web.archive.org/web/20250317102731/https://wiki.freepascal.org/Redesign_of_the_SynEdit_component)_
