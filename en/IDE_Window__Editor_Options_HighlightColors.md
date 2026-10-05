# IDE Window: Editor Options HighlightColors

│ **English (en)** │

_**This is part of the online help for the IDE.**_  
It describes the section: "Editor" / "Color and Highlight". You can open the described dialog in your IDE via:  


  * The menu: "Tools" => "Options" / Editor options ...
  * The source-editor pop-up menu: "Editor properties ...



_**Navigation**_  


  * [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options") Overview of all Editor Options

|  | 

  * [IDE Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") Overview of all options for the IDE

  
---|---|---  
  
* * *

## Contents

  * 1 About
    * 1.1 Color-Schemes
    * 1.2 Languages
  * 2 Quick overview of provided functionality
    * 2.1 Choosing Color-Schemes
    * 2.2 Editing Colors in a Scheme
    * 2.3 Exporting edited settings as a new Color-Scheme
  * 3 How Colors are determined
    * 3.1 "Default Text"
    * 3.2 Language specific colors
    * 3.3 Mark-up colors
    * 3.4 Global scheme settings
      * 3.4.1 The difference between "use global" and "Not modified"
  * 4 Settings
    * 4.1 Use syntax highlight
    * 4.2 Language
    * 4.3 Scheme
    * 4.4 File Extensions
    * 4.5 Export
    * 4.6 Element
    * 4.7 Set element to default
    * 4.8 Set all elements to default
    * 4.9 Text attributes
  * 5 User supplied schemes



# About

## Color-Schemes

A color-scheme defines how to highlight text in different languages. It also defines how to highlight language independent mark-up, such as selected text. 

A color-scheme consists of 

  * A basic default color for text and the editors background color. Usually a single scheme uses the same "default colors" for all languages.
  * Individual color and style settings/modifications for language specific elements. For example pascal-keywords or XML-tags.
  * General color and style settings/modifications for mark-up. For example selected-text, matching-brackets, syncro-edit. Again usually for one scheme those colors are the same cross all languages supported by the scheme.



## Languages

Language refers to the content type of a file. Highlighting recognizes different types of content, such as pascal files, XML/HTML, LFM, diff-files and some other programming languages 

# Quick overview of provided functionality

This dialog provides access to the following color and highlight related settings and functions 

## Choosing Color-Schemes

Color-scheme are set individually for each language. This allows to highlight different languages with different schemes. 

To change the scheme for a language choose the language from the first drop-down, then choose the color-scheme in the 2nd drop-down. 

The color-scheme will be applied to all files of this language. The language of a file is detected by the file-extension. The list of extensions can be chosen or edited in the 3rd drop-down. 

## Editing Colors in a Scheme

Individual colors and styles in any scheme can be edited. Changes like this will be applied to all files (that are of a language which is) using the scheme. 

More details of how to edit colors and styles can be found below. 

## Exporting edited settings as a new Color-Scheme

Modified color-schemes an be exported to an XML file (using the export button). Such an XML file can then be distributed. 

To make the exported scheme available, the XML file must be copied into a folder called "userschemes" located in the primary config path of Lazarus. The scheme from the XML file will then appear in the scheme-drop-down. 

To locate where your configuration directory is, within the Laz IDE, click on View> IDE Internals> About IDE. Place the userchemes directory where "Primary config directory" points to. 

# How Colors are determined

## "Default Text"

The base color (fore and background) used to display any text. All other colors act as modification to this color pair. 

Language-specific and Mark-up colors are based on the "default text" colors. They only specify Attributes that need to be modified. (e.g. pascal keywords are bold, otherwise using the default colors) This allows to change the default color and all elements will follow to the new setting (unless they have a modified fore/background color) 

## Language specific colors

Specify colors or styles for language specific elements. 

    Any color set to "not modified" uses the "default text" setting.
    The default for styles is no-style (not bold, not italic, ...).

An exception is the style/color for pascal "Case label". It modifies the style of the underlying pascal element, which can be String, Number or Identifier. If those make no modification, then the default come from the "default text" again. 

For this reason "Case label" has extended style settings like "mark-up" colors (see there). 

## Mark-up colors

Specify colors for all forms of mark-up (text-selection, brackets, ...) 

The modifications made by mark-up settings are applied on top of those made by the language specific settings. They may even be applied on top of other mark-up settings, if several mark-ups apply to a text. (e.g. selected text with bracket highlight in selection) 

Since they modify text that can already have styles (e.g language specific settings may display pascal keywords/comments in bold), they allow to either force a style on/off or invert it's current state. 

E.g Bracket Highlight inverts bold: 

  * A bracket in pascal source is normally not bold, but will be bold if highlighted as part of bracket-pair highlight
  * A bracket in a pascal comment is normally bold, but will be none bold if part of a bracket pair highlight



## Global scheme settings

Most colors can be set globally for the whole scheme or have language specific settings. Using the global-scheme default allows to quickly change a setting for all languages. 

Global settings are available for the "default text" and for all "mark-up" settings. They can be switched off on a per language base, allowing to specify a language specific exception for some or all of those colors. 

Note
    If the "Use global scheme settings" check-box is checked for any settings, then the colors shown and editing are the global colors. Changes will affect all languages.

### The difference between "use global" and "Not modified"

  * "not modified" always refers to the color/style the text has according to the language specific setting, or settings already applied to it.



    (Block-selection may act on default text, on pascal-comment, or even on bracket pair highlight.)
    "not modified" does not mean to take the color from the global scheme settings.

  * "use global" means that the values for all colors styles of the selected element will be determined according to global settings. The global settings will be treated as if they where local).



    global settings can them self specify "not modified, in which case the color will be chosen from the languages upward settings.

# Settings

## Use syntax highlight

Use colors and styles for different tokens. 

## Language

Set here the highlighter you want to edit. 

## Scheme

The IDE has some predefined color schemes - sets of colors. Choose here one you like, then edit the single elements. 

## File Extensions

You can edit the file extensions for which the IDE uses this highlighter. 

## Export

Creates an XML file with the selected scheme (for all languages). 

This file can be distributed. Users can copy it into the sub-folder "/userschemes/" in their config directory. It will be detected, when the IDE is started 

## Element

This listbox contains all elements of the current highlighter. Choose which to edit. Or click on the preview below to select the element. 

## Set element to default

Load the default colors for this element from the current highlighter and scheme. 

## Set all elements to default

Load the default colors for the current highligher and scheme. 

## Text attributes

Check the 'Use default color' checkboxes to get the color button. Then click on the button to select a custom color. 

# User supplied schemes

[User supplied scheme settings](<UserSuppliedSchemeSettings.md> "UserSuppliedSchemeSettings")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_HighlightColors](https://web.archive.org/web/20250212075958/https://wiki.freepascal.org/IDE_Window%3A_Editor_Options_HighlightColors)_
