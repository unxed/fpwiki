# Issues 2.0.4

## Issues with 2.0.4-rc1

  1. ~~powerpc-linux: "./install.sh: line 266: /home/fpc/fpc-2.0.4/lib/fpc/2.0.4/samplecfg: No such file or directory"~~ (not happening any more)
  2. ~~install.dat:~~ \- r4117 
     1. ~~unit graph missing (standalone package now)~~
     2. ~~package hash missing (renamed from MD5)~~
     3. ~~unit fftw missing (newly added - Win32+source)~~
     4. ~~Oracle and PostGreSQL units missing (Win32+source)~~
     5. ~~winunits missing (added and merged with winver and mmsystem)~~
     6. ~~wrong filename for PDF docs~~ \- r4129
  3. OS/2: 
     1. IDE (and other FV apps) crashing on startup
     2. Old installer version had to be used due to previous point, but that one has checks for bfd2e.dll, libc05.dll and iintl6i.dll (used by as.exe) being accessible from LIBPATH missing
  4. ~~powerpc-morphos: OpenGL support broken~~ \- r4089
  5. docs 
     1. (not critical) fpdoc in fpdocs/makefile.fpc doesn't seem to add rtl/bsd to the includes.
     2. ~~systemh.inc(661,69): Syntax error at token "Indentifier public" when I fix point 1.~~
     3. ~~outdated version number: doc-html.tar.gz expands to share/doc/fpdocs-2.0.2/...~~
        * this should be hopefully fixed by using release branch instead of trunk (or with fpcdocs r158 in trunk) (Tomas)
     4. ~~same for doc-pdf.tar.gz and doc-ps.tar.gz.~~
     5. ~~in general the version number 2.0.2 still shows up often in doc files.~~
     6. ~~issues with fpdoc parsing of source files~~ \- r4134
  6. ~~general: wrong package version for graph (2.1)~~ \- r4094
  7. docs: some compiler messages (in errore.msg) missing description (at least parser_e_dispid_must_be_ord_const) 
     * hm, are you sure. As much as I can see, "dispid" has never been merged from 2.1.1 to 2.0.x (Mischi)
     * Yes, I am (Tomas) - docs have been compiled from trunk - search for "Dispatch ID" in user.txt/user.pdf. Furthermore, there are other such message, even in fixes; just looking at the file with my (tired) eyes (automated check would probably find quite a few other): 
       * ~~parser_e_improper_guid_syntax~~
       * ~~parser_w_interface_mapping_not_found~~
       * ~~parser_e_default_value_only_one_para~~
       * ~~parser_e_default_value_expected_for_para~~
       * ~~type_e_interface_type_expected~~
       * ~~type_e_class_or_interface_type_expected~~
       * ~~type_e_type_is_not_completly_defined~~
       * ~~sym_n_private_identifier_not_used~~
       * ~~sym_n_private_identifier_only_set~~
       * ~~sym_n_private_method_not_used~~
       * **many** in asmr, asmw, ~~exec and execinfo sections~~
       * ~~unit_w_unit_name_error~~
       * ~~unit_u_recompile_crc_change~~
     * All messages now documented, except assembler messages.(Michael) - r4217
     * ...and the originally mentioned parser_e_dispid_must_be_ord_const ;-) (only existing in trunk - just for completeness sake)
     * Got it. Was confused by the fact, that the docs have been compiled from trunk, which I think is an additional issue. 2.1.1 only error messages should not be included in 2.0.x
  8. FreeBSD problems 
     1. The above document path problems were fixed (see also rc2)
     2. ~~Had to add OPT="-Fl/usr/local/lib -dFreeBSD5" to the main make line in makepack to get FreeBSD compiling (Marco). Will see if I can somehow massage this in.~~
     3. ~~install.sh (Marco)~~ \- r107 (fpcbuild) 
        1. ~~the == in the install.sh script makes it BASH dependant.~~
        2. ~~Moreover the same line refers to linux framebuffer devices~~
     4. ~~samplecfg always searches for libgcc.a Linux specific~~ \- r4116
     5. (freebsd building) somehow an empty 2.0.3 (the currently installed version) directory is made, but after install there are no files in them. (Marco). See also 2.0.3
  9. Debian package issues 
     1. ~~changelog updates needed~~ \- r110 (fpcbuild)
     2. ~~packaging rules updates needed - renamed packages, etc.~~ \- r109 (fpcbuild)
  10. ~~LinuxPPC aborts while compiling IDE~~ (not happening on another machine?) 
     1. ~~/usr/bin/ar: creating units/powerpc-linux/libpwos2help.a~~
     2. ~~/usr/bin/ar: creating units/powerpc-linux/libpwvphelp.a~~
     3. ~~units/powerpc-linux/wwinhelp.s: Assembler messages:~~
     4. ~~units/powerpc-linux/wwinhelp.s:2357: Error: syntax error; found `T' but expected `,'~~
     5. ~~units/powerpc-linux/wwinhelp.s:2357: Error: junk at end of line:`TW_TWINHELPFILE_$_READPHRINDEXFILE$PINTCOLLECTION$TWINHELPPHR'~~
     6. ~~units/powerpc-linux/wwinhelp.s:2373: Error: syntax error; found `T' but expected `,'~~
  11. ~~GO32v2 - old version of CWSDPMI.EXE included~~ \- r115,116 (fpcbuild)
  12. ~~linux: lNet not working correctly~~ \- r4092
  13. ~~docs: the pdf versions contain things like "Programmers’ manual for Free Pascal, version 2.0.2~~
     * This shouldn't happen if using the proper version, of course. All of trunk, fixes and release branches contain updated fpc.sty referring to version 2.0.4. The docs must have been built with an outdated version... (Tomas)
  14. _install/doc/readme.txt: already contains 2.0.4 instead of 2.0.4-rc1 (manually changed in installer package for Mac OS X)_ \- version number in readme.txt isn't supposed to be different from release
  15. ~~install/doc/whatsnew.txt: unfinished last bullet in list of compiler changes~~ \- r122 (fpcbuild)
  16. ~~errorda.msg to be added~~ \- r4135,4136
  17. errorf.msg to be updated (from ACS)
  18. ~~TryStrQWord to be removed~~ \- r4144
  19. _GO32v2 - IDE: debugging not working_ \- nobody to fix it for 2.0.4 at least :-(
  20. IDE: HTML help displaying not working properly
  21. packaging: docs sources not included in GO32v2 and OS/2 packages, nor available on FTP (make docsrc)
  22. _ANSI version of screenshots and diagrams not available in HTML docs any more_ \- not to be solved in 2.0.4
  23. ~~powerpc-linux: RTL failing on G5 machines (dcbz)~~ \- r4161
  24. ~~GO32V2 - instalation: package**DOS204rc1.zip** installed without problems but **DOS204rc1-full.zip** can't be instaled into another directory because there seem to be some keyboard handling errors (bad CTRL key recognition?).~~ \- r4175
  25. _However both packages have hidden mouse._ \- known issue, fix is not easy without breaking other platforms, not to be addressed in 2.0.4
  26. ~~GO32V2 - both packages have a error in the FP.CFG configuration. At default there are lines "-Cs" (for all compilation modes) and IDE complains about them and I had to remove it manualy.~~ \- r4198,4205
  27. _GO32V2 - problems with mouse. IDE most probably doesn't hide the mouse while changing screen so it does ugly artefacts(best seen when scrolling).The FreeVision example has another problem with mouse. When the time in the upper corner changes, the mouse hides and never shows again. IDE itself doesn't do it._ \- see above for mouse cursor problems
  28. _GO32V2 (and maybe others) - why I can't configure colors?_ \- no non-copyrighted sources available for that - dialogues need to be implemented first (not to be fixed for 2.0.4)
  29. Demos 
     1. ~~../demo/win32/menu.pp is missing a "{" in the first line to correctly comment the Copyright~~ \- r125 (fpcbuild)
     2. ~~../examples/fcl/dbugsrv.pp is missing the {Mode ObjFpc} and {H+} directives to become compilable~~ \- r4207
     3. quite a few more described on <http://lists.freepascal.org/lists/fpc-devel/2006-July/008236.html>
  30. ~~Examples - issues described on<http://lists.freepascal.org/lists/fpc-devel/2006-July/008237.html>~~
     1. ~~FCL~~
     2. ~~CDROM~~
     3. ~~OpenGL~~
     4. ~~Oracle~~
     * ~~Most are now fixed~~ \- r4314
  31. Install texts issues described on <http://lists.freepascal.org/lists/fpc-devel/2006-July/008236.html>
     1. readme.ide
     2. ~~readme.txt~~ \- r166 (fpcbuild)
  32. IDE (Win32): 
     1. ~~When the Help >Contents is called for the first time appears an explanation how to setup the docs. It points to a fpcdoc.htm when it should be fpctoc.htm~~ \- r4209
     2. ~~The buttons Yes and No of the MessageDialog that appears when adding a help file don't have the label as seen in "www.geocities.com/camara_luiz/fp_dialogbug.png"~~ Unable to reproduce. A possible reason might be config/desktop file incompatibilities which might be fixed after they are rewritten by the new IDE. --[FPK](</User:FPK> "User:FPK") 22:19, 26 July 2006 (CEST) 
        * The OK button of the Open File Dialog has similar problem as the above as well other MessageDialogs with Yes,No, Cancel buttons.
        * Strange: after running the IDE today the above problems does not occurs anymore. I did not changed any configuration in my computer. Also could not reproduce in rc2.
  33. Win32 IDE doesn't start when started from start menu on one machine immediately after installation (but starting from desktop worked). Reboot solved the problem. 
     * Old cygwin1.dll on path discussed as one possible reason for that - maybe change the installer to suggest reboot before starting FPC (if older cygwin1.dll version might have been installed previously)?
  34. Docs 
     1. ~~(Using doc-html.zip from freepascal.org - 14/07/06) In file /user/userse17.html there are malformed words including the title (Using the con guration le). When i look at the source the closing tags are malformed in several places (The " <" is "eatten"). PS: These problems are not specific for the IDE since is also seen in a HTML browser (Firefox)~~ I was using the release doc. The updated docs are Ok. (Luiz)
     2. Using the html docs from 204rc1 inside the IDE help browser i get the following behavior: 
        * Open the Contents
        * Go to Programmers Guide
        * Go to Local Directives: try to click in the "up" link -> It will not work



## Issues with 2.0.4-rc2

  1. if starting with 2.0.3 
     1. there was an empty $prefix/lib/2.0.3 is created
     2. the licensing docs also got fpcdoc-2.0.3 as version nr or so.
  2. the docs from the binary package that michael provided had paths in them, causing the docs to be installed in: $prefix/share/doc/fpc-2.0.4/share/doc/fpc-2.0.4 or so (Michael says this needs a fpcmake fix (?))
  3. ~~Hints re the PATH should be added in the Mac OS X Quickstart section of the README file~~ \- r146 (fpcbuild)
  4. ~~LinuxPPC aborts while compiling IDE if smartlinking~~
     * ~~/usr/bin/ar: creating units/powerpc-linux/libpwos2help.a~~
~~
     * /usr/bin/ar: creating units/powerpc-linux/libpwvphelp.a
     * units/powerpc-linux/wwinhelp.s: Assembler messages:
     * units/powerpc-linux/wwinhelp.s:2357: Error: syntax error; found `T' but expected `,'
     * units/powerpc-linux/wwinhelp.s:2357: Error: junk at end of line:`TW_TWINHELPFILE_$_READPHRINDEXFILE$PINTCOLLECTION$TWINHELPPHR'
~~
     * ~~units/powerpc-linux/wwinhelp.s:2373: Error: syntax error; found `T' but expected `,'~~ Not reproducable
  5. LinuxPPC IDE: 
     1. ~~When debugging hello world, finishing the debug, and quiting, the IDE aborts with a segfault.~~ Not reproducable anymore, either gdb library problem and/or bug when creating ppus in smartlinking mode (fixed in r4276)
     2. ~~When compiling with standard settings (smartlinking, gdb), the bug described in bug report 0005965 can be reproduced~~ see above
  6. ~~The install script does not like the ~ (tilde) symbol at the front of the path. E.g. an input of "~/x" gives a few of "tar" error messages. Maybe not an issue, but usually you can use the tilde in paths on *nix.~~ \- r161 (fpcbuild - trunk)
  7. ~~ARM-Linux: Uncompilable assembler code in ossysc.inc~~
  8. ~~ARM-Linux: When fixing uncompilable assembler code, cross compiler has internal error while compiling RTL.~~
  9. Dos/go32v2 is not mentioned as a supported platform in install/doc/readme.txt 
     * It isn't supported, just available... ;-) It's mentioned explicitely few lines below, so we might want to leave it as it is (although not 100% correct indeed). I'm fully open to other opinions though.
  10. Win32 
     1. If i already have a fpc version installed and choose to install the new in another directory, the old fpc path is kept in the PATH variable making the old fpc to be called. For example: i installed rc1 in (..)\fpc204 and then rc2 in (..)\fpc204rc2, here is how the PATH variable is now: 
        * _This can't be solved by inno setup. It is also a feature that you can install multiple versions of fpc. It is up to the user to update the PATH setting according to his preference._
          * OK. But in this case better would be to put the new path in front of the old because currently executing 'fpc' in the console will call the old fpc version instead of the newly installed. Also the user should be warned to change the PATH setting
        * ;C:\bin\code\fpc204\bin\i386-Win32;C:\bin\code\fpc204rc2\bin\i386-Win32
     2. [Docs] The problem reported in item 34.2 of rc1 is still present when the rc2-ide is used together with rc2-docs
     3. ~~The readme.txt points to the demo directory to compile the hello program when it should point to demo\texts directory where hello.pp resides~~ \- r166 (fpcbuild)
     4. TestSuite errors(faillist): 
        * test/tasout
        * Note: This test requires an installed nasm assembler
        * test/tfpu3
        * Note: this test requires that nasm assembler is installed
        * test/tmaclocalprocparam
        * test/tmacnonlocalexit
        * test/tmacnonlocalgoto
        * test/trangeob
        * test/tstrreal3
        * test/cg/tprintf
        * tbs/tb0038
        * Note: This test requires an installed Nasm
        * tbs/tb0039
        * tbs/tb0497c
        * webtbs/tw0803
        * webtbs/tw4056
        * webtbs/tw4229
        * webtbs/tw4390
        * webtbs/tw4541
        * webtbs/tw5086
        * webtbf/tw3183
        * webtbf/tw6686
  11. ~~Memory leak with AS operator for interfaces (regression from 2.0.2) - assigned to Florian~~ \- r4356 
     * ~~Documented on<http://www.freepascal.org/mantis/bug_view_advanced_page.php?bug_id=7174>~~
  12. ~~Installer: ufvsrc.zip (<http://www.freepascal.org/mantis/view.php?id=7194>)~~ \- r4274
  13. ~~Linux x86_64~~
     1. ~~epoll_event record uses a special packing on 64 bit linux. It needs the "packed" directive.~~ \- r4295
  14. ~~fpc.spec issues (.rpm) mentioned by Joost~~ \- r163 (fpcbuild)
  15. ~~gtk2 applications cannot be linked on linux computers with gtk 2.8. There is a patch in trunk: r4291~~ \- r4376



## Issues with 2.0.4-rc3

  1. IDE has broken mouse support when launched inside xterm (debian) : only first click on menu works, then no more mouse event seem to be processed (mazen). (see <http://www.freepascal.org/mantis/view.php?id=7233> too)
  2. Burkhart Carstens asks for r4090 and r4382 being merged. It fixes more than it could break. --[FPK](</User:FPK> "User:FPK") 21:24, 7 August 2006 (CEST) 
     * I cannot asses how dangerous this particular patch is. However, Peter implemented this patch in trunk shortly after RC1 testing started and he didn't suggest that this patch might be merged (although it was obvious at that time that at least one more RC would be needed). Now we get a proposal for merging after having created branch for the (hopefully) final RC. My recommendation would be not to merge them unless we want one more RC round (which I'd personally prefer to avoid if possible). (Tomas)
  3. please merge r4327 been tested in 2.0.3 since it was fixed around rc2, see issue <http://www.freepascal.org/mantis/view.php?id=7211>, r4303 makes a test on bufdataset.locate fuction and shouldn't break anything (jesusrmx: I can't imagine that it breaks something, and it fix a bug. Loesje: Me neither) 
     * I asked Joost about a need to merge this one before RC3 branch creation, but didn't receive any response. I recommend not to merge it unless he explicitely asks to do it and confirms that _he_ considers it absolutely safe for 2.0.4.
  4. Win32 
     1. The open issues of rc1 and rc2 are still present (See above). No regression found.
     2. The ide help viewer has the same glitches as GO32v2 (See below)
  5. GO32v2: RTE 216 during unit Graph initialization in some demoes (e.g. mandel.pp).
  6. GO32v2/IDE: HTML viewer renders non-breaking space as '&#00a0;' (see <http://www.laaca.borec.cz/fpc_help.png>)
  7. All: The following simple program is rejected by compiler with 'Error: Operation "+" not supported for types "^Byte" and "LongInt"' _if_ compiled with -Co (overflow checking on), although it works correctly in other modes and when compiled without -Co, and TP/BP and older FPC versions accept it without problems: 
     * {$MODE TP}
     * var
     * B: ^byte;
     * begin
     * Inc (B);
     * end.
  8. ~~whatsnew.txt files states that sqlitedataset is obsolete and was replaced by sqldb. There's no sqldb connection component for sqlite. sqlitedataset was replaced by sqliteds and sqlite3ds units.~~ -r179 (fpcbuild)
  9. The xmlread unit contains a bug, that is doesn't handle text nodes after an element node correctly. This causes the fpdoc of 2.0.4 to create docs, where after a link the whitespace before the text is removed. The tar.gz docs in the release don't suffer from this issue because they were generated by fpdoc from trunk (fpc 2.1.1). This issue affects packages that create their docs from source, e.g. fpc-doc rpm or fp-doc deb package. For a discussion of this issue see [fpc-devel mailing list](<http://lists.freepascal.org/lists/fpc-devel/2006-August/008632.html>).

---

_Source: [https://wiki.freepascal.org/Issues_2.0.4](https://web.archive.org/web/20200814025301/https://wiki.freepascal.org/Issues_2.0.4)_
