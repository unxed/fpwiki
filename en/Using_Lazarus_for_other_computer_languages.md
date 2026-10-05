# Using Lazarus for other computer languages

│ **English (en)** │  **[magyar (hu)](</Using_Lazarus_for_other_computer_languages/hu> "Using Lazarus for other computer languages/hu")** │  **[русский (ru)](<../ru/Using_Lazarus_for_other_computer_languages.md> "Using Lazarus for other computer languages/ru")** │    
****

Lazarus is great for Free Pascal. But you can use the IDE for other languages too. This is useful to port C code or to edit cross multi tier applications, without the need to use different editors and reducing the trouble when switching between different sets of shortcuts and menu entries. 

## Contents

  * 1 Syntax Highlighting
  * 2 Compilation
    * 2.1 1\. Create a new shortcut
      * 2.1.1 Pros
      * 2.1.2 Cons
    * 2.2 2\. Replace the compile command of the project
      * 2.2.1 Pros
      * 2.2.2 Cons
    * 2.3 3\. Define a compile command for a single file
      * 2.3.1 Pros
      * 2.3.2 Cons
    * 2.4 See Also



# Syntax Highlighting

Lazarus comes with syntax highlighting for more than a dozen languages. The language is guessed from the file extension. The colors can be setup in the editor options. 

If your language is not yet there, you can either use one of the existing highlighters by extending the list of file extensions in the editor options. Or you can create your own highlighter and send us the new highlighter. The easiest way to create a new highlighter is to copy an existing file in lazarus/components/synedit/. The highlighter units all begin with 'synhighlighter', for example synhighlighterphp.pp is the php highlighter. 

Hint: You can quick switch the syntax highlighter for a file via the popup menu of the source editor. Just right click / file settings / highlighter. 

# Compilation

To setup the compile command there are three possibilities: 

## 1\. Create a new shortcut

You can create a new shortcut and a menu item, that starts an external program with Tools / Configure Custom Tools / Add. 

For example to invoke 'make' to compile via Makefile: 

  * Title: Build via make
  * Programfilename: $MakeExe(make)
  * Parameters:
  * Working Directory: $ProjPath()
  * Options: all disabled
  * Key: choose one or leave untouched



The **MakeExe** macro function appends the file extension '.exe' under windows. The **ProjPath** macro is replaced by the current project directory - the directory where the .lpi file is. 

### Pros

  * Works for all projects. No need to setup for every new project.



### Cons

  * The same command and parameters for all projects. Individuality must be achieved by calling a script.



## 2\. Replace the compile command of the project

When you press F9 or Ctrl-F9 the IDE checks if something has changed and invokes the compiler. The compiler is normally FPC, but you can replace that with a command of your choice. 

Open Project / Compiler Options / Compilation. Then disable in the _Compiler_ box all options **Compile** , **Build** and **Run**. Then write the new command in the _Execute before_ box and enable **Show all messages**. 

For example to invoke 'make' to compile via Makefile: 

$MakeExe(make) 

The **MakeExe** macro function appends the file extension '.exe' under windows. The working directory is always the project directory - where the .lpi file is. 

### Pros

  * Command is stored in the .lpi file, so project can easily be distributed and copied to other machines.
  * Only invoked if a file is changed.
  * Required Packages are auto compiled before.



### Cons

  * Must be copied to every new project.



## 3\. Define a compile command for a single file

Normally when pressing F9 or Ctrl-F9 the IDE compiles a project. But you can override this behavior for each file in a project via Run / Configure Build+Run file. 

This works pretty much like the two above examples. 

### Pros

  * Good for mixed source projects. For example a pascal project with one or more files in another language.
  * These settings are stored for pascal sources as IDE directives directly in the pascal source and since lazarus 0.9.25 for non pascal files it is stored in the project (.lpi file).



### Cons

  * No update check. The called script/compiler must check itself if a build is needed.



## See Also

  * [LCL Bindings](<LCL_Bindings.md> "LCL Bindings")

---

_Source: [https://wiki.freepascal.org/Using_Lazarus_for_other_computer_languages](https://web.archive.org/web/20250114062051/https://wiki.freepascal.org/Using_Lazarus_for_other_computer_languages)_
