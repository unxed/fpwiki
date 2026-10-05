# Project Templates

│ **[Deutsch (de)](</Project_Templates/de> "Project Templates/de")** │  **English (en)** │  **[русский (ru)](<../ru/Project_Templates.md> "Project Templates/ru")** │    
****

  


## Contents

  * 1 Overview
  * 2 Package projtemplates
    * 2.1 ProjectFile
    * 2.2 Name
    * 2.3 Author
    * 2.4 Description
    * 2.5 Recurse
    * 2.6 Exclude
    * 2.7 Getting Started



# Overview

You can adapt how to create new projects. The IDE allows to register your own project types via designtime packages. The package **projtemplates** (the normal lazarus binary packages have this preinstalled) allows to simply use existing projects as templates. 

# Package projtemplates

This package implements project templates and comes with the lazarus sources (components/projecttemplates/projtemplates.lpk). 

The package comes preinstalled with the binary packages of Lazarus (e.g. the windows installer). This package adds a category to the **File-New** dialog: **Template projects**

It also adds a **New project from Template** menu item to the **File** menu, as well as a **project template options** menu under the **Tools** menu. This menu item is visible only if the templates directory (see next paragraph) exists and contains at least one template subdirectory. 

In the _Project template options_ , a directory can be selected. This directory should contain a subdirectory per template. Each template directory contains a template for a project: A collection of files which will be copied and used as a new project. 

During the copy, the package will scan the file/directory names and the contents of the files for variable substitution: **__VARNAME__** will be replaced by the value of a variable VARNAME 

By default, the engine knows 2 variables: 

  * ProjDir: The directory where the new project will be created.
  * ProjName: The name of the new project in ProjDir.



Additional variables must be defined in a file called **project.ini** in the directory of the project template. This is a file in Windows INI format. 

The section _[Variables]_ will be scanned for variable names: 
    
    
     [Variables]
     VarName1=Description 1[|DefaultValue 1]
     VarName2=Description 2[|DefaultValue 2]
    

The variables found will be presented with their description in a dialog, and the user must present a value for the variables. 

The' 'project.ini _file can contain a second section, called_ Project _, which_ can contain some info about the project. The following keywords can be found there: 

## ProjectFile

The filename (no extension) of the template project lpi-file. Any file that has a name equal to this (no extension) is treated specially, it is renamed to the new project name (ProjName). That is _ProjectFile=example_ will replace _example.lpi_ , _example.lpr_ and _example.cfg_ with the name given by the user. By default, the value of **ProjectFile** is assumed to be _project_. (note that the .ini file is not copied) 

ProjectFile is not mandatory. The first *.lpi file in the template project folder will be taken as ProjectFile if ProjectFile is not specified or doesn't exist. 

## Name

Name of the template. The Name and description will be presented in the 'File-New' dialog. 

## Author

Author of the template 

## Description

Short (one-line) description of the project. The Name and description will be presented in the 'File-New' dialog. 

## Recurse

A boolean value (1/0) which tells the engine to recurse in subdirectories or not. 

## Exclude

Comma separated list of filename extensions which should not be searched for keyword subsitution. 

## Getting Started

**Step 1:**

Create your own template directory (for example ~/.lazarus/mytemplates) and set this path in Lazarus (menu: Tools -> Project Template Options). 

**Step 2:**

For any template functions to even appear in the Lazarus File menu, you will need at least one template which you can base on your existing (normal) project; 

\- Create a directory in your template directory (for example ~/.lazarus/mytemplates/myfirsttemplate) 

\- Copy your project files in this new directory (*.lfm, *.pas, *.ico, *.lpi, *.lpr, *.res) 

**Step 3:**

Create a "project.ini" file in the new template directory. 

The minimum should look something like this: 
    
    
     [Variables]
     
     [Project] 
     Name=My Form Template
    

**Step 4:**

Restart Lazarus to apply changes. 

_Optional:_

The nice thing of Project Templates is that it supports variables. Since a project will probably not be named the same as your example template you will have to do some code editing and file renaming. 

**Files** : 

Rename the project file (ico, lpi, lpr, res) to __PROJNAME__.ext (where extension is the original extension of course). For example: project.lpi becomes __PROJNAME__.lpi. Note: the content of the lpi file might need some cleaning, for example removing history, reference to files, not sure if the lpi file is even needed. 

**File content** : 

For all files that referred to project.lpi etc., change the content of your files so all "project" references become "__PROJNAME__", e. g.: 
    
    
      <Filename Value="project.lpr"/>
      <Filename Value="__PROJNAME__.lpr"/>
    

You can optionally define your own variables, for example to give the "unit.pas" a different filename. You could define for example __MAINUNIT__ and __MAINFORM__ in project.ini as MAINUNIT and MAINFORM - during creation Lazarus will ask you for these names. Now every occurrence of __MAINUNIT__, for example, will be replaced with the text you provided (avoid special characters and spaces). 

Note that this works for all variables. Every occurrence of a variable in filenames and file content will be replaced by what you entered on creation of a new project based on your template. Predefined variables are PROJNAME and PROJDIR but I didn't find a purpose for PROJDIR just yet. 

**Some examples of a project.ini:**

project.ini: 
    
    
     [Variables] 
     MAINUNIT=Filename for the default unit.pas file
     MAINFORM=Form name
     
     [Project]
     Name=My Custom Form
     Author=Author's Name
     Description=Lazarus LCL form specifically for my needs
    

__PROJNAME__.lpr: 
    
    
    program __PROJNAME__;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}
      cthreads,
      {$ENDIF}
      {$IFDEF HASAMIGA}
      athreads,
      {$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, __MAINUNIT__
      { you can add units after this };
    
    {$R *.res}
    
    begin
      RequireDerivedFormResource:=True;
      Application.Scaled:=True;
      Application.Initialize;
      Application.CreateForm(T__MAINFORM__, __MAINFORM__);
      Application.Run;
    end.
    

__MAINUNIT__.pas: 
    
    
    unit __MAINUNIT__;
    
    {$mode ObjFPC}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs;
    
    type
      T__MAINFORM__ = class(TForm)
      private
    
      public
    
      end;
    
    var
      __MAINFORM__: T__MAINFORM__;
    
    implementation
    
    {$R *.lfm}
    
    end.
    

__MAINUNIT__.lfm: 
    
    
    object __MAINFORM__: T__MAINFORM__
      Left = 725
      Height = 366
      Top = 433
      Width = 503
      Caption = '__MAINFORM__'
      LCLVersion = '2.2.6.0'
    end
    

Keep in mind that this brief tutorial is just to get you started, the variable replacement mechanism offered can be utilized for many different purposes. For example to define a form caption, a copyright statement, a web link on your forms, specific objects, etc. \--[Hansaplast](</index.php?title=User:Hansaplast&action=edit&redlink=1> "User:Hansaplast \(page does not exist\)") 16:30, 4 August 2012 (UTC)

---

_Source: [https://wiki.freepascal.org/Project_Templates](https://web.archive.org/web/20250211173622/https://wiki.freepascal.org/Project_Templates)_
