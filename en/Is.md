# Is

│ **English (en)** │

The [reserved word](<Reserved_word.md> "Reserved word") `is` appears as: 

  * an [operator symbol](<Operator.md> "Operator") in [object-oriented programming](<object-oriented_programming.md> "object-oriented programming"), or as a
  * a modifier qualifying [routine variable](</index.php?title=Procedural_variable&action=edit&redlink=1> "Procedural variable \(page does not exist\)") (`is nested`).



## Operator

The operator `is` tests, whether an object (first operand) is an instance of a [class](<Class.md> "Class") or its children. 
    
    
    fruit is citrusFruit
    

is equivalent to
    
    
    fruit.inheritsFrom(citrusFruit)
    

. 

The [expression](<expression.md> "expression") is [`true`](<false_and_true.md> "false and true") if and only if `fruit` descends from `citrusFruit`. 

## Modifier

If `{$modeSwitch nestedProcVars+}` or [`{$mode ISO}`](<Mode_iso.md> "Mode iso") routine variables declared with the modifier `is nested` allows those to be assigned to nested routines. Otherwise only global routines' addresses can be assigned to routine variables.

---

_Source: [https://wiki.freepascal.org/Is](https://web.archive.org/web/20250219125802/https://wiki.freepascal.org/Is)_
