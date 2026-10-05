# Issues 2.1.4

## Contents

  * 1 Issues with 2.1.4 aka 2.2.0 beta
  * 2 The debug problem for x86_64.
  * 3 The "arch" issue
  * 4 Linux using FPC_USE_LIBC
  * 5 Bug 9141
  * 6 IDE Compiler options
  * 7 Revisions



## Issues with 2.1.4 aka 2.2.0 beta

# The debug problem for x86_64.

Florian is working on this, and asked Marc for help(?) What is the status? Do you/we think it's fixable before 2.2.0? 

    First let's identify the problem. What are is the x86_64 debug problem ? are there anu mantis entries, descriptions, anything. Florian gave me an example and I couldn't see what was wrong with it. Debugging worked. Se telling me that there is a debugging problem doesn't help in solving it. --[Marc](</User:Marc> "User:Marc") 02:10, 30 July 2007 (CEST)

# The "arch" issue

Is this the "arch"/"uname"/"-iSP" issue? Then the "arch" command returns the architecture of the running system. Doesn't 'fpc -PP' the same in all circumstances?!? 

    No. Arch returns the primary architecture of a linux installation as far as I know. fpc -PP only echoes what was compiled in at linking. Mostly a difference for biarch and systems with binary architectures. However the current use is limited, it is currently only used to decide if external binutils are needed (and then the prefix is set) or internal ones. This is hacked into the scripting code after, but afaik doesn't work for binary emulation systems. (e.g. linux compiler on FreeBSD will see "arch/uname"=FreeBSD, while there are linux binutils named "as" and "ld" if proper dependancies for the port are set)

Ok, it's committed and works. Done. 

# Linux using FPC_USE_LIBC

Everything is merged. Still have to test. x86_64 has other issues with FPC_USE_LIBC which won't make it into 2.2 

# Bug 9141

Fixed in revisions 7979 and 7982. But there's some disussion left. There are rumours that Daniel fixed it. Can we merge it? 

    Drawback of 7979 is the change of const parameter handling. Alternatively this patch could be used: <http://www.hu.freepascal.org/fpcircbot/cgipastebin?msgid=1153>

Without the fix for [bug 9261](<http://www.freepascal.org/mantis/view.php?id=9261>) Lazarus still cannot start x86_64-linux, so revision 8056 should be reviewed and merged too. 

# IDE Compiler options

Daniel is working on this one. 

  


# Revisions

These revisions should be checked, or the reason why they shoudn't be committed is given: 

Jonas: 

  * 7879, 7891, 7903 - prefer not to, seldom a problem, merge after 2.2.0
  * 7959 Unimportant, occurs seldom. real2str code, so hairy and could have unforeseen side effects.
  * 7944 Fixes bug which only appeared after range checking changes by Daniel, which have not been merged back afaik.
  * 7935 Support for bitpacked records in dwarf. Fairly invasive, and since it also changed the code which generates debug info for regular records, it could in theory break something there. Don't merge.
  * 7907 Only a change of a new warning introduced by Daniel, I don't think this warning has been merged.
  * 7363 ?



Daniel: 

  * 7252 - It's an irritating bug and Daniel is pretty confident about the patch. But not enough yet. It has a conflict, though. It needs 7183
  * 7979, 7982 - bug 9141, but Florian thinks it can have unforeseeable consequences
  * 7940 ?
  * 7937, 7938 ?
  * 7637 ?



Marco: 

  * 7627
  * 7985
  * 7896 - can somebody check this?



Michael: 

  * 7446 - 2.3.1 specific
  * 7301



Pierre: 

  * 7577 - Tomas thinks it has to be merged



Florian: 

  * 7976
  * 7925 ?
  * 7916 ?
  * 7905 ?
  * 7800 ?
  * 7794 ?



Yuri: 

  * 7965 - 2.3.1 specific



Joost: 

  * 7643 - variant/date troubles only manual merge possible - too difficult
  * 7955 - Dependent on TCustomSQLQuery, which isn't merged



fpcbuild: 

  * 341

---

_Source: [https://wiki.freepascal.org/Issues_2.1.4](https://web.archive.org/web/20210421151326/https://wiki.freepascal.org/Issues_2.1.4)_
