# Debian upstream

On Debian distribution, the upstream source is the code of the program that can be compiled for any target platform. It needs to be compilable using packages already available in the latest Debian distribution. 

If you want to distribute the .deb file directly, you may generate it following [Debian package structure](<Debian_package_structure.md> "Debian package structure"). 

## Contents

  * 1 Dependence on Lazarus custom packages
  * 2 Upstream source, package maintainer and sponsor
  * 3 Upstream source content
    * 3.1 Project and custom packages
    * 3.2 Command line manual
    * 3.3 Application description
    * 3.4 configure script
    * 3.5 Makefile
      * 3.5.1 Initialization
      * 3.5.2 Compiling
      * 3.5.3 Installation and package building
      * 3.5.4 Installation rule
      * 3.5.5 Uninstallation rule
  * 4 Compiling upstream



### Dependence on Lazarus custom packages

Generally Debian packages will use dynamically linked libraries, that will be provided as another Debian package. However in Lazarus, it is common to use statically linked libraries that are provided as Lazarus custom packages (the main file have `.lpk` extension). 

If you are using custom Lazarus packages, you will need to include them in the upstream source. You can do that by copying the custom packages source code into your projects repository or create an upstream repository that will contain your own project as well as all custom packages source code. 

Publishing a new version of your project on Debian will thus require you to copy the source of used packages with the adequate version number. It is recommended to specify the required package version in the project inspector, so that you won't use an outdated custom package for compilation and also that you will be able to know which version is supposed to be used. 

### Upstream source, package maintainer and sponsor

There are different levels when it comes to uploading packages into Debian: 

  * upstream source provided by the software developer
  * [Debian package source](<Debian_package_source.md> "Debian package source") provided by the package maintainer
  * upload done by the package sponsor/mentor



Note that it may be difficult to find a package maintainer so that the developer can as a matter of fact be the maintainer as well. This article talks only about the upstream source. 

The term sponsor/mentor does not refer to a financial support but to a Debian developer. 

### Upstream source content

The upstream source consists of: 

  * you project source code and custom Lazarus packages source code
  * compilation scripts
  * additional information about the program



It can be organized for example like this: 
    
    
    upstream
      ├─ myproject ...     # folder for main project (containing a .lpi file)
      ├─ custompackage ... # folder for custom package (containing a .lpk file)
      ├─ configure         # script to setup compilation
      ├─ Makefile          # compilation and installation script
      └─ info
          ├─ myproject.1         # command line manual
          ├─ myproject.desktop   # GUI application description
          └─ icons               # GUI icons
              ├─ 16x16.png
              ├─ 32x32.png
              ...
    

In all cases, the `debian` folder name is reserved for the Debian package so the upstream must not contain such folder unless it takes the responsibility of describing the Debian package. 

Common icon sizes: 16x16, 22x22, 24x24, 32x32, 36x36, 48x48, 64x64, 256x256, scalable, symbolic. Scalable and symbolic are in SVG format. You don't need to provide all of them, they will be stretched if necessary. 

#### Project and custom packages

It is recommended to use a specific folder for each custom package, though there is no restriction here. You just need to remember the path to those when writing the `Makefile`. 

#### Command line manual

The syntax is of the .1 file is a bit peculiar. There are some tools to help but you still need to know the syntax. 

  * description of the syntax: <https://liw.fi/manpages/>
  * editor with wizard and WYSIWYG preview mode: **gmanedit**
  * live preview editor (toggle to **AN mode**): <https://roperzh.github.io/grapse/>



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
    

#### Application description

To add your application to the Linux menu, you need to add a file called ***.desktop** containing: 
    
    
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

#### configure script

The script called `configure` is a script that is called initially before compilation. It can be used to detect the compilation environment and take appropriate actions. In the case of Lazarus, the bare minimum is to handle the _prefix_ parameter. Here is for example a simple script that will store the _prefix_ parameter in a file called `prefix`. 
    
    
    #!/bin/bash
    args=("$@")
    defaultprefix=/usr/local
    wantedprefix=$defaultprefix
    for param in "${args[@]}"
    do
    	if [ "$param" == "-h" ] || [ "$param" == "--help" ]; then
    		echo "Usage: ./configure [OPTIONS]"
    		echo ""
    		echo "    --prefix=PREFIX"
    		echo "        Specifies the install prefix."
    		echo "        By default prefix is \"$defaultprefix\"" 
    		echo "        For packages use \"/usr\""
    		exit 0
    	elif [ "${param:0:9}" == "--prefix=" ]; then
    		wantedprefix=${param:9}
    	else
    		echo "Warning: unknown option $param"
    	fi
    done
    echo "Prefix set to: $wantedprefix"
    echo $wantedprefix >prefix  # store the prefix into the file called "prefix"
    

Here are examples of calls: 
    
    
    ./configure                 # when testing the program locally
    ./configure --prefix=/usr   # when building the package
    

#### Makefile

##### Initialization

The Makefile is a sort of script. It does not have a shebang (line starting with "#!") and uses by default the minimalist shell `/bin/sh`. You can override it though and use bash with something like: 
    
    
    SHELL := /bin/bash
    

Note that in the `Makefile`, unlike a regular script, to access a variable one write `$(VARIABLE)` with brackets instead of `${VARIABLE}` with braces. To run a shell command and retrieve the result, the brackets will also be used, which can be a little bit confusing. 

Variables are set using `:=` operator. For example, to compute the actual target path, one can write the following: 
    
    
    prefix := $(shell cat prefix)   # retrieve the content of the file called "prefix"
    TARGET_DIR = $(DESTDIR)$(prefix)
    

When building locally, _DESTDIR_ will be empty so that the target path will be the same as _prefix_. When building the package though, _DESTDIR_ will indicate a temporary folder to use. 

