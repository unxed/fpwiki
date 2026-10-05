# Lazarus Team - Git Workflows

This page contains workflows for Lazarus team members, when working with GIT. 

## Contents

  * 1 git pull --rebase
  * 2 Merge Requests / Pull Requests
  * 3 Cherry picking / Merging
  * 4 ".gitattributes" - Adding new file extensions
  * 5 Issue Tracker
    * 5.1 Milestone
  * 6 Continuous integration (CI) System



# git pull --rebase

Commits made by team members should have a linear history
    

If you have local commits to push, and the remote branch has new commits too, then use **rebase**. Do not create any merge commits. 
    
    
       git pull --rebase
    

or 
    
    
       git rebase
    

* * *

See also [SVN_to_GIT_Cheatsheet#Recommended](<SVN_to_GIT_Cheatsheet.md> "SVN to GIT Cheatsheet")

# Merge Requests / Pull Requests

Merge requests should be included by creating a merge commit 
    
    
       git merge merge-request/foo
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** To pull merger request to your local git: <https://docs.gitlab.com/ee/user/project/merge_requests/reviews/#checkout-locally-by-modifying-gitconfig-for-a-given-repository>

  


* * *

Merge-requests can be (optionally / recommended, if the fork-point is way back) rebased first. Within the gitlab merge-request website, this can be done adding a comment 
    
    
       /rebase
    

# Cherry picking / Merging

Do not "squash" commits when cherry picking. 

If commits are squashed, then ["git cherry"](<https://git-scm.com/docs/git-cherry>) wont work. 

Use 
    
    
       git cherry pick -x
    

The "-x" adds "cherry picked from commit" 

# ".gitattributes" - Adding new file extensions

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Adding new extensions can have unwanted side effects on existing file. 

If you add files with an new extension (not yet covered by .gitattributes) then it is best to add that extension (or the concrete file) 

Files not covered by .gitattributes will be left to git figuring out on its own, if they are text. Since this affects line endings, it is best to tell git, what we want the file to be. 

**_After editing .gitattributes you must run (before committing):_**
    
    
       git add --renormalize .
    

Then check **git status** which files were affected. 

# Issue Tracker

## Milestone

When closing an issue, ensure to set the correct milestone. 

If the issues is (or will be) merged to a fixes branch, set the milestone for the fixes release. (If the merge later fails, the milestone must be amended). 

If you do not merge (cherry pick) yourself: 

  * leave the issue open
  * Set the fixes milestone
  * add the label



    

    merge::merge-and-close

# Continuous integration (CI) System

On every commit and every update to merge requests continuous integration (CI) system creates a pipeline, which builds Lazarus for several platforms/widgetsets. If the build fails, it notifies the creator of commit which caused the failure. 

The building process happens in a virtual machine (Docker container), which loads an operating system image. Our image is based on Debian 12 x86_64 and at the time of writing this section contains: 

  * GTK2, GTK3, Qt5, Qt6 libraries;
  * FPC (versions 3.2.0 [oldstable], 3.2.2 [stable], 3.2.3 [fixes], 3.3.1 [main]) for the following targets: 
    * x86_64-linux;
    * x86_64-win64 (cross-compiler);
    * i386-win32 (cross-compiler);
  * Pas2JS compiler (Git snapshot, compiled with FPC main compiler/sources).



Docker image configuration is defined in _buildenv.Dockerfile_ file, and pipeline configuration is defined in _.gitlab-ci.yml_ (both files are located in Lazarus root directory). 

At the time of writing this section our pipeline consists of four stages: 

  1. Update our Docker image (see above). This process can be triggered manually in **Build** ->**Pipelines** page by running **update-build-env** job of the first manual stage of the most recent pipeline belonging to _main_ branch (rebuilding process takes ~23 min). It should be run in the following cases: 
     1. When there is a desire to simply update FPC fixes and main snapshots. It is safe if FPC versions haven't changed in the meantime (in rare cases rebuilding process (FPC snapshots compilation) may fail, re-run the job in this case). It is enough to just run the job (no variables need to be defined), the image will be updated automatically and will include FPC snapshots at the time of rebuilding of image.
     2. When _buildenv.Dockerfile_ and/or FPC version constants in _.gitlab-ci.yml_ file have been changed. **Do with care!**
  2. Clear Qt5Pas/Qt6Pas bindings caches if (and only if) any file in bindings is changed. Reasons to keep this stage separate from updating Qt5Pas/Qt6Pas bindings: 
     1. Cache actually contains all files from bindings directory (both source and object ones) and is extracted after checkout, so it overwrites checked out sources, and thus prevents correct cache update if not cleared appropriately.
     2. Sources should be kept in cache, as removing them will cause bindings to be always recompiled (both in bindings and IDE compile jobs). It takes long time and thus undesirable.
     3. Simply doing full cache rebuild if any file in bindings is changed is not enough as this job will not be triggered e. g. for a new merge request (if it does not modify bindings) and will cause IDE compile jobs for it to fail. GitLab CI currently does not support conditions for triggering a job if a cache is missing.
  3. Update Qt5Pas/Qt6Pas bindings and cache results of these jobs. They are required for subsequent building of Lazarus with Qt5 and Qt6 widgetsets. This is needed because building both Qt5 and Qt6 bindings takes a long time (~25 min) and they are rarely changed.
  4. Build Lazarus for various widgetsets/platforms. The following jobs for building Lazarus are defined: 
     * x86_64-gtk2 bigide (oldstable, stable, fixes, main FPC);
     * x86_64-gtk3 bigide (oldstable, stable FPC);
     * x86_64-qt5 bigide (oldstable, stable FPC);
     * x86_64-qt6 bigide (oldstable, stable FPC);
     * x86_64-win64 bigide (oldstable, stable FPC);
     * i386-win32 bigide (oldstable, stable, fixes, main FPC);
     * Documentation (stable, fixes, main FPDoc);
     * Tests for Codetools (main FPC).



At the time of writing this section the following jobs for building Lazarus run automatically on every commit: 

  * x86_64-gtk2 bigide (stable FPC);
  * x86_64-gtk3 bigide (stable FPC);
  * x86_64-qt5 bigide (oldstable FPC);
  * x86_64-qt6 bigide (stable FPC);
  * i386-win32 bigide (stable FPC);
  * Tests for Codetools (main FPC).



All others can be triggered manually via GitLab web interface. 

In GitLab web interface you can notice **Build** menu item at the left. By clicking **Build** ->**Pipelines** you will get to pipelines list, where you can see build status for each commit. By clicking circles in the **Stages** column you will get to submenus which will show status of all jobs and will allow to run/rerun them manually (by clicking at **play** /**retry** button) and to look at job's log (by clicking at job's name). 

Important notice for Qt5/Qt6 jobs: 

They use cached results from stage 3. In absolute majority of the builds this should not cause any problems. But still if stage 3 does not pass or if you get Qt5/Qt6 widgetset build failure related to Qt5Pas/Qt6Pas version mismatch or missing cache, make sure to go to the **Pipelines** page and do the following: 

  1. Stop the currently running pipeline if needed.
  2. Press **Run pipeline** button (you will be asked about defining variables, just skip this step (we don't use them) and confirm running pipeline).
  3. In case pipeline failed to pass, press **Clear runner caches** button and then rerun pipeline by pressing **Run pipeline** button again.

---

_Source: [https://wiki.freepascal.org/Lazarus_Team_-_Git_Workflows](https://web.archive.org/web/20240319145753/https://wiki.freepascal.org/Lazarus_Team_-_Git_Workflows)_
