# User Changes 2.6.2

│ **English (en)** │  **[русский (ru)](<../ru/User_Changes_2.6.md>)** │

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Language changes
      * 2.1.1 Anonymous inherited calls
      * 2.1.2 _Overload_ modifier must be present in the interface
    * 2.2 Unit changes
      * 2.2.1 Several methods of TDataset changes signature (TRecordBuffer)
      * 2.2.2 DLLParam changed from Longint into PtrInt
      * 2.2.3 Some symbols in unit Unix and Unixutils have been deprecated
      * 2.2.4 TStrings.DelimitedText behavior changed (unit Classes)
      * 2.2.5 fcl-image TTiffIDF renamed to TTiffIFD
      * 2.2.6 unit libc issues a deprecated warning
  * 3 Other
    * 3.1 UPX support has been removed
  * 4 Previous release notes



## About this page

Below you can find a list of intentional changes since the [previous release](<User_Changes_2.6.md> "User Changes 2.6.0") that can change the behaviour of previously working code, along with why these changes were performed and how you can adapt your code if you are affected by them. The list of new features can be found [here](<FPC_New_Features_Trunk.md> "FPC New Features Trunk"). 

## All systems

### Language changes

#### Anonymous inherited calls

  * **Old behaviour** : An anonymous inherited call could call through to any method in a parent class that accepted arguments compatible to the parameters of the current method.
  * **New behaviour** : An anonymous inherited call is guaranteed to always call through to the method in a parent class that was overridden by the current one.
  * **Example** : See <http://svn.freepascal.org/svn/fpc/trunk/tests/tbs/tb0577.pp>. In previous FPC versions, the _inherited_ call in _tc3.test_ would call through to _tc2.test(b: byte; l: longint = 1234);_. Now it calls through to _tc.test_.
  * **Reason** : Conform to the FPC documentation, Delphi-compatibility.
  * **Remedy** : If you wish the compiler to decide which method to call based on the specified parameters, use a fully specified inherited call expression such as _inherited test(b)_.



#### _Overload_ modifier must be present in the interface

  * **Old behaviour** : It was possible to declare a function/procedure/method as _overload_ only in the implementation.
  * **New behaviour** : If an _overload_ directive is used, it must also appear in the interface.
  * **Reason** : The old mechanism could cause hard to find problems (depending on whether or not the implementation was already parsed, the compiler would treat the routine as if it were declared with/without _overload_), it could cause unwanted unit recompilations due to interface crc changes, and Delphi compatibility.
  * **Remedy** : Make sure that the _overload_ modifier is present both in the interface and in the implementation if you use it.



  


### Unit changes

#### Several methods of TDataset changes signature (TRecordBuffer)

  * **Old behaviour** : Several (virtual) methods of TDataset have parameters of type "pchar", which are often called "buffer".
  * **New behaviour** : The pchar type has been changed to _TRecordBuffer_. Currently this type is still an alias for p(ansi)char, but in time it will be changed to pbyte for the 2.7.1/2.8.0 branch, which is D2009+ compatible.
  * **Reason** : Preparation for Delphi 2009+ compatibility and improving of general typing. In Delphi 2009+ (and fully compatible FPC modes in the future) pchar is not pointer to byte anymore. This change will be merged back to 2.6(.2), but with TRecordBuffer=pchar.
  * **Remedy** : Change the relevant virtual methods to use TRecordBuffer for buffer parameters. Define TRecordBuffer=pansichar to keep older Delphis and FPCs working. In places where a buffer is typecasted, don't use pchar but the symbol TRecordbuffer.



#### DLLParam changed from Longint into PtrInt

  * **Old behaviour** : DLLParam was of type Longint even on Win64.
  * **New behaviour** : DLLParam is now of type PtrInt so also on 64 Bit systems.
  * **Reason** : Prevent data loss, match the declaration in the Windows headers.
  * **Remedy** : Change the declaration of the procedures used as dll hook to take a PtrInt parameter instead of Longint.



#### Some symbols in unit Unix and Unixutils have been deprecated

  * **Old behaviour** : No deprecated warning for unixutils.getfs (several variants), unix.fpsystem(shortstring version only), Unix.MS_ constants and unix.tpipe. unix.statfs
  * **New behaviour** : The compiler will emit a deprecated warning for these symbols. In future versions these may be removed.
  * **Reason** : getfs has been replaced by a wholly cross-platform function sysutils.getfilehandle long ago. fpsystem(shortstring) was a leftover of the 1.0.x->2.0.x migration (the ansistring version remains supported), the MS_ constants are for an msync call that is not supported by FPC, and thus have been unused and unchecked for over a decade and might date to kernel 1.x times, tpipe was the 1.0.x alias of baseunix.TFildes, the unit where the (fp)pipe was moved to in during 2.0 series. Unix.statfs is an overloaded version that wasn't properly renamed to fp* prefix when the others were renamed in 2.4.0
  * **Remedy** : Use the new variants(sysutils.getfilehandle,fpsystem(ansistring),baseunix.tfildes). In the case of the MS_ constants, obtain current values for the constants from the same place where you got the code that uses them.



#### TStrings.DelimitedText behavior changed (unit Classes)

  * **Old behaviour** : If StrictDelim is true, TStrings.DelimitedText did not completely follow the SDF format specification (which is defined in Delphi help) at least in case of spaces (and presumably other low ASCII characters) in front and at the end of fields as well as quotes and line endings. Worse, if StrictDelimiter is true, and in the cases mentioned above, saving a TString .DelimitedText and loading that text in another TString lead to differences between the two. Note: StrictDelimiter is false by default.
  * **New behaviour** : FPC follows Delphi behaviour.
  * **Reason** : Consistency (writing out and reading in DelimitedText should result in the same strings), Delphi compatibility (following the SDF specification).
  * **Remedy** : Review your existing code that reads or write DelimitedText; if necessary convert data or write converter code. See tests\webtbs\tw19610.pp for a detailed test.



#### fcl-image TTiffIDF renamed to TTiffIFD

  * **Old behaviour** : The tiff helper class for the "image file directory" was misspelled TiffIDF (tiffcmn unit)
  * **New behaviour** : Now renamed to TTiffIFD
  * **Reason** : Consistency, low usage
  * **Remedy** : Rename identifier as appropriate.



#### unit libc issues a deprecated warning

  * **Old behaviour** : While deprecated for years the [libc unit](<libc_unit.md> "libc unit") didn't issue a deprecated warning
  * **New behaviour** : A deprecated warning is shown when unit libc is used, urging your to update.
  * **Reason** : unit libc is a Kylix legacy unit, with limited portability
  * **Remedy** : Use proper FPC units as described in [libc unit](<libc_unit.md> "libc unit")



## Other

### UPX support has been removed

  * **Old behaviour** : There was some leftover UPX (an executable packer) support in the FPC Makefiles, and DOS and Windows FPC releases included an UPX binary.
  * **New behaviour** : All removed.
  * **Reason** : Release binaries haven't been UPX'ed for a while. The size of the FPC executables is generally insignificant these days compared to the total installation size, and using UPX occasionally causes some minor annoyances (false positives from virus scanners, worse paging behaviour by the OS, incompatibilities with certain executable sections, ...)
  * **Remedy** : Download and install UPX yourself from its [homepage](<http://upx.sourceforge.net/>) and in general reevaluate the need for it.



## Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | 2.6.2 | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_2.6.2](https://web.archive.org/web/20240914144001/https://wiki.freepascal.org/User_Changes_2.6.2)_
