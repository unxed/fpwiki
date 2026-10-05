# Assigned

│ **English (en)** │    
****

` Assigned` is a [`function`](<Function.md> "Function") defined in the [System unit](<System_unit.md> "System unit") of the [Free Pascal](<FPC.md> "FPC") [Runtime Library](<RTL.md> "RTL"). It is used to determine if certain [types](<Type.md> "Type") of [variables](<Variable.md> "Variable") have been given a value: 

  * [Pointer variables](<Pointer.md> "Pointer")
  * [Procedural variables](</index.php?title=Procedural_variable&action=edit&redlink=1> "Procedural variable \(page does not exist\)") and method procedural variables
  * [Class variables](<Class.md> "Class")



The `function assigned` receives a [`boolean`](<Boolean.md> "Boolean") value `false` if the type pointer assigned to it indicates a [`nil`](<Nil.md> "Nil") value 
    
    
    function Assigned( P: Pointer ) : Boolean;
    

## see also

  * [`Assigned`](<https://www.freepascal.org/docs-html/rtl/system/assigned.html>)

---

_Source: [https://wiki.freepascal.org/Assigned](https://web.archive.org/web/20250219124110/https://wiki.freepascal.org/Assigned)_
