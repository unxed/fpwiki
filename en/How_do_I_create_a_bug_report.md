# How do I create a bug report

│ **English (en)** │  **[русский (ru)](<../ru/How_do_I_create_a_bug_report.md>)** │

This document contains some guidelines for using the FPC / Lazarus [bug tracker](<https://gitlab.com/freepascal.org/>) as a reporter. This document is written for FPC / Lazarus users who identify bugs, have recommendations, want to submit patches or find other issues and want to report these to the Lazarus development team. 

## Contents

  * 1 Code compilation errors
  * 2 Logging in / Creating new account
  * 3 Check if the bug is not already reported
  * 4 What should be submitted via the Bug Tracker?
    * 4.1 Bug
    * 4.2 Regression caused by a certain revision
    * 4.3 Suggestion
    * 4.4 Improvement
  * 5 Attachments
  * 6 Understanding the Report Status
  * 7 See also



## Code compilation errors

If you have errors when compiling code from latest git revision, please contact a proper [FPC mailing list](<http://freepascal.org/maillist.var>) or [Lazarus mailing list](<http://lists.lazarus-ide.org/listinfo>) or use some appropriate [Forum](<https://forum.lazarus.freepascal.org>). Then the problem should be solved more promptly. 

## Logging in / Creating new account

You need to be logged in to edit or submit bug reports. If you are logged in as guest, you need to log out first (Guests cannot make reports, only watch them). If you already have an account, go to the [login page](<https://gitlab.com/users/sign_in>), otherwise create a new account on the [sign up page](<https://gitlab.com/users/sign_up>). 

## Check if the bug is not already reported

Use the search field in [View Issues](<https://gitlab.com/groups/freepascal.org/-/issues>). Hint: The searching is not smart: e.g. if you have a problem using TEdit.SelStart, search for "SelStart". If the issue is already reported: 

  * reopen it, if the bug report has been resolved or closed - use Reopen Issue button
  * add a note, if you have reproduced this bug in a different situation than reported
  * you can set the system to monitor changes in this bug report - use Monitor Issue button



Note: you need to be logged in to perform these operations, see section #Logging in / Creating new account. 

## What should be submitted via the Bug Tracker?

  * Bugs: If you identify errors, glitches, or other faults in [FPC](<FPC.md> "FPC") or [Lazarus](<Lazarus.md> "Lazarus")
  * Suggestions: If you have identified a better way to do something
  * Improvements: If you can make something work better



Please note: The Bug Tracker is **not** designed to field questions. These should be directed toward the Forums [[1]](<http://forum.lazarus.freepascal.org/>). 

  * For reporting, go to [Lazarus bug tracker](<https://gitlab.com/groups/freepascal.org/lazarus/-/issues>). You must be logged in, see section #Logging in / Creating new account.
  * Go to the [Report Issue](<https://gitlab.com/groups/freepascal.org/-/issues/new>) page. Fill in as much as you can and know. The more specific, the better.



### Bug

  * Important fields are the OS and Product fields and the steps to reproduce this issue. If an issue cannot be reproduced by the developers, they cannot start to fix it! Do not forget to mention your specific architecture/configuration (32 or 64 bit, little or big endian if both are possible on your platform, version of your operating system).
  * If possible, please **upload a small test application that shows the bug**. This will likely speed up a fix.
  * If there is some graphical error, it is useful to upload a (partial) screenshot (in png or jpeg, not bmp format).
  * If it is a crash, try to create a backtrace. See [Creating a Backtrace with GDB](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB") for more info.
  * You can try to reproduce the bug on as many different platforms as you can - it helps to determine if it is widget specific issue.
  * If you have a possible solution, you can add a patch - see [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch"), which will speed up the process.
  * You can boost fixing the bug by submitting a bounty, see [Bounties](<Bounties.md> "Bounties").



### Regression caused by a certain revision

If you can find a revision in main that caused a bug, please include also its Git hash number. The report will usually be assigned to the author of that revision. You can find a quilty revision by a "bisect" process which is a binary search over the revisions. There are tools to help with that: 

  * A Git command [git bisect](<https://git-scm.com/docs/git-bisect>). Git is fast in this operation because all the revision history is local and nothing needs to fetched from a server.



### Suggestion

Explain your idea. A GUI mockup or an example of another tool using the feature can be helpful. 

### Improvement

  * If you have implemented a new feature in the source code or improved documentation in the XML files, create a patch - see [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch").
  * If you have improved translation in a language .po file, attach the whole .po file (not a diff).
  * If you have another resource file, for example an icon, attach it to the report.



## Attachments

If you add source code or project sample attachments for the bug report (**strongly recommended** , see [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports")), please compress them using preferably these formats: 

  * zip (.zip)
  * gzip (.gz)
  * tar.gzip (.tgz/.tar.gz)



Other formats like 7zip, Bzip and RAR are ok, too. Nowadays tools for them are easily available. 

## Understanding the Report Status

An issue can have the following states: 

  * Open, but no assignee(s).
  * Open and 1 or more assignees: the issue has been assigned to one or more Lazarus developers, who will try to fix/implement it.
  * The issue has a label "Status: Confirmed": a member of the Lazarus team has duplicated the bug or agrees that the feature should be implemented
  * The issue has a label "Status: Feedback": the reporter should provide feedback to answer any questions posed by the Lazarus team, or to confirm that the issue is fixed satisfactorily.
  * Closed: the assignee has closed (and presumably fixed or dismissed) the issue.



## See also

  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports")
  * [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch") If you have modified the source code to implement a solution, this article helps you to add it to your bug report in the most efficient way, so that developers can add it to the main code as fast as possible
  * [Database bug reporting](<Database_bug_reporting.md> "Database bug reporting") Specific info and sample programs for database bugs
  * [Moderating the bug tracker](<Moderating_the_bug_tracker.md> "Moderating the bug tracker")
  * The following page contains good tips about [How to Report Bugs Effectively](<http://www.chiark.greenend.org.uk/~sgtatham/bugs.html>).

---

_Source: [https://wiki.freepascal.org/How_do_I_create_a_bug_report](https://web.archive.org/web/20250501213845/https://wiki.freepascal.org/How_do_I_create_a_bug_report)_
