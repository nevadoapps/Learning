# Object Oriented Programming with C# - Part 01
Table of contents: 
1. Understanding Encapsulation
   - Introducing the C# class type
   - Understanding Constructors
     - The role of the Default Constructors
     - Defining Custom Constructors
     - Constructors can use out parameters starting with C# 7.3
     - Constructor Chaining
     - Understanding the static keyword
     - Defining Pillars of Object Oriented Programming
     - C# Access Modifier (7.2)
     - The First Pillar: C#'s Encapsulation Services
     - Read-Only and Write-Only Properties
     - Mixing Private and Public Get/Set Methods on Properties
2. Understanding Inheritance and Polymorphism
   - The Basic Mechanics of Inheritance
   - Inheritance (The is-a relationship)
   - The Sealed Class
   - Containment/delegation model (The has-a relationship)
3. 

## Understanding Encapsulation

- ## Introducing the C# class type

The most fundamental programming structure is the class type. A class us a user-defined type that is composed of field data (member variables) and members that operate on this data (such as constructors, properties, methods, events, etc.)

Colletively, the set of field data represents the **state* of a class instance (otherwise known as an object).

Example:
```csharp
class Car
{
    //The state of the car
    public string petName;
    public int currSpeed;
}

Car myCar = new Car();
```

Notice that these member variables are declared using the public access modifier. Public members of a class directly accessible once an object of this type has been created. The term **object** is used to describe an instance of a given class  type created using the new keyword.

**Notes:** Field data of a class should seldom (if ever) be defines as public. To preserve the integrity of your state data, it is far better design to define data as private (or possibly protected) and allow controlled access to the data via properties. 

- ## Understanding Constructors
    - ## The role of the Default Constructors

By definition, a default constructor never takes arguments. After allocating the new object into memory, the default constructor ensures that all field data of the class is set to an appropriate default value.

Example:
```csharp
class Car 
{
    public string petName;
    public int currSpeed;

    //Default Constructor
    public Car()
    {
        petName = "Chuck";
        currSpeed = 10;
    }

    public override string ToString()
    {
        return $"PetName is {petName} and its current speed is {currSpeed}";
    }
}
```

- ## Defining Custom Constructors
Classes define additional constructors beyond the default. 
Example:
```csharp
class Car 
{
    public string petName;
    public int currSpeed;

    public Car() 
    {
        petName = "Chuck";
        currSpeed = 10;
    }

    //Method overloading
    public Car(string petname, int currspeed)
    {
        petName = petname;
        currSpeed = currspeed;
    }

    //Method overloading
    public Car(string petname)
    {
        petName = petname;
    }

    //Method overloading
    public Car(int currspeed)
    {
        currSpeed = currspeed;
    }
}
```
Constructors with Out Parameters (7.3)

## Constructors can use out parameters starting with C# 7.3.

Example:
```csharp
public Car(string pn, int cs, out bool inDanger)
{
    petName = pn;
    currSpeed = cn;

    if (cs > 100)
        inDanger = true;
    else
        inDanger = false;
}
```
## Constructor Chaining

Example:
```csharp
public class Car 
{
    public string petName;
    public int currSpeed;

    //Default Constructor
    public Car()
    {

    }

    //Method overloading
    public Car(string petname) : this(petname, 0) { }

    //Method overloading
    public Car(int currspeed) : this(default, currspeed) { }

    public Car(string pn, int cs)
    {
        currSpeed = cs;
        petName = pn;
    }

    public override string ToString()
    {
        return $"The Petname is {petName}, and the current speed is {currSpeed}";
    }
}

```
- ## Understanding the static keyword
A C# class may define any number of static members, which are declared using the **static** keyword. Simply put. static members are items that are deemed to be so commonplace that there is no need to create an instance of the class before invoking the member.

By definition, a utility class ise a class that does not maintain any object-level state and is not created with the new keyword. Rather, a utility class exposes all functionality as class-level (not object level) members.

The **static** keyword can be applied to the following:
    - Data of a class
    - Methods of a class
    - Properties of a class
    - A constructor
    - The entire class definition
    - In conjunction with the C# using keyword

