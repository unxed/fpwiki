# Array as list of parameters feature review

## Contents

  * 1 Forward
  * 2 Why Do we need such feature ?
    * 2.1 Example 1
      * 2.1.1 Ideas
      * 2.1.2 Problems
    * 2.2 Example 2
      * 2.2.1 Ideas
      * 2.2.2 Problems
  * 3 Answers



# Forward

This is an attempt to outline the idea behind using an array as a list of values. These values can be added as a list of parameters inside open array/array of const, instead of a variable type of variable, where the type is for example "array of string". 

I will try to write the good and bad regarding this issue and to explain why I think it is a good and very important feature, which FPC could have. 

This description will bring out problems, or a need to resolve something, and will try to explain how this thing can help, or where I can find problems with that feature. 

I hope more people will contribute to this outline, so we could have this feature implemented in the best way we can have. 

# Why Do we need such feature ?

To answer that question, let me give you examples that this feature will solve: 

## Example 1

  * We have an array of values from an SQL result, and we wish to use that array, for example to add and output using the "format" function. At this point, in order to do so, we will have to create our own "format" function that will take the array and replace the string according to the format string rules, so we could go over all of the fields of an array.



### Ideas

  1. This feature will cause the already existed format function to be valid without the need to rewrite it.
  2. This feature will give better design of how and what data types we will be using. Instead of writing loops, and even think of the different ways of design for output.
  3. Instead of rewriting "every"tm function that developers need to deal with array values, while they accept only open array/array of const,
  4. The write/writeln and read/readln procedures are pasado procedures created at compile time by the compiler. This option allow sthe compiler to create such things when developers find themselves need to create their own System unit, or just create pesado procedures just like write/writeln.
  5. This feature will give us debug or just content reading and writing, without any special code written to do so.



### Problems

  1. Is it really needed, or is this request just a wish to create a laziness when handling arrays?
  2. What happens if we need to display fields of a record ?
  3. What if we wish to use am array as a variable that contains a list of values, rather than this idea ?
  4. In what compiler modes should we add such feature ?
  5. What happen if developers wish to use only part of the array instead of the whole cells ?
  6. What happen when we wish to do the same to containers such as TList, or TStringList ?
  7. What happen if the array is array of Class, array of Record, or array of custom type ?
  8. Let's say that this feature exists, what happen to the following example:   
`format(content, [ArrayWithFields, variableA..variableN]);` ?
  9. What happen if we enable this option, and we place such array to a non open array/array of const ?



## Example 2

  * We are reading a text file and add the content to an array. We wish to change all of the occurrences of the term "C" into "Pascal" on all of the lines.   
One way would be to run across the array with nested loops (the first loop will be each "cell", and the second one will start searching for "C" and then call the procedure replaceAll). This is very inefficient way to solve the problem.   
A second way will be to convert that "array of string" into a one big AnsiString, and will call the "replaceall" procedure once, when the real loop will be inside "replaceall".



### Ideas

  1. We can replace all of the occurrences without loosing the structure of such array, and without attempting to move from one format to another.
  2. Unlike most programming languages, we do not need to create different container such as "vectors" in Java in order to store array of string, and if we do not wish to use the Classes unit for TStrings/TList decanted to store such information (for example we can not or do not want to use classes and objects), we still can mimic such a solution.
  3. If we have a need to replace more occurrences we could make it with less time, and might be even with better optimization of the code.



### Problems

  1. The array is still not one variable, and while it seems for the developers that only one loop happens, this does not means that the actual code is translated that way anyhow.
  2. Some (if not all) of the nested for loops can move into recursive functions, and that "might" increase speed.
  3. Memory maps such as mmap might give you better ways to do it without the need for such action.



# Answers

---

_Source: [https://wiki.freepascal.org/Array_as_list_of_parameters_feature_review](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Array_as_list_of_parameters_feature_review)_
