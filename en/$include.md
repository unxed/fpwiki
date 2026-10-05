# $include

│ **[Deutsch (de)](</$include/de> "$include/de")** │  **English (en)** │    
****The[compiler directive](<Compiler_directive.md> "Compiler directive") `{$include}` is used to: 

  * read and parse a file, virtually inserting its contents at the location the directive was encountered,
  * insert compiler information, or
  * insert the value of an environment variable.



The `{$I}` directive, case-_sensitive_ , is shorthand for the same directive. 

## Sourcing files

Further tokens can be read from a file using the `{$include}` directive to virtually substitute the file’s contents for the directive. A file to be substituted for the directive is specified like this `{$include magic.pas}`. 

Files are searched for in the following order: 

  1. at the path specified;
  2. in the directory containing the current source file;
  3. in all include file paths. (`-Fi` or `-I` compiler parameters.)



When specifying a file name, a file type hint suffix can be omitted. If you omit a suffix, the following suffixes are tried out: 

  1. _none_ , meaning the exact name as specified (_without_ any suffix)
  2. `.inc`
  3. `.pp`
  4. `.pas`
  5. (trailing dot stripped)



This strategy applies to every directory in the search path as described above, and the first match is taken. 

For Delphi compatibility, the special file name [`*`](<Asterisk.md> "Asterisk") will attempt to include a file of the current file’s base name. The suffix is supplemented as described above. You can, however, specify a (different) suffix too: `{$include *.dat}` will look for a file bearing same name as the source file, but the suffix (if applicable) will be replaced by (or augmented with) `.dat`. The suffix is the _last_ component of a file name separated by a [`.` (dot)](<period.md> "period") (thus `{$I *.foo.bar.dat}` is effectively the same as `{$I *.dat}`). Note, if you are reading a file via a symbolic link, the supplied _unresolved_ name is the source code file’s name, _not_ the target file name. 

If a file name contains blank characters, the file name needs to be surrounded by [`'` (typewriter quotes)](<'.md> "'"). If a file name starts with a percent sign or asterisk, you must either use straight quotes as in the case of blanks, or specify an absolute or relative path like `{$I ./%wage.pas}` (on UNIX-like platforms `./` denotes the “current directory”). 

Not finding a specified file constitutes a [fatal error](<compile-time_error.md> "compile-time error"). 

## Insertion of certain information

The compiler can be instructed to insert certain information at a specific spot. For that, a word is surrounded by [`%`](<Percent_sign.md> "Percent sign") like in `{$include %internalVariable%}`. 

This directive always inserts [strings](<String.md> "String") (except `{$include %lineNum%}`). A set of predefined variables are consulted first. If there is no predefined variable of a given name, the value of the environment variable of the given name is tried to be retrieved. If this process fails, an empty string is inserted in order to provide a consistent behavior. 

The set of predefined variables are (case-insensitive): 

  * location information 
    * `{$include %currentRoutine%}` expands to the current routine’s name. (available since FPC trunk revision 30873; FPC 3.2.0)
    * `{$include %file%}` expands to the file’s name the directive is found in.
    * `{$include %line%}` expands to the line number (starting at 1) where the directive is found at.
    * `{$include %lineNum%}` is the same as `{$I %line%}` but of an integer type.
  * [target](<Free_Pascal_supported_targets.md> "Free Pascal supported targets") information 
    * `{$include %FPCTargetCPU%}` expands to the target’s CPU name, e. g. `'x86_64'`.
    * `{$include %FPCTargetOS%}` expands to the target’s [OS](<Operating_System.md> "Operating System") name, e. g. [`'Linux'`](<Linux.md> "Linux").
  * compiler information 
    * `{$include %FPCVersion%}` expands to the version number of the used [FPC](<FPC.md> "FPC"), e. g. `'3.0.4'`.
  * regarding time/date of processing the directive 
    * `{$include %date%}` expands to a string of the form `'YYYY/mm/dd'`, e. g. `'2022/10/04'`.
    * `{$include %dateYear%}` expands to `'2022'`. (Available since FPC trunk revision 38329)
    * `{$include %dateMonth%}` expands to `'10'`. (Available since FPC trunk revision 38329)
    * `{$include %dateDay%}` expands to `'4'`. (Available since FPC trunk revision 38329)
    * `{$include %time%}` expands to a string of the form `'HH:MM:SS'`, e. g. `'15:43:42'`.
    * `{$include %timeHour%}` expands to `'15'`. (Available since FPC trunk revision 38329)
    * `{$include %timeMinute%}` expands to the minute part of `{$I %time%}`. (Available since FPC trunk revision 38329)
    * `{$include %timeSecond%}` expands to the seconds part of `{$I %time%}`. (Available since FPC trunk revision 38329)



Historical remark: Previously _some_ information like `{$include %FPCVersion%}` and `{$include %date%}` could be included with the long deprecated utility [`fpcsubst`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/3.3.1/utils/fpcsubst.pp>), a macro preprocessor program. 

## See also

  * [§ “include file”](<https://www.freepascal.org/docs-html/prog/progsu40.html>) and [§ “include compiler info”](<https://www.freepascal.org/docs-html/prog/progsu41.html>) in the _Free Pascal programmer’s guide_

---

_Source: [https://wiki.freepascal.org/$include](https://web.archive.org/web/20230326123653/https://wiki.freepascal.org/$include)_
