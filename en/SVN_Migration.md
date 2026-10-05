# SVN Migration

## Contents

  * 1 The Plan
  * 2 Usage
  * 3 Repository layout
  * 4 Migration of CVS data
  * 5 Checkout
  * 6 Updating and commiting
  * 7 Branching and tagging
  * 8 Merging
    * 8.1 Merging automatically created files
    * 8.2 Merge with plain svn
  * 9 Tools
  * 10 Committing over http



## The Plan

  * Repository reorganisation 
    * We won't use cvs2svn to migrate but simply import the whole source into svn to have a clean start.
    * Lazarus gets its own repository.
    * The following repositories are planned: 
      * fpc
      * fpcdocs
      * lazarus
      * contrib
      * fpcprojects
      * fpcbuild
      * html (web page contents)
      * logs
    * another packages dir: packages/contrib with contains packages which are maintained externally. Something like the Delphi Companion CD. The long term goal is however to flatten the packages hierarchy (packages/base and packages/contrib?), work only with "build units/packages" in each directory. Making e.g. the fcl and fv a usual package as well.
    * Stuff in packages/contrib which isn't maintained actively anymore is moved to the contrib repository.
  * In the first step only ssh based authentification will be possible.
  * After everything is setup and working, a read-only cvs mirror will be installed.
  * The SVN project is updated regularily and releases new versions, so it's necessary to have policy when to update the SVN server. The proposed policy is to keep up always with the latest stable svn version.
  * Each repository gets it's own unix user group so permissions can be easily managed.



## Usage

svn can be used very similiar to cvs. This wiki page gives some example commands to use the new subversion respository. For a complete list of commands use 
    
    
     svn help
     svn help <command>
    

or read _the_ svn book can be found here: <http://svnbook.red-bean.com/en/1.1/svn-book.html>. 

A sandbox repository is setup on idefix so you can test svn if you've an idefix account. The URL of this repository is svn+<ssh://svn.freepascal.org/FPC/svn/sandbox1>

## Repository layout

For the repository layout the defacto standard layout with trunk, tags and branches (virtual) directories will be used. 
    
    
    fpc/
      trunk/
        compiler/
        rtl/
        ...
      tags/
        release_2_0_0/
          compiler/
          rtl/
          ...
      branches/
        fixes_2_0/
          compiler/
          rtl/
          ...
        temporarydevelopment/
          compiler/
          rtl/
          ...
    
    
    
    fpcdocs/
      trunk/
        Makefile
        *.tex
        ...
      tags/
        release_2_0_0/
      branches/
    
    
    
    fpcbuild/
      trunk/
        Makefile
        demo
        ...
      tags/
        release_2_0_0/
      branches/
    
    
    
    logs/
      Changes.fpc
      Changes.fpcdocs
      ...
    

The Read-Only (anonymous) URL prefix is: 
    
    
    <http://svn.freepascal.org/svn/>
    

The actual urls are 
    
    
    <http://svn.freepascal.org/svn/fpc/>
    <http://svn.freepascal.org/svn/lazarus/>
    

  
The URL prefix needed to be able to commit changes is: 
    
    
    svn+<ssh://username@svn.freepascal.org/FPC/svn/>
    

This URL will be used in all examples below. 

On the local machine the repositories can be found with the URL 
    
    
    file:///FPC/svn
    

If you want to switch the URL prefix, for example change the logs repository svn+ssh to http, you can do this on windows with: 
    
    
    cd logs
    svn switch --relocate svn+<ssh://svn.freepascal.org:/FPC/svn/logs> <http://svn.freepascal.org/svn/logs> .
    

where svn.freepascal.org is in this case the name of the session of the svn server in Putty. 

On unix do: 
    
    
    cd logs
    svn switch --relocate svn+<ssh://svn.freepascal.org/FPC/svn/logs> <http://svn.freepascal.org/svn/logs> .
    

**Note:** Don't make a mistake when typing the second url else you've to re-checkout. 

To get a list of the directory layout the list command or its abbreviation ls can be used: 
    
    
    svn ls svn+<ssh://username@svn.freepascal.org/FPC/svn/fpcdocs/>
    svn ls svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/trunk/>
    

**Tip:** Put the URL you need in an environment variable in your shell profile like 
    
    
    export SVNROOT=svn+<ssh://username@svn.freepascal.org/FPC/svn>
    export SVNFPC=$SVNROOT/fpc
    export SVNFPCDOCS=$SVNROOT/fpcdocs
    

This way you can shorten the lines you need to type 
    
    
     svn ls $SVNFPC
     svn ls $SVNFPCDOCS
    

## Migration of CVS data

