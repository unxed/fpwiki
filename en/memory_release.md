# memory release

│ **[Deutsch (de)](</memory_release/de> "memory release/de")** │  **English (en)** │    
****

# Shared objects

  * **Destroy** releases the storage space (an object).
  * **Free** checks whether the object reference is nil. If the object reference is **not** nil, the storage space (the object) is released. This procedure prevents exceptions when releasing the storage space.
  * **Nil** deletes the reference to an object. Note: The object should be released beforehand, otherwise so-called memory leaks occur.
  * **FreeAndNil** works like **Free** and afterwards sets the object reference (the pointer) to nil.



# Pointers

  * **Dispose** releases the memory for typed pointers that were created with **New**.
  * **FreeMem** releases the memory for untyped pointers that were created with **GetMem**.

---

_Source: [https://wiki.freepascal.org/memory_release](https://web.archive.org/web/20250419145743/https://wiki.freepascal.org/memory_release)_
