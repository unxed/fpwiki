# Compiler Mode

│ **English (en)** │

The [FPC](<FPC.md> "FPC") intends to be (in part) a free and open source alternative to commercial [Pascal](<Pascal.md> "Pascal") compilers. In order to achieve this a compiler switch determining the _compiler compatibility mode_ has been introduced. 

Every mode implicitly enables or disables certain syntax requirements or other language constructs. Some of these can be enabled or disabled on an individual basis by using so called _mode switches_ , see below. 

## modes

A compiler compatibility mode can be specified in [source code](<Source_code.md> "Source code") via the [global compiler directive](<global_compiler_directives.md> "global compiler directives") `{$mode}` or via the command line or `fpc.cfg(5)` parameter `-M`. 

The following nine compiler compatibility modes are recognized: 

Free Pascal ([`{$mode FPC}`](<Mode_FPC.md> "Mode FPC"), `-MFPC`)
    This is the original FPC mode. As of FPC 3.x this is the _default mode_ if neither the source code or the command line specifies a compiler compatibility mode explicitly.
Object Pascal ([`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC"), `-MobjFPC` or `-S2`)
    This mode adds extra functionality to the `FPC` mode, including but not limited to [classes](<Class.md> "Class"), [interfaces](<Interface.md> "Interface") and [exceptions](<Exceptions.md> "Exceptions").
[Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") ([`{$mode TP}`](<Mode_TP.md> "Mode TP"), `-MTP` or `-So`)
    This is the Turbo Pascal compatibility mode. It tries to be compatible to Borland TP 7.0, e. g. by disable function overloading.
[Delphi](<Delphi.md> "Delphi") ([`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi"), `-Mdelphi` or `-Sd`)
    This is the Delphi compatibility mode.
Delphi with Unicode (`{$mode DelphiUnicode}`, `-MdelphiUnicode`) [[since FPC 3.0.0](<FPC_New_Features_3.md> "FPC New Features 3.0")]
    Like `{$mode Delphi}` but with `unicodeString` as default `string` type.
[Mac Pascal](<Mac_Pascal.md> "Mac Pascal") ([`{$mode MacPas}`](<Mode_MacPas.md> "Mode MacPas"), `-MmacPas`) [since FPC 1.9.0]
    The Mac Pascal compatibility mode.
[GNU Pascal](<GNU_Pascal.md> "GNU Pascal") ([`{$mode GPC}`](<Mode_GPC.md> "Mode GPC"), `-MGPC` or `-Sp`) [removed since FPC 2.2.0]
    The GNU Pascal compatibility mode.
ISO 7185 [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") ([`{$mode ISO}`](<Mode_iso.md> "Mode iso"), `-MISO`) [[since FPC 2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0")]
    The ISO 7185 compliant compatibility mode.
[Extended Pascal](<Extended_Pascal.md> "Extended Pascal") ([`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal")) [since FPC 3.2]
    This is the extended Pascal mode. It tries to be as ISO 10206 compliant as possible.

Furthermore the special mode `default` reverts any specifications of a compiler compatibility mode. 

Since the specifications of compiler compatibility mode implicitly imposes rigorous changes and possibly implies inclusion of other modules, it is imperative to specify the directives prior any other. 

## mode switch

Since FPC 2.3.1 the global compiler directive [`{$modeSwitch}`](<modeswitches.md> "modeswitches") allows a selective selection of _some_ features, despite the chosen mode. 

A mode switch has to appear after any mode selections, otherwise the mode switches will be overwritten. 
    
    
    // omit @-address-operator while assigning to procedural variables, despite FPC mode
    {$mode FPC}
    {$modeSwitch classicProcVars+}
    

## see also

  * [§ “compiler modes” in the _Free Pascal User’s manual_](<https://www.freepascal.org/docs-html/user/userse33.html>)

---

_Source: [https://wiki.freepascal.org/Compiler_Mode](https://web.archive.org/web/20241206071426/https://wiki.freepascal.org/Compiler_Mode)_
