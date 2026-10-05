# FPSpreadsheet: List of formulas

│ **English (en)** │  **[русский (ru)](<../ru/FPSpreadsheet__List_of_formulas.md>)** │

  


## Contents

  * 1 Introduction
  * 2 Mathematical functions
  * 3 Statistical functions
  * 4 Date/time funtions
  * 5 String functions
  * 6 Logical functions
  * 7 Info functions
  * 8 Lookup/reference functions



## Introduction

This is a list of the formulas supported by [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet"). 

The arguments can be **constants** of the given type, or **cells** containing values of the given type. Similar to the Office applications, type-checking is very relaxed, and data are automatically converted to the required type if possible. 

## Mathematical functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`ABS(num)` | Returns the absolute value of a number | float | 1 | float |   
`ACOS(num)` | Returns the arccosine (in radians) of a number | float (>= -1 and <= +1) | 1 | float |   
`ACOSH(num)` | Returns the inverse hyperbolic cosine of a number | float (>= 1) | 1 | float |   
`ASIN(num)` | Returns the arcsine (in radians) of a number | float (>= -1 and <= +1) | 1 | float |   
`ASINH(num)` | Returns the inverse hyperbolic sine of a number | float | 1 | float |   
`ATAN(num)` | Returns the arctangent (in radians) of a number | float | 1 | float |   
`ATANH(num)` | Returns the inverse hyperbolic tangent of a number | float (> -1 and < +1) | 1 | float |   
`CEILING(num,signif)` | Rounds a number up to next multiple of `signif` | float | 2 | float | sfExcel2   
`COS(num)` | Returns the cosine of an angle (in radians) | float | 1 | float |   
`COSH(num)` | Returns the hyperbolic cosine of a number | float | 1 | float |   
`DEGREES(num)` | Converts an angle from radians to degrees | float | 1 | float | sfExcel2   
`EVEN(num)` | Rounds a pos number up, a neg number down to the next even integer | float | 1 | integer | sfExcel2   
`EXP(num)` | Calculates the exponential function of a number | float | 1 | float |   
`FACT(num)` | Calculates the factorial of a number | integer | 1 | float |   
`FLOOR(num,signif)` | Rounds a number down to next multiple of `signif` | float | 2 | float | sfExcel2   
`INT(num)` | Returns the integer portion of a number, rounds down | float | 1 | integer |   
`LN(num)` | Calculates the natural logarithm of a number | float (> 0) | 1 | float |   
`LOG(num [, base])` | Calculates the logarithm of a number to a specified base.  
`base`, if omitted, is 10. | float (> 0) | 1 or 2 | float   
`LOG10(num)` | Calculates the base-10 logarithm of a number | float (> 0) | 1 | float |   
`ODD(num)` | Rounds a pos number up, a neg number down to the next odd integer | float | 1 | integer | sfExcel2   
`PI()` | Returns the mathematical constant π (3.14159265358979) | none | 0 | float |   
`POWER(num, exponent)` | Returns the result of a number raised to a given power | float | 2 | float | sfExcel2   
`RADIANS(num)` | Converts an angle from degrees to radians | float | 1 or 2 | float | sfExcel2   
`RAND()` | Returns a random number between 0 and 1 | none | 0 | float |   
`ROUND(num, digits)` | Returns a number rounded to a specified number of digits | float, integer | 2 | float |   
`ROUNDDOWN(num, digits)` | Returns a number rounded down to a specified number of digits | float, integer | 2 | float | sfExcel2   
`ROUNDUP(num, digits)` | Returns a number rounded up to a specified number of digits | float, integer | 2 | float | sfExcel2   
`SIGN(num)` | Returns the sign of a number | float | 1 | integer |   
`SIN(num)` | Returns the sine of an angle (in radians) | float | 1 | float |   
`SINH(num)` | Returns the hyperbolic sine of a number | float | 1 | float |   
`TAN(num)` | Returns the tangent of an angle (in radians) | float (<> (integer)*π/2) | 1 | float |   
`TANH(num)` | Returns the hyperbolic tangent of a number | float | 1 | float |   
  
## Statistical functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`AVEDEV(num1 [, num2, ...] )` | Average value of absolute deviations of data from their mean. | float | > 1 | float | sfExcel2   
`AVERAGE(num1 [, num2, ...] )` | Average value of a series of numbers | float | > 1 | float |   
`AVERAGEIF(range, condition [, value_range] )` | Average value of data in value_range if cells in range meet condition | cell ranges (like `A1:D5`)  
`condition` is value, string or cell | 2 or 3 | float | sfExcel2  
sfExcel5  
sfExcel8   
`COUNT(value1 [, value2, ...] )` | Counts cells and arguments containing numbers | any | > 1 | integer |   
`COUNTA(value1 [, value2, ...] )` | Counts the number of non-empty cells and arguments | any | > 1 | integer |   
`COUNTBLANK(range)` | Counts the number of empty cells in a range | cell range  
(like `A1:D5`) | 1 | integer | sfExcel2   
`COUNTIF(range, condition )` | Counts the cells in range which meet condition | cell range (like `A1:D5`)  
`condition` is value, string or cell | 2 | integer | sfExcel2   
`MAX(num1 [, num2, ...] )` | Returns the largest value from the numbers provided | float | > 1 | float |   
`MIN(num1 [, num2, ...] )` | Returns the smallest value from the numbers provided | float | > 1 | float |   
`PRODUCT(num1 [, num2, ...] )` | Calculates the product of the numbers provided | float | > 1 | float |   
`STDEV(num1 [, num2, ...] )` | Returns the standard deviation of a population based on a ample of numbers | float | > 1 | float |   
`STDEVP(num1 [, num2, ...] )` | Returns the standard deviation of a population based on an entire population | float | > 1 | float |   
`SUM(num1 [, num2, ...] )` | Calculates the sum of the numbers provided | float | > 1 | float |   
`SUMIF(range, condition [, value_range] )` | Adds the data in value_range if cells in range meet condition | cell ranges (like `A1:D5`)  
`condition` is value, string or cell | 2 or 3 | float | sfExcel2   
`SUMSQ(num1 [, num2, ...] )` | Returns the sum of the squares of a series of numbers | float | > 1 | float | sfExcel2   
`VAR(num1 [, num2, ...] )` | Returns the variance of a population based on a sample of numbers | float | >1 | float |   
`VARP(num1 [, num2, ...] )` | Returns the variance of a population based on an entire population | float | > 1 | float |   
  
## Date/time funtions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`DATE(year, month, day)` | Calculates a serial date number from year, month and day | integer | 3 | date/time |   
`DATEDIF(start_date, end_date, interval)` | Calculates the difference between two date value based on a given interval | `start_date, end_date`: date/time  
`interval` is a string:  
`Y` = number of years,   
`M` = number of months,   
`D` = number of days | 3 | integer | sfExcel2   
`DATEVALUE(date_string)` | Converts a (date) string to a date/time value. | string | 1 | date/time |   
`DAY(value)` | Extracts the day number (1..31) of a date value. | date/time, number, string | 1 | integer |   
`HOUR(value)` | Extracts the hour (0..23) of a time value. | date/time, number, string | 1 | integer |   
`MINUTE(value)` | Extracts the minute (0..59) of a time value. | date/time, number, string | 1 | integer |   
`MONTH(value)` | Extracts the month number (1..12) of a date value. | date/time, number, string | 1 | integer |   
`NOW()` | Returns the current system date and time.   
Will refresh whenever the worksheet recalculates. | none | 0 | date/time |   
`SECOND(value)` | Extracts the second (0..59) of a time value. | date/time, number, string | 1 | integer |   
`TIME(year, month, day)` | Calculates a date/time value from hours, minutes and seconds | integer | 3 | date/time |   
`TIMEVALUE(time_string)` | Converts a (time) string to a date/time value. | string | 1 | date/time |   
`TODAY()` | Returns the current system date | none | 0 | date/time |   
`WEEKDAY(value [, type])` | Returns a number code for the weekday of a date | `value`: date/time, number, string   
`type=0`: Sunday=1, Saturday=7 (default)  
`type=1`: Monday=1, Sunday=7  
`type=2`: Monday=9, Sunday=6 | 1 or 2 | integer |   
`YEAR(value)` | Extracts the year of a date value. | date/time, number, string | 1 | integer |   
  
## String functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`CHAR(ascii_value)` | Returns the character based on its ASCII value | integer | 1 | string |   
`CODE(text)` | Returns the ASCII code of the first character of a string | string | 1 | integer |   
`CONCATENATE(text1 [, text2, ...] )` | Joins strings together | float | > 1 | string |   
`EXACT(text1, text2)` | Compares two strings (case-sensitive) | float | 2 | boolean |   
`LEFT(text [, count])` | Returns the left-most characters of a string | `text`: string  
`count`: integer (default 1) | 1 or 2 | string |   
`LEN(text)` | Returns the character count of a string | string | 1 | integer |   
`LOWER(text)` | Converts a string to lower-case characters | string | 1 | string |   
`MID(text, start_pos, count)` | Returns part of a string | `text`: string  
`start_pos, count`: integer | 3 | string |   
`REPLACE(text, start_pos, count, new_text)` | Replaces a sequence of characters in a string with another string | `text`: string  
`start_pos, count`: integer  
`new_text`: string | 4 | string |   
`REPT(text, count)` | Repeats a text a specified number of times | `text`: string  
`count`: integer  
| 2 | string |   
`RIGHT(text [, count])` | Returns the right-most characters of a string | `text`: string  
`count`: integer (default 1) | 1 or 2 | string |   
`SUBSTITUTE(text, old_text, new_text [, nth_appearance])` | Replaces part of a string with another string | `text, old_text, new_text`: string  
`nth_appearance`: integer  
(default: replace all) | 3 or 4 | string |   
`TRIM(text)` | Removes leading and trailing spaces from a string | string | 1 | string |   
`UPPER(text)` | Converts a string to upper-case characters | string | 1 | string |   
`VALUE(text)` | Converts a string representing a number to a number | string | 1 | float |   
  
## Logical functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`AND(condition1 [, condition2, ...])` | Calculates the logical AND of several boolean values | boolean | any | boolean |   
`FALSE()` | Returns the boolean value FALSE | none | 0 | boolean |   
`IF(condition, value_true [, value_false])` | Returns `value_true` if `condition` is true,  
or `value_false` (or false) if `condition` is false | `condition`: boolean  
`value_true, value_false`: any type | 2 or 3 | any type |   
`NOT(value)` | Inverts a boolean value | boolean | 1 | boolean |   
`OR(condition1 [, condition2, ...])` | Calculates the logical OR of several boolean values | boolean | any | boolean |   
`TRUE()` | Returns the boolean value TRUE | none | 0 | boolean |   
  
## Info functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`ERROR.TYPE(value)` | Returns the numeric representation of one of the errors in Excel (1 = #NULL!, 2 = #DIV/0!, 3 = #VALUE!, 4 = #REF!, 5 = #NAME?, 6 = #NUM!, #N/A else). | cell | 1 | integer | sfExcel2   
`ISBLANK(value)` | Checks for blank or null values. | any, usually cell | 1 | boolean |   
`ISERR(value)` | Returns TRUE if `value` is an error but not #N/A | any, usually cell | 1 | boolean |   
`ISERROR(value)` | Returns TRUE if `value` is an error value (#N/A, #VALUE!, #REF!, #DIV/0!, #NUM!, #NAME? or #NULL). | any, usually cell | 1 | boolean |   
`ISLOGICAL(value)` | Returns TRUE if `value` is boolean | any, usually cell | 1 | boolean |   
`ISNA(value)` | Returns TRUE if `value` is a #N/A error | any, usually cell | 1 | boolean |   
`ISNONTEXT(value)` | Returns TRUE if `value` is not a string | any, usually cell | 1 | boolean |   
`ISNUMBER(value)` | Returns TRUE if `value` is a number | any, usually cell | 1 | boolean |   
`ISREF(value)` | Returns TRUE if `value` is a cell reference | any, usually cell | 1 | boolean |   
`ISTEXT(value)` | Returns TRUE if `value` is a string | any, usually cell | 1 | boolean |   
  
## Lookup/reference functions

Calling prototye  | Meaning  | Argument types  | Arguments  | Result type  | Not for   
---|---|---|---|---|---  
`ADDRESS(row, col [, ref_type] [, ref_style], [sheet_name])` | Returns a text representation of a cell address   
`ref_type` is the type of reference to use: 1=absolute (default); 2=relative column, absolute row; 3=absolute column, relative row; 4=relative   
`ref_style` if true (default) means: address in A1 dialect, otherwise in R1C1   
`sheet_name=name` of the worksheet  | 3x integer  
boolean   
string | 2 (up to 5) | string | sfExcel2   
`COLUMN( [reference] )` | Returns the (1-based) column number of a cell reference.   
`reference` is a reference to a cell or range of cells. If omitted, it is assumed that the reference is the cell address in which the COLUMN function has been entered. | string | 1 (or 0) | integer |   
`HYPERLINK(link [, display_name])` | Adds a hyperlink | string | 1 or 2 | string (hyperlink) | sfExcel2, sfExcel5   
`INDEX(address, row , [col])` | Returns the cell content at the intersection of the given row and colum of the 2D cell range.   
row and column indices are relative to the cell range (1-based). The last parameter can be omitted when the input array is 1-dimensional (i.e., a single column or row).  | 1-D or 2-D cell range   
integer   
[integer] | 2 or 3 | (cell value)   
`INDIRECT(address)` | Returns cell reference based on address string | string | 1 | cell |   
`MATCH(value, array [, type])` | Searches for a value in a 1-D array and returns the relative position of that item.   
type = 1 (default) finds the largest value <= `value` (assumes an array in ascending order)  
type = -1 finds the smallest value >= `value` (assumes an array in descending order)   
type = 0 finds the first value equal to `value` (no requirement on sort order, array can contain strings with wildcard '?').  | `value`: float or text   
`array`: 1-D cell range (e.g., `A1:A9`, or `A1:G1`)  
`type`: integer | 2 or 3 | integer |   
`ROW( [reference] )` | Returns the (1-based) row number of a cell reference.   
`reference` is a reference to a cell or range of cells. If omitted, it is assumed that the reference is the cell address in which the ROW function has been entered. | string | 1 (or 0) | integer |

---

_Source: [https://wiki.freepascal.org/FPSpreadsheet%3A_List_of_formulas](https://web.archive.org/web/20250418093348/https://wiki.freepascal.org/FPSpreadsheet%3A_List_of_formulas)_
