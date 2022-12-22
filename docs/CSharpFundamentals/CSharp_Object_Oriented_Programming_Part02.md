# Object Oriented Programming with C# - Part 02
Table of contents: 
1. Exception Handling
2. Working with Interfaces
    - Obtaining Interface References: The as Keyword
    - Obtaining Interface References: The is Keyword
3. Interfaces vs. Abstract Base Classes
4. Understanding Object Lifetime

## 1. Exception Handling

Programming with structured exception handling involves the use of four interrelated entities:

- A class type that represents the details of the exception
- A member that throws an instance of the exception class to the caller under the correct circumstances
- A block of code on the caller's side that invokes the exception-prone number
- A block of code on the caller's side that will process (or catch) the exception, should it occur

The C# offers five keyword (try, catch, throw, finally, and when) that allow you throw and handle exception.

Example: (Simple Exception Handling)
```csharp
try 
{
    //Code block in which potentionally an exception needs to be handled.   
}
catch (Exception ex)
{
    //Catch block
}
finally 
{
    //Finally block in which will be executed whether an exception is caught or not.
}
```

- ## The System.Exception Base Class

All exceptions ultimately derive from the System.Exception base class, which in turn derives from System.Object.

- Core Members of the System.Exception Type

    | System.Exception Property | Meaning in Life |
    | -- | -- |
    | Data | This read-only property retrieves a collection of key-value pairs that provide additional, programmer-defined information about the exception. | 
    | InnerException | This read-only property can be used to obtain information about the previous exceptions that caused the current exception to occur. |
    | Message | This read-only property returns the textual description of a given error. |
    | Source | This property gets or sets the name of the assembly, or the project, that threw the current exception. | 
    | StackTrace | This read-only property contains a string that identifies the sequence of calls that triggered the exception. | 
    | TargetSite | This read-only property returns a MethodBase object, which describes numerous details about the method that threw the exception. |

- ## Catch Blocks

A **catch** block can specify the type of exception to catch. The type specification is called an **exception filter**. The exception type should be derived from Exception base class.

Multiple **catch** blocks with different exception classes can be chained together. The **catch** blocks are evaluated from top to botom in your code, but only one **catch** block is executed for each exception that is thrown.

- ## Finally Blocks

A finally block enables you to clean up actions that are performed in a **try** block.  If present, the **finally** block executes last, after the **try** block and any matched **catch** block. A **finally** block always runs, whether an exception is thrown or a **catch** block matching the exception type is found.

The **finally** block can be used to release resource such as file streams, database connections, and graphics handles without waiting for the garbage collector in the runtime to finalize the objects. ([Read - Using statement](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement))

Example:
```csharp
FileStream? file = null;
FileInfo fileinfo = new System.IO.FileInfo("./file.txt");
try
{
    file = fileinfo.OpenWrite();
    file.WriteByte(0xF);
}
finally
{
    // Check for null because OpenWrite might have failed.
    file?.Close();
}
```
- ## Catching multiple exceptions & Exception Filters
```csharp
int GetInt(int[] array, int index)
{
    try
    {
        return array[index];
    }
    catch (IndexOutOfRangeException e) when (index < 0) 
    {
        throw new ArgumentOutOfRangeException(
            "Parameter index cannot be negative.", e);
    }
    catch (IndexOutOfRangeException e)
    {
        throw new ArgumentOutOfRangeException(
            "Parameter index cannot be greater than the array size.", e);
    }
}
```

- ## System-Level Exceptions (System.SystemException)
Exceptions that are thrown by the .Net Core platform are called **system exceptions**.
These exceptions are generally regarded as nonrecoverable, fatal errors. Simply put, when an exception type derives from **System.SystemException**, you are able to determine that the .Net Core runtime is the entity that has thrown the exception, rather than the codebase of the executing application.

- ## Application-Level Exceptions (System.ApplicationException)
The only purpose of **System.ApplicationException** is to identify the source of the error. When you handle an exception deriving from **System.ApplicationException**, you can assume the exception was raised by the codebase of the executing application, rather than by the .Net Core base class libraries or .Net Core runtime engine.

## 2. Working with Interfaces

