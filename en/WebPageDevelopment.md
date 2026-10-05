# WebPageDevelopment

The main web page of Lazarus project can be found here : 
    
    
    <http://www.lazarus-ide.org/>
    

It is done with HTML and PHP, and connects to SMF (Simple Machines) forum SW. 

The sources are in a subversion repository : 
    
    
    <https://svn.freepascal.org/svn/lazaruswebsite/>
    

which is almost the same address that Lazarus sources have, only "lazarus" is replaced with "lazaruswebsite". It is readable by anyone. 

Testing the page locally requires normal web server + PHP installation. SMF (Simple Machines) does not need to be installed though. The PHP code is clever enough to insert "dummy" forum data when real data is not available. 

The page can be improved by anyone by sending patches through bug tracker, the same way that patches are sent for pascal sources. However, if the changes are clearly a matter of taste (like colors) then please discuss the changes first in mailing list or forum. 

When a patch is applied to SVN, it is updated automatically to a test server : 
    
    
    <http://test.lazarus-ide.org/>
    

After it is checked for visual appearance and for potential security issues, it will be updated manually to the "production" server. 

However the test server is not very useful for a contributor because the changes must be tested locally anyway.

---

_Source: [https://wiki.freepascal.org/WebPageDevelopment](https://web.archive.org/web/20250219054337/https://wiki.freepascal.org/WebPageDevelopment)_
