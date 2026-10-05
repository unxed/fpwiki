# FPC git

│ **English (en)** │  **[русский (ru)](<../ru/FPC_git.md>)** │

## Contents

  * 1 Git setup
    * 1.1 Git repository location
    * 1.2 Your identity in git
    * 1.3 HTTP access and credentials
    * 1.4 Using access tokens
    * 1.5 Git mirror on GitHub
  * 2 Branching model
  * 3 Windows Git clients
  * 4 Concepts and SVN to GIT differences
  * 5 Common SVN operations in Git
    * 5.1 Check out
    * 5.2 Update
    * 5.3 Resolve
    * 5.4 add (adding new files)
    * 5.5 rm (removing files)
    * 5.6 mv (renaming files)
    * 5.7 commit
    * 5.8 diff (show changes to working copy)
    * 5.9 diff (show changes performed in a commit)
    * 5.10 log (show commits in a branch)
    * 5.11 ls (listing branches)
    * 5.12 copy (creating a branch)
    * 5.13 switch (checking out a branch)
    * 5.14 revert
      * 5.14.1 Revert the entire working tree
      * 5.14.2 Revert changes to specific files/directories
    * 5.15 merge (merging the changes in 2 branches)
    * 5.16 blame (check who made modifications)
    * 5.17 status (check status of files)
    * 5.18 shelve (temporarily undo changes)
    * 5.19 info (get working dir and server info)
    * 5.20 bisect'ing
    * 5.21 file properties (EOL handling etc.)
    * 5.22 cherry picking
    * 5.23 Managing the FPC fixes branch
      * 5.23.1 Mass cherry picking
      * 5.23.2 Submitting a request to merge/cherry-pick a certain commit to fixes
  * 6 Submitting merge requests



## Git setup

### Git repository location

The Git repositories are hosted on Gitlab: 

FPC compiler, RTL, packages and utils are located here: 

    <https://gitlab.com/freepascal.org/fpc/source>

The URL to clone the repository is: 

    <https://gitlab.com/freepascal.org/fpc/source.git>
    
    
    git clone https://gitlab.com/freepascal.org/fpc/source.git
    

The Documentation Git repository is here: 

    <https://gitlab.com/freepascal.org/fpc/documentation>

The URL to clone the documentation repository is: 

    <https://gitlab.com/freepascal.org/fpc/documentation.git>
    
    
    git clone https://gitlab.com/freepascal.org/fpc/documentation.git
    

The website Git repository is here: 

    <https://gitlab.com/freepascal.org/fpc/website>

The URL to clone the website repository is: 

    <https://gitlab.com/freepascal.org/fpc/website.git>
    
    
    git clone https://gitlab.com/freepascal.org/fpc/website.git
    

### Your identity in git

In order for your commits to be recognized, please configure your local (or better yet, global) git installation to use the address linked to your gitlab acccount. 
    
    
    git config --global user.email your.email.user@youremail.host
    

This will set your email address for all git repositories you use, and will allow the gitlab interface to correctly link commits to users in its HTML interface. 

If you have multiple gitlab (or even github) accounts, you may prefer to set this email only locally: 
    
    
    git config --local user.email your.email.user@youremail.host
    

this command must be execute in a particular repository, and sets the email address for only that repository. 

### HTTP access and credentials

If you access the repository using HTTPS, by default, git will ask for the username/password. you can tell git to store them permanently, issue the following command: 
    
    
    git config --global credential.helper store
    

There are other possibilities for storing this info. More info can be found on 

    <https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage>

### Using access tokens

You may want to use an access token and HTTPS access, it has some advantages. This is supported by most modern git clients: 

    <https://knasmueller.net/gitlab-authenticate-using-access-token>

and here you can see how to do this on gitlab: 

    <https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html>

The advantage of using an access token is that it can be given limited rights on gitlab, for example only accessing a repository. This also means you can easily enable 2FA for your gitlab account. 

### Git mirror on GitHub

