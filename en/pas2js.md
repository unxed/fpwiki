# pas2js.cfg

## Contents

  * 1 Locations
  * 2 Structure
  * 3 Macros
  * 4 Examples
  * 5 Creating pas2js.cfg



# Locations

You can see where [pas2js](<pas2js.md> "pas2js") searches for its cfg file and what it finds using the command line parameter **-vt**. 

On Unix (Linux, MacOS, BSD) pas2js searches 

  * $HOME/.pas2js.cfg - note the dot at the beginning of _.pas2js.cfg_
  * in the directory of the compiler binary. For example if pas2js is installed in _/usr/lib/fpc/3.0.4/bin/pas2js_ , then it searches _/usr/lib/fpc/3.0.4/bin/pas2js.cfg_
  * in _/etc/pas2js.cfg_



On Windows the compiler searches: 

  * %HOME%\pas2js.cfg
  * in the directory of the compiler binary. For example if pas2js is installed in _C:\pascal\pas2js\bin\pas2js_ , then it searches _C:\pascal\pas2js\bin\pas2js.cfg_



You can disable the search for the default config file using the parameter **-n**. 

You can pass additional config files using the @ parameter. For example: 
    
    
    pas2js -n @/etc/pas2js.cfg
    

This will skip the default config file and parse _/etc/pas2js.cfg_. 

# Structure

  * Leading spaces and tabs are skipped.
  * Empty lines or lines only containing spaces are skipped
  * Lines beginning with **#** followed by a space or tab are comment lines
  * Lines beginning with #name are config directives: 
    * #IFDEF define
    * #IFNDEF define
    * #IF expression - same as $if, except only defines
    * #ELSEIF
    * #ELSE
    * #ENDIF
    * #ERROR text
  * Other lines are treated as single parameters. For example _-FuFol der_ is the same as passing _"-FuFol der"_ as [command line parameter](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/pas2js/docs/translation.html?view=co#commandlineparameters>).
  * Search paths (e.g. _-Fu_ or _-Fi_) in config files are searched from top to bottom, while on the command line they are searched from right to left.
  * For _defines_ see [Compiler directives](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/pas2js/docs/translation.html?view=co#compilerdirectives>)



# Macros

  * $Pas2jsFullVersion - major.minor.release<extra>
  * $Pas2jsVersion - major.minor.release
  * $CfgDir - The directory of this config file
  * $Env - environment variable, e.g. $Env(HOME)



You can see the supported macros running _pas2js -h_

# Examples

Here is a typical pas2js.cfg file: 
    
    
    #
    # Minimal config file for pas2js compiler
    #
    # -d is the same as #DEFINE
    # -u is the same as #UNDEF
    #
    # Write always a nice logo ;)
    -l
    
    # Display Warnings, Notes and Hints
    -vwnh
    # If you don't want so much verbosity use
    #-vw
    
    # Allow C-operators
    -Sc
    
    #IFDEF FPC_SUBTARGET_NAMESPACED
    -Fu$CfgDir/../../packages/*/*/namespaced
    -Fi$CfgDir/../../packages/*/*/src
    #else
    -Fu$CfgDir/../../packages/*/*/src
    #endif
    
    # path to rtl.js
    -Fu../../compiler/utils/pas2js/dist/
    
    #IFDEF nodejs
    -Jirtl.js
    #ENDIF
    
    # Put all generated JavaScript into one file
    -Jc
    
    # end.
    

# Creating pas2js.cfg

When you built via **make** , it creates a **pas2js.cfg** in _bin/$(TargetCPU)-$(TargetOS)/_. 

There is a tool to create a basic pas2js.cfg: _tools/createconfig/createconfig_

---

_Source: [https://wiki.freepascal.org/pas2js.cfg](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/pas2js.cfg)_
