# RXfpc

│ **English (en)** │  **[русский (ru)](<../ru/RXfpc.md>)** │

## Contents

  * 1 About
  * 2 Screenshot
  * 3 Download
  * 4 SVN
  * 5 Dependencies / System Requirements
  * 6 Installation
  * 7 Note



## About

  * [RxLib](<http://alexs75.narod.ru/fpc/rxfpc/>) for Lazarus is a set of components that support building of flexible and robust user interfaces. It contains some of the components of the well known RxLib for Delphi.



## Screenshot

  * [![idebar rx controls.png](https://wiki.freepascal.org/images/f/f5/idebar_rx_controls.png)](</File:idebar_rx_controls.png>)



  


  * [![idebar rx dbaware.png](https://wiki.freepascal.org/images/a/ac/idebar_rx_dbaware.png)](</File:idebar_rx_dbaware.png>)



  


  * [![idebar rx tools.png](https://wiki.freepascal.org/images/f/fb/idebar_rx_tools.png)](</File:idebar_rx_tools.png>)



## Download

The last officially released package can be downloaded from the [Lazarus CCR SourceForge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=187197>). 

Note, however, that this version is quite outdated. A newer and maintained version is available via the [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") which is available in newer versions of Lazarus. 

## SVN

The component is available on Lazarus CCR's svn repository: 
    
    
     svn co <https://svn.code.sf.net/p/lazarus-ccr/svn/components/rx>
    

## Dependencies / System Requirements

Tested on Linux. Changes to be made: 

  * Canvas.BrushCopy is used. You should replace this with a call to Canvas.CopyRect.
  * Add Types to the uses in rxdbgrid.pas and rxtoolbar.pas



An updated version will be posted as soon as possible. 

## Installation

  * Place sources in any directory.
  * Go to menu "Packages" -> "Open Package File (*.lpk)". Navigate to the folder containing the rx sources.
  * At first open package "rxtools.lpk". Compile.
  * Then open rxnew.lpk. Compile the component to verify that everything is ok. Install and let Lazarus rebuild.
  * There are several addon packages. Install them only if you need them. They depend on other packages which must be installed first.



## Note

If someone knows how the authors can be contacted, please tell us. My russian is not too good.

---

_Source: [https://wiki.freepascal.org/RXfpc](https://web.archive.org/web/20250323152634/https://wiki.freepascal.org/RXfpc)_
