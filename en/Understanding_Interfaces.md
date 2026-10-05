# Understanding Interfaces

│ **English (en)** │

## The reason for interfaces

Classes that extend other classes can be called subclasses. For example, the bicycle class could be extended to have a mountain sub-class, and a child's bicycle (Monareta) sub-class. They inherit many of the common functions of the bicycle class, but unique options are added as stabilizer wheels for the child's bike. You can call bicycle methods, knowing that they generally apply to all types of bicycle. 

This is a traditional and standard use for classes, since sub-classes are only variations of a theme. But assuming you had an application where you had bike-class objects, cars, car washes and others and in this application you want each class to have the following method: 
    
    
      function isRecyclable : Boolean;
    

This would let you know if the object contains recyclable materials. You could define a high level class that contained this method, and define all the classes derived from it. Or you could simply add the method to each sub-class. Or I could use interfaces. The interfaces would simply tidy up this situation. Defining an interface that groups these methods together, sets a standard for all classes that implement the interface. They make it easier for the programmer to discipline himself in developing the application; All classes have a set of methods that are identical and the compiler insists that all methods in an interface be implemented. 

Returning to the scenario of the recyclable object, each sub-class inherits common functions from its particular parent class. This describes the basic structure of the sub-class. It also inherits (by implementation) common functions that span all classes. 

## Example of an interface

Like many ideas, an example is the best way to illustrate the concepts. We will do it in stages to keep things as simple as possible. First, let's define a car class: 
    
    
     type
       // We define our class Car
       TCar = class
    
       private
         carMake : String;
         carAge : Byte;
    
       public
         // Car constructor
         constructor Create(Make : String);
       
       published
         // Car Properties
         property Make : String read carMake;
         property Age : Byte read carAge write carAge;
       end;
    

This class is defined by default based on TObject since we do not specify a base class type. This class has two properties, and a constructor, which are shown here: 
    
    
     // Implementation of constructor for the Car class
     TCar.Create(Make : String);
     begin
       // Save the car make and default age
       carMake := Make;
       carAge := 0;
     end;
    

Here we assign the two properties of the car: its make and age. The make is passed as a parameter and we assign a default age of 0. Now we will define another class - a bicycle class: 
    
    
     type
       // We define our class Bicycle
       TBbicycle = class
       
       private
         cycleMale : Boolean;
         cycleAge : Byte ;
       
       published
         // Bicycle Properties
         property isMale: Boolean read cycleIsMale;
         property Age : Byte read cycleAge write cycleAge;
    
       // Bicycle constructor
       constructor Create(isMale : Boolean; Age : Byte);
     end;
    

This class has two properties and a constructor as shown here: 
    
    
      // Constructor implementation for the bicycle class
      constructor TBicycle.Create(isMale : Boolean; Age : Byte);
      begin
        // Save the passed parameters
        CycleIsMale := isMale;
        CycleAge := Age;
      end;
    

This class is a bit different from the car class, enough to see that we might not have based the car on the bicycle or the bicycle on the car. Now we will define an interface that says if a class object is recyclable: 
    
    
      type
        // An interface definition
        IRecyclable = Interface (IInterface)
       
        // A single function that supports the property
        function materialIsRecyclable : Boolean;
    
        // A single property
        property isRecyclable : Boolean read materialIsRecyclable ;
      end;
    

Our interface uses the standard Iinterface definition as the basis. Interface definitions are like class definitions with all abstract elements, so we don't have to declare them as abstract - by default they are. 

This interface adds a Recyclable property to each class that implements it. Each class that implements it will have to be guaranteed to have exactly the same way of asking if it is recyclable. This is the power and benefit of interfaces - uniformity through potentially very different classes. 

Any class can implement as many interfaces as it wants - it can conform to any global standard in effect. Note that we must define the property using a function - we cannot declare a Boolean data field in the interface as long as the interfaces do not contain data. 

Now let's change our classes to support this interface definition: 
    
    
      type
        // We define our class Car
        TCar = class(TInterfacedObject, IRecyclable)
    
        private
          carMake : String;
          carAge : Byte;
          carIsRecyclable : Boolean;  // Added to support IRecyclable
          function materialIsRecyclable : Boolean;  // Added for IRecyclable
    
        public
          // Car constructor
          constructor Create(Make : String);
    
        published
          // Car Properties
          property Make : String read carMake;
          property Age : Byte read carAge write carAge;
          // Added for IRecyclable
          property isRecyclable : Boolean read materialIsRecyclable;
      end;
    

Note that we put the function used by the interface property isRecyclable in the private section - we want the property to be used only by the caller. 

