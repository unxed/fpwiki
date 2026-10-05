# CMem

│ **English (en)** │  **[русский (ru)](<../ru/CMem.md>)** │

If you include the **cmem** unit in your uses clause of your program, it will replace the native Free Pascal memory manager with the C library memory manager. All memory management is then done by the C memory manager. 

The unit should be the first unit in the uses clause, otherwise memory can already be allocated by initialization routines in units that are initialized before the C memory manager is installed. 

In cases where several units like **cthreads** , **cmem** and **cwstrings** are recommended to be placed first, due to how the units work a sensible order is 

  * cmem
  * cthreads
  * cwstrings



There is a small example program [**testcmem**](<https://gitlab.com/freepascal.org/fpc/source/-/blob/release_3_2_2/tests/test/testcmem.pp>), which demonstrates the use of the cmem unit. 

CMem should not be used if [heaptrc](<heaptrc.md> "heaptrc") is used.

---

_Source: [https://wiki.freepascal.org/CMem](https://web.archive.org/web/20250517154701/https://wiki.freepascal.org/CMem)_
