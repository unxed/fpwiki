# Debian package structure

│ **English (en)** │  **[русский (ru)](<../ru/Debian_package_structure.md>)** │

Installations on Debian are provided as files with .deb extension. This article discusses how to build them manually. 

If you would like to bring your program to Debian distributions, see how to make a [Debian upstream](<Debian_upstream.md> "Debian upstream") source. 

## Contents

  * 1 General
    * 1.1 Two types of packages
    * 1.2 Preparing the package
    * 1.3 Finalizing the staging directory
    * 1.4 Creating the Deb archive
    * 1.5 Checking the archive
  * 2 Directory structure
    * 2.1 DEBIAN directory
    * 2.2 usr/bin directory
    * 2.3 Resources directory
    * 2.4 Configuration directory
    * 2.5 usr/share/applications directory
      * 2.5.1 usr/share/pixmaps directory
      * 2.5.2 usr/share/icons/hicolor directory
    * 2.6 Documentation directory
      * 2.6.1 changelog file
      * 2.6.2 copyright file
    * 2.7 usr/share/man directory



## General

### Two types of packages

There are two types of packages, a source package and a binary package. This article is about the binary packages. They are a convenient way to bundle up pre-compiled binaries ready to be supplied directly to an end user. However, distributions like Debian and Ubuntu will not accept to add a binary package in their repositories, they require a [source package](<Debian_package_source.md> "Debian package source") that can be used to build the binary version on demand. The [source package](<Debian_package_source.md> "Debian package source") is somewhat more difficult to assemble and is subject to a number of technical and legal rules before being acceptable to the major Linux distributions. 

### Preparing the package

In a Debian binary package, the installation files are compressed using **dpkg-deb** command. You can view the content of the resulting .deb files as any other compressed archive by right-clicking them and opening them with the archive manager. 

To make a .deb file, you need to prepare a directory structure with the appropriate files. This will be called the staging directory and expressed in this article with the variable `${STAGING_DIR}`. You can replace it with the adequate folder or define this variable in a script. 

You can for example create the staging directory as a subdirectory of the current folder. The simplest script would be: 
    
    
    STAGING_DIR=./staging
    mkdir ${STAGING_DIR}
    

Though if you write a script, it may be run from another location. So instead, you can ensure that the variable contains the subdirectory relative to the script with the following: 
    
    
    cd `dirname $0`
    STAGING_RELATIVEDIR="./staging"
    STAGING_DIR=$(readlink --canonicalize "${STAGING_RELATIVEDIR}")
    

Also you may need to remove the existing staging directory to make sure it is empty: 
    
    
    rm -rf "${STAGING_DIR}"
    mkdir "${STAGING_DIR}"
    

The content of this directory is explained in the rest of this article. 

### Finalizing the staging directory

Read the section _Directory structure_ to add all the necessary files. Then you can finalize the directory. 

The directories and files are supposed to be read-only in the package. To ensure that's the case, you can before calling **dpkg-deb** do the following: 
    
    
    find "${STAGING_DIR}" -type d -exec chmod 0755 {} \;  #set directory attributes
    find "${STAGING_DIR}" -type f -exec chmod 0644 {} \;  #set data file attributes
    find "${STAGING_DIR}/usr/bin" -type f -exec chmod 0755 {} \;  #set executable attributes
    

When you have all file in the staging directory, you can determine the installed size and add it to the **control** file: 
    
    
    SIZE_IN_KB="$(du -s ${STAGING_DIR} | awk '{print $1;}')"
    echo "Installed-Size: ${SIZE_IN_KB}" >> "${STAGING_DIR}/DEBIAN/control"
    

### Creating the Deb archive

Once the directory structure is ready, you can create the archive with: 
    
    
    dpkg-deb --root-owner-group --build "${STAGING_DIR}" "${PACKAGE_NAME}.deb"
    

Or for older versions of dpkg-deb: 
    
    
    fakeroot dpkg-deb --build "${STAGING_DIR}" "${PACKAGE_NAME}.deb"
    

