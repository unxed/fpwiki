# LMath

## Contents

  * 1 About
  * 2 Changes from DMath
  * 3 Download
  * 4 See also



## About

LMath is a mathematical library for Free Pascal (FPC) and Lazarus. It's a further development of [DMath](<DMath.md> "DMath") from Jean Debord. It is entirely written in Pascal and does not depend on external libraries. 

LMath provides routines and demo programs for numerical analysis, including mathematical functions, probabilities, matrices, optimization, linear and nonlinear equations, integration, Fast Fourier Transform, random numbers, curve fitting, statistics and graphics. 

Author: Viatcheslav V. Nesterov (glorfin) 

License: LGPL 3.0 

## Changes from DMath

  * Naming of packages and units made more systematic. Now names of all units begin from u (for example, uTypes), while names of all packages begin from lm (for example, lmMathUtil). Hence, you will probably change Requirements in your project inspector.
  * Units uVectorHelper, uVecUtils, uVecFunc and uVecPrn in lmMathUtil package define several handy functions for work with arrays.
  * Unit uMatrix in lmLineAlgebra defines several general operations over vectors and matrices.
  * COBYLA algorithm (Constrained Optimization by Linear Approximation, M. Powell) for tasks of constrained optimization included in lmOptimum package, unit uCobyla.
  * Procedure for constrained non-linear regression ConstrNLFit in the unit uConstrLNFit which uses COBYLA algorithm included in lmRegression package.
  * Procedure LinProgSolve in the unit uLinSimplex, package lmOptimum implements simplex method for solution of linear programming problems.
  * Unit uintPoints in lmGenMath package defines operators over TIntPoint, similar to uRealPoints.
  * Unit uUnitsFormat in lmMathUtils package allows now using of long prefixes (for example, "pico") along with short ("p") ones.



This document describes what is new in LMath compared to DMath: [New in LMath (PDF)](<https://sourceforge.net/projects/lmath-library/files/DOC/LMath05/New_in_LMath.pdf/download>). 

## Download

  * [SourceForge](<https://sourceforge.net/projects/lmath-library>).



## See also

  * [SciTech Portal](<Portal_SciTech.md> "Portal:SciTech")

---

_Source: [https://wiki.freepascal.org/LMath](https://web.archive.org/web/20210121150726/https://wiki.freepascal.org/LMath)_
