# OnCreate

## Overview

The **OnCreate** event is used to perform special processing when the form is created and is invoked by TCustomForm's constructor. 

Note that you can either implement this event or override the constructor of the form; but do not do both. Any objects created in the OnCreate event should be freed by the OnDestroy event.

---

_Source: [https://wiki.freepascal.org/OnCreate](https://web.archive.org/web/20230602212641/https://wiki.freepascal.org/OnCreate)_
