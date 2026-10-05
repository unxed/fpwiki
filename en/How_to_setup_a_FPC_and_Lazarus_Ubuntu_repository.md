# How to setup a FPC and Lazarus Ubuntu repository

[![Logo-ubuntu cof-orange-hex.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/50px-Logo-ubuntu_cof-orange-hex.svg.png)](</File:Logo-ubuntu_cof-orange-hex.svg>)

This article applies to [Ubuntu](</Category:Ubuntu> "Category:Ubuntu") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository.md>)** │

## Contents

  * 1 Beware downloaders!
  * 2 What is a repository?
  * 3 Who needs it?
  * 4 The directory structure
  * 5 The debs
    * 5.1 Creating the deb files yourself
      * 5.1.1 Install development packages
      * 5.1.2 Build new deb
      * 5.1.3 Replace the deb files in the repository
  * 6 PGP Key
    * 6.1 Creating a PGP key
    * 6.2 Upload the key to a public key server
    * 6.3 Remember the key ID
  * 7 Updating repository files
  * 8 Adding the repository to a client
    * 8.1 Add the key
    * 8.2 Add the repository
  * 9 Install Lazarus



## Beware downloaders!

If you only want to download Lazarus, then go to our [Ubuntu repository](<Lazarus_release_version_for_Ubuntu.md> "Lazarus release version for Ubuntu"). 

This page describes how to setup a repository yourself, it's not meant for normal users. 

## What is a repository?

A ubuntu repository is a directory. It can be stored on the lokal disk, or on a webserver or on a ftp server. To use it, you add its path into your /etc/apt/sources.list and setup a pgp key. Then you can simply install lazarus with your favourite package gui (e.g. synaptic) and fpc, fpc-src and lazarus will be downloaded, installed and updated automatically. 

## Who needs it?

Administrators who wants to install FPC+Lazarus on a pool of computers. Like in school. Or newbies who just want to quickly test it. 

## The directory structure

Let's assume you want create a repository available via the apache webserver. Then you need to setup a directory like /var/www/lazarus that is public readable and only writable by root. 

Create a sub directory for each target you want to support: 
    
    
    mkdir -p /var/www/lazarus/dists/lazarus-testing/universe/binary-i386
    mkdir -p /var/www/lazarus/dists/lazarus-testing/universe/binary-amd64
    

## The debs

Put the fpc, fpc-src and lazarus deb files into it. 

### Creating the deb files yourself

You can create the debs with the scripts in tools/install/ of the lazarus sources. 

#### Install development packages

  * install development packages:


    
    
    sudo apt-get install libgtk2.0-dev libgtk1.2-dev libgdk-pixbuf-dev libgpmg1-dev fakeroot libncurses5-dev
    

  * Install the latest stable FPC. This is needed to build the new FPC and Lazarus:



