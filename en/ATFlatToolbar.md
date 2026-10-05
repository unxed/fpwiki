# About

ATFlatToolbar is component which creates row of [ATButton](<ATButton.md> "ATButton")'s on it, this looks like toolbar. 

It don't support creating toolbar in IDE design time. You need to call methods: 

  * AddButton: to add usual button (with caption or not)
  * AddDropdown: to add button with attached drop-down menu
  * AddChoice: to add button which acts like a combobox, with Items and ItemIndex
  * AddSep: to add button which looks like "|" separator line
  * UpdateControls: this updates buttons placement, you must call it after changes (also after ImageList size is changed)



[![atbuttonstoolbar demo.png](https://wiki.freepascal.org/images/1/11/atbuttonstoolbar_demo.png)](</File:atbuttonstoolbar_demo.png>)

  * Use props ButtonCount and Buttons[i] to get ATButton's from component.
  * Use Buttons[i].Free to delete buttons.
  * Use prop Vertical for make vertical layout.



Author: Alexey Torgashin 

# License

MPL 2.0 or LGPL. 

# Download

GitHub page: <https://github.com/Alexey-T/ATFlatControls>

---

_Source: [https://wiki.freepascal.org/ATFlatToolbar](https://web.archive.org/web/20250418095107/https://wiki.freepascal.org/ATFlatToolbar)_
