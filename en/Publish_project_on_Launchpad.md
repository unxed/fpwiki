# Publish project on Launchpad

[Launchpad](<https://launchpad.net/>) is default Ubuntu build system. It allows us to register own project and let build server build our application. But web interface is overcomplicated and it is not easy to setup build of Lazarus project. Here this wiki pages can help. 

It is not simply possible to get own application package immediately directly to standard Ubuntu packages. Such package needs to comply with several rules to be accepted as standard distribution package. But Launchpad also supports PPA (Personal Package Archives) which is place where non distribution packages can be prepared. 

## Contents

  * 1 Build package on Launchpad
  * 2 Build package locally
  * 3 LazPackager
  * 4 See also
  * 5 External links



## Build package on Launchpad

  * First [register](<https://login.launchpad.net/+login>) new user account or login to your existing user account


  * [Register new project](<https://launchpad.net/projects/+new>) for your application


  * After the project is created you can import source code from external repository to Launchpad Bazaar project repository. Go to Code section and click to **Configure code** link. Then fill required fields and click to **Update** button.


  * Create debian directory under your project. It doesn't need to be in branch root directory. It will be appended to branch from specified location. The directory need to contain several debian package specific files.



control file: 
    
    
    Source: project-name
    Maintainer: Username <email@email.xx>
    Section: utils
    Priority: optional
    Standards-Version: 1.0.0
    Build-Depends: fpc, lazarus, lcl, lcl-utils, debhelper (>= 8)
    
    Package: project-name
    Architecture: any
    Depends: ${shlibs:Depends}, ${misc:Depends},
    Description: Short description of package.
    HomePage: http://project-homepage.xx/
    

For section you can use only predefined strings. See question here <http://askubuntu.com/questions/722597/what-packages-sections-are-available-for-trusty>

rules file: 
    
    
    #!/usr/bin/make -f
    
    # Set temporary HOME for lazarus primary config directory
    export HOME=$(CURDIR)/tmphome
    
    ROOT = $(CURDIR)/debian/project-name
    
    override_dh_auto_clean:
    	$(RM) -r lib
    	$(RM) lib *.res project-name
    
    override_dh_auto_build:
    	lazbuild --build-mode=Release project-name.lpi
    
    override_dh_auto_install:
    	install -d -m 755 $(ROOT)/usr/bin
    	install -s -m 755 project-executable $(ROOT)/usr/bin
    	install -d -m 755 $(ROOT)/usr/share/applications
    	install -m 644 project-name.desktop $(ROOT)/usr/share/applications
    	install -d -m 755 $(ROOT)/usr/share/pixmaps
    	install -m 644 project-name.png $(ROOT)/usr/share/pixmaps
    	install -d -m 755 $(ROOT)/usr/share/project-name/i18n
    	install -D -m 644 i18n/*.po $(ROOT)/usr/share/project-name/i18n
    
    %:
    	dh $@
    

changelog file: 
    
    
    project-name (1.0.0-1) precise; urgency=low
    
      * Original version 1.0.0 packaged with lazdebian
    
     -- Username <email@email.xx>  Sun, 27 Nov 2016 00:51:08 +0100
    

copyright file: 
    
    
    Format: http://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
    
    Files: *
    Copyright: 2016 Creator name
    License: GPL-2+
     This program is free software; you can redistribute it and/or modify
     it under the terms of the GNU General Public License as published by
     the Free Software Foundation; either version 2 of the License, or
     at your option) any later version.
     .
     This program is distributed in the hope that it will be useful,
     but WITHOUT ANY WARRANTY; without even the implied warranty of
     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
     GNU General Public License for more details.
     .
     You should have received a copy of the GNU General Public License along
     with this program; if not, write to the Free Software Foundation, Inc.,
     51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
     .
     On Debian systems, the full text of the GNU General Public
     License version 2 can be found in the file
     /usr/share/common-licenses/GPL-2
    

compact file: 
    
    
    8
    

source/format: 
    
    
    3.0 (quilt)
    

  * Now also in Code section of the project click to **Import a branch** link on right panel. Fill branch name with **debian-packaging** and as source select import from location where debian directory for the application is stored. If you create multi-platform application then usually debian directory is not stored in root of your application branch but rather in some subdirectory. Then click to **Request import**.


  * Create recipe for branch:


    
    
    # bzr-builder format 0.3 deb-version {debupstream}-0~{revno}
    lp:custom-app
    nest packaging lp:~user/application-name/debian-packaging debian
    

  * Click to **Request build(s)** link to start build process



## Build package locally

You can also build your deb package locally outside of launchpad. Just create script called for example build.sh. This script will copy your project files with debian subdirectory to temporary directory where build process will be started and new deb package created. 
    
    
    #!/bin/bash
    
    BUILD_ROOT=/tmp/build-root/application-name
    mkdir -p $BUILD_ROOT
    cp -r -f . $BUILD_ROOT
    cd $BUILD_ROOT
    dpkg-buildpackage -b -rfakeroot -us -uc
    

After execution new package will be placed in /tmp/build-root. 

## LazPackager

Is Lazarus IDE extension which helps to create and build deb packages. 

Download: <https://github.com/prof7bit/LazPackager>

## See also

  * [How to setup a FPC and Lazarus Ubuntu repository#The debs](<How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository.md> "How to setup a FPC and Lazarus Ubuntu repository")



## External links

  * [Required files under the debian directory](<https://www.debian.org/doc/manuals/maint-guide/dreq.en.html>)

---

_Source: [https://wiki.freepascal.org/Publish_project_on_Launchpad](https://web.archive.org/web/20250124214227/https://wiki.freepascal.org/Publish_project_on_Launchpad)_