An interface defines a contract. Any class or struct that implements that contract must provide an implementation of the members defined in the interface. An interface may define a default implementation for members. It may also define static members in order to provide a single implementation for common functionality. 

Beginning C#11, an interface may define **static abstract** or **static virtual** members to declare that an implementing type must provide the declared members. Typically, **static virtual** methods declare that an implementation must define a set of **overloaded operators**.

Example:
```csharp
interface ISampleInterface
{
    void SampleMethod();
}

class ImplementationClass: ISampleInterface
{
    void ISampleInterfaceClass.SampleMethod()
    {

    }

    static void Main()
    {
        //Declare an interface instance
        ISampleInterface obj = new ImplementationClass();
        
        //Call the member
        obj.SampleMethod();
    }
}
```
Example:
```csharp
interface IPoint
{
    // Property signatures:
    int X { get; set; }

    int Y { get; set; }

    double Distance { get; }
}

class Point : IPoint
{
    // Constructor:
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    // Property implementation:
    public int X { get; set; }

    public int Y { get; set; }

    // Property implementation
    public double Distance =>
       Math.Sqrt(X * X + Y * Y);
}

class MainClass
{
    static void PrintPoint(IPoint p)
    {
        Console.WriteLine("x={0}, y={1}", p.X, p.Y);
    }

    static void Main()
    {
        IPoint p = new Point(2, 3);
        Console.Write("My Point: ");
        PrintPoint(p);
    }
}
// Output: My Point: x=2, y=3
```
- **Interface Inheritance**

Interfaces may not contain instance state. While static field are now permitted, instance fields are not permitted in interfaces. **Interface auto-properties** aren't supported in interfaces, as they would implicitly declare a hidden field.  In an interface declaration, the following code doesn't declare an auto-implemented property as it does in a class or struct. Instead, it declares a property that doesn't have a default implementation but must be implemented in any type that implements the interface:

Example:
```csharp
public interface INamed
{
  public string Name {get; set;}
}
```

An interface can inherit from one or more base interfaces. When an interface overrides a method implemented in a base interface, it must use the explicit interface implementation syntax.

When a base type list contains a base class and interfaces, the base class must come first in the list.

A class that implements an interface can explicitly implement members of that interface. An explicitly implemented member can't be accessed through a class instance, but only through an instance of the interface. In addition, default interface members can only be accessed through an instance of the interface.

- **Explicit Interface Implementation**

If a class implements two interfaces that contain a member with the same signature, then implementing that member on the class will cause both interfaces to use that member as their implementation. In the following example, all the calls to Paint invoke the same method. This first sample defines the types:

Example:
```csharp
public interface IControl
{
    void Paint();
}
public interface ISurface
{
    void Paint();
}
public class SampleClass : IControl, ISurface
{
    // Both ISurface.Paint and IControl.Paint call this method.
    public void Paint()
    {
        Console.WriteLine("Paint method in SampleClass");
    }
}

SampleClass sample = new SampleClass();
IControl control = sample;
ISurface surface = sample;

// The following lines all call the same method.
sample.Paint();
control.Paint();
surface.Paint();

// Output:
// Paint method in SampleClass
// Paint method in SampleClass
// Paint method in SampleClass
```

But you might not want the same implementation to be called for both interfaces. To call a different implementation depending on which interface is in use, you can implement an interface member explicitly. An explicit interface implementation is a class member that is only called through the specified interface. Name the class member by prefixing it with the name of the interface and a period. For example:

Example:
```csharp
public class SampleClass : IControl, ISurface
{
    void IControl.Paint()
    {
        System.Console.WriteLine("IControl.Paint");
    }
    void ISurface.Paint()
    {
        System.Console.WriteLine("ISurface.Paint");
    }
}

SampleClass sample = new SampleClass();
IControl control = sample;
ISurface surface = sample;

// The following lines all call the same method.
//sample.Paint(); // Compiler error.
control.Paint();  // Calls IControl.Paint on SampleClass.
surface.Paint();  // Calls ISurface.Paint on SampleClass.

// Output:
// IControl.Paint
// ISurface.Paint
```

