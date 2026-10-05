# Version Numbering

│ **English (en)** │  **[русский (ru)](<../ru/Version_Numbering.md>)** │


## Contents

  * 1 FPC, Lazarus combinations
  * 2 Explanation of the different version numbers of Lazarus
    * 2.1 The Lazarus release cycle
      * 2.1.1 Odd numbers - "placeholder" aka "development" versions between releases
    * 2.2 Lazarus 3.0 and newer
    * 2.3 Pre 3.0 versioning
    * 2.4 How about fixed bugs, in which version is the fix included?
  * 3 Version numbers in graph form



## FPC, Lazarus combinations

Lazarus Release | Recommended FPC version | Notes   
---|---|---  
0.9.24 - 0.9.99 | 2.2.4 |   
1.0.0 - 1.0.6 | 2.6.0 |   
1.0.8 - 1.2.2 | 2.6.2 |   
1.2.4 - 1.4.2 | 2.6.4 |   
1.6.0 - 1.6.2 | 3.0.0 | last version for Win98/ME using fpc 2.6.4   
1.6.4 | 3.0.2 |   
1.8.0 - 2.0.8 | 3.0.4 |   
2.0.10 | 3.2.0 |   
2.2.0 and newer | 3.2.2 |   
  
(See also: [Free Pascal](<Free_Pascal.md> "Free Pascal")) 

## Explanation of the different version numbers of Lazarus

Internally, Lazarus version has the following structure: 
    
    
    Major.Minor.Release.Patch

### The Lazarus release cycle

The Lazarus project has 2 kind of releases: major and minor. Note: those do/did not always align with the names for the positions in the version number. 

Major releases
    Include new features.
Minor releases
    Only bug fixes.

When a major release preparation process starts, a new branch (git branch, earlier svn branch) is made from the "main" (aka "trunk" as it was called for svn) branch. The new branch will be called "fixes_nn", where "nn" is some numbering (see graph). 

Development continues on the main branch. Selected bug-fixes are merged to the fixes branch. Each major release is made from a newly created fixes branch. The subsequent minor releases are all made from the same fixes branch as their major release. 

In the past the first 2 numbers ("major.minor") were used for the major version, and the 3rd number ("release") indicated minor releases. There was no rule if a new major release would increase the "major" or "minor" number. It was therefore decided that major releases should always increase the "major" number. And minor releases should increase the "minor" number. The first major release with the new numbering is Lazarus 3.0. 

#### Odd numbers - "placeholder" aka "development" versions between releases

There are special version numbers reserved for the code under development. Those versions do not indicate a fixed point (i.e. specific commit), but are used for all and any commits between releases. 

After a major release process was started (i.e. fixes branch has been created), new features continue to be added to "main" branch. The version number shared by every commit in the "main" branch will be set to one (one and the same for all commits) such "placeholder" version. E.g. "Lazarus 2.1" or "Lazarus 3.99" do not correspond to any specific commit for the IDE, they denote any commit on that branch. Usually, referring to such a version is used to say "the latest commit on that branch" (which can change hourly, daily, weekly, ... whatever). 

Similar "placeholder" versions exist for minor releases. 

All those "placeholder" versions are identified by an odd number, in either the "minor" or "release" position. 

  * Prior to Lazarus 3.0 the odd number for the development version was either in the "minor" (1.1, 2.1, 2.3) or in the "release" (2.0.1, 2.0.3, 2.2.1) positions.
  * Since Lazarus 3.0 the odd number will always be in the "minor" position (3.1, 3.3, ... 3.99, 4.1, ....).



### Lazarus 3.0 and newer

In Lazarus version 3.0 and up, the "release" number is always 0 (zero).  
"Major" and "minor" version numbers are used to identify a release or a fixes branch.  
  
An even "minor" version number, like x.0 or x.2, represents a general availability release.  
An odd "minor" version number like x.1 or x.3 represents a fixes branch. For example: 3.1 would contain bug fixes for the 3.0 release and would be used as the basis for the 3.2 release.  
Development (or trunk) versions are always represented as "minor" version number 99; i.e. 3.99 is the development branch for the future 4.0 release. Also, selected commits from this branch are cherry picked to the 3.x family.  
  
"Patch" numbers identify a release candidate or a general availability release. For example: 3.0.0 RC1 has the the full version value of 3.0.0.1. RC2 has the value 3.0.0.2, and the released product could be 3.0.0.3. 

### Pre 3.0 versioning

In Lazarus preceding 3.0, versioning had the following differences: 

"Major", "minor" and "release" version numbers were used to identify a release or a fixes branch.  
An even "minor" and "release" version numbers, like x.2.0 or x.2.2, represented a general availability release.  
An odd "minor" version number indicated that it is a development (or trunk) version. Example: x.1.0.  
An odd "release" version number indicated fixes branch. Example: 1.4.1 in Lazarus 1.4 series. 1.4.1 would have eventually become 1.4.2 general availability release. 

### How about fixed bugs, in which version is the fix included?

Take the issue [[1]](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/30187>) as an example. 

It has a "milestone" set ("version 3.0"). 

  * For open bugs the milestone indicates the currently planned target by which the issue should be fixed. It may however be rescheduled.
  * For closed issues, it is set to the release that will contain the issue.



Some issues also refer to the commit containing the fix. Usually the last "mentioned in commit" note. Or a dedicated comment by a developer. In this case [[2]](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/commit/6a84f7039d3a6f48be85a968eb8f68f8d7c46a5a>)

On that commit page is the following info (needs to be unfolded) 
    
    
       Branches containing this commit
       fixes_3_0 main
       Tags containing this commit
       lazarus_3_0_RC1 main_3_99 t-fixes-3
    

`lazarus_3_0_RC1` is the indicator that it will be in the 3.0 release (after the RC, in which it also is included) 

Some older issues, that were imported from mantis have tags named "target". [[3]](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/34773>)

In the "mantis" block is the info "Fixed in revision" which links to a commit, as described above. 

## Version numbers in graph form

Maybe this will help some users understand the version numbering a bit better. 
    
    
    o - main branch becomes version 3.99: development + experimental stuff, fixes branch 3.0 is created
    |
    |\
    | \
    |  |
    |  o - (branches\fixes_3_0) - feature freeze + release candidate
    |  |\ 
    |  | o - (tags\release_3_0_RC1)
    |  |\ 
    |  | o - (tags\release_3_0) - Release 3.0. After this release version in fixes branch is incremented to 3.1.
    |  |\ 
    |  | o - (tags\release_3_2) - Release 3.2. After this release version in fixes branch is incremented to 3.3.
    |
    o - main branch becomes version 4.99: development + experimental stuff, fixes branch 4.0 is created
    |
    |\
    | \
    |  |
    |  o - (branches\fixes_4_0) - feature freeze + release candidate
    |  |\ 
    |  | o - (tags\release_4_0_RC1)
    |  |\
    |  | o - (tags\release_4_0) - this will happen in the future
    

  
Note that main branch gets a new version right after branching. So version 3.99 from main branch exists at the same time as version 3.x in fixes_3_0 branch.

---

_Source: [https://wiki.freepascal.org/Version_Numbering](https://web.archive.org/web/20250319232426/https://wiki.freepascal.org/Version_Numbering)_
