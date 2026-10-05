# pas2js command line arguments

│ **English (en)** │

[Pascal](<Pascal.md> "Pascal") to [JavaScript](<JavaScript.md> "JavaScript") transpiler [pas2js](<pas2js.md> "pas2js") [command line](<Command-line_interface.md> "Command-line interface") arguments 

## Options
    
    
    Put + after a boolean switch option to enable it, - to disable it
      @<x>    : Read compiler options from file <x> in addition to the default
                pas2js.cfg
      -B      : Rebuild all
      -d<x>   : Defines the symbol <x>. Optional: -d<x>:=<value>
      -i<x>   : Write information and halt. <x> is a combination of the following:
        D     : Write compiler date
        SO    : Write compiler OS
        SP    : Write compiler host processor
        TO    : Write target platform
        TP    : Write target processor
        V     : Write short compiler version
        W     : Write full compiler version
      -F...   Set file names and paths:
       -Fe<x> : Redirect output to <x>. UTF-8 encoded.
       -Fi<x> : Add <x> to include paths
       -Fu<x> : Add <x> to unit paths
       -FU<x> : Set unit output path to <x>
      -I<x>   : Add <x> to include paths, same as -Fi
      -J...  Extra options of pas2js
       -Jc    : Write all JavaScript concatenated into the output file
       -Je<x> : Encode messages as <x>.
         -Jeconsole : Console codepage. This is the default.
         -Jesystem  : System codepage. On non Windows console and system are the same.
         -Jeutf-8   : Unicode UTF-8. Default when using -Fe.
       -Ji<x> : Insert JS file <x> into main JS file. E.g. -Jirtl.js. Can be
                given multiple times. To remove a file name append a minus, e.g.
                -Jirtl.js-.
       -Jl    : lower case identifiers
       -Jm    : generate source maps
         -Jmsourceroot=<x> : use x as "sourceRoot", prefix URL for source file names.
         -Jmbasedir=<x> : write source file names relative to directory x.
         -Jminclude : include Pascal sources in source map.
         -Jm- : disable generating source maps
       -Ju<x> : Add <x> to foreign unit paths. Foreign units are not compiled.
      -l      : Write logo
      -MDelphi: Delphi 7 compatibility mode
      -MObjFPC: FPC's Object Pascal compatibility mode (default)
      -NS<x>  : add <x> to namespaces. Namespaces with trailing - are removed.
                Delphi calls this flag "unit scope names".
      -n      : Do not read the default config files
      -o<x>   : Change main JavaScript file to <x>, "." means stdout
      -O<x>   : Optimizations:
        -O-   : Disable optimizations
        -O1   : Level 1 optimizations (quick and debugger friendly)
        -Oo<x> : Enable or disable optimization. The x is case insensitive:
          -OoEnumNumbers[-] : write enum value as number instead of name. Default in -O1.
          -OoRemoveNotUsedPrivates[-] : Default is enabled
          -OoRemoveNotUsedDeclarations[-] : Default enabled for programs with -Jc
      -P<x>   : Set target processor. Case insensitive:
        -Pecmascript5 : default
        -Pecmascript6
      -S<x>   : Syntax options. <x> is a combination of the following letters:
        c     : Support operators like C (*=,+=,/= and -=)
        d     : Same as -Mdelphi
        2     : Same as -Mobjfpc (default)
      -T<x>   : Set target platform
        -Tbrowser : default
        -Tnodejs  : add pas.run(), includes -Jc
      -u<x>   : Undefines the symbol <x>
      -v<x>   : Be verbose. <x> is a combination of the following letters:
        e     : show errors (default)
        w     : show warnings
        n     : show notes
        h     : show hints
        i     : show info
        l     : show line numbers
        a     : show everything
        0     : show nothing (except errors)
        b     : show file names with full path
        c     : show conditionals
        t     : show tried/used files
        d     : show debug notes and info, enables -vni
        q     : show message numbers
        x     : show used tools
      -vm<x>,<y>: Do not show messages numbered <x> and <y>.
      -?      : Show this help
      -h      : Show this help
    
    Macros:  $Name, $Name$ or $Name()
      $Pas2jsFullVersion: major.minor.release<extra>
      $Pas2jsVersion: major.minor.release
      $Env(): environment variable, e.g. $Env(HOME)
      $CfgDir: Use within a config file. The directory of this config file
    

## Config files

  * Empty lines are ignored
  * Lines starting with hash+space '# ' are comments
  * One option per line
  * Directives are started by #keyword, where keyword is case insensitive: 
    * #IFDEF macroname
    * #IFNDEF macroname
    * #IF expression - same as source directive $if, except only macronames
    * #ELSEIF
    * #ELSE
    * #ENDIF
    * #ERROR text

---

_Source: [https://wiki.freepascal.org/pas2js_command_line_arguments](https://web.archive.org/web/20221209192736/https://wiki.freepascal.org/pas2js_command_line_arguments)_
