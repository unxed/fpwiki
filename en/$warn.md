# $warn

`{$warn}` is a [local compiler directive](<local_compiler_directives.md> "local compiler directives") controlling emission of certain messages during compile-time. Messages do not necessarily have to be warnings, but can be messages of any kind. The directive may also promote specific messages to errors. 

The (local) compiler directives `{$warnings on}` and `{$warnings off}` may turn on or off emission of all _warnings_ all together. 

## usage

`{$warn}` requires a message label and a treatment option as arguments, like this: 
    
    
    {$warn 3250 off}
    

The numeric message label can be retrieved during compilation with the `-vq` option supplied on the command-line (verbosity: show message numbers). Some messages and groups of messages have an alias, too, which can be seen in the table below. 

Beside `{$warn 3019 on}` or `{$warn 3019 off}` and the shorthand notation `{$warn 3019 +}` and `{$warn 3019 -}` respectively, `{$warn}` may be used to promote messages to errors. `{$warn symbol_experimental error}` will make all, if any, [hints concerning `experimental` symbols](<Hint_Directives.md> "Hint Directives") become (non-fatal) errors. Depending on the selected error treatment, the compiler may stop compiling or continue, unless a threshold of errors has been reached. 

Note, demoting messages to errors is not possible. A `{$fatal}` error will always be fatal. 

## message labels

[FPC](<FPC.md> "FPC") messages (excerpt)  index | alias | situations triggering the message   
---|---|---  
`02024` |  | 

  * there is a custom defined `{$warning}`

  
`02077` |  | 

  * the directive `{$minStackSize}` is used, but not supported on the current OS

  
`02078` |  | 

  * the directive `{$maxStackSize}` is used, but not supported on the current OS

  
`03018` |  | 

  * a `constructor` is not defined at `public` visibility level

  
`03019` |  | 

  * a `destructor` is not defined at `public` visibility level

  
`03177` |  | 

  * a `record` initialization does not define values for all fields

  
`03211` | `implicit_variants` | 

  * the `variants` unit is required, but has not yet been loaded

  
`03250` |  | 

  * a virtual method is overridden with lower visibility

  
`04044` |  | 

  * a comparison might be always `false` (disjunct ranges)

  
`04045` |  | 

  * a comparison might be always `true` (disjunct ranges)

  
`04046` | `constructing_abstract` | 

  * constructing an instance of a class with abstract methods

  
`04056` |  | 

  * a pointer to ordinal conversion is not portable

  
`04090` | `zero_nil_compat` | 

  * converting `0` to [`nil`](<Nil.md> "Nil") (integer to pointer)

  
`04104` | `implicit_string_cast` | 

  * implicit string type conversion

  
`04105` | `implicit_string_cast_loss` | 

  * implicit string type conversion with potential data loss

  
`04106` | `explicit_string_cast` | 

  * explicit string type conversion

  
`04107` | `explicit_string_cast_loss` | 

  * explicit string type conversion with potential data loss

  
`04108` | `cvt_narrowing_string_lost` | 

  * Unicode constant cast with potential data loss

  
`04116` | `intf_raise_visibility` | 

  * the interface defines a broader visibility for a method

  
`04122` | `constructing_abstract` | 

  * constructing an instance of an abstract class

  
`05023` |  | 

  * a `unit` is imported, but not used (except units containing `initialization`/`finalization` code)

  
`05024` |  | 

  * a routine parameter remains unused

  
`05033` | `no_retval` | 

  * no (for the compiler obvious) assignment to a function’s return value

  
`05036` |  | 

  * a variable which might not yet have been initialized is used

  
`05043` | `symbol_deprecated` | 

  * a symbol with the `deprecated` hint (no deprecation message defined) is used

  
`05044` | `symbol_platform` | 

  * a symbol with the `platform` hint is used

  
`05055` | `symbol_unimplemented` | 

  * a symbol with the `unimplemented` hint is used

  
`05059` |  | 

  * inside a function definition its result variable’s value is used, before it has been initialized

  
`05063` | `symbol_experimental` | 

  * a symbol with the `experimental` hint is used

  
`05065` | `symbol_library` | 

  * a library symbol is used

  
`05066` | `symbol_deprecated` | 

  * a symbol with the `deprecated` hint is used and a custom deprecation notice has been defined

  
`05073` |  | 

  * a `private property` remains unused

  
`05074` | `unit_deprecated` | 

  * a unit with the `deprecated` hint (without a custom deprecation message supplied) is used

  
`05075` | `unit_deprecated` | 

  * a unit with the `deprecated` hint is used and a custom deprecation notice has been defined

  
`05076` | `unit_platform` | 

  * a unit with the `platform` hint is used

  
`05077` | `unit_library` | 

  * [to be explained]

  
`05078` | `unit_unimplemented` | 

  * a unit with the `unimplemented` hint is used

  
`05079` | `unit_experimental` | 

  * a unit with the `experimental` hint is used

  
`06018` |  | 

  * the compiler has determined there is, or might be, unreachable code

  
`07104` |  | 

  * register indirect addressing with `ebp` is used in conjunction with a negative offset

  
  
Note, some aliases identify _multiple_ messages. 

## see also

  * [§ “`$WARN`: Control emission of warnings” in the _Free Pascal programmer’s guide_](<https://www.freepascal.org/docs-html/prog/progsu80.html>)
  * [`{$message}`](<$message.md> "$message")
  * [Turn warnings and hints on or off](<Turn_warnings_and_hints_on_or_off.md> "Turn warnings and hints on or off")

---

_Source: [https://wiki.freepascal.org/$warn](https://web.archive.org/web/20250324233240/https://wiki.freepascal.org/$warn)_
