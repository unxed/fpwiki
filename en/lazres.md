# lazres

│ **English (en)** │

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Since Lazarus v1.4.0, Lazarus has switched to .res format instead of .lrs; see [Lazarus_1.4.0_release_notes](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes").   
To create .res resource files you can use either **lazres** or **winres**

**lazres** is a [lazarus resource](<Lazarus_Resources.md> "Lazarus Resources") tool to create and convert `.rc`, `.lrs` and `.res` files. 

Lazres can be found as `<root>/lazarus/tools/lazres.lpi` and might need to be compiled. In Ubuntu installations `lazres` is precompiled and available in `/usr/bin`
    
    
    Usage: lazres resourcefilename filename1[=resname1] [filename2[=resname2] ... filenameN=resname[N]]
           lazres resourcefilename @filelist
    

To create a resource file MYRES.RES with two .bmp graphics files use: 
    
    
    lazres MYRES.RES MyImage1.bmp=IMG1 MyImage2.bmp=IMG2
    

A resource file can be of type: 

`.RC` | resource description file | plaintext |   
---|---|---|---  
`.LRS` | lazarus (pascal) resource | plaintext |   
`.RES` | compiled resource | binary | default   
  
## Contents

  * 1 Using resources
    * 1.1 .lrs
    * 1.2 .res
  * 2 See also



## Using resources

### .lrs

Include .lrs file in the [initialization](<Initialization.md> "Initialization") section 
    
    
    initialization
      // .LRS files are plain-text pascal statements and need unit LResources to be included in the Uses clause
     {$I MyResources.lrs}
    end.
    

### .res

Load .res file in the [implementation](<Implementation.md> "Implementation") section 
    
    
    implementation
      // .RES files are binary resources and can be loaded
      {$R MyResources.res}
    
    var 
      img: TImage; 
    begin
      img.Picture.Bitmap.LoadFromResourceName( hInstance, 'IMG1' );
    end;
    

## See also

  * [FCL resource units](<fcl-res.md> "fcl-res")
  * [Description of using FPC and Lazarus resources](<Lazarus_Resources.md> "Lazarus Resources")
  * [IDE Project Options > Resources](<IDE_Window__Project_Options.md> "IDE Window: Project Options")

---

_Source: [https://wiki.freepascal.org/lazres](https://web.archive.org/web/20220119021849/https://wiki.freepascal.org/lazres)_
