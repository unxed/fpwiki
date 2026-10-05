# Lazarus release engineering

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page is still work in progress. For feedback please use our mailing list.

## Contents

  * 1 Checklist
    * 1.1 Major Release
    * 1.2 For each Release Candidate
    * 1.3 For the Release
    * 1.4 Minor Release
      * 1.4.1 Approve Release (Start at day: 7 to 9 / Duration 3/4 days)
      * 1.4.2 Prepare Release (Start at day: 10 to 12 / Duration 3/4 days)
      * 1.4.3 Release (At day 14 to 16)
  * 2 Merging fixes
  * 3 Other Items
  * 4 Checklist (Suggestions)
  * 5 How to merge
    * 5.1 Using TortoiseSVN
    * 5.2 Show list of revision that can be merged
    * 5.3 Show list of revision that have been merged
    * 5.4 Merging revisions from trunk
    * 5.5 Blocking a revision to be merged
  * 6 Previous releases
  * 7 See also
  * 8 Related pages



## Checklist

### Major Release

  * Decide on new release 
    * Check internally with the team
    * Decide on approx Date for first RC (date may change). This may be optional.
  * Prepare 
    * Create fixes branch in SVN
    * Create fixes page on wiki
  * Announce 
    * _Special announcement_ for translators.
    * _Public announcement_ with approx RC date (if available).



### For each Release Candidate

  * Between 1 and 2 weeks before RC is due 
    * Check internally with the team
    * _Public reminder/announcement_ that RC is due.
    * If needed: Reschedule/Delay
  * Publish the Release Candidate 
    * Tag RC in SVN
    * Build and upload
    * _Public announcement_ of RC

Typical life time of an RC can be expected to be between 3 and 6 weeks. But in the end will be decided at the discretion of the team. 

### For the Release

  * At least 2 weeks before Release 
    * Check internally with the team
    * Public reminder/announcement that Release is due. No more RC planned.
    * Repeat _Special announcement_ for translators.
  * 1 week before Release - Depending on feedback (the team should have 3 or 4 days time to discuss this): 
    *       * keep decision to release
      * move date of release
      * schedule another RC
    * 3 to 5 days later: _Public announcement_ of above decision, if a change to the plan was made.
  * Make the Release 
    * Tag RC in SVN
    * Build and upload
    * _Public announcement_ of Release

  
| 

