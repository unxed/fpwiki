# fcl-pdf

## Contents

  * 1 About
  * 2 Examples
    * 2.1 Official test project
    * 2.2 Writing few lines, without embedded fonts
    * 2.3 Enumerate available fonts
    * 2.4 Render picture to PDF
  * 3 PDFs and Fonts
    * 3.1 gTTFontCache
    * 3.2 Flowing Text into a PDF
  * 4 See also



## About

The fcl-pdf package contains a [PDF](<PDF.md> "PDF") generating unit **fppdf** that does not depend on any external libraries. 

The PDF generator has the following features: 

  * Support for basic shapes.
  * Support for basic line styles.
  * Dictionary support.
  * Multi-page PDF.
  * Image support.
  * TTF Font support.
  * Font embedding.
  * Unicode font support.
  * Stream Compression.
  * Image embedding.
  * Several paper types.
  * Portrait/Landscape.
  * Support for multiple units.
  * Rotation matrix system.
  * PDF creator information.
  * Output validates by several PDF validators.



Contains units: 

  * fppdf
  * fpparsettf
  * fpttf
  * fpttfencodings



## Examples

### Official test project

Take a look at the "fcl-pdf/examples/testfppdf.lpr" project included with FPC's source code. It was purposely designed to help explain how to use the fcl-pdf package. Each page of that demo is defined in a separate method to help remove clutter, and explain the usage and functionality in smaller chucks of code. 

### Writing few lines, without embedded fonts

Example by forum member **Moritz** , fixed by member **paweld**. Code disables embedding of fonts into PDF. 
    
    
    procedure TMainForm.TestButtonClick(Sender: TObject);
    var
      FontID, FontBoldID: Integer;
      Document: TPDFDocument;
      Section: TPDFSection;
      Page: TPDFPage;
    begin
      Document := TPDFDocument.Create(nil);
      Document.FontDirectory := 'C:\Windows\Fonts';
      Document.Options := Document.Options + [poPageOriginAtTop, poNoEmbeddedFonts];
      Document.StartDocument;
      FontID := Document.AddFont('arial.ttf', 'Arial');
      FontBoldID := Document.AddFont('arialbd.ttf', 'Arial Bold');
     
      Section := Document.Sections.AddSection;
    
      Page := Document.Pages.AddPage;
      Section.AddPage(Page);
     
      Page.SetFont(FontID, 11);
      Page.WriteText(20, 20, 'This is normal text');
     
      Page.SetFont(FontBoldID, 11);
      Page.WriteText(20, 30, 'This is bold text');
     
      Document.SaveToFile('output.pdf');
    end;
    

### Enumerate available fonts

Example by forum member **paweld**. 
    
    
    uses
      fppdf, fpttf;
     
    procedure TForm1.FormCreate(Sender: TObject);
    var
      g: TFPFontCacheList;
      i: Integer;
    begin
      g := TFPFontCacheList.Create;
      g.SearchPath.Add('C:\Windows\Fonts');
      g.BuildFontCache;
      for i := 0 to g.Count - 1 do
        Memo1.Lines.Add(Format('File name: %s > Font name: %s > Family: %s', 
          [g.Items[i].FileName, g.Items[i].HumanFriendlyName, g.Items[i].FamilyName]));
      g.Free;
    end;
    

### Render picture to PDF

Example by forum member **CynicRus** changed by **AlexTP**. Note: on Ubuntu 20.04, default PDF viewer cannot show the picture correctly, it crops some top part of the picture. 
    
    
    uses
      fppdf;
    
    procedure WritePictureToPDF(const APictureFilename, APdfFilename: string);
    var
      PDF: TPDFDocument;
      Page: TPDFPage;
      Section: TPDFSection;
      Paper: TPDFPaper;
      Index: Integer;
      W, H: Integer;
    begin
      PDF := TPDFDocument.Create(nil);
      try
        PDF.StartDocument;
        Section := PDF.Sections.AddSection;
    
        Page := PDF.Pages.AddPage;
        Page.Orientation := ppoLandscape;
        Page.UnitOfMeasure := uomPixels;
    
        Section.AddPage(Page);
    
        Index := PDF.Images.AddFromFile(APictureFilename);
        W := PDF.Images[Index].Width;
        H := PDF.Images[Index].Height;
        paper.W := W;
        paper.H := H;
        Page.Paper := Paper;
        Page.PaperType := ptCustom;
        Page.DrawImageRawSize(0, 0, W, H, Index);
    
        PDF.SaveToFile(APdfFilename);
      finally
        PDF.Free;
      end;
    end;
    

## PDFs and Fonts

**A draft, a work in progress** so criticize, hack away, as you see fit ! dbannon. 

A practical system to produce PDFs will have to cope with text and text means fonts. While the PDF engine is in the fppdf unit, including fpttf will give you access to the gTTFontCache, a tool that loads data about the fonts you will be using. The Cache will tell you the name and path to the actual font file and will give you dimensions of the specific text you are putting into a PDF, essential information as you adjust line spacing and width. 

