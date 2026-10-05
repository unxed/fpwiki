# RichMemo/Samples

## Contents

  * 1 Tabs
    * 1.1 Basics
    * 1.2 Tab Offsets
  * 2 See Also



## Tabs

Tabs are used to organize the text in table-like layout. This would not be a real layout table, but will make the text (if it's short enough) to look table alike. 

### Basics

In order for tabs to work the text should be split tab character (#9) 

This is how the text would look like if no tab separation is used: 

[![richmemo basic1.png](https://wiki.freepascal.org/images/e/e1/richmemo_basic1.png)](</File:richmemo_basic1.png>)
    
    
      RichMemo1.Lines.Add('hello world'); 
      RichMemo1.Lines.Add('this sample');
    

  
If the words are split by Tabs, then the control tries to find the next proper tab offset in order: 

[![richmemo basic2.png](https://wiki.freepascal.org/images/3/30/richmemo_basic2.png)](</File:richmemo_basic2.png>)
    
    
      RichMemo1.Lines.Add('hello'#9'world'); 
      RichMemo1.Lines.Add('this'+#9+'sample');
    

### Tab Offsets

The length of tab offsets can be controlled using [GetParaTabs](<../RichMemo.md> "RichMemo") and [SetParaTabs](<../RichMemo.md> "RichMemo") methods. 
    
    
    var
      st : TTabStopList;
      i  : integer;
    begin
     
      RichMemo1.GetParaTabs(0, st); // getting the current tabs
      st.Count:=8;
      SetLength(st.Tabs, st.Count);
      for i:=0 to length(st.Tabs)-1 do
        st.Tabs[i].Offset:=i*72;
      RichMemo1.SetParaTabs(-1,-1,st); // assigning tabs stops
     
      // assigning text with tabs (#9)
      RichMemo1.Lines.Add('hello'#9'world'#9'to'#9'columns');
      RichMemo1.Lines.Add('does'#9'it'#9'look'#9'good'#9'?');
      RichMemo1.Lines.Add('tab'+#9+'is'+#9+'character'+#9+'9');
    end;
    

[![richmemo tabs.png](https://wiki.freepascal.org/images/b/b9/richmemo_tabs.png)](</File:richmemo_tabs.png>)

## See Also

  * [RichMemo](<../RichMemo.md> "RichMemo")

---

_Source: [https://wiki.freepascal.org/RichMemo/Samples](https://web.archive.org/web/20210921021228/https://wiki.freepascal.org/RichMemo/Samples)_
