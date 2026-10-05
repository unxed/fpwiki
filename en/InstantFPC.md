# InstantFPC

│ **[Deutsch (de)](</InstantFPC/de> "InstantFPC/de")** │  **English (en)** │  **[français (fr)](</InstantFPC/fr> "InstantFPC/fr")** │    
****

## Contents

  * 1 Overview
  * 2 Download
    * 2.1 Old versions
  * 3 Installation
  * 4 How it works
  * 5 Parameters for instantfpc
  * 6 Passing parameters to the compiler
  * 7 Example
  * 8 Using with Lazarus
    * 8.1 Compile in Lazarus
    * 8.2 Debug with Lazarus
    * 8.3 Use as Pascal script without an .lpi file
  * 9 Using in Apache
    * 9.1 Apache example showing env
  * 10 Bugs / ToDos / Limitations
  * 11 Alternatives



# Overview

This tool allows to run pascal programs as scripts under Linux, BSD, macOS and Windows. For example: 
    
    
    #!/usr/bin/env instantfpc
    begin
      writeln('Hello fpc user');
    end.
    

Save the file as hello.pas, set the permission to execute and run it: 
    
    
    chmod a+x hello.pas
    ./hello.pas
    Hello fpc user
    

**Notes:**

  * The first line is called _shebang_ and is stripped from the source before passing it to the compiler.
  * The line 'program hello;' is optional in FPC. Because **instantfpc** passes the **-o** option to the compiler the line is ignored, so you can omit it.



Of course you can pass parameters to your script: 
    
    
    #!/usr/bin/env instantfpc
    var
      i: Integer;
    begin
      for i:=0 to ParamCount do writeln(ParamStr(i));
    end.
    

Save the file as params.pas, set the permission to execute and run it: 
    
    
    chmod a+x params.pas
    ./params.pas A B
    

It gives output like: 
    
    
    /home/username/.cache/instantfpc/params.pas
    A
    B
    

# Download

No need to download: instantfpc is part of the FPC 2.6+ sources under _utils/instantfpc_ and installed by default. 

## Old versions

For fpc 2.5.1 and above: It is in the fpc sources: 
    
    
    svn co http://svn.freepascal.org/svn/fpc/trunk/utils/instantfpc instantfpc
    

For fpc 2.4.4 and 2.4.5: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/applications/instantfpc instantfpc
    

# Installation

Note: It is already installed when using fpc 2.5.1 and above. Windows users have to put the instantfpc.exe into a directory of their PATH. 

  1. Compile the instantfpc.lpi with Lazarus or lazbuild. Or compile directly with _fpc instantfpc.pas_.
  2. Put the executable into PATH, for example /usr/bin:


    
    
    sudo cp instantfpc /usr/bin/
    

# How it works

