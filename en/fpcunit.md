# fpcunit

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Use in FPC/Lazarus
  * 3 Use
    * 3.1 Setup
    * 3.2 Teardown
    * 3.3 Test decorator: OneTimeSetup and OneTimeTearDown
    * 3.4 Tests
      * 3.4.1 Example test
      * 3.4.2 Test hierarchy
      * 3.4.3 Custom Test Names
  * 4 Automatic modification to the fields
  * 5 Output
    * 5.1 Customising output
      * 5.1.1 Testdbwriter
      * 5.1.2 To do: adapt this; use the new xml unit
  * 6 Alternatives
  * 7 Lazarus
  * 8 Test Insight
    * 8.1 Using Test Insight: setting the unit test project options
    * 8.2 Using _Test insight_ : Changing an existing test project
    * 8.3 Using _Test insight_ : Viewing the results
  * 9 GDB bug/feature
  * 10 See also



## Overview

fpcunit is a unit testing framework a la DUnit/JUnit/[SUnit](<http://www.wikipedia.org/wiki/SUnit> "wikipedia:SUnit"). This allows you to quickly write a set of test for a (logical) unit of code (not necessarily the same as a Pascal unit, though often it is). 

Development methodologies like Test Driven Design use this to make sure you code your expectations/specifications in your unit tests first, then write your main code, then run your tests and improve the code until all tests pass. 

Not only does fpcunit allow you to visually inspect test runs, you can also collect the results systematically (using the XML output), and use that to compare versions for e.g. regression errors (i.e. you run your regresssion tests using the unit test output). 

Screenshot of the GUI test runner: 

[![guitestrunner.png](https://wiki.freepascal.org/images/5/5c/guitestrunner.png)](</File:guitestrunner.png>)

The image shows that out of 10 tests run, 6 tests failed. The _EAssertionFailure_ exceptions indicate the test assertions (see below) were not met - i.e. the test failed. The associated messges indicate the result the test expected and the actual result achieved. 

## Use in FPC/Lazarus

FPCUnit tests are used in the FPC database test framework: [Databases#Running_FPC_database_tests](<Databases.md> "Databases")

There are also tests for the FPC compiler/core packages, but these presumably predate fpcunit and use a simpler approach. 

## Use

It's easiest to use Lazarus to set up a new test project for you. Below are some descriptions of which procedures/methods to use for what purpose. 

### Setup

This procedure is present in all FPCUnit tests. It sets up the test environment before **each** test is run _\- in other words not only before and after the complete test suite is run, but for every test_. You can use this to e.g. fill a database with test data. 

### Teardown

This procedure is present in all FPCUnit tests and is the reverse of Setup. It cleans up the test environment after each test is run. You can use this to e.g. clear test data from a database. 

### Test decorator: OneTimeSetup and OneTimeTearDown

The Setup and Teardown procedures mentioned above are run once per test. You can also run Setup and Teardown procedures once per instance/execution of your test run. 

To do this, use OneTimeSetup and OneTimeTearDown and inherit from the TTestSetup "test decorator" and register it, e.g.: 
    
    
    uses
    ...
    testdecorator
    ...
      TDBBasicsTestSetup = class(TTestSetup)
        protected
          procedure OneTimeSetup; override;
          procedure OneTimeTearDown; override;
        end; 
    ...
    initialization
    // make sure you register your test along with the decorator so it knows to run the setup/teardowns
      RegisterTestDecorator(TDBBasicsTestSetup, TTestDBBasics);
    

### Tests

You write your own tests as published procedures in the test class (private, protected or public will not work). You can use _AssertEquals_ etc to specify what should be tested, and give a suitable message when the test fails. 

If you want to fail a test, you can e.g. use 
    
    
    if 5=0 then //ridiculous example but you understand what I mean. You can use other Assert* procedures to test equality much easier etc.
      AssertTrue('This part of the code should never have been reached in this test.',false);
    

Note: there must be a better way of doing this... 

If the test fails, an EAssertionFailedError will be raised with the message you specify in Assert*. This way, you can add a series of subtests and tell which subtest failed. Note: the test runner will stop at the first assertion failure, so subsequent subtests will not be performed. If you do want to always test everything, split out these subtests in separate test procedures. 

Instead of the Assert* procedures, you can also use the DUnit compatible Check* procedures (e.g. CheckEquals) which give more descriptive error messages in the test results: they include expected and actual values. 

The order in which the tests are run are the order in which they appear in the test class definition. 

#### Example test
    
    
      Ttestexport1 = class(Ttestcase)
    ...
      published
        procedure TestOutput;
    ...
    procedure Ttestexport1.TestOutput;
    const
      OutputFilename='output.csv';
    begin
      TestDataSet.Close;
    
      if FileExists(OutputFilename) then DeleteFile(OutputFileName);
      TestDataset.FileName:=OutputFileName;
      TestDataset.Open;
      // Fill test data
      TestDataset.Append;
      TestDataset.FieldByName('ID').AsInteger := 1;
      // Data with quotes
      TestDataset.FieldByName('NAME').AsString := 'J"T"';
      TestDataset.FieldByName('BIRTHDAY').AsDateTime := ScanDateTime('yyyymmdd', '19761231', 1);
      TestDataset.Post;
    
      TestDataset.Last;
      TestDataset.First;
      TestDataset.First;
      AssertEquals('Number of records in test dataset', 1, TestDataset.RecordCount);
      TestDataset.Close;
    end;
    

#### Test hierarchy

In simple cases, you (or Lazarus) would register all your test cases with calls like: 
    
    
    uses 
    ...
    testregistry
    ...
    initialization
      RegisterTest(Ttestexport1); //you pass the class name to register it for running
    

However, you can also create multiple layers to group your test cases if your project gets big: 
    
    
    initialization
      RegisterTest('WorldDominationApp.ExportCheesePlan',Ttestexport1); //The levels are separated by periods 
      RegisterTest('WorldDominationApp.Obsolete',TtestPinkysBrain1); //another category
    

#### Custom Test Names

By default the class name of your TTestCase descendant is used as Test Suite Name and the method names are used as Test Case names. You can also assign custom names e.g. if you have a class wich will run different tests depending on some other settings. 

The following code will create a test named MyTestName (replacing method name) within MyTestSuiteName (replacing class name). 
    
    
    interface
    
    type
      TMyTestClass = class(TTestCase)
      protected
        // override default test handling by running a published method
        procedure RunTest; override;
        // you don't need any published method here
      end;
    
    implementation
    
    procedure TMyTestClass.RunTest;
    begin
      // don't call inherited method.
      // your test logic here ...
      AssertTrue(False);
    end;
    
    initialization
      RegisterTest('MyTestSuiteName', TMyTestClass.CreateWithName('MyTestName');
    end.
    

## Automatic modification to the fields

If you add any field to a subclass of TTestCase, you should be aware that those fields will be reseted to their default values before the beginning of a test. I do not consider this to be a bug because tests results should be independent: the result of a test should not depend on the result of any other test. I have reproduced this behaviour in both Windows and Linux Mint XFCE. 

If you really need a reliable value for a variable, using _class var_ instead of fields solves the problem. 

While performing further testing of this unit, I found that, in the code below, if I replace _fx:double;_ by _class var fx:double;_ **the test no longer fails!** Declaring one variable a class variable has cleared the problem. This makes me believe that this is a bug. 

Usally, bugs are not written to the documentation, but this one has cost us many hours of lost time during unit testing. Furthermore, before reporting any bug, the expected behaviour must be clearly established. This wiki page must describe the expected behaviour. Is the bug is that the fields are assigned to their default values of 0 or is the bug is that when class var is added, they are no longer assigned to 0? According to this reference <https://sergworks.wordpress.com/2012/08/31/introduction-to-unit-testing-with-lazarus/> , the bug would be that they are cleared to their default values. 
    
    
    unit TestCase1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, fpcunit, testutils, testregistry, Dialogs;
    
    type
    
      TTestCase1= class(TTestCase)
      protected
        fx:double;
        fi:integer;
        procedure SetUp; override;
        procedure TearDown; override;
      published
        procedure One;
        procedure Two;
      end;
    
    implementation
    
    procedure TTestCase1.One;
    begin
      fx:=0.5;
      fi:=3;
      IF fi <> 3 THEN
         Fail('Must be 3.');
    end;
    procedure TTestCase1.Two;
    begin
      ShowMessage('fx='+fx.ToString);
      ShowMessage('fi='+fi.ToString);
    
      IF fi <> 3 THEN    //Will fail, fi =0, it has been automatically cleared even if test 1 has ran before
         Fail('Must be 3.');
    end;
    
    procedure TTestCase1.SetUp;
    begin
    
    end;
    
    procedure TTestCase1.TearDown;
    begin
    
    end;
    
    initialization
    
      RegisterTest(TTestCase1);
    end.
    

  


## Output

The console test runner can output in XML (either original FPCUnit format, or a more advanced DUnit2-like format if you use the _xmltestreport_ unit), plain text and latex (e.g. usable for PDF export) formats. The GUI test runner outputs to XML if needed (using the same _xmltestreport_ XML format). 

### Customising output

You can use your own "listener" that listens to the test results and outputs the test data in whatever way you want. Create a T*Listener that implements the ITestListener interface. It's only 5 required methods to implement. 

In your test runner application (e.g. a copy of fpctestconsole), add a listener object; register that test listener with the testing framework (e.g. your console test runner) via the TestResult.AddListener() call, and it will be fed test results as they happen. 

#### Testdbwriter

An example of a custom listener is the database output writer available at <https://bitbucket.org/reiniero/testdbwriter>. This writer will save all test results to a database, which is optimized for receiving large amounts of test results (handy for using on CI server like Jenkins or for importing/consolidating test results). The mentioned repository contains an example that runs the db test framework test results to (another) database. 

#### To do: adapt this; use the new xml unit

An example of an available extra listener is TXMLResultsWriter in the xmlreporter unit in <fpc>\packages\fcl-fpcunit\src\xmlreporter.pas. 

_todo: actually, dbtestframework seems to use the old xml output method..._ An example of an adapted test runner that uses an extra listener can be found in <fpc>\packages\fcl-db\tests\dbtestframework.pas, which contains this code to output to custom listeners (an XML writer and a digest writer that stuffs the output in a .tar archive, handy to process remotely): 
    
    
    uses
     ...the rest of the units needed for test runners...
     fpcunit,...
     // the units with TXMLResultsWriter and TDigestResultsWriter
     testreport,DigestTestReport
    ...
    Procedure LegacyOutput;
    
    var
      FXMLResultsWriter: TXMLResultsWriter;
      FDigestResultsWriter: TDigestResultsWriter;
      testResult: TTestResult;
    
    begin
      testResult := TTestResult.Create;
      FXMLResultsWriter := TXMLResultsWriter.Create;
      FDigestResultsWriter := TDigestResultsWriter.Create(nil);
      try
        testResult.AddListener(FXMLResultsWriter);
        testResult.AddListener(FDigestResultsWriter);
        // Set some properties specific for this results writer:
        FDigestResultsWriter.Comment:=dbtype;
        FDigestResultsWriter.Category:='DB';
        FDigestResultsWriter.RelSrcDir:='fcl-db';
        //WriteHeader is specific for this listener; it writes the header to an XML file
        //notice that it is not called for FDigestResultsWriter
        FXMLResultsWriter.WriteHeader;
        // This performs the actual test run, and the output will be processed by the listeners:
        GetTestRegistry.Run(testResult);
        // Likewise WriteResult is specific for this listener; it writes 
        FXMLResultsWriter.WriteResult(testResult);
      finally
        testResult.Free;
        FXMLResultsWriter.Free;
        FDigestResultsWriter.Free;
      end;
    end;
    

## Alternatives

  * [DUnit2](<http://www.sourceforge.net/projects/dunit2/>) \- a huge improvement over the original DUnit. Originally written for Delphi only, and which is used by the huge test suite of the [tiOPF](<tiOPF.md> "tiOPF") framework.


  * [FPTest](<FPTest.md> "FPTest") \- a fork of DUnit2, and which is tuned specifically for use with the Free Pascal Compiler.



## Lazarus

Lazarus has the consoletestrunner and GUI test runner units, which can be installed by installing the _FPCUnitTestRunner_ package. This will help you create and run your unit tests using a GUI (or console, if you want to). 

The consoletestrunner is compatible with FPC so you don't need Lazarus to compile it. The Lazarus version is slightly different to the one in FPC (e.g. use of UTF8 output etc). 

The GUI runner is easier to use. 

In the GUI runner, if you want to run all tests, currently you first need to click on a test element before the _Run all tests_ button is activated. 

## Test Insight

The FPCUnit package now also contains support for the _Test Insight_ extension to FPCunit. 

_Test Insight_ is based on a similarly named plugin for Delphi by Stefan Glienke. _Test insight_ implements a HTTP server that listens for test results on a well-known HTTP port. The FPCUnit suite contains a unit that implements a test result listener, and sends the results to the HTTP server. Thus, the test results are displayed directly in the IDE, and clicking on a test and/or an exception in the displayed results will take you directly to the method. 

### Using Test Insight: setting the unit test project options

The FPC unit test project needs to include the _Test insight_ listener to send the results to the Lazarus IDE. 

The FPCUnit project wizard has an option to modify the code so the _Test insight_ framework is used in your test code. 

The option is called _use testinsight to communicate results to the IDE_ , as can be seen in the following screenshot: 

[![testinsight-fpcunitoptions.png](https://wiki.freepascal.org/images/6/65/testinsight-fpcunitoptions.png)](</File:testinsight-fpcunitoptions.png>)

### Using _Test insight_ : Changing an existing test project

If you have an existing project that you would like to be able to use with _Test Insight_ , 2 things must be done with existing project code: 

  1. The _fpcunittestinsight_ unit must be added to the uses clause.
  2. the function _IsTestinsightListening_ must be called.



    

    If it returns True, then the _RunRegisteredTests_ routine must be called.

Both these routines are implemented in the _fpcunittestinsight_ unit. If _IsTestinsightListening_ returns False, then the original code must be executed. The 2 functions to use are declared as follows: 
    
    
    procedure RunRegisteredTests(aConfig : String = ’’; baseUrl: string = DefaultUrl);
    function IsTestinsightListening(aConfig : String = ’’; baseUrl: string = DefaultUrl) : Boolean;
    

The code will look something like this: 
    
    
    begin
      if IsTestinsightListening() then
        RunRegisteredTests(’’,’’)
      else
        begin
        Application := TMyTestRunner.Create(nil);
        Application.Initialize;
        Application.Title := ’FPCUnit Console test runner’;
        Application.Run;
        Application.Free;
        end;
    end.
    

### Using _Test insight_ : Viewing the results

When a FPC unittest project was compiled with _Test Insight_ support, the IDE will attempt to run the project to get a list of tests. When successful, the list of tests will be displayed in the _Test Insight_ window, which you can activate from the _Test Insight_ menu item under the _View_ menu. 

[![testinsight-listing.png](https://wiki.freepascal.org/images/e/ec/testinsight-listing.png)](</File:testinsight-listing.png>)

When running the tests, the results will be displayed in the same window: 

[![testinsight-results.png](https://wiki.freepascal.org/images/b/be/testinsight-results.png)](</File:testinsight-results.png>)

Double-clicking on a test name will take you to the implementation of the test, double clicking on an exception will take you to the location of the exception (if a stack trace is available). 

The buttons in the Test Insight window perform the following actions: 

  * Reset the display so it shows the list of tests (it is fetched again from the program, if possible).
  * Runs the checked tests.
  * Runs the selected test.
  * Goes to the previously failed test.
  * Goes to the next failed test.



## GDB bug/feature

**Note** (September 2012): a bug/undocumented feature in the debugger used by Lazarus/FPC (gdb) means that passing --all as run parameters has no effect. Passing this parameter can be useful when debugging console fpcunit test runners has no effect. Workaround: use -a. See bug [[1]](<http://bugs.freepascal.org/view.php?id=22893>)

## See also

  * <https://www.freepascal.org/~michael/articles/fpcunit/fpcunit.pdf> Covers both FPC and Lazarus use. Explains the various classes involved. Integrating portions of that pdf into this documentation (or vice versa) could be very helpful.
  * <http://sergworks.wordpress.com/2012/08/31/introduction-to-unit-testing-with-lazarus/> Nice article about getting FPCUnit tests up and running in the comfort of your Lazarus IDE
  * <http://www.pp4s.co.uk/main/tu-testing-auto2.html> Example program that demonstrates FPCUnit testing
  * ~~<http://www.win.tue.nl/~mousavi/testing/4.pdf>~~ Presentation on FPCUnit

---

_Source: [https://wiki.freepascal.org/fpcunit](https://web.archive.org/web/20250115083713/https://wiki.freepascal.org/fpcunit)_
