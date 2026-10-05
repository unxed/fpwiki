# User Changes 3.0.4

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Implementation Changes
    * 2.2 Unit changes
      * 2.2.1 SysUtils
        * 2.2.1.1 TList auto-growth
      * 2.2.2 Inifiles
      * 2.2.3 DB
        * 2.2.3.1 TParam.LoadFromFile sets share mode to fmShareDenyWrite
  * 3 i386-go32v2
    * 3.1 Unit changes
      * 3.1.1 go32
        * 3.1.1.1 set_segment_base_address
        * 3.1.1.2 set_segment_limit
        * 3.1.1.3 get_segment_limit
        * 3.1.1.4 set_descriptor_access_right
        * 3.1.1.5 get_linear_address
        * 3.1.1.6 map_device_in_memory_block
  * 4 Previous release notes



## About this page

Listed below are intentional changes made to the FPC compiler (3.0.4) since the [previous release](<User_Changes_3.0.md> "User Changes 3.0.2") that may break existing code. The list includes reasons why these changes have been implemented, and suggestions for how you might adapt your code if you find that previously working code has been adversely affected by these recent changes. 

The list of new features that do not break existing code can be found [here](<FPC_New_Features_Trunk.md> "FPC New Features Trunk"). 

Please add revision numbers to the entries from now on. This facilitates moving merged items to the user changes of a release. 

## All systems

### Implementation Changes

### Unit changes

#### SysUtils

##### TList auto-growth

  * **Old behaviour:** Lists with number of elements greater than 127 was expanded by 1/4 of its current capacity
  * **New behaviour:** Adds two new thresholds. If number of elements is greater than 128 MB then list is expanded by constant amount of 16 MB elements (corresponds to 1/8 of 128 MB). If number of elements is greater then 8 MB then list is expanded by 1/8 of its current capacity.
  * **Reason for change:** Avoid out-of-memory when very large lists are expanded



#### Inifiles

  * **Old behaviour:** In 3.0.2, TMemIniFile.ReadSectionValues did not read invalid name/value pairs
  * **New behaviour:** 3.0.4 reads invalid name/value pairs again.
  * **Reason for change:** Wrong defaults assumptions in 3.0.2, change is more delphi and 3.0.0 compat
  * **Remedy:** If you only want real name/value pairs, pass an additional empty set parameter to ReadSectionValues. if you need invalid lines and comments please pass [svoIncludeComments,svoIncludeInvalid]



#### DB

##### TParam.LoadFromFile sets share mode to fmShareDenyWrite

  * **Old behaviour** : TFileStream.Create(FileName, fmOpenRead) was used, which has blocked subsequent access (also read-only) to same file
  * **New behaviour** : TFileStream.Create(FileName, fmOpenRead+fmShareDenyWrite) is used, which does not block read access to same file
  * **Remedy** : If your application requires exclusive access to file specify fmShareExclusive



## i386-go32v2

### Unit changes

#### go32

##### set_segment_base_address

  * **Old behaviour:** The second parameter, specifying the new segment base address was longint (signed 32-bit).
  * **New behaviour:** The second parameter was changed to dword (unsigned 32-bit).
  * **Reason for change:** Avoid range check errors for segment base addresses larger than 2GB.



##### set_segment_limit

  * **Old behaviour:** The second parameter, specifying the new segment limit was longint (signed 32-bit).
  * **New behaviour:** The second parameter was changed to dword (unsigned 32-bit).
  * **Reason for change:** Avoid range check errors for segment limits larger than 2GB.



##### get_segment_limit

  * **Old behaviour:** The result of get_segment_limit was longint (signed 32-bit).
  * **New behaviour:** The result of get_segment_limit was changed to dword (unsigned 32-bit).
  * **Reason for change:** Avoid range check errors for segment limits larger than 2GB.



##### set_descriptor_access_right

  * **Old behaviour** : The set_descriptor_access_right function returned a longint result, which wasn't well defined.
  * **New behaviour** : The set_descriptor_access_right function now returns a boolean result - TRUE if the function has been successful, FALSE if there was an error.
  * **Reason** : Bug fix. Previously, the set_descriptor_access_right function would return a longint, but only the low 16-bits were initialized (with 1 indicating success, 0 - failure).
  * **Remedy** : Instead of checking whether the result of set_descriptor_access_right is equal or different than 0, just use the boolean return value (TRUE indicates success).



##### get_linear_address

  * **Old behaviour:** The first parameter (phys_addr - specifying the physical address to be mapped) and the function result (specifying the linear address, where phys_addr was mapped) were longint (signed 32-bit).
  * **New behaviour:** The first parameter (phys_addr) and the function result were changed to dword (unsigned 32-bit).
  * **Reason for change:** Avoid range check errors for physical or linear addresses larger than 2GB.



##### map_device_in_memory_block

  * **Old behaviour:** All four parameters to map_device_in_memory block were longint (signed 32-bit).
  * **New behaviour:** The four parameters were changed to dword (unsigned 32-bit).
  * **Reason for change:** Avoid range check errors for physical addresses, memory handles or memory offsets, larger than 2GB.



## Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | 3.0.4 | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_3.0.4](https://web.archive.org/web/20241227120218/https://wiki.freepascal.org/User_Changes_3.0.4)_
