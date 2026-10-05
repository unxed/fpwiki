# Flexible Array Member

│ **English (en)** │  **[français (fr)](</Flexible_Array_Member/fr> "Flexible Array Member/fr")** │    
****

Is a rarely used feature of C language. It was introduced with C99 standard. The feature was never adopted by C++ compiler (and that might be the reason of the rate use). Some C-like APIs could benefit from the feature, especially low level ones. 

## Contents

  * 1 Introduction
  * 2 Pascal
    * 2.1 Direct Addressing
    * 2.2 Using Additional Record
    * 2.3 Using Advanced Record
  * 3 See Also



## Introduction

**Disclaimer:** _this section is pretty much a copy-paste from[Wikipedia article](<https://en.wikipedia.org/wiki/Flexible_array_member>)_

Flexible Array Member is a member of a struct, which is an array without a given dimension. It must be the last member of such a struct and it must be accompanied by at least one other member, as in the following example: 
    
    
    typedef struct mystruct
    {
      DWORD a;
      DWORD b;
      DWORD c[]; // the flexible array member must be last
    };
    

Typically, such structures serve as the header in a larger, variable memory allocation: 
    
    
    struct mystruct* st= malloc(...);
    ...
    for (int i = 0; i < st->b; i++)
         st->c[i] = ...;
    

The sizeof operator on such a struct gives the size of the structure as if the flexible array member was empty. This may include padding added to accommodate the flexible member; the compiler is also free to re-use such padding as part of the array itself. 

It is common to allocate sizeof(struct) + array_len*sizeof(array element) bytes 

## Pascal

Pascal doesn't have flexible array member, thus the structure should be declared without the additional member, in order for sizeof(mystruct) to return the same value as in C. 
    
    
    type
      mystruct = record
        a : DWORD;
        b : DWORD;
      end;
      pmystruct = ^mystruct;
    

There are a few options on how the additional elements can be accessed: 

### Direct Addressing

By explicitly getting the address that goes after the header struct 
    
    
    var
     i   : integer;
     buf : Pmystruct;
     c   : PDword;
    begin
      GetMem(buf, sizeof(mystruct)+ extrasize);
      ...
      c:=Pointer(buf)+sizeof(mystruct);
      
      for i := 0 to buf.b-1 do
         c[i] := ...;
    

### Using Additional Record

Depending on the use, the additional record could overlap with the original record using [absolute](</index.php?title=absolute&action=edit&redlink=1> "absolute \(page does not exist\)"). 
    
    
    type
      mystructEx = record
        hdr : mystruct;
        c : PDword;
      end;
      pmystructEx = ^mystructEx;
    
    
    var
     i   : integer;
     buf : PmystructEx;
    begin
      GetMem(buf, sizeof(mystruct)+ extrasize);
      
      for i := 0 to buf.hdr.b-1 do
         buf.c[i] := ...;
    

### Using Advanced Record

Advanced records allow to methods and properties for records. Thus the field could be emulated through a method/property 
    
    
    {$modeswitch advancedrecords}
    
    type
      mystruct = packed record
        a,b: DWORD;
        function c: PDWORD; inline;
      end;
     
    function mystruct.c: PDWORD;
    begin
      Result := PDWORD(pbyte(@self.b)+sizeOf(self.b));
    end;
    

OR using an empty record. (An empty record has not struct, yet allows to get an address, which should save some time on address calculation) 
    
    
    {$modeswitch advancedrecords}
    
    type
      mystruct = packed record
        a,b: DWORD;
        function c: PDWORD; inline;
      strict private
        cStart: record end;
      end;
     
    function mystruct.c: PDWORD;
    begin
      Result := PDWORD(@cStart);
    end;
    

## See Also

  * <https://forum.lazarus.freepascal.org/index.php/topic,45998.0.html> \- original thread that started the discussion
  * <https://en.wikipedia.org/wiki/Flexible_array_member> \- wikipedia on Flexible Array Member
  * [Common problems when converting C header files](<Common_problems_when_converting_C_header_files.md> "Common problems when converting C header files")

---

_Source: [https://wiki.freepascal.org/Flexible_Array_Member](https://web.archive.org/web/20250516150150/https://wiki.freepascal.org/Flexible_Array_Member)_
