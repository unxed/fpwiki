# How To Help Developing Lazarus

│ **[Deutsch (de)](</How_To_Help_Developing_Lazarus/de> "How To Help Developing Lazarus/de")** │  **English (en)** │  **[español (es)](</How_To_Help_Developing_Lazarus/es> "How To Help Developing Lazarus/es")** │  **[Bahasa Indonesia (id)](</How_To_Help_Developing_Lazarus/id> "How To Help Developing Lazarus/id")** │  **[日本語 (ja)](</How_To_Help_Developing_Lazarus/ja> "How To Help Developing Lazarus/ja")** │  **[português (pt)](</How_To_Help_Developing_Lazarus/pt> "How To Help Developing Lazarus/pt")** │  **[русский (ru)](<../ru/How_To_Help_Developing_Lazarus.md> "How To Help Developing Lazarus/ru")** │    
****

## Contents

  * 1 Prerequisites to developing Lazarus
  * 2 Development areas
    * 2.1 Known bugs
    * 2.2 Documentation
    * 2.3 IDE
    * 2.4 Widgetsets ("interfaces")
  * 3 How to submit your changes?
  * 4 Dealing with regressions
    * 4.1 Automate searching for regression errors
      * 4.1.1 SubVersion
      * 4.1.2 Git mirrors
  * 5 Need more help?
  * 6 See also



## Prerequisites to developing Lazarus

