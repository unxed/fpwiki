# Create snap package

Snap is a containerized package format which allows the distribution of programs to all Linux distributions. A snap is in a container and has only explicitly defined access to the "outside world", e.g. some libraries of the system or the user's home directory. 

## Contents

  * 1 Create snapcraft.yaml
  * 2 Example files
    * 2.1 Qt5
    * 2.2 GTK2
  * 3 Build Snap
  * 4 Publish your package in Snap Store
  * 5 Debug snap
  * 6 Use newer Lazarus version
  * 7 GTK2 theme support
    * 7.1 Content plugs only
    * 7.2 Full support
  * 8 Sound support
  * 9 OpenGL support
  * 10 SDL support
  * 11 snapcraft.yaml file in non-standard subdirectory
  * 12 Register app in snap store
  * 13 Setup build on Launchpad
  * 14 See also
  * 15 External links



# Create snapcraft.yaml

  * Install snapcraft to your Ubuntu machine with `sudo apt install snapcraft`
  * Create **snap** subdirectory in your project
  * Execute `snapcraft init` to create **snap/snapcraft.yaml** initial file
  * See [Snapcraft.yaml reference](<https://snapcraft.io/docs/snapcraft-yaml-reference>) for full list of supported properties.
  * Adapt the file so that it contains all necessary instructions for building the Lazarus program snap.



# Example files

## Qt5

The kde-neon extension and some tricks will help you to reduce the size of the Snap package to almost the normal binary size. The only thing to keep in mind is that Qt5 needs the libqt5pas-library. [In a snapcraft forum post](<https://forum.snapcraft.io/t/reduce-size-of-qt5-app-snap/31030>), we were figuring out how to create a small Lazarus Qt5 snap with all needed libraries included. You can find an example at the [Cantara project on Github](<https://github.com/reckel-jm/cantara/blob/master/snap/snapcraft.yaml>) or check the instruction below. 
    
    
    name: myapp
    version: 'version'
    summary: any summary
    description: |
      Any description
    confinement: strict
    icon: path-to-logo.png
    base: core20
    grade: stable
    
    architectures:
      - build-on: amd64
    
    parts:
      myapp:
        plugin: nil
        source: .
        source-type: local
        build-packages:
        - fpc
        - lazarus
        - lcl
        - lcl-utils
        - build-essential
        - libqt5pas-dev
        stage-packages:
        - libqt5pas1
        - libxinerama1
        override-build: |  
          lazbuild -B --ws=qt5 myapp.lpi 
          # Here you have to install (or copy) all the files of your project which you would like to distribute to $SNAPCRAFT_PART_INSTALL/ e.g. the compiled binary, icons and language files
          cp -r languages $SNAPCRAFT_PART_INSTALL/
          install myapp $SNAPCRAFT_PART_INSTALL/
          install myapp.desktop $SNAPCRAFT_PART_INSTALL/
          install myapp.ico $SNAPCRAFT_PART_INSTALL/
          install myapp.png $SNAPCRAFT_PART_INSTALL/
        stage: 
        - lib
        - usr
        - etc
        # make sure that you stage the files and folders to which you have installed (copied) above!
        - myapp
        - languages
        - myapp.desktop
        - myapp.ico
        - myapp.png
      cleanup: # this will make your snap small but keeps the needed libraries
        after: [myapp]
        plugin: nil
        build-snaps: [kde-frameworks-5-91-qt-5-15-3-core20]
        override-prime: |
          set -eux
          for snap in "kde-frameworks-5-91-qt-5-15-3-core20"; do  # List all content-snaps you're using here
            cd "/snap/$snap/current" && find . -type f,l -exec rm -f "$SNAPCRAFT_PRIME/{}" "$SNAPCRAFT_PRIME/usr/{}" \;
          done
          for cruft in bug lintian man icons; do
            rm -rf $SNAPCRAFT_PRIME/usr/share/$cruft
          done
          find $SNAPCRAFT_PRIME/usr/share/doc/ -type f -not -name 'copyright' -delete
          find $SNAPCRAFT_PRIME/usr/share -type d -empty -delete
          find $SNAPCRAFT_PRIME/usr/lib -type f,l -name 'libQt*.so*' ! -name 'libQt5Pas*.so*' -delete
    
    apps:
      myapp:
        extensions:
          - kde-neon # Don't remove this extension for it provides access to Qt5!
        command: myapp # to change: the command on which the app gets started
        desktop: myapp.desktop # to change: the path to the desktop file which will be used to create the entry in the start menu etc.
        plugs:
        - home
        - network
        - network-bind
    

## GTK2
    
    
    name: myapp
    version: '1.0.0'
    summary: Short description of the package.
    description: |
      Some more detailed multi-line description.
    confinement: strict
    base: core20
    grade: stable
    
    parts:
      myapp:
        plugin: nil
        source: .
        source-type: local
        build-packages: 
        - fpc
        - lazarus
        - lcl
        - lcl-utils
        stage-packages:
        # Autodetected dependencies
        - libatk1.0-0
        - libcairo2
        - libdatrie1
        - libfontconfig1
        - libfreetype6
        - libfribidi0
        - libgdk-pixbuf2.0-0
        - libgraphite2-3
        - libgtk2.0-0
        - libharfbuzz0b
        - libpango-1.0-0
        - libpangocairo-1.0-0
        - libpangoft2-1.0-0
        - libpixman-1-0
        - libpng16-16
        - libthai0
        - libx11-6
        - libxau6
        - libxcb-render0
        - libxcb-shm0
        - libxcb1
        - libxcomposite1
        - libxcursor1
        - libxdamage1
        - libxdmcp6
        - libxext6
        - libxfixes3
        - libxi6
        - libxinerama1
        - libxrandr2
        - libxrender1
        override-build: |
          lazbuild --build-mode=Release myapp.lpi
          install -d -m 755 $SNAPCRAFT_PART_INSTALL/usr/bin
          install -s -m 755 myapp $SNAPCRAFT_PART_INSTALL/usr/bin/myapp
          install -d -m 755 $SNAPCRAFT_PART_INSTALL/usr/share/applications
          install -m 755 myapp.desktop $SNAPCRAFT_PART_INSTALL/usr/share/applications
        stage:
        - etc
        - lib
        - usr
        - usr/bin
        - usr/share/myapp
        - usr/share/applications/myapp.desktop
    
    apps:
      myapp:
        command: usr/bin/myapp/myapp
        desktop: usr/share/applications/myapp.desktop
        plugs:
        - home
        - desktop
        - x11
    

  * Lazarus still uses old Gtk2 libraries. See [GTK2 Applications](<https://snapcraft.io/docs/gtk2-applications>) page.



# Build Snap

  * Build snap using **snapcraft** command
  * Install newly created snap with `sudo snap install --dangerous myapp_1.0.0_amd64.snap`



# Publish your package in Snap Store
    
    
    snapcraft register
    snapcraft login
    snapcraft upload myapp.snap
    snapcraft release myapp 1 stable
    

Now your snap should be released and available on **snapcraft.io** store. 

# Debug snap

To get to console of snap virtual filesystem use: 
    
    
    snap run --shell myapp
    cd $SNAP

Type **exit** to leave snap environment. See [Debugging snaps](<https://snapcraft.io/docs/debug-snaps>) for more information. 

# Use newer Lazarus version

Normally you can use Lazarus IDE version provided by current base (2.0.6 for core20). To use latest stable version with new features you can add following part into your snapcraft.yaml file. This part should be executed before myapp part. You need to update fixed URL based on required Lazarus version. Then those packages will be downloaded and installed before build. 
    
    
    parts:
      lazarus:
        plugin: nil
        source: .
        source-type: local
        build-packages: 
        - wget
        - libgtk2.0-dev
        override-build: |
          wget -nc https://downloads.sourceforge.net/project/lazarus/Lazarus%20Linux%20amd64%20DEB/Lazarus%202.2.2/lazarus-project_2.2.2-0_amd64.deb
          wget -nc https://downloads.sourceforge.net/project/lazarus/Lazarus%20Linux%20amd64%20DEB/Lazarus%202.2.2/fpc-laz_3.2.2-210709_amd64.deb
          wget -nc https://downloads.sourceforge.net/project/lazarus/Lazarus%20Linux%20amd64%20DEB/Lazarus%202.2.2/fpc-src_3.2.2-210709_amd64.deb
          apt install ./lazarus-project_2.2.2-0_amd64.deb ./fpc-laz_3.2.2-210709_amd64.deb ./fpc-src_3.2.2-210709_amd64.deb
        prime: [-*]
      myapp:
        after: [lazarus]
    

And remove normal Lazarus build-packages from your app so they are not installed for build: 
    
    
      myapp
        build-packages: 
        - fpc
        - lazarus
        - lcl
        - lcl-utils
    

# GTK2 theme support

There is no snapcraft extension available for GTK2 similarly to GTK3. So GTK2 theming needs to be configured with additional lines. 

## Content plugs only

To support GTK2 themes used by host system the app needs to have access to GTK2 themes. Standard GTK2 themes can be made available by using content plugs. Those content plugs are external shared packages so they won't make snap bigger. But the application can still print some errors into console. 

Extend your snapcraft.yaml file with the following: 
    
    
    # Additional plugs to pick up the GTK theme and icons from the system
    plugs: 
      icon-themes:
        interface: content
        target: $SNAP/data-dir/icons
        default-provider: gtk-common-themes
      sound-themes:
        interface: content
        target: $SNAP/data-dir/sounds
        default-provider: gtk-common-themes
      gtk-2-engines:
        interface: content
        target: $SNAP/lib/gtk-2.0
        default-provider: gtk2-common-themes:gtk-2-engines
      gtk-2-themes:
        interface: content
        target: $SNAP/usr/share/themes
        default-provider: gtk2-common-themes:gtk-2-themes
    
    layout:
      /usr/share/themes:
        bind: $SNAP/usr/share/themes
      /usr/lib/$SNAPCRAFT_ARCH_TRIPLET/gtk-2.0:
        bind: $SNAP/lib/gtk-2.0
    

## Full support

To fully support GTK2 environment we need to use snapcraft desktop helper and include various standard packages. Those packages make snap bigger by ~26 MB so there is a trade-off between size of snap packages and supported features. 

Extend your snapcraft.yaml file with the following: 
    
    
    parts:
      desktop-gtk2:
        source: https://github.com/ubuntu/snapcraft-desktop-helpers.git
        source-subdir: gtk
        plugin: make
        make-parameters: ["FLAVOR=gtk2"]
        build-packages:
        - build-essential
        - libgtk2.0-dev
        stage-packages:
        - libxkbcommon0  # XKB_CONFIG_ROOT
        - ttf-ubuntu-font-family
        - dmz-cursor-theme
        - light-themes
        - adwaita-icon-theme
        - gnome-themes-standard
        - shared-mime-info
        - libgtk2.0-0
        - libgdk-pixbuf2.0-0
        - libglib2.0-bin
        - libgtk2.0-bin
        - unity-gtk2-module
        - locales-all
        - libappindicator1
        - xdg-user-dirs
        - ibus-gtk
        - libibus-1.0-5
      myapp:
        after:
        - desktop-gtk2
    
    # Additional plugs to pick up the GTK theme and icons from the system
    plugs: 
      icon-themes:
        interface: content
        target: $SNAP/data-dir/icons
        default-provider: gtk-common-themes
      sound-themes:
        interface: content
        target: $SNAP/data-dir/sounds
        default-provider: gtk-common-themes
      gtk-2-engines:
        interface: content
        target: $SNAP/lib/gtk-2.0
        default-provider: gtk2-common-themes:gtk-2-engines
      gtk-2-themes:
        interface: content
        target: $SNAP/usr/share/themes
        default-provider: gtk2-common-themes:gtk-2-themes
    
    environment:
      XDG_DATA_DIRS: $SNAP/usr/share:$XDG_DATA_DIRS
      GTK_PATH: $SNAP/lib/gtk-2.0
    
    apps:
      myapp:
        command: usr/bin/myapp
        command-chain:
        - bin/desktop-launch
    
    

# Sound support

Snap supports sound through **audio-playback** plug. 
    
    
    apps:
      myapp:
        plugs:
        - audio-playback
    

You can play wav and mp3 files from command line using **play** command. See [Play Sound Multiplatform](<Play_Sound_Multiplatform.md> "Play Sound Multiplatform"). Then you also need to add additional stage-packages to myapp part and setup sox player for pulseaudio: 
    
    
    parts:
      myapp:
        stage-packages:    
        - sox
        - libsox-fmt-mp3
        - libsox-fmt-pulse
        - libpulse0 
    
    environment:
      LD_LIBRARY_PATH: $SNAP/usr/lib/$SNAPCRAFT_ARCH_TRIPLET/pulseaudio
      PULSE_SERVER: unix:/run/user/1000/pulse/native
    
    layout:
      /usr/lib/$SNAPCRAFT_ARCH_TRIPLET/sox:
        bind: $SNAP/usr/lib/$SNAPCRAFT_ARCH_TRIPLET/sox
    

# OpenGL support

To support 3D acceleration it is required to use opengl plug, package related libraries and set correct path for drivers. 
    
    
    parts:
      myapp:
        stage-packages:    
        - libglu1-mesa
        - libgl1-mesa-dri
    
    apps:
      myapp:
        plugs:
        - opengl
    
    environment:
      LIBGL_DRIVERS_PATH: $SNAP/usr/lib/$SNAPCRAFT_ARCH_TRIPLET/dri
    

# SDL support

Add needed build and stage packages. 
    
    
    parts:
      myapp:
        build-packages: 
        - libsdl2-dev
        - libsdl2-mixer-dev
        - libsdl2-image-dev
        stage-packages:
        - libsdl2-2.0-0
        - libsdl2-image-2.0-0
        - libsdl2-mixer-2.0-0
    

# snapcraft.yaml file in non-standard subdirectory

Snapcraft normally expect snapcraft.yaml file in snap directory. If your project has packaging for multiple other packaging formats, then you need to use build workaround script. Such shell script can put into directory like myapp/install/snap/local as build.sh file. 
    
    
    #!/bin/bash
    ln -s install/snap ../../../snap
    pushd ../../..
    snapcraft
    popd
    rm ../../../snap
    

# Register app in snap store

  * Login into <https://snapcraft.io> with your user account
  * Register new snap package name <https://snapcraft.io/account/register-snap>
  * Go to your newly registered package and fill all needed details <https://snapcraft.io/snaps>



# Setup build on Launchpad

Snap packages can be built automatically on [Launchpad](<https://launchpad.net/>). Open **Create snap package** link form your application branch page. 

# See also

  * [Publish project on Launchpad](<Publish_project_on_Launchpad.md> "Publish project on Launchpad")
  * [Deploying Your Application](<Deploying_Your_Application.md> "Deploying Your Application")



# External links

  * [How To Package and Publish a Snap Application on Ubuntu 18.04](<https://www.digitalocean.com/community/tutorials/how-to-package-and-publish-a-snap-application-on-ubuntu-18-04>)
  * [Sample snapcraft.yaml files using lazbuild on Github](<https://github.com/search?q=filename%3Asnapcraft.yaml+%22lazbuild%22&type=Code>)

---

_Source: [https://wiki.freepascal.org/Create_snap_package](https://web.archive.org/web/20230610172050/https://wiki.freepascal.org/Create_snap_package)_
