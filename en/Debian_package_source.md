# Debian package source

Once a [Debian upstream](<Debian_upstream.md> "Debian upstream") is ready, it is possible to make a source package from it. This is the role of the maintainer. 

## Contents

  * 1 Requesting help
  * 2 debian directory
    * 2.1 changelog
    * 2.2 control
    * 2.3 copyright
      * 2.3.1 LGPL-2
      * 2.3.2 LGPL-2+
      * 2.3.3 LGPL-3-linking-exception
      * 2.3.4 GPL-3+
    * 2.4 rules
    * 2.5 watch
    * 2.6 source/format
    * 2.7 upstream/metadata
  * 3 Further reading



### Requesting help

You can request that someone would make the source package with an RFP bug: <https://wiki.debian.org/RFP>

It will be useful to the maintainer to know about the dependencies necessary to compile a Lazarus project: 

  * `fpc, lcl, lazarus` for all projects
  * `libqt5pas-dev` if the project uses Qt5



Though in practice, it is likely that you will have to make the package source yourself. Debian provides a number of tools to build and test a source package, as well as documentation on the process (see Further reading below). 

### debian directory

If you will do the role of the maintainer, or if you would like to check required dependencies, you will need to add the `debian` folder (in lowercase). Here is the minimal structure: 
    
    
    debian
      ├─ changelog        # changes with previous version
      ├─ control          # description of Debian packages
      ├─ copyright        # copyright information of the source code
      ├─ rules            # build script
      ├─ watch            # file to check for latest version
      ├─ source
      │   └─ format       # format of this package source
      └─ upstream
          └─ metadata     # upstream repository information
    

#### changelog