[Author's note: when compiled, the compiler insists on the presence of the materialIsRecyclable function, but not on the most important part - the property. As the author can see, this does not force the predominant interface option - the property!] 

We have now based our class on the TInterfaceObject class, which provides some standard support for the classes that implement interfaces. And we have also based our class on IRecyclable, our new interface. 

But we must also declare the new materialIsRecyclable function: 
    
    
     // Car function required by the isRecyclable attribute
     function TCar.materialIsRecyclable : Boolean;
       begin
         Result : = carIsRecyclable;
       end;
    

And we must not forget to assign this recyclable value. We will do it crudely here in the constructor: 
    
    
      // Constructor implmentation for the car class
      constructor TCar.Create(Make : String);
        begin
        // Save the car brand and assign a default age
        carName : = Make;
        carAge : = 0 ;
        carIsRecyclable : = true;  // Assign that it is recyclable
      end;
    

Wow! But we must do the same for the Bicycle class to show the real effect. We will see the complete code to define and use these classes, which you can copy and paste into the Code Editor: 
    
    
      unit Unit1 ;
    
       {$ mode objfpc} {$ H +}
    
       interface
    
       use
         Classes , SysUtils , FileUtil , Forms , Controls , Graphics , Dialogs ;
    
       type
         // An interface definition
         IRecyclable = Interface(IInterface)
           // A single function that supports the property
           function materialIsRecyclable : Boolean;
           // A single property
           property isRecyclable : Boolean read materialIsRecyclable;
         end;
    
         // We define our Car class
         TCar = class(TInterfacedObject, IRecyclable)
    
           private
             carMake : String ;
             carAge : Byte ;
             carIsRecyclable : Boolean ;
             function materialIsRecyclable : Boolean ;  // Added for IRecyclable
    
           public
             // Car constructor 
             constructor Create(Make : String);
        
           published
             // Car Properties
             property Make : String read carMake;
             property Age : Byte read carAge write carAge;
             // Added for IRecyclable
             property isRecyclable : Boolean read materialIsRecyclable ;
         end;
    
         // We define our Bicycle class
         TBicycle = class(TInterfacedObject, IRecyclable)
      
           private
             cycleIsMale : Boolean;
             cycleAge : Byte;
             function materialIsRecyclable : Boolean;  // Added for IRecyclable
         public
             // Bicycle constructor
             constructor Create(isMale : Boolean; numWheels : Byte);
    
         published
             // Bicycle Properties
             property isMale : Boolean read cycleIsMale;
             property Age : Byte read cycleAge write cycleAge;
             // Added for IRecyclable
             property isRecyclable : Boolean read materialIsRecyclable;
         end;
    
       {TForm1}
    
       TForm1 = class (TForm)
         procedure FormCreate(Sender : TObject);
         private 
           {private declarations}
         public
           {public declarations}
         end;
    
       var
         Form1 : TForm1;
    
       implementation
       
       {$R *.lfm}  
       
       {TForm1}
    
       procedure TForm1.FormCreate(Sender : TObject);
       var
         cycleMother : TBicycle;
         carFather : TCar;
       begin
         // Instance our bicycle and car objects
         cycleMother := TBicycle.Create(false, 36);
         carFather := TCar.Create('Toyota Hilux');
    
         // Ask if each one is recyclable
         if carFather.isRecyclable then
             ShowMessage ('Father's car is recyclable')
         else ShowMessage ('Father's car is not recyclable');
    
         if cycleMother.isRecyclable then
             ShowMessage ('Mother's bicycle is recyclable')
         else ShowMessage ('Mother's bicycle is not recyclable');
       end;
    
       // Implementation constructor for the Car class
       constructor TCar.Create(Make : String);
       begin
         // Save the car make and assign the default age
         carMake := Make;
         carAge := 0 ;
         carIsRecyclable := true // Assign that it is recyclable
       end;
    
       // Car function required for the isRecyclable attribute
       function TCar.materialIsRecyclable : Boolean;
       begin
         Result := carIsRecyclable;
       end;
    
       // Implementation constructor for the bicycle class
       constructor TBicycle.Create(isMale : Boolean; numWheels : Byte);
       begin
         // Save the parameters that were passed
         cycleIsMale : = isMale;
         cycleAge : = Age;
       end;
    
       // Bicycle function required for isRecyclable
       Function TBicycle.materialIsRecyclable : Boolean;
       begin
         // We will assume that only Male bicycles are recyclable
         Result := self.isMale;
       end;
      end.
    

When executing ... the ShowMessage function shows the following: 
    
    
       Father's car is recyclable Mother's bicycle is not recyclable
    

This (originally Spanish) post was taken from an Internet article that I found very interesting to modify and understand the interfaces. It has always been a taboo topic to ask and when they are used, it can give your code more clarity. 

## See also

  * [Interfaces](<Interfaces.md> "Interfaces")
  * [How To Use Interfaces](<How_To_Use_Interfaces.md> "How To Use Interfaces")
  * [Accessing the Interfaces directly](<Accessing_the_Interfaces_directly.md> "Accessing the Interfaces directly")

---

_Source: [https://wiki.freepascal.org/Understanding_Interfaces](https://web.archive.org/web/20250122103506/https://wiki.freepascal.org/Understanding_Interfaces)_
