# pas2js minifier

## Contents

  * 1 Overview
  * 2 Omit unneeded code
  * 3 Third party minifier
    * 3.1 Windows
    * 3.2 Linux/MacOS



# Overview

This page explains ways to reduce and/or compress the pas2js generated JavaScript. 

For a complete list of optimizations, including ideas not yet implemented, see [Pas2js optimizations](<Pas2js_optimizations.md> "Pas2js optimizations")

# Omit unneeded code

  * Use pas2js option -Jc to generate one JavaScript program. This allows the compiler to use Whole Program Optimization (wpo) to determine all unused identifiers.
  * Use -O1 to omit unused identifiers. If a record/class is used, every unused member/method is omitted.
  * By default published properties/methods are kept and will generate type information (RTTI). If you don't need that you can ... 
    * use {$modeswitch omitrtti} (since pas2js 1.1), which treats class section 'published' as 'public' and _typeinfo()_ does not work on symbols declared with this switch.
    * use -OoRemoveNotUsedPublished to omit all published properties not referenced at compile time.



# Third party minifier

There are several free minifiers for JavaScript. A minifier removes unneeded characters and replaces some code without changing its functionality. 

**Sourcemaps** : Keep in mind that third party minifiers usually makes the source map of pas2js unusable, so use the third party minifier only for the release version. 

As an example here is how to use uglifyjs with pas2js: 

## Windows

  * Download and install "node" form <https://nodejs.org/en/download/>
  * Start the "Node.js command prompt". This starts a console, which is setup with an environment for _Node.js_ and _npm_.
  * In the prompt type


    
    
    npm install uglify-js -g
    

and press <enter> This will download and install uglifyjs as a command line app. Test it by typing 
    
    
    uglifyjs -h
    

This should give the list of uglifyjs options. 

  * After install restart applications like Lazarus so they get the new PATH, which is needed by uglifyjs to work.
  * Find out the path of uglifyjs.cmd, for example by typing _where uglifyjs_
  * In Lazarus: 
    * With pas2js 1.1: Add to the custom compiler options **"-JpcmdC:\path\of\uglifyjs.cmd"**. Note that _-Jpcmd_ is case sensitive.
    * With pas2js 1.0: Set the _Execute After_ command: _C:\path\of\uglifyjs.cmd -oproject1.js project1.js_. Replace _project1.js_ with the name of your js file.



## Linux/MacOS

  * Install npm
  * Open a terminal and do:


    
    
    sudo npm install uglify-js -g
    

This will download and install uglifyjs as a command line app. Test it by typing 
    
    
    uglifyjs -h
    

This should give the list of uglifyjs options. 

  * In Lazarus: 
    * With pas2js 1.1: Add to the custom compiler options **-Jpcmduglifyjs**. This is the option _-Jpcmd_ plus the path to _uglifyjs_.
    * With pas2js 1.0: Set the _Execute After_ command: _uglifyjs -oproject1.js project1.js_. Replace _project1.js_ with the name of your js file.

---

_Source: [https://wiki.freepascal.org/pas2js_minifier](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/pas2js_minifier)_
