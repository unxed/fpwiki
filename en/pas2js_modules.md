# pas2js modules

## Contents

  * 1 Module import
  * 2 Demos
    * 2.1 Basic
    * 2.2 Basic-Units
    * 2.3 Flat
    * 2.4 Flat-Units
  * 3 Module export



## Module import

pas2js can convert the **$linklib** directive to a JS import statement: 

The following: 

` {$linklib ./modules/canvas.js canvas} `

is converted to 

` import * as canvas from "./modules/canvas.js"; `

If the alias is omitted, one is constructed for you: 

` {$linklib ./modules/some-api.js} `

is converted to ` import * as some_api from "./modules/some-api.js"; `

The import statements are always inserted in the main program. This is because modules are like windows libraries: self-contained programs, which only import and export well-defined symbols. 

For this reason, a new target "operating system" has been invented: the module. This means that you must compile with target module: 

` pas2js -Tmodule -Jirtl.js main.pp `

The nodejs target will also work, but in the future the 2 targets may diverge. 

## Demos

The pas2js demos/modules directory contains a series of subdirectories. Each subdirectory has 1 demo. Compile with the command-line as above: 

` pas2js -Tmodule -Jirtl.js main.pp `

### Basic

This shows a simple program, no units, that uses the linklib directive to import a javascript module. It uses an external class definition to access the exported symbols from the modules. 

### Basic-Units

This is the same as the Basic example, but the import definitions are split out in units. 

### Flat

This shows a simple program, no units, that uses the linklib directive to import a javascript module. It uses only external 'name' definitions to access the exported symbols from the modules; no external class is used. 

### Flat-Units

This is the same as the flat example, but the import definitions are split out in units. 

## Module export

Module export will be implemented using the existing pascal library concept, but this is currently a work in progress.

---

_Source: [https://wiki.freepascal.org/pas2js_modules](https://web.archive.org/web/20210921013040/https://wiki.freepascal.org/pas2js_modules)_
