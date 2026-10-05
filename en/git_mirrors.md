# git mirrors

## Contents

  * 1 Replicated Lazarus mirror
  * 2 About that _upstream_ branch
  * 3 Submitting changes
  * 4 Git mirror for the Free Pascal Compiler
  * 5 RSS feeds of commit activity
    * 5.1 Lazarus repository
    * 5.2 Free Pascal repository
    * 5.3 RSS feed directly from the SVN repository
  * 6 Problems and solutions
    * 6.1 Local changes
  * 7 Other Git repositories mirroring Lazarus
  * 8 Windows and Mac support
  * 9 Getting Help



## Replicated Lazarus mirror

Graeme Geldenhuys has setup a public [Git](<http://git-scm.com/>) mirror of the Lazarus SubVersion (trunk only) repository. The Git mirror is hosted on [GitHub](<http://github.com/>) at the following URL and is sync'ed with Lazarus SubVersion trunk every 15 minutes. 
    
    
    <https://github.com/graemeg/lazarus>
    

The Lazarus Trunk (exact duplicate) is mirrored in the **upstream** branch which I'll explain shortly. 

To clone Lazarus Git repository with full history, run: 
    
    
    $ git clone <git://github.com/graemeg/lazarus.git>
    

...this will download ±57MB of data and unpack to around 144MB on your hard drive. This has the whole history of Lazarus. A checkout via SubVersion of only the trunk revision is over 320MB - more than double what Git is with full history. 

Then move to the created lazarus directory for more action: 
    
    
    $ cd lazarus
    

## About that _upstream_ branch

Because I'm mirroring the Lazarus svn repository ([GitHub](<http://github.com/>) considers it as a fork), I want to make sure I always have a pristine copy of what is in SubVersion. That's what the **origin/upstream** branch is all about. The default **master** branch is my playground and might contain my customized version of Lazarus. 

That means you shouldn't use the local **master** branch which is created automatically when cloning a git repository. Instead you should create a local **upstream** branch to track the **origin/upstream** branch. Run the following command: 
    
    
    $ git checkout -b upstream origin/upstream
    

This creates a new local branch called **upstream** which tracks the original SVN trunk branch and later allows you to create your own branches. That's the one MOST users would want! 

Now to keep up to date with what's happening in SVN trunk, git's **upstream** branch, you simply need to run the following command to update your local repository. 
    
    
    $ git pull
    

When you modify the code to fix a bug or add a feature you must use another new branch, derived from the local upstream branch. Run the following command: 
    
    
    $ git checkout -b my_feature upstream (create and checkout a new branch)
    

This creates a local non-tracking branch called **my_feature**. In practice non-tracking means that git doesn't try to pull and merge directly into such branch from a remote repository. It does pull to a tracking branch (like the **upstream** created earlier). 

## Submitting changes

To submit your changes as a patch, see this page : [Creating A Patch#Creating the patch using Git](<Creating_A_Patch.md> "Creating A Patch")

Git is is distributed system, you can do that also with Lazarus : [Creating A Patch#Using a forked Git repository directly](<Creating_A_Patch.md> "Creating A Patch")

Making your own feature branch and committing changes : 
    
    
    $ git checkout upstream   (checkout the tracking branch)
    $ git pull                (make sure we are up to date with SVN)
    $ git checkout my_feature (checkout the feature branch)
    $ (edit..edit..edit)      (create a bug fix)
    $ git add <filename>      (stage the file for next commit)
    $ git commit -m 'describe what I changed'
    

If you work on a feature over a longer period you may want to move it to the latest upstream (trunk) HEAD. Command **git rebase** does that. Before making your patch, **my_feature** branch being active, run the following command: 
    
    
    $ git rebase upstream
    

It makes the branch look like it was created from the latest upstream version. It rewrites the repository history and makes in linear - the way SubVersion likes it. 

The nice thing about Git's merge is that if you created a patch and it was later included in Lazarus's SVN trunk (assuming it's applied verbatim), it will filter through to **upstream**. Git is supposedly smart enough to realize that it's the same change you have locally and keep things nice and tidy. Else you can force a merge and trash your local changes. 

## Git mirror for the Free Pascal Compiler

I have also setup a Git mirror for the Free Pascal Compiler repository. Due to the FPC repository being slightly less active than the Lazarus repository, I only sync the Git mirror every 30 minutes. Currently the mirror tracks the main development branch called "trunk" (version 2.5.1 at the time of writing). I used to manually track 'fixes_2.4', but don't any more. 

To clone the whole FPC repository including all the history, run the following command. This will download about **50MB** of compressed information and unpack locally on you PC to about **259MB**. The 50MB download is considerably smaller than checking out _trunk_ directly from SubVersion. 
    
    
    $ git clone <git://github.com/graemeg/freepascal.git>
    

...this will create a _freepascal_ directory and automatically checkout the _master_ branch. The _master_ branch tracks the main development branch (SubVersion trunk branch and is at v2.5.1 at the time of writing) of FPC by default. If you want to work on the more stable FPC v2.4.1 you can checkout that 'fixes' branch by running the following command: 
    
    
    $ git branch --track v2.4.x origin/fixes_2.4
    

...that created a new tracking branch, called _v2.4.x_ and tracks the remote _fixes_2.4_ from SubVersion. You can now switch to that branch by doing the following. 
    
    
    $ git checkout v2.4.x
    

Note that this will **not** download any new content. Your git repository already has all the branches locally. Switching between branches only takes a second or two. 

To clone only partial history of the Free Pascal Compiler repository, which results in downloading even less, you can do the following. Lets say we want only the history since the last 5 commits of every branch. 
    
    
    $ git clone --depth 5 <git://github.com/graemeg/freepascal.git>
    

...this will download around **35.5MB** of data and unpack to around **191MB** on your local hard drive. You will only have the last 5 commits as your history for each branch. The history will continue to grow from there onwards as you pull in new updates. 

Keeping your local repository up to date you do the following: 
    
    
    $ git checkout master
    $ git pull
    

## RSS feeds of commit activity

GitHub has built-in support for RSS feeds. 

### Lazarus repository

To view/browse the commit logs with your web browser, follow this link. 
    
    
     <https://github.com/graemeg/lazarus/commits/upstream>
    

To get an RSS feed for Lazarus trunk, use this link. 
    
    
     <https://github.com/graemeg/lazarus/commits/upstream.atom>
    

### Free Pascal repository

To view/browse the commit logs with your web browser, follow this link. 
    
    
     <https://github.com/graemeg/freepascal/commits/master>
    

To get an RSS feed for FPC trunk, use this link. 
    
    
     <https://github.com/graemeg/freepascal/commits/master.atom>
    

### RSS feed directly from the SVN repository

If you want to avoid the 15 minute lag because of the mirroring to GitHub, you can use the RSS feeds from the SVN repository. 

  * For Lazarus: <http://svn.freepascal.org/feeds/lazarussvn.rss>
  * For Free Pascal: <http://svn.freepascal.org/feeds/fpcsvn.rss>



## Problems and solutions

### Local changes

If you have modified some files locally, then do a `git pull`, you might see something like: 
    
    
    Updating 15f54eb..559abba
    error: Your local changes to the following files would be overwritten by merge:
            packages/fcl-db/src/export/fpdbexport.pp
            packages/fcl-db/src/export/fpdbfexport.pp
    Please, commit your changes or stash them before you can merge.
    Aborting
    

This means your local copies interfere with upstream updates. 

To know what the local changes actually are: 
    
    
     git diff
    

or 
    
    
     git gui
    

You can either: 

  * save them using Git (and if needed, apply them later on the new commit from upstream)
  * delete the local changes before a git pull



If you want to get rid of your local changes, you can do: 
    
    
     git reset --hard
     git pull
    

From the help: 
    
    
    Resets the index and working tree. Any changes to tracked files in the working tree since <commit> are discarded. 
    

If you temporarily want to put your local changes aside, just to get an upstream update, then do the following. 
    
    
     git stash             (1)
     git pull              (2)
     git stash pop         (3)
    

1) Local changes are stored in a temporary branch for later access  
2) Git will now update your local repository with the latest changes  
3) Git will now restore your local changes, on top of the latest code, from that temporary branch it created earlier. 

For more details on the `git stash` command, please run 
    
    
     git help stash
    

## Other Git repositories mirroring Lazarus

There is: 
    
    
     <https://github.com/alrieckert/lazarus>
    

You can also connect to a SubVersion repository directly with git-svn, see [git svn link](<Lazarus_git-svn.md> "Lazarus git-svn"). 

## Windows and Mac support

Git was originally designed just for Linux and the Linux Kernel project (this was back in 2005/6). Since Git gained popularity, things have changed for the better. There is now a native Windows port of Git called [msysGit](<http://msysgit.github.com/>) available on SourceForge. 

I've tested it and it works perfectly on Windows 2000 and Windows Vista systems. I also can't see any performance difference compared to the Linux version of Git. They both work the same for me, so you don't need the old Cygwin version any more. 

As far as I have heard, the TortoiseGit project also has 95% of the features that TortoiseSVN has, so that should be pretty usable as well. I don't use such GUI tools for any version control systems, so can't really comment further on them. 

There is also a new project called [Git#](<http://www.eqqon.com/index.php/GitSharp>) (GitSharp) which is implemented in C# and for the .NET platform. They have made very impressive progress, but msysGit still has more Git features implemented. 

There is also a native Mac OS version of Git, but I have no idea how well it works (I don't own a Mac). But seeing that the underlying OS of a Mac is much closer to Linux than Windows is, I would imagine the command line client works pretty well. I have no idea what is available in the GUI client arena. 

## Getting Help

For more info on any git commands simply type: 
    
    
    $ git help <command>
    

Example: 
    
    
    $ git help commit
    $ git help add
    $ git help format-patch
    $ git help rebase
    

Or go to the Git website where there are plenty of tutorials, downloadable PDF books etc.. The Git homepage is: <http://git-scm.com/>

---

_Source: [https://wiki.freepascal.org/git_mirrors](https://web.archive.org/web/20190722161428/https://wiki.freepascal.org/git_mirrors)_