To make things easier to update, let's define the name of your project (without the .lpi extension): 
    
    
    PROJECT_NAME := myproject
    

##### Compiling

After the initialization of variables, the Makefile consists of entries called rules. The syntax of a rule is the following: 
    
    
    rule1: require1 require2
    #... how to make it
    

Here the name of the rule is _rule1_ and it requires the rules _require1_ and _require2_ to be already made. This means that first _require1_ and _require2_ will be made and only afterwards _rule1_ will be made. The following lines contain the instructions to execute to make _rule1_. 

If a rule does not need any other, there won't be anything after the colon. In the case of a Lazarus project, one can simply write: 
    
    
    all:
    	lazbuild --build-mode=Release $(PROJECT_NAME)/$(PROJECT_NAME).lpi
    

The rule _all_ is the one invoked to compile the program. Note that before the instructions, a tab character is needed (not spaces). It is recommended to specify the build-mode, because there may be various build modes like Debug or for different widgetsets (gtk2, gtk3, qt4, qt5). So whatever build mode is active in the project, you will be assured of the build mode used to generate the Debian package. 

To clean the compiled files, there is a need for another rule: 
    
    
    clean:
    	rm -f $(PROJECT_NAME)/$(PROJECT_NAME)  # remove generated executable file
    	rm -rf lib             # remove object files
    

Note that the object files may be in another folder. Check the project options. 

For completeness, let's provide a rule to clean the whole building process: 
    
    
    distclean: clean clean_configure
    clean_configure:
    	rm -f prefix     # remove the "prefix" file generated by "configure" script
    

##### Installation and package building

The `Makefile` needs to be able to install files on the system or to put them in the package. Both will be done with the same code and when building a package, the target directory will instead be a temporary one. 

We will need additional folders for the installation. 
    
    
    BIN_DIR = $(TARGET_DIR)/bin                  # executable file
    SHARE_DIR = $(TARGET_DIR)/share              # other files
    RESOURCE_DIR = $(SHARE_DIR)/$(PROJECT_NAME)  # custom resources
    MANUAL_DIR = $(SHARE_DIR)/man                # man page
    ICON_DIR = $(SHARE_DIR)/icons/hicolor        # icons (multiple resolutions)
    PIXMAP_DIR = $(SHARE_DIR)/pixmaps            # pixmap (fallback icon)
    
    SRC_BIN_DIR = $(PROJECT_NAME)
    SRC_INFO_DIR = info
    SRC_ICON_DIR = $(SRC_INFO_DIR)/icons
    

One can retrieve all icons in the folder with: 
    
    
    SRC_ICONS = $(shell find "$(SRC_ICON_DIR)" -maxdepth 1 -type f -name *x*.png -exec basename {} .png ';')
    

##### Installation rule

The installation will require the _prefix_ file to exist. This can be specified like any other rule: 
    
    
    install: prefix
    	install -D "$(SRC_BIN_DIR)/$(PROJECT_NAME)" "$(BIN_DIR)/$(PROJECT_NAME)"
    	install -d "$(SHARE_DIR)/man/man1"
    	gzip -9 -n -c "$(SRC_INFO_DIR)/man/man1/$(PROJECT_NAME).1" >"$(SHARE_DIR)/man/man1/$(PROJECT_NAME).1.gz"
    	chmod 0644 "$(SHARE_DIR)/man/man1/$(PROJECT_NAME).1.gz"
    	install -D "$(SRC_INFO_DIR)/$(PROJECT_NAME).desktop" "$(SHARE_DIR)/applications/$(PROJECT_NAME).desktop"
    	install -D --mode=0644 "$(SRC_ICON_DIR)/48x48.png" "$(SHARE_DIR)/pixmaps/$(PROJECT_NAME).png"
    	for s in $(SRC_ICONS); do install -D --mode=0644 "$(SRC_ICON_DIR)/$$s.png" "$(ICON_DIR)/$$s/apps/$(PROJECT_NAME).png"; done
    

Note: 

  * copying is done using the `install` command. The -D option will create missing directories for the file and the -d option will create the specified directory. The option --mode=0644 must be used for data files.
  * the 48x48 icon is used as default pixmap.
  * there are no resources installed in this case but you can add them in a similar way using `install` command.



##### Uninstallation rule

A similar rule must be created to uninstall the software: 
    
    
    uninstall: prefix
    	rm -f "$(BIN_DIR)/$(PROJECT_NAME)"
    	rm -f "$(SHARE_DIR)/man/man1/$(PROJECT_NAME).1.gz"
    	rm -f "$(SHARE_DIR)/applications/$(PROJECT_NAME).desktop"
    	rm -f "$(SHARE_DIR)/pixmaps/$(PROJECT_NAME).png"
    	for s in $(SRC_ICONS); do rm -f "$(ICON_DIR)/$$s/apps/$(PROJECT_NAME).png"; done
    

### Compiling upstream

Once you have all upstream files ready, you can check it compiles by going into the upstream folder and doing: 
    
    
    ./configure
    make
    sudo make install
    #test the program
    sudo make uninstall
    

Note that before installing, make sure your program is not already installed on the system. Otherwise you won't be able to check that the installation was successful. 

To test that the command line is working, simply call your program or check the manual: 
    
    
    myproject
    man myproject
    

To test that the GUI is working, go in the Linux menu and find your program. Check that it is displayed with the appropriate icon. 

If everything works, the next step is to get your upstream packaged. You can request that someone take care of it by sending an RFP bug request: <https://wiki.debian.org/RFP>

If you don't get any answer, you may want to do the packaging yourself. See [Debian package source](<Debian_package_source.md> "Debian package source").

---

_Source: [https://wiki.freepascal.org/Debian_upstream](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/Debian_upstream)_
