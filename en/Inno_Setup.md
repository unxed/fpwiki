# Inno Setup Usage

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/60px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Inno Setup ([home](<http://www.jrsoftware.org/isinfo.php>)) is a program that lets you create Windows installers for your programs. It is coded in Delphi and has Unicode support. It is one of the most often used programs to create setup files (alongside with nsis). The advantage of Inno Setup is that it supports Pascal Scripting for more advanced tasks (although the built in capabilities are already very extensive). 

Like nsis, Inno Setup uses script files (.iss) to indicate what files to install and where. While you can create these with a simple text editor, there are some IDEs for Inno Setup that provide easier editing. 

Inno Setup comes with extensive help on the syntax of the script file. 

## Contents

  * 1 Features
  * 2 Inno Setup for Linux/macOS?
  * 3 Download
  * 4 Examples
    * 4.1 Full example
  * 5 File Association
  * 6 Default Programs



## Features

Features: 

  * Inno supports all versions of Windows in use today -- Windows 95, 98, ME, NT4, 2000, 2003, XP, Vista, 7, 8 and 10. The latest version that can create installers for Windows versions before Windows 2000 is version 5.4. See [[1]](<http://www.jrsoftware.org/files/is5-whatsnew.htm>)
  * It can compare file version info, replace in-use files, use shared file counting.
  * It can register DLL/OCXs and type libraries
  * It can install fonts.
  * It can create shortcuts anywhere, including in the Start Menu and on the desktop.
  * It can create registry and .INI entries
  * It has integrated Pascal scripting engine that allows e.g. manipulation of COM/ActiveX objects (e.g. MS Word), setting firewall rules etc.
  * It supports multilingual installs
  * It supports passworded and encrypted installs
  * It supports silent install and uninstall.



## Inno Setup for Linux/macOS?

Occassionally, on forums and mailing lists, people ask why there is no Inno Setup for macOS or Linux. However, many people agree that you should use the distribution package manager format (e.g. .rpm, .deb) for Linux and the standard macOS way (e.g. .dmg disk images with the Apple-provided tools) for that; see [this article for details](<Deploying_Your_Application.md> "Deploying Your Application"). See e.g. [this forum discussion](<http://www.lazarus.freepascal.org/index.php/topic,14789.msg78576.html>) for an example. 

## Download

[Download Inno Setup Quick Start Pack](<http://www.jrsoftware.org/isdl.php#qsp>) is the recommended download. It contains Inno Setup, some IDEs and a preprocessor. 

## Examples

  * The Lazarus setup for Windows was created with Inno Setup. This script includes PascalScript sections used to customize the installer, which may serve as inspiration for your own installers.


  * [LazPaint](<LazPaint.md> "LazPaint") setup was also created with it.



Both scripts have File Association support and are listed in Default Programs to restore the file associations if needed (Windows 7). 

Below we show some example of Inno Setup usage. Please have a look at their extensive help files and web site for information on how to perform other tasks. 

### Full example

This example is full of things: Translations, File Associations, Default Programs, Right-click with Cascade-Menu, Add to Open With list, Add to 'App Paths' (to execute application from Windows+R 'Run' window, from batch files or by code just typing the executable name (lazpaint.exe in this case). 
    
    
    #define MyAppName "LazPaint"
    #define MyAppOutputName "lazpaint"
    #define MyInstallerSuffix "_setup_win32_win64"
    #define MyAppVersion "6.4.1"
    #define MyAppPublisher "Circular, Fabien Wang, Lainz and others"
    #define MyAppURL "http://sourceforge.net/projects/lazpaint/"
    #define MyAppExeName "lazpaint.exe"
    #define ReleaseDir "release\"
    #define BasicSubCommands "LazPaint.horizontalflip;LazPaint.verticalflip;Windows.separator;LazPaint.rotatecw;LazPaint.rotateccw;Windows.separator;LazPaint.swapredblue"
    #define EffectsSubCommands "LazPaint.sharpen;LazPaint.smooth;LazPaint.median;LazPaint.cleartype;LazPaint.cleartypeinverse;LazPaint.emboss;LazPaint.contour;LazPaint.grayscale;LazPaint.negative;LazPaint.linearnegative;LazPaint.complementarycolor;LazPaint.normalize;LazPaint.smartzoom3"
    
    [Setup]
    AppId={{A177F82E-B44A-4348-A265-3D1C089D6304}
    AppName={#MyAppName}
    AppVersion={#MyAppVersion}
    AppPublisher={#MyAppPublisher}
    AppPublisherURL={#MyAppURL}
    AppSupportURL={#MyAppURL}
    AppUpdatesURL={#MyAppURL}
    ChangesAssociations=yes
    DefaultDirName={pf}\{#MyAppName}
    DefaultGroupName={#MyAppName}
    AllowNoIcons=yes
    OutputDir=installer
    OutputBaseFilename={#MyAppOutputName}{#MyAppVersion}{#MyInstallerSuffix}
    Compression=lzma2/ultra64
    InternalCompressLevel=ultra
    SolidCompression=yes
    CompressionThreads=2
    ArchitecturesInstallIn64BitMode=x64
    WizardImageFile=lazpaint_setup.bmp
    WizardSmallImageFile=lazpaint_setup_icon.bmp
    WizardImageBackColor=clWhite
    WizardImageStretch=False
    ;The next options are to do a quick setup with less dialogs
    ;If program already installed only show tasks dialog
    ;ShowLanguageDialog=no
    ;DisableWelcomePage=yes
    ;DisableDirPage=auto
    ;DisableProgramGroupPage=auto
    ;DisableReadyPage=yes
    ;DisableFinishedPage=yes
    
    [Code]
    // Start - Quick Setup //
    {procedure CurPageChanged(CurPageID: Integer);
    begin
      if CurPageID = wpSelectTasks then
        WizardForm.NextButton.Caption := SetupMessage(msgButtonInstall)
      else if CurPageID = wpFinished then
        WizardForm.NextButton.Caption := SetupMessage(msgButtonFinish)
      else
        WizardForm.NextButton.Caption := SetupMessage(msgButtonNext);
    end;}
    // End - Quick Setup //
    
    [Languages]
    Name: "english"; MessagesFile: "compiler:Default.isl"
    Name: "brazilianportuguese"; MessagesFile: "compiler:Languages\BrazilianPortuguese.isl"
    Name: "catalan"; MessagesFile: "compiler:Languages\Catalan.isl"
    Name: "corsican"; MessagesFile: "compiler:Languages\Corsican.isl"
    Name: "czech"; MessagesFile: "compiler:Languages\Czech.isl"
    Name: "danish"; MessagesFile: "compiler:Languages\Danish.isl"
    Name: "dutch"; MessagesFile: "compiler:Languages\Dutch.isl"
    Name: "finnish"; MessagesFile: "compiler:Languages\Finnish.isl"
    Name: "french"; MessagesFile: "compiler:Languages\French.isl"
    Name: "german"; MessagesFile: "compiler:Languages\German.isl"
    Name: "greek"; MessagesFile: "compiler:Languages\Greek.isl"
    Name: "hebrew"; MessagesFile: "compiler:Languages\Hebrew.isl"
    Name: "hungarian"; MessagesFile: "compiler:Languages\Hungarian.isl"
    Name: "italian"; MessagesFile: "compiler:Languages\Italian.isl"
    Name: "japanese"; MessagesFile: "compiler:Languages\Japanese.isl"
    Name: "norwegian"; MessagesFile: "compiler:Languages\Norwegian.isl"
    Name: "polish"; MessagesFile: "compiler:Languages\Polish.isl"
    Name: "portuguese"; MessagesFile: "compiler:Languages\Portuguese.isl"
    Name: "russian"; MessagesFile: "compiler:Languages\Russian.isl"
    Name: "serbiancyrillic"; MessagesFile: "compiler:Languages\SerbianCyrillic.isl"
    Name: "serbianlatin"; MessagesFile: "compiler:Languages\SerbianLatin.isl"
    Name: "slovenian"; MessagesFile: "compiler:Languages\Slovenian.isl"
    Name: "spanish"; MessagesFile: "compiler:Languages\Spanish.isl"
    Name: "ukrainian"; MessagesFile: "compiler:Languages\Ukrainian.isl"
    
    [CustomMessages]
    ; English
    english.LazPaintDescription=This program is designed to draw like with Paint.Net and to experiment this kind of programming with Lazarus.
    ; English.AssocFile
    english.AssocFileLZP=LazPaint image
    english.AssocFileORA=OpenRaster format
    english.AssocFileBMP=Bitmap
    english.AssocFilePNG=Portable Network Graphic
    english.AssocFileJPG=JPEG
    english.AssocFilePDN=Paint.NET image
    english.AssocFileGIF=Animated GIF
    english.AssocFileICO=Icon
    english.AssocFilePCX=Personal Computer eXchange
    ; English.Menu
    english.QuickEdit=Quick Edit
    english.QuickEffects=Quick Effects
    ; English.QuickEdit
    english.Open=Open
    english.HorizontalFlip=Horizontal Flip
    english.VerticalFlip=Vertical Flip
    english.SwapRedBlue=Swap Red and Blue Channels
    english.RotateCW=Rotate Clock Wise
    english.RotateCCW=Rotate Counter Clock Wise
    ; English.QuickEffects
    english.Sharpen=Sharpen
    english.Smooth=Smooth
    english.Median=Median
    english.ClearType=Clear Type
    english.ClearTypeInverse=Clear Type Inverse
    english.Emboss=Emboss
    english.Contour=Contour
    english.Grayscale=Grayscale
    english.Negative=Negative
    english.LinearNegative=Linear Negative
    english.ComplementaryColor=Complementary Color
    english.Normalize=Normalize
    english.SmartZoom3=SmartZoom3
    
    ; Spanish
    spanish.LazPaintDescription=Este programa esta diseñado para dibujar como Paint.Net y para experimentar este tipo de programación con Lazarus.
    ; Spanish.AssocFile
    spanish.AssocFileLZP=Imágen LazPaint
    spanish.AssocFileORA=Formato OpenRaster
    spanish.AssocFileBMP=Mapa de Bits
    spanish.AssocFilePNG=Portable Network Graphic
    spanish.AssocFileJPG=JPEG
    spanish.AssocFilePDN=Imágen Paint.NET
    spanish.AssocFileGIF=GIF Animado
    spanish.AssocFileICO=Icono
    spanish.AssocFilePCX=Personal Computer eXchange
    ; Spanish.Menu
    spanish.QuickEdit=Edición Rápida
    spanish.QuickEffects=Efectos Rápidos
    ; Spanish.QuickEdit
    spanish.Open=Abrir
    spanish.HorizontalFlip=Voltear Horizontalmente
    spanish.VerticalFlip=Voltear Verticalmente
    spanish.SwapRedBlue=Intercambiar Canales Rojo y Azul
    spanish.RotateCW=Rotar en Sentido Horario
    spanish.RotateCCW=Rotar en Sentido Antihorario
    ; Spanish.QuickEffects
    spanish.Sharpen=Sharpen
    spanish.Smooth=Smooth
    spanish.Median=Median
    spanish.ClearType=Clear Type
    spanish.ClearTypeInverse=Clear Type Inverse
    spanish.Emboss=Emboss
    spanish.Contour=Contour
    spanish.Grayscale=Grayscale
    spanish.Negative=Negative
    spanish.LinearNegative=Linear Negative
    spanish.ComplementaryColor=Complementary Color
    spanish.Normalize=Normalize
    spanish.SmartZoom3=SmartZoom3
    
    [Tasks]
    Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"
    Name: "assoc_lzp"; Description: "{cm:AssocFileExtension,{#MyAppName},.lzp}"
    Name: "assoc_ora"; Description: "{cm:AssocFileExtension,{#MyAppName},.ora}"
    Name: "assoc_pdn"; Description: "{cm:AssocFileExtension,{#MyAppName},.pdn}"; Flags: unchecked
    Name: "assoc_bmp"; Description: "{cm:AssocFileExtension,{#MyAppName},.bmp}"; Flags: unchecked
    Name: "assoc_pcx"; Description: "{cm:AssocFileExtension,{#MyAppName},.pcx}"; Flags: unchecked
    Name: "assoc_png"; Description: "{cm:AssocFileExtension,{#MyAppName},.png}"; Flags: unchecked
    Name: "assoc_jpg"; Description: "{cm:AssocFileExtension,{#MyAppName},.jpg}"; Flags: unchecked
    Name: "assoc_gif"; Description: "{cm:AssocFileExtension,{#MyAppName},.gif}"; Flags: unchecked
    Name: "assoc_ico"; Description: "{cm:AssocFileExtension,{#MyAppName},.ico}"; Flags: unchecked
    
    [Files]
    Source: "{#ReleaseDir}lazpaint32.exe"; DestDir: "{app}"; DestName: "{#MyAppExeName}"; Flags: ignoreversion; Check: not Is64BitInstallMode
    Source: "{#ReleaseDir}lazpaint_x64.exe"; DestDir: "{app}"; DestName: "{#MyAppExeName}"; Flags: ignoreversion; Check: Is64BitInstallMode
    Source: "{#ReleaseDir}i18n\*.po"; DestDir: "{app}\i18n"; Excludes: "i18n\lazpaint_x64.po"; Flags: ignoreversion
    Source: "{#ReleaseDir}models\*.*"; DestDir: "{app}\models"; Flags: ignoreversion
    Source: "{#ReleaseDir}readme.txt"; DestDir: "{app}"; Flags: ignoreversion
    
    [Icons]
    Name: "{group}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Comment: "LazPaint"
    Name: "{group}\{cm:ProgramOnTheWeb,{#MyAppName}}"; Filename: "{#MyAppURL}"
    Name: "{group}\{cm:UninstallProgram,{#MyAppName}}"; Filename: "{uninstallexe}"
    Name: "{commondesktop}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Comment: "LazPaint"; Tasks: desktopicon
    
    [Run]
    Filename: "{app}\{#MyAppExeName}"; Description: "{cm:LaunchProgram,{#StringChange(MyAppName, "&", "&&")}}"; Flags: nowait postinstall skipifsilent
    
    [Registry]
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp"; ValueType: String; ValueData: "{cm:AssocFileLZP}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.lzp\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora"; ValueType: String; ValueData: "{cm:AssocFileORA}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ora\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp"; ValueType: String; ValueData: "{cm:AssocFileBMP}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png"; ValueType: String; ValueData: "{cm:AssocFilePNG}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.png\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg"; ValueType: String; ValueData: "{cm:AssocFileJPG}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.jpg\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn"; ValueType: String; ValueData: "{cm:AssocFilePDN}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pdn\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif"; ValueType: String; ValueData: "{cm:AssocFileGIF}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: "{#BasicSubCommands}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.gif\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico"; ValueType: String; ValueData: "{cm:AssocFileICO}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: {#BasicSubCommands}; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.ico\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx"; ValueType: String; ValueData: "{cm:AssocFilePCX}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEdit}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu\"; ValueType: String; ValueName: SubCommands; ValueData: {#BasicSubCommands}; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu2\"; ValueType: String; ValueName: MUIVerb; ValueData: "{cm:QuickEffects}"; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu2\"; ValueType: String; ValueName: SubCommands; ValueData: "{#EffectsSubCommands}"; Flags: uninsdeletekey
    ; Windows XP compatibility
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.pcx\Shell\CascadeMenu2\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    ; BasicSubCommands methods
    ; Open
    ;Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.open"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Open}; Flags: uninsdeletekey
    ;Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.open\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    ; HorizontalFlip
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.horizontalflip"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:HorizontalFlip}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.horizontalflip\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/HorizontalFlip"" ""%1"""; Flags: uninsdeletekey
    ; VerticalFlip
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.verticalflip"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:VerticalFlip}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.verticalflip\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/VerticalFlip"" ""%1"""; Flags: uninsdeletekey
    ; SwapRedBlue
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.swapredblue"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:SwapRedBlue}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.swapredblue\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/SwapRedBlue"" ""%1"""; Flags: uninsdeletekey
    ; RotateCW
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.rotatecw"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:RotateCW}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.rotatecw\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/RotateCW"" ""%1"""; Flags: uninsdeletekey
    ; RotateCCW
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.rotateccw"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:RotateCCW}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.rotateccw\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/RotateCCW"" ""%1"""; Flags: uninsdeletekey
    
    ; EffectsSubCommands methods
    ; Sharpen
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.sharpen"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Sharpen}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.sharpen\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Sharpen"" ""%1"""; Flags: uninsdeletekey
    ; Smooth
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.smooth"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Smooth}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.smooth\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Smooth"" ""%1"""; Flags: uninsdeletekey
    ; Median
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.median"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Median}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.median\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Median"" ""%1"""; Flags: uninsdeletekey
    ; ClearType
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.cleartype"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:ClearType}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.cleartype\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/ClearType"" ""%1"""; Flags: uninsdeletekey
    ; ClearTypeInverse
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.cleartypeinverse"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:ClearTypeInverse}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.cleartypeinverse\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/ClearTypeInverse"" ""%1"""; Flags: uninsdeletekey
    ; Emboss
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.emboss"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Emboss}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.emboss\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Emboss"" ""%1"""; Flags: uninsdeletekey
    ; Contour
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.contour"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Contour}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.contour\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Contour"" ""%1"""; Flags: uninsdeletekey
    ; Grayscale
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.grayscale"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Grayscale}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.grayscale\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Grayscale"" ""%1"""; Flags: uninsdeletekey
    ; Negative
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.negative"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Negative}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.negative\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Negative"" ""%1"""; Flags: uninsdeletekey
    ; LinearNegative
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.linearnegative"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:LinearNegative}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.linearnegative\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/LinearNegative"" ""%1"""; Flags: uninsdeletekey
    ; ComplementaryColor
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.complementarycolor"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:ComplementaryColor}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.complementarycolor\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/ComplementaryColor"" ""%1"""; Flags: uninsdeletekey
    ; Normalize
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.normalize"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:Normalize}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.normalize\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/Normalize"" ""%1"""; Flags: uninsdeletekey
    ; SmartZoom3
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.smartzoom3"; ValueName: MUIVerb; ValueType: String; ValueData: {cm:SmartZoom3}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\LazPaint.smartzoom3\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"" ""/SmartZoom3"" ""%1"""; Flags: uninsdeletekey
    
    ; Real file association
    Root: HKLM; Subkey: "Software\Classes\.bmp"; ValueType: String; ValueData: "LazPaint.AssocFile.bmp"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_bmp
    Root: HKLM; Subkey: "Software\Classes\.lzp"; ValueType: String; ValueData: "LazPaint.AssocFile.lzp"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_lzp
    Root: HKLM; Subkey: "Software\Classes\.ora"; ValueType: String; ValueData: "LazPaint.AssocFile.ora"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_ora
    Root: HKLM; Subkey: "Software\Classes\.png"; ValueType: String; ValueData: "LazPaint.AssocFile.png"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_png
    Root: HKLM; Subkey: "Software\Classes\.jpg"; ValueType: String; ValueData: "LazPaint.AssocFile.jpg"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_jpg
    Root: HKLM; Subkey: "Software\Classes\.pdn"; ValueType: String; ValueData: "LazPaint.AssocFile.pdn"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_pdn
    Root: HKLM; Subkey: "Software\Classes\.gif"; ValueType: String; ValueData: "LazPaint.AssocFile.gif"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_gif
    Root: HKLM; Subkey: "Software\Classes\.ico"; ValueType: String; ValueData: "LazPaint.AssocFile.ico"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_ico
    Root: HKLM; Subkey: "Software\Classes\.pcx"; ValueType: String; ValueData: "LazPaint.AssocFile.pcx"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_pcx
    
    ; Control panel
    Root: HKLM; Subkey: "Software\LazPaint"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities"; ValueType: String; ValueName: "ApplicationName"; ValueData: "LazPaint"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities"; ValueType: String; ValueName: "ApplicationDescription"; ValueData: {cm:LazPaintDescription}; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".lzp"; ValueType: String; ValueData: "LazPaint.AssocFile.lzp"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".ora"; ValueType: String; ValueData: "LazPaint.AssocFile.ora"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".bmp"; ValueType: String; ValueData: "LazPaint.AssocFile.bmp"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".png"; ValueType: String; ValueData: "LazPaint.AssocFile.png"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".jpg"; ValueType: String; ValueData: "LazPaint.AssocFile.jpg"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".pdn"; ValueType: String; ValueData: "LazPaint.AssocFile.pdn"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".gif"; ValueType: String; ValueData: "LazPaint.AssocFile.gif"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".ico"; ValueType: String; ValueData: "LazPaint.AssocFile.ico"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".pcx"; ValueType: String; ValueData: "LazPaint.AssocFile.pcx"; Flags: uninsdeletekey
    
    Root: HKLM; Subkey: "Software\RegisteredApplications"; ValueType: String; ValueName: "LazPaint"; ValueData: "Software\LazPaint\Capabilities"; Flags: uninsdeletevalue uninsdeletekeyifempty
    
    ; Open With...
    Root: HKLM; Subkey: "Software\Classes\Applications\lazpaint.exe\shell\open\command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    
    ; App Paths (ShellExecuteEx)
    Root: HKLM; Subkey: "SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\lazpaint.exe"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"""; Flags: uninsdeletekey
    Root: HKLM; Subkey: "SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\lazpaint.exe"; ValueType: String; ValueName: Path; ValueData: """{app}"""; Flags: uninsdeletekey
    

## File Association

The file association code is in the [Registry] section of the script. 

The first thing we need is each type of association. In the example below, the type is for .bmp files. You can see the first line is for the description of the association 'Bitmap'. The second line is the icon for .bmp files in the explorer, the third is the icon for the right clic menu on .bmp files in the explorer, the last is the parameter to open the file. 
    
    
    [Registry]
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp"; ValueType: String; ValueData: "Bitmap"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\DefaultIcon"; ValueType: String; ValueData: "{app}\{#MyAppExeName},0"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\Open"; ValueName: Icon; ValueType: String; ValueData: "{app}\{#MyAppExeName}"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\Classes\LazPaint.AssocFile.bmp\Shell\Open\Command"; ValueType: String; ValueData: """{app}\{#MyAppExeName}"" ""%1"""; Flags: uninsdeletekey
    

That code doesn't associate anything, the following code is to associate our type 'LazPaint.AssocFile.bmp' with '.bmp'. It is only executed when the task assoc_bmp is selected. Read about tasks in the Inno Setup help. 
    
    
    Root: HKLM; Subkey: "Software\Classes\.bmp"; ValueType: String; ValueData: "LazPaint.AssocFile.bmp"; Flags: uninsdeletevalue uninsdeletekeyifempty; Tasks: assoc_bmp
    

That's all. Now we have associated .bmp with our program (LazPaint in the example). 

If you want to setup file associations from your program instead of a setup program use the [FileAssociation](<FileAssociation.md> "FileAssociation") component instead. 

## Default Programs

"Default Programs" is used to list the applications that have file association support. Values are the name of the program, a short description and each one of the extensions for which the program has support. 
    
    
    Root: HKLM; Subkey: "Software\LazPaint"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities"; ValueType: String; ValueName: "ApplicationName"; ValueData: "LazPaint"; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities"; ValueType: String; ValueName: "ApplicationDescription"; ValueData: "A short description..."; Flags: uninsdeletekey
    Root: HKLM; Subkey: "Software\LazPaint\Capabilities\FileAssociations"; ValueName: ".bmp"; ValueType: String; ValueData: "LazPaint.AssocFile.bmp"; Flags: uninsdeletekey
    

Finally we need to add the program to the 'Registered Applications': 
    
    
    Root: HKLM; Subkey: "Software\RegisteredApplications"; ValueType: String; ValueName: "LazPaint"; ValueData: "Software\LazPaint\Capabilities"; Flags: uninsdeletevalue uninsdeletekeyifempty

---

_Source: [https://wiki.freepascal.org/Inno_Setup](https://web.archive.org/web/20250325132337/https://wiki.freepascal.org/Inno_Setup)_
