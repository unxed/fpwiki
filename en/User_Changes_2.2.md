# User Changes 2.2.4

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Default property values
    * 2.2 db unit: TField*.*Size type
    * 2.3 db unit: TField*.Lookup property is not published anymore
  * 3 All Unix-based systems
    * 3.1 Executing external programs using TProcess
  * 4 Non-x86 and non-linux/ppc64 systems
    * 4.1 Floating point rounding
  * 5 Windows Platforms
    * 5.1 Functions removed from the Windows unit
  * 6 Previous release notes



## About this page

Below you can find a list of intentional changes between the [the 2.2.2 release](<User_Changes_2.2.md> "User Changes 2.2.2") and the 2.2.4 release which can change the behaviour of previously working code, along with why these changes were performed and how you can adapt your code if you are affected by them. 

## All systems

### Default property values

  * **Old behaviour** : Published properties without a specified _default_ were not streamed under certain circumstances.
  * **New behaviour** : Such properties are now treated as if they had _nodefault_ specified. This means that some extra properties will now be streamed in such cases: 
    * Boolean fields with the value _false_
    * Enumerated types with the ordinal value _0_
  * **Example** :


    
    
    type
      tenum = (ea,eb,ec);
      tc = class
       private
        fmybool: boolean;
        fenum: tenum;
       published
        property mybool: boolean read fmybool write fmybool;
        property enum: tenum read fenum write fenum;
      end;
    
    var
      c: tc;
    begin
      c:=tc.create;
      // write c to a stream
    end.
    

In earlier versions, _mybool_ nor _enum_ would be written to the stream in this case, while now they will be. 

  * **Reason** : Delphi compatibility.
  * **Remedy** : Explicitly specify default values as follows


    
    
    type
      tenum = (ea,eb,ec);
      tc = class
       private
        fmybool: boolean;
        fenum: tenum;
       published
        property mybool: boolean read fmybool write fmybool default false;
        property enum: tenum read fenum write fenum default ea;
      end;
    

### db unit: TField*.*Size type

  * **Old behaviour** : TFieldDef.Size, TField.Size and TField.DataSize were defined as word.
  * **New behaviour** : TFieldDef.Size, TField.Size and TField.DataSize are defined as integers. This also means that definition of the corresponding methods has changed.
  * **Example** : The following definitions of methods have been changed: 
    * constructor TFieldDef.Create(AOwner: TFieldDefs; const AName: string; ADataType: TFieldType; ASize: **Integer** ; ARequired: Boolean; AFieldNo: Longint)
    * procedure TFieldDef.SetSize(const AValue: **integer**);
    * function TField.GetDataSize: **integer** ;
    * procedure TField.SetSize(AValue: **integer**);
  * **Reason** : Delphi compatibility.
  * **Remedy** : Change the definition of these properties in your methods that override one of the methods above from word to integer.



