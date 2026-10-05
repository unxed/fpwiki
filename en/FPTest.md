# FPTest

│ **English (en)** │

FPTest (Free Pascal Testing Framework) is a unit testing framework. It is a fork of the DUnit2 project created by the late Peter McNab, but tweaked specifically for use with the Free Pascal Compiler. 

Many of the items listed below relate to the original DUnit2 project (and now the FPTest project), and how it differs to the Delphi DUnit project. Information on migrating from [fpcunit](<fpcunit.md> "fpcunit") is also presented. 

## Contents

  * 1 Features
  * 2 Current status
  * 3 Get the source code
  * 4 Documentation
  * 5 Test Example
  * 6 Screenshots
  * 7 How does it compare to FPCUnit?
  * 8 See also



## Features

Design aims used during project development, not necessarily in any particular order: 

  * Introduce SetUpOnce and TearDownOnce per TestCase and TestSuite.
  * Improves the speed of test execution.
  * Introduce multiple "Projects" and open the way to run project tests in separate threads.
  * Improve the ease of using FPTest (in part by adding more example documentation).
  * Ensure TTestCase _Constructors_ and _Destructors_ only execute once, not for each contained test method.
  * Provide a non global var means for sharing high level setup data between test units.
  * More accurate test results and with improved feedback.
  * Provide a visual count of detected memory leaks and Check()-less tests.
  * Provide an optional count to identify which Check() failed.
  * Code cleanup, by removing legacy code in FPTest, such as Delphi Win32 and .Net specific code.
  * Text Test Runner for those that like consoles, or for unattended test suite runs on servers.
  * A new GUI Test Runner for [fpGUI](<fpGUI.md> "fpGUI") or LCL based projects.
  * Much more advanced Test Decorator which actually works (FPCUnit's one is totally broken).
  * Registered tests can now be organised much better, building a hierarchy of tests, making it easier to manage (enable, disable, skip tests etc).



## Current status

  * New code remains compatible with existing test suites not relying on user modified DUnit code.
  * If your FPCUnit test projects use the CheckXXX() methods and not the AssertXXX() methods, it should be quite painless to switch from FPCUnit to the FPTest testing framework.
  * New code remains compatible with existing test suites not relying on low level TestFramework functions (e.g. TTestResult).
  * New code introduces a Project and ProjectManager class between the GUI and TextTestRunners to assist the future introduction of threaded unit testing.
  * SetupOnce and TeardownOnce code reduces node and name clutter where test decorators would have previously been required. _Test Decorator in FPCUnit has same design flaws, and doesn't work as expected._
  * New Test Decoration code is easier to comprehend and extend.
  * Execution call depth per TestCase procedure has been reduced in an effort to improve testing efficiency. (Execution path is easier to follow and so eases future code change).
  * Individual test procedure timing is now taken on just the test method and excludes SetUp and TearDown times. The intention being to provide higher timing accuracy for method profiling. Overall Timing still brackets all code.
  * All existing Check procedures are supported. Several new Check procedures have been added to complete the range available.
  * New unit tests have been created to verify all Check() procedures detect pass and fail conditions.
  * All tests run without memory leaks or the need to allow leaks.
  * New self-test unit tests are more comprehensive including checks to ensure setup conditions are properly met before executing the required tests.
  * A Project class now handles registration and execution of grouped TestCases (TestSuites) and TestCases.
  * Counted Tests can now be invoked inside nested Test Decorators to provide 2 or more dimensional test data
  * Non Optimized empty tests still fail.
  * Tests that don't call Check() can be globally armed to fail, i.e. from the GUI.
  * Similarly tests causing memory leaks can optionally be globally armed to fail.
  * Individual tests can optionally override the above GUI settings. (e.g. where a third party component has a known leak)
  * All tests that override GUI settings can optionally be identified after test execution by additional node colours.
  * A count of overridden test failures is included in the execution status display.
  * Overridden tests which would otherwise have failed generate a separate warning count visible in the updated GUI and Text output
  * Test Method names are decoupled from the execution process, allowing post compile information to be displayed at the GUI level.
  * Added XML report generator, supplementing both GUITestRunner and TextTestRunner.
  * Added capability in both GUITestRunner and TextTestRunner to skip (Exclude) individual tests separate from Enabling/Disabling tests.
  * Tool buttons to GUITestRunner to ease control and show at runtime which soft options have been selected.



To facilitate early testing without completely re-developing GUITestsRunner and TextTestRunner an intermediate Proxy unit and a TTestResults replacement has been interposed between the GUI and the new TestFramework. Although the original aim was to eventually completely re-develop the GUI the "Proxy" classes are working very well and may not need further work. 

## Get the source code

The source code is freely available from GitHub using the following command: 
    
    
    git clone git://github.com/graemeg/fptest.git
    

or 
    
    
    git clone https://github.com/graemeg/fptest.git
    

If you don't have [Git](<http://git-scm.com/>) installed, you can always grab a source code tarball (archive) too, using the following URL. 
    
    
    <https://github.com/graemeg/fptest/tarball/master>
    

The FPTest project is under active development, so code and documentation are constantly improved. For this reason I highly recommend you get the source code via git, instead of as a tarball. 

## Documentation

FPTest documentation can be found in the _docs_ directory as HTML files. 

## Test Example

Here is a console sample testing project 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    uses
     Classes,
     TextTestRunner,
     sample_tests;
    
    begin
     // Register all tests
     sample_tests.RegisterTests;
    
     RunRegisteredTests;
    end.
    

and the unit containing the actual tests... 
    
    
    unit sample_tests;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
     TestFramework;  // required for TTestCase and CheckXXX() references
    
    type
     TTestCaseFirst = class(TTestCase)
     published
       procedure TestWarning;
       procedure TestOne;
       procedure TestNoError;
       procedure TestThree;
       procedure TestFour;
     end;
    
    
    procedure RegisterTests;
    
    
    implementation
    
    uses
      SysUtils;
    
    
    { here we register all our test classes }
    procedure RegisterTests;
    begin
      TestFramework.RegisterTest(TTestCaseFirst.Suite);
    end;
    
    { TTestCaseFirst }
    
    procedure TTestCaseFirst.TestWarning;
    begin
      // Do nothing here - should cause a Warning
    end;
    
    procedure TTestCaseFirst.TestOne;
    begin
      Check(1 + 1 = 3, 'Catastrophic arithmetic failure!');
    end;
    
    procedure TTestCaseFirst.TestNoError;
    begin
      Check(1 + 1 = 2, 'Catastrophic arithmetic failure!');
    end;
    
    procedure TTestCaseFirst.TestThree;
    var
      s: string;
    begin
      s := 'hello';
      CheckEquals('Hello', s, 'Failed CheckEquals');
    end;
    
    procedure TTestCaseFirst.TestFour;
    var
      x, y: integer;
    begin
      x := 10;
      y := 0;
      Check(x / y = 0, 'Failed on 1');
    end;
    
    end.
    

## Screenshots

Here is a screenshot of the Text Test Runner output. 

[![Sample output of the console Text Test Runner.](https://wiki.freepascal.org/images/2/22/fptest_console.png)](</File:fptest_console.png> "Sample output of the console Text Test Runner.")

Here is a screenshot of the GUI Test Runner (using the fpGUI Toolkit) to run the tiOPF test suite. 

[![A screenshot of the GUI Test Runner using the fpGUI toolkit.](https://wiki.freepascal.org/images/3/36/fptest_fpgui.png)](</File:fptest_fpgui.png> "A screenshot of the GUI Test Runner using the fpGUI toolkit.")

## How does it compare to FPCUnit?

I ([Graeme Geldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys")) was a long time FPCUnit user. I contributed towards, and helped extend the FPCUnit project. But FPCUnit has some design flaws which makes it mostly useful for smaller projects and less complicated test suites. FPCUnit's test decorators, Setup/Teardown etc is severely broken. The FPTest testing framework overcomes all those issues, adds many more features, is easier to manage your tests, and executes faster that FPCUnit or DUnit equivalent projects. 

The good news is that you are not forced to choose between either testing framework. Years ago I included a DUnit/FPTest compatibility interface to FPCUnit. This gives you a nice upgrade path from FPCUnit to FPTest. So if you design your test suites using the CheckXXX() calls (instead of the AssertXXX() calls), later you can easily switch to the FPTest framework without any need for changing your testing code. 

## See also

  * [fpcunit](<fpcunit.md> "fpcunit") Unit testing framework supplied with FPC

---

_Source: [https://wiki.freepascal.org/FPTest](https://web.archive.org/web/20241209225518/https://wiki.freepascal.org/FPTest)_
