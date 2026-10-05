# WebAssembly/Internals

## Contents

  * 1 Outdated page
  * 2 Assemblers
    * 2.1 wat2wasm (Wabt)
    * 2.2 wasm-as (emscripten)
    * 2.3 llvm-mc
  * 3 Use on the Wiki
  * 4 Limitations
  * 5 Validation
  * 6 Global Symbols
  * 7 Code Branching/Flow Control
    * 7.1 IF-block
    * 7.2 Loop-block
    * 7.3 FPC Labels
  * 8 External
    * 8.1 PChar Strings
  * 9 Object File Contents
  * 10 Static Linking
    * 10.1 Linking section
    * 10.2 Linking Symbols
    * 10.3 Tools
  * 11 See Also



## Outdated page

This page is outdated. It is currently used as a content staging area during refactoring of the various pages. 

## Assemblers

There are different assemblers available, from Wabt, emscripten.org and LLVM. The expected format is a slightly different between those two: 

### wat2wasm (Wabt)

Example: 
    
    
     (module
       (func $add (param $lhs i32) (param $rhs i32) (result i32)
         local.get $lhs
         local.get $rhs
         i32.add
       )
       (export "add" (func $add))
     )
    

According to the official site "Wabt" is using it's own format of the Wasm. It's slightly different from the official documentation. The most current version of Wabt matches the specs, as well as supports the old syntax. 

Online studio, that's using the older version of wabt syntax. <https://webassembly.studio/>

For example. instead of 
    
    
    local.get
    

it's using 
    
    
    get_local
    

### wasm-as (emscripten)

The assembler is recommended for the use in a compiler by the WebAssembly.org 
    
    
     (module
       (func $add (param $lhs i32) (param $rhs i32) (result i32)
         (
         local.get $lhs
         local.get $rhs
         i32.add
         )
       )
       (export "add" (func $add))
     )
    

### llvm-mc

This is the assembler from the LLVM project. It uses a GAS-like syntax. 

## Use on the Wiki

WebAssembly is using s-expressions as its textual format (for either Wabt or Emscript) . it's handy to use syntax highlighter for the code and use "lisp" language to set colors. 
    
    
      <source lang="lisp">
        ;; web assembly goes here
      </source>
    

## Limitations

  * WebAssembly doesn't have a stand-alone executable format. It's always a "plugin" to some environment. Thus the compiler currently only supports producing a **library** and program should not be used. Using **library** requires exporting a certain functions from the library. Those functions will be the "export" part of Wasm module as well.
  * WebAssembly format doesn't provide any special information for static linking (for the purpose of combining multiple .wasm files into one). No support is provided at this time, thus the entire Pascal code should be put in a single library file.
  * Importing functions into Pascal is supported partially; see External.
  * A memory object must always be provided externally, using the module **env** with the name **memory**. This is hard-coded in the Wasm assembler. (Todo: needs to be configurable.)



## Validation

There's a certain requirements to the binary code of WebAssembly. 

The validation is described at the specification <http://webassembly.github.io/spec/core/valid/index.html>

A code that satisfies the validation rules, is considered to be well-formed 

  
It's possible to generate a binary file with the code that doesn't satisfy the validation steps. However, the browsers would not instantiate such binary file. Neither any other host should. 

In some cases, there might be an overhead to create well-formed code. 

## Global Symbols

Global symbols are stored in **global** variables. Each variable is of i32 type and stores an offset in memory, where the object is usually stored. 

In order to get the address of the variable, one should invoke get_global for the desired symbols. The memory offset if predefined and cannot be changed during the runtime. The contents of the memory can be changed. 

## Code Branching/Flow Control

_todo: rewrite to the better explanation_

WebAssembly doesn't allow direct jumps to (address/label)/or jump by offset. It only allows to jump "out-of" a code block, where the code block would be identified by a label. I.e. beginning of the loop label (for **continue**) or end of the loop label (for **break**). Loop itself is a single code block, known as "loop" in WebAssembly ). 

FPC basic implementation is based of the ability of conditional jump instructions to a specified label. For that reason, the default implementation of TCGIfNode doesn't apply. Also, not every conditional jump instructions can have a label assign to it. Only instructions that jump out of the block are should use labels. Other, simply rely on the fact, that the next instruction IS the point they needs to be. 

### IF-block

Wasm IF do return a value (one of the basic WebAsm types). Pascal IF-statement does not return a value. 

Thus the feature of value returning is not used (**todo:** but should be). 

A dummy 0 value is pushed on the stack. IF is hard-coded to return i32 at all times. 

  
According to the official documentation, IF always comes with "ELSE" block. However, some earlier versions do allow IF without an else statement. (which is also allowed in Binary specification of WASM). Such IF comes without any resulting type, and the execution of IF should leave stack unmodified in the end. 

At this time "ELSE" branch is always generated for any IF statement. However, if there's no actual ELSE code (in pascal), a single "nop" instruction and a dummy result value produced. 
    
    
    function cmp(a,b: integer): Integer; 
    begin
      if a > b then 
        cmp := 1
      else 
        cmp := 0;
    end;
    

