# macOS Frameworks

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

A framework is a bundle (a structured directory) that contains a dynamic shared library along with its associated resources, such as image files and header files. When you develop an application, your project links to one or more frameworks. Your code accesses the capabilities of a framework through the application programming interface (API), which is published by the framework through its header files. Because the library is dynamically shared, multiple applications can access the framework code and resources simultaneously. The system loads the code and resources of a framework into memory, as needed, and shares the one copy of a resource among all applications. A special kind of Framework is a system Plugin (or driver). 

Distributing dynamic libraries as Frameworks is unique to macOS. All macOS system APIs come in the form of a framework. Many third-party libraries also come in the form of Frameworks for macOS (eg SDL) while providing an option to be loaded as a regular dynamic library. 

## Contents

  * 1 Linking to a Framework
  * 2 Distributing an application with a Framework
    * 2.1 Troubleshooting
  * 3 Frameworks or Dynamic Libraries
  * 4 See also
  * 5 External links



## Linking to a Framework

The framework can be linked either via a source code: 
    
    
    {$linkframework SDL2}
    

or via a linker instructions: 
    
    
    -framework SDL2
    

For FPC command-line, each parameter should be prefixed with "-k": 
    
    
    -k-framework -kSDL2
    

Since the concept of Frameworks exists in macOS only, these instructions should be carefully isolated from other systems (otherwise they might produce compile time or link time errors). 

If linker produces an error such as "framework cannot be found", the search path for the framework must be specified (using the "-F" linker parameter): 
    
    
    -F /Library/Frameworks
    

**Note** : 

  * the linker search path will be set to the current system-wide SDK used, as configured by Xcode command-line tools.
  * any third party Frameworks should either be installed, or the path should be explicitly specified via -F parameter.
  * -F is only used for the developer machine at link-time. It doesn't affect how the framework is searched at the run-time



## Distributing an application with a Framework

[![](https://wiki.freepascal.org/images/9/92/frameworkconfigureoption.png)](</File:frameworkconfigureoption.png>)

[](</File:frameworkconfigureoption.png> "Enlarge")

Configuring search and rpath for frameworks in Lazarus

One of the beauties of the **bundles** system in macOS is that the application hides all of its necessary resources within a [bundle](<Application_Bundle.md> "Application Bundle"). Resources include library dependencies, such as third-party frameworks, which remove the need for the end user to have to install separately any needed third-party framework in their macOS system. 

(NEVER distribute system frameworks with your application!) 

The typical placement of a framework in **macOS** bundle is under "Frameworks" directory of "Contents": 
    
    
    MyApp.app/
      Contents/
        Frameworks/
          SDL2.Framework
          Else.Framework
          Foo.Framework
        Info.plist
        MacOS/
          myapp
        PkgInfo
        Resources/
    

(for iOS the placement is similar, but different) 

However, such framework will need to be loaded in runtime. Thus the linker MUST BE given an instruction to specify the "Frameworks" folder as a runtime framework load path. 

This is done by specifying additional linker command: 
    
    
    -rpath @loader_path/../Frameworks
    

  * "@loader_path" - is a special value that is recognized by the dynamic loader (see "man dyld") during the application load. For the macOS application bundle it would be the "MacOS" directory. As a result "@loader_path../Frameworks" will actually point to "Frameworks" folder of the bundle.



### Troubleshooting

If "rpath" was not specified, the application might fail to start. The error log (or running from the command line) might reveal an error like this: 
    
    
    user@user-mac MacOS % ./myapp 
     dyld: Library not loaded: @rpath/SDL2.framework/Versions/A/SDL2_mixer
        Referenced from: /Applications/MyApp.app/Contents/MacOS/./myapp
     Reason: image not found
    zsh: abort      ./myapp
    

In order to see what RPATH was specified by the linker, simply run the `otool -l myapp` command. At the end of the load commands list you should see an "LC_RPATH" entry (or entries): 
    
    
    Load command 28
            cmd LC_RPATH
        cmdsize 40
           path @loader_path/../Frameworks (offset 12)
    

If you see no LC_RPATH then none was specified. 

## Frameworks or Dynamic Libraries

Frameworks themselves are simply special directory structures that contain a dynamic library, and possibly headers and other associated resources. The same approach as application [bundles](<Application_Bundle.md> "Application Bundle"). 

In many cases it's possible to load a dynamic library that resides within a framework structure at run-time, without needing to link the framework. 

There's no restriction so far NOT to use dynamic libraries. The choice should be driven by the code maintenance tasks. Linking frameworks might be easier when it comes to an update (as there's no need to track the version of the library, if it needs to change). 

## See also

  * [Application Bundle](<Application_Bundle.md> "Application Bundle").
  * [macOS Dynamic Libraries](<macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries").
  * [macOS Static Libraries](<macOS_Static_Libraries.md> "macOS Static Libraries").
  * [macOS Libraries](<macOS_Programming_Tips.md> "macOS Programming Tips").



## External links

  * [Apple: Framework Programming Guide](<https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i>).

---

_Source: [https://wiki.freepascal.org/macOS_Frameworks](https://web.archive.org/web/20240227035254/https://wiki.freepascal.org/macOS_Frameworks)_
