# ExCtrls

│ **English (en)** │  **[русский (ru)](<../ru/ExCtrls.md>)** │

## Contents

  * 1 About
    * 1.1 Authors
    * 1.2 License
    * 1.3 Download and Installation
      * 1.3.1 Development version
      * 1.3.2 Release version
      * 1.3.3 Installation
  * 2 Usage
    * 2.1 Sample code
    * 2.2 TRadioButtonEx, TCheckBoxEx, TRadioGroupEx and TCheckGroupEx
    * 2.3 TCurrSpinEditEx
    * 2.4 TFloatSISpinEditEx
    * 2.5 TColumnComboBoxEx
    * 2.6 TCheckComboBoxEx
    * 2.7 TShapeEx
    * 2.8 Unit _ExImgList_



## About

_ExCtrls_ is a collection of controls which try to overcome limitations posed by the widgetset. Moreover, some missing controls are added. 

### Authors

The package is written by Werner Pamler, but re-uses a lot of code of the LCL components written by the Lazarus team. The `TColumnComboBoxEx` was contributed by Howard Page-Clark. 

### License

Modified LGPL (with linking exception, like Lazarus LCL) 

### Download and Installation

#### Development version

Use an svn client to download the current trunk version from <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/exctrls>. Alternatively download the zipped snapshot from <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/exctrls/> and unzip it to some folder. 

#### Release version

There is no release version, yet. 

#### Installation

  * In Lazarus, go to "Package" > "Open Package File .lpk".
  * Navigate to the folder with the ExCtrls sources and select the runtime package **exctrlspkg.lpk**. Click "Compile".
  * Then select the designtime package **extrlspkg_dsgn.lpk** and click "Use" > "Install". This will rebuild the IDE (it may take some time). When the process is finished the IDE will restart, and you'll find the components on the palette **ExCtrls**.



## Usage

### Sample code

The application of each component is demonstrated by a project in the folder _examples_ coming along with the component code. 

### TRadioButtonEx, TCheckBoxEx, TRadioGroupEx and TCheckGroupEx

These components re-implement the standard TRadioButton, TCheckBox, TRadioGroup and TCheckGroup, but inherit from TCustomControl and thus are not drawn by the widgetset but by the ThemeServices. As a consequence there are less widgetset related restrictions although the components look and behave like their original LCL counter-parts. 

**New features:**

  * **Wordwrapped captions** : If the new property `WordWrap` is set to `true`, long captions are word-wrapped to fit into the width of the control. Note that when `AutoSize` is active before `WordWrap` is activated the caption is not wrapped initially and all the text is placed into a single line. Turn `AutoSize` off and reduce the width of the control. Once the text is wrapped `AutoSize` may be switche on again to adjust the height.
  * **Vertical layout** : `ButtonLayout` and `TextLayout` control the vertical positioning of checkbox/radiobutton and caption, respectively. Default is `tlCenter`, i.e. the corresponding element is centered vertically within the bounds of the control; other values are `tlTop` and `tlBottom`. Normally `ButtonLayout` and `TextLayout` are changed in pairs.
  * **User provided check images** : The checkboxes and radiobuttons are normally drawn by the ThemeServices, but it is possible to provide dedicated bitmaps which can be used instead. The bitmaps must be stored in a `TImageList` attached to property `Images`. The corresponding image index must be assigned in a handler of the event `OnGetImageIndex` which has appropriate parameters to distinguishe between mouse-over, pressed, enabled/disabled and checked/unchecked states.
  * **Custom caption colors and font settings** : The font of the caption can be changed when the property `ThemedCaption` is switched to `false`. In case of `TRadioGroupEx` the font of individual items can only be changed at runtime by accessing the `RadioGroup1.Buttons[Andex]` property pointing to the `TRadioButtonEx` at the given index in the groupbox. The same is possible also with the `TCheckGroupEx`.



