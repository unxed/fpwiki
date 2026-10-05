# Startup

The **startup** of an [application](<Application.md> "Application") is the point after the [initialization](<Initialization.md> "Initialization") of the [run-time library](<RTL.md> "RTL") where the user program has any necessary variables assigned, any needed [files](</File> "File") to be opened, any window handles allocated (on windowing systems), and other [resources](<resource.md> "resource") needed for the program to operate. If there is startup code necessary to be executed in any [unit](<Unit.md> "Unit") which is called by the primary application, the [main](</index.php?title=main&action=edit&redlink=1> "main \(page does not exist\)") procedure of each unit is called in the order it is declared in the program. 

After startup occurs, the [main](</index.php?title=main&action=edit&redlink=1> "main \(page does not exist\)") procedure of the main program then begins execution.

---

_Source: [https://wiki.freepascal.org/Startup](https://web.archive.org/web/20240920204128/https://wiki.freepascal.org/Startup)_
