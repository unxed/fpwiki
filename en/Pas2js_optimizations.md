# Pas2js optimizations

## Contents

  * 1 Overview
  * 2 Generated JavaScript
    * 2.1 Existing optimizations
    * 2.2 Feature requests
  * 3 Compiler itself



# Overview

This page describes existing optimizations for the [pas2js](<pas2js.md> "pas2js") transpiler as well as feature requests. 

# Generated JavaScript

## Existing optimizations

  * -O-: Disable all optimizations
  * -O1 : Level 1 optimizations (quick and debugger friendly)
  * -O2 : Level 2 optimizations, slow and not debugger friendly
  * -OoEnumNumbers[-]: write enum value as number instead of name. Default in -O1.
  * -OoRemoveNotUsedPrivates[-]: Default is enabled
  * -OoRemoveNotUsedDeclarations[-]: Default enabled for programs with -Jc
  * -OoShortRefGlobals[-]: Insert JS local var for types from other units and modules. Default enabled in -O2
  * omit unneeded brackets in associative operations (a||b)||(c||d), (a&&b)&&(c&&d), (a|b)|c, (a&b)&c, (a^b)^c, (a+b)+c, (a-b)-c, (a*b)*c
  * Use {$modeswitch OmitRTTI} to treat published sections as public and thus remove them by WPO. Published section provide RTTI, so the compiler cannot know at compile time if a published identifier is needed, so it keeps it.



## Feature requests

There are some predefined sets of optimizations. At the moment there is only -O1, but eventually there will be _-O2, -O3, -O4_ similar to FPC. Each optimization can be enabled/disabled separately via some corresponding -Oo flag. 

  * -O1 : Level 1 optimizations, fast and debugger friendly
  * -O2 : Level 2 optimizations, slow and not debugger friendly
  * -O3 : Level 3 optimizations, slow and might break special code
  * -O4 : Level 4 optimizations, very slow and/or only for rare cases



Specific optimizations: 

  * move rtl.js functions to system.pp and let the WPO decide if they are needed
  * add $mod only if needed
  * add Self only if needed
  * use a number for small sets instead of JS objects
  * shortcut for test if a set is empty a=[] a<>[]
  * SetLength(scope.a,l) -> read scope only once, same for Include, Exclude, Inc, Dec, +=, -=, *=, /=
  * ECMAScript6: use _const_ for local const
  * add an option to let WPO omit all published members not used at compile time
  * procedure message modifier: only use message methods which message parameter type is used.
  * omit implicit "return Result" if last statement is return. issue 35860
  * WPO: omit empty overrides
  * -O1: shortcut: a in [b] -> a=b
  * -O1: shortcut: [a]*b=[] -> not (a in b)
  * -O1: shortcut: [a]*b<>[] -> a in b
  * -O1: combine multiple var a=0,b=0
  * -O1: init a local var with the first assignment
  * -O1: skip clone array for new array and arraysetlength
  * -O1/-O2: autoinline
  * -O1: replace constant expression with result, e.g. b:=1+2 -> b:=3
  * -O1: create constants for set literals in expressions [lit,lit]
  * -O1: only in WPO: omit constant definition if all references were replaced by literal
  * -O1: no function Result var when assigned only once, e.g. Result = 3; return Result; -> return 3;
  * -O1: by reference: create getter/setter object only once
  * -O1: pass array element by reference: when index is constant, use that directly (i.e. no temporary variable)
  * -O1: var/out argument: create accessor as var outside loops and combine multiple to one
  * -O1: $mod.$rtti["name"] -> $mod.$rtti.name
  * -O2: replace constant with literal only if reference is longer, e.g. use pas.mymath.tau instead of 6.2831853071795862.
  * -O2: compressed format: omit spaces and line breaks in JavaScript
  * -O2: inline -Si
  * -O2: case-of with 6+ elements as binary tree
  * -O2: case-of using JS operators Case1?Statement1:Case2?:Statement2:Else
  * -O2: insert local/unit vars for types in same unit
  * -O2: insert local/unit vars for static functions
  * -O2: {$optimization removeemptyprocs} as FPC
  * -O2: skip dead code If(false){...}
  * -O2: simplify: _"abc" + ("" + expr)_ to _("abc" + expr)_
  * -O2: simplify: _a = (b)_ with b not a function declaration to _a = b_
  * -O2: combine _var a = v; var b = t;_ to _var a=v, b=t;_
  * -O2: replace multiple reads (writes) of a variable with a getter (setter).
  * -O2: CSE
  * -O3: DFA
  * -O3: shorten local identifiers: rename identifiers, often used identifiers should get the shortest names, might break asm-blocks.
  * -O3: shorten global identifiers: requires WPO, might break asm-blocks and foreign JS accessing the Pascal generated code.



# Compiler itself

The compiler was deliberately with the following design policies: 

  * Modular fashion. Each part has its own test suite and is used by other projects.
  * Can be used in a library, i.e. no restart or recreate required to compile another program. Simply call Reset. It reuses files from cache if fileage has not changed.
  * Can be used without a filesystem, i.e. in a browser
  * Threadsafe, i.e. the compilation can run in a thread. Compiling a program with multiple threads is planned.
  * Easy maintenance has higher priority than high speed and low memory usage

---

_Source: [https://wiki.freepascal.org/Pas2js_optimizations](https://web.archive.org/web/20231205212519/https://wiki.freepascal.org/Pas2js_optimizations)_