Example: (Static Field Data)
```csharp
public class Car 
{
    public static int maxSpeed = 100;
    public currSpeed;
    public petName;

    public Car()
    {
        
    }

    public override string ToString()
    {
        Console.WriteLine($"Current Speed is {currSpeed}, but MaxSpeed is {maxSpeed}");
    }
}
```
Example: (Static Methods)
```csharp
public class SavingsAccount
{
    public double currBalance;
    public static int tranCount = 0;
    public static double currInterestRate = 0.04;

    public SavingsAccount(double balance)
    {
        currBalance = balance;
        tranCount += 1;
    }

    public static void SetInterestRate(double newRate)
    {
        currInterestRate = newRate;
    }

    public static double GetInterestRate()
    {
        return currInterestRate;
    }

    public double CalculateAdjustedBalance()
    {
        return currBalance * (1 + currInterestRate);
    }
}

public class Program
{
    static void Main(string[] args)
    {
        SavingsAccount s1 = new SavingsAccount(10);
        SavingsAccount s2 = new SavingsAccount(20);

        Console.WriteLine($"Transactions Count: {SavingsAccount.tranCount}");
        Console.WriteLine($"Adjusted Balance: {s1.CalculateAdjustedBalance()}");
    }
}
```
- ## Defining Static Classes
It is also possible to apply the **static** keyword directly on the class level. When a class has been defined as static, it is not creatable using the new keyword, and it can contain only members or data fields marked with the static keyword. 

Example:
```csharp
public static class TimeUtilClass
{
    public static void PrintTime() => Console.WriteLine(DataTime.Now.ToShortTimeString());

    public static void PrintDate() => Console.WriteLine(dateTime.ToDay.ToShorDateStrig());
}
```

- ## Defining the Pillars of Object Oriented Programming
C# is an object-oriented programming language. The four principles of object-oriented programming are:
- Abstraction Modeling the relevant attributes and interactions of entities as classes to define an abstract representation of a system.
- Encapsulation Hiding the internal state and functionality of an object and only allowing access through a public set of functions
- Inheritance Ability to create a new abstractions based on existing abstractions.
- Polymorphism Ability to implement inherited properties or methods in different ways across multiple abstractions.

- ## C# Access Modifier (7.2)
| C# Access Modifier | May Be Applied To | Meaning in Life |
| -- | -- | -- |
| public | Types or type members | Public items have no access restrictions. A public member can be access from an object, as well as any derived class. A public type can be access from other external assemblies.|
| private | Type members or nested types | Private members can be access only by the class (or structure) that defines them. |
| protected | Type members or nested types | Protected items can be used by the class that defines it and any child class. They cannot be access from outside the inheritance chain.|
| internal | Types or type members | Internal items are accessible only within the current assembly. Other assemblies can be explicitly granted permission to see the internal items.|
| protected internal | Type members or nested types | When the protected and internal keywords are combined on an item, the item is accessible within the defining assembly, within the defining class, and by derived classes inside or outside of the defining assembly.|
| private protected (7.2) | Type members or nested types | When the private and protected keywords are combined on an item, the item is accessible within the defining class and by derived classes in the same assembly. |

**Notes:**
- By default, type members are implicitly private, while types are implicitly internal. Thus, the following class definition is automatically set to internal, while the type's default constructor is automatically set to private.
Example:
```csharp
//An internal class with a private default constructor
class Radio
{
    Radio() {}
}

//is identically with the code below

internal class Radio 
{
    private Radio() {}
}
```
- ## The First Pillar: C#'s Encapsulation Services
The concept of encapsulatino revolves around the notion of that an object's data should not be directly accessible from an object instance. Rather class data is defined as private. If the object user wants to alter the state of an object, it does so indirectly using the public members. 

Example:
```csharp
public class Program
{
    internal class Book
    {
        private int numberOfPages;

        public int NumberOfPages
        {
            get { return numberOfPages; }
        }

        private string bookName;

        //Expression-Bodied Members (7.0)
        public string BookName { get => bookName; }

        public Book() { }

        public Book(int numberofpages) => numberOfPages = numberofpages;

        #region Methods
        public void SetBookPages(int pages) => numberOfPages = pages;
        public void SetBookName(string name) => bookName = name;

        public override string ToString()
        {
            return $"The name of the book is {bookName} and it is {numberOfPages} pages";
        }
        #endregion
    }

    static void Main(string[] args)
    {
        Book book = new Book(10);
        book.SetBookName("Seha");
        book.SetBookPages(1000);
        Console.WriteLine(book.ToString());
    }
}
```
- ## Read-Only and Write-Only Properties
    - **Read-Only Properties:**

Example:
```csharp
public class Person
{
    private string _empSSN;

    public string SocialSecurityNumber
    {
        get { return _empSSN; }
    }

    //or
    public string SocialSecurityNumber => _empSSN;

    //or    
    public string SocialSecurityNumber { get => _empSSN; }

    //or
    public string SocialSecurityNumber { get; }
}
```

   - **Write-Only Properties:**

