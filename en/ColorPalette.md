# ColorPalette

│ **English (en)** │  **[français (fr)](</ColorPalette/fr> "ColorPalette/fr")** │    
****

## Contents

  * 1 About
    * 1.1 Authors
    * 1.2 License
  * 2 Usage
    * 2.1 Color selection
    * 2.2 Palettes
      * 2.2.1 Built-in palettes
      * 2.2.2 Custom palettes and palette files
    * 2.3 Methods and properties for color access
    * 2.4 Other properties
      * 2.4.1 Layout
      * 2.4.2 Visual appearance
      * 2.4.3 Hints
    * 2.5 Demo projects
  * 3 Download and installation
  * 4 Change Log
    * 4.1 Version 0.2
    * 4.2 Version 0.1



## About

[![ColorPalette](https://wiki.freepascal.org/images/8/82/ColorPalette.png)](</File:ColorPalette.png> "ColorPalette")

ColorPalette is a cross-platform color palette grid with ability to select a color from a palette. The OnMouseColorPick event is fired when the user clicks onto a color. 

### Authors

[Tom Gregorovic](</User:Tombo> "User:Tombo") and Werner Pamler 

### License

Modified LGPL 

## Usage

### Color selection

When a color is selected by clicking with the mouse onto a color box an event `OnColorPick` is generated; the event gets the rgb value of the _picked color_ as a parameter. Another parameter passes the _ShiftState_ active during the click; usually this identifies the mouse button pressed. If the property `ShowSelection` is turned on then the picked color is highlighted. 

The time flow when exactly the color is selected is determined by the property `PickMode`: 

  * `pmImmediate`: The color is selected, and the event is generated, when the mouse button goes down.
  * `pmContinuous`: The color is selected, and the event is generated, while the mouse moves and when the mouse button goes up.
  * `pmDefault`: The color is selected when the mouse button goes down, and the event is generated when the mouse button goes up. Both events must occur at the same location. This mode is a left-over from a previous version of the component and should not be used any more.



### Palettes

#### Built-in palettes

The components supports a series of **built-in palettes** as determined by the `PaletteKind` property: 

  * `pkStandardPalette`: the 16 standard color defined by the `graphics` unit
  * `pkExtendedPalette`: the same, but extended by the 4 extra colors `clMoneyGreen, clSkyBlue, clCream, clMedGray`
  * `pkSystemPalette`: the 25 system colors defined by the `graphics` unit
  * `pkStandardAndSystemPalette`: the standard colors plus the system colors
  * `pkExtendedAndSystemPalette`: the extended colors plus the system colors
  * `pkGradientPalette`: "all" colors nicely arranged; use `GradientSteps` to define the horizontal resolution.
  * `pkWebSafePalette`: a set of web-safe colors as defined by a [wikipedia article](<http://www.wikipedia.org/wiki/Web_colors#Color_table> "wikipedia:Web colors").
  * `pkWebSafePalette2`: another set of web-safe colors.



#### Custom palettes and palette files

In addition, a **custom palette** can be loaded from a file by calling the `LoadPalette` method. The palette files are simple text files according to this example (see also _Default.pal_ in the component folder): 
    
    
    Custom palette example:
        
     $COLS 8
     # sets count of palette grid columns
     
     0,0,0
     # inserts color r,g,b colorname
     # colorname is optional.
     255,255,255 White
     
     $NONE
     # inserts empty palette grid cell
     
     $BLENDWB 128,128,128 3
     # creates color gradient white -> color -> black with specified steps
     # The specified steps are applied to the transitions between "white" and "color"
     # and between "color" and "black", i.e., in total, there are <tt>2*steps+1</tt> color boxes.
    

A set of **keywords** is defined to identify the palette file entries: 

  * `r,g,b colorname`: Defines the next entry of the color palette by means of its red-green-blue components (0..255). The `colorname` is optional.
  * `$NONE`: Is a place-holder for an empty color button. Useful for layout optimization. If the property `UseSpacers` is false then the button can be picked as color `clNone`.
  * `$BLENDBW r,g,b n`: creates a series of palette entries corresponding to a gradient from white to the specified color and to black. Each one of the two gradient consists of `n` steps, i.e. in total, `2*n+1` palette entries are created.
  * `$COLS value`: defines the number of columns used to display the palette.
  * `$BTNDIST value`: specifies the pixel distance between the color palette button tiles (`value` is an `integer`)
  * `$BTNWIDTH value`: specifies the pixel width of each color palette button (`value` is an `integer`)
  * `$BTNHEIGHT value`: specifies the pixcel height of each color palette button (`value` is an `integer`)
  * `$BTNBORDER r,g,b`: specifies the color of the border of a palette button in terms of r, g, b values (`0..255`)
  * `$FLIPPED value`: specifies whether the palette is flipped (`value = TRUE`) or not (`value = FALSE`)
  * `$SELKIND value`: specifies how the selected palette button is highlighted. `value` can be `NONE`, `THIN`, `THICK`, `THIN-INV`, `THICK-INV`.
  * `$SELCOLOR r,g,b`: specifies the color with which the selected button is highlighted if `$SELKIND` is `THIN` or `THICK`. The color is given by its red, green and blue components (`0..255`).
  * `#`: defines a comment - all characters in the same line following the `#` are ignored.



### Methods and properties for color access

There are several methods and properties to manipulate and query the entries of the used palette: 

  * `AddColor(AColor: TColor; AColorName: String = '')`: adds a new entry to the palette. An optional color name can be provided.
  * `DeleteColor(AIndex: Integer)`: removes the entry with the specified index from the palette
  * `ClearColors`: removes all colors from the palette.
  * `LoadPalette(AFileName: String; AItems: TPaletteItems = [])`: load a palette from specified file. The optional parameter `AItems` allows to define which items stored in the palette are loaded: `TPaletteItems = set of [piColors, piColumnCount, piFlipped, piButtonBorder, piButtonSize, piButtonDistance, piSelKind, piSelColor]`. By default, `AItems` is `[piColors, piColumnCount]` which loads all colors and the column count. Use `piAll` as an abbreviation to load all items.
  * `SavePalette(AFileName: String)`: saves the currently used palette to a palette file which, in turn, can be re-loaded by means of the `LoadPalette` method.
  * Access individual colors by using the `Colors[Index]` property.
  * `ColorCount` returns the number of colors in the palette.
  * `PickedColor`: returns the currently selected color
  * `PickedIndex`: returns the index of the currently selected color (indexes are counted first along the rows).
  * `MouseColor`: returns the color of the button underneath the mouse cursor
  * `MouseIndex`: returns the index of the color underneath the mouse cursor (indexes are counted first along the rows).
  * `Colors[AIndex: Integer]`: allows to read and write a color at the specified palette index
  * `ColorNames[AIndex:Integer]`: allows to read and write the name of the color at the specified palette index. If an empty string is written to the ColorNames property then the colorname defined by the graphics unit is used.



### Other properties

#### Layout

  * `ButtonHeight`: height of the color buttons (in pixels). Is used along with `ColumnCount` to define the height of the control.
  * `ButtonWidth`: width of the color buttons (in pixels). Is used along with `ColumnCount` to define the width of the control.
  * `ButtonDistance`: distance, in pixels, between the individual color buttons.
  * `ColumnCount`: Defines how many color buttons are drawn horizontally.



#### Visual appearance

  * `ButtonBorderColor`: defines the borderline around a color button. Can be removed by selecting `clNone`.
  * `Color`: defines the background color of the component. Note that the background is not visible if <ttButtondistance = 0.
  * `ShowSelection`: If true a thick border is drawn around the button of the selected color.
  * `UseSpacers`: If true `clNone` color entries serve as spacers to arrange groups of colors. If false `clNone` is shown as a crossed-out button meaning "transparent color".



#### Hints

  * `ShowColorHint`: If true a hint popup window is displayed for the color button above which the mouse cursor hovers. The built-in hint text can be modified by means of the `OnGetHintText` event.
  * The `OnGetHintText` event allows to modify the built-in text used for creating the popup hint.



### Demo projects

Two demo projects are contained in the folder _demo_ of the component installation directory: 

  * **GeneralDemo** shows application of all the features of TColorPalette.
  * **ToolbarDemo** adds a ColorPalette to a coolbar/toolar.



## Download and installation
    
    
    svn checkout <https://svn.code.sf.net/p/lazarus-ccr/svn/components/colorpalette>
    

  * Download from [Lazarus CCR at SourceForge](<http://sourceforge.net/projects/lazarus-ccr/files/ColorPalette/>)
  * Install the package `LazColorPalette` in the Lazarus IDE. The ColorPalette icon is added to the _Misc_ component palette page.



## Change Log

### Version 0.2

  * **New features**
    * "AddColor", "DeleteColor" etc.
    * Color names
    * Builtin palettes
    * Highlight selected color
    * Color button distance, border color
    * Color hint window
    * New keywords in palette file
  * **Tested** on Windows 2000/XP/7/10, and with gtk2 and qt under Linux (Mint 17).



### Version 0.1

  * **Initial release**
  * **Tested** on Windows XP, and with gtk1 and gtk2 under Linux (Kubuntu 6.06).

---

_Source: [https://wiki.freepascal.org/ColorPalette](https://web.archive.org/web/20241202172733/https://wiki.freepascal.org/ColorPalette)_
