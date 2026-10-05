# SVN to GIT Cheatsheet

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page is **not an extension** of [FPC_git](<FPC_git.md> "FPC git")  
This is an **alternative** to it. 

## Contents

  * 1 ABOUT
    * 1.1 Alternative to this Page
    * 1.2 Useful to know background
    * 1.3 Outdated info on the Web / ~~git checkout~~
  * 2 Setup
    * 2.1 Recommended
  * 3 Command MAP
    * 3.1 svn checkout
    * 3.2 svn update
    * 3.3 svn update -r <rev>
    * 3.4 svn switch
    * 3.5 svn commit
    * 3.6 svn add
    * 3.7 svn revert
    * 3.8 svn rename
    * 3.9 svn delete
    * 3.10 svn log
    * 3.11 svn ls
    * 3.12 svn diff
    * 3.13 svn resolved
    * 3.14 svn shelve
    * 3.15 svn export
  * 4 Other git commands
    * 4.1 git status
  * 5 Fast-Forward, Rebase and Merge
    * 5.1 Fast-Forward
    * 5.2 merge (merge within the current branch / merge as pull strategy)
    * 5.3 rebase (merge within the current branch / merge as pull strategy)
  * 6 "Tracking Branches" - Connect your local branch and its remote counter-part (aka "Upstream")
    * 6.1 What is tracking
    * 6.2 Setting / Un-Setting tracking
  * 7 Nice to know / for later
    * 7.1 git worktree / having more than one checkout
    * 7.2 Improving git diff for pascal



# ABOUT

This page contains an introduction to GIT, for users with prior SVN experience. 

The long version: <https://git-scm.com/book/en/v2>

## Alternative to this Page

There is a prior page with similar intent: [FPC_git](<FPC_git.md> "FPC git")

This alternative page differs in some main points: 

  * The alternative is based on the older/established git commands "checkout" and "reset". This page uses "switch" and "restore"



    Those older commands are harder to learn/understand. They do not have clean distinction between different tasks.
    The makers of GIT introduced the new commands as a replacement. But the new commands are still marked as "experimental". They are stable and safe to use. But they may still have changes in future git versions.
    In my opinion the fact that they are easier to learn, compensates well for the off chance that one may later have to learn about potential changes to them. 

    (Also, even old/established commands can get changed. But less likely)

  * The alternative page teaches "using the index" as the way to go.



    This is only one option to use git. As it requires additional steps in several commands, it is less preferable to users coming from svn.
    The GIT documentation contains all the steps, how to use GIT without "index". So this is as good a way to use git as any other. 

    (One still needs to be aware, in case one accidentally access the index.)

## Useful to know background

There a few concepts about git, that everyone should at least be aware of. 

**Please read the page[FPC_git_concepts](<FPC_git_concepts.md> "FPC git concepts")**

## Outdated info on the Web / ~~git checkout~~

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Some OS ship older versions of git, that do not yet support this. In this case see [FPC_git](<FPC_git.md> "FPC git") for how to use "git checkout"

A lot of pages on the web, will still give examples for using ~~git checkout~~. **Do not use this.**

The new git commands are: 

git switch
    for changing (and creating) branches.
git restore
    for restoring (resetting/reverting) files.

# Setup

Clone, to create a repository
    

If you go to the gitlab home page of the project <https://gitlab.com/freepascal.org/lazarus-ide/lazarus_testconversion> there is a "clone" button, which offers you to put the clone-url into your clipboard. 
    
    
       git clone <https://gitlab.com/freepascal.org/lazarus-ide/lazarus_testconversion.git>  ./lazarus_git
    

  * The folder lazarus_git will be created for you.



Set up your user details
    
    
    
       cd ./lazarus_git
       git config user.name  martin
       git config user.email martin@email-domain.host
    

  * Those details will appear on every commit you make



    

  * If you clone to an other device, you have to apply the settings again.
  * On the same PC, you can make the settings global "git config --global user.email foo@bar.com"



  * Those are **not** your login details



