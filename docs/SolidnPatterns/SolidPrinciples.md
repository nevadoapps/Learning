Table of contents:
- What is SOLID principles?
- (**S**)ingle Responsibility Principle
- (**O**)pen-Closed Principle
- (**L**)iskov Substitution Principle
- (**I**)nterface Segregation Principle
- (**D**)ependency Inversion Principle 

## (**S**)ingle Responsibility Principle

The Single Responsibility Principle in C# states that “Each software module or class should have only one reason to change“. In other words, we can say that each module or class should have only one responsibility to do.

A module or function should be responsbile for a single purpose.

Example:

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using static System.Console;

namespace DotNetDesignPatternDemos.SOLID.SRP
{
  // just stores a couple of journal entries and ways of
  // working with them
  public class Journal
  {
    private readonly List<string> entries = new List<string>();

    private static int count = 0;

    public int AddEntry(string text)
    {
      entries.Add($"{++count}: {text}");
      return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
      entries.RemoveAt(index);
    }

    public override string ToString()
    {
      return string.Join(Environment.NewLine, entries);
    }

    // breaks single responsibility principle
    public void Save(string filename, bool overwrite = false)
    {
      File.WriteAllText(filename, ToString());
    }

    public void Load(string filename)
    {
      
    }

    public void Load(Uri uri)
    {
      
    }
  }

  // handles the responsibility of persisting objects
  public class Persistence
  {
    public void SaveToFile(Journal journal, string filename, bool overwrite = false)
    {
      if (overwrite || !File.Exists(filename))
        File.WriteAllText(filename, journal.ToString());
    }
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var j = new Journal();
      j.AddEntry("I cried today.");
      j.AddEntry("I ate a bug.");
      WriteLine(j);

      var p = new Persistence();
      var filename = @"c:\temp\journal.txt";
      p.SaveToFile(j, filename);
      Process.Start(filename);
    }
  }
}
```

## (**O**)pen-Closed Principle

The Open-Closed Principle is a software design principle that states that software entities (such as classes, modules, functions, etc.) should be open for extension but closed for modification.

In other words, it means that you should design your software in such a way that you can add new functionality to it without changing its existing code. This helps to reduce the risk of introducing bugs or breaking existing functionality when you make changes to your code.

To achieve this in C#, you can use a number of techniques, such as:
- Inheritance: Derive new classes from existing ones and override or extend their behavior as needed.
- Composition: Use object composition to combine smaller, independent objects to create more complex behavior.
- Polymorphism: Use polymorphism to allow different objects to respond to the same method calls in different ways.
- Interfaces: Use interfaces to specify a set of methods that a class must implement, allowing you to extend the behavior of a class without modifying its code.

By following the Open-Closed Principle, you can create flexible, extensible software that is easier to maintain and modify over time.

One way to implement the open-closed principle in C# is to use inheritance and polymorphism. For example, consider a Shape class with a Draw() method that is defined as follows:

```csharp
public abstract class Shape
{
    public abstract void Draw();
}
```
You can then create concrete implementations of the Shape class for different types of shapes, such as Circle, Rectangle, and Triangle, as follows:
```csharp
public class Circle : Shape
{
    public override void Draw()
    {
        // code to draw a circle
    }
}

public class Rectangle : Shape
{
    public override void Draw()
    {
        // code to draw a rectangle
    }
}

public class Triangle : Shape
{
    public override void Draw()
    {
        // code to draw a triangle
    }
}

```
With this design, you can add new shapes simply by creating new classes that inherit from the Shape class and override the Draw() method. You don't have to modify the Shape class itself, which helps to ensure that the class is closed for modification.

Here's an example of how you might use these classes in a program:
```csharp
List<Shape> shapes = new List<Shape>();
shapes.Add(new Circle());
shapes.Add(new Rectangle());
shapes.Add(new Triangle());

foreach (Shape shape in shapes)
{
    shape.Draw();
}
```

In this example, the Draw() method of each shape is called without the need to know the specific type of the shape. This is an example of polymorphism, which allows you to treat objects of different types in a uniform manner.

[Further Reading - Specification Pattern](docs/SolidnPatterns/SpecificationPattern.md)

## (**L**)iskov Substitution Principle

Liskov substitution principle is a principle in object-oriented programming that states that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. In other words, a subclass should be able to extend the functionality of its superclass without changing the behavior of the program that uses the superclass.

To implement Liskov substitution in C#, you can follow these steps:

Define a base class that represents the functionality that you want to extend. This class should contain the methods and properties that the subclass will need to implement.

Create a subclass that extends the base class. The subclass should override the methods and properties of the base class as needed to provide additional functionality.

When using the base class in your program, make sure to use it in a way that is compatible with the subclass. This means that you should not rely on specific implementation details of the base class, but rather use it in a way that is abstracted from the implementation details.

For example, consider a base class Shape that has a method GetArea() to calculate the area of a shape. You could create a subclass Rectangle that extends Shape and overrides GetArea() to calculate the area of a rectangle. When using Shape in your program, you should call GetArea() without knowing or caring whether the object is a Shape or a Rectangle.

Here is an example of how this might look in C#:

```csharp
internal class Solid_LSP
{
    static void Main(string[] args)
    {
        Rectangle shape1 = new Rectangle(4, 10);
        Square shape2 = new Square(5, 5);
        PrintArea(shape1);
        PrintArea(shape2);

    }

