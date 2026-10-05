# Cocoa Internals/Dialogs

## Contents

  * 1 Common Considerations
    * 1.1 Modality
    * 1.2 No restoration
    * 1.3 Class Override
    * 1.4 No Tags
    * 1.5 File Type Selection
    * 1.6 File Type Filters
  * 2 File Dialog Options
  * 3 See Also



## Common Considerations

### Modality

None of the Cocoa dialogs (OpenFile, SaveFile, Font or Color) are modal! 

This means that one can open a dialog and keep using the other application windows and the application menu. 

This doesn't work well with the LCL design, where each dialog is opened by an **Execute** method which doesn't return until the modal dialog is closed. 

For this particular reason, Cocoa WS does the following: 

  * every dialog is called as the application modal window (preventing other windows of the application from getting any user input);
  * the application menu is disabled, until the dialog is closed;
  * where applicable, "Ok" and "Cancel" buttons are added to dialogs to indicate to a user that the dialog can be closed to make the selection.



### No restoration

Starting with macOS 10.7 Cocoa will attempt to restore the Application windows between Launches. 

It's a nice user feature, which conflicts with LCL. Since LCL adds custom controls to a dialog, and the restoration requires a special "restorationClass" to be provided. 

Instead of restoring dialogs, CocoaWS explicitly forbids the restoration of them. 

### Class Override

Unlike NSApplication, dialog classes cannot be overwritten in the following manner: 
    
    
     TCocoaColorPanel.sharedPanel
    

Doing that will cause problems using class methods of the sharedPanel (and likely something else) 

### No Tags

macOS system is using "tags" on files. It seems to be a file system feature that's used commonly in macOS. 

LCL doesn't have a concept of tags. 

By default Save dialog is showing a user an ability to select a tag. Since LCL cannot handle it properly (it cannot return the value, nor it has APIs to save the value), the dialog hides the tag selection, by default. 

### File Type Selection

There's no "native" file type selection in macOS dialogs. Instead, there's an opportunity to add "custom options" to an open/save dialog, known as **accessory view**. 

The accessory view is used to place the run-time file type selection into the open/save dialog. 

### File Type Filters

There are two ways of filtering out files that can be opened. (Note that "filtering types" is not used when using Save dialog.) 

1) Assign the list of file extensions or file types (UTI's) via the **setAllowedFileTypes:** method. Allowing "any" file can be achieved by using the **setAllowsOtherFileTypes:** method. Considerations: 

    

  * a filter is assigned by "generic type" rather than extension. And thus specifying "txt" as a filter, will assign "public.plain-text" type. Which might correspond to other file extensions other than "txt" file. (i.e. ".pas" files might also be allowed, as they are part of "public.plain-text" group).



    

  * setAllowedFileTypes - is unable to verify compound extensions, such as "tar.gz". In which case the filter configured for ".tag.gz" and a filter configured for ".gz" will provide the exactly the same set of files for selection.



    As a result of you should not rely on FilterIndex returned. Instead, you should verify the actual contents of the file selected.

2) Perform the file check manually through the **panel:shouldEnableURL:** method. Considerations: 

    

  * starting with macOS 10.15, there's a security change done in macOS. Save/Open file dialogs are executed in a separate process. Passing the information between the main process and the dialog process introduces a certain lag. If a great number of files, it becomes noticeable for the user. Thus it's possible that not all files might be "available" for the selection as the check runs slowly. (see [issue #38399](<https://bugs.freepascal.org/view.php?id=38399>))



A developer can control what method is used in the application by changing the global variable **CocoaUseUTIFilter** at CocoaWSDialogs unit. 

  * CocoaUseUTIFilter = false (default) - "panel:shouldEnableURL" is used
  * CocoaUseUTIFilter = true - macOS UTI method is used


    
    
    uses
      ..
     {$ifdef LCLCOCOA} 
     ,CocoaWSDialogs
     {$endif}
      ...
    
    initialization
     {$ifdef LCLCOCOA} 
     CocoaUseUTIFilter := true;    
     {$endif}
    

## File Dialog Options

Some of the options are not supported, please refer to the table below for the implementation details 

Options  | Supported  | Description   
---|---|---  
ofReadOnly  | No  |   
ofOverwritePrompt  | No  | In macOS prior to 10.15 it was possible to implement the feature by tricking the NSSaveDialog when selecting the name. However, this ability was removed in 10.15 (due to security considerations?) So now, overwrite prompt will always show up   
ofHideReadOnly  | No  | There's no concept of "readOnly" files in macOS filesystems. There's also no configuration in the dialog, to hide files   
ofNoChangeDir  | No  |   
ofShowHelp  | No  |   
ofNoValidate  | No  |   
ofAllowMultiSelect  | Yes  | NSOpenPanel.setAllowsMultipleSelection()   
ofExtensionDifferent,  | No  |   
ofPathMustExist  | No  |   
ofFileMustExist  | No  |   
ofCreatePrompt  | No  |   
ofShareAware  | No  | System native dialogs are always share aware OR act within sandbox allowances   
ofNoReadOnlyReturn  | No  | No concept of "readonly" file   
ofNoTestFileCreate  | No  |   
ofNoNetworkButton  | No  | All shares and networks are available or restricted by SandBox limitations   
ofNoLongNames  | No  | Any file can have long name. OSX never suffered from 8.3 limitation   
ofOldStyleDialog  | No  | There's no old style on macOS ...but, there's an opportunity to use in order to allow some emulation   
ofNoDereferenceLinks  | No  | Links are treated by the system.   
ofNoResolveLinks  | No  | Links are treated by the system   
ofEnableIncludeNotify  | No  |   
ofEnableSizing  | No  | Dialogs are always sizable to the macOS rules   
ofDontAddToRecent  | No  |   
ofForceShowHidden  | Yes  | NSSaveDialog.setShowsHiddenFiles()   
ofViewDetail  | No  |   
ofAutoPreview  | No  |   
\-  | (default)  | canCreateDirectries The feature controls, if a user can create directories within the dialog. Since Windows dialogs allows creation of folders, macOS LCL dialogs also allows the creation at all times.   
\-  | (default)  | extensionHidden The name of the file, selected by the user hides the extension. Since on Windows there's no control over how the file is displayed. The property is left to its system default.   
\-  | (default)  | treatsFilePackagesAsDirectories If set to true, a user can treat (Application) bundles as directories and navigate into the bundle. There's no such concept in LCL, the value is at system default   
  
## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Dialogs](https://web.archive.org/web/20240914150418/https://wiki.freepascal.org/Cocoa_Internals/Dialogs)_
