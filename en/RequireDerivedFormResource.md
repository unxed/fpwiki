# RequireDerivedFormResource

**RequireDerivedFormResource** is a switch to toggle the requirement for a [TForm](<TForm.md> "TForm")'s resource 

Each form created by the IDE should have a resource, so if resource is not found there is something wrong with either a resource or the unit which contains the faulty form. Therefore the application should inform the developer that the form can't function correctly without the resource. This change is also Delphi compatible and compatible with [TFrame](<TFrame.md> "TFrame") and [TDataModule](</index.php?title=TDataModule&action=edit&redlink=1> "TDataModule \(page does not exist\)") components. 

If you need a resourceless form you have 3 options: 

  * Create a TForm class (not a descendant)
  * Construct your form using the `CreateNew()` [constructor](<Constructor.md> "Constructor").
  * Disable the exception by setting the global variable RequireDerivedFormResource to False.



RequireDerivedFormResource is automatically inserted on creating a new form in the [IDE](<IDE.md> "IDE"). 
    
    
    begin
      RequireDerivedFormResource:=True;
      Application.Scaled:=True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.

---

_Source: [https://wiki.freepascal.org/RequireDerivedFormResource](https://web.archive.org/web/20221002125308/https://wiki.freepascal.org/RequireDerivedFormResource)_
