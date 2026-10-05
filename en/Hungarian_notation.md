# Hungarian notation

Hungarian notation in a coding style adopted by MS for C development under Windows. This variable naming technique claims to harmonize, to improve the readability of the existing variables, so to save time. Here is an example list, neither official nor fixed (the prefixes are usually based on the information theory, which concludes that consonants can be inferred in the middle of vowels. It uses too, the Camel case notation a lot). The name refection tool is perfect to use this technique easily. 

  
  
  
[Miscellaneous types:](</index.php?title=Miscellaneous_types:&action=edit&redlink=1> "Miscellaneous types: \(page does not exist\)")  

    
    
    F       Property Variable
    T       Type
    P       Type of pointer to another type
    On      Event
    

  
[Basic types:](</index.php?title=Basic_types:&action=edit&redlink=1> "Basic types: \(page does not exist\)")  

    
    
    b       Boolean
    c       Char
    s       String
    d       for all reals\floats as Double, Extended, ...
    i       for all integers as Integer, Long Integer, Smallint, ...
    p       Generic Pointer
    pz      PChar
    ...\...
    

  
[Published name (controls):](</index.php?title=Published_name_\(controls\):&action=edit&redlink=1> "Published name \(controls\): \(page does not exist\)")  

    
    
    btn       TButton; ex.: btnFoo1
                   bbtn    TBitmap Button
                   optbtn  TOption Button
                   sbtn    TSpeedButton
    chk       TCheck Box
    cbx       TComboBox
    db        TDatabase
    dts       TDataSource
    edt       TEdit Box
    frm       TForm
    grd       TDrawGrid
                   sgrd   TString Grid
                   dbgrd  TDBGrid
    img       Image
    itm       TMenuItem
    lbl       TLabel
    lbx       TListbox2
                   dblbx  TDBListbox
    mnu       TMainMenu
                   pmnu   TPopup Menu
    mem       TMemo
    notbk     TNotebook
    tabs      TTabset
    tabnotbk  Tabbed Notebook
    hdr       THeader
    stb       TStatusBar 
    outl      TOutline
    pnl       TPanel
    tbl       TTable
    qry       TQuery
    timr      Timer
    fld       TField
    inif      TIniFile
    ...\...
    

  
[Components \ Objects:](</index.php?title=Components_%5C_Objects:&action=edit&redlink=1> "Components \\ Objects: \(page does not exist\)")
    
    
    cmp       TComponent
    oSL       TStringList; ex.: oSLfoo
    oStrm     TStream; ex.: oStrmFoo
    ...\...
    

  
[Miscellaneous notable scope:](</index.php?title=Miscellaneous_notable_scope:&action=edit&redlink=1> "Miscellaneous notable scope: \(page does not exist\)")  

    
    
    g  means Global, wheras its type (simple or object)
    

  
[Memory scope management:](</index.php?title=Memory_scope_management:&action=edit&redlink=1> "Memory scope management: \(page does not exist\)")  

    
    
    o  means, from a memory scope management, that oMyObj must be freed within the scope of its declaration; ex.: oMyObject; ex.: goMyObject
    o_ means, from a memory scope management, that o_myObj must not be freed here, i.e. must not be freed in the scope of its declaration, but elsewhere, in a remote scope, where there is an statement declaration of oMyObj, which is the one where this object was created!; ex: o_myObj
    

  
[Constants:](</index.php?title=Constants:&action=edit&redlink=1> "Constants: \(page does not exist\)")  
  
Constants are often written in uppercase ( like in C): 
    
    
    const LF_WINDOWS = #13#10;
    const LF_LINUX = #10;
    

Another notation could consist of mixing a lowercase c - for constant - followed by its basic type: 
    
    
    const ciIndex = 1;
    const csPathHome = '/home';
    const ccNull = #0 
    ...\...

---

_Source: [https://wiki.freepascal.org/Hungarian_notation](https://web.archive.org/web/20220817134636/https://wiki.freepascal.org/Hungarian_notation)_
