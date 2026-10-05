# Delphi language features missing from the Free Pascal Compiler

Note: some new Delphi features are already implemented in [FPC trunk](<FPC_New_Features_Trunk.md> "FPC New Features Trunk"). 

## Contents

  * 1 New since D4
    * 1.1 Exporting of overloaded functions
    * 1.2 implements delegation of interfaces
  * 2 New since Delphi 2007
  * 3 New since Delphi 2009
    * 3.1 Anonymous Methods
  * 4 New since Delphi 2010
    * 4.1 Custom Attributes
    * 4.2 Enhanced RTTI
    * 4.3 Delayed directive
  * 5 Misc
  * 6 Already implemented
    * 6.1 FPC 3.0.4/trunk
      * 6.1.1 Advanced records
    * 6.2 FPC 2.6.0
      * 6.2.1 AS and IS extended for interfaces
  * 7 Reference



## New since D4

### Exporting of overloaded functions

(version not entirely sure, but came in with overloading, so definitely pre-D6) 

In library packages select an overloaded variant to export. Assume we have some overloaded methods: 

function doSomeThing(a:type1):type2;stdcall; function doSomeThing(a:type3):type4;stdcall; 

and in the library (for building the dll using the unit above) we want to export them: 

exports 
    
    
       doSomeThing(a:type1) name 'doSomeThingTYPE1',
       doSomeThing(a:type3) name 'doSomeThingTYPE3';
    

### implements delegation of interfaces

Delegating interfaces to classes is not always supported, 

  * [Mantis 16531](<http://bugs.freepascal.org/view.php?id=16531>)
  * [Mantis 8591](<http://bugs.freepascal.org/view.php?id=8951>)
  * [Mantis 23403](<http://bugs.freepascal.org/view.php?id=23403>)



## New since Delphi 2007

## New since Delphi 2009

#### Anonymous Methods

  1. <http://docs.embarcadero.com/products/rad_studio/delphiAndcpp2009/HelpUpdate2/EN/html/devcommon/anonymousmethods_xml.html>
  2. <http://delphi.about.com/od/objectpascalide/a/understanding-anonymous-methods-in-delphi.htm>



## New since Delphi 2010

#### Custom Attributes

Custom Attributes are a language feature in Delphi that allow annotating types and type members with special objects that carry additional information. Attributes extend the object-oriented model with aspect-oriented elements. 

<http://delphi.about.com/od/oopindelphi/a/delphi-attributes-understanding-using-attributes-in-delphi.htm>

#### Enhanced RTTI

<http://docwiki.embarcadero.com/RADStudio/en/RTTI_directive_(Delphi)>

#### Delayed directive

Info is here: <http://docwiki.embarcadero.com/RADStudio/en/Libraries_and_Packages#Delayed_Loading> Some info about internals: 

  1. <http://blogs.embarcadero.com/abauer/2009/08/25/38894>
  2. <http://blogs.embarcadero.com/abauer/2009/08/29/38896>
  3. <http://blogs.embarcadero.com/chrishesik/2009/11/02/35056>



fpc wiki about that topic: [http://wiki.freepascal.org/Dynamically_loading_headers](<Dynamically_loading_headers.md>)

Originally I thought that delayed is similar to weakexternal which FPC has for some platforms but after reading this: <http://docwiki.embarcadero.com/CodeExamples/XE3/en/DelayedLoading_(Delphi)> I understand that this is simply a wrapper around LoadLibrary and GetProcessAdress with automatic loading and unloading.--[Paul Ishenin](</User:Paul_Ishenin> "User:Paul Ishenin") 13:45, 18 January 2013 (UTC) 

## Misc

  1. [(Library) packages](<packages.md> "packages")
  2. "automated" keyword, which is like "public", but generates COM specific RTTI. Afaik this kind of COM usage is deprecated though.



## Already implemented

### FPC 3.0.4/trunk

#### Advanced records

<http://docwiki.embarcadero.com/RADStudio/en/Structured_Types#Records_.28advanced.29>

### FPC 2.6.0

#### AS and IS extended for interfaces

New since Delphi 2010. Info is here: 

  1. <http://docwiki.embarcadero.com/RADStudio/en/Interface_References#Casting_Interface_References_to_Objects>
  2. <http://blogs.embarcadero.com/abauer/2009/08/21/38893>



Implemented in FPC 2.6.0. 

## Reference

  1. <http://edn.embarcadero.com/article/34324>
  2. <http://edn.embarcadero.com/article/images/39076/New_Delphi_Coding_Styles_and_Architectures.pdf>
  3. <http://docwiki.embarcadero.com/RADStudio/en/What's_New_in_Delphi_and_C%2B%2BBuilder_2010>

---

_Source: [https://wiki.freepascal.org/delphi_language_features_which_fpc_does_not_have](https://web.archive.org/web/20221001022151/https://wiki.freepascal.org/delphi_language_features_which_fpc_does_not_have)_
