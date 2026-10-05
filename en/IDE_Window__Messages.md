# IDE Window: Messages

│ **English (en)** │

The Messages window contains the compiler messages. It can also contain codetools messages, or linker errors, etc.. 

[![IDEMessagesWindowFind1.png](https://wiki.freepascal.org/images/a/ae/IDEMessagesWindowFind1.png)](</File:IDEMessagesWindowFind1.png>)

## Contents

  * 1 Overview
  * 2 Popup menu
    * 2.1 Find
    * 2.2 Quick fixes
    * 2.3 About
    * 2.4 Filter all messages of type
    * 2.5 Filter non urgent messages
    * 2.6 Filter hints without source position
    * 2.7 Switch filter settings
    * 2.8 Copy
    * 2.9 Save
    * 2.10 Help
    * 2.11 Edit Help
    * 2.12 Clear
    * 2.13 Options
  * 3 Options
    * 3.1 Colors
    * 3.2 Miscellaneous
  * 4 Notes and Tips



# Overview

When compiling the Messages window is shown automatically. You can see messages appear briefly and vanishing. The IDE classifies the compiler messages. Messages like "compiling unit1.pas" are treated as "progress" lines. These are shown, but hidden as soon as the next messages is shown, so that only the important messages (e.g. hints, notes, warning, errors) are shown. You can choose what lines to show, see filtering below. 

Click on a message to jump to the position in the source editor. You can set whether a double click jumps or a single click in Tools -> Options -> Messages Window -> Double click on messages jumps. 

When the IDE compiles a project it also compiles changed packages. In this case you see several blocks of messages, each having a header. The header is yellow while compiling, it turns green when compilation was successful, and turns red when there was an error. You can change the colors in the options. 

# Popup menu

## Find

Shows a search function at the bottom of the Messages window. Click on the left button to hide the search. Edit the text to highlight all places containing the text and to jump to the first message (starting from the selected). Click on the arrows to jump to the next or previous place. 

## Quick fixes

Below **Find** there can be arbitrary quick fixes. These are added depending on the current message and what IDE add-ons you have installed. See [Quick Fixes](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools"). 

## About

Shows a dialog containing information what task has created this output. For example a message from compiling a package gives the reason why the IDE needed to compile the package, the compiler path and parameters. 

## Filter all messages of type

This menu item only appears if the selected message has a type (e.g. a compiler note has, a header does not). Each compiler message has an ID, which can be hidden by passing -vmID to the compiler. Clicking on this option adds the flag to the compiler options. This requires a recompile to take effect. 

## Filter non urgent messages

Set the level when messages are shown. Levels are Warnings, Notes, Hints, Debug. You cannot hide errors. You can choose to not filter by urgency level by selecting "Filter none, do not filter by urgency". Default is to show all Hints and above. 

## Filter hints without source position

When enabled hides the messages without source position and urgency level below error. Errors and other important messages are always shown. Default is enabled. 

## Switch filter settings

This sub menu contains an Add item and your list of filter sets. You can store your current filter set by clicking on "Add filter...". For example you might want to create one set for showing all and one for showing only the important messages. 

## Copy

  * Copy file name to clipboard - copy the file name of the selected message
  * Copy selected messages to clipboard - copy the full text of the selected message as shown in the Messages window
  * Copy all shown messages to clipboard - copy all text in the Messages window
  * Copy all original messages to clipboard - copy the original output of the compiler



## Save

  * Save shown messages to clipboard - save all text in the Messages window to a file of your choice
  * Save all original messages to clipboard - save the original output of the compiler to a file of your choice



## Help

Show Help for the current message. For FPC messages it usually shows the note of the compiler errore.msg file. This can be overriden for each message with Edit Help. 

## Edit Help

Shows a dialog to edit the special help for FPC messages. 

[![LazarusEditorFPCMessageHelp.png](https://wiki.freepascal.org/images/c/c5/LazarusEditorFPCMessageHelp.png)](</File:LazarusEditorFPCMessageHelp.png>)

## Clear

Wipes out the Messages window clean. 

## Options

  * **Windows stay on top** \- enable to show the Messages window on top
  * **Filename style** \- defines how source file names are shown 
    * **Short, no path** \- e.g. unit1.pas
    * **Relative** \- relative to the working directory of the process, e.g. ../folder/unit1.pas
    * **Full** \- e.g. /home/user/pascal/folder/unit1.pas
  * **Translate the English messages** \- uses the FPC message file in **Tools / Options/ Files / Compiler messages language file** to show the messages in another language, e.g. _$(FPCSrcDir)/compiler/msg/errordu.msg_ for German. The translation is done during compile. Changing this setting does not change old messages in the Messages window.
  * **Show message type ID** \- show the FPC message ID in brackets.
  * **More...** \- shows the **Tool / Options / Messages Window**



# Options

[![MessagesWindowOptions1.png](https://wiki.freepascal.org/images/b/b2/MessagesWindowOptions1.png)](</File:MessagesWindowOptions1.png>)

## Colors

  * **Header colors** : 
    * **Background** \- the background color behind the text
    * **Tool Header: Running** \- the background color of a header line, while the compiler is compiling.
    * **Tool Header: Success** \- the background color of a header line, when the compiler successfully finished
    * **Tool Header: Failed** \- the background color of a header line, when there was an error
    * **Tool Header: Scrolled up** \- the lower background color of a header line, when the block was scrolled down and some messages of the block are hidden above.
    * **Text** \- the default text color



[![MessagesWindowOptionMsgColors.png](https://wiki.freepascal.org/images/8/85/MessagesWindowOptionMsgColors.png)](</File:MessagesWindowOptionMsgColors.png>)

  * **Message colors** : use these to override the default text color for each message urgency level (Hint, Warning, Error, etc...)



## Miscellaneous

  * **Show Messages Icons** \- show an icon left of each message depending on urgency level (Hint, Warning, Error, etc...)
  * **Always draw selected items focused** \- When enabled draw focused even if the Messages window does not have focus.
  * **Focus messages after compilation** \- When compilation stops, focus the Messages window.
  * **Maximum parallel processes, 0 means default** \- By default (0) the IDE guesses the number of cores and uses up to this number of parallel processes to compile.
  * **Show FPC message "lines compiled"** \- By default this message is filtered. Enable to see this line, which basically gives you a footer for a successful built.



# Notes and Tips

  * MacOS trackpad users: the Control + track pad click (right click or secondary click) does not work. Please use the two finger click, or whatever you set in System Preferences / Trackpad / Secondary click.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Messages](https://web.archive.org/web/20250117070841/https://wiki.freepascal.org/IDE_Window%3A_Messages)_
