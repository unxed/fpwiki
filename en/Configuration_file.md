# Configuration file

## Contents

  * 1 Description
  * 2 Format
    * 2.1 Comments
    * 2.2 Conditional Processing
    * 2.3 Example
  * 3 Name
  * 4 Create
  * 5 Location
  * 6 Not Combined
  * 7 Ignore



## Description

To avoid having to include compiler options in build files or repeat them on the command line, Free Pascal allows them to be placed in a configuration file. The compiler options in the file will be read along with options given on the command line, with the configuration file being processed first. 

## Format

One _command line option_ is written per line with the same format as when used on the command line. 

### Comments

Comment lines are allowed. They follow the Unix shell/Python/Perl comment-style: a '#' signifies the start of a comment. 

### Conditional Processing

Special comment directives are available to allow a configuration file to be conditionally processed. Available directives are: 

  * #CFGDIR - use the directory specified in this directive as the location to find files named in subsequent #INCLUDE directives.
  * #SECTION
  * #WRITE - print a string



Directives that are equivalent to Free Pascal source code compiler directives. For example, #DEFINE in a config file is equivalent to {$DEFINE } in source code. 

  * #DEFINE
  * #ELSE
  * #ENDIF
  * #IFDEF -
  * #IFNDEF
  * #INCLUDE
  * #UNDEF



### Example
    
    
    # Perform range checking, IO checking, overflow checking, IEEE error checking
    -Crio3
    # compiler halts after warnings
    -Sew
    

## Name

The file is named **fpc.cfg** , unless it is located in the HOME directory on a Unix-like system, in which case it will follow Unix configuration file convention and start with a '.' ( **.fpc.cfg** ) . 

## Create

Example for Windows: Open a command prompt and go to the directory where fpc.exe is. This command will create a fpc.cfg for your installation and it will be placed in the current directory. 
    
    
    $> fpcmkcfg -d basepath=C:\fpc\ -o .\fpc.cfg
    

Example for Linux : in this case, FPC has been installed in the user's home dir and path setting added to $PATH (so both fpc and fpcmkcfg can be found). 
    
    
    $> fpcmkcfg -d basepath="$HOME"/bin/FPC/fpc-3.2.4 > "$HOME"/.fpc.cfg
    

Or, because fpc defines $fpcversion, the following is fpc version independent : 
    
    
    $> fpcmkcfg -d basepath="$HOME/bin/FPC/fpc-""\$fpcversion" > "$HOME"/.fpc.cfg
    

See fpcmkcfg -? for meaning of commandline switches. 

## Location

  * Under a Unix or Unix-like operating systems (e.g. FreeBSD, Linux)


  1. The current directory.
  2. Your home directory, it looks for .fpc.cfg.
  3. The directory specified in the environment variable PPC_CONFIG_PATH, and if it is not set, it will look in the etc directory above the compiler directory. (For instance, if the compiler is in /usr/local/bin, it will look in /usr/local/etc)
  4. The directory /etc.


  * Under all other operating systems:


  1. The current directory.
  2. If it is set, the directory specified in the environment variable PPC_CONFIG_PATH.
  3. The directory where the compiler is.



## Not Combined

Unlike Unix shell configuration files, FPC configuration files are not automatically combined (user-specific and system-wide). It will only use the first one that it finds and ignore any others that may also be on the system, including the configuration file that comes with Free Pascal. However, you can use the #INCLUDE directive (identical to {$INCLUDE } in Free Pascal source code) to include another configuration file. For example, your personal fpc.cfg in the current directory file could include the /etc/fpc.cfg file that came with Free Pascal. 
    
    
    # To avoid having to duplicate options in the FPC-provided configuration file
    # that file can be included in a custom configuration file.
    # This is how the file would be included on a Unix-like system:
    
    #INCLUDE /etc/fpc.cfg
    
    # Then custom options can be added.
    
    -Sew
    

You also can specify additional configuration files by specifying them on the command line, preceded by the @ symbol: 
    
    
    fpc @/dir1/dir2/configfile2.cfg @subdir/configfile3.txt @configfile3.txt my_program.pp
    

## Ignore

You can tell the compiler to not use a configuration file by giving the **-n** command line option.

---

_Source: [https://wiki.freepascal.org/Configuration_file](https://web.archive.org/web/20250219042140/https://wiki.freepascal.org/Configuration_file)_