### db unit: TField*.Lookup property is not published anymore

  * **Old behaviour** : TField.Lookup was published, so it was streamed if it was set to True (i.e. for a fkLookup field)
  * **New behaviour** : TField.Lookup is not published anymore, so if you try to read back its streamed value (e.g., loading Lazarus forms) it will fail.
  * **Reason** : The value of this property was also modified by setting FieldKind, which is also streamed. See [Bug 12809](<http://bugs.freepascal.org/view.php?id=12809>)
  * **Remedy** : If you use Lazarus, remove references to _Lookup_ properties in your project's lfm files.



## All Unix-based systems

### Executing external programs using TProcess

  * **Old behaviour** : When TProcess was used to execute a program and no explicit path to the executable was given, TProcess searched for the executable in the current directory, and only if it was not found there it looked in the system path.
  * **New behaviour** : On all Unix-based systems, TProcess will not search for the executable in the current directory anymore, unless the current directory is in the system path.
  * **Example** : When you use TProcess to execute 'ls' and there is an 'ls' executable in both the current directory and in '/bin/', then programs compiled with FPC 2.2.2 will execute './ls' (thus in the current directory). Programs compiled with FPC 2.2.4 will run '/bin/ls' instead.
  * **Reason** : Security. Unix-users don't expect that files in the current directory are executed in favour of those in the system path.
  * **Remedy** : If you really want to execute an executable in the current directory, use './exec-name' as executable name.



## Non-x86 and non-linux/ppc64 systems

### Floating point rounding

  * **Old behaviour** : On these platforms, the _round()_ function always rounded to the nearest integer, halfway away from zero.
  * **New behaviour** : By default, _round()_ now performs _banker's rounding_ (round-to-nearest, halfway to even). If the rounding mode is changed using the _Math_ unit's _SetRoundMode()_ function, the _round()_ function now honours this change.
  * **Example** :


    
    
    begin
      write(round(-2.5),' ');
      write(round(-1.5),' ');
      write(round(-0.5),' ');
      write(round(0.5),' ');
      write(round(1.5),' ');
      writeln(round(2.5));
    end.
    

The above program used to print: 
    
    
     -3 -2 -1 1 2 3
    

Now it will print: 

  

    
    
     -2 -2 0 0 2 2
    

  * **Reason** : Compatibility with TP/Delphi.
  * **Remedy** : If you want to use other kinds of rounding, you can change the rounding mode using _Math.SetRoundMode()_ , or use John Herbster's unit attached to [bug report 12687](<http://bugs.freepascal.org/view.php?id=12687>).



## Windows Platforms

### Functions removed from the Windows unit

  * **Old behaviour** : The following functions were declared in the Windows unit (including _A_ and _W_ variants where applicable): 
    * comdlg32.dll: 
      * _ChooseColor_
      * _ChooseFont_
      * _CommDlgExtendedError_
      * _CreateStatusWindow_
      * _FindText_
      * _GetFileTitle_
      * _GetOpenFileName_
      * _GetSaveFileName_
      * _PageSetupDlg_
      * _PrintDlg_
      * _ReplaceText_
    * comctl32.dll (and some helper routines): 
      * _CreateMappedBitmap_
      * _CreatePropertySheetPage_
      * _CreateToolbarEx_
      * _CreateUpDownControl_
      * _DrawInsert_
      * _DrawStatusText_
      * _GetEffectiveClientRect_
      * _ImageList_Add_
      * _ImageList_AddIcon_
      * _ImageList_AddMasked_
      * _ImageList_BeginDrag_
      * _ImageList_Create_
      * _ImageList_Destroy_
      * _ImageList_DragEnter_
      * _ImageList_DragLeave_
      * _ImageList_DragMove_
      * _ImageList_DragShowNolock_
      * _ImageList_Draw_
      * _ImageList_DrawEx_
      * _ImageList_EndDrag_
      * _ImageList_GetBkColor_
      * _ImageList_GetDragImage_
      * _ImageList_GetIcon_
      * _ImageList_GetIconSize_
      * _ImageList_GetImageCount_
      * _ImageList_GetImageInfo_
      * _ImageList_LoadImage_
      * _ImageList_Merge_
      * _ImageList_Remove_
      * _ImageList_Replace_
      * _ImageList_ReplaceIcon_
      * _ImageList_SetBkColor_
      * _ImageList_SetDragCursorImage_
      * _ImageList_SetIconSize_
      * _ImageList_SetImageCount_
      * _ImageList_SetOverlayImage_
      * _LBItemFromPt_
      * _MakeDragList_
      * _MenuHelp_
      * _PropertySheet_
      * _ShowHideMenuCtl_
  * **New behaviour** : These function have been removed from the Windows unit.
  * **Reason** : Delphi compatibility, remove definitions of functions also declared in the _commdlg_ and _commctrl_ units.
  * **Remedy** : Add the _commdlg_ and/or _commctrl_ unit(s) to your uses clause.



## Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | 2.2.4 | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_2.2.4](https://web.archive.org/web/20241227114003/https://wiki.freepascal.org/User_Changes_2.2.4)_