`${STAGING_DIR}` is the path to the directory structure to compress. `${PACKAGE_NAME}` is the filename to use for the archive. 

Generally you will create the package as a regular user, not with the _root_ account. But installation files must not belong to a specific user. Using `--root-owner-group` option or **fakeroot** prefix will save in the archive that the files belong instead to _root_. 

After that, you will probably not need the staging directory anymore, you can thus delete it: 
    
    
    rm -rf "${STAGING_DIR}"
    

### Checking the archive

Once you've created the package, you can check it follows the guidelines with the command **lintian** : 
    
    
    lintian "${PACKAGE_NAME}.deb" --info
    

Here is the list of potential problems: <https://lintian.debian.org/tags.html>

With Lazarus, you will get by default the `hardening-no-pie` (and `hardening-no-bindnow` warnings in mentors). To avoid such warnings, add the following compiler options in the project options: 
    
    
    -Cg
    -k-pie
    -k-znow
    

## Directory structure

Here is the structure of the staging directory that you need to prepare. 
    
    
    staging
      ├─ DEBIAN
      └─ usr
          ├─ bin
          └─ share
              ├─ project1       #resources of your package
              ├─ applications   #shortcut
              ├─ pixmaps        #default icon
              ├─ icons
              │   └─ hicolor    #icons of various sizes
              ├─ doc
              │   └─ project1   #information about your package
              └─ man            #user manual
                  └─ man1       #index
    

### DEBIAN directory

The name of this directory is uppercase. It needs to contain one file called **control**. Here is an example for a package called **project1** at version 2.0. 
    
    
    Section: graphics
    Priority: optional
    Maintainer: myname <myname@webserver.com>
    Homepage: <https://www.project1.com>
    Package: project1
    Architecture: amd64
    Version: 2.0
    Depends: libatk1.0-0 (>= 1.12.4), libc6 (>= 2.2.5), libcairo2 (>= 1.2.4), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.12.0), libgtk2.0-0 (>= 2.24.0), libpango-1.0-0 (>= 1.18.0), libx11-6
    Description: This is a test project.
     Long description blah blah blah.
    

Sample values for **Section** : comm, database, devel, editors, electronics, fonts, games, graphics, math, web, net, news, science, sound, utils, video. 

The **Architecture** field is the one used to compile the program. To know the current architecture, use the following command: 
    
    
    dpkg --print-architecture
    

The **Depends** field contains the list of Linux libraries used. You can get this list with the following commands run in the directory of your binary file: 
    
    
    mkdir debian                 #need a debian directory
    touch debian/control         #with one control file in it
    dpkg-shlibdeps -O project1   #supposing your binary is called project1
    

You can then remove the almost empty _debian_ folder if you don't use it. 

This will output the dependencies (after the equal sign): 
    
    
    shlibs:Depends=libatk1.0-0 (>= 1.12.4), libc6 (>= 2.2.5), libcairo2 (>= 1.2.4), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.12.0), libgtk2.0-0 (>= 2.24.0), libpango-1.0-0 (>= 1.18.0), libx11-6
    

To be complete, it should be noted that all these packages are not absolutely necessary, because several packages on this list may already be part of the dependencies - and therefore be part of the installation - of another package listed in this same list (see [[1]](<Deploying_Your_Application.md>)to cross-check, and eliminate superfluous dependencies). 

### usr/bin directory

This directory contains the binary file and only this file (_for information, avoid underscore in the name of your project - for example, dpkg-deb refuses it - because the package name should follow this regex: ${PackageName}_${Version}_${Arch}.deb, e.g. dpkg-dev_1.19.0.5ubuntu2_all.deb_). All resources files need to go in _usr/share/project1_. To access resources, your program can use a relative path like _../share/project1_. 

