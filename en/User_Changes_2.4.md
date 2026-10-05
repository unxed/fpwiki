# User Changes 2.4.2

## Contents

  * 1 About this page
  * 2 All Systems
    * 2.1 Language changes
      * 2.1.1 Abstract and Sealed class modifiers
    * 2.2 Types changes
      * 2.2.1 TObject class declaration
    * 2.3 Unit changes
      * 2.3.1 Daemonapp unit moved to new fcl-extras package
      * 2.3.2 DOM unit: no longer uses avl_tree for node search optimization
      * 2.3.3 MacOSAll unit: changed parameter types
      * 2.3.4 Sockets unit, sockaddr_in type and aliases
      * 2.3.5 System.TTime
  * 3 Windows
    * 3.1 Microsoft Windows 95 support
  * 4 Previous release notes



## About this page

Below you can find a list of intentional changes since the [previous release](<User_Changes_2.4.md> "User Changes 2.4.0") that can change the behaviour of previously working code, along with why these changes were performed and how you can adapt your code if you are affected by them. 

## All Systems

### Language changes

#### Abstract and Sealed class modifiers

  * **Old behaviour** : It was possible to have _abstract_ or _sealed_ fields immediately following the _class_ keyword.
  * **New behaviour** : The compiler now interprets _abstract_ and _sealed_ as class modifiers if they immediately follow the _class_ keyword.
  * **Example** :


    
    
    {$mode objfpc}
    type
      TSomeClass1 = class
         abstract: integer;
      end;
    
      TSomeClass2 = class
         sealed: integer;
      end;
    

The above code will no longer compile. 

  * **Reason** : _abstract_ and _sealed_ are now the class modifiers, which is Delphi-compatible
  * **Remedy** : Separate the _abstract_ or _sealed_ field name from the _class_ keyword with a visibility section (_published_ , _public_ , ...) or with another field declaration.



### Types changes

#### TObject class declaration

  * **Old behaviour** : TObject class did not have UnitName, Equals, GetHashCode, ToString methods.
  * **New behaviour** : Those methods were added to the TObject class.
  * **Effect** : Compiler produces warnings in delphi mode and errors in objfpc mode if user classes have the same identifier(s).
  * **Reason** : Delphi compatibility, these methods are present in the TObject class since Delphi 2009.
  * **Remedy** : Adjust your code so it does not conflict with the TObject class members. Override TObject method where is possible or rename/reintroduce identifier in other case.



### Unit changes

#### Daemonapp unit moved to new fcl-extras package

  * **Old behaviour** : unit Daemonapp resided in package fcl-base.
  * **New behaviour** : unit Daemonapp resides in package fcl-extra.
  * **Effect** : On OSes that have a package for every directory in packages, daemonapp seems to be missing if fcl-extra is not installed.
  * **Reason** : Breaking dependency cycles in the FCL, most notably on Windows.
  * **Remedy** : On operating systems where a package in packages/ corresponds to a separate package to install (like on Debian), an additional package corresponding to fcl-extra must be installed



#### DOM unit: no longer uses avl_tree for node search optimization

Note: This is an implementation-only change. Nevertheless, it's worth documenting and explanation. 

  * **Old behaviour** : each 'parent' DOM node (Elements and Attributes, in the first place) had an associated TAvgLvlTree object, that was used as child node node index, and intended to speed up the `TDOMNode.FindNode` method by replacing linear search by binary search.
  * **New behaviour** : the TAvgLvlTree is no longer used, and `TDOMNode.FindNode` method uses a linear search.
  * **Effect** : The XML parsing speed has increased about twice, and average memory usage of a DOM document has decreased by about 75%. The performance of the `TDOMNode.FindNode` method, and methods of TXMLConfig class (xmlcfg unit), may become lower in some scenarios.
  * **Reason** : The optimization offered by child node indexing was applicable only to one particular usage case: XML config files, which require all node names within a single parent to be unique. The memory/performance penalty caused for all other usage cases has become way too big.
  * **Remedy** : If you experience slowdown of your code using TXMLConfig class from xmlcfg unit, consider replacing xmlcfg with a newer xmlconf unit. Then, use `OpenKey` and `CloseKey` methods to limit the amount of searching in the DOM.
  * **Example** :


    
    
    // the code like this
    uses xmlcfg;
    ...
    begin
      config.WriteString('the/very/long/path/value1', string1);
      config.WriteString('the/very/long/path/value2', string2);
      config.WriteString('the/very/long/path/value3', string3);
      ...
    end;
    
    // can be changed to:
    uses xmlconf;
    ...
    begin
      config.OpenKey('the/very/long/path');
      config.WriteString('value1', string1);
      config.WriteString('value2', string2);
      config.WriteString('value3', string3);
      config.CloseKey;
    end;
    // in this example, 'the/very/long/path' will be searched only once rather than 3 times,
    // which will improve the performance.
    

