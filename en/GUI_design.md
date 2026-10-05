# GUI design

│ **[Deutsch (de)](</GUI_design/de> "GUI design/de")** │  **English (en)** │  **[français (fr)](</GUI_design/fr> "GUI design/fr")** │  **[日本語 (ja)](</GUI_design/ja> "GUI design/ja")** │    
****

## Contents

  * 1 Overview
  * 2 Comments
    * 2.1 Cross platform
    * 2.2 CAFE standard
  * 3 See also



## Overview

When working on lazarus, certain design guidelines need to be taken into consideration. To prevent a diversity of styles and ensure the clarity of dialogs. This article tries to summarize these guidelines to help you designing. There is also a buch of screenshots, artwork and dialog information available. 

  * [GUI design guidelines](<GUI_design_guidelines.md> "GUI design guidelines")
  * [Screenshots / examples](<Screenshots_/_examples.md> "Screenshots / examples")
  * [Lazarus dialogs information](<Lazarus_dialogs_information.md> "Lazarus dialogs information")
  * [Lazarus art work](<Lazarus_art_work.md> "Lazarus art work")



## Comments

### Cross platform

What you need to keep in mind is the cross-platform nature of Lazarus. If you make all the changes you suggest, the text on the buttons won't be visible in most standard GTK+ themes, or there will be other problems. I fully support the idea of some tweaks like this, but it should be tested in Linux, Win32 and even in Mac OS X (you guys are running it in Mac OS X already, right?) before it's committed. -[Tony Maro](<http://wiki.lazarus.freepascal.org/index.php/User:Tonymaro>)

    That's the only way to do it ;). I have patches of the different dialogs and I would encourage people to test them and post screenshots here. - Darius

### CAFE standard

I found these ideas on form designs very interesting. If you can, I would be grateful if you could visit <http://www.nvtech.com.au/ProjCurr/CAFE/CAFE.html> to read about a convention called CAFE; standing for "Common Application Front End". 

The intention of CAFE is not so much to have attractive forms but to have ones that reduce: 

  * training required for application users,
  * effort required to input data and navigate around the application, and
  * the chances of a user accidentally inputting incorrect data.



The keypoints of the CAFE standard are that it requires each form to: 

  * have a menu of options displayed (ie, not drop down menus or other hidden features)
  * be capable of being driven by both keyboard and pointing device with the option selection being done primarily by using numbers typed into the number pad on a conventional keyboard (tests have shown that clerks using the keyboard and number-pad are 60% faster than those that have to take their hands off a keyboard to use a pointing device.)
  * have a field devoted to "form help" which tells the user the purpose of the form and a field devoted to "field help" which changes as focus moves from field to field on the form. The "field help" informs the user of the purpose of the field and the input into that field which is acceptable.



If all contractors wrote their applications to conform with the CAFE standard, not only would it ensure an application that was best for business, but it would also allow many different contractors to participate in software development with all applications having exactly the same look and feel and so giving the perception they all belonged to one coherent applications suite. This would be good for everyone. 

I hope you find these ideas interesting. 

Kevin Loughrey 

CEO Non Volatile Technologies Pty Ltd 

(NVTech supports and promotes the use of Open Source Software as a means of accelerating societal advancement throughout the world.) 

## See also

  * [Lazarus look and feel, 16 Jul 2005](<http://www.lazarus.freepascal.org/list_archives/lazarus/msg08842.html>) Related discussions on the mailing list

---

_Source: [https://wiki.freepascal.org/GUI_design](https://web.archive.org/web/20250122112843/https://wiki.freepascal.org/GUI_design)_
