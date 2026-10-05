# lzRichEdit

## Contents

  * 1 About
  * 2 Author
  * 3 Platforms
  * 4 License
  * 5 Download
  * 6 Installation
  * 7 Methods
    * 7.1 CopyToClipboard
    * 7.2 CutToClipboard
    * 7.3 PasteFromClipboard
    * 7.4 FindText
    * 7.5 GetFirstVisibleLine
    * 7.6 GetRTFSelection
    * 7.7 PutRTFSelection
    * 7.8 GetWordAtPoint
    * 7.9 GetWordAtPos
    * 7.10 GetZoomState
    * 7.11 SetZoomState
    * 7.12 LoadFromFile
    * 7.13 SaveToFile
    * 7.14 Print
    * 7.15 Undo
    * 7.16 Redo
    * 7.17 ScrollLine
    * 7.18 ScrollToCaret
    * 7.19 SelectAll
  * 8 Properties
    * 8.1 Paragraph
      * 8.1.1 Alignment: TRichEditAlignment
      * 8.1.2 traLeft
      * 8.1.3 traRight
      * 8.1.4 traCenter
      * 8.1.5 traJustify
      * 8.1.6 FirstIndent: Longint
      * 8.1.7 LeftIndent: Longint
      * 8.1.8 RightIndent: Longint
      * 8.1.9 Numbering: TnumberingStyle
    * 8.2 SelAttributes
      * 8.2.1 Color: TColor
      * 8.2.2 BackColor: TColor
      * 8.2.3 Name: TFontName
      * 8.2.4 Size: Integer
      * 8.2.5 Style: TFontStyles
    * 8.3 CaretCoordinates
      * 8.3.1 Column :Integer
      * 8.3.2 Line :Integer
    * 8.4 CaretPoint
      * 8.4.1 X :Integer
      * 8.4.2 Y :Integer
    * 8.5 ScrollPoint
      * 8.5.1 X :Integer
      * 8.5.2 Y :Integer
    * 8.6 DefaultExtension
      * 8.6.1 string
  * 9 Graphic support
  * 10 More information



## About

This component was created to solve or at least alleviate the lack of RichText component. Component runs on two platforms: Linux (GTK2) / Windows, and has different ways to work on both OS'es. 

  * Control in Windows is provided by OS API. Win version supports all features that Win32 API has.
  * Linux file reading is made by FPC TRTFParser class, with some changes to support images, this class is slower on complex files. Linux version supports only text formatting.



The two most important properties are Paragraph and SelAttributes, one takes care of the attributes of the paragraph and the other attributes of text. 

Screenshot: 

[![lzRichEdit.png](https://wiki.freepascal.org/images/e/e4/lzRichEdit.png)](</File:lzRichEdit.png>)

## Author

Elson Junio (elsonjunio@yahoo.com.br). 

Antônio Galvão. 

## Platforms

**Linux** (Gtk2) and **Win32**. 

## License

modified LGPL. 

## Download

The latest version is available here: <http://sourceforge.net/projects/lazarusfiles/files/lzRichEdit.zip/download>

SVN: <https://lazarus-br.googlecode.com/svn/trunk/package/lzRichEdit>

## Installation

  * Download the package
  * Open the package, and install it, rebuilding the IDE
  * TlzRichEdit is added to 'Common Controls' component page.



## Methods

### CopyToClipboard

Copies RTF text to clipboard. 

### CutToClipboard

Cuts RTF text to clipboard. 

### PasteFromClipboard

Pastes RTF text from clipboard. 

### FindText

Finds text in the control contents. 

### GetFirstVisibleLine

Gets the first visible line. 

### GetRTFSelection

Gets the selected RTF text to a provided stream. 

### PutRTFSelection

Puts the contents of a source RTF stream in the caret position. 

### GetWordAtPoint

Gets the word at the (x,y) position. 

### GetWordAtPos

Gets the word at the text position. 

### GetZoomState

Gets the zoom state of the RichEdit. 

### SetZoomState

Sets the zoom state of the RichEdit. 

### LoadFromFile

Loads a file. 

### SaveToFile

Saves a file. 

### Print

Prints the contents of the control. 

### Undo

Undo modifications. 

### Redo

Redo modifications. 

### ScrollLine

Scrolls for a given delta line position. 

### ScrollToCaret

Scrolls to the caret. 

### SelectAll

Selects all text. 

## Properties

### Paragraph

It is possible to change the current paragraph attributes (it is guided by SelStart) property. 

##### Alignment: TRichEditAlignment

Sets / gets the alignment of the paragraph or selected text. 

##### traLeft

Paragraph aligned to left. 

##### traRight

Paragraph aligned to right. 

##### traCenter

Paragraph centered. 

##### traJustify

Paragraph justified. 

##### FirstIndent: Longint

Sets / gets the indentation of the first line of a paragraph, FirstIndent works differently on Linux and Windows. Under Windows it has a relationship LeftIndent proportional to its value and shouldnt be negative if you want your effect is the recoil of the line, Otherwise the line is advanced. Linux does not the relationship between LeftIndent and FirstIndent and its value must be positive for Causing the retreat of line. 

##### LeftIndent: Longint

Sets / gets the paragraph indentation left. 

##### RightIndent: Longint

Sets / gets the distance from the text in the paragraph right corner of the control. 

##### Numbering: TnumberingStyle

Inserts \ checks in paragraph marker, only one paragraph at a time. 

### SelAttributes

It is possible to change and get attribute values of the selected text. 

##### Color: TColor

Used to set / get the color of the selected text. 

##### BackColor: TColor

Used to set / get the background color of the selected text. 

##### Name: TFontName

Used to set / get the font of the selected text. 

##### Size: Integer

Used to set / get the font size of the selected text. 

##### Style: TFontStyles

Used to set / recer the font style of the selected text (Linux / Windows). 

### CaretCoordinates

##### Column :Integer

Reads the caret column position. 

##### Line :Integer

Reads the caret line position. 

### CaretPoint

##### X :Integer

Reads the X pixels position of the caret. 

##### Y :Integer

Reads the Y pixels position of the caret. 

### ScrollPoint

##### X :Integer

Reads and writes the X scrolling position. 

##### Y :Integer

Reads and writes the Y scrolling position. 

### DefaultExtension

##### string

Provides a way to set a file extension different than ".RTF" which can be correctly opened. 

## Graphic support

The images support is left up to 3 units: 

  * RichOleBox
  * RichOle
  * GTKTextImage



These units are in the sample folder and can be placed along with the source code of your program. 

## More information

For more information, see the Example Project or contact the author.

---

_Source: [https://wiki.freepascal.org/lzRichEdit](https://web.archive.org/web/20250417153814/https://wiki.freepascal.org/lzRichEdit)_
