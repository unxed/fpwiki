# TForm.OnDestroy

│ **English (en)** │  **[русский (ru)](<../ru/TForm.md>)** │

## Overview

The **OnDestroy** event is used to perform special processing when the form is destroyed. 

Note that you can either implement this event or override the destructor of the form; but do not do both. 

This event should destroy any objects created in the OnCreate event.

---

_Source: [https://wiki.freepascal.org/TForm.OnDestroy](https://web.archive.org/web/20230322071650/https://wiki.freepascal.org/TForm.OnDestroy)_
