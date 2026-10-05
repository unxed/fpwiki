# Testing FPC

Development of a compiler and a corresponding runtime library requires an extensive amount of tests to ensure that the addition of a new feature or the fixing of a bug doesn't break any existing code. In FPC this task is handled by the testsuite located in the _tests_ subdirectory of the top level directory ([SVN WebView](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/tests/>)). 

## Contents

  * 1 Overview
    * 1.1 Directory Structure
    * 1.2 Test Results
    * 1.3 Test Files
    * 1.4 Test Options
  * 2 Running the testsuite
  * 3 Recommended Workflow



# Overview

The testsuite consists of the makefiles and utilities that execute the tests as well as the tests themselves. For this article the makefiles and utilities will be used as is and thus they won't be looked at any further. 

Detailed information about the testsuite and its parameters can be found in the [ReadMe](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/tests/readme.txt?view=markup>). 

## Directory Structure

The tests themselves are separated into multiple directories: 

  * _test_ : systematic tests, usually developed by test driven development
  * _tbs_ : tests derived from non tracker reports or ideas while fixing something requiring successfull compilation and (optionally) run
  * _tbf_ : tests derived from non tracker reports or ideas while fixing something requiring failing compilation
  * _webtbs_ : tests derived from bug tracker bugs requiring successful compilation and (optionally) run
  * _webtbf_ : tests derived from bug tracker bugs requiring failing of compilation



Additionally selected _tests_ subdirectories of directories in _packages_ are searched. These are specified in _$fpcdir/tests/Makefile.fpc_ with the variable _TESTPACKAGESDIRECTDIRS_ (the directories are simply separated by spaces). You need to regenerate the Makefile using _fpcmake -Tall_ if you change this. 

Not all files in these directories are eligible for the testsuite however as they need to fullfill two formal criteria: 

  * correct prefix (_t_ for _test_ , _tb_ for _tbs_ and _tbf_ , _tw_ for _webtbs_ and _webtbf_)
  * correct file extension (only '.pp' is accepted)



The remaining name of the test is freely chosen, however there are some general guidelines for this as well: 

  * tests in _test_ have some short name describing the feature the tests are testing (e.g. _rhlp_ for record helper tests and _genfunc_ for generic function tests)
  * tests in _tbf_ and _tbs_ have a unique, four digit index
  * tests in _webtbs_ and _webtbf_ have their corresponding bug ID from Mantis as name plus eventually a small letter as suffix if there are multiple tests for the same report
  * units that are used by some test have the prefix _u_ , _ub_ or _uw_ depending on the test directory as well as the name of the first test they are used for (e.g. if both _thlp3.pp_ and _thlp7.pp_ use a unit the unit is named _uhlp3.pp_)



## Test Results

When a test is executed its execution essentially consists of two _stages_ , namely _compilation_ and _run_. The _run_ stage is optional (see Test Options) or not required at all (e.g. if the test should already fail the _compilation_ stage). 

The _compilation_ stage can have the following outcomes: 

  * successful compilation
  * compilation failed with a compiler error
  * compilation failed with an internal error
  * compilation failed with another exception



If the test should succeed then the first is treated as success, the other three as failures. If a test should fail then the second outcome is treated as a success, the other three are treated as failures. 

The _run_ stage can have the following outcomes: 

  * successful run
  * failed run



The result of the run is determined with the exit code of the test's execution. 

## Test Files

A test file is either a program, library or unit file. A library and unit test is never run, only compiled while a program is always run unless told otherwise with the _{%NORUN}_ option. 

## Test Options

Various options can be used to influence the behavior of the testsuite. This options are given at the top of the testfile using a Borland style comment that starts with a _%_. The first comment without that marker ends the option parsing (usually that's the mode switch). 

The following is a non-extensive list of options that are supported: 

  * _NORUN_ : don't execute the resulting test program (useful if merely the compilation should be tested)
  * _SKIP_ : don't compile the test at all
  * _CONFIGFILE_ : Specifies a config file in _$fpcdir/tests/config_ that is to be copied to test. This option can take one or two parameters: in the two parameter form the first name is the source filename and the second the destination filename, in the one parameter form source and destination filename are equal.



For a complete overview of all possible options, see the [ReadMe](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/tests/readme.txt?view=markup>), under the header _Test directives_. 

# Running the testsuite

To run the test suite a complete compilation (compiler, rtl, packages) should have been done beforehand. Then after changing into the _tests_ directory you execute the following command: 
    
    
    make full TEST_FPC=/path/to/ppc TEST_CPU=<cpu> TEST_OS=<os> TEST_OPT=<options> CHUNKSIZE=50 -j <count>
    

  * _TEST_FPC_ : absolute path to the compiler that should be tested (mandatory)
  * _TEST_CPU_ : name of the CPU for which the tests are run (default is the given compiler's default CPU)
  * _TEST_OS_ : name of the OS for which the test should be run (default is the given compiler's default OS)
  * _TEST_OPT_ : options that should be passed to the compiler
  * _CHUNKSIZE_ and _-j <count>_: the testsuite can use multiple cpu cores in parallel. It does so by launching _< count>_ instances of the _dotest_ program, each of which will execute _CHUNKSIZE_ tests before terminating (after which a new instance of _dotest_ will be started to check _CHUNKSIZE_ more tests). We split up the test collection in chunks because not all tests take the same amount of time to compile and run. To get optimal throughput (avoiding some cores sitting idle near the end of the testsuite run), choose a smaller CHUNKSIZE when your number of cores increases. The default, _CHUNKSIZE=100_ , performs fairly well for up to 4 cores. Don't make _CHUNKSIZE_ too small, or the overhead from starting more instances of _dotest_ will erase any gains you may get.



At the end of the testsuite run there'll be an ordered list of failed tests available in _tests/output/ <cpu>-<os>/faillist_ that can be compared with other runs. 

# Recommended Workflow

Before doing any changes: 

  * complete build
  * testsuite run
  * save _faillist_ file (e.g. to _tests/output/unmodified/faillist_)



To test your changes: 

  * complete build (fix any failures that might occur here)
  * testsuite run
  * compare new _faillist_ with stored _faillist_ (due to the tests being ordered a diff or similar is sufficient)
  * fix failures in compiler/RTL/packages
  * rinse and repeat

---

_Source: [https://wiki.freepascal.org/Testing_FPC](https://web.archive.org/web/20250115200533/https://wiki.freepascal.org/Testing_FPC)_