    static void PrintArea(Shape shape)
    {
        WriteLine($"The area is : {shape.CalculateArea()}, Type is {shape.GetType().ToString()}");
    }
}

public abstract class Shape
{
    public abstract double CalculateArea();
}

public enum ShapeType
{
    Rectangle,
    Square
}

public class Square: Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public readonly ShapeType ShapeType = ShapeType.Square;

    public Square(double height, double width) 
    { 
        Height= height;
        Width= width;
    }

    public override double CalculateArea()
    {
        return Height * Width;
    }
}

public class Rectangle: Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public readonly ShapeType ShapeType = ShapeType.Rectangle;

    public Rectangle(double width, double height)
    {
        Width= width;
        Height= height;
    }

    public override double CalculateArea()
    {
        return Width * Height;
    }
}
```
In this example, the PrintArea() method can be called with a Shape or a Rectangle object, and it will work correctly in both cases because the subclass Rectangle correctly implements the GetArea() method that is required by the base class Shape. This demonstrates the principle of Liskov substitution, as objects of the subclass can be used in place of objects of the superclass without affecting the correctness of the program.

## (**I**)nterface Segregation Principle

Creating huge interfaces will cause dependencies to occur while you are building concrete object, but these are harmful to the system architecture.

The Interface Segregation Principle (ISP) is a software design principle that states that clients should not be forced to depend on interfaces they do not use. This means that it is better to have multiple smaller, specialized interfaces rather than a single, large, general-purpose interface.

In C#, interfaces are defined using the interface keyword, and a class or struct can implement one or more interfaces. 

For example:
```csharp
public interface IDrawable
{
    void Draw();
}

public class Circle : IDrawable
{
    public void Draw()
    {
        // code to draw a circle
    }
}
```

The Circle class implements the IDrawable interface, which has a single method called Draw. This means that any client that depends on the IDrawable interface can call the Draw method on an object of type Circle.

However, if the IDrawable interface had additional methods that are not relevant to the Circle class, it would violate the Interface Segregation Principle. 

For example:

```csharp
public interface IDrawable
{
    void Draw();
    void Rotate();
    void Resize();
}

public class Circle : IDrawable
{
    public void Draw()
    {
        // code to draw a circle
    }

    // The Circle class must provide implementations for the Rotate and Resize methods,
    // even though they may not be relevant to its functionality.
    public void Rotate()
    {
        // code to rotate a circle (not applicable)
    }

    public void Resize()
    {
        // code to resize a circle (not applicable)
    }
}
```

In this case, the Circle class is forced to implement methods that are not relevant to its functionality, which violates the Interface Segregation Principle. To fix this, the IDrawable interface could be split into two separate interfaces: IDrawable and ITransformable. The Circle class could then implement only the IDrawable interface, and other classes could implement both interfaces if necessary.

```csharp
public interface IDrawable
{
    void Draw();
}

public interface ITransformable
{
    void Rotate();
    void Resize();
}

public class Circle : IDrawable
{
    public void Draw()
    {
        // code to draw a circle
    }
}

public class Rectangle : IDrawable, ITransformable
{
    public void Draw()
    {
        // code to draw a rectangle
    }

    public void Rotate()
    {
        // code to rotate a rectangle
    }

    public void Resize()
    {
        // code to resize a rectangle
    }
}
````

