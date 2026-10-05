# WebAssembly/JS

The primary target for WebAssembly is the browser. With WebAssembly, you can create functions in Pascal and use these functions in web pages with short glue code in Javascript. 

WebAssembly is not Javascript, it is an addition to Javascript. 

WebAssembly is not asm.js. 

## Contents

  * 1 API
  * 2 Using Wasm Functions
  * 3 Dependencies
    * 3.1 Example
  * 4 See Also



## API

The primary API for dealing with WebAssembly (.wasm) files is the [WebAssembly JS](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly>) standard 

## Using Wasm Functions

In order to use a .wasm file in Javascript the following steps needs to be taken: 

  * The .wasm file needs to be loaded



    

  * as an external resource (a web server is required due to browser security)
  * a binary file loaded through a javascript (i.e. inlined).



  * The loaded .wasm binary file needs to be compiled.
  * The compiled file needs to be linked (aka instantiated). It is also common to do the compilation and instantiation in one step, rather than two separate steps. (Two separate steps are typically needed, when the "compiled" form is used, either for caching OR for the dependencies resolution stage)



    

  * when creating an instance (linking) all dependencies must be resolved.



  * after creating an instance, the exported functions of the instance can be called through Javascript as normal JS functions.



Here is a common JS example: 
    
    
    // step #1 - loading the file and converting it to array of bytes, using the fetch() function
    fetch('../out/main.wasm').then(
      response => response.arrayBuffer()
    ).then(
      // step #2 and #3 - compiling and linking the array of bytes (in one call) to WebAssembly.instantiate()
      bytes => WebAssembly.instantiate(bytes)).then(
    
        // step #4 using the linked instance object to call the exported function add()
        results => {
          instance = results.instance;
          document.getElementById("container").textContent = instance.exports.add(1,1);
        }
      ).catch(console.error);
    

## Dependencies

As of 2019 there is no "out-of-the-box" way to resolve WebAssembly file **import** dependencies. 

WebAssembly expects an import to be resolved by providing the import module name. The import module name does not have to be a WebAssembly module, it can be a plain JavaScript function as well. And the name of the object (i.e. function of memory) WebAssembly is trying to access. 

### Example

Here is an example of dependencies loading. 

**WARNING:** you should NOT use this code in production. It is only used for educational purposes! Javascript and its APIs are designed to do all the tasks asynchronously. 

Most of the code is based on the callbacks (closures), that are triggered when the particular task is done. I.e. loading a resources from the web, would trigger an event once the loading is completed (or failed) - as shown in the example above. 

The example below is **intentionally** performs each step synchronously (by using "await" syntax is used to explicitly wait for a completion of synchronous actions). Such intentional synchronization would impose slowness in a real-life application and should not be used. 

The example consists of two WebAssembly files and a single Javascript file to load them. 

    **main.wasm** :

The module imports function named "add" from an external module named "test2" 
    
    
    (module 
      (import "test2" "add" (func $add (param i32)(param i32)(result i32) ))
    
      ;; the main() function calls the external add() with arguments 4 and 5 
      ;; and forwards the result of add as its own result
      (func $main
        (result i32)
        i32.const	4
        i32.const	5
        call	$add
        return
      )
      ;; module exports the main function so it can be called outside (in javascript)
      (export "main" (func $main))
    )
    

    **test2.wasm** :

The module implements the function "add" and exports it. 
    
    
    (module
      (func $add (param $lhs i32) (param $rhs i32) (result i32)
        get_local $lhs
        get_local $rhs
        i32.add)
      (export "add" (func $add))
    )
    

    **javascript**

The code below loads the "main.wasm". 

Once loaded, it compiles the module. The compiled module is investigated, if it needs any dependencies. 

For each dependency found, the code tries to load the next module (assuming it is a .wasm unit). 

The dependency module is loaded, compiled and linked. After it is linked, its exported functions are populated into the Import object. 

The Import object is used to satisfy the "main.wasm" unit dependencies. 

Once all imported entities are processed, the main module is finally linked and the main function is executed. 
    
    
    // only "async" functions are allowed to perform actions "synchronously" via "await" syntax. 
    // Just because "async" functions themselves are performed "on the thread"
    //
    // Typically, calling fetch() function returns right away, with the Promise object.
    // The Promise object should be studied, in order to identify if the job has been finished or not 
    // or an event needs to be assigned that would be called once the job is completed.
    //
    // using "await fetch()" syntax is forcing the fetch() operation to perform synchronously
    // and the next line of code will be executed only after the operation has completed.
    async function runWasm()
    {
       // Reading the main.wasm file
       var r = await fetch('../out/main.wasm'); 
       var bytes = await r.arrayBuffer(); // converting the file to array of bytes
    
       // compilation is also a time-consuming process. Thus, "await" is needed.
       // the result of compile is WebAssembly.Module type
       var mainmod = await WebAssembly.compile(bytes); 
    
       // console.log(mainmod); // print the main module
       
       var impobj = {}; // the Import object is supposed to resolve the main unit dependencies 
                        // originally we set it to an empty object
    
       // asking the compiled module for the list of imported objects
       var imports = WebAssembly.Module.imports(mainmod); 
       // console.log(imports); // print the list of dependencies
    
       // keeping track of loaded dependencies instances
       // just to avoid loading the same module twice
       var loadedInst = []; 
    
       // loading dependencies should be recursive!
       // for simplicity, we assume that dependencies 
       // do not have dependencies themselves. 
       for (var i = 0; i < imports.length; i++)
       {
          var dep  = imports[i]; // dep contains the fields "module","name" and "kind"
    
          var subinst = loadedInst[dep.module];
          if (!subinst) 
          {
            // the module has not been loaded yet, let's load it!
            var rr = await fetch('../out/'+dep.module+'.wasm');
            var buf = await rr.arrayBuffer();
            // try to link the module right away
            var submod = await WebAssembly.instantiate(buf);
            subinst = submod.instance;
            // saving the instance for later use, if it is used more than once
            loadedInst[dep.module] = subinst;
          }
          //console.log(subinst); // print the sub module instance
    
          // adding the module to the list of Import objects (if not there yet)
          var impmod = impobj[dep.module];
          if (!impmod) {
            impmod = {};
            impobj[dep.module] = impmod;
          } 
          // extracting the function exported function
          impmod[dep.name] = await subinst.exports[dep.name];
          impobj[dep.module] = impmod;
       }
       // console.log(impobj); // print the imported object
    
       // impobj - should now contain all the necessary functions of the main module
       // in this example, it should only have 
       //   imbobj.test2.add
       var maininst = await WebAssembly.instantiate(mainmod, impobj);
       // console.log(maininst); // print the main instance object
    
       // call the main() function of the main unit
       document.getElementById("container").textContent = maininst.exports.main();
    }
    runWasm();
    

## See Also

  * [WebAssembly](<../WebAssembly.md> "WebAssembly")
  * [WebAssembly JS](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly>) at MDN

---

_Source: [https://wiki.freepascal.org/WebAssembly/JS](https://web.archive.org/web/20250316083937/https://wiki.freepascal.org/WebAssembly/JS)_
