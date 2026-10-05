# DoDont

│ **English (en)** │

# The right and wrong of Pascal Programming

All of these are almost generic tips for programming.  
I've learned the right and wrong of programming the hard way, by doing things wrong first. 

## Right

  1. Use **checks** (Range, IO, Overflow) when developing. You can enable them with the "-Crio" compiler switch.
  2. Use **[heaptrace](<heaptrc.md> "heaptrc")** from time to time to see if you have any memory leaks. Enable via the "-ghl" compiler switch to get best result.
  3. Split huge [units](<Glossary.md> "Glossary"). Big units are **almost always** a sign of bad design or logic. (you know it's bad when you have unrelated constructs in one unit).
  4. Split huge [functions](<Function.md> "Function") / [procedures](<Procedure.md> "Procedure") / [methods](<Method.md> "Method"). Big functions/procedures/methods are sign of bad design or logic. A good rule of thumb is to have no function, procedure or method longer than one printed page (about 55 lines) or, even better, one screen if you can. What you can visualize in one screen (or on one printed page, if you use printed listings) you can probably understand better than what you can't see or have to flip from page to page to read.
  5. Name your variables/functions properly lest you damn yourself with unmaintainable code. Be consistent in how you do things.
  6. It is commonplace to use **meaningful identifier names.** While every language advises this, it is a much more common practice in Pascal. However, the variable names I, J and K are commonly used as general purpose FOR control variables and have similar usage for loop counters, especially where the loop is only being used to initialize or display members of arrays. Loop variables must be local anyway.
  7. Hungarian notation (a "prefix code" to indicate what the type of a variable is) is frowned upon (Pascal has strong enough typing) except maybe for components on a Lazarus form, e.g., edsomething for an edit field and labsomething for the corresponding label, etc.
  8. Stick with one indentation style. Change only if you hadn't followed a standard style till then.
  9. The only right language for the job is the one you started the work with. And that's [Pascal](<Pascal.md> "Pascal")!



## Wrong

  1. Put all points from Right section here and negate them.
  2. Have doubts in Pascal.
  3. NEVER use -Ct (stack check) in Win32 with threads! There's a "feature" which can be a problem.
  4. Don't name your arguments with names of fields, even if they are private, e.g., FField and Field in an argument.

---

_Source: [https://wiki.freepascal.org/DoDont](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/DoDont)_
