# FPC Internals/Parameters

│ **English (en)** │

back to contents [FPC internals](<../FPC_internals.md> "FPC internals")

FPC is using the CPU target specified parameter manager to manage the location of parameters passed into and routines. 

  
CPU target should implement parameter manager in **cpupara.pas** unit. 

The unit should initialize the global **ParaManager** variable if an instance of the CPU specific class. 
    
    
    ParaManager:=tcpuparamanager.create
    

The basic TParaManager class cannot and should not be created, as it contain the abstract methods that should be implemented by CPU target implementation class. 

## Contents

  * 1 Methods to Implement
    * 1.1 push_addr_param
    * 1.2 create_paraloc_info
    * 1.3 create_varargs_paraloc_info
    * 1.4 get_funcretloc
  * 2 See Also



## Methods to Implement

The following methods are critical for CPU target implementation 

### push_addr_param

Returns true if a parameter is too large to copy and only the address is pushed or if the parameter should be passed by address reference at all time. 
    
    
     function push_addr_param(varspez: tvarspez; def: tdef; calloption: tproccalloption) : boolean;
    

The typical approach would be first to inspect variable specifier argument **varspez**. 

Normally, vs_var, vs_out, vs_constref are always passed by reference and **push_addr_param** should return true. 

In many cases **var_const** should return true as well, however, it's driven by CPU target ABI and calling convention. (for example, if definition (**def**) is a record and it can fit CPU word, then the value of the structure can be put into memory/register. Thus the function should return false) 

If there variable specifier doesn't indicate a need to pass by address, the next step is to inspect a definition. 

The following definition types (**def.typ**) are typically passed via address (and the function should return true for those): 

  * Formal (formaldef)
  * Object instances (objectdef, but only if is_object() returns true)
  * Strings (stringdef) for types st_shortstring or st_longstring.
  * Arrays (arraydef) for open arrays, arrays of const.



Depending on CPU type **calloption** should be considered, if different calling conventions apply for the CPU target. 

### create_paraloc_info

This is used to populate the location information on all parameters for the routine as seen in either the caller or the callee. It returns the size allocated on the stack 
    
    
     function  create_paraloc_info(p : tabstractprocdef; side: tcallercallee):longint;
    

### create_varargs_paraloc_info

This is used to populate the location information on all parameters for the routine that are passed as varargs. It returns the size allocated on the stack (including the normal parameters) 
    
    
     function  create_varargs_paraloc_info(p : tabstractprocdef; side: tcallercallee; varargspara:tvarargsparalist):longint;
    

### get_funcretloc

Returns the location of the function result if p had def as function result instead of its actual result. Used if the compiler forces the function result to something different than the real result. 
    
    
     function  get_funcretloc(p : tabstractprocdef; side: tcallercallee; forcetempdef: tdef): tcgpara;
    

## See Also

  * [FPC Internals](<../FPC_Internals.md> "FPC Internals")

---

_Source: [https://wiki.freepascal.org/FPC_Internals/Parameters](https://web.archive.org/web/20231002191611/https://wiki.freepascal.org/FPC_Internals/Parameters)_
