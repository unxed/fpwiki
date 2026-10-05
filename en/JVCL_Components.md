# JVCL Components

│ **English (en)** │  **[русский (ru)](<../ru/JVCL_Components.md> "JVCL Components/ru")** │    
****

  


## Contents

  * 1 About
  * 2 Packages
    * 2.1 JvCore
    * 2.2 JvStdCtrls
    * 2.3 JvCtrls
    * 2.4 JvCustom
    * 2.5 JvCmp
    * 2.6 JvDB
    * 2.7 JvAppFrm
    * 2.8 JvHMI
    * 2.9 JvJans
    * 2.10 JvMM
    * 2.11 JvPageComps
    * 2.12 JvPascalInterpreter
    * 2.13 JvRuntimeDesign
    * 2.14 JvTimeFramework
    * 2.15 JvValidators
    * 2.16 JvWizard
    * 2.17 JvXPCtrls
  * 3 Download and installation
    * 3.1 Download
    * 3.2 Installation



## About

JVCL is a library of more than 600 Delphi visual and non-visual components (<https://github.com/project-jedi/jvcl>). Although heavily Windows-centered, some of them have been ported to Lazarus. This page intends to give a summary of the JVCL components available for Lazarus. Some of the info snippets were taken from <http://wiki.delphi-jedi.org/wiki/JVCL_Component_Overview>. 

## Packages

Due to the large number of components the original Delphi version is split into a variety of packages. The Lazarus port follows this convention because the user can install only the functionality needed. 

For each package, a runtime and designtime version is available. Runtime packages can be identified with an appended R, designtime packages with an appended D. In the requirements for a project always use the runtime package. 

### JvCore

Provides basic functionality needed by all other packages. 

### JvStdCtrls

Depends on package _JvCore_.   
Enhanced standard controls 

  * **TJvCheckbox** : Has a property `LinkedControls`. Linked controls can be enabled/disabled according to the `Checked` state of the Checkbox.
  * **TJvPanel** : an extended panel with hot-tracking effects and automatic placement of child components (similar to LCL's `ChildSizing` property). Can be dragged and resize at runtime. See example project _JvPanelDemo_.
  * **JvCalcEdit** : a numeric edit control which opens a calculator dialog.



### JvCtrls

Depends on packages _JvCore_ and _JvStdCtrls_.   
Visual controls 

  * **TJvBehaviorLabel** : a label with special display effects: Blinking, bouncing, scrolling, typing, appearing, "special", "code breaker". See demo _examples/JvBehaviorLabel_.
  * [![tjvmovablebevel.png](https://wiki.freepascal.org/images/1/1a/tjvmovablebevel.png)](</File:tjvmovablebevel.png>) **TJvMovableBevel** : a bevel which can be dragged and resized at runtime
  * [![tjvmovablepanel.png](https://wiki.freepascal.org/images/e/e5/tjvmovablepanel.png)](</File:tjvmovablepanel.png>) **TJvMovablePanel** : the same, but now for a panel. Note that this component does not belong to JVCL, it is a simple adaption of TJvMovableBevel to the panel case.
  * [![tjvruler.png](https://wiki.freepascal.org/images/3/3e/tjvruler.png)](</File:tjvruler.png>) **TJvRuler** : a ruler to indicate distances in centimeters, inches or pixels
  * [![tjvgroupheader.png](https://wiki.freepascal.org/images/2/2a/tjvgroupheader.png)](</File:tjvgroupheader.png>) **TJvGroupHeader** : header with attached bevelled line.
  * [![tjvrollout.png](https://wiki.freepascal.org/images/5/54/tjvrollout.png)](</File:tjvrollout.png>) **TJvRollOut** : expandable panel with headerline.
  * [![tjvhint.png](https://wiki.freepascal.org/images/8/8d/tjvhint.png)](</File:tjvhint.png>) [![tjvhtlabel.png](https://wiki.freepascal.org/images/f/f8/tjvhtlabel.png)](</File:tjvhtlabel.png>) [![tjvhtlistbox.png](https://wiki.freepascal.org/images/b/b5/tjvhtlistbox.png)](</File:tjvhtlistbox.png>) [![tjvhtcombobox.png](https://wiki.freepascal.org/images/d/df/tjvhtcombobox.png)](</File:tjvhtcombobox.png>) **TJvHtLabel** , **TJvHTCombobox** , **TJvHTListbox** and **TJvHint** : a set of controls which can display HTML text



[![JvHTCcontrols.png](https://wiki.freepascal.org/images/2/26/JvHTCcontrols.png)](</File:JvHTCcontrols.png>)

  * **TJvComboListBox** : a listbox which can display a combobox overlaying the selected listbox item. Assign a TPopupMenu to the DropdownMenu property and it will be shown when the combo button is clicked. You can also handle the OnDropDown event for custom handling when the button is clicked, or example, displaying a drop down form. These features are demonstrated in the _JvComboListBox_ example project.
  * **TJvOfficeColorPanel** : a panel for color selection designed like the one used by Office 97.



[![TJvOfficeColorPanel.png](https://wiki.freepascal.org/images/c/cb/TJvOfficeColorPanel.png)](</File:TJvOfficeColorPanel.png>)

  * **TJvLookupAutoComplete** : adds autocompletion of TEdit by ListBox or StringList items. (In the original JVCL, this component is in the Core package.)



### JvCustom

Depends on package _JvCore_   
Custom components 

  * [![tjvvalidateedit.png](https://wiki.freepascal.org/images/6/64/tjvvalidateedit.png)](</File:tjvvalidateedit.png>) **TJvValidateEdit** : General purpose edit control for text and numbers (integer, float, currency). MaxValue, MinValue, HasMaxValue, HasMinValue - Value is corrected when Edit loses focus. See demo in _JvValidateEdit_.
  * [![tjvtimeline.png](https://wiki.freepascal.org/images/c/c1/tjvtimeline.png)](</File:tjvtimeline.png>) **TJvTimeLine** : a one-dimensional calendar ("time-line") which can display various events and tasks. See demo _JvTimeLine_.
  * [![tjvtmtimeline.png](https://wiki.freepascal.org/images/1/1e/tjvtmtimeline.png)](</File:tjvtmtimeline.png>) **TJvTMTimeLine** : another timeline component. See demo _JvTMTimeLine_.
  * [![tjvgammapanel.png](https://wiki.freepascal.org/images/c/c3/tjvgammapanel.png)](</File:tjvgammapanel.png>) **TJvGammaPanel** : advanced color selection tool for foreground and background colors. See demo _JvGammaPanel_.
  * [![tjvthumbview.png](https://wiki.freepascal.org/images/9/9d/tjvthumbview.png)](</File:tjvthumbview.png>) **TJvThumbView** : displays a list of images as thumbnails
  * [![tjvoutlookbar.png](https://wiki.freepascal.org/images/6/6e/tjvoutlookbar.png)](</File:tjvoutlookbar.png>) **TJvOutlookBar** : an accordeon-like bar of containers for other controls as used by Outlook 97.
  * [![tjvimagesviewer.png](https://wiki.freepascal.org/images/3/33/tjvimagesviewer.png)](</File:tjvimagesviewer.png>) **TJvImagesViewer** : displays a list of images as thumbnails, similar to TJvThumbView. See _JvItemViewer_ demo.
  * [![tjvimagelistviewer.png](https://wiki.freepascal.org/images/7/78/tjvimagelistviewer.png)](</File:tjvimagelistviewer.png>) **TJvImageListViewer** : dispays all images of a TImageList. See _JvItemViewer_ demo.
  * [![tjvownerdrawviewer palette.png](https://wiki.freepascal.org/images/d/dc/tjvownerdrawviewer_palette.png)](</File:tjvownerdrawviewer_palette.png>) **TJvOwnerDrawViewer** : presents arbitrary information in a grid-like view. See _JvItemViewer_ demo.



[![tjvownerdrawviewer.png](https://wiki.freepascal.org/images/3/3a/tjvownerdrawviewer.png)](</File:tjvownerdrawviewer.png>)

  * **TJvChart** : ~~component for drawing time-series charts. Some bugs... See _JvChartDemo_ project.~~ Removed: Incomplete, too many bugs.



### JvCmp

Depends on package _JvCore_.  
Non-visual components 

  * **TJvStrHolder** , **TJvMultiStringHolder** : a wrapper component making it easier to work with TStrings and TStringList at design time. Using TStrHolder component you can set TStrings properties and event handlers in form designer and hold a number of strings in your forms.
  * **TJvSpellChecker** : a spell-checker class with sample dictionaries for English and Dutch. See demo in folder _examples/JvSpellChecker_ of the JVCL installation.



[![JvSpellChecker.png](https://wiki.freepascal.org/images/7/7e/JvSpellChecker.png)](</File:JvSpellChecker.png>)

  * **TJvProfiler** : --- to be written ---



### JvDB

Depends on packages _JvCore_ , _JvStdCtrls_ , and _JvCtrls_  
Data-aware controls 

  * **TJvSearchEdit** : a TEdit with incremental search capabilities within a field of a dataset.
  * **TJvDBLookupList** and **TJbDBLookupCombo** : lookup controls with additional features: 
    * can display more than one column (the LookupDisplay property can contain a list of columns separated by a semicolon, the column width is calculated based on Field.DisplayWidth)
    * can display pictures (OnGetImage/OnGetImageIndex events)
    * configurable empty value (DisplayEmpty,EmptyItemColor,EmptyStrIsNull,EmptyValue)
  * **TJvDBTreeView** : a treeview constructed from database records. Link the unique ID field to "MasterField", the ID field of the parent node to "DetailField", and the field with the node text to "ItemField". Optionally, the field "IconField" provides the image index for the ImageList. "StartMasterValue" is the beginning level to start building the TreeView, 0 = start from the root items, 1 = start from the second level, and so on.
  * **TJvDBCalcEdit** : a numeric edit box which opens a calculator dialog.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The packages _JvStdCtrlsLazR_ and _JvCtrlsLazR_ must have been compiled before this package can be installed.

### JvAppFrm

Depends on package _JvCore_  
Components for applications and forms 

  * **JvAppAnimatedIcon** and **JvFormAnimatedIcon** : display an animated icon (created from an imagelist) in the taskbar and in the form's title bar, respectively.
  * **JvAnimTitle** : provides an animated form title
  * **JvFormWallpaper** : tiles the background of a form with an image.



### JvHMI

Depends on package _JVCore_.   
The packages JvHDMILazR (run time) and JvHDMILazD (design time) provide some components for designing a "human-machine-interface" (HMI). 

  * **JvLED** : an LED indicator. See demo _examples/JvLED_.
  * **JvDialButton** : a rotatory knob. See demo _examples/JvDialButton_.



### JvJans

Depends on package _JvCore_.   
Named after the original author, Jan Verhoeven, who donated these components to the JEDI project. The following components have been ported: 

  * [**TJvSIM components**](<JvSIM_components.md> "JvSIM components"): a set of components for simulation of digital and analog electronics.
  * [![tjvjanled.png](https://wiki.freepascal.org/images/7/7b/tjvjanled.png)](</File:tjvjanled.png>) **JvJanLED** : an LED indicator component (not contained in original JVCL, ported from Jan Verhoeven's own repository).
  * [![tjvjantoggle.png](https://wiki.freepascal.org/images/1/1d/tjvjantoggle.png)](</File:tjvjantoggle.png>) **JvJanToggle** : a toggle button with an on and off area (not contained in original JVCL, ported from Jan Verhoeven's own repository).
  * [![tjvmarkupviewer.png](https://wiki.freepascal.org/images/8/82/tjvmarkupviewer.png)](</File:tjvmarkupviewer.png>) **JvMarkupViewer** and [![tjvmarkuplabel.png](https://wiki.freepascal.org/images/d/dd/tjvmarkuplabel.png)](</File:tjvmarkuplabel.png>) **JvMarkupLabel** : display simple HTML-formatted text (font-related tags only). Similar to JvHTControls.
  * [![tjvgridfilter.png](https://wiki.freepascal.org/images/c/c8/tjvgridfilter.png)](</File:tjvgridfilter.png>) **TvGridFilter** : a component for filtering rows in a column-oriented stringgrid. Call `Filter(condition)` to hide all rows meeting the specified condition string, e.g. `[Business]="Music"` removes all rows in which the strings in the column titled `Business` are equal to `Music`. Allowed operators are `=`, `<>`, `<`, `>`, or `like`. See demo in folder _JvGridFilterDemo_.
  * [![tjvyeargrid.png](https://wiki.freepascal.org/images/5/5d/tjvyeargrid.png)](</File:tjvyeargrid.png>) **JvYearGrid** : a year-calendar



[![TJvYearGrid.png](https://wiki.freepascal.org/images/8/8b/TJvYearGrid.png)](</File:TJvYearGrid.png>)

### JvMM

Depends on package _JvCore_.   
The original JVCL package for Delphi contains a set of multimedia components, image and some general-purpose components. 

  * [![tjvid3v1.png](https://wiki.freepascal.org/images/c/c9/tjvid3v1.png)](</File:tjvid3v1.png>) **JvID3v1** : a non-visual component for displaying and editing of ID3v1 tags of mp3 files. See demo _examples/JvID3v1_.
  * [![tjvid3v2.png](https://wiki.freepascal.org/images/4/4b/tjvid3v2.png)](</File:tjvid3v2.png>) **JvID3v2** : a non-visual component for displaying and editing of ID3v2 tags of mp3 files. See demo _examples/JvID3v2_.
  * [![tjvanimatedimage.png](https://wiki.freepascal.org/images/4/4e/tjvanimatedimage.png)](</File:tjvanimatedimage.png>) **JvAnimatedImage** : animates a series of images take from a glyph containing all images in a horizontal or vertical stripe.
  * [![tjvbmpanimator.png](https://wiki.freepascal.org/images/9/98/tjvbmpanimator.png)](</File:tjvbmpanimator.png>) **JvBmpAnimator** : animates a series of images taken from a TImagelist. Unlike suggested by its name, it works with other image types as well in Lazarus. See demo _examples/JvBmpAnimator_. Similar to JvAnimatedImage.
  * [![tjvimagetransform.png](https://wiki.freepascal.org/images/0/04/tjvimagetransform.png)](</File:tjvimagetransform.png>) **JvImageTransform** : image transition effects like in a simple slide show. See demo _examples/JvImageTransform_.
  * [![tjvspecialimage.png](https://wiki.freepascal.org/images/c/cd/tjvspecialimage.png)](</File:tjvspecialimage.png>) **JvSpecialImage** : an advanced image component with simple built-in image manipulation capabilities (flip, mirror, invert, fade-in, fade-out, brightness). Note that the Brightness property has been modified from the Delphi version to cover a more convenient scale from -100 to 100 (instead of 0 to 200). See demo _examples/JvSpecialImage_.
  * [![tjvspecialprogress.png](https://wiki.freepascal.org/images/a/ae/tjvspecialprogress.png)](</File:tjvspecialprogress.png>) **JvSpecialProgress** : an advanced progress bar with gradient and text display. See demo _examples/JvSpecialProgress_.
  * [![tjvgradient.png](https://wiki.freepascal.org/images/4/42/tjvgradient.png)](</File:tjvgradient.png>) **JvGradient** : draws several kinds of gradients (horizontal, vertical, elliptic, pyramidic).
  * [![tjvgradientheaderpanel.png](https://wiki.freepascal.org/images/4/4f/tjvgradientheaderpanel.png)](</File:tjvgradientheaderpanel.png>) **JvGradientHeaderPanel** : a header bar with a gradient background (drawn by JvGradient).
  * [![tjvfullcolorlabel.png](https://wiki.freepascal.org/images/3/3a/tjvfullcolorlabel.png)](</File:tjvfullcolorlabel.png>) **JvFullColorLabel** : a label with attached color sample
  * [![tjvfullcolordialog.png](https://wiki.freepascal.org/images/c/cc/tjvfullcolordialog.png)](</File:tjvfullcolordialog.png>) **JvFullColorDialog** and [![tjvfullcolorcircledialog.png](https://wiki.freepascal.org/images/a/a4/tjvfullcolorcircledialog.png)](</File:tjvfullcolorcircledialog.png>) **JvFullColorCircleDialog** : dialogs for color selection based on several color models. Use the [![tjvfullcolortrackbar.png](https://wiki.freepascal.org/images/9/98/tjvfullcolortrackbar.png)](</File:tjvfullcolortrackbar.png>) **JvFullColorTrackbar** for one-dimensional, and a [![tjvfullcolorpanel.png](https://wiki.freepascal.org/images/c/cd/tjvfullcolorpanel.png)](</File:tjvfullcolorpanel.png>) **JvFullColorPanel** and a [![tjvfullcolorcircle.png](https://wiki.freepascal.org/images/3/32/tjvfullcolorcircle.png)](</File:tjvfullcolorcircle.png>) **JvFullColorCircle** for two-dimensional color selection. There is also a swatch-like [![tjvfullcolorgroup.png](https://wiki.freepascal.org/images/3/37/tjvfullcolorgroup.png)](</File:tjvfullcolorgroup.png>) **JvFullColorGroup**. See demos _examples/JvFullColorDialog_ and _examples/JvFullColorCircleDialog_. Similar to [mbColorLib](<mbColorLib.md> "mbColorLib"), however, several color models are combined within the same control.



[![TJvFullColorDialog.png](https://wiki.freepascal.org/images/8/8c/TJvFullColorDialog.png)](</File:TJvFullColorDialog.png>)

  * [![tjvpicclip.png](https://wiki.freepascal.org/images/3/3b/tjvpicclip.png)](</File:tjvpicclip.png>) **JvPicClip** : similar to TImageList. Splices a large bitmap to a grid of `Cols x Rows` smaller images which can be retrieved as `Cells[ACol, ARow]`



### JvPageComps

Depends on packages _JvCore_ and _JvStdCtrls_   


  * **TJvTabBar** : a panel of tabs like in a TTabControl. Fully configurable. Drawing is delegated to separate painter components, **TJvModernTabBarPainter** (used by default) and **TJvTabBarXPPainter**. See demo in folder _examples/JvTabBar_.
  * **TJvPageList** : a stack of pages like a TNotebook. Designed to cooperate with TJvTabBar and TJvPageListTreeView. See demo in folder _examples/JvTabBar_PageList_.
  * **TJvNotebookPageList** : a TNotebook descendant designed to cooperate with TJvTabBar. See demo in folder _examples/JvTabBar_NotebookPages_. Note that this component is not contained in the original Delphi JVCL collection.
  * **TJvNavigationPane** : a navigation panel similar to MS Outlook. See demo in the folder _examples/JvNavigationPane_ of the Lazarus JVCL installation.



[![JvNavigationPane demo.png](https://wiki.freepascal.org/images/6/60/JvNavigationPane_demo.png)](</File:JvNavigationPane_demo.png>)

### JvPascalInterpreter

Scripting engine for Pascal. Depends on package _JvCore_. 

  * **TJvInterpreterProgram** : Non-visual component interpreting Pascal code. Add code to the `Pas` Strings, execute if by the `Run` method and get the result from the `VResult` variant. See the tutorials in folder _example/JvInterpreterDemos_. There is also a (German) tutorial at [[1]](<https://wiki.delphigl.com/index.php/Tutorial_Scripting_mit_JvInterpreterProgram>).



### JvRuntimeDesign

Depends on package _JvCore_. 

  * A runtime **form designer**. See demo in folder _example/JvDesigner_ of the JVCL installation. Note that the designer is not limited to the components shown on the demo's toolbar. Following the instructions in the header of the demo's main unit it is easy to register any other component.



[![JvDesignerDemo.png](https://wiki.freepascal.org/images/a/a4/JvDesignerDemo.png)](</File:JvDesignerDemo.png>). 

### JvTimeFramework

Depends on package _JvCore_. Calendars and schedule planner, similar to [TvPlanIt](<Turbopower_Visual_PlanIt.md> "Turbopower Visual PlanIt"). Can compare schedules of several persons. 

  * **TJvTFScheduleManager** : administration of data, access to database via events.
  * **TJvTFDays** , **TJvTFWeeks** or **TJvTFMonths** are day, week or month calendars, respectively, to display appointments.
  * **TJvTFDaysPrinter** : prints the appointments shown by TJvTFDays
  * **TJvTFGlanceTextViewer** : viewing parameters for TJvTFWeeks and TJvTFMonths



See extended demo in _examples/JvTimeFramework_

[![JvTimeFrameworkDemo.png](https://wiki.freepascal.org/images/9/9d/JvTimeFrameworkDemo.png)](</File:JvTimeFrameworkDemo.png>)

### JvValidators

Depends on package _JvCore_.   
Various validator classes for input validataion. See demo in the folder _examples/JvValidators_ of the Lazarus JVCL installation 

[![JvValidationDemo.png](https://wiki.freepascal.org/images/9/9e/JvValidationDemo.png)](</File:JvValidationDemo.png>)

  * **TJvErrorIndicator** : Shows error indicators next to edit-fields etc. similar to "missing fields" indicators in web forumlars.
  * **TJvValidators** : To validate user input. Rightclick on Field editor to define validation rules. Checks are performed when method "Validate" is called.



### JvWizard

Depends on package _JvCore_.   


  * A multiplage dialog as used, e.g., for installation programs. Use the component editor to add pages with title, subtitle, images etc. at designtime ("welcome page", "interior page"). Buttons to navigate to other pages are automatically added. Additional navigation tools can be provided by adding a `TJvWizardRouteMapSteps`, `TJvWizardRouteMapNodes`, or `TJvWizardRouteMapList` component. See demo _JvWizard_.



[![JvWizardDemo.png](https://wiki.freepascal.org/images/c/ce/JvWizardDemo.png)](</File:JvWizardDemo.png>)

### JvXPCtrls

Depends on package _JvCore_ and _JvStdCtrls_.   
Several controls imitating the look and feel of Windows XP. 

  * **JvXPButton** and **JvXPCheckbox** : a button and a checkbox in XP-style
  * **TJvXPProgressBar** : Draws a progressbar in XP-style. The color of the the background and of the blocks is configurable. Doesn't support Marquee-style.
  * **[JvXPBar](<JvXPBar.md> "JvXPBar")** : a container side-bar with collapsable panels. Similar to TJvRollOut-Panel, but with string items instead of controls. Collapsing/Expanding happens smooth and nicely animated.



[![JvXPBarDemo.png](https://wiki.freepascal.org/images/d/dd/JvXPBarDemo.png)](</File:JvXPBarDemo.png>)

## Download and installation

### Download

The developers version of the Lazarus port is available via svn from <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/jvcllaz/>. The current release version can be downloaded from <https://sourceforge.net/projects/lazarus-ccr/files/jvcllaz/>; it is also offered by the Lazarus Online Package Manager. 

### Installation

  * **Released version** distributed by the Online Package Manager: check the "jvcllaz" item in the Online Package manager, click the _"Install"_ button and follow the instructions.
  * **Development version** on Lazarus CCR (svn): 
    * If you want to install **all JVCL packages** , load the package **jvcl_all.lpk** and select "Use" > "Install". In older Lazarus versions, there will be a warning _"The package jvcl_all does not have any "Register" procedure which typically means it does not provide any IDE addon. Installing it will probably only increase the size of the IDE and may even make it unstable."_. You can ignore this and click on _"Install it, I like the fat"_.
    * If you want to install **only a few packages** , you must find out in above summary which other packages are needed. Compile the needed runtime packages (having an appended "R" at the package name). Then "Use" > "Install" the required designtime packages (having an appended "D" as the package name). It is required to rebuild the IDE only after the last package.

---

_Source: [https://wiki.freepascal.org/JVCL_Components](https://web.archive.org/web/20250323003132/https://wiki.freepascal.org/JVCL_Components)_
