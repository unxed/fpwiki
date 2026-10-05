# germesorders

It's a mobile PocketPC database. Requires SQLite3 and RxLib. 

The purpose of this database application is very simple. It's just a working tool for people working in trade branch. Synchonizing to the main database, a man gets todays available goods and their remains in the store. Each goods indentity has a group and subgroup. 

After that he travels to several regular customers and stores their invoices in this small db. That's it. Again synchonization and his invoices go the main database to become documents. 

Current version is in Russian language and optimized for WinCE interface using PocketPC stylus. Tested with latest [RxLib](<http://wiki.lazarus.freepascal.org/RXfpc>) (svn revision 638). 

## Install procedure

Remark. If you build for WinCE and ARM processor you must make sure you have ppcarm, arm binutils, lazarus arm units built and installed properly. 

  1. Create a directory for project
         
         $ mkdir germesorders
         $ cd germesorders
         

  2. Get GermesOrders sources
         
         $ svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/examples/germesorders/
         

  3. Get RxLib svn sources
         
         $ svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/rx/
         

  4. Build executable file
         
         $ cd germesorders/scripts
         $ ./ppc-build
         




## Screenshots

  * Native build
  * [![germes1.png](https://wiki.freepascal.org/images/3/3b/germes1.png)](</File:germes1.png>)

  * [![germes2.png](https://wiki.freepascal.org/images/b/b2/germes2.png)](</File:germes2.png>)

  * [![germes3.png](https://wiki.freepascal.org/images/6/67/germes3.png)](</File:germes3.png>)



  * WinCE build
  * [![germes wince1.png](https://wiki.freepascal.org/images/0/08/germes_wince1.png)](</File:germes_wince1.png>)

  * [![germes wince2.png](https://wiki.freepascal.org/images/2/25/germes_wince2.png)](</File:germes_wince2.png>)

  * [![germes wince3.png](https://wiki.freepascal.org/images/b/bd/germes_wince3.png)](</File:germes_wince3.png>)

---

_Source: [https://wiki.freepascal.org/germesorders](https://web.archive.org/web/20240907021836/https://wiki.freepascal.org/germesorders)_