The GitLab repositories are mirrored on [GitHub](<https://github.com/fpc/>). Cloning of the sources can be done with the following command 
    
    
    git clone https://github.com/fpc/FPCSource.git fpc
    

and for documentation: 
    
    
    git clone https://github.com/fpc/FPCDocumentation.git fpcdocs
    

In case you want to submit patches (pull requests) it is _strongly recommended_ to use GitLab instead. 

## Branching model

In a first stage, the branching model as used in Subversion will be used: fixes done in trunk, and later cherry picked to the fixes branch. 

At a later stage, the branching model may be changed into one of the many git branching schemes. 

## Windows Git clients

On macOS, Linux & BSD, the git command is installed with the system package manager (you must install XCode command line tools on macOS). On Windows, a separate git client must be installed. There are many available, but the following are popular ones: 

  * **Git** for windows: a command-line client with a bash shell, so you can copy & paste commands you find on internet:



    <https://gitforwindows.org/>

  * **TortoiseGit** integrates with the Windows explorer, much like TortoiseSVN does:



    <https://tortoisegit.org/>
    if you already have TortoisSVN installed, both can work side-by-side.

  * **Sourcetree** is a free client for Windows and macOS:



    <https://www.sourcetreeapp.com/>
    It is made by the people from bitbucket.

  * **Smartgit** runs on Windows, Linux and macOS:



    <https://www.syntevo.com/smartgit/>

  * The VSCode and Atom editors have git support built in.



## Concepts and SVN to GIT differences

A few "good to know" differences: [FPC git concepts](<FPC_git_concepts.md> "FPC git concepts"). 

## Common SVN operations in Git

### Check out

To check out a new copy of a repository is called 'cloning' in git. 
    
    
    git clone https://gitlab.com/freepascal.org/testconversion
    

As in subversion, you can give it another name: 
    
    
    git clone https://gitlab.com/freepascal.org/testconversion myconversion
    

This operation can take a while, the FPC source base is large. 

### Update

To update your source code with the changes in the remote repository, is called 'pulling' in git: 
    
    
    git pull
    

Note that in difference with subversion, git always updates the whole repository. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** While git will fetch and apply changes to all _remote_ branches with this command, of all _local_ branches it will only update the one that's currently checked out (see [ls (listing branches)](<FPC_git.md> "FPC git") for more information about remote/local branches). To update other local branches, you have to [switch](<FPC_git.md> "FPC git") to them and either execute `git pull` again, or rebase/merge (will be documented once we decide which strategy to use).

To keep a commit history which is as linear as possible and avoids merge commits, it is recommended to do a rebase when you pull. 
    
    
    git pull --rebase
    

You can make the use of rebase for every pull the default by executing: 
    
    
    git config pull.rebase true
    

### Resolve

If you have conflicts in svn after updating or merging, you fix the conflicts and then tell svn everything is fine with the _resolved_ command. 

In git, conflicts generally occur when [merging](<FPC_git.md> "FPC git") or when applying [shelved/stashed](<FPC_git.md> "FPC git") changes. 

When conflicts occur, then 

  1. Changes to files that are not in conflict, will be [staged](<FPC_git.md> "FPC git")
  2. Changes to files that do conflict will be applied without staging to the working copy (so that a plain [diff](<FPC_git.md> "FPC git") will only show conflicting changes)



To resolve conflicts, 

  1. If you want to commit the resolved version of the file, mark it as resolved by [staging/adding](<FPC_git.md> "FPC git") it as well
  2. If you want to just get rid of the "both modified" status (= conflicted) without adding the file, use `git reset file`



Once all files have been staged or marked as not conflicted, you can commit (only staged files will be committed). 

### add (adding new files)

Subversion uses the _add_ command to add a file to the versioning system. This is no different in git: If you want to add a file, you must add it first, like this: 
    
    
    git add newfile.pas
    

You must then (just as in subversion) still commit this newly added file. 

### rm (removing files)

Subversion uses the _rm_ command to remove a file from the versioning system. This is no different in git: 
    
    
    git rm nolongerneededfile.pas
    

You must then (just as in subversion) still commit this removed file. 

### mv (renaming files)

Subversion uses the _mv_ command to rename a file in the versioning system. This is no different in git: 
    
    
    git mv oldfile.pas newfile.pas
    

You must then (just as in subversion) still commit this change. 

As in subversion, you can move one or more files to another directory: 
    
    
    git mv file1.pas file2.pas somesubdir
    

### commit

If you have made some changes locally and wish to commit them, this is a 2-step process: 
    
    
    git add myfile.pas
    

This schedules the file to be committed (called _staging_ in git, so the changes are now _staged_). You can add as many files as you want like this. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** An easy way to remember this is that svn requires the same for newly added files. Git extends this to all changes, because that way you not only can selectively schedule individual new files for the next commit, but also changes to existing files. The opposite of `git add` is `git reset` (**without** the `--hard` parameter!)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** To see the changes that have been staged/scheduled for commit, use `git diff --cached`. Plain `git diff` will show local changes that are not (yet) staged.

When you are ready to commit what you have staged, you can commit: 
    
    
    git commit -m '* Some nice comment telling what you did'
    

In fact, if you're in a hurry and know you want to commit a file, you can combine the 2 commands; 
    
    
    git commit -m '* Some nice comment telling what you did' myfile.pas
    

If you are really in a hurry, you can commit all changed files in one fell swoop: 
    
    
    git commit -m '* Some nice comment telling what you did' -a
    

But it is not really recommended to use this, as it will commit _all_ changes in your local copy, not just the ones in the current working directory. 

At this point, your changes have been saved locally, but have not yet been sent to the remote repository. To do that, you must additionally send the changes to the remote repository. This is called pushing: 
    
    
    git push
    

That will send all locally committed changes to the server. Obviously only changes that have not yet been sent previously are sent. 

### diff (show changes to working copy)

In svn, you can see the changes that have been made but have not yet been committed with _diff_. This is also correct in git: 
    
    
    git diff
    

This will show all changes in your working copy _**that are not yet staged**_ (scheduled for committing). 

You can view this for a single file as well: 
    
    
    git diff myfile.pas
    

This will show unstaged changes made to your working copy of **myfile.pas**. 

If you have already staged one or more files (i.e. marked for inclusion in the next commit), the above will not show them in the generated diff. If you wish to see the staged changes (and _only_ the staged changed), use the _\--cached_ command-line option: 
    
    
    git diff --cached
    

### diff (show changes performed in a commit)

In svn, you can see the changes that a commit did through `svn diff -c <revision>`. In git, you use the _show_ command instead: 
    
    
    git show -p <commit_hash_or_branch_or_tag>
    

If you omit the commit hash, the last commit to the current branch will be shown instead. 

### log (show commits in a branch)

In svn, you can see the commits in a branch with `svn log`. The same works in git: 
    
    
    git log
    

This will list the commit hashes (the string that git uses to identify commits, similar to revision numbers in svn) followed by the log messages. If you also want to see which files were changed, use 
    
    
    git log --stat
    

### ls (listing branches)

To get a list of branches in Subversion, you use the 'ls' command with a server URL. By convention, all directories under '/branches' are the names of branches. Branches always exist both on the client(s) and server. 

In git, branches are formalised as part of the version control system: commands deal specifically with branches rather than with paths that are treated as branches. Additionally, git differentiates between **remote branches** (branches that exist in the repository that you cloned, i.e., in this case on the gitlab server), and **local branches** (branches that you created on your machine after cloning). The repository that you cloned is referenced using the alias "origin" by default, and the names of remote branches are prepended by this alias. 

The list of local branches can be obtained using: 
    
    
    git branch 
    

By default, the only local branch you will have is `main`, which corresponds to svn trunk. This local branch is automatically created when performing the initial clone operation. 

The list of remote branches can be obtained using: 
    
    
    git branch -r
    

By default, the remote branches are the branches that exist in the repository on the gitlab server, whose alias is "origin". The local `main` branch automatically _tracks_ (~ is linked to) the remote `origin/main` branch. Because of this tracking, executing `git pull` resp. `git push` commands while you have your local `main` branch checked out will synchronise the two from resp. to gitlab. See [switch](<FPC_git.md> "FPC git") on how to create more local branches and how to link them to remote branches. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** While git always downloads all branches and their commits by default when cloning or pulling and while you can use their content without a network connection, you cannot directly work on remote branches (since they're remote and not local). Instead, you have to create a local branch that "tracks" this remote branch.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Do not directly checkout a remote branch. That will put you in a so-called "detached HEAD" state: your working copy will contain the contents from that branch, but you can't commit anything as you can only commit to local branches. You can get out of a "detached HEAD" state by simply checking out another branch.

### copy (creating a branch)

To create a branch from the current working directory situation, you can create a branch like this: 
    
    
    git branch mybugfix
    

this will create a branch mybugfix from the currently checked out branch at the current commit. But it does not yet make this the active branch. 

You need to check out the newly made branch first: 
    
    
    git checkout mybugfix
    

The two operations can also be combined with one command: 
    
    
    git checkout -b mybugfix
    

### switch (checking out a branch)

To switch to an existing _local_ branch **mybugfix** , you can use the checkout command: 
    
    
    git checkout mybugfix
    

this will check out branch mybugfix. If there are any uncommitted changes which would cause a conflict, git will refuse the checkout. 

  


To switch to a _remote_ branch `mybugfix2`, which does not exist yet locally, you can also use the checkout command: 
    
    
    git checkout mybugfix2
    

this will check out branch mybugfix2 and automatically set it up to track the remote branch. Here again, if there are any uncommitted changes which would cause a conflict, git will refuse the checkout. 

Note that if the branch does not exist remotely, this will simply create a new branch locally. It is better to be explicit: 
    
    
    git checkout -b mybugfix2 --track origin/mybugfix2
    

If you created a branch locally which does not yet exist on the server, made some modifications, and you wish to push this branch to the server, you must tell this to git when pushing: 
    
    
    git push -u origin/mybugfix mybugfix
    

the **-u origin/mybugfix** tells git all it needs to know to connect the local with the remote branch. 

### revert

#### Revert the entire working tree

If you have made some changes locally and wish to undo them, you can do a _hard reset_ in git: 
    
    
    git reset --hard
    

This will undo any changes you made in the entire checkout, including any [staged changes](<FPC_git.md> "FPC git"). 

#### Revert changes to specific files/directories

You cannot undo changes to individual files with `git reset --hard`. Instead, use 
    
    
    git checkout <pathspec>
    

`pathspec` can be any combination of files and directories. Note that if the files had [staged changes](<FPC_git.md> "FPC git"), this command will restore the staged version rather than the last committed version. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** At first sight, the git `checkout` command replaces an arbitrary collection of svn commands. The reason is that its operation is defined as "update the files in my working tree to match the specified staged/committed version of the files". So switching to a branch corresponds to getting the version of the files from that branch, while reverting corresponds to the latest version of the files from the current branch.

### merge (merging the changes in 2 branches)

To merge changes in 2 branches, you use the _merge_ command. If we want to merge the changes in _mybugfix_ to _fixes_ then we do: 
    
    
    git checkout fixes
    git merge mybugfix
    

### blame (check who made modifications)

To see who changes what line (and in what commit) svn offers you the 'blame' command. The same command exists in git. 
    
    
    git blame yourfile.pas
    

### status (check status of files)

To see whether there are any changed files in your working repository, svn offers the 'status' command. The same command exists in git. 
    
    
    git status 
    

will present you with all changed, staged and new files in the repository. 

If you want only the status of files below a certain directory, you can specify the name of the directory. So to get the changes in the current working directory (or below) that would be: 
    
    
    git status .
    

will present you with all changed, staged and new files in the current directory. This is the default behaviour of svn. 

### shelve (temporarily undo changes)

Subversion has an (experimental) feature that allows to temporarily set aside changes to your working copy : shelve. This feature exists since a long time in git and is called stash: 
    
    
    git stash 
    

will set aside any changes to the working copy, and restore the working copy to the state it would be in if you did a 'git pull' (or svn update). 

To reapply the changes you set aside to the working copy, and to remove them from the list of stashes at the same time, you can execute the following command: 
    
    
    git stash pop
    

If any conflicts occur, you can 

  * resolve them, or
  * use [git reset --hard](<FPC_git.md> "FPC git") to revert all files again



In either case, the stash will not be touched when conflicts occurred while applying it. If you still with to remove it, you can do so with 
    
    
    git stash drop
    

### info (get working dir and server info)

In subversion, you can get information about the current working directory and remote server configuration using _svn info_. Since git is a distributed system, no direct equivalent of the info command exists: there is no single server, and the revision number as known in subversion does not exist. 

The following script attempts to display similar information as the _svn info_ command: 
    
    
    #!/bin/bash
    
    # author: Duane Johnson
    # email: duane.johnson@gmail.com
    # date: 2008 Jun 12
    # license: MIT
    # 
    # Based on discussion at http://kerneltrap.org/mailarchive/git/2007/11/12/406496
    
    pushd . >/dev/null
    
    # Find base of git directory
    while [ ! -d .git ] && [ ! `pwd` = "/" ]; do cd ..; done
    
    # Show various information about this git directory
    if [ -d .git ]; then
      echo "== Remote URL: `git remote -v`"
    
      echo "== Remote Branches: "
      git branch -r
      echo
    
      echo "== Local Branches:"
      git branch
      echo
    
      echo "== Configuration (.git/config)"
      cat .git/config
      echo
    
      echo "== Most Recent Commit"
      git --no-pager log  -n1
      echo
    
      echo "Type 'git log' for more commits, or 'git show' for full commit details."
    else
      echo "Not a git repository."
    fi
    
    popd >/dev/null
    

Note that this script will also work in the Windows environment if you have the windows git client installed, because it comes with a minimal unix environment. 

### bisect'ing

When it is known that for example "make cycle" worked for a certain revision but it is broken with the current head of main, one can do a manual "binary search" to find the broken revision. With git, this can be done fully automatically using the git command `bisect`: 
    
    
    # change into a directory with a fpc git repository
    git bisect start       # tell git that we start bisecting
    git bisect bad         # mark the currently checkout commit as bad
    git checkout 12345678  # checkout a commit you know that it is good
    git bisect good        # tell git that at this commit everything is good
    

During this command, git checks out the commit it wants to know if it is good or bad. In case of a broken "make cycle" one can run this now and tell git by "git bisect good" or "git bisect bad" what the status of the commit is. Then git will automatically checkout the next commit to test. After several tests, git will tell which commit that one is which broke "make cycle". As this is a tedious task, the search can be also automated (Unix shell): 
    
    
    git bisect run sh -c 'cd $PWD/compiler && make cycle'
    

git runs the passed command and depending on the result value, the commit will be marked as good (0) or bad (1 to 127, 255 tells git to abort search). git continues to run the command till it finds the commit which breaks things. 

### file properties (EOL handling etc.)

Subversion allows you to set some properties on files. There are 'reserved' properties that svn itself uses, for example to handle EOL handling and file type. Because git operates on diffs and not on files, no similar concept exists. 

However, git does allow you to set global or local attributes for classes of files. These are in the _.gitattributes_ file. That file should be located in the root of your repository, and can contain some attributes. For example: 
    
    
    *               text=auto
    *.txt		text
    *.vcproj	text eol=crlf
    *.sh		text eol=lf
    *.jpg		-text
    

This is a regular file which you can (and must) add with _git add_ so it is saved for all users. 

### cherry picking

To copy one commit from branch to another, cherry picking can be used, checkout the branch where the commit shall be added and execute: 
    
    
    git cherry-pick -x <hash of commit>
    

If this operation causes a conflict, fix it and continue the operation with: 
    
    
    git cherry-pick --continue
    

To track which commits are cherry picked, make sure to always use the option `-x`. This appends a small note to the commit message containing the hash of the cherry-picked commit. 

In case you want to abort the cherry picking e.g. because of too many conflicts, execute 
    
    
    git cherry-pick --abort
    

### Managing the FPC fixes branch

So far, FPC uses a developement model which is sometimes called the "ghetto development model". Patches battle out in the main branch if they are good and if they are good, they are ported to the fixes branch from which releases are created. This development model is not naturally supported by git but can be implemented based on cherry-picking. To make tracking of cherry picks easier, `git cherry-pick` must be called with `-x`. Furthermore, only one commit at once may be cherry-pick. 

Example: 
    
    
    # change to the top-level of a clone of the FPC Source repository
    
    git checkout fixes_3_2       # switch to/checkout out the fixes branches, if you did not do so before
    git pull                     # ensure fixes_3_2 is up-to-date
    git cherry-pick -x 12345678  # 12345678 is the hash (can be a short or full hash) of the commit to be cherry picked to fixes
    git push                     # push the change to the remote repository
    

For conflict resolution, see section about cherry picking. 

To make things a little bit more easy, to get more information in case of conflits and avoid to forget `-x`, use a script `gcp` like: 
    
    
    #!/usr/bin/env bash
    git show --name-only $1
    git cherry-pick -x $1 || exit 255
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Always cherry pick to fixes with -x for tracking and cherry pick only one commit at once.

To figure out which hashes are eliglibe (i.e. not yet cherry picked) for cherry picking, use a script like <https://gitlab.com/freepascal.org/fpc/merging/-/blob/main/findeligible> This script is comparable complicated as it takes also merges from svn times into account. A regularily updated list of eliglible commits can be found at: [https://gitlab.com/freepascal.org/fpc/merging/-/blob/main/eligible.log?expanded=true&viewer=simple](<https://gitlab.com/freepascal.org/fpc/merging/-/blob/main/eligible.log?expanded=true&viewer=simple>)

#### Mass cherry picking

If you have a list of commits identified by their hashes in a file, they can be cherry picked using the script `gcp` from above like: 
    
    
    xargs -n 1 gcp < filewithhashes
    

Using the script from above is important because xargs only stops if the called program returns 255. So in case of a cherry pick conflict, the command aborts, one can fix the conflict, remove the already cherry picked commits from `filewithwith` and continue. 

A file with svn revisions can be converted easily to a list of hashes by something like: 
    
    
    cat filewithrevs | xargs -n 1 git log --grep='trunk@{}' --format='%H' > filewithhashes
    

#### Submitting a request to merge/cherry-pick a certain commit to fixes

Using Gitlab allows to have a defined procedure to merge or actually cherry-pick a certain commit from main(/trunk) to fixes. 

  1. Login to Gitlab
  2. Open the commit in Gitlab
  3. Click the Options button at the upper right corner and select Cherry-pick
  4. Choose the latest fixes branch (fixes_3_2 at the time of writing) in the "Pick into branch" field
  5. Click Cherry-pick, if the commit cannot be cherry-picked due to textual conflicts, you get an error in this step. Possible solutions are: 
     1. Find the commit which needs to be cherry-picked before the current one to get proper textual merging and submit this cherry-pick request first
     2. Cherry-pick the commit in a local clone of the respository and submit a merge request against this newly created commit.
  6. Do no change anything on the next page, just click Create merge request



## Submitting merge requests

A big advantage of a distributed version control system is that you can have your own private repository. So you can fully version your work without any access to the pristine repository and when you are ready, you can submit a so-called merge request (MR) to get your work into the official repository. 

The workflow is as follows: 

  1. Login to GitLab
  2. Go to the repository of the project you want to work on, e.g. FPC Source: <https://gitlab.com/freepascal.org/fpc/source>
  3. Click Fork at the upper right corner and choose the settings as you like in the following dialog, then click Fork at the bottom. GitLab creates now a full copy of the chosen repository.
  4. Clone the newly create repository to your machine by getting the URL from the Clone button at the upper right corner of the respository's page on GitLab (e.g. `git clone <https://gitlab.com/YourUsername/fpcsource.git>`).
  5. Create a new branch for your work: `git checkout -b myfeature`
  6. Now do your work: change, commit, change, etc.
  7. When you are ready, push your branch with `git push origin` into your private repository on GitLab. At this point GitLab shows you already a link how to submit the changes as a merge request to the official/pristine repository. Click the link and follow the instructions.
  8. If you want to automatically keep your fork up to date with the pristine repository, go to Settings -> Repository -> Mirroring repositories and fill in the required fields as described in the [GitLab docs](<https://docs.gitlab.com/ee/user/project/repository/mirror/index.html>).



Of course, it is possible work with more than one remote repository in a local one. I.e. push/pull changes from and to your own private one and pull from the official repository. This would go to far for this wiki, have a look at <https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes> instead.

---

_Source: [https://wiki.freepascal.org/FPC_git](https://web.archive.org/web/20250401000502/https://wiki.freepascal.org/FPC_git)_
