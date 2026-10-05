# How to start

## Contents

  * 1 How to start
    * 1.1 Setting up the development environment
    * 1.2 Directory layout of the compiler
    * 1.3 Building the compiler
      * 1.3.1 Building a cross compiler
    * 1.4 Testing changes
      * 1.4.1 Running the testsuite
    * 1.5 Recommended readings
  * 2 See also



# How to start

## Setting up the development environment

  1. Install the most recent official FPC release for your operating system, get it [here](<http://www.freepascal.org/sdown.html>)
  2. Get the latest FPC source from the repository, read [here](<http://www.freepascal.org/develop.html>) how to do so.
  3. Test your setup: 
     1. Change to the directory `fpc`
     2. type `make all` and test see if it works without problems. If you've problems with the build, or for a general background info for the process, be sure to check the [buildfaq](<buildfaq.md> "buildfaq").
     3. If you wish to do so, you can install the newly generated compiler with `make install` (This may need sudo).



Now you're ready to modify the compiler. 

## Directory layout of the compiler

The compiler sources are located in fpc/compiler and its subdirectories. 

  * `fpc/` the main directory 
    * `compiler/` files common for all target processors. 
      * The processor specific files are in subdirectories `alpha, arm, powerpc, i386` (i386 only), `ia64, m68k, mips, powerpc, powerpc64, sparc, x86` (x86 general) and `x86-64` (x86-64 only).
      * `systems/` information files for the supported operating systems. 

    The `i_*.pas` files contain the information that is necessary for either hosting or building a compiler compiling for this target.
    The `t_*.pas` files contain the code which is necessary if a compiler is built for compiling for this particular target.
      * `msg/` compiler messages in various languages.
      * `utils/` mainly help programs for compiler development. The are usually not important at the beginning.



## Building the compiler

To build the compiler only, a `make` in fpc/compiler is enough. However, if you want to test the generated compiler and you aren't building a cross compiler, better do a `make cycle`. This tests if the modified compiler can compile itself and the rtl. 

### Building a cross compiler

See [Cross compiling](<Cross_compiling.md> "Cross compiling"). 

## Testing changes

When doing changes, it is always good to test these changes, for example by running the testsuite. 

### Running the testsuite

The readme file in the fpc/tests directory contains more complete test suite instructions. 
    
    
     cd fpc
     make clean all
     cd tests
     make clean all digest
    

You'll get something like this at the end: 
    
    
     ...
     /FPC/home/fpc/bin/digest output/i386-linux/log
     Total = 2975 (28:2947)
     Total number of compilations = 1692 (12:1680)
     Successfully compiled = 1361
     Successfully failed = 319
     Compilation failures = 12
     Compilation that did not fail while they should = 0
     Total number of runs = 1283 (16:1267)
     Successful runs = 1267
     Failed runs = 16
     ...
    

The important line is 
    
    
     Total = 2975 (28:2947)
    

It shows how much tests were run: 2975, how much failed: 28 and now much succeded: 2947. If you want to know what failed, see fpc/tests/output/<target>/longlog and fpc/tests/output/<target>/log. 

## Recommended readings

Unfortunately, there is no documentation about the internals of the 2.x.x compiler yet. However, there is a documentation of the internals of 1.0.x which should give first clues: <http://www.stack.nl/~marcov/comparch.pdf>

Further, some information about the 2.x.x will be collected here: [FPC internals](<FPC_internals.md> "FPC internals")

# See also

  * [Control Pascal Tutorial](<Control_Pascal_Tutorial.md> "Control Pascal Tutorial") \- a simple way to start with no installation needed
  * [Compilers](<Compilers.md> "Compilers") \- another way to start with an installation

---

_Source: [https://wiki.freepascal.org/How_to_start](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/How_to_start)_
