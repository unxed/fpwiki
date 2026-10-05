# LAMW install windows fpcupdeluxe

## Install LAMW and Android Development Environment on Windows using [fpcupdeluxe](<fpcupdeluxe.md>)

See also
    
    
    
    Forum Post: <https://forum.lazarus.freepascal.org/index.php?topic=59226>
    For Linux How-To see here: [https://wiki.freepascal.org/LAMW_install_linux_fpcupdeluxe](<LAMW_install_linux_fpcupdeluxe.md>)
    
    
    
    [https://wiki.freepascal.org/LAMW](<LAMW.md>)
    [https://wiki.freepascal.org/fpcupdeluxe](<fpcupdeluxe.md>)
    
    
    
    As of 2023-Feb-06, this error is still happening:
    <https://forum.lazarus.freepascal.org/index.php/topic,21919.msg441567.html#msg441567>
    

Motivation
    
    
    Needed simple instructions to get an up-to-date Android build environment setup on Windows.
    (installs and runs as a user, no admin privileges are needed)
    Thanks to other developers for sharing their hard work, utilities, and helpful information.
    

Everything is self-contained under the "$BASE\android" folder
    
    
    ($BASE defaults to the user's home folder)
    $BASE\android\sdk  (android sdk and build tools)
    $BASE\android\sdk\ndk-bundle  (android ndk)
    $BASE\android\fpcupdeluxe_app  (FPCUPdeluxe installer)
    $BASE\android\projects  (default LAMW projects folder)
    $BASE\android\fpc-lazarus  (FPC, Lazarus built w/ LAMW)
    $BASE\android\fpc-lazarus\ccr\lamw  (LAMW files)
    $BASE\android\fpc-lazarus\ccr\lamw-gradle  (gradle build tool)
    $BASE\android\fpc-lazarus\ccr\lamw\demos (LAMW demo projects)
    

Versions Tested
    
    
    Windows 10 Pro 22H2 64-bit with latest updates installed
    
    Android Command-Line Tools for Linux build 9477386
    OpenJDK 11.0.18 LTS (Microsoft's build)
    Android SDK/API Level 31 (Android 12)
    Android NDK revision 22
    Gradle 6.8.3 (installed automatically by FPCUPdeluxe)
    
    FPCUPdeluxe ver 2.2.0n
    FPC stable v3.2.2
    Lazarus stable v2.2.4
    
    32-bit CPU Target: ARMv7a + VFPv3 (hardware floating point)
    64-bit CPU Target: Aarch64 (this always uses hardware floating point)
    

## Main steps begin here
    
    
    Run these commands in a non-admin powershell console.  I recommend running these
    one at a time so you can verify the results before moving on to the next step.
    
    
    
    # (powershell code)
    # Setup Android Development Environment on Windows (FPCUPdeluxe / LAMW)
    # Tested on Windows 10 Pro 22H2 64-bit
    # Targets: 32-bit ARMv7a and 64-bit Aarch64
    # 2023-Feb-05 ver 0.7
    
    $BASE = "$HOME" #change this if you want to install to a different location
    $ProgressPreference = 'SilentlyContinue'  # hide progress for faster downloads
    $PSDefaultParameterValues=@{'out-file:encoding'='ascii'}  #set text redirection to file as ascii
    
    # Install OpenJDK
    # https://docs.microsoft.com/en-us/java/openjdk/download
    md "$BASE\android"
    cd "$BASE\android"
    Invoke-WebRequest "https://aka.ms/download-jdk/microsoft-jdk-11.0.18-windows-x64.zip" -OutFile "microsoft-jdk-Win-x64.zip"
    Expand-Archive "microsoft-jdk-Win-x64.zip" ".\"
    del "microsoft-jdk-Win-x64.zip"
    
    # Setup java environment for current process
    ${env:JAVA_HOME} = "$BASE\android\jdk-11.0.18+10"
    ${env:Path} = "${env:JAVA_HOME}\bin;" + ${env:Path}
    
    # Setup java environment for current user
    # (this makes it persistent across reboots)
    [Environment]::SetEnvironmentVariable("JAVA_HOME", "${env:JAVA_HOME}", "User")
    $UserPath = "${env:JAVA_HOME}\bin;" + [Environment]::GetEnvironmentVariable("Path", "User")
    [Environment]::SetEnvironmentVariable("Path", $UserPath, "User")
    
    # Download and extract Android SDK Command Line Tools for Windows
    # https://developer.android.com/studio/#downloads
    md "$BASE\android\sdk\cmdline-tools"
    cd "$BASE\android\sdk\cmdline-tools"
    Invoke-WebRequest "https://dl.google.com/android/repository/commandlinetools-win-9477386_latest.zip" -OutFile "commandlinetools-win.zip"
    Expand-Archive "commandlinetools-win.zip" ".\"
    ren "cmdline-tools" "latest"
    del "commandlinetools-win.zip"
    
    # Install SDK packages and NDK
    cd "$BASE\android\sdk\cmdline-tools/latest/bin"
    @('y','y','y','y','y','y','y','y','y','y') | ./sdkmanager --licenses
    # (Note: to view a list of available packages use .\sdkmanager --list)
    .\sdkmanager "platforms;android-31" "build-tools;31.0.0" "tools" "ndk;22.1.7171670" "extras;android;m2repository"
    
    # Download FPCUPdeluxe from https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases/latest
    # (tested with version 2.2.0n)
    md "$BASE\android\fpcupdeluxe_app"
    cd "$BASE\android\fpcupdeluxe_app"
    Invoke-WebRequest "https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases/download/v2.2.0n/fpcupdeluxe-x86_64-win64.exe" -OutFile "fpcupdeluxe-x86_64-win64.exe"
    "[General]" > fpcupdeluxe.ini
    "InstallDirectory=$BASE\android\fpc-lazarus" >> fpcupdeluxe.ini
    .\fpcupdeluxe-x86_64-win64.exe
    
    # Using the FPCUPdeluxe GUI
    # (The install path is already set. e.g. c:\users\[your_user]\android\fpc-lazarus)
    #
    # 1) At the bottom of the main Window, click the "LAMW" button
    #
    # As of 2023-Feb-05, this installs stable FPC (3.2.2), stable Lazarus (v2.2.4), cross
    # compilers for arm/android and aarch64/android, gradle v6.8.3, and latest LAMW from git.
    #
    # 2) (Optional) On modules tab click the "OPM" button to install Online Package Manager.
    # 3) Close FPCUPdeluxe
    
    # Change name for the desktop shortcut
    # (this works if $BASE = $HOME, otherwise rename the desktop shortcut manually)
    ren "$BASE\Desktop\Lazarus_fpc-lazarus.lnk" "Lazarus LAMW.lnk"
    
    # Create a directory for android projects
    md "$BASE\android\projects"
    
    # Edit LAMW settings via the ini file
    $LAMWini="$BASE\android\fpc-lazarus\config_lazarus\LAMW.ini"
    (Get-Content $LAMWini | Select-String -pattern "PathToWorkspace" -notmatch) | Set-Content $LAMWini
    "PathToWorkspace=$BASE\android\projects" >> $LAMWini
    "PathToAndroidSDK=$BASE\android\sdk" >> $LAMWini
    "PathToAndroidNDK=$BASE\android\sdk\ndk\22.1.7171670" >> $LAMWini
    "NDK=6" >> $LAMWini
    
    # After launching Lazarus-LAMW, you can confirm paths and settings in
    # the Tools | [LAMW] Android Module Wizard | Path Settings menu
    

For new LAMW projects
    
    
    for 32-bit:  ARMv7a + VFPv3 for Architecture/Instructions
    for 64-bit:  Aarch64 for Architecture/Instructions
    Min. Device API: 19  (Google recommended minimum SDK version)
    See here:  <https://android-developers.googleblog.com/2021/07/google-play-services-discontinuing-jelly-bean.html>
    

For demo LAMW projects
    
    
    (Project | Open Project) Open the controls.lpi file in the demo project jni folder
    If it asks for a Target API change enter 31 for the target.
    for 32-bit:  Change LAMW project options | Min SDK: 19 and Build tab | Chipset: ARMv7a+VFPv3
    for 64-bit:  Change LAMW project options | Min SDK: 19 and Build tab | Chipset: Aarch64
    Perform a Run | Clean up and Build
    

Changes
    
    
    2023-Feb-05 ver 0.7
     Use $BASE for installation directory (defaults to $HOME)
     Tested on Win10 Pro 22H2 64-bit
     Android Command-Line Tools for Linux build 9477386
     OpenJDK 11.0.18 LTS (Microsoft's build)
     Gradle 6.8.3 (installed automatically by FPCUPdeluxe)
     FPCUPdeluxe ver 2.2.0n
     Lazarus stable v2.2.4
    
    
    
    2022-May-01 ver 0.6 - Initial release
     Tested on Win10 Pro 21H2 64-bit
     Android Command-Line Tools for Linux build 8092744
     OpenJDK 11.0.14 LTS (Microsoft's build)
     Gradle 7.3.1 (installed automatically by FPCUPdeluxe)
     FPCUPdeluxe ver 2.2.0i
     FPC stable v3.2.2
     Lazarus stable v2.2.0

---

_Source: [https://wiki.freepascal.org/LAMW_install_windows_fpcupdeluxe](https://web.archive.org/web/20230604064419/https://wiki.freepascal.org/LAMW_install_windows_fpcupdeluxe)_