To view current settings 
    
    
       git config -l
    

  


Adding your credentials (required to push commits)
    

In order for your credentials to be remembered run (you may check "git config -l" first, to see if already set) 

  * on Linux / Mac)


    
    
       git config --global credential.helper store
    

  * On windows credentials should be stored in the Windows Credential storage.



    (Unable to test currently / should be either manager-core or wincred)
    The install may have added this already
    
    
       git config --global credential.helper wincred
       
    

Then run 
    
    
       git push
    

This will add you for your credentials. 

  * Enter the username you use to access the gitlab webpage. You can also use the primary email with which you registered.
  * Enter your password for the gitlab webpage



    

  * If you do not want to use your main password it is advised to use an "access token" as password (the username remains as is).



    Log into the gitlab webpage and in your user settings section find the "access token" page. Scope should be read/write repository.

## Recommended

Some advised defaults. 

For svn users, it is often desirable to keep the commit history "flat". That means within a single branch, you want to avoid having 2 strains of commits (diverge) and then merge together again.   
By default git may do exactly that, when you have local commits, and get new commits from the server. 

To tell git you prefer a single chain of commits (in each branch) run 
    
    
       git config pull.ff only
    

  
Depending on preference you can **alternatively** run (see details on fast-forward, rebase, and merge) 
    
    
       git config pull.rebase true
    

  
Having added this config allows you to drop the matching argument from "git pull" in the command map below. You can then simply run "git pull" (depending on which config you made, and which option would be advised) 

**If you do not yet want to read up on the topic, you should go with "pull.ff only"**

# Command MAP

## svn checkout
    
    
       git clone <url> <directory>
    

The amount of history can be limited with (for the master branch, the given branch, all branches) 
    
    
       git clone --depth <number> <url> <directory>
       git clone --depth <number> --branch <somebranch> <url> <directory>
    

## svn update

See the notes on fast-forward, rebase and merging. To keep a flat commit line use --ff-only or –rebase. To update the current branch: 
    
    
       git pull --ff-only
    

If you have unsaved (i.e. uncommitted) changes in files that need to be updated: 
    
    
       git stash save
       git pull --ff-only
       git stash pop
    

You can always try to "pull" without "stash". If it is not possible, git will tell you. You can then use the command sequence with "stash" and "pop". 

"git stash pop" may give a "conflict" error, if the changes can not be merged. You then need to resolve this yourself (same in svn) 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** "svn checkout" into a checkout with local changes will force the checkout, and introduce any conflicts immediately.   
"git pull --ff-only" will not do that. It will tell you that there would be a conflict. You can then decide to deal with it now or later. 

You can also use 
    
    
       git pull --rebase
    

instead of “--ff-only” if you have local commits. This will move your local commits, behind any new commits pulled from the server. 

  
To update the all remote branch(es), without touching the local branches. 
    
    
       git fetch --all
    

After this, you can use "git log origin/branch" to see what changed remotely. 

  * You may also find examples with: "git remote update"



## svn update -r <rev>

To go to an earlier revision, in git you create a branch at this earlier commit. 
    
    
       git switch -c <local-branch-name> <commit-hash>
    

Or to go (let’s say) 3 commits back 
    
    
       git switch -c <local-branch-name> HEAD~3
    

## svn switch

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** "svn switch" has to retrieve the other branch from the server, and may include an update to its latest version.   
In GIT all the branches are already downloaded. So switching only needs to update the files in your working directory.  
If you wish to get the latest version from the server you can run either: 

  * git fetch --all ## before you switch
  * git pull --ff-only ## after the switch


    
    
      git switch <local-branch-name>
    

will either: 

  * switch to an existing local branch
  * create a new local branch, from a remote branch of the same name



To force create a new local branch (this will give an error, if the local branch already exists): 
    
    
       git switch -c <local-branch-name> <remote-branch-name>
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** With -C (uppercase) you can force the creation, abandoning the old local branch. This may loose the data on any commits existing on the old local branch. (This can be used, if the commits in question are hold by yet another local branch)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Some pages on the internet will use “git checkout”. Do not use this.

