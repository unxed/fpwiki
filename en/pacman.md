# pacman

## Contents

  * 1 Pacman
    * 1.1 A Makefile
    * 1.2 The PKGBUILD file
    * 1.3 The Process
    * 1.4 Using that Package
    * 1.5 References



## Pacman

Pacman is the package management tool on Arch Linux and many other distributions based on it. Such as Manjaro. The packages themselves are really the equivalent of a Deb Binary package, not a source package. 

The Arch packaging system uses a file, that should be called PKGBUILD, that contains everything the makepkg command needs to get your source code direct from its repository, build it and bundle everything into single compressed archive. If you already have a Makefile that builds and installs, or scripts that do the same thing, you will be pleasantly surprised just how easy it is to make a Pacman compatible package. 

Here we will step through the process of making an Arch package from a Lazarus project. The PKGBUILD file is pretty stripped down to close to the minimal needed, the package system includes processes to check the integrity of the input sources, test the final product and several other package related functions, all that we ignore. In a way, its nice how the system is not quite so prescriptive as a Debian Package. 

We assume the package to be build is in a repository such as github and everything needed is stored there. 

  


### A Makefile

If you already have a Makefile that works you definitely should use it here. What we need is the **default build** and an **install** make target. You can easily substitute a couple of conventional scripts if you prefer. In either case, you need to be able to build your package from the command line, calling either lazbuild or fpc. If your project uses other, external FPC/Lazarus packages, it makes a lot of sense to have your scripts download and build them as well. That way, anyone can build your project starting from just the PKGBUILD file. That is, you don't depend on an especially setup Lazarus. 

In my example here, building a tomboy-ng Arch package, I use a Makefile, the same one used to make a Debian Source package, that actually calls a bash script (buildit.bash) because I need to prebuild another Lazarus package, KControls. Further, I need to be able to build with either a distribution's standard install of FPC and Lazarus or "trunk" ones I have build. And, frankly, doing stuff in a bash script is a lot easier than in a Makefile ! But that is your choice, if you do everything in the Makefile, great, go for it ! 

You will see how the Makefile is used in the PKGBUILD file next. 

### The PKGBUILD file

On a system that uses pacman, you will find a prototype PKGBUILD file in /usr/share/pacman, it has many, if not all options that makepkg command finds interesting. But you do not need all of them, a minimal version, setup to use an existing Makefile is shown below - 

  

    
    
    # This file is used to build a pacman usable gtk2 package for tomboy-ng
    
    # Requirments : sudo pacman -S base-devel fpc lazarus-gtk2 unzip
    
    # Usage : makepkg --skipinteg 
    
    pkgname=tomboy-ng
    pkgver=0.34			# Remember to update this
    pkgrel=1			# inc this if releasing a re-package of same ver
    pkgdesc="Manage a large or small set of notes with a range of fonts and markup."
    arch=('x86_64')
    url="https://github.com/tomboy-notes/tomboy-ng"
    license=('BSD' 'GPL2' 'LGPL3')
    depends=('gtk2' 'wmctrl')
    makedepends=('fpc' 'lazarus-gtk2')
    provides=('tomboy-ng')
    
    #	we want to get the taged release version, could also target master
    #        https://github.com/tomboy-notes/tomboy-ng/archive/refs/tags/v0.34.tar.gz
    source=("https://github.com/tomboy-notes/$pkgname/archive/refs/tags/v$pkgver.tar.gz"
    	"https://github.com/davidbannon/KControls/archive/master.zip")
    noextract=('master.zip')	# leave kcontrols alone, I'll deal with it.
    
    prepare() {
    	cd "$pkgname-$pkgver"
    	unzip ../master.zip
    	mv KControls-master kcontrols
    }
    
    build() {
    	cd "$pkgname-$pkgver"
    	# ./configure --prefix=/usr
    	make
    }
    
    package() {
    	cd "$pkgname-$pkgver"
    	make DESTDIR="$pkgdir/" install
    }
    

We give it some basic information about the product, version number, what its called, how to find it, where to download the source from and what we will be needing. Then we define a few functions that will be called by makepkg and it will do everything needed. 

In the example, we allow makepkg to download KControls for us but tell it to not unzip it. In the prepare function, we unzip it and rename the directory to what the buildit.bash script, called from the Makefile, expects. Remember, my example does all the building in a bash script that gets called by my Makefile, you don't need to do that if you build everything in the Makefile. 

The variable, $pkgdir, comes from makepkg, our Makefile will, under a fakeroot, 'install' the application under a local dir of that name. That directory, along with some meta data, is compressed into the Package. 

### The Process

First, install prerequisites. 
    
    
    $> sudo packman -S base-devel fpc lazarus unzip [enter]
    

Then, make a new directory, copy your PKGBUILD there, change to it and run (the skipinteg switch is because we are not doing any of the integrity tests, its my code, I know its unchanged) - 
    
    
    $> makepkg --skipinteg [enter]
    

Thats it, makepkg will, if all goes well, make a file called, in this case, tomboy-ng-0.34-1-x86_64.pkg.tar.zst 

  


### Using that Package

To install that package on the current machine or some other Arch Package using machine, you would - 
    
    
    $> sudo pacman -U ./tomboy-ng-0.34-1-x86_64.pkg.tar.zst [enter]
    

As you would expect, pacman will resolve any dependencies and put your package files in the right places, make the menus work and remember where it put things. Neat ! 

  


### References

You are strongly recommended to read, at least, the following. I am sure a "proper" package requires a more complicated PKGBUILD file. 

  * Much more [detail](<https://wiki.archlinux.org/title/Creating_packages>)


  * All the options than can be specified in [PKGBUILD](<https://wiki.archlinux.org/title/PKGBUILD>)

---

_Source: [https://wiki.freepascal.org/pacman](https://web.archive.org/web/20230205141718/https://wiki.freepascal.org/pacman)_
