# LongTimer

## Contents

  * 1 Summary
  * 2 Download
  * 3 Installation
  * 4 License
  * 5 Support
  * 6 Tips



# Summary

  * TLongTimer is a descendant of TIdleTimer, and keeps all of its properties and methods except the Interval property
  * The interval has been changed into a set of additional properties, that allow for very long intervals - Daily, Weekly and Monthly
  * The intended use is for TrayIcon applications or services like alarms, updaters or notifiers



[![sc longtimer.png](https://wiki.freepascal.org/images/3/39/sc_longtimer.png)](</File:sc_longtimer.png>)

# Download

Download from the [Lazarus CCR here](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/longtimer/>)

# Installation

  * Download the 'longtimer' package from the Lazarus CCR repository and copy the folder to a folder off your lazarus components folder
  * Install the package (open it, Compile then Install it) and TLongTimer will appear on your 'System' components tab
  * Drop onto a form, set properties and you are good to go
  * Opening the demo application will give you a good idea how to set the additional properties and use the component



# License

  * LGPLv2



# Support

  * minesadorada on the Freepascal.org forum
  * Updates will go to the CCR repository when available



# Tips

  * Initially, AutoEnabled=False, so TLongTimer will act much as a regular timer
  * The enumerated properties IntervalType, SampleInterval and WeeklyDay are displayed in the 'correct' order, and you can set them via code by TypeCasting if you don't want to use the lt type literals.



eg. 
    
    
    LongTimer1.WeeklyDay:=TDay(1); // sets the Weekly Day to Tuesday (2nd in the list)
    LongTimer1.SampleInterval:=TSampleInterval(1); // sets the interval sample rate to every 5 minutes (2nd in the list)
    LongTimer1.IntervalType:=TIntervalType(1); // sets the interval type to to Weekly (2nd in the list)
    

\- this is exactly equivalent to: 
    
    
    LongTimer1.WeeklyDay:=lt2Tuesday�; // sets the Weekly Day to Tuesday
    LongTimer1.SampleInterval:=lt2Every5minutes�; // sets the interval sample rate to every 5 minutes
    LongTimer1.IntervalType:=lt2Weekly�; // sets the interval type to to Weekly
    

  * Every time an interval property is changed, the Timer is temporarily suspended if it is already running
  * Number of days in a month is automatically catered for. If you set the MonthlyDate to 31 for instance, in Sept, Apr Jun, Nov it will fire on day 30, and on the last day of February (according to Leap Year)
  * The SampleInterval property determines the trade-off between CPU use and accuracy. EveryMinute is the most accurate, but checks the time/date etc every minute so uses more CPU than Every45minutes. However, when Sample interval is Every45Minutes,the IntervalType is ltDaily and the time is set to 2pm, the event could fire as late as 2:45 (which may be an acceptable option)
  * If you set AutoEnabled to TRUE then the timer will switch on and off according to Idle status, and could use significant CPU time. Note: This could be desired behaviour if you want to be sure LongTimer won't be working when some time-critical operation in your application is in progress. (This is why TLongTimer was descended from TIdleTimer rather than TTimer)
  * You can change SampleInterval and the Interval properties whilst LongTimer is running and it will update
  * The event OnSample is fired according to SampleInterval property



  


* * *

Demo application for TLongTimer 

[![sc longtimerdemo.png](https://wiki.freepascal.org/images/2/2d/sc_longtimerdemo.png)](</File:sc_longtimerdemo.png>)

---

_Source: [https://wiki.freepascal.org/LongTimer](https://web.archive.org/web/20211203172308/https://wiki.freepascal.org/LongTimer)_
