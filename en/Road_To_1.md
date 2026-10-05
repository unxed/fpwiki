# Lazarus Development Process

│ **English (en)** │

## Contents

  * 1 Who are developers
  * 2 Setting the target of a bugfix
  * 3 What we are planning to do
    * 3.1 TODOs
    * 3.2 Tasks
    * 3.3 Roadmaps
  * 4 What we have done
  * 5 What we will not do
  * 6 Lazarus branches / version numbers
  * 7 See also



## Who are developers

You can find a recent list of Lazarus developers here: [Developer pages](<Developer_pages.md> "Developer pages")  
You can find history of lazarus developers here: [History](<History.md> "History")

## Setting the target of a bugfix

When new bugs are entered, we try to give them a target in which version the bug will be fixed. If a bug is set to _post 1.2_ , that means the developers think this bug is not important enough to block a 1.0.x release. Of course you can make sure these _post 1.2_ issues are fixed in the 1.0.x release by providing patches for these issues. 

Some criteria are: 

  * Only gtk2, qt and win32 widget sets are stable in 1.0.x. Bugs for other widget set (carbon) are set to post 1.2.
  * Until 1.0 there will be a feature freeze. New features and components generally get a post 1.2 target. Bugs affecting stability have a higher priority than bugs fixing the implementation of a property.
  * Some components are not stable enough and should be disabled for 1.0.x. If they are disabled, then fixing them before 1.2 will not be necessary.



For more details on the various versions of Lazarus, see [Version Numbering](<Version_Numbering.md> "Version Numbering")

## What we are planning to do

### TODOs

  1. [Lazarus 1.8.0 release notes](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes")
  2. [Detailed Lazarus release template todo](<Detailed_Lazarus_release_template_todo.md> "Detailed Lazarus release template todo")



### Tasks

  1. [IDE Development](<IDE_Development.md> "IDE Development")



### Roadmaps

  1. [Current Roadmap](<http://bugs.freepascal.org/roadmap_page.php?project_id=1>) \- Roadmap of current Lazarus version.
  2. [Roadmap](<Roadmap.md> "Roadmap") \- Current status of some parts of Lazarus (IDE, LCL and others)
  3. [Icon Editor Roadmap](<Icon_Editor_Roadmap.md> "Icon Editor Roadmap") \- Roadmap of Icon Editor Tool
  4. [LCL Documentation Roadmap](<LCL_Documentation_Roadmap.md> "LCL Documentation Roadmap") \- Roadmap of LCL Documentation



## What we have done

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

  


## What we will not do

  1. [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)")



## Lazarus branches / version numbers

This ASCII art schema shows what the Lazarus developers have chosen as branching policy for the Lazarus 1.0 release. It also illustrates the way current development works. Time goes from left to right; the different branches are shown vertically. B indicates a branch point, T a tag (release). 
    
    
    
    
                           0.9.30         0.9.30.2       0.9.30.4
                             |              |              |
    fixes_0_9_30:         -- T - 0.9.30.1 - T - 0.9.30.3 - T - 0.9.30.5 -- End of life
                         /  
                        |                        0.99.0(1.0.RC1)      1.0.0        1.0.2         1.0.4         1.0.6         1.0.8         1.0.10         1.0.12
                        |                          |          more RCs  |            |             |             |             |             |              |
    fixes_1_0:          |                    ------T - 0.99.1 -< .. >-- T - 1.0.1 -- T -- 1.0.3 -- T -- 1.0.5 -- T -- 1.0.7 -- T -- 1.0.9 -- T -- 1.0.11 -- T -- End of life
                        |                   /
                        |                  /                                              1.2.0         1.2.2          1.2.4
                        |                 /                                                 |             |              |                     
    fixes_1_2:          |                |                                            ----- T - 1.2.1 --- T - 1.2.3----- T - 1.2.5 ---- End of life
                        |                |                                           /  
                        |                |                                           | 
    trunk: --- 0.9.29 --B-- 0.9.31 ------B- 1.1 -------------------------------------B------- 1.3 ------------ Developing for 1.4 or 2.0 
    

## See also

  * [Roadmap](<Roadmap.md> "Roadmap")
  * [Lazarus release engineering](<Lazarus_release_engineering.md> "Lazarus release engineering")
  * [Lazarus website development](<WebPageDevelopment.md> "WebPageDevelopment")

---

_Source: [https://wiki.freepascal.org/Road_To_1.0](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Road_To_1.0)_
