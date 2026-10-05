# Method

│ **English (en)** │  **[русский (ru)](<../ru/Method.md>)** │

A **method** is a [routine](<Routine.md> "Routine") that is associated with an [`object`](<Object.md> "Object") or [`class`](<Class.md> "Class"). 

## self identifier

Inside method definitions the special identifier `self` is available. In static or class methods it identifies the `object`/`class` type itself. In instance methods `self` identifies the very instance. 

However, since static class methods are just “global” routines within the type's namespace, such methods do not know the `self` identifier. [¹](<https://www.freepascal.org/docs-html/ref/refsu30.html>)[²](<https://www.freepascal.org/docs-html/ref/refsu22.html#x66-880005.5.2>)

## see also

  * [`property`](</Property> "Property")

---

_Source: [https://wiki.freepascal.org/Method](https://web.archive.org/web/20250420120254/https://wiki.freepascal.org/Method)_
