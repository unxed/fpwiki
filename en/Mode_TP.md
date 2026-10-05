# Mode TP

│ **English (en)** │  **[русский (ru)](<../ru/Mode_TP.md>)** │

This mode is provided for highest level of compatibility with [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")/[Borland Pascal](<Borland_Pascal.md> "Borland Pascal") compilers in order to simplify porting of existing code to FPC. It turns on some features which are not considered as recommended to use in general (e.g. because of their ambiguity or potential side-effects), slightly modifies syntax rules where necessary, changes the default assembler mode to $ASMMODE INTEL, etc. You enable it with mode switch **${mode TP}** in source code or with the compiler command line option **-Mtp**. 

See [standalone page](<Porting_low-level_DOS_code_for_TP/BP_to_GO32v2_with_FPC.md> "Porting low-level DOS code for TP/BP to GO32v2 with FPC") for more information about porting low-level DOS code written with TP/BP to GO32v2 target of FPC. 

## see also

  * [sub-§ “Turbo Pascal compatibility mode” in the _Free Pascal user’s guide_](<https://www.freepascal.org/docs-html/user/usersu84.html>)

---

_Source: [https://wiki.freepascal.org/Mode_TP](https://web.archive.org/web/20241221172505/https://wiki.freepascal.org/Mode_TP)_
