# LAMW install linux fpcupdeluxe

## Install LAMW and Android Development Environment on Linux using [fpcupdeluxe](<fpcupdeluxe.md>)

See also
    
    
    
    Forum Post: <https://forum.lazarus.freepascal.org/index.php?topic=40750>
    For Windows How-To see here:  [https://wiki.freepascal.org/LAMW_install_windows_fpcupdeluxe](<LAMW_install_windows_fpcupdeluxe.md>)
    
    
    
    [https://wiki.freepascal.org/LAMW](<LAMW.md>)
    <https://github.com/jmpessoa/lazandroidmodulewizard>
    [https://wiki.freepascal.org/fpcupdeluxe](<fpcupdeluxe.md>)
    <https://github.com/LongDirtyAnimAlf/fpcupdeluxe>
    

Known Issues
    
    
    As of 2023-May-22, this error is still happening:
    <https://forum.lazarus.freepascal.org/index.php/topic,21919.msg441567.html#msg441567>
    
    
    
    Manjaro / Arch Linux: As of 2023-May-22 installing on Manjaro works,
    and can be used for building a project but there is an issue
    with a workaround:
     * Lazarus Menu "Run" | "[LAMW] Build Android Apk and Run" fails
       - Instead use "Run" | "Build" and if the build is successful open a terminal
         in the project folder and run ./gradle-local-build.sh to build the APKs
    

Motivation
    
    
    Needed simple instructions to get an up-to-date Android build environment setup on Linux.
    Thanks to other developers for sharing their hard work, utilities, and helpful information.
    

Everything is self-contained under the "$BASE\android" folder
    
    
    ($BASE defaults to the user's home folder)
    $BASE/android/sdk  (android sdk and build tools)
    $BASE/android/sdk/ndk-bundle  (android ndk)
    $BASE/android/fpcupdeluxe_app  (FPCUPdeluxe installer)
    $BASE/android/projects  (default LAMW projects folder)
    $BASE/android/fpc-lazarus  (FPC, Lazarus built w/ LAMW)
    $BASE/android/fpc-lazarus/ccr/lamw  (LAMW files)
    $BASE/android/fpc-lazarus/ccr/lamw-gradle  (gradle build tool)
    $BASE/android/fpc-lazarus/ccr/lamw/demos (LAMW demo projects)
    

Versions Tested
    
    
    xubuntu-20.04.6-desktop-amd64
    xubuntu-22.04.2-desktop-amd64 (minimal install)
    xubuntu-23.04-minimal-amd64
    manjaro-xfce-22.1.2-minimal-230518-linux61
    
    
    
    Android Command-Line Tools for Linux build 9477386
    OpenJDK 11.0.19
    Android SDK/API Level 31 (Android 12)
    Android NDK revision 22
    Gradle 6.8.3 (installed automatically by FPCUPdeluxe)
    
    
    
    FPCUPdeluxe ver 2.2.0r
    FPC stable v3.2.2
    Lazarus stable v2.2.6
    
    
    
    32-bit CPU Target: ARMv7a + VFPv3 (hardware floating point)
    64-bit CPU Target: Aarch64 (this always uses hardware floating point)
    

## Main steps begin here
    
    
    Open a terminal and run the commands below.  I recommend running them one
    at a time in order to verify the results before moving on to the next step.
    
    
    
    #!/usr/bin/env bash
    # Setup Android Development Environment on Linux (FPCUPdeluxe / LAMW)
    # Tested on
    #   xubuntu-20.04.6-desktop-amd64
    #   xubuntu-22.04.2-desktop-amd64 (minimal install)
    #   xubuntu-23.04-minimal-amd64
    #   manjaro-xfce-22.1.2-minimal-230518-linux61
    # Targets: 32-bit ARMv7a and 64-bit Aarch64
    # 2023-May-22 ver 2.0
    
    BASE="$HOME" #change this if you want to install to a different location
    
    # Install libraries and tools (Ubuntu based distros only)
    sudo apt update
    sudo apt install -y libx11-dev libgtk2.0-dev libgdk-pixbuf2.0-dev libcairo2-dev libpango1.0-dev libxtst-dev libatk1.0-dev libghc-x11-dev freeglut3-dev
    sudo apt install -y git make build-essential gdb zip unzip unrar wget
    sudo apt install -y openjdk-11-jdk android-tools-adb
    
    # Install libraries and tools (Arch Linux based distros only)
    # uncomment these if using an Arch Linux based distro
    #sudo pacman -Syu
    #sudo pacman -S --needed libx11 gtk2 gdk-pixbuf2 cairo pango libxtst atk freeglut
    #sudo pacman -S --needed git make gdb zip unzip unrar wget
    #sudo pacman -S --needed android-tools jdk11-openjdk
    
    # download and extract Android SDK Command Line Tools for Linux
    # from https://developer.android.com/studio/#downloads
    mkdir -p "$BASE/android/sdk/cmdline-tools"
    cd "$BASE/android/sdk/cmdline-tools"
    wget "https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip"
    unzip "commandlinetools-linux-9477386_latest.zip"
    rm "commandlinetools-linux-9477386_latest.zip"
    mv cmdline-tools tools
    
    # Install SDK packages and NDK
    cd "$BASE/android/sdk/cmdline-tools/tools/bin"
    yes | ./sdkmanager --licenses
    # (Note: to view a list of available packages use ./sdkmanager --list)
    ./sdkmanager "platforms;android-31" "build-tools;31.0.0" "tools" "ndk;22.1.7171670" "extras;android;m2repository"
    
    # Download FPCUPdeluxe from https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases/latest
    # (tested with version 2.2.0r)
    mkdir "$BASE/android/fpcupdeluxe_app"
    cd "$BASE/android/fpcupdeluxe_app"
    wget "https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases/download/v2.2.0r/fpcupdeluxe-x86_64-linux"
    chmod +x fpcupdeluxe-x86_64-linux
    echo "[General]" > fpcupdeluxe.ini
    echo "InstallDirectory=$BASE/android/fpc-lazarus" >> fpcupdeluxe.ini
    ./fpcupdeluxe-x86_64-linux
    
    # Using the FPCUPdeluxe GUI
    # (The install path is already set to: /home/[your_user]/android/fpc-lazarus)
    #
    # 1) At the bottom of the main Window, click the "LAMW" button
    #
    # As of 2023-May-22, this installs stable FPC (3.2.2), stable Lazarus (v2.2.6), cross
    # compilers for arm/android and aarch64/android, gradle v6.8.3, and latest LAMW from git.
    #
    # 2) (Optional) On module tab click the "OPM" button to install Online Package Manager.
    # 3) Close FPCUPdeluxe
    
    # Change name for the desktop shortcut
    mv "$(xdg-user-dir DESKTOP)/fpcup-Lazarus_fpc-lazarus.desktop" "$(xdg-user-dir DESKTOP)/Lazarus-LAMW.desktop"
    sed -i "/^Name=/c\Name=Lazarus-LAMW" "$(xdg-user-dir DESKTOP)/Lazarus-LAMW.desktop"
    
    # Create a directory for android projects
    mkdir -p "$BASE/android/projects"
    
    # Edit LAMW settings via the ini file
    LAMWini="$BASE/android/fpc-lazarus/config_lazarus/LAMW.ini"
    sed -i "/PathToWorkspace/c\PathToWorkspace=$BASE/android/projects" $LAMWini
    echo "PathToAndroidSDK=$BASE/android/sdk" >> $LAMWini
    echo "PathToAndroidNDK=$BASE/android/sdk/ndk/22.1.7171670" >> $LAMWini
    echo "NDK=6" >> $LAMWini
    
    # After launching Lazarus-LAMW, you can confirm paths and settings in
    # the Tools | [LAMW] Android Module Wizard | Path Settings menu
    

For new LAMW projects
    
    
    for 32-bit:  ARMv7a + VFPv3 for Architecture/Instructions
    for 64-bit:  Aarch64 for Architecture/Instructions
    Min. Device API: 19  (Google recommended minimum SDK version)
    See here:  <https://android-developers.googleblog.com/2021/07/google-play-services-discontinuing-jelly-bean.html>
    Bug work-around: After creating a new project, save all, close lazarus, and re-open lazarus (it reloads the last open project)
    (this allows placement of controls on form without throwing an error - [SEE HERE](<https://forum.lazarus.freepascal.org/index.php/topic,21919.msg441567.html#msg441567>))
    

For demo LAMW projects
    
    
    (Project | Open Project) Open the controls.lpi file in the demo project jni folder
    Bug work-around: Save the project, close lazarus, and re-run lazarus (by default it opens the last project).
    If it asks for a Target API change enter 31 for the target.
    for 32-bit:  Change LAMW project options | Min SDK: 19 and Build tab | Chipset: ARMv7a+VFPv3
    for 64-bit:  Change LAMW project options | Min SDK: 19 and Build tab | Chipset: Aarch64
    Perform a Run | Clean up and Build
    

Optional script to create terminal launcher for command line work
    
    
    #Create shell script and desktop shortcut
    $BASE = "$HOME" #change this if you installed to a different location above
    
    LAMWini="$BASE/android/fpc-lazarus/config_lazarus/LAMW.ini"
    # Note: the following lines must be copy/pasted as one line, starting at "cat" and ending at "EOF"
    cat <<EOF > $BASE/android/set_dev_environment
    #!/bin/sh
    # Setup environment for fpc / lazarus / android command line work
    
    FPC_PATH="$BASE/android/fpc-lazarus/fpc/bin/x86_64-linux"
    LAZ_PATH="$BASE/android/fpc-lazarus/lazarus"
    
    export PPC_CONFIG_PATH=\${FPC_PATH}
    export PATH=\${FPC_PATH}:\${LAZ_PATH}:\${PATH}
    
    export ANDROID_SDK_ROOT="${BASE}/android/sdk"
    export GRADLE_HOME="$(awk -F '=' '/PathToGradle/{print $NF}' $LAMWini)"
    export PATH=\${PATH}:\${GRADLE_HOME}/bin
    
    cd "$BASE/android/projects"
    exec /bin/bash
    EOF
    chmod +x $BASE/android/set_dev_environment
    
    # Note: the following lines must be copy/pasted as one line, starting at "cat" and ending at "EOF"
    cat <<EOF > "$(xdg-user-dir DESKTOP)/Android-Command-Line.desktop"
    [Desktop Entry]
    Type=Application
    Icon=utilities-terminal
    Exec=$BASE/android/set_dev_environment
    Name=Android-Command-Line
    Terminal=true
    StartupNotify=false
    EOF
    chmod +x "$(xdg-user-dir DESKTOP)/Android-Command-Line.desktop"
    

Changes
    
    
    2023-May-22 ver 2.0
      Added LAMW bug work-arounds for two cases:
        1) Directly after creating a new project, save the project, close Lazarus, and re-open.
        2) Directly after opening a demo project, save the project, close Lazarus, and re-open.
        
      Tested on more distro versions
        xubuntu-20.04.6-desktop-amd64
        xubuntu-22.04.2-desktop-amd64 (minimal install)
        xubuntu-23.04-minimal-amd64 (NOTE: I will not usually test on non-LTS versions)
        manjaro-xfce-22.1.2-minimal-230518-linux61 (based on arch linux)
        
      Manjaro does not have special issues with changing the LAMW project chipset - remove previous comment
      Don't explicitly install freeglut3 (it gets pulled in if needed by freeglut3-dev)
      Remove subversion from install packages (fpc and lazarus both use git now)
      Remove apache-ant from pacman (arch linux) install (fpcupdeluxe installs a private copy)
      Testing with updated software components:
        fpcupdeluxe 2.2.0r
        OpenJDK ver 11.0.19
        Lazarus stable v2.2.6
    
    
    
    2023-Feb-06 ver 1.9
     Add support for Arch Linux based distros
     Test on Manjaro 22.0
     Add known issues based Manjaro testing
    
    
    
    2023-Feb-06 ver 1.8
     Use $BASE for installation directory (defaults to $HOME)
     Android Command-Line Tools for Linux build 9477386
     OpenJDK 11.0.17
     Gradle 6.8.3 (installed automatically by FPCUPdeluxe)
     FPCUPdeluxe ver 2.2.0n
     Lazarus stable v2.2.4

---

_Source: [https://wiki.freepascal.org/LAMW_install_linux_fpcupdeluxe](https://web.archive.org/web/20230924193324/https://wiki.freepascal.org/LAMW_install_linux_fpcupdeluxe)_
