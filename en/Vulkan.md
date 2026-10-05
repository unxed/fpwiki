# Vulkan

Vulkan is a new low-level Graphics API, intended to replace OpenGL. 

## Float Point Exception

Similar to OpenGL apps, the float-point exceptions should be turned off. This can be achieved by adding the following code prior to calling Vulkan API: 

    
    
    
    SetExceptionMask([exInvalidOp, exDenormalized, exPrecision]);
    

[SetExceptionMask](<http://www.freepascal.org/docs-html/rtl/math/setexceptionmask.html>) function is declared in Math unit 

## Headers

There are no Vulkan headers in fpc packages, but some are available online: 

  * <https://github.com/BeRo1985/pasvulkan>
  * <https://github.com/MaksymTymkovych/Delphi-Vulkan>
    * (Delphi-Vulkan fork) <https://github.com/skalogryz/FPC-Vulkan>
  * <http://git.ccs-baumann.de/bitspace/Vulkan/tree/master/projects>
  * <https://github.com/james-mcjohnson/VulkanLibraryForFreePascal>



## See Also

  * [Vulkan on Wikipedia](<https://en.wikipedia.org/wiki/Vulkan_\(API\)>)
  * <https://www.khronos.org/vulkan/> \- official site
  * [OpenGL](<OpenGL.md> "OpenGL")

---

_Source: [https://wiki.freepascal.org/Vulkan](https://web.archive.org/web/20241212120841/https://wiki.freepascal.org/Vulkan)_
