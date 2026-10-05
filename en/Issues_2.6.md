# Issues with 2.6.0 release candidates

This page is supposed to contain all issues discovered while testing release candidates and their status. Make sure to include information about the platform where the problem appears (if applicable). Once the issue gets resolved, you should strike it over and add number of SVN revision fixing it. If the revision number refers to a SVN repository different from "fpc" (i.e. "fpcbuild" or "fpcdocs"), include information about the SVN repository too. 

Example: 

  1. ~~cpu-os: IE209101221 while compiling XYZ.pas~~ \- r123456
  2. docs: missing description of XYZ
  3. ~~readme.txt: wrong version number~~ \- r654321 (fpcbuild)



  


## Merge requests for post RC1 fixes so far

strikken means merged, bold means decisions/review/actions needed. 

  1. ~~r19515 already merged (typo in winunits-base shlobj.pp)~~
  2. ~~r19380 bug in exesearch~~
  3. ~~r19485 fpunits.conf- >.cfg~~
  4. ~~r19486 safecall on win64 flag in i_win.pas~~
  5. ~~r19525 Fix problem with QueryHook leading to crashes~~ Merged after checking that the new externals were general GDB funcs, and not just on Windows.
  6. ~~r19526,r19531 haiku (confirmed by Olivier)~~
  7. ~~r19342 nativent system unit (nativent dir only)~~
  8. ~~Jonas and Florian would discuss what to do with**r18961/r19312/r19313** ? -> still ongoing~~ Florian committed a fix to softfpu that resovled broken Sparc target
  9. ~~**r19497** check environment variable LOCALAPPDATA on windows as a possible cache location for instantfpc~~
  10. ~~**r19339** (Francesco) Updated libogcfpc to ver.1.8.8~~
  11. ~~**r19493** deprecation msgs of tdbf. Can starting compiler handle this? Does that matter at all for non- compiler/RTL? -> 2.4.4 can ~~
  12. ~~**r19328** byte typecast in set declaration gives syntax error (Jonas remarked it has a mantis item target 2.6.0 Mantis [mantis 20380](<http://bugs.freepascal.org/view.php?id=19328>) by Paul) **conflict on merge due to "not typeonly" introduced by r19148**~~ Merge not needed according to Paul. Related to cpnewstr
  13. ~~**r19343** mantis 19975 (classhelper, no commit msg) -> looks complex, so initially I'd say deny; Comments? (mostly under ifdef extended recorddef)~~
     1. ~~19342:[http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19292](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19292>) )~~
     2. ~~19343:[http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19281](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19281>)~~
     3. ~~19343:[http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19282](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19282>)~~
     4. Class helpers are the new feature added by Sven. In my opinion it is bad to make a release with the new feature and leave the bugs unfixed if they already fixed somewhere else--[Paul Ishenin](</User:Paul_Ishenin> "User:Paul Ishenin") 12:23, 30 November 2011 (CET)
     5. Merging is always a tradeoff of risks vs benefits.
  14. ~~OS/2 merging, many revs, see[[1]](<http://www.stack.nl/~marcov/mergelogs26/postrc1.txt>) but hinges on **r18138** causes many conflicts. (also r19551,r19552)~~
     1. ~~18138 partially merged, but not marked as merged. Merging 18136 was also needed.~~
     2. ~~19551-19552 merged~~
  15. ~~Paul: Fixed today a serious bug which may cause compiler crashes:**r19563** Without the change any record member symbol was treated as a tfieldvarsym and tfieldvarsym members where accessed without a check. ~~
  16. ~~fpcbuild r918,926,928~~
  17. **open tickets** with 2.6.0 as target 
     1. [mantis 18702](<http://bugs.freepascal.org/view.php?id=18702>) (error in rewritten win32 cdecl handling, Joost)
     2. ~~[mantis 20034](<http://bugs.freepascal.org/view.php?id=20034>) (improved versioning of CHM's, Marco -- can probably easily be delayed)~~
     3. [mantis 20482](<http://bugs.freepascal.org/view.php?id=20482>) (fpmake problem, Joost)
  18. ~~19684: fixes internal error in handling of dynamic arrays~~
  19. ~~19694: fixes compiler crash due to accessing potentially unassigned field~~
  20. ~~19605 ([issue 0020638](<http://bugs.freepascal.org/view.php?id=0020638>)),~~
  21. ~~19641 ([issue 0020690](<http://bugs.freepascal.org/view.php?id=0020690>), [issue 0020038](<http://bugs.freepascal.org/view.php?id=0020038>)), ~~
  22. ~~19681 (issue[issue 0020721](<http://bugs.freepascal.org/view.php?id=0020721>)): issues with nested types in classes. Looks more complicated, so I committed it separately( different day ->testsuite)~~



## Issues with 2.6.0-rc1

  1. (potential) the usage of the heapmanager in a thread before it is initialized (Sergei, fpc-devel, nov 10th "Threads crash on freebsd")

---

_Source: [https://wiki.freepascal.org/Issues_2.6.0](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Issues_2.6.0)_
