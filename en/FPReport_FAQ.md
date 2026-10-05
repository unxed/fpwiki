# FPReport FAQ

│ **English (en)** │

## Contents

  * 1 General
    * 1.1 Read the README.txt file
    * 1.2 FPReport <> LazReport
    * 1.3 Papermanager must registered
    * 1.4 Fontmanager
  * 2 FPC FPReport
    * 2.1 I did not find FPReport in my FPC dir
    * 2.2 What is the minimum requirement
    * 2.3 Expression and Variables
  * 3 Lazarus FPReport
    * 3.1 I did not find FPReport in my Lazarus directory
    * 3.2 I cannot compile the FPReport packages
    * 3.3 Lazarus does not start anymore in Windows
    * 3.4 My Report does not run/start in Windows
    * 3.5 My Font is not found in windows



## General

### Read the README.txt file

Before you compile the FPReport, have a look at the README.txt 

### FPReport <> LazReport

FPReport is not LazReport! These are two different reporting systems. 

  * LazReport is based on FreeReport 2.32, see [LazReport_Documentation](<LazReport_Documentation.md> "LazReport Documentation").
  * FPReport is written from scratch, see [FPReport](<FPReport.md> "FPReport") documentation.



### Papermanager must registered

You never registered the standard page sizes with: 
    
    
      if PaperManager.PaperCount=0 then
        PaperManager.RegisterStandardSi
    

### Fontmanager

Your report used LiberationSans font. Make sure you added the search paths to the font cache. eg: 
    
    
      {$IFDEF UNIX}
        gTTFontCache.SearchPath.Add(GetUserDir + '.fonts/');
        gTTFontCache.SearchPath.Add('/data/devel/Wisa/fonts/Liberation/');
        gTTFontCache.BuildFontCache;
      {$ENDIF}
    

In windows this is usable: 
    
    
      gTTFontCache.ReadStandardFonts;
      gTTFontCache.BuildFontCache;
    

Default Fonts (cDefaultFont and ReportDefaultFont) on 

  * **Unix** LiberationSans
  * **Windows** Arial
  * **Not Unix and not windows** Helvetica



In some environments like Raspbian you may have trouble with the defaults! You may see a message that the font is not found and the report is not generated. 

## FPC FPReport

### I did not find FPReport in my FPC dir

It is located in the FPC\packages\fcl-report dir, but only in FPC trunk (=3.1.1) since rev. 36962 (20 Aug 2017). 

### What is the minimum requirement

Info from the mailing list - FPC 2.6.4 (and Lazarus 1.6.2), but you will need to copy some additional units from the trunk version of the FPC source repo (in particular, fpexprpars, and fcl-pdf) and the Lazarus trunk repo too. 

### Expression and Variables

If you use float variables and string values on a non-English configured system you will have problems with the comma as a decimal separator (eg German configured computer). All results will have a dot instead of the the configured value in format setting. This is by design (see Bug report 0036545 and 0036525)! 

_Explanation:_ The Value property of a variable is written to the JSON or XML when saving/loading the report: see ReadElement/WriteElement. So it must be in a transportable, locale-neutral format. 

Imagine saving the report to stream and in a File or DB Blob field, then loading it on another PC with different locale settings. If the stream is written with decimal separator, and read with decimal separator there will be an error on load. 

See also SetValue() where Val() is used to create the value from a string. 

## Lazarus FPReport

### I did not find FPReport in my Lazarus directory

Only in Lazarus trunk since rev. 55719 (20 Aug 2017). Look in LAZARUSDIR\components\fpreport there is a runtimepackage lclfpreport.lpk. 

### I cannot compile the FPReport packages

As of 20th January 2019 FPReport does not compile out of the box in 64 bit Lazarus (Windows version at least). It should compile flawlessly in 32 bit Lazarus, though (see also "I did not find FPReport in my Lazarus directory" above). 

### Lazarus does not start anymore in Windows

You need to copy freetype-6.dll and zlib1.dll to your LAZARUSDIR (the folder where lazarus.exe resides). Make sure you use 32 bit DLLs with 32 bit Lazarus and 64 bit DLLs with 64 bit Lazarus. These DLLs are not included. Please see the FreeType [[1]](<https://www.freetype.org/>) and Zlib [[2]](<https://www.zlib.net/>) project pages for details on how to get them. _Please note that FPReport expects a "freetype**-6**.dll" whereas the install package from freetype.org includes a "freetype6.dll" (without the dash) or a "freetype.dll", so you have to rename the file to "freetype-6.dll" before starting Lazarus._

There is a breaking change, the library must be "freetype.dll" in newer versions of fpc (actual fixes32 and trunk) 

### My Report does not run/start in Windows

Start the application from the console, so you can see if some dll's are missing. Normally freetype-6.dll and zlib1.dll are missing. For more information read the README.txt. On FPC 3.3.1 it is freetype.dll instead of freetype-6.dll. Use the correct version for your bitness (32 or 64 Bit) 

### My Font is not found in windows

You have to use the Postscript name of the font. For example, under Windows you have the font 'Arial' - this is one of the standard fonts - but you have to write Memo.Font.Name := 'ArialMT' because ArialMT is the correct Postscript name. 
    
    
      for i:= 0 to gTTFontCache.Count-1 do begin
        try
          Memo1.Append(TFPFontCacheItem(gTTFontCache.Items[i]).HumanFriendlyName +'...'+ TFPFontCacheItem(gTTFontCache.Items[i]).PostScriptName);
        except
          Memo1.Append('ERROR in...'+ TFPFontCacheItem(gTTFontCache.Items[i]).FileName)
        end;
      end;

---

_Source: [https://wiki.freepascal.org/FPReport_FAQ](https://web.archive.org/web/20230528232904/https://wiki.freepascal.org/FPReport_FAQ)_
