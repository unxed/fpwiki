# TFunctionPlot

From Free Pascal wiki

## Contents

  * 1 About
  * 2 Usage
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation

  
---  
  
###  About 

_TFunctionPlot_ is a special function plotting control based on TCustomPlot. 

###  Usage 

Almost the same as TCustomPlot (it can work the such way), but a new event (OnFunctionCall) is defined. You should write a function body in it, like 

  
begin 

y=sin(x); 

end; 

Nothing more. Besides, AutoSizeX means nothing while plotting functions, you should set MinX and MaxX. Property DividingTabs means how many points will be used to tabulate (and then draw) the function. 

###  Author 

Presently Vasily I.Volchenko 

###  License 

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

###  Download 

The latest stable release can be found on __. 

###  Change Log 

  * Version 0.0.4 _21.11.2007_ \- First Lazarus-CCR release (used before in other projects) 



###  Dependencies / System Requirements 

  * RTL, LCL 



Status: _Beta_

  


###  Installation 

  * Download and unpack _plots.zip_
  * Install plots.lpk via Lazarus IDE

---

_Source: [https://wiki.freepascal.org/TFunctionPlot](https://web.archive.org/web/20150421011141/https://wiki.freepascal.org/TFunctionPlot)_
