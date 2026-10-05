# Creating A Patch

│ **[Deutsch (de)](</Creating_A_Patch/de> "Creating A Patch/de")** │  **English (en)** │  **[español (es)](</Creating_A_Patch/es> "Creating A Patch/es")** │  **[français (fr)](</Creating_A_Patch/fr> "Creating A Patch/fr")** │  **[日本語 (ja)](</Creating_A_Patch/ja> "Creating A Patch/ja")** │  **[português (pt)](</Creating_A_Patch/pt> "Creating A Patch/pt")** │  **[русский (ru)](<../ru/Creating_A_Patch.md> "Creating A Patch/ru")** │  **[slovenčina (sk)](</Creating_A_Patch/sk> "Creating A Patch/sk")** │    
****

If you want to submit improvements to the FPC or Lazarus code, you need to submit a patch which developers can easily merge. 

Exceptions: 

  1. .po translation files should be sent as whole files
  2. new files should be sent as whole files, with instructions where they should be placed



## Contents

  * 1 Requirements
  * 2 Platform differences
    * 2.1 Windows
    * 2.2 *nix systems
  * 3 Creating a patch using Git
  * 4 Submitting the patch
  * 5 Git
  * 6 Applying a patch
    * 6.1 A patch made with "git format-patch"
      * 6.1.1 Git
      * 6.1.2 patch
      * 6.1.3 TortoiseMerge
    * 6.2 Troubleshooting
  * 7 See also



## Requirements

You need the **main/development** version of Lazarus (or FPC). You can get Lazarus from GitLab repository. [Getting Lazarus development version](<Getting_Lazarus.md> "Getting Lazarus"). This is the native repository. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** **If you submit patches, do _not_ base them on stable versions of Lazarus/FPC. Typically the development version is very different from stable versions as many improvements and fixes have been applied by the developers**

[^Note]: The term **"main/development version of Lazarus"** appears to mean minor version number 99 (i.e., currently version 4.99, at the time of this writing), as per <https://wiki.lazarus.freepascal.org/index.php/Version_Numbering#Lazarus_3.0_and_newer>. 

## Platform differences

The instructions later assume you have opened a command prompt and moved (cd) to the directory of the repository. Here are the details: 

### Windows

Assuming you checked out Lazarus into C:\lazarus, open command prompt (cmd.exe) and type "cd \lazarus". 

### *nix systems

Assuming you checked out Lazarus into ~/lazarus, open terminal and type "cd ~/lazarus". 

## Creating a patch using Git

First, develop your code in a separate branch. While your development branch is active, you can create patches of all your local commits by : 
    
    
    git format-patch main
    

It creates a set of patches named like "0001-CommitMsg.patch", "0002-CommitMsg.patch" and so on. 

If you want all the changes in one patch, either combine the commits using "git rebase -i ..." or use the following command : 
    
    
    git format-patch main --stdout > mypatch.patch
    

## Submitting the patch

Now you have a patch. I'd suggest to look the file over to see if it looks ok (no unexpected changes). 

The recommended way to submit a patch is through the [bug tracker](<http://bugs.freepascal.org>), see [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") for details. If there is a report for the issue your patch fixes, use that, otherwise create a new issue. Upload the file to attach it to the issue. 

## Git

The GitHub repository [[1]](<https://github.com/graemeg/lazarus>) is only a mirror and does not accept pull-requests. 

## Applying a patch

This explains how to apply somebody else's patch to your local repository. You can test the patch by using the --dry-run toggle switch like this: 
    
    
    patch --dry-run < somepatch.diff
    

The output of the patch program will be identical to the actual patching, only it does not alter the sourcecode files. Very handy for testing, without the possibility to screw up your source. 

### A patch made with "git format-patch"

#### Git

Git itself applies the patch like this : 
    
    
    git apply 0001-gitpatch.patch
    

#### patch

The "patch" command now supports git format patches with -p1. This is tested with patch v.2.6.1 on Linux, old versions may not support it. 
    
    
    patch -p1 < 0001-gitpatch.patch
    

"patch" is available for Windows, too, but there are also GUI tools for the job. 

#### TortoiseMerge

TortoiseMerge supports the Git format patch without problems. It is installed together with Tortoise SVN but is not integrated in explorer. It must be opened from the Start menu. 

ToDo: add more GUI tools that support Git format patches 

### Troubleshooting

Finally, patches may have a Unix/Linux line ending (LF) while your local file has Windows (CR+LF) line endings or vice versa. You'll have to convert the patch file before applying on Windows at least, as the supplied patch.exe is picky about line endings. 

On Windows, the patch.exe supplied with FPC/Lazarus is very picky; you may have better luck with the patch.exe supplied by Git: 
    
    
    "C:\Program Files (x86)\Git\bin\patch.exe" -p0 < mypatch.diff
    

## See also

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") general information on submitting bugs, what should be covered in a bug report, and using the bug tracker.
  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports") detailed information on what should be covered in a bug report.
  * [Database bug reporting](<Database_bug_reporting.md> "Database bug reporting") Specific info and sample programs for database bugs
  * [Moderating the bug tracker](<Moderating_the_bug_tracker.md> "Moderating the bug tracker")

---

_Source: [https://wiki.freepascal.org/Creating_A_Patch](https://web.archive.org/web/20250513133216/https://wiki.freepascal.org/Creating_A_Patch)_
