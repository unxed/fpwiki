# Poweredby

│ **English (en)** │  **[français (fr)](</Poweredby/fr> "Poweredby/fr")** │  **[русский (ru)](<../ru/Poweredby.md> "Poweredby/ru")** │    
****  


## Contents

  * 1 TPoweredBy component
    * 1.1 by minesadorada@charcodelvalle.com
      * 1.1.1 Windows graphic
        * 1.1.1.1 Linux/macOS graphic
    * 1.2 Other logos and banners
    * 1.3 Summary
    * 1.4 Download
    * 1.5 Install
    * 1.6 Use
      * 1.6.1 Other uses
    * 1.7 License
    * 1.8 Platform
      * 1.8.1 Windows
      * 1.8.2 Linux
      * 1.8.3 macOS
    * 1.9 Tested
    * 1.10 Version
      * 1.10.1 Support
    * 1.11 See Also



## TPoweredBy component

### by minesadorada@charcodelvalle.com

#### Windows graphic

[![powered by graphic.png](https://wiki.freepascal.org/images/d/da/powered_by_graphic.png)](</File:powered_by_graphic.png>)

##### Linux/macOS graphic

[![linux powered by graphic.jpg](https://wiki.freepascal.org/images/7/7b/linux_powered_by_graphic.jpg)](</File:linux_powered_by_graphic.jpg>)

### Other logos and banners

  * [Logos and Banners](<Logos_and_Banners.md> "Logos and Banners")



* * *

### Summary

  * A visual component (installed on the 'Additional' tab) that fades in above shaped graphic image for 1 second (or in Linux/MacOS displays it for 1 second)
  * Drop into your form.create() event



### Download

Download from the lazarus CCR [here](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/>)

### Install

  1. Make a new folder 'poweredby'
  2. Unzip the archive into the folder
  3. In the Lazarus IDE choose 'Open' poweredby.lpk
  4. When asked 'Open as a project' answer 'yes'
  5. Click Compile
  6. Click Use/Install
  7. When asked 'would you like to compile Lazarus?'m answer 'yes'
  8. After Lazarus has restarted, check the 'Additional' component tab/palette for the new 'poweredby' component



### Use

  * Start a new project application
  * Drop a 'poweredby' component onto the form
  * Double-click the empty form to show the TForm1.Create method
  * Add poweredby1.showpoweredbyform
  * Run the application



That's it! 

#### Other uses

The PoweredBy component lends itself well to adding as a subcomponent to an existing custom component: 
    
    
    Uses uPoweredBy, Propedits, ..other units
    
    Type
    TMyComponent = Class(TComponent)
    private 
      fPoweredBy:TPoweredBy;
      ..other stuff
    public
      procedure ShowPoweredByLogo; // Call fPoweredBy.ShowPoweredByForm method in this proc.
      ..other stuff
    published
      property PoweredBy:TPoweredBy read fPoweredBy write fPoweredBy;
      ..other stuff
    end;
    
    procedure Register;
    RegisterPropertyEditor(TypeInfo(TPoweredBy),
        TMyComponent, 'PoweredBy', TClassPropertyEditor);
    
    Constructor TMyComponent.Create()
    // Use tPoweredBy as a subcomponent
    // Register a TClassPropertyEditor in order to display it correctly
      fPoweredBy := TPoweredBy.Create(Self);
      fPoweredBy.SetSubComponent(true);  // Tell the IDE to store the modified properties
      fPoweredBy.Name:='PoweredBy';
    

### License

LGPL license 

### Platform

#### Windows

  * PoweredBy will fade-in a shaped graphic



#### Linux

  * PoweredBy will display a square graphic 
    * This is due to the inability of GTK widgetset to deal with transparent shaped screens



#### macOS

  * PoweredBy displays a square graphic, similar to the Linux version.



### Tested

Windows 7 32/64-bit Laz v1.x fpc 2.6.x Linux 32-bit Laz v0.9.x fpc 2.2.x 

### Version

V1.0.1.2 

#### Support

[Email the author](<mailto:minesadorada@charcodelvalle.com>) with any queries 

### See Also

  * [Logos_and_Banners](<Logos_and_Banners.md> "Logos and Banners")
  * [Standalone Scrolling Text component](<ScrollText.md> "ScrollText")
  * [Components and code examples](<Components_and_Code_examples.md> "Components and Code examples")

---

_Source: [https://wiki.freepascal.org/Poweredby](https://web.archive.org/web/20250122120627/https://wiki.freepascal.org/Poweredby)_
