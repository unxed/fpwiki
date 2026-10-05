# git migration

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The migration is finished, so this page is kept mainly for historic reference

This page is about migrating FPC from SVN to git 

## Contents

  * 1 Why?
  * 2 Why not ?
  * 3 Concerns/Questions
    * 3.1 What part of SVN to migrate ?
    * 3.2 Build repository
    * 3.3 Branching model ?
      * 3.3.1 Merging model
      * 3.3.2 How to handle the fixes branch
      * 3.3.3 A successful Git branching model
      * 3.3.4 The cactus model
      * 3.3.5 See the Git project itself
    * 3.4 User management ?
      * 3.4.1 gitorious
    * 3.5 gitea
    * 3.6 What about Lazarus ?
    * 3.7 Dependencies on Unix-world tools / ports ?
    * 3.8 Misc.
    * 3.9 Scenarios to document
      * 3.9.1 SVN scenarios that need an equivalent: (note that equivalency means that changes should be visible on the central server)
      * 3.9.2 (New) git specific scenarios
  * 4 Tool for migration
    * 4.1 subgit
    * 4.2 git-svn
  * 5 Work to do
    * 5.1 Migrate SVN repo.
      * 5.1.1 Branch mapping for subgit
    * 5.2 Set up user management and permissions
    * 5.3 Set up and automate github mirror



## Why?

  * More flexible and modern SCM (normally faster than svn, FPC development workflow might improve)
  * Some of the developers (known: Florian, Jonas) already use git svn regularily, for a lot of reasons (easier testing on different machines, recording of commits before pushing, parallel work on different features easily possible, before/after testing before pushing easily possible).
  * Third party contributors might have an easier life.
  * Stitching of the svn repository with the old cvs repositories possible: very complete history of FPC available



## Why not ?

  * Subversion allows inexperienced users to be given a simple "fixed from revision 12345 onwards" or "revert to r12340" statement.
  * Some of the developers don't use git (at least Marco, Nikolay), so it is an extra burden.
  * Conversion time, retraining.
  * Client configuration needed after install ? SVN client does not need much configuration, if any.
  * See list "misc"
  * Simple operations get more complicated. Complications also affect users that don't benefit from git. (double so if every commit must be done in a branch to emulate current merging model)
  * Uncertain merge model.
  * The third party contribution (pull request and the like) is very informal, and many of the new, inexperienced contributors routinely run formatters and the like. Practicality might be limited. In the Linux kernel these go through a layer of contributors and then branch lieutenants, we don't have such tiered structure for vetting, and require some discretion on the hand of the submitter.
  * Can users perform (accidental) irreversible damage to the central repository by pushing certain conflicts? GIT seems to allow much deeper internal meddling in the repo, and if these can be accidentally pushed......



## Concerns/Questions

### What part of SVN to migrate ?

  * More is better.   
