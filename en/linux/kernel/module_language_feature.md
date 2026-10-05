# Overview

The purpose of this page is to give a specification of a FPC feature allowing writing (Linux) kernel modules using FPC. The main purpose is to an easy syntax hiding all the details of sectioning and linking. The ultimate goal of these wiki pages is to provide a proposal for a future support of natural linux kernel module support in particular and driver support in general (for win32...) A dedicated page module language feature is intended to provide specification for this future support. 

# Language enhancement

This proposal is based on adding a new keyword **module** triggering a special linking mechanism. Please note that this keyword **module** is already recognized by GPC for other purpose. The concept is very similar, however the use is different. Indeed, GPC modules are intended to be linked to programs, however these featured modules are intended to be loadable by the kernel. The latter doesn't, of course, prevent the former. 

The easiest way to illustrate this is using the following example 
    
    
     module kpmod;
     uses unit1, unit2,...unitn;
     const
      MODULE_NAME = 'kpmod';
     type
       TType = some type;
     const 
       aConst = a value;
     initialization
       {Module initialization code}
       printk(KERN_INFO + 'Module "%s" is being loaded'#10, MODULE_NAME);
     finalization
       {Module cleanup code}
       printk(KERN_INFO + 'Module "%s" is being unloaded'#10, MODULE_NAME);
     end.
    

# Working details

---

_Source: [https://wiki.freepascal.org/linux/kernel/module_language_feature](https://web.archive.org/web/20240226045230/https://wiki.freepascal.org/linux/kernel/module_language_feature)_