The PDF and Font Cache has had substantial developments, especially for Windows, after the release of of FPC3.2.2 so, probably a good idea to consider FPC-fixes as of mid 2024. But even then, there are some limitations, chief being that it cannot handle the newer font file format, 'collections' typically with an extension of .ttc (True Type Collection) or .otc (Open Type Collection). The cache, when scanning for font files, will ignore .ttc (and, perhaps .otc) but if you offer the PDF engine a .ttc file you will, later, cause an exception. Better to take the Cache's advice, skip .ttc. 

There are a set of "standard PDF" fonts, Helvetica, Courier, Times-Roman that are expected to be available in any PDF Viewer and are therefore not needed to be 'embedded' in the document. However, these fonts have problems with some non-Latin or accented characters and, apparently may not be usable at all on some localized systems, particularly Asian system. Better, by far to find appropriate available .ttf or .otf fonts and embed them. To embed fonts is the default option in fpPDF. 

Options, there is an option, poSubsetFont, who's purpose is unclear to this author that causes the DPF engine to crash when used with .otf fonts. And, interesting, with some Chinese .ttf fonts. Some pundits suggest that there are OpenTypeFonts packaged with .otc extensions, maybe, maybe not. In either case, leave that particular option out, does no apparent good, definite harm. 

### gTTFontCache

As mentioned above, gTTFontCache is brought into your application by including the fpttf unit. It does not need to be created (or freed) but does need a touch of setup and some initialization. Because its global, this only needs be done once for the app's run time, even if called repeatably (as long as the availability of fonts does not change during the run). Your code may look like this - 
    
    
    const HaveReadFonts : boolean = false; 
    ...
    ...
        if not HaveReadFonts then begin
           {$if defined(CPU32) and defined(LINUX)}
           gTTFontCache.SearchPath.Add('/usr/share/fonts/');  // Avoids a problem noted on 32bit linux where
           gTTFontCache.BuildFontCache;                       // libfontconfig returns a nil pointer to font.cfg file
           {$else}
           {$ifdef WINDOWS}
           gTTFontCache.SearchPath.Add('./');               // CHECK DAVO Also look for fonts where binary lives on Windows
           {$endif}
           gTTFontCache.ReadStandardFonts;
           {$endif}
           HaveReadFonts := True;  
       end;
    

The Font Cache looks for fonts in the appropriate places for each operating system but you can add to that list with the gTTFontCache.SearchPath.add() line. On Linux for example, it also looks in $HOME/.fonts, a great place for a user to add their own, perhaps short term fonts. 

Its possible (but a bit slow) to iterate over the Cache's internal list once populated by the ReadStandardFonts;, see above. What is really useful is how you can find what you need by declaring a pointer to a Cache Item and using the Cache's Find() command - 
    
    
    var
        CachedFont : TFPFontCacheItem;
        DescenderH : single;
    ...
    ...
    CachedFont := gTTFontCache.Find('Noto Sans CJK SC', Bold, Italic);  // Bold and Italic are booleans
    if Assigned(CachedFont) then begin
        writeln('Width of ABC in a PDF at 12pt is ', CachedFont.TextWidth('ABC',  12));
        writeln('Height of above is ', CachedFont.TextHeight('ABC', 12, DescenderH))
    end else writeln('Sorry, don''t seem to have that font.');
    

That is key information if you are "flowing" text into a PDF, it tells you both the vertical and horizontal information in millimeters of each "word", you keep running totals to to tell you when to start a new line and how much further down the page to put it. You will have to deal with some offset issues if changing font sizes but it is not difficult. 

### Flowing Text into a PDF

You may have a quantity of text that you need to write to a PDF. Maybe that text is 'marked up', has some large fonts, some small, some monospaced etc. Your code must calculate where each line goes and, in particular, how many words you can fit into each line. And, obviously, you need to start a new page when you get to the bottom of the existing one. Here is one approach ! 

I work by breaking the content up into (text)words, I copy the content into a list of records, each record has a word of text, a font type, name, size and bold or italic. Then while writing a line of text, I add up the width of each word I consider until I get to the desired width for the line. The method that calculates the width also calculates the height and I keep a running max-height for that line. 

When I know how much text I can fit into the line, I then calculate the y point to start writing that text to the PDF. 

If all your text is the same size, its a bit easier but you still need to work out how many words fit into each line as you process the content. This system will also work for right aligned text, but slightly different calculation. Right and Left justified is a bit harder, you need to calculate how much space would be left at the end of a line and distribute that extra across the line. 

See <https://github.com/tomboy-notes/tomboy-ng/blob/master/source/kmemo2pdf.pas> for a specific example. 

## See also

  * [Package List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-pdf](https://web.archive.org/web/20250211185506/https://wiki.freepascal.org/fcl-pdf)_
