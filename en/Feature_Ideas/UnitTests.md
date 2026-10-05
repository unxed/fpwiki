# Feature Ideas/UnitTests

## Contents

  * 1 Feature idea - Eclipse-style unit tests.
    * 1.1 Main idea
    * 1.2 Introduction and references
    * 1.3 Example: JUnit tests in Eclipse Maven Project
    * 1.4 Conclusion



# Feature idea - Eclipse-style unit tests.

## Main idea

The main idea is to **have ability to write and run Unit tests from a subpackage of main project**. This is to avoid creating a special project specifically for UnitTests. By this way, creation and running of UnitTests will be significantly simplified. 

IMHO, **Unit Tests are very important**. I also consider, that currently UnitTests support in Lazarus is very uncomfortable. And I believe, that it's one of the most important factors, or even **the most important which limits dissemination and distribution of Lazarus and FreePascal**. 

## Introduction and references

I decided to write my feature idea after the following discussions on the forum: 

[[1]](<http://lazarus.freepascal.org/index.php/topic,15681.0.html>) [[2]](<http://lazarus.freepascal.org/index.php/topic,15733.0.html>)

I consider that You are familiar with concept of unit testing. You can read about Unit tests on Wikipedia: [Unit testing on wikipedia](<http://en.wikipedia.org/wiki/Unit_testing>)

If you are not familiar, please read some more articles about software testing in general (for example, [Software Testing on Wikipedia](<http://en.wikipedia.org/wiki/Software_testing>)) and why unit tests are so important and why people need them (e.g., [Purpose of unit testing](<http://simpleprogrammer.com/2010/10/15/the-purpose-of-unit-testing/>)). 

## Example: JUnit tests in Eclipse Maven Project

Let me show, how it is implemented in Eclipse Maven Project (using screenshots from Package Explorer in Eclipse). 

[![EclipseMavenProjectStructure.jpg](https://wiki.freepascal.org/images/d/de/EclipseMavenProjectStructure.jpg)](</File:EclipseMavenProjectStructure.jpg>)

Here is a sample project structure of Eclipse Maven Project. There are several packages in one project. Main program code is located in src/main/java. And Unit tests are located in the same project, but as a separated package - src/test/java. Physically, it's different folders on the filesystem. 

[![EclipseMavenRunAsJUnitTest.jpg](https://wiki.freepascal.org/images/5/57/EclipseMavenRunAsJUnitTest.jpg)](</File:EclipseMavenRunAsJUnitTest.jpg>)

So, it's very easy to create and run UnitTests for an IDE user. All the user need is: 

  * create a new Unit Test class under src/test/java, which will test functionality of one of classes under src/main/java. Both **main classes and Unit tests are in the same project here**
  * write a test
  * right click on individual test or the whole test package and choose Run --> Run as JUnit test
  * test(s) will be runned, results and stack trace (in case of exception) will be presented.



  
**Note: Maven features - not directly relevant to the main feature idea of this page.**

_There are also 2 resource folders - src/main/resources and src/test/resources. This is a cool feature of Maven. For example, if your class uses my_property_file.txt to load properties, you can have 2 files with this filename, but with different content in main/resources and test/resources. Thus, when you will run one of classes (in Java, one project can have several runnable classes, not just 1 program unit; class could be runnable as Application if it has method main()) in the src/main, it will use property file from (will look for my_property_file.txt in) src/main/resources. But, if you run a JUnit test of the same class from src/test, the same class will load properties from (will look for my_property_file.txt in) src/test/resources._

## Conclusion

  * Unit testing is very important concept widely used by all developers.
  * Support for unit testing is important factor for any IDE for any programming language.
  * I suggest that unit tests are implemented very comfortable in Eclipse (especially, when using Maven - e.g., to have different properties while running the main program and testing).
  * I also believe, that if it will be possible **for user** (does not matter how it will be exactly implemented in Lazarus code) to create and run unit tests in the same project with the main program, it could significantly increase popularity of Lazarus. Also it will simplify work of many developers.

---

_Source: [https://wiki.freepascal.org/Feature_Ideas/UnitTests](https://web.archive.org/web/20240226045621/https://wiki.freepascal.org/Feature_Ideas/UnitTests)_