Example:
```csharp
public class Person
{
    public string PhoneNumber { set; }

    //or 
    private string _phoneNumer;
    public string PhoneNumber { 
        set {
            _phoneNumber = value;
        }
    }
}
```
- ## Mixing Private and Public Get/Set Methods on Properties
Example:
```csharp
public string SocialSecurityNumber
{
    get => _empSSN;
    private set => _empSSN = value;
}
```
## 2. Understanding Inheritance and Polymorphism
   - ## The Basic Mechanics of Inheritance
Code reuse comes in two flavors: 
- **Inheritance (the is-a relationship):** It allows you to define a child cass that reuses (inherits), extends, or modifies the behavior of a **parent class**. The class whose members are inherited is called **the base class**. The class that inherits the members of the base class is called **the derived class**.

    **Notes:** 

- Not all members of a base class are inherited by derived (child) classes. The following members are not inherited:
    - Static constructors, which initialize the static data of a class.
    - Instance constructors, which you call to create a new instance of the class. Each class must define its own constructurs.
    - Finalizers, which are called by the runtime's garbage collector to destroy instances of a class.
- While all other members of a base class inherited by derived classes, whether they are visible of not depends on their accessibility. A member's accessibility affects its visibility for derived classes as follows:
    - Private members are visible only in derived classes that are nested in their base class. Otherwise, they are not visible in derived classes. In the following example, **A.B** is nested class that derives from **A**, and **C** derives from A. The private **A._value** field is visible for in A.B. However, if you remove the comments from the **C.GetValue** method an attempt to compile the example, it produces compiler error, CS0122: "A._value is inacsessible due to its protection level."

    Example:

    ```csharp
    public class A
    {
        private int _value = 10;

        public class B : A
        {
            public int GetValue()
            {
                return _value;
            }
        }
    }

    public class C : A
    {
        //    public int GetValue()
        //    {
        //        return _value;
        //    }
    }

    public class AccessExample
    {
        public static void Main(string[] args)
        {
            var b = new A.B();
            Console.WriteLine(b.GetValue());
        }
    }
    // The example displays the following output:
    //       10
    ```

    - **Protected** members are visible only in derived classes.
    - **Internal** members are visible only in derived classes that are located in the same assembly as the base class. They are not visible in derived classes in a different assembly from the base class.
    - **Public** members are visible in derived classes and are part of the derived class' public interface. Public inherited members can be called just as if they are defined in the derived class. In the following example, class **A** defines a method named **Method1**, and a class **B** inherits from class **A**. 

    Example:
    ```csharp
    public class A
    {
        public void Method1()
        {
            // Method implementation.
        }
    }

    public class B : A
    { }

    public class Example
    {
        public static void Main()
        {
            B b = new ();
            b.Method1();
        }
    }
    ```

    Example:
    ```csharp
    public class Program
    {
        static void Main(string[] args)
        {
            var manager = new Manager("X12145665", "Seha Yetginer", true);

            //FullName in Employee Base Class is marked as internal, can be accessed from derived class
            Console.WriteLine($"Manager Full Name: {manager.FullName}");

            //SocialNumber is Employee Base Class is marked as protected, can only be accessed in derived class.
            //So, trying to access protected property from derived class of a base class, gives compiler error.
            Console.WriteLine($"Manager Social Number: {manager.SocialNumber}");

            //IsActive in Employee Base Class is marked as public, can be accessed from everywhere.
            Console.WriteLine($"Manager Is Active: {manager.IsActive}");
        }
    }
    internal class Employee
    {
        protected string SocialNumber { get; set; }
        internal string FullName { get; set; }
        public bool IsActive { get; set; }

        public Employee(string socialnumber, string fullname, bool isactive)
        {
            SocialNumber = socialnumber;
            FullName = fullname;
            IsActive = isactive;
        }
    }

    internal class Manager: Employee
    {
        public Manager() : base(string.Empty, string.Empty, default)
        { }

        public Manager(string socialnumber, string fullname, bool isactive): 
            base(socialnumber, fullname, isactive)
        {

        }

        //SocialNumber in Employee Base Class is marked as protected, can only be accessed in derived class.
        public void ChangeSocialNumber(string socialnumber)
        {
            base.SocialNumber = socialnumber;
        }

        //FullName in Employee Base Class is marked as internal, can be accessed from derived class
        public void ChangeFullName(string fullname)
        { 
            base.FullName = fullname; 
        }

        public bool HasBonus { get; set; }
    }
    ```
    - **The Sealed Class**
    
    A sealed class cannot be extended by other classes. This technique is most often used when you are designing a utility class. However, when building class hieararchies, you might find that a certain branch in the inheritance chain should be **"capped off"**, as it makes no sense to further extend linage.

    Example: 
    ```csharp

    //Error: CS0509: Executive cannot derive from sealed type Manager.
    public class Executive: Manager
    {

    }

    internal sealed class Manager: Employee
    {
        public Manager() : base(string.Empty, string.Empty, default)
        { }

        public Manager(string socialnumber, string fullname, bool isactive): 
            base(socialnumber, fullname, isactive)
        {

        }

        //SocialNumber is Employee Base Class is marked as protected, can only be accessed in derived class.
        public void ChangeSocialNumber(string socialnumber)
        {
            base.SocialNumber = socialnumber;
        }

        //FullName in Employee Base Class is marked as internal, can be accessed from derived class
        public void ChangeFullName(string fullname)
        { 
            base.FullName = fullname; 
        }

        public bool HasBonus { get; set; }
    }
    ```

