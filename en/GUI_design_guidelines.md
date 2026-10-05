# GUI design guidelines

│ **English (en)** │  **[русский (ru)](<../ru/GUI_design_guidelines.md>)** │

These guidelines apply to the Lazarus IDE but may be used for other projects as well. 

## Contents

  * 1 General
  * 2 Dialogs (modal forms)
    * 2.1 Dialog Settings
    * 2.2 General layout
      * 2.2.1 Spacings and sizes
      * 2.2.2 Button panel
    * 2.3 Anchoring
    * 2.4 Aligning
      * 2.4.1 BorderSpacing
  * 3 See also



## General

All dialogs and windows must not use fixed sizes. Keep in mind that [font](<Font.md> "Font") sizes and translated captions can vary. Use splitters and panels. 

All forms and dialogs must work on a screen of 800x600 or higher resolution. 

## Dialogs (modal forms)

Dialogs are special forms that need the user to make / change some setting before lazarus can continue. 

  * Close on Escape (if the key is not used otherwise)
  * Define default button and Return activates it (if the key is not used)
  * If it has scrollable content (e.g. contains a TMemo, TListBox, TTreeView) it should be resizable and size is stored



### Dialog Settings

Settings that should be applied in general are: 

  * **BorderStyle** : 
    * `bsSizeable` \- if the dialog is complex and resizing is really needed.
    * `bsDialog` \- if dialog is simple and resizing is not needed (NOTE: all dialogs should remain resizable internally because of localization issues).
  * **Position** : `poScreenCenter`.
  * **FormStyle** : Do not use `fsStayOnTop`, as this can be pretty annoying under some window managers.
  * **BorderIcons** : 
    * `[biSystemMenu]` \- for modal shown dialogs
    * `[biSystemMenu,biMinimize,biMaximize]` \- for non-modal shown dialogs



### General layout

  * On the bottom of the form Button panel should be placed.
  * Close on Escape (if key not used otherwise).
  * Define default button and Return activates it (if key not used).
  * Avoid using long captions on buttons. Use `Hint` property to explain button action in details.
  * The size of resizable dialogs should be saved and restored next time the dialog will be opened.
  * All hints and captions used on a form should be as resourcestring, so localization can be applied.



#### Spacings and sizes

  * Dialogs must be designed to work independently of resolution and font height.
  * Spacing between elements: 6 pixels.
  * Do not set maximum constraints.
  * Do not set minimum height of regular buttons.
  * You can set a minimum width of 90 for buttons.
  * `AutoSize` property need to be set for buttons, labels, ... to properly display localized text if it is longer than expected.
  * All buttons in a group (including buttons that contain glyphs) must have the same width and height. Use a TPanel and set the Align and BorderSpacing properties.



#### Button panel

The button panel should consist at least of the following elements: 

  * OK button.
  * Cancel button.
  * Next button (optional).
  * Previous button (optional).
  * Help button.



### Anchoring

To make sure that a dialog after resizing will keep on showing the components in a good way, anchoring is being used. Each components has a property called Anchors. With this property you can determine the way a component is anchored to it's parent. 

Hint: Use the anchor editor of the IDE. 

### Aligning

Another way to organise components is using alignment. Each component has a property Align. By selecting the appropriate align position several components can be positioned properly. 

#### BorderSpacing

Especially useful in combination with Align is the BorderSpacing property. (Also found on the Anchors dialog). A typical borderspacing of 6 pixels around a component is used. 

## See also

  * [High DPI](<High_DPI.md> "High DPI")
  * [Anchor Sides](<Anchor_Sides.md> "Anchor Sides")
  * [Autosize / Layout](<Autosize_/_Layout.md> "Autosize / Layout")

---

_Source: [https://wiki.freepascal.org/GUI_design_guidelines](https://web.archive.org/web/20230530075349/https://wiki.freepascal.org/GUI_design_guidelines)_
