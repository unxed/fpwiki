# Using the LCL without Lazarus

## Contents

  * 1 Introduction
    * 1.1 Why would I want to use LCL and not use Lazarus?
    * 1.2 Is this really possible?! Don't you need Lazarus to build a Lazarus project?!
    * 1.3 Why would I want to build a Lazarus project without Lazarus?
  * 2 What do I really need to build my projects?
    * 2.1 If you are downloading Lazarus from the website
      * 2.1.1 Windows
      * 2.1.2 GNU/Linux
    * 2.2 If you are installing Lazarus from repositories
  * 3 So, how to build a Lazarus project without Lazarus?
    * 3.1 The easy way
    * 3.2 The hard way
    * 3.3 If you still can not find the correct directories
    * 3.4 Using LCL with Free Pascal IDE



# Introduction

### Why would I want to use LCL and not use Lazarus?

If you want to use LCL without Lazarus, usually it means one of two things: 

  * You want to use LCL in your existing Free Pascal project without switching it to Lazarus;
  * **You are looking for a way to build a Lazarus project without Lazarus**.



  


### Is this really possible?! Don't you need Lazarus to build a Lazarus project?!

**Lazarus** is an **IDE** \- a tool to make development easier. It uses the Free Pascal Compiler installed on your system to build its projects. Naturally, you do not need the **graphical development tool** to simply build a Free Pascal program from sources; you only need it help you develop that software easier. Basically, **it is not "a program written in Lazarus" - it is "a program written in Free Pascal using the Lazarus tool"**. 

When you download a source package on GNU/Linux and build it, you do not need to install _Oracle Eclipse_ or _Microsoft Development Studio_ (or whatever tool the developers used to write that program). You only need a compiler for the language the program is written in. 

Lazarus comes with a component library called _LCL (Lazarus Component Library)_. That library contains most of the "visual" components you use with Lazarus: forms, buttons. etc. Because of this, most people have gone under a false impression that you can only build **Lazarus** projects using the **Lazarus IDE**. That is not correct; to **build** an application created with Lazarus, you need **LCL** , but you do not need Lazarus. 

  


### Why would I want to build a Lazarus project without Lazarus?

  * If you're only beginning to learn Pascal, you may want to see how it all works "under the hood" before having the confidence to use an IDE that just _automagically_ builds your project for you.


  * Just as a proof that you are actually developing a Free Pascal program potentially compatible with other Pascal compilers, not a Lazarus-only project.


  * If you already are developing a Free Pascal program, you can use the Lazarus libraries without having to use Lazarus itself. Also, you may want proof that if you decide to use Lazarus in developing part of this program, you won't lock yourself out of your usual Free Pascal-only workflow. Also, you may actually develop another project in Lazarus and copy parts of it into your Free Pascal program. Also, in some schools, students are required to "write a program in Pascal"; they may want to use a convenient tool like Lazarus IDE to do it, but their results will be examined by compiling it with a Free Pascal compiler, so they will have to know how to do it.


  * The most generic reason would be to avoid too many build dependencies. You may develop an application and distribute its sources. Some people will just want to build it, not develop it. Therefore, they should not be required to install the whole Lazarus development environment - it should be enough to only install the required libraries. Imagine downloading a simple console utility and having to install _Microsoft Visual Studio_ or _Oracle Eclipse_ just to build it! Not to mention there is no _Microsoft Visual Studio_ for most platforms at all.


  * You may want to build it yourself on another machine, or a 32-bit Intel Atom laptop, or a Raspberry Pi (with a memory card that is small and prone to wear), or an automated build server that runs many containers in RAM. And you may not want to install too much unnecessary software just to build it - because you want to save space on your memory card or tmpfs mount, or simply to keep the system as clean as possible.


  * You may have some other reason to do it; for example, _for science_.


  * You may want to compile (using FPC) a command line app that uses LCL non GUI units, eg LazUTF8 or LazFileUtil that work nicely with UTF8,



# What do I really need to build my projects?

### If you are downloading Lazarus from the website

#### Windows

If you are using Windows and downloading Lazarus from the website, then you will get everything in one folder: Lazarus, LCL and Free Pascal Compiler. If you just want to build Lazarus projects, then you don't need Lazarus itself, and you can safely delete it. Then you can pack what you really need and copy it to your build machine. 

You only need the following items: 

  * `components`
  * `fpc`
  * `lcl`
  * `packager`
  * `units`
  * `lazbuild`



Also, you don't need to "install" anything - just copy those folders. 

#### GNU/Linux

If you're downloading a GNU/Linux version of Lazarus from the website, then you will get a DEB package, not just an archive. It's generally not a good idea to mess with it, so you'll just have to install it as it is. It will be a much more complex task to remove Lazarus from it (and most likely not worth the result). If you really want to avoid installing Lazarus, then see the next section. Alternatively, you may want to build Lazarus from source to get it all in one directory, but that is out of the scope of this article. 

  


### If you are installing Lazarus from repositories

On GNU/Linux, if you prefer to install your stuff from the repositories, then it may depend onyour distribution's packaging specifics, but here is what I can tell about Debian Jessie and Raspbian Jessie (so it probably applies to most other distributions, too). 

