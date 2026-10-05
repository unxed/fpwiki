# fpsvnsync

fpsvnsync is a tool I have written to mirror the lazarus svn repository to SourceForge. It has similar functionality as [svnsync](<http://svn.collab.net/repos/svn/trunk/notes/svnsync.txt>) that comes with the svn command line tools. But svnsync requires that you can change revision properties, which [SourceForge didn't allow](<http://sourceforge.net/tracker/?func=detail&aid=1655191&group_id=1&atid=350001>). 

fpsvnsync uses [SvnClasses](<SvnClasses.md> "SvnClasses"). 

### SVN

fpsnvsync is in the lazarus-ccr SourceForge repository. You can check out the source with the following command: 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/applications/fpsvnsync> fpsvnsync
    

The necessary svnpkg Lazarus package can be checked out by doing: 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/svn> svn
    

### Download

Before publishing I want to do a bit of code cleanup, it is very verbose now.

---

_Source: [https://wiki.freepascal.org/fpsvnsync](https://web.archive.org/web/20230323213739/https://wiki.freepascal.org/fpsvnsync)_