There are two things to note: 

  * You should have the latest release of the Free Pascal compiler (FPC) **or** a more recent GIT version of it (ie development version). For obtaining FPC, see [Free Pascal download](<http://www.freepascal.org/download.var>) for the latest release version **or** [Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler") for building a development version from source.


  * You **must** have the very latest Lazarus development version from the GIT repository. For obtaining the latest version, see [Getting Lazarus from the GitLab server](<Getting_Lazarus.md> "Getting Lazarus").



## Development areas

So now you have the latest version of Lazarus and wish to start improving it, but are asking yourself "where do I begin?" Well, that depends. 

### Known bugs

If you don't have any particular woes about Lazarus but just want to help, then I would recommend looking at the bug list [Bug Tracker](<http://bugs.freepascal.org/set_project.php?project_id=1>) find a bug that you think you can fix, and start hacking. Additional information on submitting bug reports and using the bug tracker can be found at [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report"). 

### Documentation

Lazarus needs more documentation! If you don't want to fix a bug you can help by writing documentation. Even this page is a work in progress. If you have useful information to add, or if you see mistakes, please feel free to improve the contents of this page. 

Look at [Lazarus Documentation Editor](<Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor") and [LCL Documentation Roadmap](<LCL_Documentation_Roadmap.md> "LCL Documentation Roadmap") for some help on how to and a list of units to be documented. 

The on-line IDE help is gradually being created as a part of the wiki. Recently a lot of stub pages of the [Lazarus IDE](<Lazarus_IDE.md> "Lazarus IDE") documentation about the IDE windows have been added. When working in IDE, if you need help, press F1. You will be presented a help wiki page (possibly empty or incomplete). Improve it, if you have the relevant knowledge. 

### IDE

See these link: [Extending the IDE](<Extending_the_IDE.md> "Extending the IDE"). 

### Widgetsets ("interfaces")

A [widgetset](<Widgetset.md> "Widgetset") (WS) is the "glue code" between the LCL code parts that are independent of the target operating system and the target operating system itself. For each supported target OS, the corresponding WS units are to be found in one of the subdirectories under the C:\Lazarus\lcl\interfaces\\. 

Here is an outline of the steps one should follow, in order to improve a widgetset (description provided by Mattias Gärtner on the Lazarus mailing list on 2006-07-15). When making changes to a WS, it is not necessary to rebuild everything (Lazarus including the IDE), in order to test the efects of the changes. Proceed as follows:  

    
    
    * Create your testbed project (a small program
      which should contain the testing code for your WS changes);
    * Setup the keyboard shortcuts for 'Build Lazarus' and 'Configure Build Lazarus' 
      (in IDE, go to Editor Options/Keymapping);
    _**repeat**_
     * build your application (this will re-build the WS you altered, and will re-build the LCL);
     _**repeat**_
      * Make your improvement in the WS code;
      * Build Lazarus (in IDE, go to Tools/Build Lazarus 
        - this only rebuilds the LCL, which rebuilds also the selected WS);
      * Now compile your testbed project;
      * Run and debug your program;
     _**until**_ no errors are found in your change;
     * Reconfigure "Build Lazarus" to build all 
       (in IDE, go again to Tools/Configure "Build Lazarus");
     * Build Lazarus and test the IDE;
    _**until**_ no errors/degradation due to your changes are found in IDE;
    * Create a patch and send it (see further below for details).
    

## How to submit your changes?

You will need to make a "patch" (see [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch")). The preferred way of submitting the patch to the Lazarus developers is to create an issue in the bug tracker and attach the patch to it. 

## Dealing with regressions

From time to time changes in the Lazarus source code might cause features which worked before to stop working. In a case where there is no clue as to what caused the break it may be useful to do an iteration method to determine exactly which revision caused the problem. 

This process is simple, although somewhat time consuming: 

Suppose it works with rev 1000 and not with 5000. Then test with 3000. Testing requires updating the svn code, rebuilding the LCL for the desired widgetset, rebuilding a test application which uses the feature and testing this application. If it works, repeat with 3000 and 5000 as extremes. If not, use 1000 and 3000 as extremes. 

After some time you should be able to isolate which revision broke it. This information makes fixing the problem much easier, so we encourage people helping to develop Lazarus to try this process and post this information in bug reports in case there are any regressions with no clear clue of what went wrong. 

To check to which data each revision corresponds, one can use the [Lazarus svn browser (ViewVC)](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/?root=lazarus>). After the interval of revisions has been reduced to a relatively small number, like 25 or so, it may be quicker to check the revisions with ViewVC and check which are possible candidates for the break, to speed up the final part. 

One can obtain a particular revision using the command: 
    
    
    svn update -r <revision number>
    

**A word of caution.** If you have a slow and unreliable internet connection (like modem dial-up or 3G) and limited bandwidth, then checking out various revisions from SubVersion is a slow and costly process. Every revision you checkout has to be downloaded from the internet. 

This issue is completely eliminated if you use Git, because the whole repository history is local on your computer. So you can checkout any older revisions without needing an internet connection and checkouts are instant. 

### Automate searching for regression errors

#### SubVersion

Florian wrote a small Unix script which can help automate the process of finding a regression error. The script is called [searchrev](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/scripts/florian/unix/searchrev?view=markup&root=fpcbuild>). 

There are command-line tools inspired by "git-bisect". A Unix shell script can be found in Debian-based Linux distributions as part of _subversion-tools_ package. A Perl-based version can be found in some other Linux distributions (eg. SuSE, Fedora, CentOS) as _svn-bisect_ package or [here](<https://github.com/Infinoid/svn-bisect>). 

#### Git mirrors

Git includes a command called _bisect_ which helps with finding regression bugs. It also has built-in support for automated testing. You need to create a small testcase and let Git use that testcase to determine if a revision is buggy or not. So with the automated bisecting, regression bugs can be found in seconds. For more information on the _git bisect_ command, see the [Git User Manual](<http://www.kernel.org/pub/software/scm/git/docs/user-manual.html#using-bisect>). 

## Need more help?

If you have any question you can ask them on one of the following places: 

  * The Free Pascal mailing lists: <http://www.freepascal.org/maillist.var>
  * The Lazarus Mailing lists <http://lists.lazarus.freepascal.org/mailman/listinfo>
  * The [Lazarus Forum](<http://forum.lazarus.freepascal.org>)
  * The [Lazarus Developer's blog](<http://lazarus-dev.blogspot.de>)



## See also

  * [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch") If you have modified the source code to implement a solution, this article helps you to add it to your bug report in the most efficient way, so that developers can add it to the main code as fast as possible
  * [Database bug reporting](<Database_bug_reporting.md> "Database bug reporting") Specific info and sample programs for database bugs
  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") general information on submitting bugs, what should be covered in a bug report, and using the bug tracker.
  * [Moderating the bug tracker](<Moderating_the_bug_tracker.md> "Moderating the bug tracker")
  * [Lazarus Development Process](<Lazarus_Development_Process.md> "Lazarus Development Process")

---

_Source: [https://wiki.freepascal.org/How_To_Help_Developing_Lazarus](https://web.archive.org/web/20240910144242/https://wiki.freepascal.org/How_To_Help_Developing_Lazarus)_
