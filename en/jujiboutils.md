# jujiboutils

**English (en)** | [**español (es)**](</jujiboutils/es> "jujiboutils/es")   
****

  


## Contents

  * 1 About
  * 2 Authors and contributors
  * 3 License
  * 4 Requirements
  * 5 Download
  * 6 Bug reports
  * 7 Change Log
  * 8 Working system
  * 9 Locate the components and Documentation



## About

Set of components to input and validate different kinds of data, floats, dates, etc. while working with db and non db controls. 

## Authors and contributors

[Julio Jiménez Borreguero](</User:Jujibo> "User:Jujibo") \- jujibo at gmail dot com. 

[Jesús Reyes A.](</User:Jesusrmx> "User:Jesusrmx")

## License

The Jujiboutils Project files are distributed under the GNU Library General Public License (see the file COPYING.LGPL) with the following modification: 

As a special exception, the copyright holders of this library give you permission to link this library with independent modules to produce an executable, regardless of the license terms of these independent modules, and to copy and distribute the resulting executable under terms of your choice, provided that you also meet, for each linked independent module, the terms and conditions of the license of that module. An independent module is a module which is not derived from or based on this library. If you modify this library, you may extend this exception to your version of the library, but you are not obligated to do so. If you do not wish to do so, delete this exception statement from your version. 

## Requirements

Jujiboutils needs Lazarus 1.0 or higher. 

## Download

The latest stable release can be found at: [Lazarus-CCR SourceForge mirror](<http://sourceforge.net/projects/lazarus-ccr/files/Jujiboutils>)

**Latest sources using SVN:**
    
    
      svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/jujiboutils>
    

## Bug reports

Bug reports and feedback: jujibo at gmail dot com 

You also can find me on Freenode IRC channels: #lazarus-es, #lazarus-ide Nick: jujibo 

## Change Log

2013-08-29 jujiboutils 2.0 released 

More details: Read changes.txt file, included in the sources. 

## Working system

The components have been tested using: 

  * Lazarus Linux 32 and Linux 64
  * Lazarus Windows 32
  * [ZeosLib 7 (trunk)](<http://zeos.firmos.at/portal.php>)
  * Sqldb
  * [IBX for Lazarus ](<http://www.mwasoftware.co.uk/index.php?option=com_content&task=view&id=102&Itemid=102>)
  * Memdataset



## Locate the components and Documentation

**Non-DB aware:** [![Additional-light.png](https://wiki.freepascal.org/images/7/72/Additional-light.png)](</File:Additional-light.png>)

**TJLabeledIntegerEdit:** input of integer numbers. Main properties: DisplayFormat, EditLabel.Caption, LabelPosition and Value. DisplayFormat example value: '0 ºC' 

**TJLabeledFloatEdit:** input of float numbers. Main properties: Decimals, DisplayFormat, EditLabel.Caption, LabelPosition and Value. DisplayFormat example value: '#,0.00' 

**TJLabeledCurrencyEdit:** input of currency numbers. Main properties: Decimals, DisplayFormat, EditLabel.Caption, LabelPosition and Value. DisplayFormat default value is '#,0.00' and Decimals=2 

**TJLabeledDateEdit:** input of dates values. Main properties: DisplayFormat, EditLabel.Caption, LabelPosition and Value. 

**TJLabeledTimeEdit:** input of time values. Main properties: DisplayFormat, EditLabel.Caption, LabelPosition and Value. 

**TJLabeledDateTimeEdit:** input of date-time values. Main properties: DisplayFormat, EditLabel.Caption, LabelPosition and Value. The date and time must be separated by a space 

**Widgets in action:** [![Example-nondb.png](https://wiki.freepascal.org/images/6/63/Example-nondb.png)](</File:Example-nondb.png>)

**Notes about Date and time input:**

Valid date separators: **. , -** and **/** ie: 1.1.2011 

Valid time separator: **:** ie: 10:20:45 

In example, date: '01/02/2011' if you input .3, the result date will be '01/03/2011'. So for a system date format 'dd/mm/yyyy' 2. will set the day to two .5 will set the month to May and ..2012 will set the year to 2012. Of course you can supply none, one, two or three values 

You can display the calendar by pressing the button or with the keyboard shortcut: Alt + Down Arrow. 

**Screenshot JLabel and JButton:** [![jbutton.png](https://wiki.freepascal.org/images/7/78/jbutton.png)](</File:jbutton.png>)

  


**DB-aware:**

[![DataControls-light.png](https://wiki.freepascal.org/images/3/32/DataControls-light.png)](</File:DataControls-light.png>)

These components work in the same way that Non-DB aware, except TJDBGridControl. 

**TJDBGridControl:** input of db values in a grid. Auto detects type of data. Specialized cell editors for types: ftString, ftFloat, ftCurrency, ftBCD, ftDate, ftTime and ftDateTime. Special properties in columns like: Decimals (for ftFloat and ftCurrency), MaxLength (for ftString). Automatic decimals/scale detection for ftBCD. Allows null dates, times and date-times (empty values) 

When you edit a field of type Date or Date-Time, you can display the calendar by pressing the keys: Alt + Down Arrow while editing. 

**Widgets in action:** [![Example-db.png](https://wiki.freepascal.org/images/a/a8/Example-db.png)](</File:Example-db.png>)

**TJDBGridControl in action:** [![Example-grid.png](https://wiki.freepascal.org/images/8/86/Example-grid.png)](</File:Example-grid.png>)

---

_Source: [https://wiki.freepascal.org/jujiboutils](https://web.archive.org/web/20250420133457/https://wiki.freepascal.org/jujiboutils)_