turns into: 
    
    
    (func $cmp
    	(param	i32)
    	(param	i32)
    	(result	i32)
    	(local	i32)	;; Temp 2,1 allocated
    	(local	i32)	;; Temp 3,1 allocated
    ;; [3] begin
    ;; [4] if a > b then
    	get_local	0
    	set_local	3
    	get_local	3
    	get_local	1
    	i32.gt_s
    	if (result i32)  ;; hard-coded i32 type of IF result
    ;; [5] cmp := 1
    	i32.const	1
    	set_local	2
    	i32.const	0 ;; mandatory wasm-IF result 
    	else
    ;; [7] cmp := 0;
    	i32.const	0
    	set_local	2
    	i32.const	0  ;; mandatory wasm-IF result
    	end
    	drop            ;; dropping IF-result from the stack, as unused
    ;; [8] end;
    	get_local	2
    	return
    )
    

The code generation of **cifnode** is overridden. 

### Loop-block

A loop is a block of instructions that has the label set to the beginning of the code. 

Here is an example of a loop. The function tries to sum up a number X amount of times 
    
    
    function mulbyadd(Num, Cnt: integer): integer;
      Result := 0;
      while Cnt>0 do begin
        dec(Cnt);
        Result := Result + Num;
      end;
    end;
    
    
    
      (func $mulbyadd (param $Num i32) (param $Cnt i32) (result i32)
        (local i32)       ;; local variable (that gets index #2) this is $Result
        i32.const 0
        set_local 2
    
        block
        loop 
          get_local $Cnt  ;; comparing Cnt to 0
          i32.const 0     ;; 
          i32.le_s         
          br_if 1         ;; if Cnt is less or equal to 0, then we should exit the loop
    
          get_local $Cnt  ;; decreasing Cnt
          i32.const 1 
          i32.sub  
          set_local $Cnt  
    
          get_local $Num  ;;
          get_local 2     ;; 
          i32.add         ;;
          set_local 2     ;; Result := Result + Num
    
          br 0            ;; loop back to the beginning 
        end 
        end
        get_local 2       ;; pushing the result to the stack (for the return value)
        )
    

### FPC Labels

Since branching is **relative** in Wasm (the absolute goto-like is only a proposed feature for Wasm), the use of TAsmLabel is different than other targets. The field **labelnr** is changed to indicate the relative jump. Typically 0 - to return to the loop or 1 - to jump out of the loop - or N to exit the function. 

## External

One can declare an external function. For a WebAssembly module the function must be provided during a module instantiation. Any external function must have a library name specified (Static linking is not supported!). The library name and the "exteral" name will be used during instantiation phase. 

Only basic types can be used as parameters (integer, floats, memory pointers) and function result. 

Here's an example: 

The code tries to use an external function that accepts only 1 integer parameter. (The function should write out the parameter) 
    
    
    library testext;
    
    procedure logint(i: integer); external 'env' name 'logint'; // name of external module and function name
    
    procedure run;
    begin
      logint(304);
    end;
    
    exports
      run name 'run';
    end.
    

  
In the browser, during instantiation the function must be provided: 
    
    
    const importObject  = {
      // note "env" should match the name of the function's library used in the pascal 
      // thus it's possible to import functions from different sources.
      "env":  {
    
        "memory": new WebAssembly.Memory({initial:32, maximum:32})
        // this is the implementation of "logint" function
        // it accepts exactly 1 parameter, and puts it into a the browser's console
       ,"logint": function(k) { console.log(k) }
      }
        
    }
    fetch('../out/main.wasm').then(response =>
      response.arrayBuffer()
    ).then(bytes => WebAssembly.instantiate(bytes, importObject)).then(results => {
      instance = results.instance;
     
      document.getElementById("container").textContent = 
        // running the export run() function
        instance.exports.run();
    }).catch(console.error);
    

### PChar Strings

Strings would require a more work from the client side. There's no "string" type in Wasm, thus strings are treated as chunks in memory. From Javascript perspective that means, that a reference to a memory is received from a Wasm code and should be converted to JS string (that typically involves a text decoder) 
    
    
    library testext;
    
    type
      pchar = ^char;
    
    procedure logstr(p: pchar); external 'env' name 'logstr';
    procedure writedom(p: pchar); external 'dom' name 'writedom';
    
    procedure run;
    begin
      logstr('hello');
      logstr('world');
      writedom('<b>Hello World</b> and <input type="button" value="FPC">');
    end;
    
    exports
      run name 'run';
    
    end.
    

On the host side the function needs to read the memory and prepare the string for the output: 
    
    
    const memory = new WebAssembly.Memory({initial:32, maximum:32});
    
    // the function takes, strofs that indicates the start of string
    // and then tries to get the string out of the memory, by searching for null-terminating character
    function getStr(strofs)
    {
      var arr = new Uint8Array(memory.buffer,strofs);
      for (var i=0; i < arr.length; i++) 
        if (arr[i]==0) // null terminating char is found
          arr = arr.subarray(0, i);
      var str = new TextDecoder("utf-8").decode(arr);
      return str;
    }
    
    
    const importObject  = {
      "env":  {
        "memory": memory
       ,"logstr": function(k) 
         { console.log(getStr(k));  }
      },
      "dom": {
       "writedom": function(k) {  document.write(getStr(k)) }
      }
    }
    fetch('../out/main.wasm').then(response =>
      response.arrayBuffer()
    ).then(bytes => WebAssembly.instantiate(bytes, importObject)).then(results => {
      instance = results.instance;
      document.getElementById("container").textContent = instance.exports.run();
    }).catch(console.error);
    

Obviously pascal Ansi or WideStrings can work in the similar manner, and can be more efficient (as the size of the string is known ahead of time). However, strings can only be passed as constant strings, rather than a string variable for change (as implementing a reference count between Wasm and JS can be a bit more complicated) 

## Object File Contents

There's **wasm-objdump** that's useful for inspection of a generated object file. 

## Static Linking

See LLD linker: <https://lld.llvm.org/WebAssembly.html> <https://github.com/WebAssembly/tool-conventions/blob/master/Linking.md>

So far, there's only 1 wasm linker exists: lld (aka wasm-ld). LLVM linked ported for WASM. 

There's no official specification on object linking in WebAssembly. Tools are typically a common convention with a special "linking" section. 

One should keep in mind, that "linking" section is binary format specific. There are no way to explicitly define it via textual format. (which imposes some risk, when the implementation of a particular assembler changes) 

### Linking section

For each linkable part of the code (i.e. function) an entry must be created in "linking" section. 

### Linking Symbols

In order for **wasm-ld** to successfully connect two symbols, the following approach must be taken: 

  * a symbol declared in **implementation** section


    
    
    WASM_SYM_VISIBILITY_HIDDEN 
    WASM_SYM_BINDING_LOCAL 
    

This indicates that the symbol should not be linked anywhere, except for within the object file itself. 

  


  * the unit declaring a symbol in **interface** section (linkable by other units) should declare the unit with flags set to:


    
    
    WASM_SYM_VISIBILITY_HIDDEN 
    

This indicates that while the symbol can be linked to other object files, but should not be part of the linked library (executable) 

  


  * a same symbol that's being used by a different unit, should have a stub function (with **unreachable** in its body) and have the following flags set:


    
    
    WASM_SYM_BINDING_WEAK
    WASM_SYM_VISIBILITY_HIDDEN
    

This indicates to **wasm-ld** linker to get rid of the **unreachable** stub and use the actual implementation. 

  * a symbols that's explicitly declared in **exports** section of a library should not have a specific flags. However, the following flag can be also be used:


    
    
    WASM_SYM_EXPORTED
    

  * an external symbol (that's imported from the host) should be marked by


    
    
    WASM_SYM_UNDEFINED 
    

The flag is automatically set by **wat2wasm** for every **import** function. 

### Tools

**wat2wasm** \- specifics: 

  * a function must be either called in the unit OR it should be declared as a part of a (function pointer)**table** (via element) in order to be added to "linking" section;
  * in order to mark a function as **undefined** it should be either "exported" or "imported" function. Note "importing" a function requires declaring a unit name, which is divided by a "dot" (.) character from the function import name.
  * functions declared as "export" functions, would NOT be put into **linking** section and will be removed by the **wasm-ld** linker



**wasm-ld** (LLD 9.0.0) - specifics: 

  * when linking two or more modules for the function with the same name, linker keeps the code that's marked as **defined** (a special flag in linking section). The code that's marked as **weak** or **undefined** would be discarded.
  * wasm exported functions are ignored by default, if they don't have a symbol assigned in linking section.
  * the linker would use a symbol name for an exported function, instead of declared export name



**wasmtool** <https://github.com/skalogryz/wasmbin>

  * the utility allows to change export names



## See Also

  * [WebAssembly](<../WebAssembly.md> "WebAssembly")
  * [WebAssembly/Instructions](<Instructions.md> "WebAssembly/Instructions") \- Instructions of the WASM code
  * <https://webassembly.org/> \- the official site
  * <https://webassembly.github.io/spec/core/text/index.html> \- text format (S-Expression) specs
  * <https://webassembly.github.io/spec/core/binary/index.html> \- binary format specs
  * <https://developer.mozilla.org/en-US/docs/WebAssembly/Understanding_the_text_format>
  * <https://rsms.me/wasm-intro> \- introduction to webassembly
  * <https://blog.scottlogic.com/2018/04/26/webassembly-by-hand.html>

---

_Source: [https://wiki.freepascal.org/WebAssembly/Internals](https://web.archive.org/web/20220522082434/https://wiki.freepascal.org/WebAssembly/Internals)_
