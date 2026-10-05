# pas2js AsyncAWait

## Contents

  * 1 Overview
  * 2 Basics
  * 3 AWait
    * 3.1 Example Async try fetch
    * 3.2 Example Manually created promise
      * 3.2.1 Manually created promise Version 1
      * 3.2.2 Manually created promise Version 2
    * 3.3 Example Multi await
  * 4 Navigation



# Overview

[Pas2js](<pas2js.md> "pas2js") supports the JS operators async/await to simplify the use of Promise, which itself simplifies writing asynchronous code, like starting a download, waiting for the result, then depending on the result start the next and so forth. Instead of writing a tedious and hard to read mass of sub functions an async procedure can be written in the common serial aka imperative way. 

# Basics

First of we have the **async** keyword, which you add as a _procedure type modifier_ to turn it into an _async function_ (or _async procedure_). An _async function_ knows how to expect the possibility of the _await_ function being used to invoke **asynchronous** code. 

Usually you call an **async** function via the **await** function. To explain the inner workings it is useful to see what happens when you call an _async_ function directly. Then you get a **TJSPromise** : 
    
    
    uses JS;
    
    function Run: word; async;
    begin
      Result:=3;
    end;
    
    var p: TJSPromise;
    begin
      p:=Run(); // calling directly without await() returns a TJSPromise!
    end.
    

That's because an _async_ function runs till an _await_ and returns. If there is no _await_ it works like a normal function, returning immediately a resolved TJSPromise. If it contains one or more _await_ calls, each _await_ pauses the execution of the async function and continues it asynchronously (single threaded) later. A _TJSPromise_ has a hook for success and one for fail, which usually requires writing some anonymous functions. With async/await you can write much more readable code. 

# AWait

Pas2js provides a built-in **await** function, which supports three flavours: 
    
    
    function await(AsyncFunctionOfResultTypeT): T;
    function await(aType; p: TJSPromise): aType; // explicit promise requires the resolved type
    function await(aType; v: JSValue): aType; // explicit optional promise requires the resolved type
    

The **await** function can only be used inside a procedure with the **async** modifier. 

An **async** procedure can contain multiple **await** calls. 

Giving a promise to _await_ works as if the following code is inside a _TJSPromise._then()_. If the promise is already resolved, _await_ returns the value immediately. Otherwise the promise is returned as the _async_ function result and the rest of the _async_ function is executed when the promise is resolved. 

You can enclose an **await** function in _try..finally_ or _try..except_ for the case the _Promise_ raises an exception. 

You can call _await_ on a jsvalue, any promise will be resolved. For example when a JS function returns either a number or a promise of the number. 

You can define your own external class 'Promise'. It does not need to be called _TJSPromise_. 

## Example Async try fetch

Example demonstrating waiting for an async function, catching errors with a try..except and no anonymous functions. 
    
    
    // Translated from https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await
    program tryfetch;
    
    {$mode objfpc}
    
    uses
      browserconsole, JS, Web, SysUtils;
    
    procedure myFetch; async;
    var
      response: TJSResponse;
      myBlob: TJSBlob;
      image: TJSHTMLImageElement;
      objectURL: string;
    begin
      try
        response := await(window.fetch('test1.png'));
    
        if not response.ok then
          raise Exception.Create('HTTP error! status: '+str(response.status))
        else begin
          myBlob := await(response.blob());
          objectURL := TJSURL.createObjectURL(myBlob);
          image := TJSHTMLImageElement(document.createElement('img'));
          image.src := objectURL;
          document.body.appendChild(image);
        end;
      except
        console.log(JSExceptValue);
      end;
    end;
    
    begin
      myFetch;
    end.
    

## Example Manually created promise

Example demonstrating using **await** with a non async function, creating the TJSPromise manually: 