#### MacOSAll unit: changed parameter types

  * **Old behavior** : The old headers were a combination of the original Apple-written Pascal headers and translated C headers. The Pascal headers were written for compilers that did not support unsigned 32 bit types, and in many cases used "var" parameters where the actual parameter was an array in C.
  * **New behaviour** : The new headers have all been translated from the latest Mac OS X 10.6 headers for 64 bit and iPhoneOS support. As a result, several parameters types have changed and a number of units that no longer exist under Mac OS X have been removed.
  * **Remedy** : Either adjust your code, or file bug reports asking to revert to the old signatures (especially for deprecated routines that won't be updated in the future anymore anyway, this should be no problem).
  * **Changes:**
    * Several units have been removed because they were specific to Classic Mac OS
    * Many cases where signed parameters were turned into unsigned parameters and vice versa, too many to list (due to differences between the classic Pascal headers and the new C headers)
    * ATSFontFindFromContainer: ioArray var -> ptr because array
    * ATSFontGetFileSpecification: oFile FSSpec -> ATSFSSpec (was translation error?)
    * CFNetworkCopyProxiesForAutoConfigurationScript: got extra CFErrorRef para
    * CMGetDeviceFactoryProfiles: defaultProfID var -> ptr, because can be nil
    * CMIterateColorDevices: seed and count var -> ptr, because can be nil
    * CMIterateDeviceProfiles: seed and count var -> ptr, because can be nil
    * CMSetDeviceProfiles: profileScope and deviceProfiles const -> ptr, because can be nil
    * ConvertFromUnicodeToScriptCodeRun: oScriptCodeRuns var -> ptr because array
    * ConvertFromUnicodeToTextRun: oEncodingRuns var -> ptr because array
    * CreateUnicodeToTextRunInfo: iUnicodeMappings var -> ptr because array
    * DCMCreateFieldInfoRecord: findMethods var -> ptr because array
    * DCMGetFieldFindMethods: findMethods var -> ptr because array
    * GetIconRefFromFileInfo: outlabel var -> ptr because can be nil
    * HIImageViewCreate: last parameter ControlRef -> HIViewRef
    * HIViewNewTrackingArea: outRef var -> ptr because can be nil in 10.5+
    * LAGetEnvironmentList: environmentList var -> ptr because array
    * LAListAvailableDictionaries: dictionaryList and opened var -> ptr because array
    * LSGetApplicationForURL: outAppRef and outAppURL var -> ptr because can be nil
    * LocaleOperationGetLocales: localeVariantList var -> ptr because can be nil
    * MPWaitOnQueue: param1-3 value -> var because "void **" in C
    * QTVRSetBackBufferImagingProc: areasOfInterest var -> ptr because array
    * QueryUnicodeMappings: oReturnedMappings var -> ptr because array
    * TECConvertTextToMultipleEncodings: outEncodingsBuffer var -> ptr because array
    * TECFlushMultipleEncodings: outEncodingsBuffer var -> ptr because array
    * TECSniffTextEncoding: numErrsArray and numFeaturesArray var -> ptr because array
    * FSCreateResFile: catalogInfo const -> ptr because can be nil



#### Sockets unit, sockaddr_in type and aliases

  * **Old behavior** : This record is a variant record (union) of the POSIX naming (2.0.x+) and the old 1.0.x naming (mostly without sin_ prefix), to ease migrating 1.0.x -> 2.0.x codebases.
  * **New behaviour** : Since the migration period is long over, "Deprecated" modifiers have been added to the 1.0.x fields in preparation for their removal in a later version. This might trigger warnings.
  * **Reason** : These legacy fields were never documented properly as legacy, and with 2.4.0 "deprecated" modifier on fields now works reliably.
  * **Remedy** : If you are still using the old 1.0.x fieldnames, use the new sin_* prefixed fields as much possible to avoid breakage using future versions.



#### System.TTime

  * **Old behavior** : This type only existed on Unix and was introduced in 2.0 as an "Pascal friendly" alias for time_t.
  * **New behaviour** : This type is now defined as "type tdatetime"
  * **Reason** : Delphi compatibility. Delphi defines ttime and tdate in system.
  * **Remedy** : Use unixtype.ttime or time_t. Be aware that importing (base)Unix units change the meaning of the ttime type.



## Windows

#### Microsoft Windows 95 support

  * **Old behaviour** : FPC-compiled programs, including FPC itself, ran on Windows 95.
  * **New behaviour** : FPC-compiled programs refuse to start on Windows 95.
  * **Reason** : The 2.4.2 version of the Windows system unit references a function that is not present in the Windows 95 version of "kernel32.dll".
  * **Remedy** : See [RTL fix for Windows 95 support](<http://www.freepascal.org/rtl/win95-rtl-242-fix.html>).



## Previous release notes

Lazarus - Release Notes and Subversion Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | 2.4.2 | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_2.4.2](https://web.archive.org/web/20210421150246/https://wiki.freepascal.org/User_Changes_2.4.2)_
