# gdbm

## GNU database management routines

The **gdbm unit** is a translation of the gdbm.h header files, with some additional routines. The headers translated without any problems, the only thing that should be taken into account is that the GDBM SYNC constant (for open flags) has been renamed to GDMB DOSYNC because it conflicts with the gdbm sync function. 

Be careful: the TDatum.dptr data pointer which is allocated by the gdbm routines should be freed by the C free() call, NOT with the pascal FreeMem() call. 

A solution for this is to use the 'cmem' unit, which replaces the standard FPC memory manager with the C memory manager. In that case, freemem() may be used to free the dptr field of the TDatum record. 

On top of the plain C header translations, the GDBM routines have been overloaded with routines that accept plain strings as key or data parameters. This means the following routines have been added: 
    
    
    function gdbm open(Const para1:string; para2:longint; para3:longint; para4:longint; para5:TGDBMErrorCallBack ):PGDBM FILE;
    function gdbm store(para1:PGDBM FILE; Const para2:string; Const para3:string; para4:longint):Boolean;
    function gdbm fetch(para1:PGDBM FILE; Const para2:string):string;
    function gdbm delete(para1:PGDBM FILE; Const para2:string):boolean;
    procedure gdbm firstkey(para1:PGDBM FILE; var key :string);
    function gdbm nextkey(para1:PGDBM FILE; Const para2:string):string;
    function gdbm exists(para1:PGDBM FILE; Const para2:string):boolean;
    

They are just the C routines, but with the TDatum type (a record) replaced by a string. The routines take automatically care of memory deallocation. 

Functions that returned an integer to indicate success or failure have been replaced by functions that return a boolean. 

There are two example programs, **testgdbm** and **testgdbm2** which test the routines. 

Go back to [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/gdbm](https://web.archive.org/web/20220706221546/https://wiki.freepascal.org/gdbm)_
