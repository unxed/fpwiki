# pas2js exceptions

## Contents

  * 1 Exception handling
    * 1.1 Uncaught exceptions
    * 1.2 Exceptions in event handlers
    * 1.3 Hooking into exception handling
  * 2 Navigation



# Exception handling

## Uncaught exceptions

Pas2js translates exceptions to actual javascript exceptions. You can use the usual mechanisms _try..finally..end_ and _try..except..end_ to handle them. 

In case you do not handle exceptions, the rtl.js runtime has several mechanisms to deal with uncaught exceptions. 

In case you don't handle the exception, the default behaviour of the rtl is not to do anything. If you run the program in the browser, you'll see a message in the console, but that is it. To visualize the exception, you can add the following in your html file: 
    
    
    <script type="application/javascript">
      rtl.showUncaughtExceptions=true;
      rtl.run();
    </script>
    

the browser will then use a _window.alert()_ to show any uncaught exceptions. 

Note that this is handled by the small _rtl.js_ runtime, the pas2js system units do not handle this (but see below). 

## Exceptions in event handlers

When an exception is raised in an event handler, the rtl will not catch it. In fact, you will not see it. 

The way to handle this is to use the _safecall_ calling convention on event handler types. 

When you do this, the compiler will create a callback that does what a normal callback does, but inserts code to catch exceptions and route them to the rtl. 

The event handlers in the web.pas unit are declared using this calling convention (version 1.5.1 and above only): 
    
    
      TJSRawEventHandler = reference to Procedure(Event: TJSEvent); safecall;
    

This means that the event handler will catch an exception and route it to the rtl event handling mechanism. 

## Hooking into exception handling

As explained above, the default way to deal with exceptions is to show them using _window.alert()_. 

It is possible to override this behaviour using the sysutils unit. 
    
    
    Type
      TShowExceptionHandler = Procedure (Const Msg : String);
      TUncaughtPascalExceptionHandler = reference to Procedure(aObject : TObject);
      TUncaughtJSExceptionHandler = reference to Procedure(aObject : TJSObject);
    
      // Handler to show an exception (used when showexception is called)
      OnShowException : TShowExceptionHandler = nil;
    
    Function SetOnUnCaughtExceptionHandler(aValue : TUncaughtPascalExceptionHandler) : TUncaughtPascalExceptionHandler;
    Function SetOnUnCaughtExceptionHandler(aValue : TUncaughtJSExceptionHandler) : TUncaughtJSExceptionHandler;
    Procedure HookUncaughtExceptions;
    

The _HookUncaughtExceptions_ call hooks into the rtl handler for uncaught exceptions. 

After a call to _HookUncaughtExceptions_ the exceptions caught by the rtl handling mechanism will be displayed using ShowException. 

You can set your own handlers for uncaught exceptions with _SetOnUnCaughtExceptionHandler_. 

There are 2 handlers: one for every object that is an actual pascal object, one for Javascript objects such as JSError. This is because the roots of these 2 object trees are different. 

These calls return the old exception handler, if there were any. And if you set your own handler, _HookUncaughtExceptions_ will be called automatically. 

# Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [lazarus pas2js integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/pas2js_exceptions](https://web.archive.org/web/20250319120918/https://wiki.freepascal.org/pas2js_exceptions)_