### Minor Release

  * Decide on new Release (Day: approx -4 / Duration 3-4 days) 
    * Check internally with the team (3 to 4 days)
    * Release Date will be 2 weeks from announcement (1 week for community feedback)
  * Prepare 
    * Update fixes page on wiki (New section "Fixes for ..." next release


  * Announce upcoming Release (Day: 0 / Duration 7-8 days) 
    * _Special announcement_ for translators.
    * _Public announcement_ with approx date. 
      * Mail
      * Forum



    Ask for 

  * new regressions
  * missing merges


Community has about 1 week to respond. Urgent matters can still be reported during "approve" phase. Depending on feedback received: reschedule and if needed re-announce. 

#### Approve Release (Start at day: 7 to 9 / Duration 3/4 days)

  * Check internally with the team (3 to 4 days)

Depending on feedback received: reschedule and if needed re-announce. 

#### Prepare Release (Start at day: 10 to 12 / Duration 3/4 days)

  * TAG SVN
  * Build docs => update binaries svn
  * Start building
  * Build Release binaries 
    * Test
  * Upload (staged)

At the same time: 

  * Update mantis
  * Collect checksums and commit to web-page svn 
    * Optional: Update webpage to contain checksums
  * Update web-page svn with new download links (to be published *after* release)



#### Release (At day 14 to 16)

  * Make files public on Sourceforge (un-stage) 
    * Set files as current release


  * _Public announcement_ of Release 
    * Mail
    * Forum
    * Twitter
    * Sourceforge news


  * Update web-page (Download-links / Checksums)

  
  
---|---  
  
## Merging fixes

Team members will add any revision that should be merged to the "fixes branch" page (see "Related Pages"). 

User can contact Team members 

  * on the forum
  * on the mail list
  * on open & assigned mantis reports



to request revisions to be added. 

## Other Items

In addition to the items on the checklist: 

  * create new targets on Mantis, with each RC, and for the Release
  * Publish checksums on webpage
  * Update links on webpage
  * Updating of help files
  * Announcements for Translators



TODO: Decide on channels for public feedback on: 

  * Open regressions
  * Missing merges



## Checklist (Suggestions)

Please add suggestions here. The editing of the Checklist above should be reserved to the team. Thanks. 

## How to merge

The lazarus developers have decided to use the native svn merge for this branch. Other branches used the svnmerge.py script to manage the revisions to be merged. 

  * TODO: Maybe this information should be put in a separate page.



[SVN_Migration#Merge_with_plain_svn](<SVN_Migration.md> "SVN Migration")

### Using TortoiseSVN

As noted in the link above using TortoiseSVN is more or less self explaining. 

  * TODO: add some screen shots.



When committing, in the recent messages a nice commit message is available. 

Tested with TortoiseSVN 1.7.6, SVN 1.7.4. 

### Show list of revision that can be merged

In the lazarus fixes_1_0 directory do: 
    
    
    svn mergeinfo ^/trunk --show-revs eligible
    

Or if you don't have a fixes directory checked out, you can pass the URL path of the fixes branch: 
    
    
    svn mergeinfo ^/trunk <http://svn.freepascal.org/svn/lazarus/branches/fixes_1_0> --show-revs eligible
    

### Show list of revision that have been merged

In the lazarus fixes_1_0 directory do: 
    
    
    svn mergeinfo ^/trunk --show-revs merged
    

### Merging revisions from trunk

To merge one or more revisions from trunk, use the svn merge command. For example to merge revision 36506 and 36510 use: 
    
    
     svn merge -c 36506,36510 ^/trunk
    

To generate a commit log message, use: 
    
    
    svn log ^/trunk -c 36506,36510 > svnmerge-commit-message.txt
    

Edit svnmerge-commit-message.txt with your favorite text editor and add a first line like: 
    
    
    Merged revision 36506,36510 from /trunk.
    

Commit this with: 
    
    
    svn commit -F svnmerge-commit-message.txt
    

### Blocking a revision to be merged

Sometimes you want to block a revision to be merged, i.e. you want to make sure that this revision is never merged to the fixes branch, for example, because it contains a new feature or it contains a version number change not applicable to the fixes branch, but only to trunk. To block revision 36507, use: 
    
    
    svn merge -c 36507 --record-only ^/trunk
    

Then you can create a log message with: 
    
    
    svn log ^/trunk -c 36507 > svnmerge-commit-message.txt
    

Edit svnmerge-commit-message.txt with your favorite text editor and add first line like: 
    
    
    Blocked revision 36507 from /trunk
    

Then commit this change: 
    
    
    svn commit -F svnmerge-commit-message.txt
    

## Previous releases

  * [Lazarus 0.9.30 todo](<Lazarus_0.9.md> "Lazarus 0.9.30 todo")
  * [Lazarus 0.9.28 todo](<Lazarus_0.9.md> "Lazarus 0.9.28 todo")
  * [Lazarus 0.9.22 todo](<Detailed_Lazarus_0.9.md> "Detailed Lazarus 0.9.22 todo")


  * [Lazarus 0.9.30.2 release plan](<Lazarus_0.9.30.md> "Lazarus 0.9.30.2 release plan")
  * [Lazarus 0.9.28.2 release plan](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release plan")



## See also

  * [Detailed Lazarus release template todo](<Detailed_Lazarus_release_template_todo.md> "Detailed Lazarus release template todo")



## Related pages

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/Lazarus_release_engineering](https://web.archive.org/web/20250219052023/https://wiki.freepascal.org/Lazarus_release_engineering)_
