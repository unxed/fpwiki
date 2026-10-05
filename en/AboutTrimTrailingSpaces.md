# AboutTrimTrailingSpaces

Lazarus original behaviour (0.9.24) was that pressing a space at the end (or past the end) of line would not insert this space, but simple move the caret. 

  * Tabs:



While this seems a good and easy approach in first, it brings up some problems. Press tab (assuming you work with real tabs, and do not substitute them with spaces) at the end of line. The caret would move the correct amount of positions, but the tab was lost (since no text was inserted). Pressing a none-whitespace char now, would fill the line with spaces. This problem would be most noticeable, if you are on an empty line, and wish to intend it with a tab. It was not possible. 

The minimum solution would be to just store the space/tab mix that brought you do the caret position, and use it to fill the line, if a none-whitespace was inserted. But ignore it totally for anything else (So the end key jumps to the last none-whitespaces). 

  * Tabs and backspace:



This again would be inconsistent. If the last key you pressed was a tab (and inserted a real tab, not spaces), and you press backspace, you expect to delete the whole tab, not just go one single position to the left. A tab at the end of the line, should be deleted the same way than a tab in the middle of the line. (otherwise you had half a tab in the memory, if a none whitespace was inserted) So now you need to use the remembered mix for backspace too. > This is especially important, if your auto-indent copied tabs from the previous line, and you want to un-indent by one tab <

This differs from the original implementation where the editor never inserted the trailing whitespace, and *all* actions behaved as if they (the spaces) were not there. There are at least some actions that are more consistent, if there behaviour is based on the (temporarily) existence of those spaces. 

  * Caret Left/Right



Once you do this, the next question is, how caret left/right should behave. If "caret past EOL" is "off", then caret-right jumps to the next line, if pressed at EOL. If you type a few trailing spaces at the end of the line there are 2 possible behaviours for the sequence "caret-left" - "caret-right" 1) go one left, jump to the next line (as you are at the end of line). This is strange, because you went one left from the end of line, and still were at the end of line.... 2) go one left; and going one right behave as everywhere else in the code => you end up on the same location as before 

Yet bot are valid expected behaviours 

  * End Key



Having looked at all this, the behaviour of the end key, can probably depend on the Caret Left/Right. If caret-right can see the trailing space, then so should the end key. (Again there is the case where neither of them will see it) > Of course the behaviour must be defined for both Normal, and smart end-key <

  *     *       * 


So trim trailing spaces must have a well defined behaviour for 

  *     * dealing with tabs
    * handling backspace inside the trailing whitespaces
    * handling caret left/right
    * handling end-key


  *     *       * 


currently we have 4 modes: 

  *     * On leave line
    * On leave or Edit line
    * On any caret move or edit line
    * Position Only (looses tabs)



I do not see a case for a "store whitespace, but use only for padding" mode. This would be remember the tabs, if a none-whitespace is inserted, but do not remember it for backspace, end and caret moves. 

  
[See Ide Editorr Options Help for how to setup your IDE](<IDE_Window__Editor_Options_General_Misc.md> "IDE Window: Editor Options General Misc")

---

_Source: [https://wiki.freepascal.org/AboutTrimTrailingSpaces](https://web.archive.org/web/20240714065127/https://wiki.freepascal.org/AboutTrimTrailingSpaces)_
