# IDE Window: Configure Build file

│ **English (en)** │

Normally 'building' and 'running' menu items and shortcuts invokes building (running) the current project. 

With this dialog you can override this behaviour for each file. This is useful, if you created a small test program or a sub project. Then you can compile them without changing the project. These settings are stored with the project (.lpi file). They are not global settings. 

## Contents

  * 1 Build / Run File
    * 1.1 Options
    * 1.2 Build
      * 1.2.1 Working directory
      * 1.2.2 Build Command
      * 1.2.3 Scan for FPC messages
      * 1.2.4 Scan for Make messages
      * 1.2.5 Macros
    * 1.3 Run
      * 1.3.1 Always build before run
      * 1.3.2 Working directory
      * 1.3.3 Run Command
      * 1.3.4 Macros
  * 2 IDE directives



## Build / Run File

The IDE can build and run single files instead of the project. For example when pressing the _Run_ button, the IDE checks if you enabled this feature for the current source file in the source editor. If yes, the IDE executes the Run File command instead. The information how to build or run a file is stored as IDE directives in the source file itself and can be setup via the dialog described on this page (Run / Configure Build+Run File). 

### Options

When this file is active in the source editor ... 

To override the behaviour for run and/or build by checking the appropriate checkbox. 

  * On build project execute the Build File command instead
  * On run project execute the Run File command instead



### Build

#### Working directory

Before building, change to this directory. Relative paths in the build command will then be relative to this directory. 

#### Build Command

Provide the command to call for building the file. The default is: 
    
    
     $(CompPath) $(EdFile)
    

#### Scan for FPC messages

Parse output for FPC errors and hints. The IDE will jump to errors. 

#### Scan for Make messages

Parse output for 'Make' errors and hints. The IDE will jump to errors. 

#### Macros

[IDE Macros in paths and filenames](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames")

### Run

#### Always build before run

If checked, the Build File command is executed before running. 

#### Working directory

Before running, change to this directory. Relative paths in the run command will then be relative to this directory. 

#### Run Command

Provide the command to call to run file. The default is: 
    
    
     $MakeExe($(EdFile))
    

#### Macros

[IDE Macros in paths and filenames](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames")

## IDE directives

  * **BuildCommand**
  * **BuildWorkingDir**
  * **BuildScan**
  * **RunCommand**
  * **RunWorkingDir**
  * **RunFlags**



See [IDE directives](<IDE_directives.md> "IDE directives").

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Configure_Build_file](https://web.archive.org/web/20230530103410/https://wiki.freepascal.org/IDE_Window%3A_Configure_Build_file)_
