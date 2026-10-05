# Link on target

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS Classic](</Category:Mac_OS_Classic> "Category:Mac OS Classic") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Link_on_target.md>)** │

The -st option defers assembling and linking to be made manually after compiling with fpc. Instead a script is generated which, when is executed on the target, completes the build process. The name of the script is <target>_ppas. 

The script assumes the file tree is the same on the target as on the host, and that the script's directory is the current directory. Depending on the setup, this can be accomplished in different ways. If the target is on a different computer, the files have to be transfered via a network, pendrive or by some other means. If the target is a different operating system on the same machine, the files don't have to be moved physically. Instead, it is enough to shut down the host and start the target operating system. When compiling on Darwin (macOS) as host for (classic) Mac OS as target, one even does not have to restart, if the classic compatibility environment is installed. Instead just switch to [MPW](<MPW.md> "MPW") (which is running in the classic compatibility environment) and execute your script there (but read the remark below). 

A caveat when running the script in MPW: Because the script is created in Darwin, it has no Mac file type and creator. Because of this MPW refuses to accept it as a script. So you have to give it a Mac file type, which can be done with the following command in MPW: 
    
    
     SetFile -c 'MPS ' -t 'TEXT' filename
    

Read more about working with MPW in [MPW](<MPW.md> "MPW"). 

Note that some options given to the compiler, will also affect the generated script: 
    
    
     -a
    

... normally means that the compiler doesn't delete the generated assembler file. When used with -st ( or -s ) the generated script will, when executed, not delete the assembler file. This is good for debuging purposes.

---

_Source: [https://wiki.freepascal.org/Link_on_target](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/Link_on_target)_