Either the deb files from the official site or the tar.gz. e.g.: 
    
    
    sudo apt-get install fp-compiler
    

  * Download the FPC sources. To get the current development version you can use the command below. To get a more stable version, see [Installing_Lazarus#FPC Sources](<Installing_Lazarus.md> "Installing Lazarus"):


    
    
    svn co http://svn.freepascal.org/svn/fpc/trunk fpc
    

  * Download the lazarus sources:


    
    
    svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus
    

#### Build new deb

  * go into the lazarus install script directory:


    
    
    cd lazarus/tools/install
    

  * build the fpc deb. The following script will build a single fpc deb using the date as version. As parameter you must specify the path of the FPC sources you downloaded above:


    
    
    sudo ./create_fpc_deb.sh fpc /path/to/the/sources/of/fpc/
    

  * install the new fpc deb. This is needed to build the lazarus deb, which depends on the new fpc deb. Don't forget to uninstall first your old FPC.


    
    
    sudo dpkg -i fpc_2.2.5-090517_i386.deb
    

  * build the fpc-src deb. This works pretty much the same as above (parameter fpc-src instead of fpc):


    
    
    ./create_fpc_deb.sh fpc-src /path/to/the/sources/of/fpc/
    

  * build the lazarus deb. You can either build a normal lazarus using gtk2:


    
    
    ./create_lazarus_deb.sh append-revision
    

or a lazarus using gtk1: 
    
    
    ./create_lazarus_deb.sh gtk1 append-revision
    

  * To test the lazarus package you can install the new fpc-src deb and the new lazarus deb with:


    
    
    sudo dpkg -i fpc-src_2.2.5-090517_i386.deb lazarus_0.9.27.20004-0_i386.deb
    

#### Replace the deb files in the repository

Now you have 3 deb files. 

You can install them directly, but first un-install the build version 

  * If you installed fp-compiler you will need to:


    
    
    dpkg -r fp-compiler fp-units-rtl fp-compiler
    

  * Copy them to your repository:


    
    
    cp fpc_2.3.1-070726_i386.deb fpc-src_2.3.1-070726_i386.deb lazarus_0.9.23.11636-0_i386.deb \
    /var/www/lazarus/dists/lazarus-testing/universe/binary-i386/
    

  * Don't forget to remove the old ones.



## PGP Key

You need to sign the debs with a PGP key, so that the target systems can be sure, that no evil-doer replaced the files. 

### Creating a PGP key

You can use tools like seahorse or thunderbird to create the PGP key. 

  * Install seahorse
  * start seahorse
  * Key > Create new key
  * A window popup up asking for the type. Choose _PGP Key_.
  * Give a full name and an email adress and click _Create_.
  * The passphrase is needed to encrypt the created files. This way no one can use the keys but you, even if they manage to steal your files. If you think, your files will never stolen or read by others you can leave them empty.
  * Creating the keys will take some minutes



### Upload the key to a public key server

In order to share the key you can upload the key to a public key server. 

  * start seahorse
  * Edit > Preferences > Key servers > Publish key to: choose a key server. For example: hkp://pgp.mit.edu:11371. Close the dialog.
  * Remote > Sync and publish key > Sync.



### Remember the key ID

You need the key ID later. The key ID is shown in seahorse. But you can see it also via: 
    
    
    gpg --list-keys
    

## Updating repository files

Put the following script into /var/www/lazarus, edit it for your needs and run it: 
    
    
    #!/usr/bin/env bash
    
    set -x
    
    GPGHome=/home/gaertner/.gnupg/
    MainDir=dists/lazarus-testing
    
    for Arch in i386 amd64; do
      Dir=$MainDir/universe/binary-$Arch
    
      # create index
      apt-ftparchive packages $Dir > $Dir/Packages
      cat $Dir/Packages | gzip -9c > $Dir/Packages.gz
      cat $Dir/Packages | bzip2 > $Dir/Packages.bz2
    done
    
    # create Release file
    rm -f $MainDir/Release*
    Date=`date`
    echo "Origin: Lazarus" >> $MainDir/Release
    echo "Label: Lazarus" >> $MainDir/Release
    echo "Suite: unstable" >> $MainDir/Release
    echo "Codename: lazarus-testing" >> $MainDir/Release
    echo "Version: 1.0" >> $MainDir/Release
    echo "Date: $Date" >> $MainDir/Release
    echo "Architectures: amd64 i386" >> $MainDir/Release
    echo "Components: universe" >> $MainDir/Release
    echo "Description: Lazarus testing 1.0" >> $MainDir/Release
    
    apt-ftparchive release $MainDir >> $MainDir/Release
    
    # sign Release file
    gpg --sign --homedir=$GPGHome -ba -o $MainDir/Release.gpg $MainDir/Release
    
    # end.
    

This will create index files _Packages_ , _Packages.bz2_ and _Packages.gz_. And it will create the _Release_ file containing the checksums of the deb packages and sign it (_Release.gpg_). 

## Adding the repository to a client

_IMPORTANT_ : If you only came here to download lazarus for ubuntu/debian then use the stable repository [Ubuntu repository](<Getting_Lazarus.md> "Getting Lazarus"). The repository below is an unstable, testing repository. 

The following steps must be done on each computer, you want to use your repository. 

### Add the key

Download the key from the public key server: 
    
    
    gpg --keyserver hkp://pgp.mit.edu:11371 --recv-keys 3A5B1204
    

The 3A5B1204 should be replaced with your key id. Check the output, that you got the right key. 

Add it to the apt system: 
    
    
    gpg --export 3A5B1204 | sudo apt-key add -
    

You can see the list of apt keys with: 
    
    
    sudo apt-key list
    

### Add the repository

You can use synaptic for this or edit the /etc/apt/sources.list directly. Add the line: 
    
    
    deb http://progprak.scale.uni-koeln.de/lazarus/ lazarus-testing universe
    

Replace the http path with your own. 

## Install Lazarus

For example: 
    
    
    sudo apt-get update
    sudo apt-get install lazarus

---

_Source: [https://wiki.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository](https://web.archive.org/web/20230708190033/https://wiki.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository)_
