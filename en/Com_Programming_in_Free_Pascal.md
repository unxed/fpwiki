# Com Programming in Free Pascal

## Contents

  * 1 Overview
  * 2 Bug reports about COM
  * 3 Urls
  * 4 TODO



## Overview

COM is a Windows only technology used for communicating with external code. COM is e.g. used in [ActiveX](<LazActiveX.md> "LazActiveX"). 

## Bug reports about COM

See [Open Mantis items with COM tag ](<http://bugs.freepascal.org/search.php?project_id=6&status_id%5B%5D=10&status_id%5B%5D=20&status_id%5B%5D=30&status_id%5B%5D=40&status_id%5B%5D=50&sticky_issues=on&sortby=last_updated&dir=DESC&hide_status_id=-2&tag_string=com>). 

Some of these also have nice demos attached. See also closed bug [Issue #0014204](<https://bugs.freepascal.org/view.php?id=0014204>). 

## Urls

  1. <http://delphi.about.com/library/weekly/aa121404b.htm>
  2. <http://www.codeproject.com/KB/atl/udtdemo.aspx>
  3. <http://edndoc.esri.com/arcobjects/9.1/ExtendingArcObjects/Ch02/TypeLibrariesAndIDL.htm>
  4. <http://delphi.about.com/library/weekly/aa121404a.htm>
  5. <http://www.codeproject.com/KB/atl/udtdemo.aspx>
  6. <http://www.codeproject.com/KB/atl/com_atl.aspx>
  7. <http://www.codeproject.com/KB/atl/RegistryMap.aspx>
  8. <http://www.techvanguards.com/stepbystep/comdelphi/server.asp>
  9. <http://www.techvanguards.com/com/tutorials/tips.asp>
  10. <http://docs.embarcadero.com/products/rad_studio/radstudio2007/RS2007_helpupdates/HUpdate4/EN/html/delphivclwin32/ComObj.html>
  11. <http://docs.embarcadero.com/products/rad_studio/radstudio2007/RS2007_helpupdates/HUpdate4/EN/html/delphivclwin32/ComServ.html>



## TODO

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This section has not been updated in a long time. Please verify if this is still correct

  * reference counting is not working (DllCanUnloadClass returns 1)
  * load/register typelib
  * register/unregister (incomplete implementation from visual studio RGS sample file)
  * integrate WIDL.exe (wine version of MIDL)
  * finish TTypedComObject
  * create TAutoObject
  * implement tlbimp.exe
  * port to linux as NPAPI wrapper (just partially kidding ;) as base you can us my updated NPAPI scripting code (<https://www.mozdev.org/bugs/show_bug.cgi?id=8708>) [^] - already working with FPC (similar entry point functions as in COM) :)


  * integrate MIDL.exe (wine old version of MIDL has an incorrect output format :( )
  * try building latest WIDL -> report bugs to wine
  * create TAutoObject
  * finish tlbimp.exe (bug 0014802)
  * TEST, TEST, TEST :) serious

---

_Source: [https://wiki.freepascal.org/Com_Programming_in_Free_Pascal](https://web.archive.org/web/20241212095437/https://wiki.freepascal.org/Com_Programming_in_Free_Pascal)_