You can define an implementation for members declared in an interface. If a class inherits a method implementation from an interface, that method is only accessible through a reference of the interface type. The inherited member doesn't appear as part of the public interface. The following sample defines a default implementation for an interface method:

Example:
```csharp
public interface IControl
{
    void Paint() => Console.WriteLine("Default Paint method");
}
public class SampleClass : IControl
{
    // Paint() is inherited from IControl.
}

var sample = new SampleClass();
//sample.Paint();// "Paint" isn't accessible.
var control = sample as IControl;
control.Paint();
```

- **Obtaining Interface References: The as Keyword**

If the object can be treated as the specified interface, you are returned a reference to the interface in question. If not, you receive a null reference. Therefore, be sure to check against a **null** value before proceeding.

Example:
```csharp
static void Main(string[] args)
{
    Hexagon hex2 = new Hexagon("Peter");
    IPointy itfPt2 = hex as IPoint;

    if (itfPt2 != null)
        Console.WriteLine("Points: {0}", itfPt2.Points);
    else
        Console.WriteLine("Ooops");
}
```
- **Obtaining Interface References: The is keyword (7.0)**

You may also check for an implemented interface using the **is** keyword. If the object inn question is not compatible with the specified interface, you are return the value false. If you supply a variable name in statement, the type is assigned into the variable, eliminating the need to do the type check and perform a cast.

Example:
```csharp
static void Main(string[] args)
{
    if (hex is IPointy hvar)
        Console.WriteLine($"Points:{hvar.Points}");
    else
        Console.WriteLine($"Oops");
}
```

- **Interfaces as Parameters**

The interfaces are valid types, you may construct methods that take interfaces as parameters.

```csharp
public interface IBasePerson
{
    void GetFullName();
    void GetSocialNumber();
}

public class Employee: IBasePerson
{
    private string _department;
    private string _name;
    private string _socialNumber;

    public string Department { get { return _department }; set { _department = value } ; }

    public Employee() {}

    public Employee(string name, string socialNumber)
    {
        _name = name;
        _socialNumber = socialNumber;
    }

    public void GetDepartment()
    {
        Console.WriteLine($"The department of the employee is: {this._department}");
    }

    public void GetFullName()
    {
        Console.WriteLine($"The full name of the employee is: {this._name}");
    }

    public void GetSocialNumber()
    {
        Console.WriteLine($"The social number of the employee is: {this._socialNumber}");
    }
}

public class Manager: Employee, IBasePerson
{
    IList<Employee> _listOfEmployees;

    public Manager() {
        _listOfEmployees = new List<Employee>();
    }

    public Manager(IList<Employee> employees)
    {
        _listOfEmployee = employees;
    }

    public void GetListofEmployees()
    {
        Console.WriteLine($"List of Employees: {string.Join(",\n", _listofEmployees.Name})");
    }
}

public class Program
{
    static void Main(string[] args)
    {

    }
}
```
- **Explicit Interface Implementation**

A class or structure can implement any number of interfaces. Given this, there is always the possibility you might implement interfaces that contain identical member and, there have a name class to contend with.

Example:
```csharp
public interface IDrawToForm
{
    void Draw();
}

public interface IDrawToMemory
{
    void Draw();
}

public interface IDrawToPrinter
{
    void Draw();
}

public class Octagon: IDrawToForm, IDrawToMemory, IDrawToPrinter
{
    void IDrawToForm.Draw()
    {
        Console.WriteLine("Draw to form");
    }    

    void IDrawToMemory.Draw()
    {
        Console.WriteLine("Draw to memory");
    }        

    void IDrawToPrinter.Draw()
    {
        Console.WriteLine("Draw to printer");
    }        
}

static void Main(string[] args)
{
    Octagon oct = new Octagon();

    IDrawToForm itfForm = (IDrawToForm) oct;
    itfForm.Draw();

    //Or
    ((IDrawToForm) oct).Draw();

    //or

    if (oct is IDrawToForm dtm)
        dtm.Draw();
}
```

**Notes:**
-- When using this syntax, you do not supply an access modifier; explicitly implemented members are automatically private.
## 4. Understanding Object Lifetime