The directory _usr/local/bin_ is invalid in a package[[2]](<https://lintian.debian.org/tags/file-in-usr-local.html>) because _local_ is used for projects compiled locally and installed using their Makefile. 

### Resources directory

It will be called _usr/share/project1_ if the package name is "project1".  
You can put here all the data files needed by your program. Note that they will be read-only. 

### Configuration directory

There are other places to store [configuration files](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide"). Configuration files should be originally created by the application itself (with default values), if they do not exist when the application starts. 

### usr/share/applications directory

To add your application to the Linux menu, you need to add a file called **project1.desktop** containing: 
    
    
    [Desktop Entry]
    Name=Project1
    Comment=Cool project
    Icon=project1
    Exec=project1
    Terminal=false
    Type=Application
    Categories=Graphics
    GenericName=Project1
    Keywords=feature1; feature2; feature3
    

This file is similar to a shortcut to your program. 

The **Categories** field is similar to the one in the **control** file. It can be for example: Communication, Database, Development, Editors, Electronics, Fonts, Games, Graphics, Math, Internet, Networking, News, Science, Sound, Utility, Video. 

The keywords help search through applications. 

Note that you need to provide the icon as a PNG file, either in the directory _usr/share/pixmaps_ or in _usr/share/icons/hicolor_. 

#### usr/share/pixmaps directory

Will contain the single-size icon of your menu shortcut in PNG format called **project1.png**. A medium size like 48x48 will be fine. 

#### usr/share/icons/hicolor directory

Will contain the multi-size icon. Here is the directory structure: 
    
    
    icons
     └─ hicolor
         ├─ 8x8
         │   └─ apps
         ├─ 16x16
         │   └─ apps
         ...
    

There is a subdirectory for each possible size. You can display all the sizes currently defined on your system by writing: 
    
    
    ls /usr/share/icons/hicolor
    

In each apps folder, put an image with always the same name. If you project is called project1 then it will be called **project1.png** for most folders. In the scalable and symbolic folders you can put an SVG file. 

### Documentation directory

It will be called _usr/share/doc/project1_ if your the package name is "project1". 

The folder contains the **changelog.Debian.gz** and **copyright** files and optionally the **README** file. 

#### changelog file

The minimum changelog would be: 
    
    
    project1 (1.0) unstable; urgency=low
    
      * Initial release
    
     -- myname <myname@webserver.com>  Tue, 19 May 2020 11:06:00 +0100
    

You can add many lines starting with * to indicate the list of changes in the version. The spaces at the beginning of the line are necessary. Only the line indicating the version starts directly at the beginning of the line. 

The latest version must be at the top of the file. 

The changelog must be compressed with maximum compression. To do that, supposing you already have the uncompressed **changelog** file, compress it with the following command: 
    
    
    gzip -9 -n usr/share/doc/project1/changelog.Debian
    

The -9 -n options are necessary for the package to follow the guidelines. 

#### copyright file

If you provide the Deb package yourself, it is not mandatory to use the machine-readable format, however it will prepare you for publishing the package in Debian distributions. See: [copyright in Debian package structure](<Debian_package_source.md> "Debian package source"). 

### usr/share/man directory

Contains the user manual that you can view with the **man** command. It must contain at least **project1.1.gz** in the _man1_ subdirectory, which will be the index page of your manual. 

The syntax is a bit peculiar. There are some tools to help but you still need to know the syntax. 

  * description of the syntax: <https://liw.fi/manpages/>
  * editor with wizard and WYSIWYG preview mode: **gmanedit**
  * live preview editor (toggle to AN mode): <https://roperzh.github.io/grapse/>



Here is a sample file: 
    
    
    .TH Project1 1 "19 May 2020" "" Project1
    .SH NAME
    Project1 - My cool project
    .SH SYNOPSIS
    .B project1
    [PARAMETER]
    .SH DESCRIPTION
    What my program does.
    .PP
    More description.
    

The manual, like the changelog, must be compressed using the following command: 
    
    
    gzip -9 -n usr/share/man/man1/project1.1
    

The -9 -n options are necessary for the package to follow the guidelines.

---

_Source: [https://wiki.freepascal.org/Debian_package_structure](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Debian_package_structure)_
