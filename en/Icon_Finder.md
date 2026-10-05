# Icon Finder

## Contents

  * 1 Overview
  * 2 Installation
  * 3 Usage
    * 3.1 Adding an icon to TImageList
    * 3.2 Adding an icon to the TImage.Picture, TBitBtn.Glyph or TSpeedButton.Glyph
  * 4 Configuration



# Overview

**IconFinder** is a tool which simplifies searching of specific icons for the image-related components (TImageList, TImage, TBitBtn, TSpeedButton). 

Designtime package: components/iconfinder/packages/designtime/iconfinder_dsgn_pkg.lpk 

Runtime package: components/iconfinder/packages/runtime/iconfinder_pkg.lpk 

# Installation

  * Install the "iconfinder_dsgn_pkg" package via "Package" -> "Add/Remove Packages ..." dialog.
  * Click "Rebuild IDE", and confirm the question to rebuild the IDE.
  * When the IDE restarts the TImageList and TPicture component/property editors have a new option to load an icon from the icon library. There is also an option in "Tools" > "Options"



# Usage

## Adding an icon to TImageList

  * Double-click on the TImageList component on the form to open the TImageList component editor.
  * Click on "Add" > "Add from Icon Finder" to open the Icon Finder window.
  * Type the search keyword into the "Enter keywords here..." field, and the icon finder will display all icons for this keyword. (Or click on '...' to open a windows with a keyword editor listing all available keywords).
  * Double-click on the icon required to add the icon to the image list. Note that when several image sizes are registered for the image list then images of all these sizes will be added.



## Adding an icon to the TImage.Picture, TBitBtn.Glyph or TSpeedButton.Glyph

  * Click on the '...' next to the "Picture" or "Glyph" property, respectively, to open TPicture editor.
  * Click on "Icon Finder" to open the Icon Finder Window.
  * Proceed like above.



# Configuration

Icon Finder adds a new option "Icon Finder" > "General" to the "Tools" > "Options" menu. 

Here you can 

  * Add/remove/re-order directories with icons ("Folders..." button) 
    * The Icon Finder by default looks for icons in the "images/general_purpose" folder of the Lazarus installation.


  * add/edit keywords and other metadata ("style") for each icon 
    * ("Metadata..." button): Double-click on the icon to open the metadata editor where you can enter keywords (one per line) and select the icon "style".

---

_Source: [https://wiki.freepascal.org/Icon_Finder](https://web.archive.org/web/20250115082315/https://wiki.freepascal.org/Icon_Finder)_
