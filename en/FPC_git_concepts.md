# FPC git concepts

## Contents

  * 1 Differences between SVN and GIT
  * 2 Local and Remote Branches
    * 2.1 Naming of your local branches - Potential pitfalls
  * 3 The INDEX (and what it means for “git add”)
  * 4 Conflict resolution when pushing to server



## Differences between SVN and GIT

This page describes some of the GIT concepts and differences to SVN. 

Subversion vs Git: [YouTube video, a brief overview of how git differs from subversion.](<https://m.youtube.com/watch?v=Bb0_lO8N2yU>)

Information on using git can be found at either: 

  * [FPC git](<FPC_git.md> "FPC git")
  * [SVN to GIT Cheatsheet](<SVN_to_GIT_Cheatsheet.md> "SVN to GIT Cheatsheet")



The latter contains a section on why there are 2 pages, and how they differ. 

## Local and Remote Branches

Remote Branches
    reflect the branches on the server(s) at the time of the last synchronization (pull/push). They will only be updated when a synchronization takes place.  
Remote branches have names that contain their server: “remotes/gitlab/master”.
Local Branches
    reflect your current work. They hold all the changes not yet uploaded. When you push, they will be added to the branch on the server, and the remote branch will be updated to reflect that.  
The local branches would just be called “master”.

Local Branches can either be: 

  * "tracking" a remote branch. They represent your work on the published branch, and your work will be pushed into the remote branch.
  * "non tracking", without a remote branch. Either for private work, or to create a new remote branch, or to later assign an existing remote branch.



### Naming of your local branches - Potential pitfalls

It is good practice to keep the names of local branches equal to the name of their remote branch. However, **this is not enforced**. 

Local branches can be created with any name you choose. It is possible to create a local branch "master" to track "remote/fixes_3_2". 

This can happen by accident. Local branches can also be renamed or “moved”. The branch name is a marker for the content of a given branch. And this marker can be set from one branch to another branch. For example "git reset" can be used to move a branch like that. 

If in doubt use “git status” or “git branch --all --contain HEAD” to check your current local branch, and the remote branch to which it belongs. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** A branch is a marker/pointer to a commit.   
From that commit you can then walk backwards using each commits links to its parent(s).  
Branches do not need to have diverged from each other. One branch can simply be at an earlier commit of another branch. (Local Branches often build this way on their remote branches)

  


## The INDEX (and what it means for “git add”)

The “index” is a pre-commit area, in which changes for the next commit can be collected. Using the index is optional. Users coming from SVN may prefer not to do so. 

svn add
    adds a new file by its name to the repository. When the file is committed, the file will be added as it is at the time of the commit.
git add
    adds a new or existing file to the “index”. That is, it adds the current state (content) to the “index”. A later commit (depending on the command line arguments to "git commit") will add the content as it was at the time of "git add". **Any changes made between "git add" and "git commit" may not get committed**.

git add -N
    can be used to just add the filename as in SVN.

## Conflict resolution when pushing to server

SVN
    allows you to “svn commit” (send changes to server), even if you did not “svn update” to the latest commit.  
If the svn server has commits that you do not have, but none of that commit affects the files, that you are sending, then the server accepts your data, and moves your commit behind the existing other commits.  
This could mean, that you missed changes affecting your commit, and that the overall result is broken (not compile-able).
Git
    expects you do first get all existing commits, and move your commit (see rebase or merge) to the end. Of course git can not force you to check that your commit is still good and compiles (hopefully you do anyway).

Mimicking the svn workflow. You can 
    
    
       git pull --rebase
       git commit -m "message" file1 file2 …
       git push
    

In most cases, when you have local uncommitted changes, "git pull" will be able to put them on top of the newly pulled changes from the server. Doing commit directly followed by a push means you are likely to be still up-to-date when you push. In case there was an update between your pull and your push, repeat the "git pull --rebase" and "git push". 

If there are conflicts between your changes and those from the server, like in SVN you must resolve them. See the [FPC git](<FPC_git.md> "FPC git") page.

---

_Source: [https://wiki.freepascal.org/FPC_git_concepts](https://web.archive.org/web/20230324094336/https://wiki.freepascal.org/FPC_git_concepts)_
