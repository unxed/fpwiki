# Lazarus git-svn

## Contents

  * 1 Using git to work with Lazarus SVN repository
  * 2 Cloning the repository
  * 3 Workflow
  * 4 Issues



## Using git to work with Lazarus SVN repository

This method of using git makes the most sense when you have write access to Lazarus SVN repository and want to commit your changes directly there. Otherwise using a [Git mirror](<git_mirrors.md> "git mirrors") may be a better option. 

## Cloning the repository

Move to home directory (or other directory of your choice). 
    
    
    cd
    

Clone a repo (like "git clone" for a real git repository): 
    
    
    git svn clone <https://svn.freepascal.org/svn/lazarus/trunk> lazarus_gitsvn --revision 50000:HEAD
    

The command fetches all revisions since revision #41400. If you want more history or less history, change the number. Usually you don't need many revisions when changing and committing code. If you really need all revisions, run the command without --revision parameter. Be warned as it takes a long time (many days). However, once the repository is cloned it will be compressed efficiently and doesn't take much disk space, even with full revision history. 

[ Note: you can also fetch all SVN branches like this: 
    
    
    git svn clone <https://svn.freepascal.org/svn/lazarus> lazarus_all --stdlayout --revision 50000:HEAD
    

Usage of those branches is not covered here. Please see git manual for details. ] 

Enter the newly cloned local directory: 
    
    
    cd lazarus_gitsvn
    

You should be on master branch, double-check with "git branch". 
    
    
    git branch
    

You may also want to append svn:ignore settings to the default git exclude file. See git svn help for details. Note that the following command takes a long time to run. 
    
    
    git svn show-ignore >> .git/info/exclude
    

## Workflow

Work with the local git repository just like with any git repository. Make local commits, combine them, reorder them. Make local branches and later merge them to master. 

There may be new commits in SVN. You must rebase your local changes against the latest changes in SVN before committing to server: 
    
    
    git svn rebase
    

Now commit your changes (that were earlier committed locally using git) to SVN, and also automatically update your working HEAD: 
    
    
    git svn dcommit
    

Note : Git reads the Subversion settings directly from its configuration file (~/subversion/config), thus [auto-props] section should list all needed file suffices, and "enable-auto-props = yes" should be set. Otherwise the commit may fail. 

## Issues

Rebase and dcommit together take longer than git push or git pull in a pure git repository, but usually it is not a problem.

---

_Source: [https://wiki.freepascal.org/Lazarus_git-svn](https://web.archive.org/web/20190722161321/https://wiki.freepascal.org/Lazarus_git-svn)_
