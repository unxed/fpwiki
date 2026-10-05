# FCL

│ **[Deutsch (de)](</FCL/de> "FCL/de")** │  **English (en)** │  **[español (es)](</FCL/es> "FCL/es")** │  **[suomi (fi)](</FCL/fi> "FCL/fi")** │  **[français (fr)](</FCL/fr> "FCL/fr")** │  **[Bahasa Indonesia (id)](</FCL/id> "FCL/id")** │  **[日本語 (ja)](</FCL/ja> "FCL/ja")** │  **[русский (ru)](<../ru/FCL.md> "FCL/ru")** │  **[中文（中国大陆） (zh_CN)](</FCL/zh_CN> "FCL/zh CN")** │    
****

The _Free Component Library_ (**FCL**) consists of a collection of units, providing components (mostly classes) for common tasks. You can also see the [LCL Components](<LCL_Components.md> "LCL Components") available directly within Lazarus. The FCL intends to be compatible with Delphi's _Visual Component Library_ (VCL), but the FCL is restricted to non-visual components. On the other hand, the FCL also goes beyond the VCL. 

See [Free Component Library](<http://www.freepascal.org/fcl/fcl.var>) for the current development status and an overview of the available components (though this seems inconsistent with [Reference for 'fcl'](<http://lazarus-ccr.sourceforge.net/docs/fcl/index.html>) in Lazarus). You can also check the [source repository](<http://www.freepascal.org/cgi-bin/viewcvs.cgi/trunk/packages/>). Note that there are also some platform-specific files in the FCL. 

After the example, you can find a list of some FCL components. 

## Contents

  * 1 Usage
  * 2 Subpackages
  * 3 Documentation
  * 4 Example
  * 5 FCL Components



### Usage

To use an FCL _component_ you need to include the name of the [unit](<Unit.md> "Unit") which implemented it, in a **uses** clause in your program or unit (see example below). The default compiler [configuration file](<Configuration_file.md> "Configuration file") is set up to search the FCL directories for such units. You can also set the appropriate search path with a command-line compiler option of the form -Fu<path-to-fcl-units>. 

### Subpackages

The full list of subpackages of the FCL can be found here: [Package List](<Package_List.md> "Package List")

The fcl-prefixed subpackages are: 

  * [fcl-base](<fcl-base.md> "fcl-base") The base units (including e.g. an [expression parser](<How_To_Use_TFPExpressionParser.md> "How To Use TFPExpressionParser"))
  * [fcl-async](<fcl-async.md> "fcl-async") Asynchronous I/O (serial?)
  * [fcl-db](<fcl-db.md> "fcl-db") Generic database support + prepackaged drivers
  * [fcl-fpcunit](<fcl-fpcunit.md> "fcl-fpcunit") Unit testing framework
  * [fcl-image](<fcl-image.md> "fcl-image") Raster Image reading and writing (aka fpimage)
  * [fcl-json](<fcl-json.md> "fcl-json") Routines for javascript object streaming
  * [fcl-net](<fcl-net.md> "fcl-net") Network related units
  * [fcl-passrc](<fcl-passrc.md> "fcl-passrc") Pascal language parsing and transformation
  * [fcl-process](<fcl-process.md> "fcl-process") Process controll
  * [fcl-registry](<fcl-registry.md> "fcl-registry") Registry
  * [fcl-res](<fcl-res.md> "fcl-res") Resource handling
  * [fcl-stl](</index.php?title=fcl-stl&action=edit&redlink=1> "fcl-stl \(page does not exist\)") Generic container library (standard template library)
  * [fcl-web](<fcl-web.md> "fcl-web") Helper for web development
  * [fcl-xml](<fcl-xml.md> "fcl-xml") XML (DOM) unit and related units.



### Documentation

Currently, the FCL is not completely documented (feel free to contribute; also take a look at [Reference for 'fcl'](<http://lazarus-ccr.sourceforge.net/docs/fcl> "doc:fcl")). For Delphi compatible units, you could consult the Delphi documentation. You can always take a look at the source files in the [source repository](<http://www.freepascal.org/cgi-bin/viewcvs.cgi/trunk/packages/>). 

### Example

The following program illustrates the use of the class TObjectList in the FCL unit Contnrs (providing various containers, including lists, stacks, and queues): 
    
    
     program TObjectListExample;
     {$mode ObjFPC} 
     uses
       Classes, { from RTL for TObject }
       Contnrs; { from FCL for TObjectList }
     
     type
        TMyObject = class(TObject)  { just some application-specific class }
        private
          FName: String; { with a string field }
        public
          constructor Create(AName: String); { and a constructor to create it with a given name }
          property Name: String read FName; { and a property to read the name }
       end;
     
     constructor TMyObject.Create(AName: String);
     begin
       inherited Create;
       FName := AName;
     end;
     
     var
       VObjectList: TObjectList; { for a list of objects; it is a reference to such a list! }
     
     begin
       VObjectList := TObjectList.Create;  { create an empty list }
       with VObjectList do
       begin
         Add(TMyObject.Create('Thing One'));
         Writeln((Last as TMyObject).Name);
         Add(TMyObject.Create('Thing Two'));
         Writeln((Last as TMyObject).Name);
       end;
     end.
    

This program must be compiled in an object-oriented mode, such as -Mobjfpc or -Mdelphi. 

### FCL Components

This is not an exhaustive list (to avoid duplication of effort). It only mentions some important components, or components that are otherwise not easy to find. 

Classes
    Base classes for Object Pascal extensions in Delphi mode
Contnrs
    Some common container classes
[FPCUnit](<fpcunit.md> "fpcunit")
    A Unit testing framework (based on Kent Beck's unit testing framework. See [JUnit](<http://www.junit.org/>)),[FPCUnit tutorial (pdf)](<http://www.freepascal.org/~michael/articles/fpcunit/fpcunit.pdf>)
XMLRead, XMLWrite and DOM
    Detailed at the [XML Tutorial](<XML_Tutorial.md> "XML Tutorial")

---

_Source: [https://wiki.freepascal.org/FCL](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/FCL)_
