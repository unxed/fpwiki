# datatype visibility

│ **English (en)** │

Back to [data types](<Data_type.md> "Data type"). 

  
The visibility and thus the usability of data fields (eg of the type variable and constant) depends on the level of their publication. 

## Global level

Data fields that are published in the interface section: 

Can be seen within the unit in which they were advertised. This means all procedures and functions of the unit can see and use these data fields. 

These data fields can also be seen by all units that include this unit under the uses clause. That means all procedures and functions that have integrated this unit can see these data fields and can use these data fields. 

## Module level

Data fields published in the implementation section: 

Can only be seen within the unit in which they were published. This means all procedures and functions in the unit can see and use these data fields. 

## Subroutine level (procedures and functions)

Data fields that are published in a subroutine: 

Can only be seen within the subroutine in which they were published. This means that these data fields can only be used in the subroutine in which they were published. 
    
    
     interface
     // data fields in this section are seen by each unit,
     // which includes this unit in its unit clause and
     // they are seen by the subroutines of this unit.
    
     var
       ...;
     const
       ...;
    
    
     implementation
     // Data fields in this section are only seen by the subroutines in this unit.
    
     var
       ...;
     const
       ...;
    
    
     procedure Example1();
     // Data fields that are published here are only seen by the subroutine
     var
       ...;
     const
       ...;
     begin
       ...;
     end;
    
     function Example2() : Int64;
     // Data fields that are published here are only seen by the subroutine
     var
       ...;
     const
       ...;
     begin
       ...;
     end;
    
     end.

---

_Source: [https://wiki.freepascal.org/datatype_visibility](https://web.archive.org/web/20250516143517/https://wiki.freepascal.org/datatype_visibility)_