First release_2_0_0 tag will be imported, branched (fixes_2_0) and tagged (release_2_0_0). After that all changes made to CVS HEAD will be commited to the trunk and merged to the fixes_2_0 branch. 

The CVS repository for fpc is locked to prevent new commits. 

  


## Checkout

To checkout the trunk (main development) of fpc use the following command: 
    
    
    svn co svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/trunk/> fpc
    

To checkout the latest stable that will make up the next release use the following command: 
    
    
    svn co svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/branches/fixes_2_0/> fpc-2.0
    

Doing this with the win32 command line client is more tricky: first, get putty and install it and copy the plink.exe to the same directory as the svn client. Then create a new session in Putty, name it e.g. Idefix. Configure this session to auto login on idefix. Futher, create an enviroment variable SVN_SSH with the value plink.exe. Then do 
    
    
     svn co svn+<ssh://Idefix:/FPC/svn/fpc/trunk/> fpc
    

Example read-only checkout commands: 
    
    
    2.1.x: svn co <http://svn.freepascal.org/svn/fpc/trunk/>
    2.0.x: svn co <http://svn.freepascal.org/svn/fpc/branches/fixes_2_0/>
    2.0.0: svn co <http://svn.freepascal.org/svn/fpc/tags/release_2_0_0/>
    

## Updating and commiting

Updating and commiting works the same as with CVS with one expection. The exception is for files that had conflicts. Subversion requires you to explicitly mark files (or directories) that had conflicts as resolved. 

To update the current directory use the follwing command: 
    
    
    svn up
    

To only update the compiler directory use the following command: 
    
    
    svn up compiler
    

To commit your changes 
    
    
     svn ci
    

To mark a conflict as resolved: 
    
    
     svn resolved <file>
    

For committing with proper mime settings, you should enable auto-prop'ing in your svn client configuration (unix: ~/.subversion/config; Windows: %AppData%/subversion/config). Make the following settings: 
    
    
    [miscellany]
    enable-auto-props = yes
    [auto-props]
    *.sh = svn:eol-style=native;svn:executable
    *.pp = svn:eol-style=native;svn:mime-type=text/pascal
    *.pas = svn:eol-style=native;svn:mime-type=text/pascal
    *.fpc = svn:eol-style=native;svn:mime-type=text/plain
    *.inc = svn:eol-style=native;svn:mime-type=text/plain
    *.ini = svn:eol-style=native;svn:mime-type=text/plain
    *.xml = svn:eol-style=native;svn:mime-type=text/plain
    *.tex = svn:eol-style=native;svn:mime-type=text/plain
    *.log = svn:eol-style=native;svn:mime-type=text/plain
    *.htm = svn:eol-style=native;svn:mime-type=text/plain
    *.html = svn:eol-style=native;svn:mime-type=text/plain
    *.fig = svn:eol-style=native;svn:mime-type=text/plain
    *.php = svn:eol-style=native;svn:mime-type=text/plain
    *.php3 = svn:eol-style=native;svn:mime-type=text/plain
    *.tmpl = svn:eol-style=native;svn:mime-type=text/plain
    *.fpc = svn:eol-style=native;svn:mime-type=text/plain
    *.fp = svn:eol-style=native;svn:mime-type=text/plain
    *.sql = svn:eol-style=native;svn:mime-type=text/plain
    *.txt = svn:eol-style=native;svn:mime-type=text/plain
    *.png = svn:mime-type=image/png
    *.jpg = svn:mime-type=image/jpeg
    *.gif = svn:mime-type=image/gif
    *.ico = svn:mime-type=image/ico
    *.o = svn:mime-type=application/octet-stream
    Makefile = svn:eol-style=native;svn:mime-type=text/plain
    *.lfm = svn:eol-style=native;svn:mime-type=text/plain
    *.lrs = svn:eol-style=native;svn:mime-type=text/pascal
    *.lpk = svn:eol-style=native;svn:mime-type=text/plain
    *.lpi = svn:eol-style=native;svn:mime-type=text/plain
    *.lpl = svn:eol-style=native;svn:mime-type=text/plain
    *.lpr = svn:eol-style=native;svn:mime-type=text/pascal
    *.cfg = svn:eol-style=native;svn:mime-type=text/plain
    *.rc = svn:eol-style=native;svn:mime-type=text/plain
    *.manifest = svn:eol-style=native;svn:mime-type=text/plain
    

The following Delphi specific types are useful when porting delphi code: 
    
    
     *.bdsproj = svn:eol-style=native;svn:mime-type=text/plain
     *.dof = svn:eol-style=native;svn:mime-type=text/plain
     *.dpr = svn:eol-style=native;svn:mime-type=text/pascal
     *.dfm = svn:eol-style=native;svn:mime-type=text/plain
     *.dpk = svn:eol-style=native;svn:mime-type=text/plain
     *.bpg = svn:eol-style=native;svn:mime-type=text/plain
    

  
