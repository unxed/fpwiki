# Mach-O

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

  
_Mach-O, short for Mach object file format, is a file format for executables, object code, shared libraries, dynamically-loaded code, and core dumps. A derivation of the a.out format, Mach-O offered more extensibility and faster access to information in the symbol table._ Source: [Wikipedia](<http://en.wikipedia.org/wiki/Mach-O>). 

The Apple file format reference can be found [here](<https://developer.apple.com/library/archive/documentation/Performance/Conceptual/CodeFootprint/Articles/MachOOverview.html>)

  
The following tools are used in macOS to view Mach-O files: 

**otool** \- object file displaying tool 

**nm** \- display name list (symbol table) 

  


## Contents

  * 1 Objective-C segment
    * 1.1 Structures strings
  * 2 Sections
    * 2.1 __image_info
    * 2.2 __module_info
    * 2.3 __symbols
    * 2.4 __class, __meta_class
    * 2.5 __instance_vars
    * 2.6 __inst_meth, __cls_meth
    * 2.7 __protocol
    * 2.8 __cat_cls_meth (protocols list)
    * 2.9 __cat_inst_meth
    * 2.10 __cat_cls_meth
  * 3 Mach-O additional 30Kb size



## Objective-C segment

There is no documentation about __OBJC segment and its sections. The following information has been gathered from cctools [sources](<http://opensource.apple.com/source/cctools/cctools-667.3/>)

### Structures strings

Some structures in sections contain name pointers. These names are stored in the c-strings section (segment: __TEXT; section: __cstring). The file offset for the string name can be evaluated in the following way: 
    
    
    string_file_offset := cstr_section.offset + (name_addr - cstr_section.addr);
    

## Sections

### __image_info

The section contains only the image info information: 
    
    
    imageInfo = packed record
      version : uint32_t;  // zero
      flags   : uint32_t;  // for objc 1.0 - zero
    end;
    

Flags values: 
    
    
    ImageInfo_F_and_C = $01;
    ImageInfo_GC      = $02;
    ImageInfo_GC_only = $04;
    

### __module_info

(objc_module record is declared at objc headers). 

The number of objc_module structures depends on the number of .m files with objects declarations compiled. _symtab contains the number of classes and categories declared in the module. 
    
    
    objc_module = packed record
      version : culong; // version number = 7 
      size    : culong; // sizeof(objc_module)?
      name    : PChar;  // virtual memory address of the module name
                        // Usually mapped to NULL string
      _symtab : Symtab; // virtual memory address of a proper objc_symtab structure (in __symbols section)
    end;
    

### __symbols

the section contains symbol table for a module. (symtable record is declared at objc headers) 
    
    
     objc_symtab = record
       sel_ref_cnt : culong;  // zero
       refs        : PSEL;    // zero
       cls_def_cnt : cushort; // number of declared classes in the module
       cat_def_cnt : cushort; // number of declared categories
       defs: array [0..cls_def_cnt+cat_def_cnt-1] of Pointer; // array of virtual address of declarations
          // 0..cls_def_cnt - 1           : virtual address of class declarations (in "__class" section, can be empty)
          // cls_def_cnt..cat_def_cnt-1   : virtual address of categories declarations (can be empty)
     end;
    

### __class, __meta_class

_the section may omit, if none of modules declare any custom classes_

Both sections use identical structure objc_class. (objc_class record is declared at objc header) Objective-C classes are declared in pair with their meta_classes. 
    
    
    objc_class = record
      isa           : PChar;   // for class declaration: virtual address of meta-class declaration
                               // for meta-class declaration: virtual address of "NSObject" string?
    
      super_class   : PChar;   // contains the pointer to super_class name
      name          : PChar;   // pointer to class name 		
      version       : PChar;   // = 0 (for obj-c version 1?)
    
      info          : culong;  // CLS_CLASS for classes
                               // CLS_META  for meta-classes 
    
      instance_size : culong;  // size of the instance:
                               // class: size of the class instance 
                               // meta-class: 48 bytes 
                               
      ivars         : Pobjc_ivar_list;       // virtual address of objc_ivar_list (stored in "__instance_vars" section)
                                             // meta-classes don't have ivars list (=0)
    
      methodLists   : PPobjc_method_list;    // virtual address of objc_method_list
                                             // class declaration has instance methods list (stored in "__inst_meth" section)
                                             // meta-class declaration has class methods list (stored in "__cls_meth" section)
     
      cache         : Pobjc_cache;           // zero
      protocols     : Pobjc_protocol_list;   // pointer to protocols list. (stored in "__cat_cls_meth" section)
    end;
    

### __instance_vars

_the section is optional and may omit if none of classes declares instance variables_

The section consists of number objc_ivar_list structutres. The number of structures depends upon number of declared classes using instance variables. (objc_ivar_list is declared in objc header) 
    
    
    objc_ivar_list = record
      ivar_count  : cint;   // number of variables in the list
      ivar_list   : array[0..ivar_count-1] of objc_ivar;  variable length structure }
    end;
    
    
    
    objc_ivar = record
      ivar_name   : PChar; // vm addr of the variable name
      ivar_type   : PChar; // vm addr of obj-c variable encoded type 
      ivar_offset : cint;  // offset from the start of the instance. (the lowest ivar_offset is 40,
                           // because sizeof(objc_class) = 40, and objc_class is also part of the instance)
    end;
    

### __inst_meth, __cls_meth

_the section is optional and may omit if none of classes declares any methods_

Both section has identical format. They contain number of objc_method_list records. Number of records is depending on classes declared in modules. 
    
    
    objc_method_list = record
      obsolete     : Pobjc_method_list;  // not used, always zero
      method_count : cint;               // number of objc_method in method_list array 
      method_list  : array[0..method_count-1] of objc_method;	
    end;
    
    
    
    objc_method = record
      method_name   : SEL;   // virtual address of the selector name 
                             // (selector name is stored as other names is in __TEXT __cstring section)
                             // selector name is short: i.e. "methodName:", and not "+[className methodName:]"
      method_types  : PChar; // obj-c encoded function parameters 
      method_imp    : IMP;   // virtual address of method implementation function entry point (in __TEXT __text section)
    end;
    

### __protocol

### __cat_cls_meth (protocols list)

### __cat_inst_meth

### __cat_cls_meth

## Mach-O additional 30Kb size

FPC built mach-o executables are somehow larger, compared to the win32 target, for example 
    
    
    begin
      writeln('hello world');
    end.
    

gives a 30k (stripped) executable for Win, and 60k for macOS. 

Jonas Maebe: _It's because there was a bug in older versions of the Darwin linker that required adding ".reference" assembler directives for routines that have more than one assembler name (most of the compiler helpers in the RTL have that). This fixed the problem, but as a result they are never smart linked out. It's only a fixed overhead of that 30kb (most programs don't contain any extra routines with multiple assembler names)_

FPC 3.3.1 (trunk r44876) produces a 64 bit binary of 427,920 bytes; add -XX to smart link and it is reduced to 55,656 bytes; now strip it and it's just 47,224 bytes (~46K) which is pretty reasonable.

---

_Source: [https://wiki.freepascal.org/Mach-O](https://web.archive.org/web/20241209080015/https://wiki.freepascal.org/Mach-O)_
