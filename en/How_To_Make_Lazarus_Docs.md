# How To Make Lazarus Docs

│ **English (en)** │

## Contents

  * 1 Making Documentation Files for Lazarus
    * 1.1 Overview and XML description files
    * 1.2 Creating output
      * 1.2.1 Creating output with lazde
      * 1.2.2 Creating output with build_lcl_docs
      * 1.2.3 Creating output with fpdoc



## Making Documentation Files for Lazarus

### Overview and XML description files

Documentation files, which will eventually be incorporated into the on-line Help system, can be generated from the Unit files in the Lazarus Component Library (lcl). The FpDoc utility can generate HTML (and other, such as CHM) files using the Pascal source files (*.pp or *.pas) as input. A great deal more useful information can be incorporated if an XML description file can be used in conjunction with the source file. This description file contains the actual help text, while the source code provides the function names etc. 

While it is perfectly possible to generate the XML tags by hand, using the instructions in the FpDoc manual, it is much easier to use the utility _makeskel_ which is provided as part of the FpDoc suite. This utility generates an XML file with a set of empty tags for every identifier, type, procedure and function in the source file. 

The user can then edit this XML file, either with 

  * a simple text editor
  * an XML editor like KXMLEditor
  * the specialized [FPDoc editor](<FPDoc_Editor.md> "FPDoc Editor") which is included with Lazarus. This tool can be used to quickly edit entries when writing code.
  * the specialized [Lazarus Documentation Editor](<Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor") which is included with Lazarus (you'll need to compile it and use it as an external tool). This is the most extensive tool and is quite user-friendly.



There is a huge number of entries in these files: I suggest that only the tags describing Objects be edited, at least initially (use the Search facility to find 'object', and then enter short titles and descriptions for the components). 

### Creating output

Having an XML description file is fine, but you probably want some more readable output. You can generate e.g. html, chm and other output (note: not all platforms support all outputs). 

#### Creating output with lazde

The [Lazarus_Documentation_Editor](<Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor") (LazDE) tool has a dialog (Menu Extra, Build) that lets you build help files as well. This tool offers GUI controls for the various files so it might be the easiest for beginners or casual users. This tool calls fpdoc to process the files. 

#### Creating output with build_lcl_docs

The Lazarus tool build_lcl_docs in $(lazarusdir)\docs\html allows you to combine many description files and source file and generate all kinds of output. Internally it uses fpdoc like the previous tool. 

#### Creating output with fpdoc

Finally, you can also call fpdoc directly. 

When you have finished editing the XML file, save it and then run 
    
    
    fpdoc --package=lcl --input=thisunit.pp --descr=thisunit.xml
    

The "lcl" indicates you're editing LCL help; if you're editing FPC help, it would probably be "fcl" (these names are determined by the developers in charge of documentation and are not apparent from other sources). The resultant HTML files will be placed in a subdirectory called 'thisunit'. 

(As mentioned, you can also output in other formats, but those are not discussed here) 

There will be a huge number of HTML files (typically about 160) - these will eventually all be used for Help files, but there is too much information, for example, to put on this WiKi site. It is helpful to filter the files, only including those that describe components. 

This can be done with: 
    
    
    grep -l Inheritance * > outputfile
    

as only components have a description of their inheritance. 

The outputfile can then be used to drive a utility to convert HTML to text. One such application is html2text. The process has been partly automated in the following Pascal program (which is excessively simple, not particularly robust, but works): 
    
    
    program convhtml;
    { CT Kirkpatrick, MD 20040720 }
    {$mode objfpc}{$H+}
    { reads a list of filenames from listfile, and converts them in
    turn from HTML to plain text, writing the output of all the files to outfile }
    uses
      Classes, unix;
    var
      listfile, outfile: text;
      pathname, listfname, outfname, fname: string;
      execstring, comstring: string;
    begin
      Writeln ('Enter path for files to convert');
      Readln (pathname);
      Writeln ('Enter name of file to contain list of files for conversion');
      Readln (listfname);
      shell ('grep -l Inheritance ' + pathname + '* > ' + pathname + 'filelist.txt');
      Writeln ('Enter name of output file');
      Readln (outfname);
      Assign (listfile, pathname +'filelist.txt');
      Assign (outfile, pathname + outfname);
      Reset (listfile);
      Rewrite (outfile);
      while not eof (listfile) do
      begin
        readln (listfile, fname); //writeln (fname);
        //if fname = '' then begin readln (listfile, fname); writeln (fname) end;
        execstring := '/usr/bin/html2text ';
        comstring := '-nobs ' + fname
                  + ' >> ' + pathname + outfname;
        writeln ('Executing ', execstring, comstring);
        shell (execstring + comstring);
      end;
    end.

---

_Source: [https://wiki.freepascal.org/How_To_Make_Lazarus_Docs](https://web.archive.org/web/20240910175947/https://wiki.freepascal.org/How_To_Make_Lazarus_Docs)_
