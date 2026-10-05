# Site Feedback/old

## Contents

  * 1 Old requests
    * 1.1 Cannot delete image files
    * 1.2 Cannot update image files
    * 1.3 Categories not updating again
    * 1.4 Portals are hidden gems which should be obvious
    * 1.5 Main Page: Specialized Search Engines
    * 1.6 Categories not updating
  * 2 Even older requests
  * 3 IRC ?



## Old requests

### Cannot delete image files
    
    
    [f14f8e66fba701d84a1e6980] /index.php?title=File:foto_tiziano.jpg&action=delete Wikimedia\Rdbms\DBUnexpectedError from line 3628 of  
    /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php: Invalid atomic section ended (got LocalFileDeleteBatch::doDBInserts but 
    expected LocalFile::lockingTransaction).
    Backtrace:
    #0 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(2960): Wikimedia\Rdbms\Database->cancelAtomic(string)
    #1 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(2880): Wikimedia\Rdbms\Database->nonNativeInsertSelect(string, array, 
       array, array, string, array, array, array)
    #2 /srv/www/lazaruswiki/includes/filerepo/file/LocalFile.php(2589): Wikimedia\Rdbms\Database->insertSelect(string, array, array, array,  
       string, array, array, array)
    #3 /srv/www/lazaruswiki/includes/filerepo/file/LocalFile.php(2716): LocalFileDeleteBatch->doDBInserts()
    #4 /srv/www/lazaruswiki/includes/filerepo/file/LocalFile.php(1982): LocalFileDeleteBatch->execute()
    #5 /srv/www/lazaruswiki/includes/FileDeleteForm.php(201): LocalFile->delete(string, boolean, User)
    #6 /srv/www/lazaruswiki/includes/FileDeleteForm.php(118): FileDeleteForm::doDelete(Title, LocalFile, string, string, boolean, User)
    #7 /srv/www/lazaruswiki/includes/page/ImagePage.php(992): FileDeleteForm->execute()
    #8 /srv/www/lazaruswiki/includes/actions/DeleteAction.php(46): ImagePage->delete()
    #9 /srv/www/lazaruswiki/includes/MediaWiki.php(500): DeleteAction->show()
    #10 /srv/www/lazaruswiki/includes/MediaWiki.php(294): MediaWiki->performAction(ImagePage, Title)
    #11 /srv/www/lazaruswiki/includes/MediaWiki.php(861): MediaWiki->performRequest()
    #12 /srv/www/lazaruswiki/includes/MediaWiki.php(524): MediaWiki->main()
    #13 /srv/www/lazaruswiki/index.php(42): MediaWiki->run()
    #14 {main}
    

    

    See Bugtracker [Issue #36784](<https://bugs.freepascal.org/view.php?id=36784>)

### Cannot update image files

[5c61702508cc7689ac7e4a83] /Special:Upload Wikimedia\Rdbms\DBQueryError from line 1457 of /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php: A database query error has occurred. Did you forget to run your application's database schema updater after upgrading? Query: RELEASE SAVEPOINT `wikimedia_rdbms_atomic1` Function: LocalFile::recordUpload2 Error: 1305 SAVEPOINT wikimedia_rdbms_atomic1 does not exist (localhost) Backtrace: 

  1. 0 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(1427): Wikimedia\Rdbms\Database->makeQueryException(string, integer, string, string)
  2. 1 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(1200): Wikimedia\Rdbms\Database->reportQueryError(string, integer, string, string, boolean)
  3. 2 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(3498): Wikimedia\Rdbms\Database->query(string, string)
  4. 3 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(3584): Wikimedia\Rdbms\Database->doReleaseSavepoint(string, string)
  5. 4 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(2953): Wikimedia\Rdbms\Database->endAtomic(string)
  6. 5 /srv/www/lazaruswiki/includes/libs/rdbms/database/Database.php(2880): Wikimedia\Rdbms\Database->nonNativeInsertSelect(string, array, array, array, string, array, array, array)
  7. 6 /srv/www/lazaruswiki/includes/filerepo/file/LocalFile.php(1606): Wikimedia\Rdbms\Database->insertSelect(string, array, array, array, string, array, array, array)
  8. 7 /srv/www/lazaruswiki/includes/filerepo/file/LocalFile.php(1364): LocalFile->recordUpload2(string, string, boolean, array, string, User, array)
  9. 8 /srv/www/lazaruswiki/includes/upload/UploadBase.php(868): LocalFile->upload(string, string, boolean, integer, array, boolean, User, array)
  10. 9 /srv/www/lazaruswiki/includes/specials/SpecialUpload.php(567): UploadBase->performUpload(string, boolean, boolean, User, array)
  11. 10 /srv/www/lazaruswiki/includes/specials/SpecialUpload.php(207): SpecialUpload->processUpload()
  12. 11 /srv/www/lazaruswiki/includes/specialpage/SpecialPage.php(565): SpecialUpload->execute(NULL)
  13. 12 /srv/www/lazaruswiki/includes/specialpage/SpecialPageFactory.php(568): SpecialPage->run(NULL)
  14. 13 /srv/www/lazaruswiki/includes/MediaWiki.php(288): SpecialPageFactory::executePath(Title, RequestContext)
  15. 14 /srv/www/lazaruswiki/includes/MediaWiki.php(861): MediaWiki->performRequest()
  16. 15 /srv/www/lazaruswiki/includes/MediaWiki.php(524): MediaWiki->main()
  17. 16 /srv/www/lazaruswiki/index.php(42): MediaWiki->run()
  18. 17 {main}



    

    See Bugtracker [Issue #36784](<https://bugs.freepascal.org/view.php?id=36784>)

### Categories not updating again

If someone could run the refreshLinks.php script again to update the categories, that would be appreciated (see old request below for solution). Thanks! 

### Portals are hidden gems which should be obvious

The Portal content is currently difficult to discover. See [Sand Box](<../Sand_Box.md> "Sand Box") for an example by [Serbod](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=52561>)of portal buttons for the main page, with some images from gallery. It would be great if this, or something like it, could be added to the main page. 

    

    The main page has been updated. Portals are now included. -- [Martin](</User:Martin> "User:Martin") 19 March 2020

### Main Page: Specialized Search Engines

The whole paragraph on the Wiki Main page "Specialized Search Engines" is now dead (dead links, missing content). Someone with higher privileges than me needs to attend to its removal. 

    Thanks for fixing Martin. [Trev](</User:Trev> "User:Trev") ([talk](</index.php?title=User_talk:Trev&action=edit&redlink=1> "User talk:Trev \(page does not exist\)")) 12:09, 17 January 2020 (CET)

### Categories not updating

There are a number of pages in the Wiki that are categorised but do not show up in any of the categories. For example: 
    
    
    1. [https://wiki.freepascal.org/Cocoa_Internals/Forms](<../Cocoa_Internals/Forms.md>)
    2. [https://wiki.freepascal.org/Add_an_Apple_Help_Book_to_your_macOS_app](<../Add_an_Apple_Help_Book_to_your_macOS_app.md>)
    3. <https://wiki.lazarus.freepascal.org/FreeBSD_Programming_Tips>
    

The above issue is not a "caching" issue. The pages show up in Special Pages Uncategorised despite the categories. 

    Fixed
    [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas"))

Number 3 above is fixed - numbers 1 and 2 are still not found (I purged the categories to ensure it was not caching). [Trev](</User:Trev> "User:Trev") ([talk](</index.php?title=User_talk:Trev&action=edit&redlink=1> "User talk:Trev \(page does not exist\)")) 01:55, 21 July 2019 (CEST) 

    The nightly job to update the categories was failing. I fixed that. I have no idea what else I could do.
    [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas"))

Checking with my mate Google shows this issue has existed for years with MediaWiki :( The solution which seems to work, other than rebuilding the entire thing, seems to be running the refreshLinks.php script which I assume means having command line access to the system. The cause seems to be adding categories AFTER having created the page - category not updated. Whereas, adding the category AT THE TIME of page creation - category is updated. [Trev](</User:Trev> "User:Trev") ([talk](</index.php?title=User_talk:Trev&action=edit&redlink=1> "User talk:Trev \(page does not exist\)")) 11:50, 21 July 2019 (CEST) 

    I have shell access to the server, but not right now. It will have to wait until next week Sunday.
    [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas")) 

    Done
    [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas")) 

    That fixed it! Thanks Jonas.
    [Trev](</User:Trev> "User:Trev") ([talk](</index.php?title=User_talk:Trev&action=edit&redlink=1> "User talk:Trev \(page does not exist\)")) 01:57, 14 August 2019 (CEST)

## Even older requests

  * [https://wiki.freepascal.org/Main_Page](<../Main_Page.md>)



"If you have any problems, please notify the site administrator" <<\-- the link to poor Tom is way out of date - he says he has not been the Admin since the days when the Wiki was on Sourceforge. 

    Done
    [Jonas](</User:Jonas> "User:Jonas") ([talk](</User_talk:Jonas> "User talk:Jonas"))

  * Add some kind of chapters/tree/index on the main page, searching for various documents is harder without it.
  * You could add a few links on the main Lazarus website to various sections in the Lazarus-CCR(**C** omponent and **C** ode **R** epository) like **Components** , **Examples** , **Online Docs** , **Tutorials**. 
    * The **Components** section should contain various components that are not used for "normal" programming or have similar behaviour to existing IDE components, maybe also platform specific components that can not be implemented in a CrossPlaform fashion.
    * The **Examples** section should contain as many useful examples as possible, many people report that examples made them appreciate the "power" of other RAD tools like Delphi/Kylix, C++Builder, JBuilder and even Visual Basic, so i think examples will also help the Lazarus project too in becomeing the most widely used tool by OpenSource and Commercial software developers.
    * The **Online Docs** section should contain FCL, LCL, IDE, Compiler, Classes organisation and description, various popular API calls, even some OS specific calls.
    * The **Tutorials** section is one of the most important parts for productivity, it should cover various aspects of programming ranging from Console, GUI, Database, Component Design, IDE/Compiler Enhancing to Hardware I/O and even Multimedia, 3D Graphics, Audio, Game Design.


  * The IDE could also have some links in the **Help** menu.


  * The documentation of **RTL** , **FCL** and **LCL** could contain hints on dependencies of functions which will only work after other specific functions have been called (for example: "InitKeyboard" has to be called before "PollKeyEvent" can work as suggested). Otherwise it could be difficult to track errors down using the documentation provided.



    The lazarus documentation is located on the lazarus subversion on the directory lazarus/docs. The RTL and FCL documentation is located at <http://svn.freepascal.org/svn/fpcdocs/trunk/>. It is composed of XML files. Please send a patch improving the documentation in the way you suggested and it will be applied.

## IRC ?

Nowhere on the main site is something written about contact via **IRC** or something. 

    It is a bit hidden, but is there: [http://www.lazarus.freepascal.org/modules.php?op=modload&name=News&file=article&sid=47](<http://www.lazarus.freepascal.org/modules.php?op=modload&name=News&file=article&sid=47>)

    The link is now dead. --[LV](</index.php?title=User:LV&action=edit&redlink=1> "User:LV \(page does not exist\)") 16:29, 28 January 2016 (CET) 

    The main page has been updated. Link to IRC (via Forum) is now included. -- [Martin](</User:Martin> "User:Martin") 19 March 2020

  * I hope you don't mind that I linked from the WikiIndex ( <http://wikiindex.org/Free_Pascal_wiki> ) to this wiki. --[DavidCary](</index.php?title=User:DavidCary&action=edit&redlink=1> "User:DavidCary \(page does not exist\)") 06:25, 2 July 2007 (CEST)


  * [ Site about IRC and links to Webchat](<../FPC_IRC_channel.md> "FPC IRC channel") contains links to the webchat for both lazarus and freepascal

---

_Source: [https://wiki.freepascal.org/Site_Feedback/old](https://web.archive.org/web/20241104201655/https://wiki.freepascal.org/Site_Feedback/old)_
