# WebAssembly/JS-Promise Integration

The WebAssembly JavaScript-Promise Integration Proposal is a proposed extension to the browser JavaScript API: [[1]](<https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md>) It allows calling asynchronous JavaScript functions from WebAssembly code as if they are synchronous, as well as calling WebAssembly functions asynchronously from JavaScript, even though they are implemented as synchronous. 

## Contents

  * 1 Browser support
  * 2 Free Pascal support
  * 3 Free Pascal language extensions
    * 3.1 Suspending externals
    * 3.2 Promising exports
  * 4 See Also



# Browser support

There is currently experimental support implemented in Chrome, Chromium and Edge. So far, browser support is only implemented on the x86_64 and aarch64 platforms. Minimum Chrome versions that support it are 110.0.5473.0 (macOS) / 110.0.5469.0 (Windows, Android) / 110.0.5478.4 (Linux). As an experimental feature, it is disabled by default. To enable it, go to the URL: 
    
    
    chrome://flags
    

Search for "Experimental WebAssembly JavaScript Promise Integration (JSPI)". Click on the box, select "enable", then restart the browser to enable the feature. 

# Free Pascal support

Free Pascal support for this extension was merged into the main FPC branch on Jun 26 2023. 

# Free Pascal language extensions

## Suspending externals

Suspending externals allows calling JavaScript functions that are run asynchronously on the JavaScript side, even though they are called as if they are synchronous on the WebAssembly side. Example: 
    
    
    function compute_delta: double; external 'js' suspending;
    

Or: 
    
    
    function compute_delta: double; external 'js' suspending first;
    

Now we can implement this function asynchronously on the JavaScript side: 
    
    
    var compute_delta = () => 
      fetch('<https://example.com/data.txt'>)
        .then(res => res.text())
        .then(txt => parseFloat(txt));
    

We then prepare the asynchronous function for use as a suspending WebAssembly external, like this: 
    
    
    var suspending_compute_delta = new WebAssembly.Function(
      {parameters:[externref],results:['f64']},
      compute_delta,
      {suspending:"first"}
    )
    

We can the use this to form the complete import object: 
    
    
    var importObj = {js: {
        compute_delta:suspending_compute_delta}};
    

Note the extra _externref_ parameter, as well as the extra attribute _{suspending:"first"}_. The extra _externref_ parameter is automatically added, when a function is declared 'suspending'. It contains a special suspender object that is used internally by the WebAssembly JavaScript Promise Integration extension. For more details, see the [spec](<https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md#specification>). Free Pascal automatically adds the suspender as an extra first parameter for functions that are declared external suspending. There's also an option to add it as a last parameter. In this case, the declaration in Free Pascal needs to be changed to: 
    
    
    function compute_delta: double; external 'js' suspending last;
    

And on the JavaScript side, the value of the _suspending_ attribute needs to be changed to: _{suspending:"last"}_. 

## Promising exports

In order for calls to suspending functions to work, WebAssembly code execution must be entered via an export, marked as 'promising'. Example: 
    
    
    var
      state: double;
    
    function update_state: double;
    begin
      state := state + compute_delta;  // this calls the suspending function 'compute_delta'
      update_state := state;
    end;
    
    exports
      update_state promising;
    

Once again, a _promising_ export has an extra externref parameter automatically added. Here's how it can be wrapper correctly on the JavaScript side: 
    
    
    var sampleModule = WebAssembly.instantiate(demoBuffer,importObj);
    var update_state = new WebAssembly.function(
      {parameters:[], results:['externref']},
      sampleModule.exports.update_state_export,
      {promising : "first"})
    

Alternatively, you can use _promising last_. If you export it like this in Pascal: 
    
    
    exports
      update_state promising last;
    

Then you can use _{promising : "last"}_ instead of _{promising : "first"}_ in JavaScript. 

# See Also

  * [A working example](<https://www.freepascal.org/~michael/pas2js-demos/jspi/>)
  * [JavaScript-Promise Integration Proposal](<https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md>)
  * [Introducing the WebAssembly JavaScript Promise Integration API](<https://v8.dev/blog/jspi>)
  * [WebAssembly](<../WebAssembly.md> "WebAssembly")

---

_Source: [https://wiki.freepascal.org/WebAssembly/JS-Promise_Integration](https://web.archive.org/web/20250323014346/https://wiki.freepascal.org/WebAssembly/JS-Promise_Integration)_