- **Containment/delegation model (the has-a relationship)**

    Example - 01:
    ```csharp
    class BenefitPackage
    {
        public double ComputePayDeduction()
        {
            return 125.0;
        }
    }

    class Employee 
    {
        protected BenetifPackage EmpBenefits = new BenefitPackage();

        public double GetBenefitsCost() => EmpBenefits.ComputePayDeduction();

        public BenefitPackage Benefits {
            get { return EmpBenefits; }
            set { EmpBenefits = value; }
        }
    }
    ```

    Example - 02:
    ```csharp
    public class Program
    {
        static void Main(string[] args)
        {
            var manager = new Manager("X12145665", "Seha Yetginer", true);

            //FullName in Employee Base Class is marked as internal, can be accessed from derived class
            Console.WriteLine($"Manager Full Name: {manager.FullName}");

            //SocialNumber is Employee Base Class is marked as protected, can only be accessed in derived class.
            //So, trying to access protected property from derived class of a base class, gives compiler error.
            //Console.WriteLine($"Manager Social Number: {manager.SocialNumber}");

            //IsActive in Employee Base Class is marked as public, can be accessed from everywhere.
            Console.WriteLine($"Manager Is Active: {manager.IsActive}");

            manager.AddBenefit("X", 100, 100);
            manager.AddBenefit("Y", 200, 100);
            Console.WriteLine($"Get Benefits:\n{manager.GetBenefits()}");
        }
    }
    internal class Employee
    {
        protected string SocialNumber { get; set; }
        internal string FullName { get; set; }
        public bool IsActive { get; set; }

        public Employee(string socialnumber, string fullname, bool isactive)
        {
            SocialNumber = socialnumber;
            FullName = fullname;
            IsActive = isactive;
        }
    }

    internal sealed class BenefitsPackage
    {
        internal string? BenefitName { get; set; }
        internal double? BenefitValue { get; set; }
        public static double BenefitRatio { get; set; }

        public static void SetBenefitRatio(double ratio)
        {
            BenefitRatio = ratio;
        }

        public BenefitsPackage (string? benefitName, double? benefitValue)
        {
            BenefitName = benefitName;
            BenefitValue = benefitValue;
        }

        public override string ToString()
        {
            return $"Name: {this.BenefitName}, Value: {this.BenefitValue}\n";
        }
    }

    internal class Manager: Employee
    {
        private List<BenefitsPackage> Benefits { get; set; } = new List<BenefitsPackage>();

        public Manager() : base(string.Empty, string.Empty, default)
        { 

        }

        public Manager(string socialnumber, string fullname, bool isactive): 
            base(socialnumber, fullname, isactive)
        {

        }

        public void AddBenefit(string benefitname, double benefitvalue, double benefitratio)
        {
            Benefits.Add(new BenefitsPackage(benefitname, benefitvalue));
            BenefitsPackage.SetBenefitRatio(100);
        }

        public string GetBenefits()
        {
            string retVal = "";

            foreach (var benefit in Benefits)
            {
                retVal += string.Join(",", benefit.ToString());
            }

            return retVal;
        }

        //SocialNumber is Employee Base Class is marked as protected, can only be accessed in derived class.
        public void ChangeSocialNumber(string socialnumber)
        {
            base.SocialNumber = socialnumber;
        }

        //FullName in Employee Base Class is marked as internal, can be accessed from derived class
        public void ChangeFullName(string fullname)
        { 
            base.FullName = fullname; 
        }

        public bool HasBonus { get; set; }
    }
    ```