What you need is: 

  * `fp-compiler` \- the Free Pascal Compiler. You don't need the whole Free Pascal IDE (though it's less than 5 Mb total);
  * Free Pascal units that you use in your program (RTL, FCL and others);
  * LCL (without Lazarus).



The easiest way to install them is: 

  * First, install `lcl --no-install-recommends` . LCL recommends Lazarus, but you don't need it, so you have to specify that you don't want it. `lcl` depends on `fp-compiler` and some essential units.
  * Second, you can manually choose and install all the necessary `fp-units-*`, but you'll probably end up needing all of them, and they don't take much space anyway, so you can simply install `fpc` that depends on most of them. `fp-units-gfx` and `fp-units-multimedia` depend on quite a few development packages, but you will probably need at least fp-units-gfx if you want to use forms in your application.



This way, you do not have to install another 400 megabytes worth of Lazarus IDE, documentation and sources for Free Pascal Compiler. 

  


# So, how to build a Lazarus project without Lazarus?

### The easy way

Use `lazbuild`. It does **NOT** launch Lazarus; it's just a wrapper for `fpc`. Running `lazbuild project1.lpr` should do it. 

If you've installed LCL from the repositories, you'll be happy to know that `lazbuild` is included in the `lcl` package, not in `lazarus` package, so you can simply use it. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Remember that you will have to specify the full path to the `lazbuild` executable unles you have its path added to the `PATH` environment variable.

  


### The hard way

If you want to manually invoke `fpc` to build your project, then: 

  * You have to point it at the `project.lpr` file. Don't let the extensions confuse you; despite its extension, it's just a normal Free Pascal program. You can view all those files with your text editor, and you'll see that those are either just normal Free Pascal units, or XML files that store some preferences information for Lazarus. The `.lpr` file is the Free Pascal "program" file - the one that has `program project1;` in it, and `uses` other units.


  * However, if you simply run `fpc project1.lpr`, you will get an error, because most likely your application `uses` units from LCL, but since LCL is not a standard part of Free Pascal, `fpc` does not know where to look for it. The exact paths may vary greatly between different systems and different versions, so you can simply find out the correct paths yourself; what's even better, you can see exactly how does Lazarus invoke the compiler - with all the options passed to it!



On your development system (the one where you do have the complete Lazarus installation) open your project, click "Project" - "Project Options", and then click the "Show options" button in the bottom. Here is the list of all compiler options used in compiling the project. You can read the `fpc` documentation and find out the meanings of all those options, but what we need most is the unit directories: the ones that start with `-Fu` and have paths in them. 

If you want to build your project on the same system, then you can simply use those options without changing them. If you are planning to use them on another system, then you'll have to deduce the correct paths according to where you put the library folders. You need all three or four paths, not only the ones that include "`lcl`"! 

You can either append them to the compiler command line (**BEFORE** `"project1.lpr"`), or you can add them to the compiler configuration file. On GNU/Linux, it is customary to pass them in a command line - that's what Lazarus does. 

The `fpc` configuration file is located either in `/etc/fpc.cfg` , or in `<LazarusDir>/fpc/<FpcVersion>/bin/<YourPlatform>/fpc.cfg` . Just append those lines in the end of the file. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Remember that you will have to specify the full path to the `fpc` executable unless you have its path added to the `PATH` environment variable.

To compile an app that uses (eg) LazUTF8, an LCL unit, you might - 

`fpc -Fu/usr/share/lazarus/1.8.0/components/lazutils/lib/x86_64-linux -Fu. project1.pas`

### If you still can not find the correct directories

If you are still struggling with those paths - for example, if you don't have Lazarus installed at all and you want to use LCL from your Free Pascal application, or you have Lazarus installed on Windows and have no idea where is LCL on GNU/Linux when installed from the repositories: 

On GNU/Linux systems, the Lazarus directory is usually in `/usr/lib/lazarus` or `/usr/share/lazarus`. You can probably find it by running `whereis lazarus` . All the other directories should be there. 

The four paths you need to specify are something like this: 
    
    
    -Fu<YourLazarusDir>/lcl/units/<YourPlatform>
    -Fu<YourLazarusDir>/lcl/units/<YourPlatform>/<YourToolkit>
    -Fu<YourLazarusDir>/components/lazutils/lib/<YourPlatform>
    -Fu<YourLazarusDir>/packager/units/<YourPlatform>
    

For example, on a 64-bit Debian: 

`-Fu/usr/lib/lazarus/1.2.4/lcl/units/x86_64-linux/gtk2`

Or: 

`-FuD:\Lazarus\lcl\units\i386-win32\win32`

Take a look in those directories; there is usually only one folder to choose from - not much room for a mistake. 

  


### Using LCL with Free Pascal IDE

It does not have to be a Lazarus project; you can build your normal Free Pascal project that uses LCL the same way. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For some reason, I could not make `fp` build anything that uses LCL. I've tried adding those lines into `fp.cfg` and `fpc.cfg`, and it did build once, but then it suddenly stopped working. `fpc`, however, builds everything just fine. If somebody manages to do that, please update this page.

---

_Source: [https://wiki.freepascal.org/Using_the_LCL_without_Lazarus](https://web.archive.org/web/20241009175239/https://wiki.freepascal.org/Using_the_LCL_without_Lazarus)_
