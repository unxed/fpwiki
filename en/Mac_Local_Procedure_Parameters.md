# Mac Local Procedure Parameters

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This is a proposal

In procedure A, a reference to a local or global procedure C is to be passed as a parameter to a procedure B. Procedure B might be declared in another unit, unaware of possible type of callers. The body of B will in turn call procedure C. If procedure C is local, it can now access variables local to A, if global, it can't. Procedure B might also pass the parameter further, but is not allowed to store the parameter in a variable. 

The procedure parameters can be implemented as a conceptual record with one procedure pointer and one local scope pointer. 

When passing local procedures, the local scope pointer will point to the local variables, when passing a global procedure, the local scope pointer is set to nil. 

The tricky problem is how B should call C. If the local scope pointer is nil, C should be called as a global procedure, if not nil it should be called as a local procedure, that is along with the local scope pointer. 

By putting the local scope pointer last, the procedure calling code chunk can be the same for local and global calls. A runtime check checks whether the local scope pointer is not nil, and if so pushes it. 

All three procedures A, B and C have to be declared in MacPas units for this to work, otherwise there is a compilation error. 

In the future, for some register based calling conventions, if the local scope pointer is put in a register which otherwise would not be in use, the runtime check can be optimized away. (for PowerPC, if the local scope pointer happens to be allocated in one of r3 - r10) 

Also, cdecl for local procedure calls should not be allowed.

---

_Source: [https://wiki.freepascal.org/Mac_Local_Procedure_Parameters](https://web.archive.org/web/20240226045228/https://wiki.freepascal.org/Mac_Local_Procedure_Parameters)_
