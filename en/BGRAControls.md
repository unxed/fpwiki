# BGRAControls

│ **English (en)** │  **[русский (ru)](<../ru/BGRAControls.md>)** │

[![bgracontrols.png](https://wiki.freepascal.org/images/6/6e/bgracontrols.png)](</File:bgracontrols.png>)

## Contents

  * 1 Install
    * 1.1 Optional Components
    * 1.2 Website
  * 2 BGRA Controls
    * 2.1 TBCButton
    * 2.2 TBCButtonFocus
    * 2.3 TBCGameGrid
    * 2.4 TBCImageButton
    * 2.5 TBCXButton
    * 2.6 TBCLabel
    * 2.7 TBCMaterialDesignButton
    * 2.8 TBCPanel
    * 2.9 TBCRadialProgressBar
    * 2.10 TBCToolBar
    * 2.11 TBCTrackBarUpdown
    * 2.12 TBGRAFlashProgressBar
    * 2.13 TBGRAGraphicControl
    * 2.14 TBGRAImageList
    * 2.15 TBGRASVGImageList
    * 2.16 TBGRAImageManipulation
    * 2.17 TBGRAKnob
    * 2.18 TBGRAResizeSpeedButton
    * 2.19 TBGRAShape
    * 2.20 TBGRASpeedButton
    * 2.21 TBGRASpriteAnimation
    * 2.22 TBGRAVirtualScreen
    * 2.23 TDTAnalogClock
    * 2.24 TDTAnalogGauge
    * 2.25 TDTThemedClock
    * 2.26 TDTThemedGauge
    * 2.27 TPSImport_BGRAPascalScript
  * 3 BGRA Custom Drawn
    * 3.1 TBCDButton
    * 3.2 TBCDEdit
    * 3.3 TBCDStaticText
    * 3.4 TBCDProgressBar
    * 3.5 TBCDSpinEdit
    * 3.6 TBCDCheckBox
    * 3.7 TBCRadioButton
  * 4 Sample code
    * 4.1 Pascal Script Library
    * 4.2 BGRA Ribbon Custom
    * 4.3 Tests
    * 4.4 Tests Extra
  * 5 Other units
    * 5.1 BCEffect
    * 5.2 BCFilters
    * 5.3 BGRAScript
  * 6 Related Articles



## Install

Use the [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") to get BGRABitmap and BGRAControls. 

Notice that you must check only the packages "bgrabitmappack.lpk" and "bgracontrols.lpk" in the Online Package Manager. The other packages are optional and may need third party packages / libraries to work (OpenGL and PascalScript). 

### Optional Components

Since v4.4 the components TBCDefaultThemeManager, TBCKeyboard and TBCNumericKeyboard are not installed by default to allow Linux users to get a seamless installation with the Online Package Manager not installing third party stuff. If you want these components turn on the "Register unit" in the package options for each file (bcdefaulthememanager.pas, bckeyboard.pas, bcnumerickeyboard.pas) then compile and rebuild Lazarus. On Linux you need to install libxtst-dev and libgl-dev first. 

### Website

BGRABitmap Organization on GitHub: <https://github.com/bgrabitmap/>

## BGRA Controls

BGRA Controls is a set of graphical UI elements that you can use with Lazarus LCL applications. Under Linux you need to have installed libxtst-dev and libgl-dev. 

### TBCButton

[![bcbutton.png](https://wiki.freepascal.org/images/6/63/bcbutton.png)](</File:bcbutton.png>)

A button control that can be styled through properties for each state like StateClicked, StateHover, StateNormal with settings like gradients, border and text with shadows. You can assign an already made style through the property AssignStyle. 

### TBCButtonFocus

Like TBCButton but it supports focus like normal TButton. 

### TBCGameGrid

[![bcgamegrid.png](https://wiki.freepascal.org/images/7/7c/bcgamegrid.png)](</File:bcgamegrid.png>)

A grid with custom width and height of items and any number of horizontal and vertical cells that can be drawn with BGRABitmap directly with the OnRenderControl event. 

### TBCImageButton

[![samplebgraimagebutton.png](https://wiki.freepascal.org/images/5/56/samplebgraimagebutton.png)](</File:samplebgraimagebutton.png>)

[![samplebgraimagebuttonalpha.png](https://wiki.freepascal.org/images/d/d9/samplebgraimagebuttonalpha.png)](</File:samplebgraimagebuttonalpha.png>)

A button control that can be styled with one image file, containing the drawing for each state Normal, Hovered, Active and Disabled. It supports 9-slice scaling feature. It supports a nice fading animation that can be turned on. 

### TBCXButton

[![bcxbutton.png](https://wiki.freepascal.org/images/7/7e/bcxbutton.png)](</File:bcxbutton.png>)

A button control that can be styled by code with the OnRenderControl event. Or even better create your own child control inheriting from this class. 

### TBCLabel

[![bclabel.png](https://wiki.freepascal.org/images/7/79/bclabel.png)](</File:bclabel.png>)

A label control that can be styled through properties, it supports shadow, custom borders and background. 

### TBCMaterialDesignButton

A button control that has an animation effect according to Google Material Design guidelines. It supports custom color for background and for the circle animation, also you can customize the shadow. 

### TBCPanel

[![bcpanel.png](https://wiki.freepascal.org/images/3/3f/bcpanel.png)](</File:bcpanel.png>)

A panel control that can be styled through properties. You can assign an already made style through the property AssignStyle. 

### TBCRadialProgressBar

A progress bar with radial style. You can set the color and text properties as you like. 

### TBCToolBar

A TToolBar with an event OnRedraw to paint it using BGRABitmap. It supports also the default OnPaintButton to customize the buttons drawing. By default it comes with a Windows 7 like explorer toolbar style. 

### TBCTrackBarUpdown

A control to input numeric values with works like a trackbar and a spinedit both in one control. 

### TBGRAFlashProgressBar

[![BC-Bgraflashprogressbar.png](https://wiki.freepascal.org/images/0/0b/BC-Bgraflashprogressbar.png)](</File:BC-Bgraflashprogressbar.png>)

A progress bar with a default style inspired in the old Flash Player Setup for Windows progress dialog. You can change the color property to have different styles and also you can use the event OnRedraw to paint custom styles on it like text or override the entire default drawing. 

### TBGRAGraphicControl

Is like a paintbox. You can draw with transparency with this control using the OnRedraw event. 

### TBGRAImageList

An image list that supports alpha in all supported platforms. 

How app looks with TImageList (with transparent icons): 

[![before-TImageList.png](https://wiki.freepascal.org/images/a/aa/before-TImageList.png)](</File:before-TImageList.png>)

How app looks with BGRAImageList: 

[![after-TBGRAImageList.png](https://wiki.freepascal.org/images/3/3b/after-TBGRAImageList.png)](</File:after-TBGRAImageList.png>)

### TBGRASVGImageList

It is located in the BGRA Themes tab. 

[![bgrasvgimagelist edit.png](https://wiki.freepascal.org/images/4/4d/bgrasvgimagelist_edit.png)](</File:bgrasvgimagelist_edit.png>)

In its properties, one can define the size and a target raster image list (TImageList or TBGRAImageList): 

[![bgrasvgimagelist prop.png](https://wiki.freepascal.org/images/4/4e/bgrasvgimagelist_prop.png)](</File:bgrasvgimagelist_prop.png>)

This will automatically populate the target image list (here on MacOS, it provides the double sized icons for Retina): 

[![targetimagelist edit.png](https://wiki.freepascal.org/images/b/b8/targetimagelist_edit.png)](</File:targetimagelist_edit.png>)

### TBGRAImageManipulation

[![bgraimagemanipulation.jpg](https://wiki.freepascal.org/images/6/6a/bgraimagemanipulation.jpg)](</File:bgraimagemanipulation.jpg>)

A tool to manipulate pictures, see the demo that shows all the capability that comes with it. 

### TBGRAKnob

[![BC-Bgraknob.png](https://wiki.freepascal.org/images/3/39/BC-Bgraknob.png)](</File:BC-Bgraknob.png>)

A knob that can be styled through properties. 

### TBGRAResizeSpeedButton

A speed button that can resize the glyph to fit in the entire control. 

### TBGRAShape

[![samplebgrashape.png](https://wiki.freepascal.org/images/b/b8/samplebgrashape.png)](</File:samplebgrashape.png>)

A control with configurable shapes like polygon and ellipse that can be filled with gradients and can have custom borders and many other visual settings. 

### TBGRASpeedButton

[![BGRASpeedButton.png](https://wiki.freepascal.org/images/e/e9/BGRASpeedButton.png)](</File:BGRASpeedButton.png>)

A speed button that in GTK and GTK2 provides BGRABitmap powered transparency to the glyph. 

### TBGRASpriteAnimation

[![bgraspriteanimation.png](https://wiki.freepascal.org/images/e/ee/bgraspriteanimation.png)](</File:bgraspriteanimation.png>)

A component that can be used as image viewer or animation viewer, supports the loading of gif files. 

### TBGRAVirtualScreen

Is like a panel. You can draw this control using the OnRedraw event. 

### TDTAnalogClock

A clock. 

### TDTAnalogGauge

A gauge. 

### TDTThemedClock

Another clock. 

### TDTThemedGauge

Another gauge. 

### TPSImport_BGRAPascalScript

A component to load BGRABitmap pascal script utilities. 

## BGRA Custom Drawn

BGRA Custom Drawn is a set of controls inherited from Custom Drawn. These come with a default dark style that is like Photoshop. 

### TBCDButton

A button control that is styled with TBGRADrawer. 

### TBCDEdit

An edit control that is styled with TBGRADrawer. 

### TBCDStaticText

A label control that is styled with TBGRADrawer. 

### TBCDProgressBar

A progress bar control that is styled with TBGRADrawer. 

### TBCDSpinEdit

A spin edit control that is styled with TBGRADrawer. 

### TBCDCheckBox

A check box control that is styled with TBGRADrawer. 

### TBCRadioButton

A radio button that is styled with TBGRADrawer. 

## Sample code

BGRA Controls comes with nice demos to show how to use the stuff and extra things you can use in your own projects. 

### Pascal Script Library

Putting BGRABitmap methods into a .dll with c#, java and pascal headers. 

### BGRA Ribbon Custom

How to create a fully themed window using the controls to achieve a Ribbon like application. 

### Tests

There are test for analog controls (clock and gauge), BC prefixed controls, BGRA prefixed controls, BGRA Custom Drawn controls, how to use Pascal Script and BGRABitmap, bgrascript or how to create your own scripting solution with BGRABitmap. 

### Tests Extra

[![game maze.png](https://wiki.freepascal.org/images/0/01/game_maze.png)](</File:game_maze.png>)

[![game puzzle.png](https://wiki.freepascal.org/images/1/1f/game_puzzle.png)](</File:game_puzzle.png>)

[![customdrawnwindows7.png](https://wiki.freepascal.org/images/7/78/customdrawnwindows7.png)](</File:customdrawnwindows7.png>)

[![slicescaledtachart.png](https://wiki.freepascal.org/images/7/70/slicescaledtachart.png)](</File:slicescaledtachart.png>)

These are extra tests like how to use fading effect, an fpGUI theme, games like maze and puzzle, how we created the material design animation, pix2svg or how to convert a small picture to svg using hexagons, rectangles and ellipses, plugins or how to load .dlls and use into a TBGRAVirtualScreen, rain effect, shadow effect, 9-slice-scaling with Custom Drawn or how to theme with bitmaps an application to look like Windows themes and 9-slice-scaling with charts. 

## Other units

These units come with BGRA Controls and contains more functionality that is sometimes used with the controls, sometimes not but are usefull in some way. Some are listed here, others you can see linked directly with any control like bcrtti, bcstylesform, bctools and bctypes. 

### BCEffect

Fading effect with BGRABitmap. 

### BCFilters

A set of pixel filters to use with BGRABitmap. 

### BGRAScript

Scripting with BGRABitmap, see test project. 

## Related Articles

[BGRASpriteAnimation](<BGRASpriteAnimation.md> "BGRASpriteAnimation") \- Usage of the sprite animation component. 

[uE_Controls](<uE_Controls.md> "uE Controls") \- Other controls developed with BGRABitmap. 

[BGRABitmap](<BGRABitmap.md> "BGRABitmap") \- The library used to create this controls. 

[LazPaint](<LazPaint.md> "LazPaint") \- A paint program developed with Lazarus and BGRABitmap.

---

_Source: [https://wiki.freepascal.org/BGRAControls](https://web.archive.org/web/20250318120105/https://wiki.freepascal.org/BGRAControls)_
