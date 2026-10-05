# Using Pascal Libraries with Java

## Contents

  * 1 Introduction
  * 2 Accessing FPC libraries
  * 3 Dealing with string in Java
  * 4 Dealing with callback procedures
  * 5 Java program example
  * 6 Dealing with threads and synchronize Java method inside threads.
  * 7 The final touch: the Java wrapper class
  * 8 How to run the main Java class



## Introduction

Java cannot access "normal" native libraries. 

## Accessing FPC libraries

The easiest way to use your Pascal code from Java is to run the [pas2jni](<pas2jni.md> "pas2jni") utility which will generate all needed headers and code for you. 

If your wish to know how to do things manually, check the information below. 

To access native libraries, Java needs some more arguments. => **jni.pas** helps to do it... 

You may add 2 more arguments for all your functions/procedures: 

=> **PEnv: PJNIEnv** as first argument and **Obj: JObject** as second argument. 

And you have to export the procedures/functions with Java look. 

Here how may look a fpc library Java compatible: 
    
    
    library Myfpc4JavaLib;
    
    uses
      jni
    
    procedure proc4java(PEnv: PJNIEnv; Obj: JObject); cdecl;
    begin
       /// your code here
    end;
    
    exports
      proc4java name 'Java_Myfpc4JavaLib_proc4java';
    
    begin
    
    end.
    

## Dealing with string in Java

Here how to translate a Java string into a fpc string: 
    
    
    function JStringtoString(PEnv: PJNIEnv; Obj: JObject; JavaStr: JString) : String;
    begin
    result  := (PEnv^^).GetStringUTFChars(PEnv, JavaStr, nil);
     (PEnv^^).ReleaseStringUTFChars(PEnv, JavaStr, nil); // release memory to avoid memory leak
    end;
    

## Dealing with callback procedures
    
    
    library mycallbacklib;
    
    uses
      jni;
    
    ...
    var
    theclass : JClass;  //// => This added to define Java class to use...
    ...
    
    procedure JavaInit( PEnv: PJNIEnv; Obj: JObject ; MClass : Jstring); cdecl; // => to find the Java class used...
    var
    MainClass : Pchar ;
    begin
     MainClass :=  (PEnv^^).GetStringUTFChars(PEnv, MClass, nil); 
     theclass := (PEnv^^).FindClass(PEnv,MainClass) ;
     (PEnv^^).ReleaseStringUTFChars(PEnv, MClass, nil);
    end;  
    
    procedure libcallback(PEnv: PJNIEnv; Obj: JObject; callback : JString); cdecl;
    var
      theproc  : PChar;
      theMethod: JMethodID;
    begin
      theproc := (PEnv^^).GetStringUTFChars(PEnv, callback, nil);
      theMethod := (PEnv^^).GetStaticMethodID(PEnv, theclass, theproc,'()V'); // theclass was defined with Javainit
      (PEnv^^).CallVoidMethod(PEnv,Obj,theMethod) ; // run the callback...
     (PEnv^^).ReleaseStringUTFChars(PEnv, callback, nil); // release memory to avoid memory leak
    end;
    
    exports
      libcallback name 'Java_MyCallback_libcallback', 
      JavaInit name 'Java_MyCallback_javainit';
    
    begin
    end.
    

=> Java side : 
    
    
    public class MyCallback {
      public static void javacallback() {
        System.out.println("Yep, it works...");
      }
      
      public static native void libcallback(String st);
      public static native void javainit(String st); 
     
       
      public static void main(String[] args) {
        System.loadLibrary("mycallbacklib");
        javainit("MyCallback");
        libcallback("javacallback");
      }
    }
    

## Java program example

And, finally, a Java program using fpc native Java library and callbacks: 
    
    
    public class test {
      // The native library declarations. //
      public static native void nativemethod1(String t, String cb);
      public static native void nativemethod2(int i, int j, String cb);
      public static native void nativemethod3(String cb);
      public static native void javainit(String st, String cb); 
      
      static {
        System.loadLibrary("mylib");
      }  
    
      ...
    
      // The callback methods used by library //
      public static void method1() { // callback used by native method1
        nativemethod3();
      } 
      
      public static void method2() { // callback used by native method2
        nativemethod1("Hello");
      } 
      
      public static void method3() { // callback used by native method3
        nativemethod1(1,2);
      } 
    
      ...
    
      // The main application //
      public static void main(String[] args) {
       javainit("test");
        nativemethod1("sometest", "method1");
        nativemethod2(1, 2, "method2");
        nativemethod3("method3");
        ...
      }
    }
    

## Dealing with threads and synchronize Java method inside threads.

It is possible to use threads with your Java compatible fpc library. If you have to synchronize some Java-methods inside the thread, you must call "checksynchronize()" from the main Java class. 

So, you may add one more procedure into your library : 
    
    
    library mysynchrolib;
    
    uses
    ... 
     classes, jni;
    
    ...
    
    procedure checksynchro(PEnv: PJNIEnv; Obj: JObject); cdecl;
    begin
      checksynchronize();
    end;  
    
    exports
    ...
      checksynchro name 'Java_mysynchrolib_checksynchro', 
    ...
    
    begin
    end.
    

And, in main java class, call that checksynchro() procedure, with a Java timer, for example... 

## The final touch: the Java wrapper class

You may create a wrapper class who contents all the declarations of the methods exported by your library. 

Assuming that your fpc library exports the procedures/functions like that: 
    
    
    exports
      nativemethod1 name 'Java_TheWrapLib_nativemethod1', 
      nativemethod1 name 'Java_TheWrapLib_nativemethod2', 
      nativemethod2 name 'Java_TheWrapLib_nativemethod3';
    

Here is the look of a wrapper Java class: 
    
    
    public class TheWrapLib {
    // The native library declarations. //
    public static native void nativemethod1(String t);
    public static native void nativemethod2(int i, int j);
    public static native void nativemethod3();
    static {System.loadLibrary("mylib");}
    ...
    }
    

and here how to use that wrapper in the main Java class: 
    
    
    public class test {
    // The main application //
    public static void main(String[] args)
    {
    TheWrapLib.nativemethod1("test");
    TheWrapLib.nativemethod2(0, 1);
    TheWrapLib.nativemethod3();
    ...
    }
    }
    

## How to run the main Java class

You may run your Java class like this: 

=> if library in same folder than main java class =>
    
    
    java -Djava.library.path=.  myjavaprogram.java
    

=> if library in other folder than main java class =>
    
    
    java -Djava.library.path=/thefolder/iwant/ myjavaprogram.java

---

_Source: [https://wiki.freepascal.org/Using_Pascal_Libraries_with_Java](https://web.archive.org/web/20241202171632/https://wiki.freepascal.org/Using_Pascal_Libraries_with_Java)_
