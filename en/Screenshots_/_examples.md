# Screenshots / examples

## Look and Feel

This paragraph show several examples of look and feel concepts for Lazarus. On the right side, the current layout is shown. Please comment... 

General features are; 

  * each dialog has a help button (currently disabled) that will show a help page (hint: the Lazarus help system is format independent, so normally it is sufficient to add a unique help path, like 'IDE/CodeExplorer/RefreshButton').
  * all modal forms are bsSizeToolWin; non-modal forms are bsSizable.
  * to make Lazarus easier for non-English speakers I recommend you use button glyphs where you have a good icon.
  * all main controls are accessible with accelerator keys e. g. "&Cancel".



TODO: What about translations? The accelerator keys have to be written intelligently for each supported language. There may be some languages in which avoiding accelerator key clashes is almost impossible, even with very creative naming. 

  
[![Todo.png](https://wiki.freepascal.org/images/b/b6/Todo.png)](</File:Todo.png>) [![Todo old.png](https://wiki.freepascal.org/images/c/c3/Todo_old.png)](</File:Todo_old.png>)

The todo form shows a typical form with buttons. Instead of stretching the buttons across the entire clientwidth (code explorer, project inpector, etc.) a toolbar on top is shown with labeled buttons. 

  
[![UnitInformation.png](https://wiki.freepascal.org/images/d/d4/UnitInformation.png)](</File:UnitInformation.png>) [![UnitInfo.png](https://wiki.freepascal.org/images/9/9a/UnitInfo.png)](</File:UnitInfo.png>)

This form shows a typical information dialog. Notice the notebook that splits the information, so that there is not too much information visible at the same time. 

  
[![FindReplace.png](https://wiki.freepascal.org/images/5/5c/FindReplace.png)](</File:FindReplace.png>) [![FindReplace old2.png](https://wiki.freepascal.org/images/2/2a/FindReplace_old2.png)](</File:FindReplace_old2.png>)

This form shows a typical dialog. Captions before controls are right aligned (not easily seen in this example though) and the options are grouped under bold labels. 

### Proposal for preference forms

Here's a proposal for the preference forms used in Lazarus. I would like to focus on the use of treeviews instead of the traditional use of tabs. By using a treeview pages can more easily be divided (less options per page) and on the other hand more options per dialog could be used (perhaps some option dialogs can be merged?). Please add your comments below. 

[![Example optionsdlg.png](https://wiki.freepascal.org/images/b/b7/Example_optionsdlg.png)](</File:Example_optionsdlg.png>) [![Env options.png](https://wiki.freepascal.org/images/e/e7/Env_options.png)](</File:Env_options.png>)

#### Comments to: Proposal for preference forms

I like the old Environment options more. The treeview only wastes screen estate, and is IMHO too expensive for the marginal benefit (grouping of tabs) it provides. This benefit is also a bit doubtful since the number of screens will increase. Also, iconification in areas that aren't used several times a session are useless (and confusing), since nobody will be able to memorize them. 

Such dialogs are more tailored to initial computer users (and I still have to see that confirmed), but are wasteful for intermediate users. Since Lazarus is a programming environment, there will be more usage of this dialog by people already familiar with it, than by initial users.

---

_Source: [https://wiki.freepascal.org/Screenshots_/_examples](https://web.archive.org/web/20240528005544/https://wiki.freepascal.org/Screenshots_/_examples)_
