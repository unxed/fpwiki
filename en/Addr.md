# Addr

│ **English (en)** │

[`Addr`](<https://www.freepascal.org/docs-html/rtl/system/addr.html>) is a compiler intrinsic behaving like a unary [function](<Function.md> "Function") returning the address of an object (if such exits). It is equivalent to the [`@` address operator](<@.md> "@"), however, `addr` always returns an _untyped_ [pointer](<Pointer.md> "Pointer") ([since FPC 2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0")). This behavior is compliant to [Borland Pascal’s](<Borland_Pascal.md> "Borland Pascal") definition of the `addr` function, where this function originally comes from. 

In [Pascal-XSC](<PXSC.md> "PXSC"), the `loc` function is the same as `addr` presented here. 

## usage

`Addr` is used just like every other function. 
    
    
    program addrDemo(input, output, stdErr);
    var
    	p: pointer;
    begin
    	p := addr(p);
    	writeLn('Pointer p is located at ', sysBackTraceStr(p), '.');
    end.
    

`Addr` can only be used on objects that have an address, that means reserve memory. The following objects do not have an address, thus `addr` cannot be used on them: 

  * module [identifiers](<Identifier.md> "Identifier"): modules form [namespaces](<Namespaces.md> "Namespaces"), i. e. become part of identifier definitions within the modules
  * constant expressions: [constants](<Constant.md> "Constant") do not identify objects, meaning they do not occupy any memory
  * compiler intrinsics, such as [`writeLn`](<Write.md> "Write") or `addr` itself
  * (despite being a special case of `function`s) [operator overloads](<Operator_overloading.md> "Operator overloading")
  * [properties](</Property> "Property")



In case of [`type`s](<Type.md> "Type"), the [`typeInfo`](<https://www.freepascal.org/docs-html/rtl/system/typeinfo.html>) compiler intrinsic has to be used in order to obtain a reference to [RTTI](<RTTI.md> "RTTI"). 

## application

`Addr` primarily exists for compatibility with code that was written for/with Borland Pascal. The `@`‑address operator (in conjunction with `{$typedAddress on}`) should be preferred, since it can return _typed_ addresses. This will prevent some programming mistakes. 

Also, in [`{$mode FPC}`](<Mode_FPC.md> "Mode FPC") and [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") the `@`‑address operator _has_ to be used to assign values to procedural values (unless `{$modeSwitch classicalProcVars+}` is set). In [`{$mode TP}`](<Mode_TP.md> "Mode TP") and [`{$mode Delphi}`](<Mode_TP.md> "Mode TP"), however, no operator at all may be used. 

Last but not least, in [`asm`](<Asm.md> "Asm") blocks, `addr` cannot be used to obtain addresses of labels, but `@` can. 

## see also

  * [`farAddr` (relevant for i8086-msdos platform)](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0")

---

_Source: [https://wiki.freepascal.org/Addr](https://web.archive.org/web/20250323150022/https://wiki.freepascal.org/Addr)_
