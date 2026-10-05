# NumLib

## Contents

  * 1 About
  * 2 History
    * 2.1 List of sources
  * 3 Units



## About

NumLib is a numerical package. 

Authors: Kees van Ginneken, Wil Kortsmit and Loek van Reij of the Computational centre of the Eindhoven University of Technology. 

It is preinstalled in Free Pascal in the folder packages/numlib. Ported to FPC: by Marco van de Voort. 

[NumLib Documentation](<NumLib_Documentation.md> "NumLib Documentation")

## History

During the early nineties, I did my numeric math classes using this library. About a decade later (2003?), because of an existing contact with mr Tom Verhoeff, I asked him if the lib still existed. He contacted the original authors, and was able to provide a copy of the manual, and a backup of the authors working directory (the author was retired iirc), as well as verbal permission to release the package under FPC's LGPL license. 

I cleaned up most of the sources in 2003-2004, and in 2005 I got some feedback, patches and help with the documentation from Darius Blaszijk (Jedi-Math). However that stopped, Darius' help was invaluable though, since I received back critical sources from him, after my house was broken into, and my computer was stolen. 

During early 2010 I mentioned the package on the German lazarusforum, and decided to give it a boost. 

Unfortunately I no longer had the scanned copy from the manual, so I'm redoing that work bit by bit (and put it on the FPC server this time) 

### List of sources

I originally received three items: 

  1. a directory with the main sources.
  2. a separate directory with examples, the data file the examples operate on and tex sources.
  3. a printed copy of the manual.



The printed copy is newer than the Tex sources of the manual, but the author didn't have the sources of that revision. The library sources are newer than both the source docs as the printed docs. All documentation is in a very mathematically formal Dutch. 

These three sources will have to be linked and made consistent, and the "tex" document sources needs to be updated to a more modern "latex" dialect, and enhanced/corrected with the (more recent) scans of the printed version. The already finished units can be used as a template. 

When this is finished, a classes based interface could be added. 

## Units

  * det: Determinants for different kinds of matrices (different with respect to symmetry)
  * dsl: Unknown unit. There doesn't exist any documentation for it, it isn't commented, and cannot recognize the algortism directly. Suffixes of the procedures seem to indicate some features of the matrixtype (from unit SLE). Probably some pivot matrix?
  * eig, eigh1, eigh2: Unknown units.
  * int: Integration. This routine is fit for smooth "integrand" so no singularities, sharp edges, or quickly oscillating behaviour.
  * inv: Calculate inverses for different kinds of matrices (different with respect to symmetry).
  * iom: Basic in/output of matrix and vector types. Maybe too simple for your application, but still handy for logging and debugging.
  * ipf: Interpolate and (curve) fitting. Slegpb in this unit patched parameters slightly. Units IPF and SLE were not in the same revision in this NumLib copy (which was a copy of the work directory of the author).
  * mdt: Unit was originally undocumented, but is probably an variant of DET. DET accepts vectors as arguments, while MDT calculates determinants for matrices.
  * numlib: This unit exports all functions in the tpnumlib dll. (A header file more or less). Programs based on this unit don't require the other sources to compile/build, only the DLL, direct.inc and this file are needed.
  * ode: Solve first order starting value differential eqs, and sets of first order starting value differential eqs.
  * omv: Contains some basic matrix operations.
  * roo: Find roots of (various kinds of) equations.
  * sle: Solve set of linear equations of the type Ax=b, for generic, and a variety of special matrices.
  * spe: Special functions. (Currently, most of these functions only work for ArbFloat=Real/Double, not for Extended.
  * spl: Undocumented unit. B- and other Splines. Not imported by the other units.
  * tpnumlib: This "library" imports 119 procedures from the numlib units, and throws them in a dll file. The dll file can be accessed via numlib.pas
  * typ: This is the most basic unit from NumLib. The most important items this unit defines are matrix types and machine constants.

---

_Source: [https://wiki.freepascal.org/numlib](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/numlib)_
