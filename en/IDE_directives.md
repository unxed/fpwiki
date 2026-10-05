# IDE directives

│ **English (en)** │  **[español (es)](</IDE_directives/es> "IDE directives/es")** │  **[français (fr)](</IDE_directives/fr> "IDE directives/fr")** │    
****

## Contents

  * 1 Overview
  * 2 Hide one hint, note or warning
  * 3 File encoding
  * 4 Run/Build file
  * 5 Other IDE Directives
  * 6 Reading / finding IDE directives



# Overview

IDE directives are similar to compiler directives: a special kind of comment placed in your source code. They are enclosed by curly brackets with an opening percentage sign. For example: 
    
    
    {%H-}
    

The compiler sees them simply as comments which it ignores. The IDE uses these directives to store source-specific information which is independent of project and session. 

# Hide one hint, note or warning

To hide a single hint, note or warning you use the following IDE directive: 
    
    
    procedure DoSomething({%H-}NotUsed: char);
    begin
    
    end;
    

This will suppress the line in the Lazarus Messges window that otherwise would report 
    
    
    Hint: Parameter "NotUsed" not used
    

The compiler still issues the Hint, but the {%H-} IDE directive causes the IDE to filter the Hint out of the Messages view. 

Note: You can right click on a hint in the message window and choose the context menu option "Hide message via directive" to insert the directive automatically. 

This feature was introduced in Lazarus 0.9.29. 

# File encoding

You can specify what encoding the IDE should use for a source file by adding a BOM or an encoding directive at the very start of a source file: 
    
    
    {%encoding CP1250}
    

See the unit lcl/lconvencoding.pas for all possible values. 

# Run/Build file

The IDE can run and build single files instead of projects. The parameters for building/running a file are stored as IDE directives within the file itself. 

  * {%BuildCommand <command line used for building, default is $(CompPath) $(EdFile)>}
  * {%BuildWorkingDir <working directory, default is the file path>}
  * {%BuildScan <parsers>} List of parsers used for build output
  * {%RunCommand <command line used to run, default is $NameOnly($(EdFile))>}
  * {%RunWorkingDir <working directory, default is the file path>}
  * {%RunFlags <flags, default Build+>} where flags is a space separated list of options. E.g. {%RunFlags Messages+ Build+} 
    * Build+ build before run, default: on
    * Messages+ show run output in the Messages window, default: off



See [Configure Build File](<IDE_Window__Configure_Build_file.md> "IDE Window: Configure Build file"). 

# Other IDE Directives

  * {%Region} Allows you to define your own [Code Folding](<IDE_Window__Editor_Options_Code_Folding.md> "IDE Window: Editor Options Code Folding") blocks, by putting this special comment into the code. You have to specify the end of the folded region with {%Endregion}
  * {%MainUnit path/to/unit.pas} Used as first line in an include file to give codetools a hint where the unit file is. Any **/** and **\** are converted to the current path delimiter.



# Reading / finding IDE directives

The codetools unit basiccodetools contains several functions for reading IDE directives: 

  * FindNextIDEDirective
  * CodeToolBoss.GetIDEDirectives

---

_Source: [https://wiki.freepascal.org/IDE_directives](https://web.archive.org/web/20250323145534/https://wiki.freepascal.org/IDE_directives)_
