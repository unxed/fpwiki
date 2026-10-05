# Functions for descriptive statistics

[![fpc source logo.png](https://wiki.freepascal.org/images/e/e1/fpc_source_logo.png)](</File:fpc_source_logo.png>)

│ **English (en)** │


Descriptive statistics aim at characterising empirical data by summative parameters (and also by tables and plots}. 

## Contents

  * 1 Standard functions defined in math unit
  * 2 Standard functions defined in other units
  * 3 Custom functions
    * 3.1 Median
    * 3.2 Standard error of the mean
    * 3.3 Coefficient of variation



## Standard functions defined in math unit

The unit [math](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/index.html> "doc:rtl/math/index.html") of the [RTL](<RTL.md> "RTL") provides a plethora of routines for descriptive statistics. 

  * `[mean](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/mean.html> "doc:rtl/math/mean.html")`: Returns the mean value of an array. 
  * `[ stddev](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/stddev.html> "doc:rtl/math/stddev.html")`: Returns the (sample) standard deviation of an array. 
  * `[ popnstddev](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/popnstddev.html> "doc:rtl/math/popnstddev.html")`: Returns the (population) standard deviation of an array. 
  * `[ meanandstddev](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/meanandstddev.html> "doc:rtl/math/meanandstddev.html")`: Returns mean and standard deviation of an array. 
  * `[ momentskewkurtosis](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/momentskewkurtosis.html> "doc:rtl/math/momentskewkurtosis.html")`: Returns the first four moments of an array. 
  * `[ variance](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/variance.html> "doc:rtl/math/variance.html")`: Returns the (sample) variance of an array. 
  * `[ popnvariance](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/popnvariance.html> "doc:rtl/math/popnvariance.html")`: Returns the (population) variance of an array. 
  * `[ totalvariance](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/totalvariance.html> "doc:rtl/math/totalvariance.html")`: Returns the total variance of an array. 
  * `[ sumofsquares](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/sumofsquares.html> "doc:rtl/math/sumofsquares.html")`: Returns the sum of squares of an array. 
  * `[ sum](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/sum.html> "doc:rtl/math/sum.html")`: Returns the sum of values of an array. 
  * `[ sumsandsquares](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/sumsandsquares.html> "doc:rtl/math/sumsandsquares.html")`: Returns sum and sum of squares of the values in an array. 



These functions expect an array of predefined length (e.g. `array[1..100] of float`) or a 0-based open array (e.g. `array of extended`) with subsequent call of the `[ SetLength](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/setlength.html> "doc:rtl/system/setlength.html")` procedure. 

## Standard functions defined in other units

  * [ length](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/length.html> "doc:rtl/system/length.html"): Delivers the length (n) of an array. 



## Custom functions

Some functions aren't defined in the RTL. The subsequent section lists source code of some commonly used measures for centrality and dispersion. Where not otherwise specified the code is provided with a BSD license. 

Together with other useful statistical code expanded and thoroughly tested versions of these functions are also available in the [QUANTUM SALIS](<http://quantum-salis.sf.net/>) project. 

  


### Median

The term _median_ denotes the 50% quantile of a sample, i.e. the value separating the higher half of a data vector from its lower half. It can be calculated with: 
    
    
    type
      TExtArray = array of Extended;
     
    function SortExtArray(const data: TExtArray): TExtArray;
    { Based on Shell Sort - avoiding recursion allows for sorting of very
      large arrays, too }
    var
      data2: TExtArray;
      arrayLength, i, j, k: longint;
      h: extended;
    begin
      arrayLength := high(data);
      data2 := copy(data, 0, arrayLength + 1);
      k := arrayLength div 2;
      while k > 0 do
      begin
        for i := 0 to arrayLength - k do
        begin
          j := i;
          while (j >= 0) and (data2[j] > data2[j + k]) do
          begin
            h := data2[j];
            data2[j] := data2[j + k];
            data2[j + k] := h;
            if j > k then
              dec(j, k)
            else
              j := 0;
          end;
        end;
        k := k div 2
      end;
      result := data2;
    end;       
     
    function median(const data: TExtArray): extended;
    var
      centralElement: integer;
      sortedData: TExtArray;
    begin
      sortedData := SortExtArray(data);
      centralElement := length(sortedData) div 2;
      if odd(length(sortedData)) then
        result := sortedData[centralElement]
      else
        result := (sortedData[centralElement - 1] + sortedData[centralElement]) / 2;
    end;

Of course, the function **SortExtArray** may be replaced with another sorting algorithm, e.g. QuickSort. The Shell Sort algorithm presented here has the advantage that it is able to sort very large vectors even on machines with a very small amount of memory (albeit with the expense of slightly reduced speed compared to QuickSort). 

### Standard error of the mean

The standard error of the mean (SEM) is a measure that estimates how precisely the true mean of the population can be known. 

Calculation of SEM is simple: 
    
    
    function sem(const data: array of Extended): extended;
    begin
      sem := stddev(data) / sqrt(length(data));
    end;

### Coefficient of variation

The coefficient of variation (CoV or CV), also known as relative standard deviation (RSD), is a measure of dispersion that is standardised with respect to the data's mean. 

It can be calculated with: 
    
    
    function cv(const data: array of Extended): extended;
    { calculates the coefficient of variation (CV or CoV) of a vector of extended }
    begin
      result := stddev(data) / mean(data);
    end;

---

_Source: [https://wiki.freepascal.org/Functions_for_descriptive_statistics](https://web.archive.org/web/20171016004506/https://wiki.freepascal.org/Functions_for_descriptive_statistics)_
