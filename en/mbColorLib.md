# mbColorLib

[Template:mbColorLib](</index.php?title=Template:mbColorLib&action=edit&redlink=1> "Template:mbColorLib \(page does not exist\)")

## Contents

  * 1 About
    * 1.1 Authors
    * 1.2 License
  * 2 Usage
    * 2.1 RGB color pickers
    * 2.2 CMYK color pickers
    * 2.3 HSL and HSV color pickers
    * 2.4 CIELab pickers
    * 2.5 Palette pickers
      * 2.5.1 HexaColorPicker
      * 2.5.2 ColorPalette
      * 2.5.3 ColorList and ColorTree
    * 2.6 Other tools
      * 2.6.1 OfficeColorDialog
      * 2.6.2 DeskPickerButton and DeskPickerAction
      * 2.6.3 ColorPreview
    * 2.7 Demo projects
  * 3 Download and installation
  * 4 Change Log
    * 4.1 Version 2.2
    * 4.2 Changes with respect to Delphi version



## About

[![FullDemo screenshot](https://wiki.freepascal.org/images/f/fc/mbColorLib_hexa.png)](</File:mbColorLib_hexa.png> "FullDemo screenshot")

Comprehensive library with more than 30 components for color selection in several color models (RGB, CYMK, HSV, HSL, CIE-Lab). The components can be combined to create numerous new tools and color dialogs. 

### Authors

  * Original author: Marko Binić
  * Lazarus port: Werner Pamler



### License

Original license text by Marko Binić from the file _Readme.rtf_ in the installation folder: _"License is granted to use, modify and redistribute these units in your applications as you see fit. You are given COMPLETE FREEDOM with the sources found in this pack; you're free to use it in ANY kind of app without even mentioning my name, my site or any other stuff, that depends on your good will and nothing else. I will accept any modifications and incorporate them in this pack if they'll help make it better. You are under NO obligation to pay for these components to neither me nor anyone else trying to sell them in their current form. If you wish to support development of these components you can do so by contributing some source or making a donation, again this solely depends on your good will."_

  


## Usage

[![Component palette with mgColorLib components](https://wiki.freepascal.org/images/f/f3/mbColorLib_ComponentPalette.png)](</File:mbColorLib_ComponentPalette.png> "Component palette with mgColorLib components")

Currently, the mbColorLib consists of 30 visual multi-purpose color picker components, a color selection dialog and 1 standard action for picking color from the desktop. The visual components can be classified by the color model to which they apply. 

### RGB color pickers

[![Component palette icons of the R/G/B color pickers](https://wiki.freepascal.org/images/e/ea/rgb_pickers.png)](</File:rgb_pickers.png> "Component palette icons of the R/G/B color pickers") The RColorPicker, GColorPicker, BColorPicker, as well as the RAxisColorPicker, GAxisColorPicker, BAxisColorPicker can be used to select a color using the RGB color model. In this model, the colors red, green and blue are used as base colors. If these color components are turned off the resulting color is black, like on a monitor screen. Increasing the red, green, blue values adds color to the monitor until the final color, white, is reached. Therefore, this is called _addtive color mixing_. 

The **RColorPicker** is a simple 1-dimensional slider which can be used to adjust the red component of the final color which is accessible from the property `SelectedColor`. Whenever the color changes an event `OnChange` is generated. The green and blue components can be set by corresponding properties. 

Correspondingly, the **GColorPicker** and **BColorPicker** components are responsible for the green and blue base color values. 

The **RAxisColorPicker** is a 2-dimensional color picker. It is the complement of the RColorPicker and varies the green and blue color values in the rectangle defined by the rectangle spanned by the component. The blue value increases horizontally, and the green value increases vertically. The red value is held fixed. The point of the `SelectedColor` is marked by a circle or a cross (property `MarkerStyle`) and can be controlled by dragging the mouse or using the arrow keys. Whenever the selected color changed, the event `OnChange` is fired. 

The event `OnChange` can be used to link the red color component of the RAxisColorPicker to the red color component selected by a RColorPicker: 
    
    
    procedure TForm1.RColorPickerChange(Sender: TObject);
    begin
      RAxisColorPicker.Red := RColorPicker.Red;
    end;
    

Similarly, the **GAxisColorPicker** and **BAxisColorPicker** are the complements of the GColorPicker and BColorPicker. The GAxisColorPicker varies blue and red, the BAxisColorPicker green and red on the horizontal and vertical axes, respectively. 

### CMYK color pickers

[![Component palette icons of the C/M/Y/K color pickers](https://wiki.freepascal.org/images/9/95/cmyk_pickers.png)](</File:cmyk_pickers.png> "Component palette icons of the C/M/Y/K color pickers") The CMYK color model is usually employed by color printers. The unprinted sheet of paper is white, i.e. reflects all light. Printing a cyan dye onto the paper absorbs its complementary color, red, from the spectrum; green and blue can pass the dye without being changed (ideally). Similarly magenta and yellow are used to subtract the green and blue colors from the white color of the paper. Since colors are subtracted this process is called _subtractive color mixing_. Cyan, magenta, and yellow are the base colors of the CMY color model, but usually a black dye is added for achieving better color saturation, and this leads to the name CMYK. 

The **CColorPicker** , **MColorPicker** , **YColorPicker** , and **KColorPicker** are 1-dimensional trackbars to modify the color components cyan, magenta, yellow and black in the CMYK color model. Like before with the RGB pickers, the resulting color is accessible in the property `SelectedColor`. 

### HSL and HSV color pickers

In contrast to the technically oriented RGB and CMYK color models, HSL and HSV provide a more natural access to color. HSL stands for **h** ue, **s** aturation and **l** uminance, while the V in HSV stands for **v** alue. 

Both models describe colors as points in a cylinder. The central axis of the cylinder ranges from black at the bottom to white at the top with neutral colors in between. The angle around the cylinder corresponds to the _hue_ , the radial distance from the axis is the _saturation_ , and the distance along the axis is the _luminance_ or _value_ (brightness). While the _luminance_ of the HSL model scans the full axial direction from black to white, the _value_ of the HSV model begins at black and stops halfways between black and white where the color is "purest". 

Numbers for hue, saturation, luminance and value can be expressed in several units. Hence each component offers a property `MaxXXX` (XXX = hue, saturation, luminance, or value). The same settings must be used if several pickers are combined. 

In addition, all hue/saturation/luminance/value pickers share the property `BrightnessMode` which acts as a switch between the HSL and HSV color models. If `BrightnessMode` is `bmLuminance` then the HSL model is active, and the brightness of a color is described by the _luminance_ value L, and if `bmValue` is used then the HSV model is active and the brightness is expressed in terms of the _value_ V. Again, if several pickers are combined they must share the same `BrighnessMode` setting because the HSL and HSV color models result in different number pairs of saturation/luminance and saturation/value. 

mbColorLib provides a variety of 1-dimensional, 2-dimensional and combined pickers: 

[![Component palette icons of the hue pickers](https://wiki.freepascal.org/images/f/fb/hue_pickers.png)](</File:hue_pickers.png> "Component palette icons of the hue pickers")

_Hue_ can be varied by means of the **HRingPicker** which resembles the cylindrical representation described above. But there is also a linear slider, the **HColorPicker**. Being a cylindrical coordinate, the hues are usually given in degrees, but the scale can be changed by the property `MaxHue` (`360` by default). 

[![Component palette icons of the saturation, luminance and value pickers](https://wiki.freepascal.org/images/7/76/slv_pickers.png)](</File:slv_pickers.png> "Component palette icons of the saturation, luminance and value pickers")

There exist also linear trackbar color pickers for _saturation_ (**SColorPicker** , gray-to-red gradient icon) and _luminance_ or _value_ (**LVColorPicker** , black-to-white gradient icon). The luminance and value pickers are combined within the same component, luminance is selected by setting the property `BrighnessMode` to `bmLuminance` while `bmValue` is for picking color value data. 

[![Component palette icon of the hue-saturation picker](https://wiki.freepascal.org/images/6/61/hs_picker.png)](</File:hs_picker.png> "Component palette icon of the hue-saturation picker")

_Hue_ and _saturation_ can be selected simultaneously by means of the **HSColorPicker**. Here, hue varies horizontally while saturation increases vertically. Thus, each point in this rectangle is a specific combination of hue and saturation. 

[![Component palette icon of the hue-saturation picker](https://wiki.freepascal.org/images/8/85/hs_circle_picker.png)](</File:hs_circle_picker.png> "Component palette icon of the hue-saturation picker")

The **HSCirclePicker** allows selection of _hue_ and _saturation_ as well. Here the hues are arranged around a circle with decreasing saturation towards the center. The selected color is defined by the intersection point of a constant-hue radial line and a constant-saturation circle. 

[![Component palette icon of the saturation-luminance picker](https://wiki.freepascal.org/images/4/41/sl_picker.png)](</File:sl_picker.png> "Component palette icon of the saturation-luminance picker")

Similarly, the **SLColorPicker** can be used to select pairs of _saturation_ and _luminance_ for a specific hue. Saturation varies along the horizontal axis, while luminance increases vertically.   


[![Component palette icon of the HSLColorPicker and HSLRingPicker](https://wiki.freepascal.org/images/2/2b/hsl_pickers.png)](</File:hsl_pickers.png> "Component palette icon of the HSLColorPicker and HSLRingPicker")

The **HSLColorPicker** (left icon) combines a HSColorPicker for _hue_ and _saturation_ selection with an LVColorpicker for _luminance_ selection. In the **HSLRingPicker** (right icon) the _hue_ can be picked from a HRingPicker arranged around a SLColorPicker panel for selection of _saturation_ and _luminance_. In each case, the `SelectedColor` is a unique color which does not depend on any other parameter. Please note that in spite of their name name the pickers work in both HSL and HSV color models depending on the setting of `BrightnessMode`. 

[![Component palette icon of the SLHColorPicker](https://wiki.freepascal.org/images/3/33/slh_picker.png)](</File:slh_picker.png> "Component palette icon of the SLHColorPicker")

The **SLHColorPicker** , finally, consists of a SLColor picker panel for selection of _saturation_ and _luminance_ and a vertical HPicker trackbar for selection of _hue_.   


### CIELab pickers

The **CIELab color space** includes all perceivable colors. It is device-independent, i.e. colors are defined independently of how they are created and of the device they are displayed on. It contains dimensions `L` for lightness and `a` and `b` for a transition between opponent color pairs: 

  * The **lightness** , L, represents the darkest black at L = 0, and the brightest white at L = 100
  * **a** is the axis of the red/green opponent colors, with green at negative a and red at positive a values
  * **b** , similarly, is the axis of the yellow/blue opponent colors with blue at negative and yellow at positive b values.



a and b usually run between -100 and 100. 

[![Component palette icons of the CIELab color pickers](https://wiki.freepascal.org/images/5/59/cielab_pickers.png)](</File:cielab_pickers.png> "Component palette icons of the CIELab color pickers")

mbColorLib contains three similar color pickers for the CIELab color model. Each one of them holds one of the three dimensions constant and allows selection of the two other dimensions by clicking in a rectangle spanned by these other dimensions. The name of the constant dimension is specified in the name of the component: TCIEAColorPicker (left icon), TCIEBColorPicker (center icon), TCIELColorPicker (right icon). 

### Palette pickers

#### HexaColorPicker

[![Component palette icon of the HexaColorPicker](https://wiki.freepascal.org/images/a/a5/hexa_picker.png)](</File:hexa_picker.png> "Component palette icon of the HexaColorPicker")

The **HexaColorPicker** is similar to the HSCirclePicker, but it plays only discrete colors arranged in a hexagon. Hue varies around the circumference of the hexagon. The center hexagon is gray, its brightness depends the setting of the adjacent slider. The brightness of the colors between the perimeter and center hexagons is interpolated. In addition, there is also a set of predefined gray value hexagons (see the screenshot above).   


#### ColorPalette

[![Component palette icon of the TColorPalette of mbColorLib](https://wiki.freepascal.org/images/6/65/colorpalette_picker.png)](</File:colorpalette_picker.png> "Component palette icon of the TColorPalette of mbColorLib")

The **ColorPalette** is useful for selection of a color from a set of predefined colors ("palette"). In this picker, the colors are arranged by rows in any order. 

Colors can be added to the palette in several ways: 

  * Call `GeneratePalette(ABaseColor: Color)` to create a palette with various shades of the base color provided
  * Call `GenerateGradientPalette(Color: array of TColor)` to create a palette for a gradient defined by the specified color array
  * Assign a stringlist with color strings (in rgb format) to the `Colors` property; an additional list with color names can be assigned to the `ColorNames` \- it will be used for popup hints to be displayed if the mouse is above a palette field.
  * Load a palette from a file by setting the filename ot the `Palette` property. The reader accepts palette files in the JASC .pal and Photoshop .aco and .act formats.



#### ColorList and ColorTree

[![Component palette icons of the TmbColorList and TmbColorTree](https://wiki.freepascal.org/images/b/b2/colorlist_colortree.png)](</File:colorlist_colortree.png> "Component palette icons of the TmbColorList and TmbColorTree")

**TmbColorList** (left icon) is a color selection listbox which displays a color box along with the color name. **TmbColorTree** (right icon) looks very similar, but clicking on the dropdown field at the right opens a subnode with the RGB and Hex color information. 

Colors are added to both controls by calling `AddColor(AName: String; AColor: TColor)`   


### Other tools

#### OfficeColorDialog

[![Component palette icon of the TmbOfficeColorDialog](https://wiki.freepascal.org/images/3/31/mbofficecolordialog.png)](</File:mbofficecolordialog.png> "Component palette icon of the TmbOfficeColorDialog")

The two-page **OfficeColorDialog** is similar to the kind of color selection tool that Microsoft had used in its Office applications. It is a two-page dialog with these properties: 

  * On the "standard" page, it contains a hexacolor picker.
  * On the "custom" page, there are several combined color pickers which can be selected by the "picker" combobox.
  * In addition, colors can be selected directly by entering the R, G, B, or H, S, L values.



#### DeskPickerButton and DeskPickerAction

[![Component palette icon of the TmbDeskPickerButton](https://wiki.freepascal.org/images/9/9f/mbdeskpickerbutton.png)](</File:mbdeskpickerbutton.png> "Component palette icon of the TmbDeskPickerButton")

The **mbDeskPickerButton** , if clicked by the mouse, changes the mouse cursor to a pipette. Move the mouse to the point on the desktop with the color that you want to use, click the mouse, and the color will be available in the property `SelectedColor`. 

The same task can be performed with the **mbDeskPickerAction** which can be assigned to the `Action` property of any other control to trigger the color selection process. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** While the pipette cursor is shown an invisible window is placed on top of the desktop to catch the mouse events. Debugging during this phase by means of breakpoints can make the system unresponsive.

#### ColorPreview

[![Component palette icon of the TmbColorPreview](https://wiki.freepascal.org/images/6/61/mbcolorpreview.png)](</File:mbcolorpreview.png> "Component palette icon of the TmbColorPreview")

The **mbColorPreview** is not a color picker, it can be used to display the currently selected color of other pickers or the color under the mouse cursor. Set its property `Color` to the `SelectedColor` of the picker in the picker's `OnChange` event.   


### Demo projects

Two demo projects are contained in the folder _examples_ of the component installation directory: 

  * **fulldemo** shows application of all the components included in the _mbColorLib_.
  * **axispickers** demonstrates the combination of RAxisColorPicker and RColorPicker, GAxisColorPicker and GColorPicker and BAxisColorPicker and BColorPicker.
  * **trackbars** demonstrates all trackbar color selectors.



## Download and installation

  * **Development version:** Use an svn client to download from <https://svn.code.sf.net/p/lazarus-ccr/svn/components/mbColorLib>
  * **Release version:** download a zip file from [Lazarus CCR at SourceForge](<http://sourceforge.net/projects/lazarus-ccr/files/mbColorLib/>)
  * **Install** the package `mbcolorliblaz` in the Lazarus IDE. All icons are added to the component palette page _"mbColorLib"_.



## Change Log

### Version 2.2

  * Initial commit of the library to the Lazarus Components and Code Repository. It is based on the last published Delphi version 2.0.1 available from <http://mxs.bergsoft.net>. In the Lazarus version Delphi support has been dropped.



### Changes with respect to Delphi version

If you are planning to convert an existing application written with the mbColorLib for Delphi to Lazarus you must be aware of several code-breaking changes introduced in the new version: 

  * All **units typical of Delphi** (Windows, Messages etc) were replaced by Lazarus units (LCLIntf, LMessages etc).
  * The pickers for the HSL/HSV color model have an enumeration property **BrightnessMode** to switch between both color models (`bmLuminance` \--> HSL, `bmValue` \--> HSV)
  * Each HSL/HSV picker has new properties `MaxHue`, `MaxSaturation`, `MaxLuminance`, and `MaxValue`. This was introduced because there is no unique unit system for the corresponding quantities, and the old implementation was a mix-up of both color models.
  * TLColorPicker and TVColorPicker were removed and replaced by **TLVColorPicker** which contains both picker variants.
  * The original THSVColorPicker was renamed to **THSCirclePicker** because this better describes its functionality. The original unit _HSVColorPicker.pas_ was renamed to _HSCirclePicker.pas_.
  * Former properties `HValue`, `HueValue` were replaced by `Hue` for better naming consistency. Similarly with the properties for saturation, luminance and value. Also, in some RGB pickers the properties `RValue`, `GValue`, and `BValue` were replaced by `Red`, `Green`, and `Blue`.

---

_Source: [https://wiki.freepascal.org/mbColorLib](https://web.archive.org/web/20250323133129/https://wiki.freepascal.org/mbColorLib)_
