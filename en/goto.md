# Goto

│ **English (en)** │

` goto` is an unconditional jump to a previously declared [`label`](<Label.md> "Label") (either before or after the `goto` command). It is a [reserved word](<Reserved_words.md> "Reserved words"). 

Usage of `goto` in high-level programming languages such as Pascal is highly discredited, since control structures of all sorts are available. 

The last situation a `goto` is agreed with bad grace to be a significant system error, where a “graceful exit” is better than causing a system breakdown. 

As an example, here an excerpt from FPC’s code base [`rtl/inc/extres.inc`](<https://gitlab.com/freepascal.org/fpc/source/blob/release_3_2_0/rtl/inc/extres.inc#L324-376>)]: 
    
    
    procedure InitResources;
    
    
    
    label ExitErrMem, ExitErrFile, ExitNoErr;
    begin
    
    
    
      ResHeader:=GetMem(sizeof(TExtHeader));
      if ResHeader=nil then goto ExitErrFile;
    
    
    
      goto ExitNoErr;
    
      ExitErrMem:
        FreeMem(ResHeader);
        ResHeader:=nil;
      ExitErrFile:
        {$I-}
        Close(fd);
        {$I+}
      ExitNoErr:
    end;
    

According to the value of [`returnNilIfGrowHeapFails`](<https://www.freepascal.org/docs-html/rtl/system/returnnilifgrowheapfails.html>) [`getMem`](<https://www.freepascal.org/docs-html/rtl/system/getmem.html>) possibly may return [`nil`](<Nil.md> "Nil"). Instead of placing _everything_ in a “success”-branch, a couple `goto` instructions were chosen. 

## See also

  * [`exit`](<Exit.md> "Exit")
  * [`{$goto}` compiler directive](<sGoto.md> "sGoto")
  * [`system.longJmp`](<https://www.freepascal.org/docs-html/rtl/system/longjmp.html>)

---

_Source: [https://wiki.freepascal.org/goto](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/goto)_