### Manually created promise Version 1
    
    
    Program MyModule;
    
    uses JS, Web;
    
    function ResolveAfter2Seconds: TJSPromise;
    // returns immediately with a Promise,
    // which after 2 seconds gets resolved
    begin
      Result:=TJSPromise.new(procedure(resolve, reject : TJSPromiseResolver)
        begin
        window.setTimeout(procedure
          begin
          resolve('resolved');
          end,
          2000); // wait 2 seconds
        end);
    end;
    
    procedure AsyncCall; async;
    var s: string;
    begin
      writeln('calling');
      s := await(string,resolveAfter2Seconds());
      // the await pauses this procedure returning to the caller
      // when the Promise from resolveAfter2Seconds gets resolved 
      // this procedure is continued
      writeln(s); // expected output: 'resolved'
    end;
    
    begin
      AsyncCall;
      // calling AsyncCall returns immediately, while the Promise is waiting
      writeln('called');
    end.
    

Expected output: 
    
    
    calling
    called
    resolved
    

### Manually created promise Version 2

Alternatively you can use the promise result type (here: string), by passing the _TJSPromise_ to the **exit()** function: 
    
    
    Program MyModule;
    
    uses JS, Web;
    
    function ResolveAfter2Seconds: string; async;
    // returns immediately with a Promise,
    // which after 2 seconds gets resolved
    var p: TJSPromise;
    begin
      p:=TJSPromise.new(procedure(resolve, reject : TJSPromiseResolver)
        begin
        window.setTimeout(procedure
          begin
          resolve('resolved');
          end,
          2000); // wait 2 seconds
        end);
      exit(p); // the exit() function takes either a TJSPromise or a string
    end;
    
    procedure AsyncCall; async;
    var s: string;
    begin
      writeln('calling');
      s := await(resolveAfter2Seconds()); // no "string" needed
      // the await pauses this procedure returning to the caller
      // when the Promise from resolveAfter2Seconds gets resolved 
      // this procedure is continued
      writeln(s); // expected output: 'resolved'
    end;
    
    begin
      AsyncCall;
      // calling AsyncCall returns immediately, while the Promise is waiting
      writeln('called');
    end.
    

Note: The **Result** variable is of type string. You cannot assign it a promise. 

## Example Multi await

Example demonstrating starting multiple async functions and waiting for them while they execute in parallel. 
    
    
    // Translated from https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await
    program tryfetchmany;
    
    {$mode objfpc}
    
    uses
      browserconsole, JS, Web, SysUtils, Types;
    
    function FetchBlob(url: string): TJSBlob; async;
    var
      response: TJSResponse;
    begin
      response := await(window.fetch(url));
      if not response.ok then
        raise Exception.create('HTTP error! status: '+str(response.status))
      else
        Result:=await(response.blob());
    end;
    
    function FetchText(url: string): string; async;
    var
      response: TJSResponse;
    begin
      response := await(window.fetch(url));
      if not response.ok then
        raise Exception.create('HTTP error! status: '+str(response.status))
      else
        Result:=await(response.text());
    end;
    
    procedure DisplayContent; async;
    var
      coffee, tea, description: TJSPromise;
      objectURL1, objectURL2, descText: String;
      values: TJSValueDynArray;
      image1, image2: TJSHTMLImageElement;
      para: TJSHTMLElement;
    begin
      try
        coffee := FetchBlob('test1.png');
        tea := FetchBlob('test1.png');
        description := FetchText('description.txt');
    
        values := await(TJSValueDynArray,TJSPromise.all([coffee, tea, description]));
    
        objectURL1 := TJSURL.createObjectURL(values[0]);
        objectURL2 := TJSURL.createObjectURL(values[1]);
        descText := string(values[2]);
    
        image1 := TJSHTMLImageElement(document.createElement('img'));
        image2 := TJSHTMLImageElement(document.createElement('img'));
        image1.src := objectURL1;
        image2.src := objectURL2;
        document.body.appendChild(image1);
        document.body.appendChild(image2);
    
        para := TJSHTMLElement(document.createElement('p'));
        para.textContent := descText;
        document.body.appendChild(para);
    
      except
        writeln(JSExceptValue);
      end;
    end;
    
    begin
      DisplayContent;
    end.
    

# Navigation

  * [Pas2js](<pas2js.md> "pas2js")

---

_Source: [https://wiki.freepascal.org/pas2js_AsyncAWait](https://web.archive.org/web/20250513150351/https://wiki.freepascal.org/pas2js_AsyncAWait)_
