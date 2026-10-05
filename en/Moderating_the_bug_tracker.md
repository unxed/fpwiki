# Moderating the bug tracker

│ **English (en)** │  **[русский (ru)](<../ru/Moderating_the_bug_tracker.md>)** │

This document contains some guidelines for moderating the Lazarus [bug tracker](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues>).  
Moderating the bugtracker can only be done by Lazarus developers. 

## Contents

  * 1 Referring questions to the mailing list and forums
  * 2 Removing duplicate entries
  * 3 Adding relations to other issues
  * 4 Moving an issue to the fpc bug tracker
  * 5 Moving an issue to the Lazarus-CCR bug tracker
  * 6 Confirming issues
  * 7 Asking for feedback
  * 8 Assigning issues to yourself
  * 9 Setting a Milestone
  * 10 GTK 1 issues
  * 11 Carbon issues
  * 12 See also



## Referring questions to the mailing list and forums

If an issue doesn't describe a bug, but is only question (or the reporter doesn't know how to use certain feature) you can refer him to the mailing list and/or the forums to ask his question. Then you close the isse and optionally add the label "Resolution: not a bug" to the report. If you wish, you can provide a short answer to his question, but the bug tracker is for entering bugs and feature requests, not for providing support. 

## Removing duplicate entries

When looking at a 'new' issue, you may need to remove duplicates that have arisen because people submitted the same issue more than once. Such 'issues' should be closed and a relationship "Duplicate" should be added, optionally a label "Resolution: Duplicate" could be added. 

## Adding relations to other issues

The bug tracker supports setting relations between issues. 

The weakest relation is **Related**. This merely links the two issues. This can be useful since if you fixed one issue, it may fix the related issue(s) too. You can use [QuickACtions](<https://docs.gitlab.com/ee/user/project/quick_actions.html>) to set a relationship. 

If two reports decribe the same issue, then you can set **Duplicate of** to the newest report. You can use [QuickACtions](<https://docs.gitlab.com/ee/user/project/quick_actions.html>) to set this relationship. 

## Moving an issue to the fpc bug tracker

If a bug is not in the IDE or the LCL, but in the RTL, FCL or the compiler, you can move the issue to the FPC project in the bug tracker.  
Currently only devlopers who also have developer access to the [fpc bugtracker](<https://gitlab.com/freepascal.org/lazarus-sandbox/lazarus_testconversion/-/issues>) can do this. 

## Moving an issue to the Lazarus-CCR bug tracker

Some projects or components hosted on Lazarus-CCR use the Lazarus bug tracker for tracking issues. Those issues should not reside in the Lazarus project: they should be moved to the [Lazarus CCR bug tracker](<https://gitlab.com/freepascal.org/lazarus/ccr/-/issues>). 

## Confirming issues

Most of the time, the first step in fixing an issue is creating a small example program that demonstrates the issue. Reporters vary in the effort they make when submitting their initial report. Some people add test programs to their report (very nice), some people only add a code snippet (better than nothing) and some people add nothing at all. Not all reports provide adequate steps for others to reproduce the supposed issue. Going to the length of adding a small, compilable example program speeds identification and resolution of the issue you have identified. 

If you manage to reproduce the issue, then you can add the label "Status: Confirmed" (you can use [QuickACtions](<https://docs.gitlab.com/ee/user/project/quick_actions.html>) for that), or simply just say so in a comment. 

## Asking for feedback

If it is unclear how to reproduce an issue (e.g. no code example, bug may rely on compiler options etc.) then you should ask the reporter for feedback.  
You can use [QuickACtions](<https://docs.gitlab.com/ee/user/project/quick_actions.html>) to add the label "Status: Feedback" and it is best to also "hail" the reporting using @reporters_username in the message. 

## Assigning issues to yourself

If you think you can fix a reported issue, you can assign the issue to yourself.  
Other users, including the original reporter will then know the issue is under someones attention.  
If it turns out that you are unable to fix the issue you have assigned to yourself, simply remove yourself as assignee. 

## Setting a Milestone

When resolving (fixing) an issue (or when assigning), a developer may set a milstone.  
This, however is not a guarantee that a fix will make it into the chosen version. 

## GTK 1 issues

The gtk (i.e. the gtk version 1) widget set is not actively supported anymore by the Lazarus team, see [announcement](<http://lists.lazarus.freepascal.org/pipermail/lazarus/2009-November/047069.html>). If there is no problem when the gtk2 widget set is used instead, then the issue can be resolved with state suspended. 

## Carbon issues

The carbon widget set is not actively supported anymore by the Lazarus team, see [announcement](<https://forum.lazarus.freepascal.org/index.php/topic,60917.0.html>). If there is no problem when the cocoa widget set is used instead, then the issue can be resolved with state suspended. 

## See also

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") general information on submitting bugs, what should be covered in a bug report, and using the bug tracker.

---

_Source: [https://wiki.freepascal.org/Moderating_the_bug_tracker](https://web.archive.org/web/20250123172926/https://wiki.freepascal.org/Moderating_the_bug_tracker)_