For adding new directories it is sometimes needed to add ignores for generated files or directories. The following textfile and script can be used to add the svn:ignore property recursivly 
    
    
    > cat ignore.txt 
    *.bak
    *.exe
    *.ppu
    *.o
    *.s
    fpcmade.*
    units
    
    
    
    > cat updignore.sh
    #!/bin/sh
    for f in `find . -type d | grep -v svn`; do
      svn ps svn:ignore -F ignore.txt $f
    done
    

## Branching and tagging

For all major development tasks a branch will be created. This is to have the trunk allways be in a buildable state to don't disturb snapshot building. Only in branch it is allowed to have code that can't be build. 

Branching will only be done on a complete repository. Example command to branch: 
    
    
    svn copy svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/trunk/> \
             svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/branches/newdevelopment/>
    

Tagging works the same as branching: 
    
    
    svn copy svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/trunk/> \
             svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/tags/newrelease/>
    

After a branch or tag created it can be checkout with the following command: 
    
    
    svn checkout svn+<ssh://username@svn.freepascal.org/FPC/svn/fpc/branches/newdevelopment/> fpc-newdevelopment
    

## Merging

For the compiler the policy is that all fixes and development will be done in the trunk. Small fixes can be merged to the fixes branch directly. Big fixes and minor development changes are also allowed to be merged if the core developpers agree. 

For the RTL, FCL and other packages. All fixes and new development can be merged as long as it also works with the latest released compiler. 

#### Merging automatically created files

When merging commits containing automatically created files like Makefiles or the msg*.inc files of the compiler, you usually get conflicts when merging them. To solve this you've to revert the merges of the automatically created files, recreate them and then commit everything. 

For a merge containing a change to Makefile.fpc the steps would be: 

  * execute the merge with svnmerge
  * revert all Makefiles
  * regenerate the Makefiles from the mutated Makefile.fpc's with fpcmake -Tall -w
  * commit everything



#### Merge with plain svn

  * On Windows use TortoiseSvn it has GUI (self-explaining)
  * For command-line svn, use



    

    svn [help] merge
    svn [help] mergeinfo

When committing the merge, in case of doing a selective commit (merged files only) include directories (including top-level). They contain svn-properties about the merge status 

  * **Always run in top level dir / specify top level dir for fixes/trunk**



    The root dir has a property 

    svn:mergeinfo
    containing what was already merged. It will be modified and must be committed

Using mergeinfo (which revisions have or have not been merged)
    

Raw list of all merged rev: 
    
    
    svn mergeinfo  trunk_dir  fixes_0_9_30_dir 
    

Raw list of all not yet merged rev 
    
    
    svn mergeinfo  --show-revs eligible  trunk_dir  fixes_0_9_30_dir
    

On Linux, to get log for mergeinfo 
    
    
    svn mergeinfo  --show-revs eligible  trunk_dir  fixes_0_9_30_dir  |  awk 'gsub("r", "-r ")' | xargs svn log trunk_dir
    

Using merge
    
    
    
    svn merge  trunk_dir  fixes_0_9_30_dir  -c 123[,124,128] [--force] [--dry-run] [--record-only] 
    

-c 123 

    The change that made rev 123 (change between r122 and r123 / equals "-r 122:123")
    Use "-r 123:130" to merge changes after r123 up to (including) 130

\--record-only 

    mark as merged, without merging (block this from being merged)

## Tools

Some people prefer GUItools instead of command line clients. 

  * A good client with explorer integration for win32 is TortoiseSVN (<http://tortoisesvn.tigris.org/> ). How to use it with ssh authentication can be found at [http://tortoisesvn.berlios.de/faq/index.php?action=artikel&cat=8&id=30&artlang=en](<http://tortoisesvn.berlios.de/faq/index.php?action=artikel&cat=8&id=30&artlang=en>).
  * RapidSVN is a GUI for Windows and Linux. It can be found at <http://rapidsvn.tigris.org>.
  * GUI tools for Mac OS X (<http://versiontracker.com>): SvnX, eSvn, iSVN, Subcommander



(Cygwin also has an SVN client) 

## Committing over http

For people behind firewalls and proxies, SVN access over HTTP was also configured: 
    
    
     http://svn.freepascal.org/FPC/svn/<repository>  

When committing you will be prompted for username and passwd. 

Note that some HTTP proxies don't support webdav correctly, so a HTTP connection cannot be used and ssh remains the only alternative for committing.

---

_Source: [https://wiki.freepascal.org/SVN_Migration](https://web.archive.org/web/20250125104215/https://wiki.freepascal.org/SVN_Migration)_
