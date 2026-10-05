# New

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **new** : 

  * belongs to object-oriented programming;
  * creates a new instance.



Example 1: 
    
    
      var
       intI : ^ Integer ;
    
     begin
       ...
       if assigned ( intI ) then ... // Checks whether an address has been assigned to intI
       ...
       new ( intI ) ;  // Assigns a valid address to intI
       ...
       dispose ( intI ) ;  // Releases the address
       ...
    

Example 2: 
    
    
      grade
       PListItem = ^ TListItem ;
       TListItem = record
         data : string ;
         next : PListItem ;  // reference to the next element
       end ;
    
     var
       List : PListItem ;
    
     begin
       ...
       New ( list ^. Next ) ;  // Adds a new item to the list
       List ^.  next ^.  data : = '' ;  // empties the element
       List ^.  next ^.  next : = nil ;  // sets the following pointer to nothing for security
       ...
       dispose ( list ) ;  // Releases the list
       ...

---

_Source: [https://wiki.freepascal.org/New](https://web.archive.org/web/20240528013257/https://wiki.freepascal.org/New)_