The first changelog of your program should be something like (spaces are important): 
    
    
    myproject (1.0-1) unstable; urgency=medium
    
      * Initial release. (Closes: #ITP_NUMBER)
    
     -- Firstname Lastname <email@server.com>  Sun, 1 Nov 2020 15:01:00 +0100
    

Of course you need to specify _myproject_ , _ITP_NUMBER_ , _Firstname_ , _Lastname_ , email and date. The version of the upstream program here is supposed to be 1.0 but it can be otherwise. The "-1" specifies the version of the package source as there a may be newer version of the package source but based on the same upstream version, in order to fix bugs for example. 

You may not have an ITP number yet, as it is a number you will get when you make the request for your package to be added to Debian. If you would like to be the maintainer, here is an example of e-mail to send. In this case the ITP number is 972503: <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=972503>

#### control

This file gives all the information on the Debian packages, what is needed to build it, etc. This format is a bit different from the one found inside the [.deb files](<Debian_package_structure.md> "Debian package structure"). 
    
    
    Source: myproject
    Section: graphics
    Priority: optional
    Maintainer: Firstname Lastname <email@server.com>
    Build-Depends: fpc, lcl, lazarus, libqt5pas-dev, debhelper-compat (= 13)
    Rules-Requires-Root: no
    Standards-Version: 4.5.0
    Homepage: <https://mywebsite/myproject>
    Vcs-Git: <https://github.com/myorg/myproject.git>
    Vcs-Browser: <https://github.com/myorg/myproject>
    
    Package: myproject
    Architecture: any
    Depends: ${misc:Depends}, ${shlibs:Depends}
    Conflicts: myoldproject
    Description: Short description
     More details, blablabla
    

Here we suppose it is a graphics application using qt5 and with its repository on GitHub. If the project does not use qt5, you can remove the build-dependency `libqt5pas-dev`. 

Note that the empty line is important. It introduces one package and there can be multiple packages in one package source, for example one for each widgetset and one common package. 

#### copyright

Basically a copyright file contains a reference to all copyrights in the source code, as well as a global copyright for the package and for the debian folder. If you include custom packages in your upstream (that are statically linked), you need to specify the copyright for them as well. The first part of the file looks like this: 
    
    
    Format: <https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/>
    Upstream-Name: mypackage
    Upstream-Contact: Firstname Lastname <email@server.com>
    Source: <https://github.com/myorg/myproject>
    
    Files: *
    Copyright: 2011-2020 Firstname Lastname <email@server.com>
    License: GPL-3+
    
    Files: debian/*
    Copyright: 2011-2020 Maintainer <email@server.com>
    License: GPL-3+
    
    Files: custompackage1/*
    Copyright: 2012 Author <mail@server.com>
    License: LGPL-2+
    

Then the file contains an empty line and the description of the licenses. Those descriptions are also separated by empty lines. 

Make sure you look for all mention of copyright in the source code (using grep or search in files). Even if the code is opensource, it is better to provide the full name of the person that created the files in the first place and the year. This can be needed for legal purposes as writing the copyright statement in a digital file counts as declaring the copyright. It is valid up to 70 years after the death of the copyright owner. This is based on the American legislation though other legislations may apply as well. For example, in French legislation, there is no need to declare a copyright, just being identified as the author is sufficient. 

File format documentation: <https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/>

Complete list of licenses: <https://spdx.org/licenses/> and <https://spdx.org/licenses/exceptions-index.html>

##### LGPL-2

This is the license for libraries that can be used with commercial projects. With Lazarus, you need to add the linking exception, but it is not part of the SPDX list, so I would add it in the license description (second paragraph). 
    
    
    License: LGPL-2
     This program is free software; you can redistribute it and/or modify
     it under the terms of the GNU Lesser General Public License as published
     by the Free Software Foundation; version 2, with the following modification:
     .
     As a special exception, the copyright holders of this library give you
     permission to link this library with independent modules to produce an
     executable, regardless of the license terms of these independent modules,
     and to copy and distribute the resulting executable under terms of your choice,
     provided that you also meet, for each linked independent module, the terms
     and conditions of the license of that module. An independent module is a
     module which is not derived from or based on this library. If you modify this
     library, you may extend this exception to your version of the library, but
     you are not obligated to do so. If you do not wish to do so, delete this
     exception statement from your version.
     .
     This program is distributed in the hope that it will be useful,
     but WITHOUT ANY WARRANTY; without even the implied warranty of
     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
     GNU Lesser General Public License for more details.
     .
     You should have received a copy of the GNU Lesser General Public License
     along with this program; if not, write to the Free Software Foundation,
     Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
     .
     On Debian systems, the full text of the GNU Lesser General Public
     License version 2 can be found in the file
     '/usr/share/common-licenses/LGPL-2'.
    

##### LGPL-2+

Same as above, but not limited to version 2. 
    
    
    License: LGPL-2+
     This program is free software; you can redistribute it and/or modify
     it under the terms of the GNU Lesser General Public License as published
     by the Free Software Foundation; either version 2 of the License, or
     (at your option) any later version, with the following modification:
     .
     As a special exception, the copyright holders of this library give you
     permission to link this library with independent modules to produce an
     executable, regardless of the license terms of these independent modules,
     and to copy and distribute the resulting executable under terms of your choice,
     provided that you also meet, for each linked independent module, the terms
     and conditions of the license of that module. An independent module is a
     module which is not derived from or based on this library. If you modify this
     library, you may extend this exception to your version of the library, but
     you are not obligated to do so. If you do not wish to do so, delete this
     exception statement from your version.
     .
     This program is distributed in the hope that it will be useful,
     but WITHOUT ANY WARRANTY; without even the implied warranty of
     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
     GNU Lesser General Public License for more details.
     .
     You should have received a copy of the GNU Lesser General Public License
     along with this program; if not, write to the Free Software Foundation,
     Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
     .
     On Debian systems, the full text of the GNU Lesser General Public
     License version 2 can be found in the file
     '/usr/share/common-licenses/LGPL-2'.
    

##### LGPL-3-linking-exception

The version 3 has a specific name when there is the linking exception. 
    
    
    License: LGPL-3-linking-exception
     This program is free software; you can redistribute it and/or modify
     it under the terms of the GNU Lesser General Public License as published
     by the Free Software Foundation; version 3, with the following modification:
     .
     As a special exception, the copyright holders of this library give you
     permission to link this library with independent modules to produce an
     executable, regardless of the license terms of these independent modules,
     and to copy and distribute the resulting executable under terms of your choice,
     provided that you also meet, for each linked independent module, the terms
     and conditions of the license of that module. An independent module is a
     module which is not derived from or based on this library. If you modify this
     library, you may extend this exception to your version of the library, but
     you are not obligated to do so. If you do not wish to do so, delete this
     exception statement from your version.
     .
     This program is distributed in the hope that it will be useful,
     but WITHOUT ANY WARRANTY; without even the implied warranty of
     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
     GNU Lesser General Public License for more details.
     .
     You should have received a copy of the GNU Lesser General Public License
     along with this program; if not, write to the Free Software Foundation,
     Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
     .
     On Debian systems, the full text of the GNU Lesser General Public
     License version 3 can be found in the file
     '/usr/share/common-licenses/LGPL-3'.
    

##### GPL-3+

This is the recommended license when possible. 
    
    
    License: GPL-3+
     This program is free software; you can redistribute it and/or modify
     it under the terms of the GNU General Public License as published by
     the Free Software Foundation; either version 3 of the License, or
     (at your option) any later version. 
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
     License version 3 can be found in the file
     '/usr/share/common-licenses/GPL-3'.
    

#### rules

The `rules` file contains build instructions. If you are building a simple package, it will be the following: 
    
    
    #!/usr/bin/make -f
    
    # see FEATURE AREAS in dpkg-buildflags(1)
    export DEB_BUILD_MAINT_OPTIONS = hardening=+all
    
    %:
    	dh $@ --no-parallel
    

The `--no-parallel` flag is needed to avoid potential problems with parallel builds. 

#### watch

The `watch` file explains how to check for latest version. For example, on GitHub, that would be: 
    
    
    version=4
    opts=filenamemangle=s/.+\/v?(\d\S+)\.tar\.gz/myproject-$1\.tar\.gz/ \
      <https://github.com/myorg/myproject/tags> .*/v?(\d\S+)\.tar\.gz
    

The version 4 is the for the file format, not your program. You need to change the _myorg_ and _myproject_ to your actual project. 

See documentation: <https://wiki.debian.org/debian/watch>

#### source/format

At the moment of this writing, it is: 
    
    
    3.0 (quilt)
    

#### upstream/metadata

For a project on GitHub, that would be: 
    
    
    Bug-Database: <https://github.com/myorg/myproject/issues>
    Bug-Submit: <https://github.com/myorg/myproject/issues/new>
    Repository: <https://github.com/myorg/myproject.git>
    Repository-Browse: <https://github.com/myorg/myproject>
    

You may need to change those path according to the tools you are using. 

### Further reading

  * <https://wiki.debian.org/BuildingTutorial>
  * <https://www.debian.org/doc/manuals/maint-guide/first.en.html>
  * <https://lintian.debian.org/tags.html> and <https://lintian.debian.org/sources/fpc>

---

_Source: [https://wiki.freepascal.org/Debian_package_source](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Debian_package_source)_
