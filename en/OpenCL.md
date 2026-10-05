# OpenCL

OpenCL (Open Computing Language) is a framework for writing programs that execute across heterogeneous platforms consisting of CPUs, GPUs, and other processors. OpenCL includes a language (based on C99) for writing kernels (functions that execute on OpenCL devices), plus APIs that are used to define and then control the heterogeneous platform. OpenCL provides [parallel programming](<Parallel_procedures.md> "Parallel procedures") using both task-based and data-based parallelism. 

[Official site](<http://www.khronos.org/opencl/>)

[Online Man pages on the official site](<http://www.khronos.org/opencl/sdk/1.0/docs/man/xhtml/>)

[Wikipedia OpenCL](<http://en.wikipedia.org/wiki/OpenCL>)

[OpenCL at Apple Developer library](<http://developer.apple.com/mac/library/documentation/Performance/Conceptual/OpenCL_MacProgGuide/Introduction/Introduction.html>)

  
OpenCL headers are available in standard FPC packages. Headers are can be used for: 

  * MacOSX 10.6 OpenCL framework
  * NVidia OpenCL Windows .dll
  * ATI using ATI Stream SDK ([ATI Stream SDK](<http://developer.amd.com/gpu/ATIStreamSDK/Pages/default.aspx>)) under Linux. NVidia OpenCL under Linux should work as well, but it is untested.



  
**Important!** ATI OpenCL under Linux requires X server running and building with GUI staff in (GUI window application). Tested under Linux v2.6.36 32bit, X server v1.8.2, ATI proprietary drivers v10.10, ati stream sdk v2.2.

---

_Source: [https://wiki.freepascal.org/OpenCL](https://web.archive.org/web/20230326222804/https://wiki.freepascal.org/OpenCL)_
