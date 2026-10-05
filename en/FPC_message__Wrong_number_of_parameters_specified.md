# FPC message: Wrong number of parameters specified

│ **English (en)** │

## Missing parameter or too many parameters

You confused the function and forgot a parameter or added a parameter too much. 

## Missing `@`

For example: 
    
    
    Button1.Click := Button1Click;
    

In [`{$mode objfpc}`](<Mode_ObjFPC.md> "Mode ObjFPC") you must add the [`@`-address-operator](<@.md> "@") to tell the compiler, that you want the pointer to the function, not the result of the function: 
    
    
    Button1.Click := @Button1Click;
    

Delphi users often confuse this, because Delphi allows it and adds the @ internally. If you prefer the Delphi syntax you can use [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") instead of `{$mode ObjFPC}`, or use the [mode switch](</index.php?title=sGlobalModeswitch&action=edit&redlink=1> "sGlobalModeswitch \(page does not exist\)") `{$modeswitch classicprocvars on}`.

---

_Source: [https://wiki.freepascal.org/FPC_message%3A_Wrong_number_of_parameters_specified](https://web.archive.org/web/20210621091145/https://wiki.freepascal.org/FPC_message%3A_Wrong_number_of_parameters_specified)_
