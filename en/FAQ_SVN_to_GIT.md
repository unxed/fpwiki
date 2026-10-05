# FAQ SVN to GIT

## Contents

  * 1 Where is the new GIT repo
  * 2 Where can I get help to use GIT
    * 2.1 How can I get the (checkout/download) sources from GIT
  * 3 Why did we move to GIT
    * 3.1 Why GitLab
    * 3.2 Why now
  * 4 What if I really can't switch to GIT



## Where is the new GIT repo

  * <https://gitlab.com/freepascal.org/fpc>
  * <https://gitlab.com/freepascal.org/lazarus>



And a mirror (FPC and Lazarus) at 

  * <https://github.com/fpc>



And for Lazarus 

  * <https://sourceforge.net/p/lazarus/laz.git/ci/main/tree/>



(As long as Sourceforge supports it) 

## Where can I get help to use GIT

There are 2 pages to help moving from SVN to GIT. Each page is a full "SVN to GIT" overview. The pages are alternatives to each other. You should choose one and stick with that one. 

  * [FPC_git](<FPC_git.md> "FPC git") (based on old/established "git checkout")
  * [SVN_to_GIT_Cheatsheet](<SVN_to_GIT_Cheatsheet.md> "SVN to GIT Cheatsheet") (based on new "git switch")


  * <https://git-scm.com/docs> (the official Git help)
  * [GIT Succinctly](<https://www.syncfusion.com/succinctly-free-ebooks/git>) \- Free Ebook (PDF, MOBI, EPUB) from SyncFusion.



### How can I get the (checkout/download) sources from GIT

See the above pages. 

Install a git client (command line) and run 
    
    
       git clone  <https://gitlab.com/freepascal.org/fpc/source.git>  my_local_dir
       git clone  <https://gitlab.com/freepascal.org/lazarus/lazarus.git>  my_local_dir
    

After that (to update) 
    
    
       git pull    
    

Then use (depending which guide you follow) 
    
    
       git checkout main
       git checkout fixes-2_2
       git switch main
       git switch fixes-2_2
    

## Why did we move to GIT

There are many reasons. Some of them were: 

  * For those who make commits to the project, GIT offers a lot of useful feature.   
That is: **Perceived as useful** by those who make the commits. And therefore helping the daily workflow of the team. (It doesn't matter, if those features are overall better or worse than SVN. They are better for those who work most with the repository) 
    * Local branches, as quick temp storage (not svn shelves / that would be git stash) 
      * This can be used as "savepoint" while working on a commit. (Saving a partial commit, in case one needs to roll back)
      * Storing an unfinished attempt on some idea
    * Easy switching between branches
    * **Syncing between different machines** (Laptop, Virtual machine) without needing an (external) server. This helps tremendously with testing for different platforms.
    * Working offline
    * Personal forks of the repo. For example to share a patch by publishing a branch (unfinished work / may still need to be completely redone) on your personal fork.
    * ... (I need to go through old mails to find the remaining reasons)
  * Maintenance: Hosted solution (see "Why GitLab"). To the best of our knowledge there is no hosted SVN platform offering even half of what hosted GIT platform offer.  
This includes the high maintenance cost of running Mantis



Note that several team members have in the past already used "git svn" and similar solutions. Despite the downsides (e.g. git svn, is really slow to update), this means that the above points are not just hopeful assumptions. The listed "perceived git advantages" have already been experienced by those team members. 

On the long run, we may also be able to use the additional features offered by the hoster (that would have applied to GitHup or GitLab), such as CI integration. But that's to be seen in future. 

### Why GitLab

Self hosting becomes more and more involved. Aside from the technical maintenance needs, the legal requirements are getting bigger and bigger. In order to protect our self, and save time we decided to use a hosted platform. 

  * GitLab offers a very good deal to open source projects. Better than GitHub, at this time.
  * GitLab is open source. If ever we need to self host again, we have the choice to keep the platform.



### Why now

The old server would have had to be replaced now. So we were at a point where we had to invest considerable time and effort in any way we could go. Given that the move to Git had been decided, there was no point to invest time into temporarily extending SVN. 

## What if I really can't switch to GIT

We have mirrors at GitHub. And GitHub (currently) allows to access them with an svn client. (see the drop-down for the "clone url"). 

However, this should be only used as a last resort. The teams do not have any plans to invest time into supporting this. It is there as long as GitHub offers it for free. 

Further, it is incompatible with the old svn. GitHub reverse translates it from git. So each revision gets a new arbitrary revision number. And those numbers can not be used to report/discuss issues. No one in the team uses this svn-mirror, so non of us has access to those revision numbers. To report issues you need the git sha1.

---

_Source: [https://wiki.freepascal.org/index.php?title=FAQ_SVN_to_GIT&oldid=149276](https://web.archive.org/web/20220706013231/https://wiki.freepascal.org/index.php?title=FAQ_SVN_to_GIT&oldid=149276)_