## svn commit

In git your commit goes to your local branch. To do the full svn commit, you need to commit and push in git. 
    
    
       git commit -m ‘message’ file1 file2 file2 …
       git push
    

You can collect as many commits as you want before you push them. 

New files need to be “add”ed first. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Using git commit, without specifying files will commit the change you “index”ed. Read the section on the index. Indexed files may have different content, than the file on your hard-drive. Using commit with a list of files will always commit the listed files only and with the content you last saved to your hard-drive.

To commit all modified files (as on your hard-drive), including any file you deleted (new files still need to be added first): 
    
    
       git commit -m ‘my message’ -a
    

## svn add
    
    
      git add -N <file>
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you forget the “-N” the notes on the “index” apply. In that case, if you make further changes to the file, then you must specify the file on the “git commit” command line again.

## svn revert
    
    
       git restore <file>
       git restore <folder>
       git restore **/<file>
    

<folder> includes all subfolders. So does folder/* because * matches the names of subfolders. 

"**" means any path of zero, one or more levels. "anywhere within" 

Revert to the content of the previous commit: 
    
    
       git restore --source HEAD~1 <file>
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Some pages on the internet will use “git checkout”. Do not use this.

## svn rename
    
    
       git mv <source> <dest>
    

## svn delete
    
    
       git rm <file>
    

  * Files that are modified can only be deleted by force -f
  * For recursive removal use -r



## svn log
    
    
       git log -n <number_of_entries>
       git log --oneline –graph -n <number_of_entries>
    

Instead of a number of commits, you can give a range of revisions (by hash, branchname, …). This also allows you do get a log of a different branch. 
    
    
       git log HEAD~10..HEAD
       git log branch~10..branch
    

## svn ls

for local branches 
    
    
       git branch
    

to include remote branches 
    
    
       git branch -r
    

to be verbose 
    
    
       git branch -v
    

To see tags 
    
    
       git tag
    

## svn diff

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** the first of the below commands can be affected by the “index”, if you use the index.
    
    
      git diff 
      git diff –no-index
     
    

To compare with other commits 
    
    
      git diff <hash>
      git diff HEAD~10
    

## svn resolved

Conflicted files can be seen using git status. 

Conflicted lines in the code are marked similar to svn, using <<<, >>>>, ==== sections. You can edit the file, to merge the changes by hand. Or you can use either of the following to only keep your changes, or the changes of the other: 
    
    
       git restore --ours file
       git restore --theirs file
    

You can restore the conflict info in the file with 
    
    
       git restore --merge file
    

You can switch between the above states as often as you like. But be aware that any changes you edited in the file will be reset with the above restore. Once you have the files in an acceptable state, you can mark them as resolved resolved with either: 
    
    
       git reset file
       git add file  ## this will use the index
    

  


Conflicts during update 

If the conflict occurred during a “git pull”, then you still have to execute the “git stash pop” (You should have seen a message that it failed) If the conflict occurred after a “git stash pop”, then the stash is also still existent, despite being applied too. To verify view the stash “git stash list”, and to remove the last of the list “git stash drop” (optional specify the hash from the list). 

## svn shelve

To put all your current changes away (the short form only works without arguments / see below)  
The files in your workdir will be restored to their unmodified state, after the changes were saved. 
    
    
       git stash push
       git stash
    

To retrieve your changes 
    
    
       git stash pop
    

Changes are stored as patch. So you can apply (pop) them to any commit, not just the one from which you stashed them. 

To view all stashes 
    
    
       git stash list
    

More ways to add stashes 
    
    
       git stash push -m 'log message'
    

And to stash selected files only 
    
    
       git stash push -- file1 file2
    

## svn export

Assuming you current dir is the git repo dir. 
    
    
       git  --git-dir=.git  --work-tree=/path/to/export/dest  restore  .
    

If you only want a specific folder (and its subfolders) to be exported 
    
    
       git  --git-dir=.git  --work-tree=/path/to/export/dest  restore  --source=HEAD:folder/in/git  .
    

of course you can also do 
    
    
       git  --git-dir=.git  --work-tree=/path/to/export/dest  restore  folder/in/git
    

but then you get your files in 
    
    
       /path/to/export/dest/folder/in/git/
    

# Other git commands

## git status
    
    
       git status
    

Will list files grouped in the following sections. Files can be listed as modified, deleted, new file, renamed. 

  * Changes to be committed



    This is the section explained below as “index”. This are files that you specified to git add, git rm, git mv.
    Note that files can be listed again in other sections. Indicating that they had further changes after they where “added” (or rm/mv).

  * Unmerged Path



    Files that are in conflicted state

  * Changes not staged for committed



    Modified files (before add,rm,mv)

  * Untracked files



    Anything in your working tree that is not known to git (yet)

If you have plenty of "untracked files" you can omit them by using 
    
    
       git status -uno
    

# Fast-Forward, Rebase and Merge

Please read
    <https://wiki.lazarus.freepascal.org/FPC_git_concepts#Local_and_Remote_Branches> and tracking section below

Unlike svn (where any commit goes straight to the server), in git you make commits locally and send them to the server later. 

Therefore it can happen, that you added a local commit on the master branch, and some one else has pushed and added his commit to the remote. 

  

    
    
       A - B - C - D (origin/master as of last pull) - E (master your local commit) 
                    \- E' (commit pushed to the server by someone else => origin/master as it will be after the next pull)
    

Both "E" commits are based on the same parent "D". The commits no longe form a single flat line. 

At this point you can't push, as the server would not know how to arrange the two commits. Both commits want to be the head of the same master branch. So you must pull first, and sort this out. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In svn the server would rename your commit to F, and put it behind the other E. The svn server does so without knowledge if that result is any good.  
The svn server will reject your commit, but only if the changes for your E and the remote E conflict. In that case you have to svn update first.  
In git it is always up to you, to decide if reordering will give a useful result. Hence you always have to pull and sort out the commits.

## Fast-Forward

This is the most similar to what svn does. This can be done if your local branch is up to date with its remote, and you have no new local commits on top of the remote. 
    
    
       A - B - C - D (master, origin/master)
    

You pull from the remote, and new commits are loaded from the remote. 
    
    
       git fetch --all
       A - B - C - D (master) - E - F (origin/master)
    

Now you branch is behind, and you want to catch up. You can pull. Even though you already downloaded, and there is nothing new to download, git will perform a fast forward. (Like a life stream, with time shift, when you fell behind) 
    
    
       A - B - C - D (master) - E - F (origin/master)
       git pull --ff-only
       A - B - C - D - E - F (master, origin/master)
    

Since you already have the remote changes, and if you do not want to check the server again, you can use (**make sure you specify --ff-only**)  
For this form of "git merge" with no target the current branch must be tracking a remote branch. It will fast forward to the head of the tracked branch. 
    
    
       A - B - C - D (master) - E - F (origin/master)
       git merge --ff-only
       A - B - C - D - E - F (master, origin/master)
    

With git merge you can also decide to forward to one of the commits half way to the remote's origin/master 
    
    
       A - B - C - D (master) - E - F (origin/master)
       git merge --ff-only  HASH-OF-E
       git merge --ff-only  origin/master~1  # One before origin/master
       A - B - C - D - E (master) - F (origin/master)
    

  
A fast forward simply move the pointer of your branch from the commit it is on, to the commit of the tracked remote branch. 

You can only fast forward to commits, that you can reach by **only** going forward (to commits made on top of the current commit). This ensures that the current commit will remain part of the current branch, and all that is done is that new commits are added to the branch. 

  


What happens to your local modifications?
    

Lets say you had some modifications to "source.pp". 

  * If the commits that you forward over, do not change that file, then all is ok. All other files will be updated, and your changes will remain in place.
  * If source.pp is changed by any of the commits then fast forward will not work.



    The same applies, if you have a new local file feature.pp, and one of the commits tries to create this. See the note in #svn_update
    Use svn stash / svn stash pop

## merge (merge within the current branch / merge as pull strategy)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** "merge" is the "git way which differs from SVN"  
The svn-like way is "rebase"

Take the initial example. 
    
    
       A - B - C - D (origin/master as of last pull) - E (master your local commit) 
                    \- E' (commit pushed to the server by someone else => origin/master as it will be after the next pull)
    

If you merge (which is the default in git), then you will keep the E both having the parent D. 

The branch master will diverge (split into 2 parallel strains) and come together again. On a tracking branch: 
    
    
       git pull --ff-only  ## fast forward will not be possible
       A - B - C - D - E (master your local commit) 
                    \- E' (origin/master)
       
       git merge
       
       A - B - C - D - E  - F (master) 
                    \- E' /
                       (origin/master)
    

The merge commit "F" was created. It incorporates the changes from both its parents. 
    
    
       git push
       A - B - C - D - E  - F (origin/master, master) 
                    \- E' / 
    
    

The remote server can accept the diversion. Important is that there is only one head commit (to which the branch points). That is now "F". 

  
Both E and E' are on the same branch "master". SVN does not know such a layout. 

**"git rebase" can be used to avoid such "diverging within a branch".**

## rebase (merge within the current branch / merge as pull strategy)

Take the initial example. 
    
    
       A - B - C - D (origin/master as of last pull) - E (master your local commit) 
                    \- E' (commit pushed to the server by someone else => origin/master as it will be after the next pull)
    

You want to keep a flat line of commits. 

The commit E' is already on the server, and there it has the parent D. So that can't be changed any more. E' will remain next after D. 

This means your commit E must go after E'.  
You want to get the following commit order 
    
    
       A - B - C - D - E' - E
    
    
    
       git pull --ff-only  ## fast forward will not be possible
       A - B - C - D - E (master your local commit) 
                    \- E' (origin/master)
       
       git rebase
       
       A - B - C - D - E' (origin/master) - E (master) 
       
       git push
       A - B - C - D - E' - E (origin/master, master)
    

# "Tracking Branches" - Connect your local branch and its remote counter-part (aka "Upstream")

A.k.a
    Upstream branch
Please read
    <https://wiki.lazarus.freepascal.org/FPC_git_concepts#Local_and_Remote_Branches>

## What is tracking

Say you clone a remote git repository. This repository has the following branches 

  * master
  * fixes-2



Those branches are reflected in your local git as ("origin" is the default name for your remote server / this assumes you use this default) 

  * remote/origin/master
  * remote/origin/fixes-2



You can checkout to the commits in those branches. But you can not commit to those branches. As they represent the last known state on the server, the only way to update them, is by updating the server. Either you pushing to the server, or you pulling someone else's commits from the server. 

  


Tracking branch
    

In order to commit, you need (a) local branch(es). 
    
    
       git switch fixes-2
    

  * If you did not yet have this branch, this will create a local fixes-2 (no prefix) branch for you. If newly created the branch will be at the same commit as origin/fixes-2 (short form).
  * If the branch already existed, you may have to fast-forward or rebase it first (see the section on ff/rebase).



Because the local name matched the remote name, **and** because you used "git switch", the local branch will have been set to track the remote. 

  
Now you have may have commits like this 
    
    
       A => B => C (fixes-2, origin/fixes-2)
    

Edit a file in the worktree and commit it, you get 
    
    
       A => B => C (origin/fixes-2) => D (fixes-2)
    

This is described as "Your local branch is one commit ahead". 

You can now push your local branch to the remote server 
    
    
       git push
    

This will add your commit "D" to the server, and update the remote branch 
    
    
       A => B => C => D (origin/fixes-2, fixes-2)
    

  


Non tracking branch
    

You can create a new branch, at the current checked out commit. 
    
    
       git switch -c new-branch-foo
       A => B => C => D (origin/fixes-2, fixes-2, new-branch-foo)
    

This branch does not know about origin/fixes-2. It is not tracking. 

Now again edit a file and commit. You get 
    
    
       A => B => C => D (origin/fixes-2, fixes-2) => E (new-branch-foo)
    

If you now try to do 
    
    
       git push
    

you will get an error. GIT does not know to which branch on the server it should send your commit. 

For a non tracking branch you need to do: 
    
    
       git push origin fixes-2
    

If instead of updating the existing remote/fixes-2 branch, you want to create a new branch on the server the you can do: 
    
    
       git push origin new-branch-foo
    

Here "new-branch-foo" is not the local name, but the name on the server, that will then be reflected an remote/origin/new-branch-foo. 

So you could also do 
    
    
       git push origin b-foo
    

And your local branch new-branch-foo would create remote/origin/b-foo 

## Setting / Un-Setting tracking

In the above example the local branch of the same name as the remote was tracking. And ideally that is how you want it to be. But there is no enforcement. While "git switch" may do this for you when creating a new branch, it is possible to have a local "foo" branch that does not track "origin/foo" 

In order to make a local branch tracking you can give an argument to push. 
    
    
       git push  --set-upstream  origin  remote-branch
       git push  -u              origin  remote-branch
    

When you create a new local branch you can do any of those 
    
    
       git switch --no-track  -c  local-name  origin/remote-name
       git switch --track  -c  local-name  origin/remote-name
       git switch -t       -c  local-name  origin/remote-name
    

Using "git switch" to set up a new local branch, which has the same name as the base-name of the remote, will add tracking by default 
    
    
       git switch fixes-3   # if local fixes-3 does not yet exist, and origin/fixes-3 does exist, then the new fixes-3 will be tracking
       git switch -c fixes-4 origin/fixes-4 # same base-name, tracking will be enabled
    

To view the tracking info for your branches 
    
    
       git branch -vv
    

# Nice to know / for later

## git worktree / having more than one checkout

Just to mention here, if you want different branches/commits checked out in different directory, you do not need a 2nd clone. You can have several working dirs from one clone. See <https://git-scm.com/docs/git-worktree>

## Improving git diff for pascal

Git can add context (function name, etc) to the section headers of a diff. For better results with pascal. 

\- Edit .git/info/attributes 
    
    
       *.pas diff=fpc
       *.pp diff=fpc
       *.lpr diff=fpc
       *.inc diff=fpc
    

Add to your congif (edit the file) 
    
    
       [diff "fpc"]
         wordRegex =  "[a-zA-Z_][a-zA-Z0-9_]*|[0-9.e]+|[%&][0-9]+|(\\$|0[xX])[0-9a-fA-F]+|<>|<=|>=|:=|>>|<<|\\.\\.|\\(\\*|\\*\\)"
         xfuncname = "^([ \\t]*(class[ \\t]+)?(procedure|function|constructor|destructor|operator)[ \\t]+&?[a-zA-Z_][a-zA-Z0-9_]*\\.&?[a-zA-Z_][a-zA-Z0-9_]*.*)$\n^((class[ \\t]+)?procedure|function|constructor|destructor|operator)[ \\t]+&?[a-zA-Z_][a-zA-Z0-9_]*.*)$\n^([ \\t]*(implementation|interface|initialization|finalization)[ \\t]*([/][/].*)?)$\n^([ \\t]*&?[a-zA-Z_][a-zA-Z0-9_]*[ \\t]*=[ \\t]*(class|interface|object|record|type[ \\t]helper)([\\( \\t][^;]*)?([/][/].*)?)$"

---

_Source: [https://wiki.freepascal.org/SVN_to_GIT_Cheatsheet](https://web.archive.org/web/20250219010124/https://wiki.freepascal.org/SVN_to_GIT_Cheatsheet)_
