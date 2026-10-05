# Register (package)

A [Lazarus package](<Lazarus_Packages.md> "Lazarus Packages") that is intended to be installed needs a **Register** procedure which declares 

  * all the package components and corresponding files
  * the tab on which the [components](</index.php?title=component&action=edit&redlink=1> "component \(page does not exist\)") are to be located.



Related procedures/functions 

  * [RegisterUnit](</index.php?title=RegisterUnit&action=edit&redlink=1> "RegisterUnit \(page does not exist\)")
  * [RegisterPackage](</index.php?title=RegisterPackage&action=edit&redlink=1> "RegisterPackage \(page does not exist\)")
  * [RegisterComponents](</index.php?title=RegisterComponents&action=edit&redlink=1> "RegisterComponents \(page does not exist\)")
  * [RegisterPropertyEditor](</index.php?title=RegisterPropertyEditor&action=edit&redlink=1> "RegisterPropertyEditor \(page does not exist\)")
  * [RegisterIDEMenuCommand](</index.php?title=RegisterIDEMenuCommand&action=edit&redlink=1> "RegisterIDEMenuCommand \(page does not exist\)")


    
    
    unit RegisterMyPackage;
    
    interface
    
      uses 
        LazarusPackageIntf, MyPackage1, MyPackage2, MyOtherPackage;
    
    implementation
    
    procedure Register;
    begin
      RegisterUnit( 'MyTab', @MyPackage1.Register );
      RegisterUnit( 'MyTab', @MyPackage2.Register );
      RegisterComponents( 'OtherTab', [TOtherComponent1,TOtherComponent2] );
    end;
    
    initialization
      {$I myresourcefile.lrs}
    
      RegisterPackage( 'MyTab', @Register );
    end.

---

_Source: [https://wiki.freepascal.org/Register_(package)](https://web.archive.org/web/20250219183807/https://wiki.freepascal.org/Register_(package))_