Jonas has a very complete git mirror of the SVN+CVS part.  
(care needs to be taken: there used to be a time when copyrighted code was checked in)  
A first test conversion by Florian using subgit was attempted: completed in 5 hours 1, crash. Looks OK.
  * Does git have some form of obliterate option (remove revs based on copyrighted sources) ? Would solve some of the importing old history problems.  




    

    Yes, it seems it does. [See this StackOverflow topic](<https://stackoverflow.com/questions/18010048/how-to-permanently-delete-a-commit-from-gits-history>) \--[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 14:42, 19 December 2017 (CET) 

    The SO topic does not help much, but after some experimenting I got `git filter-branch --index-filter 'git merge-base --is-ancestor <hash> $GIT_COMMIT ; if [ $? -eq 1 ] ; then git rm --cached --ignore-unmatch <path in repo/file> ; fi' HEAD` which removes all revisions of <path in repo/file> which are not ancestor of <hash>. So this removes old versions of files which were cleaned by the commit identified by <hash>. This is reasonable fast (~ 1 hour with my currently converted repository). Afterwards, the file appears to be committed by <hash>. Drawbacks: All history before of the file is lost, also of clean parts. All revisions afterwards get new hashes, so this means, all clones done before must be removed. --[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 19:59, 19 December 2017 (CET)

  * In order to save on diskspace, find ways to tell user how to clone only a part.  




    

    Use `git clone --depth 1` to get only the latest revision (with fetch, later on more revisions can be fetched if needed)
    Is this really a problem? A git clone of FPC trunk with full history is 50% smaller (disk space usage) than a HEAD revision 'svn co' checkout using SubVersion. --[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 14:36, 19 December 2017 (CET) 

    It is not: my test respositry is around 500 MB on linux after gc, sources itself are around 350 MB

\--[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 20:18, 19 December 2017 (CET) 

    

    

    

    Just checked, only .svn directory of trunk checkout is 800MB If git clone is 500MB it is about 50% smaller. --[AlexVinS](</User:AlexVinS> "User:AlexVinS") ([talk](</index.php?title=User_talk:AlexVinS&action=edit&redlink=1> "User talk:AlexVinS \(page does not exist\)")) 00:35, 20 December 2017 (CET) 

    But after **svn cleanup** it gets reduced to 327MB. --[Nickysn](</User:Nickysn> "User:Nickysn") ([talk](</User_talk:Nickysn> "User talk:Nickysn")) 01:19, 20 December 2017 (CET)
    With aggressive gc .git gets 425 MB --[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 21:32, 21 December 2017 (CET)
    And do not forget: using git worktrees, you need only one .git dir for "trunk" and "fixes" --[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 21:35, 21 December 2017 (CET)
    And I worry mainly about fpcbuild with its non differential history, since my most major disk constraints are VMs for release building [Marcov](</User:Marcov> "User:Marcov") ([talk](</index.php?title=User_talk:Marcov&action=edit&redlink=1> "User talk:Marcov \(page does not exist\)")) 15:01, 21 December 2017 (CET) 

    `--depth 1` works with submodules too--[AlexVinS](</User:AlexVinS> "User:AlexVinS") ([talk](</index.php?title=User_talk:AlexVinS&action=edit&redlink=1> "User talk:AlexVinS \(page does not exist\)")) 15:55, 21 December 2017 (CET)
    Don't worry too much: fpcbuild trunk .svn: 57.2 MB; full fpcbuild .git: 40.4 MB. So if you are still worried about disk space, we should switch to git asap --[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 21:32, 21 December 2017 (CET) 

    That would resolve my scenario with the biggest (feared) size problems. [Marcov](</User:Marcov> "User:Marcov") ([talk](</index.php?title=User_talk:Marcov&action=edit&redlink=1> "User talk:Marcov \(page does not exist\)")) 12:01, 22 December 2017 (CET)

### Build repository

The fpc build repository uses svn:external references. 

Git has modules, which is in essence the same. This needs to be properly set up. 

### Branching model ?

#### Merging model

Merge branches using fast-forward or not ? 

The 2 models mentioned below use a different merging technique, fast-forward using merge commit. A hybrid could enforce fast-forward on some branches (notably, the releases branch), see [track all remote git branches as local branches](<https://stackoverflow.com/questions/379081/track-all-remote-git-branches-as-local-branches>)

In particular, the command to force fast-forward: 
    
    
    git config --add branch.master.mergeoptions --ff-only
    

#### How to handle the fixes branch

FPC uses trunk for development. Release are generated from the fixes branch. The fixes branch consists of cherry picked commits from trunk. After a patch proved to be good in trunk, it is cherry picked and committed to the fixes branch. This is a development model which works very well with subversion and is used by FPC for almost 20 years. However, to make this development work well, it requires that the SCM supports cherry pick tracking, something git does not have. So this workflow does not work well with git. Changing the workflow of FPC is not an option at this point, it proved to work, furthermore there are good reasons to use it: When making a patch, it is often hard to know if it is good enough for fixes or not, normally, it is decided after some time or even short before the next release, if a patch is good and safe enough to make it into fixes. This might not happen because it required more changes and those changes got too invasive or because it e.g. turned out that caused incompatibilites which make the patch a candidate for the next minor release instead. So this is probably the key question for a full git migration: can git be used with the workflow fpc uses? 

So far, various workflow models exist: 

#### [A successful Git branching model](<http://nvie.com/posts/a-successful-git-branching-model/>)

Also known as "git-flow" model. 

Advantages

  * A simple and logical workflow that plays to the strengths of Git - branching and merging.
  * 'master', 'develop' and 'release' are good branch names which immediately makes it obvious what they are for. Many developers clone a repo and want or expect a stable version. This workflow allows just that - the 'master' branch is the default branch, and is always the latest stable release of the product.
  * Features or multi-commit (complex) bug fixes show a clear commit history or related commits, that is easily tracked, viewed or even rolled-back in the commit history using a tool like gitk.



Disadvantages

  * It works only well if the the whole release policy of FPC is changed. This is something very likely not going to happen.
  * cluttered and basically unreadable history
  * doubled regression testing effort during development of new stuff 
    * test branch before merge
    * test branch after merge, as merging could have broken things
  * ~~micro management of branches: every single bug fixes requires and results in a new branch~~ This is simply not the case. single bug fixes can be committed into the 'develop' branch as a linear history. Only more complex (multi-commit) fixes or features will result in a parallel history or development, but quickly merged back into 'develop'. --[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 18:50, 17 December 2017 (CET) 

    

    You do not see the problem: if it is in develop, it will never make it into master without merging whole develop into master (i.e. in terms of FPC releases this would mean a new minor release x.y+1.0). So each bug fix has to go into an own hotfixes branch (e.g. hotfix_bug12345) which is branched from master and merged first into develop. If it works, it will be merged back into master. --[FPK](</User:FPK> "User:FPK") ([talk](</index.php?title=User_talk:FPK&action=edit&redlink=1> "User talk:FPK \(page does not exist\)")) 22:58, 17 December 2017 (CET)
  * One has to know before pushing how invasive a change is because it has to go in the right branch. For compiler development this is very cumbersome.
  * develop and release have no clearly described function it seems.



#### [The cactus model](<https://barro.github.io/2016/02/a-succesful-git-branching-model-considered-harmful/>)

Also known as the anti-"A successful Git branching Model" 

Advantages

  * clear and straight history
  * no time consuming micro management of branches
  * history and logs not cluttered with merge commits
  * no breakage by wrong conflict resolving during merge commits by less experienced users



Disadvantages

  * Git was designed to work well with branches and handle merging as a common task. This workflow suggests merging as a bad idea, which is crazy.
  * This model recommends that a human must now keep track of which commits to cherry-pick into other branches. This is bounds to fail in the long run and vital commits will be missed at some point. Merging two or more branches on the contrary will automatically handle all commits in a branch seamlessly, so why not use what Git was designed to do well.
  * This model suggests that you can also cherry-pick from an unstable branch back into a stable release branch. This is just the wrong way round.
  * This model sees some of its own flaws and suggests a 3rd party tool to help keep track of things like cherry-picking. This simply isn't needed if you use a better workflow.
  * It suggests that 'rebase' get used often. This has always been frowned upon in the Git community, and it is widely known to use rebase sparingly as it rewrites history and doesn't result in a logical evolution of the code history.
  * This worklfow suggests that a merge commit is a bad thing. A merge commit is a commit like any other. It can be a simple commit (with no conflicts), or can be a commit that resolves conflicts (thus code changes were required - which this commit records). Nothing bad about that.



#### [See the Git project itself](<https://git-scm.com/docs/gitworkflows>)

Advantages

  * The git project itself gets an extreme amount of contributions via pull requests and emailed patches. Their workflow model clearly works well and uses branching and merging extensively - even though the commit history looks quite scary. :-)



Disadvantages
    

  * No real description of the process.



### User management ?

Git has no concept of users. To manage permissions on a server, a separate program is needed. 

  * Correction: A separate program makes some functionality a bit easier but not mandatory. Simple UNIX style group and file permissions with SSH public keys work very well too. --[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 14:32, 2 January 2018 (CET)



#### gitorious

  * Advantages 
    * uses git repo for administration
    * No server binary
  * Disadvantages 
    * No web interface
    * administration needs ssh key, only ssh possible.
    * Web integration ?



### gitea

  * Advantages 
    * Web based
    * Fine tuning possible
  * Disadvantages 
    * Separate config
    * Requires running binary all the time, on a separate port.



Git differentiates between the person that does a commit, and the author of a change/patch. The latter is always available and doesn't need any repository permissions. Somebody with write permissions can commit patches (or merge pull requests) and keep both the Committer and Author information intact. SubVersion doesn't track the author of a change, only the person that did the commit. 

### What about Lazarus ?

The Lazarus team (Martin Friebe) is currently maintaining a copy of the SVN repo on gitea. 

  * <http://svn.freepascal.org:8080/Lazarus/lazarus> (does not work)
  * <https://github.com/graemeg/lazarus>



### Dependencies on Unix-world tools / ports ?

There seem to be dependencies on quite a few tools common on Unix platforms. Unfortunately, ports of these tools may not be up-to-date and/or working well on non-Unix platforms. As an example, port of GIT 2.13.3 to OS/2 lists dependencies on a Unix shell, Python, Perl and CURL among others. Using Unix shell for launching other programs on a non-Unix platform may have various unwanted effects (remember cygwin ports which we tried to avoid on MS Windows as much as possible for similar reasons). The list of dependencies seems to suggest that quite a few operations require external tools (similarly to merging with early SVN versions). It may be difficult to test all such cases potentially important for certain workflows in advance before the final switch. 

    

    OS/2 being the first commercial operating system to ship with Java built-in. So why not simply use the Java version of Git. The Eclipse and IntelliJ IDEA project do on a daily basis. --[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 14:39, 18 December 2017 (CET)
    History of OS/2 and Java are one thing, practical usability is another. First, I'm not sure that I want to run Java runtime to commit a change to one file, get a log of recent changes, etc. Second, which JRE version would be necessary for that? Are such JRE versions available for OS/2? --[User:Xhajt03](</User:Xhajt03> "User:Xhajt03")
    Sorry, another addition - the MS Windows installation of GIT contains a subdirectory with a mingw64 tree (containing a bunch of stuff like bzip2.exe, gettext.exe, antiword.exe, openssl.exe, odt2txt.exe, sqlite3_analyzer.sh or tclsh.exe), plus another subdirectory containing another copy of bzip2.exe plus e.g. gawk.exe, gzip.exe, iconv.exe, nano.exe, perl.exe, sed.exe, ssh.exe, xargs.exe, i.e. a _lot_ of 3rd party tools from various sources, not even mentioning the main bin directory containing bash.exe and sh.exe in addition to git.exe). In total, the Git installation there amounts to 500 MB!!! SVN installation on the same machine is 17 MB, and even the TortoiseSVN (i.e. the GUI addon) is 47 MB. Don't get me wrong, it isn't the size that scares me, but the amount of dependencies. Although many of these tools are probably ported to OS/2 too, they may not be compatible to each other (e.g. requiring different versions of the same DLLs, etc.). :-( I'd need to understand which of the tools are really strictly necessary for regular working with Git (see the section with scenarios on this page). 

    There is supposedly an rpm package for OS/2 that installs everything you need (for command line git): <http://www.edm2.com/index.php/Using_Git_under_eComStation>
    This is indeed annoying on windows if files like bash or "sh.exe" are in path due to this. [Marcov](</User:Marcov> "User:Marcov") ([talk](</index.php?title=User_talk:Marcov&action=edit&redlink=1> "User talk:Marcov \(page does not exist\)")) 13:33, 1 February 2020 (CET)

### Misc.

Collected from mails of Marco which is not handled in other sections: 

  * I really dislike losing global revs.
  * Will we need to store anything in one repo (build and fpc?) Would be huge with all histo, and problematic for release building VMs etc . What are the options for partial checkouts/externals ?
  * Can branches be in a not usable (multi-head) state? I heard some complaints about that, where users avoided work till it was fixed, and had flashbacks of locking VCSes all over again. Should not be possible.
  * git requiring sequences of commands for simple operations (local then global push etc)
  * lineendings, should be entirely server dictated. In general, most scenarios should be simple singular commands not several with bunches of parameters.
  * from the "branching" paragraph: readable logs. Marco didn't add it because he never would have guessed it would be a problem.
  * I heard github mentioned a few times, did sb check GH licensing?



    Answer from Chain|Q: 

    <https://help.github.com/articles/github-terms-of-service/>
    Point "D" caused an uproar a while ago, because some thought that it implied that you're granting GitHub rights to use your content as they see fit. This has been clarified since, I think.

### Scenarios to document

#### SVN scenarios that need an equivalent: (note that equivalency means that changes should be visible on the central server)

  * install on new machine, mandatory configuration (preferably: none mandatory other than URL) for at least linux and windows supported client (tortoise?). Things to setup/configure(server dictated?) 
    * crlf? 
      * The defaults which git supplies (Linux, Mac and Windows) should be just fine. Repo internal storage is LF and checked out code is the native line endings of your system. The ".gitattributes" file could control line endings of specific file types. 
        * this file is in the repo, iow server dictated, making it zero conf?
    * Configure user name and email address for commit messages 
      * git config user.name "John Doe"
      * git config user.email "johndoe@example.com"
      * Can add "--global" after "config" if you want to set this as the default for all repositories
  * Checkout a working repo 
    * command: git clone <url>
  * Changing from trunk to a fixes branch (and vice versa) 
    * command: git checkout [<branch>]
    * Two different checkouts are usually used with SVN, but if it's done differently with git, it should be documented here as well
  * update/sync 
    * Get latest changes via command: git fetch [remote_server] 
      * This only downloads the changes, without applying them. In order to apply them, you have to do a 'git rebase', which will reapply your local commits on top of the remote changes. If there are conflicts, you will have to resolve them (TODO: "git add" as equivalent for "svn resolved", and "git rebase --continue" to continue after adding all resolving conflicting files)
    * Push latest changes via command: git push
  * check for modifications (svn status) 
    * command: git status
  * see history log (svn log -v) 
    * command (console only): git log
    * or command (gui showing all branches): gitk --all
  * commit a simple modification. 
    * A two/three step process: 
      * git add <file_that_changed_or_that_is_new>
        * Trap for Subversion users: If you apply further changes to the file, you need to 'git add' again the same file, otherwise git won't include them in the commit (i.e. 'git add' takes a snapshot of the file at the moment you enter the command).
        * To undo 'git add <file>', use 'git reset <file>'.
      * git commit -m "commit message"
      * git push (to send all local commits to the server) 
        * This will refuse to work if there have been any remote changes (even if they are in different files). In that case, first "git fetch" and "git rebase" before pushing again. For comparison, Subversion will ask you to update and merge the remote version, only if there are any changes in the files you're trying to commit.
  * get the list of eligibles
  * create a branch/tag (fixes/release/rc) 
    * All-in-one command: git checkout -b <new_branch_name> [starting_point]
  * merge a revision to a different branch (trunk-> fixes or fixes ->release/rc) (added later:I assumed this was tracked. The answer below might not reflect that) 
    * Very easy via the GUI gitk tool. Right-click and select "cherry pick" 
      * This works only in simple priyatna.orgcases. In general I think


    
    
     git cherry-pick -x -n <commit-id>
    

<if needed, fix conflicts>
    
    
     git commit -F .git/MERGE_MSG 
    

is the way to go. This tracks the merged revision in the commit message and it can be search by 
    
    
     git log --grep <commit-id>
    

if a certain revision is cherry picked. 

  * edit commit message 
    * Generic, safe way 
      * git rebase -i <commit-hash>^ (note the extra ^ at the end)
      * in the window that appears, change "pick" into "r" or "reword" for the commits whose commit message you wish to change
      * save the file, and git will open a new editor for each commit message you selected for rewording
    * Shortcut in case you want to change the commit message of HEAD (in case you did not yet push it to the remote server) 
      * git commit --amend
      * Note: if you added any changes to the index (= used "git add" to select changes to commit), these changes will also be added to that same commit. Use "git reset" to remove them from the index again, if you do not want this)


  * revert local changes (svn cleanup) 
    * git reset --hard HEAD
  * show annotations for a particular file (svn blame) 
    * git blame
  * show diffs for a particular revision of a particular file or of all files included in that revision (comparing it to the previous revision or to some other explicitly specified revision) 
    * git show -p <commit-hash>
  * exporting 'clean' sources (without repository metadata) for release building (svn export) 
    * git checkout-index --prefix=destination/directory/ -a



#### (New) git specific scenarios

  * Perform a change not (yet) visible on the central server, but still recognized as a specific change with a specific commit message in the local repository
  * Restructure local changes from the previous scenario to a different set of commits



## Tool for migration

### subgit

Advantages 

  * very flexible
  * very fast (remote cloning of the whole FPC repository takes 5-6 hours)



Disadvantages 

  * external java-based tool, so some dependencies



### git-svn

Advantages 

  * included in git it self



Disadvantages 

  * less flexible



## Work to do

### Migrate SVN repo.

  * 2017-12-16: A first test conversion by Florian using subgit was attempted: completed in 5 hours, 1 crash. Looks OK



#### Branch mapping for subgit

Proposal for branch mapping 
    
    
    trunk = trunk:refs/heads/master
    branches = branches/merged/*:refs/heads/merged/*
    branches = branches/joost/*:refs/heads/joost/*
    branches = branches/laksen/*:refs/heads/laksen/*
    branches = branches/maciej/*:refs/heads/maciej/*
    branches = branches/olivier/*:refs/heads/olivier/*
    branches = branches/paul/*:refs/heads/paul/*
    branches = branches/*:refs/heads/svn/*
    branches = branches/svenbarth/*:refs/heads/svenbarth/*
    branches = branches/tg74/*:refs/heads/tg74/*	
    tags = tags/*:refs/tags/*
    shelves = shelves/*:refs/shelves/*
    excludeBranches = branches/aspect
    excludeBranches = branches/avr32
    excludeBranches = branches/blaise
    excludeBranches = branches/cpstr
    excludeBranches = branches/cpstrnew
    excludeBranches = branches/ctypes
    excludeBranches = branches/dodi
    excludeBranches = branches/florian
    excludeBranches = branches/foxsen
    excludeBranches = branches/fcl-web_joost
    excludeBranches = branches/generics
    excludeBranches = branches/genfunc
    excludeBranches = branches/janbruns
    excludeBranches = branches/linker
    excludeBranches = branches/merged/avr
    excludeBranches = branches/merged/generics
    excludeBranches = branches/merged/nodeopt
    excludeBranches = branches/newthreading
    excludeBranches = branches/peterjan
    excludeBranches = branches/ssa
    excludeBranches = branches/tg74/rtl
    excludeBranches = branches/tg74/tests
    excludeBranches = branches/tg74/utils
    excludeBranches = branches/unitrw
    excludeBranches = branches/wkrenn
    excludeBranches = branches/FIXES_2_2
    excludePath = /fixes_2_0
    excludePath = /fixes_2_4
    excludePath = /trunk
    

### Set up user management and permissions

### Set up and automate github mirror

  * I host the unofficial Git mirror at (<http://github.com/graemeg/freepascal.git>). I can share my cron jobs and script if anybody is interested. --[Graeme](</User:Ggeldenhuys> "User:Ggeldenhuys") ([talk](</User_talk:Ggeldenhuys> "User talk:Ggeldenhuys")) 14:25, 2 January 2018 (CET)

---

_Source: [https://wiki.freepascal.org/git_migration](https://web.archive.org/web/20241212112312/https://wiki.freepascal.org/git_migration)_