Example - 02:
```csharp
using System;

namespace DotNetDesignPatternDemos.SOLID.InterfaceSegregationPrinciple
{
  public class Document
  {
  }

  public interface IMachine
  {
    void Print(Document d);
    void Fax(Document d);
    void Scan(Document d);
  }

  // ok if you need a multifunction machine
  public class MultiFunctionPrinter : IMachine
  {
    public void Print(Document d)
    {
      //
    }

    public void Fax(Document d)
    {
      //
    }

    public void Scan(Document d)
    {
      //
    }
  }

  public class OldFashionedPrinter : IMachine
  {
    public void Print(Document d)
    {
      // yep
    }

    public void Fax(Document d)
    {
      throw new System.NotImplementedException();
    }

    public void Scan(Document d)
    {
      throw new System.NotImplementedException();
    }
  }

  public interface IPrinter
  {
    void Print(Document d);
  }

  public interface IScanner
  {
    void Scan(Document d);
  }

  public class Printer : IPrinter
  {
    public void Print(Document d)
    {
      
    }
  }

  public class Photocopier : IPrinter, IScanner
  {
    public void Print(Document d)
    {
      throw new System.NotImplementedException();
    }

    public void Scan(Document d)
    {
      throw new System.NotImplementedException();
    }
  }

  public interface IMultiFunctionDevice : IPrinter, IScanner //
  {
    
  }

  public struct MultiFunctionMachine : IMultiFunctionDevice
  {
    // compose this out of several modules
    private IPrinter printer;
    private IScanner scanner;

    public MultiFunctionMachine(IPrinter printer, IScanner scanner)
    {
      if (printer == null)
      {
        throw new ArgumentNullException(paramName: nameof(printer));
      }
      if (scanner == null)
      {
        throw new ArgumentNullException(paramName: nameof(scanner));
      }
      this.printer = printer;
      this.scanner = scanner;
    }

    public void Print(Document d)
    {
      printer.Print(d);
    }

    public void Scan(Document d)
    {
      scanner.Scan(d);
    }
  }
}
```

## (**D**)ependency Inversion Principle

The dependency inversion principle is a software design principle that states that:

- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.

In C#, the dependency inversion principle can be implemented through the use of abstractions, such as interfaces and base classes. These abstractions allow high-level modules to depend on abstractions rather than on concrete implementations of low-level modules.

For example, consider a system that has a class PaymentProcessor that depends on a class CreditCardProcessor to process credit card payments. The PaymentProcessor class has a direct dependency on the CreditCardProcessor class, which violates the dependency inversion principle. To fix this, we can create an abstraction for the payment processing functionality and have both the PaymentProcessor and CreditCardProcessor classes depend on this abstraction. This way, the PaymentProcessor class is no longer directly dependent on the CreditCardProcessor class, and the dependency between the two classes is inverted.

Here is an example of how this can be implemented in C#:

```csharp
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}

public class PaymentProcessor : IPaymentProcessor
{
    private readonly IPaymentProcessor _paymentProcessor;

    public PaymentProcessor(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public void ProcessPayment(decimal amount)
    {
        _paymentProcessor.ProcessPayment(amount);
    }
}

public class CreditCardProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        // Process credit card payment
    }
}
```

In this example, the PaymentProcessor class depends on the IPaymentProcessor interface, and the CreditCardProcessor class implements this interface. This allows the PaymentProcessor class to depend on the abstraction rather than on a concrete implementation of the CreditCardProcessor class.

Example - 02:
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using static System.Console;

namespace DotNetDesignPatternDemos.SOLID.DependencyInversionPrinciple
{
  // hl modules should not depend on low-level; both should depend on abstractions
  // abstractions should not depend on details; details should depend on abstractions

  public enum Relationship
  {
    Parent,
    Child,
    Sibling
  }

  public class Person
  {
    public string Name;
    // public DateTime DateOfBirth;
  }

  public interface IRelationshipBrowser
  {
    IEnumerable<Person> FindAllChildrenOf(string name);
  }

  public class Relationships : IRelationshipBrowser // low-level
  {
    private List<(Person,Relationship,Person)> relations
      = new List<(Person, Relationship, Person)>();

    public void AddParentAndChild(Person parent, Person child)
    {
      relations.Add((parent, Relationship.Parent, child));
      relations.Add((child, Relationship.Child, parent));
    }

    public List<(Person, Relationship, Person)> Relations => relations;

    public IEnumerable<Person> FindAllChildrenOf(string name)
    {
      return relations
        .Where(x => x.Item1.Name == name
                    && x.Item2 == Relationship.Parent).Select(r => r.Item3);
    }
  }

  public class Research
  {
    public Research(Relationships relationships) 
    {
      // high-level: find all of john's children
      //var relations = relationships.Relations;
      //foreach (var r in relations
      //  .Where(x => x.Item1.Name == "John"
      //              && x.Item2 == Relationship.Parent))
      //{
      //  WriteLine($"John has a child called {r.Item3.Name}");
      //}
    }

    public Research(IRelationshipBrowser browser) {
      foreach (var p in browser.FindAllChildrenOf("John"))
      {
        WriteLine($"John has a child called {p.Name}");
      }
    }

    static void Main(string[] args)
    {
      var parent = new Person {Name = "John"};
      var child1 = new Person {Name = "Chris"};
      var child2 = new Person {Name = "Matt"};

      // low-level module
      var relationships = new Relationships();
      relationships.AddParentAndChild(parent, child1);
      relationships.AddParentAndChild(parent, child2);

      new Research((IRelationshipBrowser)relationships);
    }
  }
}
```