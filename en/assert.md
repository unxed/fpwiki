# assert

The compiler procedure [`assert`](<https://www.freepascal.org/docs-html/rtl/system/assert.html>) inserts an assertion. This is a procedure requiring the [`Boolean`](<Boolean.md> "Boolean") value [`true`](<false_and_true.md> "false and true") in order to proceed, otherwise a [run-time error](<runtime_error.md> "runtime error") is generated. 

## Contents

  * 1 use
  * 2 behavior
  * 3 application
  * 4 see also



## use

The procedure `assert` has the following signature: 
    
    
    assert(Boolean)
    

The passed parameter has to be `true` to continue program flow. Usually this is an expression. 

By default the [FPC](<FPC.md> "FPC") has code generation for assertions disabled. That means, invocations of `assert` do not end up in the generated binary, thus have no effect whatsoever. By specifying the [local compiler directive](<local_compiler_directives.md> "local compiler directives") [`{$assertions on}`](<$Assertions.md> "$Assertions") (or `{$C+}` for short) or specifying the `‑Sa` command-line switch, appropriate code for assertions is inserted. 

## behavior

If the first parameter is `false`, the [`assertErrorProc` procedure](<https://www.freepascal.org/docs-html/rtl/system/asserterrorproc.html>) is called. This is by default a procedure generating the RTE 227 “Assertion failed error”. In order to convey more information, the `assert` procedure accepts a second optional short string parameter: 
    
    
    assert(Boolean, shortstring)
    

The second parameter’s value is passed to the current `assertErrorProc`. The default handler prints the message to standard error. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The [`sysUtils` unit](<sysutils.md> "sysutils") installs an `assertErrorProc` handler generating an [`eAssertionFailed` exception](<https://www.freepascal.org/docs-html/rtl/sysutils/eassertionfailed.html>).

## application

Assertions are a straightforward concept ensuring certain statements hold true. However, they do not guarantee your program is indeed correct: Assertions can be a tool to _confirm the presence_ of programming mistakes (“bugs”), but they _cannot_ prove the _absence_ of any. 

Assertions are frequently used _during development_. For example, in the following [operator overload](<Operator_overloading.md> "Operator overloading") the assertion ensures certain properties about the `‑` operation: 
    
    
    operator - (const positive: foo): foo;
    begin
    	result := negation(foo);
    	assert(sum(positive, result) = neutralElementOfAddition);
    end;
    

However, checking this over and over again is not necessary in a production program. This is a decision that has to be made on a per-application-basis, sometimes on a per-assertion basis. 

Although `assert` is usually provided with a non-trivial Boolean expression, constants are allowed too. In the following piece of code the programmer used an assertion to ensure there is always one alternative taken. 
    
    
    case … of
    	a: …
    	b: …
    	c: …
    	otherwise
    	begin
    		assert(false, 'one case has to match!');
    	end;
    end;
    

Otherwise, for Delphi-compatibility there would be no error if no case matches. Using an `assert` statement provides the flexibility to include or omit it from the generated code, though. 

Assertions are _not_ used to verify that the compiler works: 
    
    
    procedure foo(var bar: toot);
    begin
    	assert(assigned(@bar)); // wrong
    	…
    

Assertions are also not used for circumstances more specialized means are available for. That means, assertions are not supposed to replace 

  * [`{$rangeChecks}`](<$rangeChecks.md> "$rangeChecks")
  * [`{$objectChecks}`](</index.php?title=$objectChecks&action=edit&redlink=1> "$objectChecks \(page does not exist\)")
  * [`{$overflowChecks}`](</index.php?title=$overflowChecks&action=edit&redlink=1> "$overflowChecks \(page does not exist\)")



among other kinds of checks. 

The paper [the power of 10](<The_Power_of_10.md> "The Power of 10") suggest two assertions minimum per function. See also 

  * [The Power of Proper Planning and Practices](<The_Power_of_Proper_Planning_and_Practices.md> "The Power of Proper Planning and Practices").
  * [Defensive programming techniques § “How to use meaningful Assertions”](<Defensive_programming_techniques.md> "Defensive programming techniques")



## see also

  * Article: [Assertion (software development)](<https://en.wikipedia.org/wiki/Assertion_\(software_development\)>) in the English Wikipedia

---

_Source: [https://wiki.freepascal.org/assert](https://web.archive.org/web/20250219130709/https://wiki.freepascal.org/assert)_
