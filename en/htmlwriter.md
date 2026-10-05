# htmlwriter

Implements a verified HTML producer. 

  * THTMLwriter: This is a class which allows to write certified correct HTML. It works using the DOM for HTML. It also has forms support.



Writing HTML is done as follows: 
    
    
     StartBold;
     Write('This text is bold');
     EndBold;
    
    
    
    or
    
    
    
     Bold('This text is bold');
    
    

But the following is also possible 
    
    
     Bold(Center('Bold centered text'));
    

Open tags will be closed automatically. [wtagsintf.inc](</index.php?title=wtagsintf.inc&action=edit&redlink=1> "wtagsintf.inc \(page does not exist\)") contains all possible tags. 

Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview.

---

_Source: [https://wiki.freepascal.org/htmlwriter](https://web.archive.org/web/20230324033016/https://wiki.freepascal.org/htmlwriter)_