It uses a cache directory to compile. Default is $HOME/.cache/instantfpc. If HOME is not set it gives an error. You can override the directory by setting the environment variable INSTANTFPCCACHE. It compares the source with the stored file in cache and if it differs compiles the source, given the parameters in the shebang line (the first line of the script after #!). After successful compile it executes the program. If compilation fails it writes the fpc output to stdout. 

# Parameters for instantfpc

  * **-h** : Print help and exit
  * **-v** : Print current version and exit
  * **\--get-cache** : Print current cache directory and exit
  * **\--set-cache= <directory>**: Set cache directory
  * **-B** : always compile even if cache is valid.
  * **\--compiler= <path to compiler>** use this compiler. By default fpc is searched in PATH.
  * **\--skip-run** : Do not execute the program. Useful to test if script compiles. Best combined with -B.
  * **\--** : Read the program source from standard input. Note that the compiled program has no input then.
  * Environment variable **INSTANTFPCCACHE** : Set cache directory. The --get-cache can override this.
  * Environment variable **INSTANTFPCOPTIONS** : Options to pass to the compiler. Passed before the command line parameters.



# Passing parameters to the compiler

Compiler parameters can be passed in the shebang line. Note: You can not use the 'env' command. 
    
    
    #!/usr/bin/instantfpc -O1 -Ci
    begin
    end.
    

And/Or you can put compiler options in the environment variable **INSTANTFPCOPTIONS** : 
    
    
    export INSTANTFPCOPTIONS="-Fu~/lazarus/components/codetools/units/x86_64-linux/"
    
    
    
    #!/usr/bin/instantfpc
    uses FileProcs; // unit FileProcs is in codetools
    begin
    end.
    

# Example

The following example script delivers the (unintelligible!) numeronym of a string (e.g. i18n for internationalization) that may be provided either as a parameter or, if missing, be entered by the user: 
    
    
    #!/usr/bin/env instantfpc
    uses
      SysUtils;
    var
      theWord: String;
    begin  
      if ParamStr(1) = '' then
      begin
        write('Please enter the word to be numeronymized: ');
        readln(theWord);
      end
      else
        theWord := ParamStr(1);
      if length(theWord) > 2 then
        writeln(theWord[1] + IntToStr(length(theWord)-2) + theWord[length(theWord)])
      else
        writeln('N/A');
    end.
    

The following shows how you don't even need to have a source file to use instantfpc: 
    
    
    echo "begin writeln('hello, world!') end." | ./instantfpc --
    

# Using with Lazarus

Since Lazarus 0.9.31 the IDE ignores the shebang line. You get all the normal code features. 

Install the IDE package **instantfpclaz** , which comes with Lazarus. This will add a new project item to create instantfpc programs, for example under File / New / Project / InstantFPC program. 

## Compile in Lazarus

How to convert a Lazarus project to compile it with **instantfpc** : 

Requires: 

  * Lazarus project main source file is the instantfpc program



Setup: 

  * Go to Project Options / Compiler Options / Compilation
  * Disable all **Call on** checks for **Compiler**
  * Set in **Execute before** the **Command** to:


    
    
    instantfpc --skip-run -B -gl "-Fu$(ProjUnitPath)" $Name($(ProjFile))
    

  * Enable checkboxes to scan _make_ and _fpc_



Notes: 

  * The _-gl_ will tell FPC to compile with debugging information
  * The _\--skip-run -B_ compiles without executing
  * See [IDE Macros](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames")



## Debug with Lazarus

How to convert a Lazarus project to debug an instantfpc program: 

Requires: 

  * The above **Compile in Lazarus**



Setup: 

  * Go to Run / Run parameters
  * Set **Local / Host application** to


    
    
    $(InstantFPCCache)/$NameOnly($(ProjFile))
    

  * Set **Working Directory** to


    
    
    $(ProjPath)
    

## Use as Pascal script without an .lpi file

Open the script in the source editor. Then click in menu **Run / Configure Build and Run**. 

  * Page **Options** : 
    * **On build project execute the Build File command instead** : Enable this if you like to compile the script on Ctrl+F9.
    * **On run project execute the Run File command instead** : Enable this if you like to execute the script on F9. Note: **instantfpc** automatically compiles if the file has changed.
  * Page **Build** : 
    * '_Build Command_ : /path/to/instantfpc --skip-run -B -gl $(EdFile)
    * **Scan for FPC messages** : enable
  * Page **Run** : 
    * '_Build Command_ : /path/to/instantfpc -gl $(EdFile)



Click Ok. You should see something like this: 
    
    
    #!/usr/bin/instantfpc
    {%BuildCommand /path/to/instantfpc --skip-run -B -gl $(EdFile)}
    {%RunCommand /path/to/instantfpc -gl $(EdFile)}
    begin
    end.
    

# Using in Apache

This is how to use instantfpc in Apache Webserver. Instantfpc needs the cache directory, which must be writable by the account of Apache. Let's say Apache is running under user www-data, group www-data (Redhat, Fedora: apache.apache, see apache config for User and Group). 
    
    
    mkdir /var/apacheinstantfpc
    chown www-data.www-data /var/apacheinstantfpc
    

Pass an environment variable to cgi scripts by adding the following line to the Apache configs: 
    
    
    SetEnv INSTANTFPCCACHE /var/apacheinstantfpc
    

**Security advice:** Although the apache user must have full access to the cache directory, make sure the directory is not available from outside (e.g. do not put it under /var/www). It may contain old scripts. And like any other type of scripts: Check all input for attacks. 

**Optimization note:** The instantfpc cache uses one file per script name. **Do not use the same script name twice** , otherwise it will be compiled many times. You can create multiple cache directories. Each <directory> and <virtualhost> section can define their own environment variables, and therefore you can setup one cache directory for each. 

## Apache example showing env

Configure a directory to execute cgi scripts. For example: 
    
    
     ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
     <Directory "/usr/lib/cgi-bin">
       AllowOverride None
       Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
       Order allow,deny
       Allow from all
       SetEnv INSTANTFPCCACHE /var/apacheinstantfpc
       # You can put fpc options into /etc/fpc.cfg or here:
       #SetEnv INSTANTFPCOPTIONS -Fu/path/to/somevar/ppufiles
       # If this is root directory, you might have to set:
       #Require all granted
     </Directory>
    

If your apache has not yet enabled the **cgi** module: 
    
    
    a2enmod cgi
    

Put the following script into /usr/lib/cgi-bin/showenv.pas: 
    
    
    #!/usr/bin/instantfpc
    {$mode objfpc}{$H+}
    uses  SysUtils;
    var  i: Integer;
    begin
      writeln('Content-type: text/html');
      writeln;
      writeln('<HTML><BODY>');
      for i:=0 to Paramcount do
        writeln('Param ',i,' ',ParamStr(i),'<br>');
      for i:=1 to GetEnvironmentVariableCount do
        writeln('Env ',GetEnvironmentString(i),'<br>');
      writeln('</BODY></HTML>');
    end.
    

# Bugs / ToDos / Limitations

  * Changes to the compiler or installed units are not checked. If you install a new compiler you should clean the cache (e.g. delete the directory ~/.cache/instantfpc).
  * Under Windows only the first parameter is passed to the script.
  * With instantfpc versions less than 1.3 you can not compile in parallel. The compiler creates hidden files like 'link.res' while compiling. Since 1.3 instantfpc uses temporary directories for compiling.
  * The cache uses the file name without the path. So you cannot execute two scripts in parallel of the same name. Every run time the script will be compiled.



# Alternatives

The following trick works using the _bash_ shell. Put the program into a file _fpc_script.sh_ : 
    
    
    // 2>/dev/null; fpc fpc_script.pp &> build.log && exec ./fpc_script "$@" || cat build.log; exit
    begin
      writeln(' no one knows how this is a trick or offers any advantage at all ');
    end.

---

_Source: [https://wiki.freepascal.org/InstantFPC](https://web.archive.org/web/20250405140216/https://wiki.freepascal.org/InstantFPC)_
