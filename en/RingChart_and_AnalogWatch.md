# RingChart and AnalogWatch

│ **English (en)** │

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The chart components needed by this component have been removed from Lazarus in version 1.4. Since this time this page is outdated. 

  


## Contents

  * 1 About
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 Usage
  * 10 The ringchartwatch Example Application
  * 11 See also



## About

_TRingChart_ component is an elliptic Sectors Chart that we can consider a twin of the LCL TChart Component in the sense that has the same structure, particularly for the data input structure, but the rest is original. 

**What's new?**

  1. it is accurate in the sense that every sector surface is really proportionated to his pie percentage (TCanvas.RadialPie is similar but not exact for area) so if a green sector value is 17.27% of total values the green area is the 17.27% of the total ellipse area;
  2. it allows to include: 
     * label (value and/or percentage)
     * legend (none, inside or outside)
  3. it manage value as double (not only integer)
  4. it allows: 
     * change a single sector (Name or label, Value and color)
     * delete a single sector;
  5. it can appeares as a ring sectorized or a Pie (very rough)



_TAnalogWatch_ is a graphic component that use a timer to show the current time in analog way. Just put it on a form. You can personalize the watch with your fovourites colors. An OnAppointment notify event has been added in the latest version that is triggered when the current time is equal to MeetTime (please read carefully the readme.txt file). From the version 0.3 (thanks to Seth Grover) the Watch has differents "mode": 

  1. "Clock" - the current default mode
  2. "Stopwatch" - behaves like a stopwatch (you set the mode to stopwatch mode and then set StopwatchRunning to true or false to start/stop the stopwatch)
  3. "Defined" mode - simply displays the time you tell it to display with the DefinedTime property.



Furthermore there are some properties that hide/shows thinks like the tick marks, the digits on the clock face, and each hand individually . The zip file contains a Lazarus Package that allows you to easy install the components (just compile and install). Furthermore, there is a simple example that shows how to use the components. 

These components have been tested on win32 and linux (gtk1) but it should be cross-platform. 

## Screenshot

Here are how they appear. RingChart is with these setting: 

  * Legend:=lpOutSide;
  * KindOfLabel:=klBoth;
  * Ring:=true;



[![Ringandwatch.png](https://wiki.freepascal.org/images/c/c0/Ringandwatch.png)](</File:Ringandwatch.png>)

## Author

Salvatore Coppola 

## License

modified LGPL same as Lazarus LCL license (see the COPYING.LGPL and the COPYING.modifiedLGPL that come with this package or the same files that come with the Lazarus distro) 

## Download

The latest release can be found on the [Lazarus CCR Files page](<http://sourceforge.net/projects/lazarus-ccr/files/RingChart%20and%20AnalogWatch/>)

## Change Log

  * Version 0.1 2005/11/22
  * Version 0.2 2006/02/26
  * Version 0.3 2007/04/13



## Dependencies / System Requirements

  * FCL and LCL that come with Lazarus



Status: Beta 

## Installation

  * Unpack the zip file where you want, for instance "mylazarus\mycomponents": you will find "mylazarus\mycomponents\ringwatch" that contain the files package and the example
  * Open lazarus
  * Open the package ringwatch.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install
  * After installation the components will be available in the _Misc_ tab of the IDE.



## Usage

After installed, you will find in 'Misc' tab of the Lazarus Compontens 2 new components: 

_RingChart_ : Drop the components on a form. Change some properties as you like and use this code to fill the Chart (how many sectors you want): 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      RingChart1.Clear;
      RingChart1.AddSector('Red', 25.5, clRed);
      RingChart1.AddSector('Fuchsia', 12.2, clFuchsia);
      RingChart1.AddSector('Green', 15, clGreen);
      RingChart1.AddSector('Yellow', 4, clYellow);
      RingChart1.AddSector('Aqua', 20, clAqua);
      RingChart1.AddSector('Cream', 6.33, clCream);
    end;
    

_AnalogWatch_ : Put it on a form Set colors as you want. If you assign the OnAppointment don't forget to set the MeetTime. You can use the MeetTime property Editor registered in the Lazarus IDE to easy assign his value or simply set it in the OI 

## The ringchartwatch Example Application

The ringchartwatch.lpi example is included in the example directory Install the ringwatch package Before to compile and run it with Lazarus and FPC 

## See also

  * [TAChart](<TAChart.md> "TAChart")

---

_Source: [https://wiki.freepascal.org/RingChart_and_AnalogWatch](https://web.archive.org/web/20250326021228/https://wiki.freepascal.org/RingChart_and_AnalogWatch)_
