# $O

│ **English (en)** │

  
Back to [global compiler directives](<global_compiler_directives.md> "global compiler directives"). 

  
Since Free Pascal v2.0.0, the [_global compiler directive_](<global_compiler_directives.md> "global compiler directives") **$O** has the same meaning as the [_local compiler directive_](<local_compiler_directives.md> "local compiler directives") **$OPTIMIZATION** switching on/off level two optimizations. 

**$O** uses the + and - switches. 

**$OPTIMIZATION** uses the ON and OFF switches. 

Example: 
    
    
      // Enable global level 2 optimization
      {$O+}
    
      // Enable local level 2 optimization
      {$OPTIMIZATION ON}

---

_Source: [https://wiki.freepascal.org/$O](https://web.archive.org/web/20241004031829/https://wiki.freepascal.org/$O)_
