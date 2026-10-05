# chmhelp

Lazarus has a **chmhelp** package (components/chmhelp). 

The package is an external help viewer ([lhelp](<lhelp.md> "lhelp")) that communicates with the IDE using SimpleIPC; see [Installing Help in the IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE") and [Help protocol](<Help_protocol.md> "Help protocol") for more details. The CHM support is based on FPC package [chm](<chm.md> "chm"). 

Troubleshooting: recompiling and configuring the package and lhelp.exe often do the trick. Read the file components/chmhelp/README.txt. 

There are multiple help files you can/should download/use: 

  1. the FPC rtl.chm and fcl.chm that document FreePascal components can be downloaded in a zip file from [[1]](<https://www.freepascal.org/down/docs/docs.var>).
  2. the Free Pascal Programmer's Guide (prog.chm), Free Pascal: Reference guide (ref.chm), Free Pascal: User’s Guide (user.chm) and FPDoc: Free Pascal code documenter: Reference manual (fpdoc.chm), probably available in the zip file mentioned above
  3. the Lazarus Component Library help file, lcl.chm, can be downloaded from [[2]](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Documentation/>).



If you want to edit/generate your own chm help files, also put any .xct (cross-reference) files in the same directory as the chm files. If you are just using help, there's no need for that. 

## Lazarus IDE help

Most help for the Lazarus IDE is kept on the Lazarus wiki. A drawback is that it is not available as an (regularly updated) archive, and with a free-form structure (as expected in a wiki), which is less suitable for automated transformation. 

There is a wikihelp tool in lazarus-ccr that should be able to download and relink the wiki for offline use (which then could be transformed into a meagre quality but working chm), but the Lazarus Wiki was closed for bulk downloads due to attacks in the past. 

Wikihelp: 

  * original home: <http://wiki.ullihome.de/index.php/WikiHelp-Download/de>
  * lazarus-ccr location : applications/wikihelp (requires synapse)



## See also

  * [Add Help to Your Application](<Add_Help_to_Your_Application.md> "Add Help to Your Application")
  * [htmlhelp compiler](<htmlhelp_compiler.md> "htmlhelp compiler")
  * [lhelp](<lhelp.md> "lhelp")

---

_Source: [https://wiki.freepascal.org/chmhelp](https://web.archive.org/web/20250115181639/https://wiki.freepascal.org/chmhelp)_