[![checkcontrolsEx.png](https://wiki.freepascal.org/images/8/8f/checkcontrolsEx.png)](</File:checkcontrolsEx.png>)

### [![tcurrspineditex 200.png](https://wiki.freepascal.org/images/b/bd/tcurrspineditex_200.png)](</File:tcurrspineditex_200.png>) TCurrSpinEditEx

The default _LazControls_ palette contains already extended spin edit controls for integers and floating point numbers (`TSpinEditEx` and `TFloatSpinEditEx`, respectively). The missing edit for currency values is implemented in package _ExCtrls_. 

**New and modified properties**

  * `Value`: The data type of the control is `currency`.
  * `CurrencyString`: The currency name, for example '$' or '€', is added to the numeric value in the edit box. The default value is taken from the `DefaultFormatSettings` provided by the `SysUtils` unit.
  * `CurrencyFormat`: Defines the order of (positive) numeric value and currency name in the edit field: 
    * `secfCurrVal`: currency string first, no space (e.g. '$1')
    * `secfValCurr`: value first, no space (e.g. '1$')
    * `secfCurrSpaceVal`: currency string first, space to value (e.g. '$ 1');
    * `secfValSpaceCurr`: value first, space to currency string (e.g. '1 $');
  * `NegCurrencyFormat`: Like `CurrencyFormat`, but now for negative values: 
    * `sencfParCurrValPar`: currency string first, no space, the entire string is put in parenthesis (e.g. '($1)')
    * `sencfMinusCurrVal`: currency string first with leading '-' character, no space (e.g. '-1$')
    * `sencfCurrMinusVal`: currency string first, no space, '-' at value (e.g. '$-1')
    * `sencfCurrValMinus`: currency string first, value with appended '-' (eg. '$1-')
    * etc... Other allowed values are `sencfParValCurrPar` ('1$'), `sencfMinusValCurr` ('-1$'), `sencfValMinusCurr` ('1-$'), `sencfValCurrMinus` ('1$-'), `sencfMinusValSpaceCurr` ('-1 $'), `sencfMinusCurrSpaceVal` ('-$ 1'), `sencfValSpaceCurrMinus` ('1 $-'), `sencfCurrSpaceValMinus` ('$ 1-'), `sencfCurrSpaceMinusVal` ('$ -1'), `sencfValMinusSpaceCurr` ('1- $'), `sencfParCurrSpaceValPar` ('($ 1)'), `sencfParValSpaceCurrPar` ('(1 $)')
  * `DecimalSeparator`: Character used to separate decimals from the integer part. Defaults to the value defined by the `DefaultFormatSettings`.
  * `ThousandSeparator`:If not empty (i.e. not #0) the integer part of the Value is formatted with a thousand separator for better readability. Note that the control does not allow DecimalSeparator and ThousandSeparator to be equal.



[![savingscalculator.png](https://wiki.freepascal.org/images/5/53/savingscalculator.png)](</File:savingscalculator.png>)

### [![tfloatsispineditex 200.png](https://wiki.freepascal.org/images/d/d0/tfloatsispineditex_200.png)](</File:tfloatsispineditex_200.png>) TFloatSISpinEditEx

This is an extended version of `TFloatSpinEditEx` which accepts scientific multiplier suffixes, e.g. k for "kilo" or the factor 1000; the user can input the text `'1.23k'` and the `Value` of the component is 1230. 

### [![tcolumncomboboxex 200.png](https://wiki.freepascal.org/images/b/b0/tcolumncomboboxex_200.png)](</File:tcolumncomboboxex_200.png>) TColumnComboBoxEx

`TColumnComboBoxEx`, written by Howard Page-Clark, is a text-only combobox that displays its dropdown items list in single phrase columns, which are parsed according to the delimiter property. The column width in the dropdown is adjusted automatically to accomomodate the longest word/phrase in each column. The number of columns shown depends entirely on the number of delimiters found in each listed item, hence is an unpublished read-only property. 

[![TColumnComboBoxEx.png](https://wiki.freepascal.org/images/c/cc/TColumnComboBoxEx.png)](</File:TColumnComboBoxEx.png>)

  * `Delimiter`: the `char` value which separates the columns in each combobox item. The default is the comma.
  * `ShowColSeparators`: a `boolean` display property which activates the display of vertical dividing lines between the columns. Default: `false`.
  * `ColSeparatorColor` (`TColor`): defines the color of the vertical dividing lines between the columns. Default: `clSilver`.
  * `ColumnMargin` (`Integer`): allows for adjustment of all column widths by a fixed amount.



### [![tcheckcomboboxex 200.png](https://wiki.freepascal.org/images/6/67/tcheckcomboboxex_200.png)](</File:tcheckcomboboxex_200.png>) TCheckComboBoxEx

`TCheckComboBoxEx` overcomes some annoyances of the `TCheckComboBox` which comes with the Lazarus IDE: 

  * Incidentally checking an item by selecting an item is no longer possible
  * The item list stays in the dropped state until ENTER, ESC, ALT-UP or ALT-DOWN are pressed, or another control is selected. This way several items can be checked without having to drop down the list multiple times. The easiest way to do this is by the UP/DOWN arrow keys and pressing the SPACE bar for checking/unchecking items.
  * The main field of the control lists all checked items rather than only the selected item.



Although the control inherits from `TEditButton` rather than from `TCustomComboBox`, many its **properties and events** are based on `TComboBox`; other properties were added to achieve the unique behaviour of the component: 

  * `AllowGrayed` (boolean): if false (default), the items in the dropdown list can either be checked or not. If true, the items can also be switched to a third, "grayed" state.
  * `AutoDropDown` (boolean): if true, the list window drops down automatically when the control is focused. Otherwise the arrow icon at the right must be clicked, or the key ALT-DOWN must be pressed.
  * `Delimiter` (char): character separating the items in the list of checked items in the main control field.
  * `DropDownCount` (integer): maximum count of lines shown in the dropdown list
  * `EscCancels` (boolean): when true and the dropdown list is closed by pressing the ESC key the changes in the list are cancelled and all items have their checked states as when the list had dropped down initially.
  * `HintMode` (enumeration of `cbhmDefault`, `cbhmItems`): 
    * `cbhmDefault`: the default hint is shown as specified by the `Hint` property
    * `cbhmItems`: the hint is composed of the checked items.
  * `Items` (`TStrings`): list of the items names shown in the dropdown window.
  * `ItemIndex` (integer): Index of the selected (= highlighted) item in the dropdown window
  * `Sorted` (boolean): the items in the dropdown window are sorted alphabetically, when `true`.
  * `Checked[AIndex]` (boolean): it `true` when the item at the given index is checked
  * `State[AIndex]` (boolean): depending on the checked state of the item at the given index the property can have three values: `cbUnchecked`, `cbChecked`, or `cbGrayed`. `AllowGrayed` must be `true` to use this option.
  * `Enabled[AIndex]` (boolean): when false, the checked state of the item at the given index cannot be changed by the user.
  * `OnDropDown`: this event fires when the dropdown windows is dropping down
  * `OnCloseUp`: this event is generated when the dropdown window closes
  * `OnItemChange`: when an item is checked, either by the user or by code, this event is triggered.
  * `OnItemClick`: this event is fired when the user clicks on a checkbox in the dropdown list or when he presses the SPACE bar. It is not fired when the item is checked by code.
  * `OnChange`: In addition to changes of inidividual items, this event fires when all items are changes (methods `Clear()`, `CheckAll`).



The most important **methods** : 

  * `function AddItem(AText: String; AChecked: Boolean = false; AEnabled: Boolean = true): Integer;` \- Adds a new item to the combobox list and sets is Checked and Enabled states.
  * `procedure CheckAll(AState: TCheckBoxState; aAllowGrayed: Boolean = true; aAllowDisabled: Boolean = true);` \- Sets all existing items to the given state. When `aAllowGrayed` is `false` the `cbGrayed` state is not applied. When `aAllowDisabled` is `false` then only enabled items are changed.
  * `procedure Clear` \- removes all items from the dropdown list.
  * `procedure Toggle(AIndex: Integer)` \- toggle the list item at the given index between checked and unchecked.



[![TCheckComboBoxEx Demo1.png](https://wiki.freepascal.org/images/e/e1/TCheckComboBoxEx_Demo1.png)](</File:TCheckComboBoxEx_Demo1.png>)

### [![tshapeex 200.png](https://wiki.freepascal.org/images/3/37/tshapeex_200.png)](</File:tshapeex_200.png>) TShapeEx

`TShapeEx` is a `TShape` descendant with Header and Text properties. Both texts can consist of multiple lines and be left-/right-/top-/bottom-aligned/centered; margins can be specified. The unit is self-contained and can be used without the package. 

[![TShapeEx.png](https://wiki.freepascal.org/images/7/7c/TShapeEx.png)](</File:TShapeEx.png>)

The `Shape` property inherited from the `TShape` ancestor defines the kind of shape to be drawn (`TShapeType = (stRectangle, stSquare, stRoundRect, stRoundSquare, stEllipse, stCircle, stSquaredDiamond, stDiamond, stTriangle, stTriangleLeft, stTriangleRight, stTriangleDown, stStar, stStarDown)`. 

The properties `Header` and `Text` describe two text elements which can be displayed inside the control. They are controlled by properties of type `TShapeText`: 

  * `HorAlignment: TAlignment = (taLeftJustify, taRightJustify, taCenter)` \- the horizontal alignment of the text: left/right aligned, or centered.
  * `HorMargin: Integer` \- the distance to the left or right border of the client rectangle. Note that some shapes are not drawn to the border of the control; in this case the text may appear outside the shape.
  * `Lines: TStrings` \- holds the text to be displayed. It can consist of multiple lines.
  * `VertAlignment: TVerticalAlignment = (taAlignTop, taAlignBottom, taVerticalCenter)` \- the vertical alignment of the text: at the top, at the bottom, or in the center of the control
  * `VertMargin: Integer` \- the distance to the top or bottom border of the client rectangle. Note that some shapes are not drawn to the border of the control; in this case the text may appear outside the shape.



The fonts used for painting `Header` and `Text` are given by the `HeaderFont` and `Font` properties, respectively. 

### Unit _ExImgList_

This unit contains functions to modify existing image lists: 

  * `function CreateDisabledImagelist(AImageList: TCustomImageList; AOwner: TComponent): TCustomImageList`: creates a new image list with copies of the images contained in `AImageList`. The copied images are converted to grayscale. Correct handling of the alpha-channel leads to improved quality compared to the built-in grayscale conversion of the standard `TImageList` (Note the spurious pixels at the border of some icons in the second disabled toolbar of the _ImgListEx demo project_ \-- see screenshot). The primary intention is to assign the result of this function to the `DisabledImages` property of a `TToolbar`.
  * `function SetImageListColor(AImageList: TCustomImageList; ANewColor: TColor): TCustomImageList`: Replaces all pixels with non-zero alpha channel in the images of an existing `ImageList` by the specified new color. It is intended to be applied to modern outline icons consisting of only a single color. This way the user has the possibility to present the icons in a different color.
  * `function CreateRecoloredImagelist(AImageList: TCustomImagelist; ANewColor: TColor; AOwner: TComponent): TCustomImageList`: creates a new image list with copies of the images contained in `AImageList` and then applies the function `SetImageListColor` described above.



The unit is self-contained and can be used without the package. 

[![ImgListEx Demo 1.png](https://wiki.freepascal.org/images/e/e7/ImgListEx_Demo_1.png)](</File:ImgListEx_Demo_1.png>)

---

_Source: [https://wiki.freepascal.org/ExCtrls](https://web.archive.org/web/20240308084922/https://wiki.freepascal.org/ExCtrls)_
