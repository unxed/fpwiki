# FPCupDeLuxe install on Linux cheatsheet

│ **English (en)** │ 

## Contents

  * 1 Likely problems to occur



long story short: 

  * install Lazarus via **apt** or some other package-manager
  * download, unzip and compile [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") \- or use fpcupdeluxe-binary and omit steps 1 and 3
  * purge the above Lazarus
  * now use [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") to install an isolated Laz: choose **trunk** , **x86_64** , **linux** , **trunk** and click the **green thumbs-up button**.
  * **fpc** , the actual Pascal compiler, will not be in the searchpath, hence **make** will require extra parameters; a mere **make all** won't work now.
  * Use the provided launcher in ~ or home/YOU/Desktop (.desktop file) to run "**startlazarus** " employing a **\--[pcp](<pcp.md> "pcp")** directive to keep multiple Lazaruses apart.
  * This method may or may not succeed. It can be made to work though.



## Likely problems to occur

Since the Mint / Ubuntu Laz package and f.deluxe do not harmonize, you will very likely end up with 2 compilers in $PATH if you mix **f-deluxe** , **make** from sources and **apt** methods. Do **ls** to verify in case that nothing ever compiles due to "**unit not found** " errors : 
    
    
     ls -l /usr/bin/fpc-3.0.0       ##   mv /xx 
     ls -l /usr/local/bin/fpc       ##   <---  which fpc   -- must remain !
    

Move /usr/bin/fpc* out of the path e.g. 
    
    
    sudo mv -f /usr/bin/fpc* /xxbroken
    

After IDE restart, everything compiles nicely. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** This condition can also be found via clicking project options button, then Test [Test-button](<IDE_Window__Project_Options.md> "IDE Window: Project Options") to find problematic path settings.

See also: 

  * [Unit not found - How to find units](<Unit_not_found_-_How_to_find_units.md> "Unit not found - How to find units")

---

_Source: [https://wiki.freepascal.org/FPCupDeLuxe_install_on_Linux_cheatsheet](https://web.archive.org/web/20221209024717/https://wiki.freepascal.org/FPCupDeLuxe_install_on_Linux_cheatsheet)_
