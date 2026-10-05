# KControls

## Contents

  * 1 About
  * 2 Download
  * 3 All controls
  * 4 KGrid control
  * 5 KMemo control
  * 6 License
  * 7 Notes
    * 7.1 See also



# About

**KControls** is a set of components. Main components: KGrid/KDbGrid, KMemo, KHexEditor. Almost all controls are cross-IDE compatible (Delphi/C++Builder, Lazarus) and cross-platform compatible in Lazarus. **KControls** does **not** support RightToLeft languages. 

# Download

Repository: <https://bitbucket.org/tomkrysl/kcontrols>

# All controls

  * TKGrid - powerfull control for working with table data, replacement for TStringGrid and TDrawGrid - full compatibility and lot of new features
  * TKDBGrid - control for working with database tables, possible replacement for TDBGrid
  * TKMemo - native replacement for TMemo/TRichEdit - written from scratch, great features, full RTF support etc.
  * TKHexEditor - powerfull control for binary file editing
  * TKIcon - powerfull component for working with icons and cursors (files *.ico and *.cur, works only on Windows)
  * TKPrintPreview - control for displaying print preview, it is part of KControls built in printing capabilities, every visual control supports printing and previewing
  * TKPrintPreviewDialog - nonvisual component - dialog - for an easy print preview implementation
  * TKPrintSetupDialog - nonvisual component - dialog - for an easy implementation of printing preferences
  * TKCustomControl - a base for your visual controls, for which you want to implement e.g. printing and previewing capabilities in Delphi or Lazarus (just 2 short functions to write).
  * TKNumberEdit - edit control to input numbers in various formats
  * TKFileNameEdit - edit control to input file names
  * TKLinkLabel - label to show clickable link
  * TKGradientLabel - label with gradient background
  * TKLog - component for event logging
  * TKPercentProgressBar - custom drawn progress bar which shows actual percentage of the progress
  * TKBrowseFolderDialog - wrapper for the standard Windows API browse for folder dialog (works only on Windows)
  * TKBitBtn - like TBitBtn - draws PNG and alpha channel correctly in Delphi, has WordWrap property, works in Lazarus as well
  * TKColorButton - just another button to pick color



# KGrid control

There were several reasons to write an own grid component for Delphi: 

  * Borland's inoperability to fix reported bugs (this applied not only for TStringGrid)
  * wanted different editing controls like TEdit, TComboBox, TCheckBox to be inserted in cells
  * wanted the last column to be aligned within the client area edge
  * wanted a slightly better behavior while moving or resizing columns or rows



Now KGrid has a lot of features and is becoming a versatile solution for table visualisations, table data modifications and creating table reports under Delphi and Lazarus IDEs. 

[![kgrid.png](https://wiki.freepascal.org/images/2/2b/kgrid.png)](</File:kgrid.png>)

# KMemo control

Unlike the rich edit components known to Lazarus users (lzRichedit or RichMemo), KMemo is not bound to external rich edit libraries, was written from scratch and uses its native powerful word processor. 

[![kmemo.png](https://wiki.freepascal.org/images/3/3a/kmemo.png)](</File:kmemo.png>)

The features: 

  * text formatting - font colors, styles, text backgrounds, capitals, small capitals, subscript, superscript etc.
  * paragraph formatting - left, center, right alignments, top and bottom indents, first line indents, fully automated bullets and numbering even with multilevel heading numbering (MS Word 97+ style), paragraph backgrounds, paragraph borders, selectable word wrap mode etc.
  * positioned objects (images, text containers) with absolute or relative positioning, different modes of text floating (none, both sides, left or right side)
  * advanced table support with full row/cell formatting and even with cell merging support, so even very complex tables created with MS Word can be loaded, saved, viewed and edited in KMemo
  * document background image/color
  * full load/save support from/to Rich Text Format featuring all supported text/paragraph styles, tables, positioned objects, etc. Furthermore, these load/save routines support files and streams, they support saving/loading only part of a document, so e.g. append or insert one RTF document into other is easy to implement.
  * clipboard operations (cut/copy/paste) with full RTF support, working hopefully on every OS
  * print/preview support
  * KMemoFrame featuring a simple editor including basic dialogs for text, paragraph and numbering formatting, easily embeddable to existing applications without coding
  * user manual showing the basic features and two simple demos



# License

KControls is provided free on 'as is' basis, with complete source code. However, it is licensed software. More information about license can be found in source file headers. 

# Notes

All controls support Delphi and Lazarus IDEs fully. But there are some exceptions: TKIcon and TKBrowseFolderDialog work only on Windows and printing and previewing might not work in some widgetsets in Lazarus. 

## See also

  * [KControls/KmemoNotes](<KControls/KmemoNotes.md> "KControls/KmemoNotes") \- some notes about using KMemo, one of the components in KControls.
  * [RichMemo/Features](<RichMemo/Features.md> "RichMemo/Features") \- comments on some features in progress
  * [MyNotex](<https://sites.google.com/site/mynotex/>) application which uses modified by Massimo Nardello RichMemo package on Linux-Gtk2
  * [TMemo](<TMemo.md> "TMemo")
  * [RichMemo](<RichMemo.md> "RichMemo")

---

_Source: [https://wiki.freepascal.org/KControls](https://web.archive.org/web/20221201100545/https://wiki.freepascal.org/KControls)_
