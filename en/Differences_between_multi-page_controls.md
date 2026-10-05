# Differences between multi-page controls

[![](https://wiki.freepascal.org/images/9/95/PageControlsComparison.png)](</File:PageControlsComparison.png>)

[](</File:PageControlsComparison.png> "Enlarge")

Example renderings of TPageControl and TTabControl with a TNotebook as a child control. TMemos added as examples (set to borderstyle bsNone, align alClient, font size 14).

[![](https://wiki.freepascal.org/images/9/98/PageControlsComparison2.png)](</File:PageControlsComparison2.png>)

[](</File:PageControlsComparison2.png> "Enlarge")

To emphasize the differences in outline rendering the TMemos have been shrunken in size. Also TPageControl tabs now have glyphs while the TTabControl does not support them.

[![](https://wiki.freepascal.org/images/7/75/Structure1.png)](</File:Structure1.png>)

[](</File:Structure1.png> "Enlarge")

Structure for the examples used. Note how TNotebook has only one content page (but could provide more) while the TTabControl has four tab buttons (independent controls). TPageControl has one TTabSheet for each page with the content being on the second one in this example.

Lazarus (as of version 3.6 at time of writing) provides [TPageControl](<TPageControl.md> "TPageControl") and separate [TTabControl](<TTabControl.md> "TTabControl") and [TNotebook](<TNotebook.md> "TNotebook") controls for multi-page use cases (as Delphi also does). 

However, even though TNotebook and TTabControl can mimic the behavior and appearance of a TPageControl, there are differences between the controls, which this article will highlight upon. 

Additionally there are the TExtendedNotebook and TExtendedTabControl on the [LazControls](<LazControls.md> "LazControls") tab, which will be handled in a separate section below. 

## Contents

  * 1 General Functionality
  * 2 Outline rendering
  * 3 Page types
  * 4 Tab button icons (Glyphs)
  * 5 Properties comparison
  * 6 Specialised controls
  * 7 Workaround (as of Lazarus 3.6)



## General Functionality

TPageControl is immediately familiar from many Windows applications and menus. In short: It provides an area where you can switch between tabs and have each page show a different content, which also can be used this way at design-time (swiching pages via context menu or changing the tabindex in the Object Inspector, which defines which tab - or _page_ rather - is currently visible). 

The functionality can be split up into a control providing the display of tab buttons and an area which provides switchable pages, which is covered by TTabControl and TNotebook respectively. Splitting them up yields more flexibility when trying to solve use cases where a switchable page mechanism is needed but the changing of the pages has to be different than just having tabs to click - for example in a automatic slideshow kind of way (other example use cases left to your imagination here). 

## Outline rendering

In the picture on the right, you have a side-by-side view of a TPageControl and a TTabControl with a TNotebook arranged in a way to most closely resemble the TPageControl appearence. As you might have spotted, TPageControl shows a beveled outline which you cannot get rid of with standard means. 

The combination of controls on the right, thus especially the rendering of a TNotebook control, does not have a beveled outline. This can best be seen on the right and bottom sides next to the scrollbars. The extra beveled outline can have annoying visual effects if you need to attach other controls to the right or bottom of a TPageControl. Best check this out in advance to assess if a TNotebook _might_ be the better option (yet, also check the section on glyphs below, if this is applicable). 

## Page types

A TPageControl will expose objects of the type TTabSheet in the Object Inspector, representing the content page including its tab button. While a TNotebook will expose its subpages of the type TPage. 

Both page types are quite similar in nature, yet they are not type compatible - a TPage does not include any properties that belong to the tab buttons, as a TTabControl will have to handle this. TTabControl on the other hand does not manage any real subpages, it just uses a TStrings list to maintain the tab captions and provides a static client area to allow other components be added there. This situation is inferior to TPageControl, because TTabControl cannot (or at least does not) provide the same base features - especially when it comes to displaying icons in the tab button. 

## Tab button icons (Glyphs)

TPageControl provides an images property for specifing a [TImageList](<TImageList.md> "TImageList") with icons to use. Each subpage TTabSheet then has a property for an imageindex into this image list to specify which icon should be shown in the tab caption. 

While TNotebook also provides a TImageList property, neither TPage nor TTabControl provide a property for an imageindex. TTabControl manages the "tabs" as a pure list of TStrings defining the caption text only. As of Lazarus 3.6 it seems not to be possible to have icons showing up in the TTabControl. 

## Properties comparison

The following gallery shows all properties in the Object Inspector for the components mentioned: 

  * [![TPageControl](https://wiki.freepascal.org/images/f/fd/TPageControl-Inspector.png)](</File:TPageControl-Inspector.png> "TPageControl")

_TPageControl_

  * [![TTabControl](https://wiki.freepascal.org/images/1/13/TTabControl-Inspector.png)](</File:TTabControl-Inspector.png> "TTabControl")

_TTabControl_

  * [![TabSheet of TPageControl](https://wiki.freepascal.org/images/d/dd/TTabSheet-Inspector.png)](</File:TTabSheet-Inspector.png> "TabSheet of TPageControl")

_TabSheet of TPageControl_

  * [![TNotebook](https://wiki.freepascal.org/images/e/ee/TNotebook-Inspector.png)](</File:TNotebook-Inspector.png> "TNotebook")

_TNotebook_

  * [![TPage of TNotebook](https://wiki.freepascal.org/images/a/ab/TPage-Inspector.png)](</File:TPage-Inspector.png> "TPage of TNotebook")

_TPage of TNotebook_




## Specialised controls

Lazarus additionally provides the components [TExtendedNotebook](</index.php?title=TExtendedNotebook&action=edit&redlink=1> "TExtendedNotebook \(page does not exist\)") and [TExtendedTabControl](</index.php?title=TExtendedTabControl&action=edit&redlink=1> "TExtendedTabControl \(page does not exist\)") in the [LazControls](<LazControls.md> "LazControls") tab. They are used in the Lazarus IDE itself, but are also available for own projects this way. 

The main difference for the TExtendedTabControl is that it provides a wrapper for a TToolbar which is displayed right aligned on the tab header level. 

The TExtendedNotebook however provides an integrated tab header again. In fact making it the same as a TPageControl. It even exposes TTabSheets and not TPage subpage elements. And also renders exactly like a TPageControl with visible borders around the subpages. 

## Workaround (as of Lazarus 3.6)

To have a consistent and clean rendering of the outline frame - or more precise: having no visible outline around the TPageControl - a TNotebook seems to be the way to go. To also have icons in the tab header, you cannot use a TTabControl though - but, you can "misuse" a TPageControl for that. In fact you have to resize the TPageControl in a way that just the header section is visible. Below that you place a TNotebook. Maybe combine both in a TPanel to make things easier. After that you can setup the icons to be used in the TPageControl and you need to add code that reacts on tab changes in the TPageControl and consequently activates the right page in the TNotebook.

---

_Source: [https://wiki.freepascal.org/Differences_between_multi-page_controls](https://web.archive.org/web/20260115111836/https://wiki.freepascal.org/Differences_between_multi-page_controls)_
