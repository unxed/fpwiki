# TSubForm

│ **English (en)** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** TSubform and its documentation seem to be unmaintained...

**TSubForm** is a Data-Aware SubForm 

This component is exactly what it sounds like - a subform component. Use it as you would a TPanel - drop components on it, assign a DataSource and at runtime the magic begins. 

At runtime, all the original components are hidden - Don't Panic! They are duplicated using streams for every record in the DataSource's dataset. If you have added any data-aware components to the component, and their Datasources are set to the same as the component, the duplicates are assigned a new DataSource which maps onto a TMemDataSet. Okay, that sounds nice and complex, but the end result is that the component replicates your controls for every row in the table it's assigned to. 

This means with NO CODE, you can create a customised row (or Column - see the Direction property) layout for any dataset. Assign a very small amount of code to TDBEdit's KeyUp event to check for VK_RETURN and you can even edit it. This is achieved by a BeforePost event on the underlying TMemDataSet. 

## Contents

  * 1 Bugs/Enhancements
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 The _TSubForm_ Example Application



#### Bugs/Enhancements

Well, it's probably pretty easy to make this fall over I would imagine, but it is only 0.1 :-) I really want to make a .Post procedure so you can fire a post to the active memds (and therefore the real DS) but I've run out of debugging time for now. The code is mostly written, just something causes a sigsegv I haven't had time to chase down. 

Thanks : giantm and all the folks on #lazarus-ide for helping me figure some of the more complex stuff especially RTTI 

Its main caracteristics are : 

  * Row or Column per row in the dataset
  * Seamless Memory Dataset per row
  * Data-Aware components are set to Memory Dataset if set to the same as the component.
  * Posts to the Memory Dataset will post back to the DataSource's dataset.



The download contains the component, an installation package and a demo application, that illustrates the features of the component. 

If you add components to the form that are not in the Standard, Additional or Data Controls pallettes, you will need to write your own OnFindClass procedure as at [Reading a component from a stream](<Streaming_components.md> "Streaming components"). I elected to create a property you can use called CascadeFindClass, so you can decide if to use mine, your own or mine then your own. 

### Screenshot

To follow soon. 

### Author

### License

Your choice of : 

  * [GPL](<http://www.opensource.org/licenses/gpl-license.php>)
  * [LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)
  * The modified LGPL that Lazarus is distributed under - see [Lazarus' COPYING.modifiedLGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>)



(please contact the author if the License options above don't work with your project licensing) 

### Download

The latest stable release can be found on [Lazarus-ccr's SF.net download page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=230010>). 

### Change Log

  * Version 0.1 _Apr 26 2007_



### Dependencies / System Requirements

Requirements : A Bookmarkable DataSet and a DataSource. Note: at least TDBF and TMemDS in current FPC ARE bookmarkable, but appear not to be (.BookMarkValid always returns false even for valid bookmarks). 

Built in Lazarus svn 10988 on Linux, doesn't seem to work on win32 svn10809. 

Status: _Alpha_ First Release, works here. 

Issues: Might have issues with AutoInc Fields, as MemDS doesn't implement them 

### Installation

  * Open the .lpk file in lazarus
  * Press the Install button on the Package manager



### The _TSubForm_ Example Application

**Installation**

  * Open example/testproject.lpi
  * compile
  * run

---

_Source: [https://wiki.freepascal.org/TSubForm](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/TSubForm)_
