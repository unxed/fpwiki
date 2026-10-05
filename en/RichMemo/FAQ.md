# RichMemo/FAQ

Forum Asked Questions 

### Don't show horizontal bar

    Q: I started to work with richmemo but don't show me horizontal bar only vertical bar even i typing or loadfile, in properties i select SSboth on srcollbars.
    A: **Horizontal scroll bar** might not show up if **WordWrap** is enabled.

### Add text with font

    Q: how i can add text or line richmemo with font or textstyle?
    A: The fastest way is to use **RichUtilsUtils** which comes with RichMemo and call **InsertFontText** function
    
    
    uses ... RichUtilsUtils ..
    
    ..
    
    var
      prms : TFontParams;
    begin
      prms := GetFontParams(RichMemo1.Font);
      prms.Color := clBlue;
      prms.Style := [fsBold];
      // this adds the blue and bolded Hello World at the end of the richmemo
      // it's possible to pass the 4th parameter to the function to specify 
      // where the text should be added. By default - at the end.
      InsertFontText(RichMemo1,'hello world', prms);
    end;
    

    RichMemo's base API has the action of inserting the text and applying the style separated.

## See Also

  * [RichMemo](<../RichMemo.md> "RichMemo")

---

_Source: [https://wiki.freepascal.org/RichMemo/FAQ](https://web.archive.org/web/20230207173311/https://wiki.freepascal.org/RichMemo/FAQ)_
