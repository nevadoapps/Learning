# Object Oriented Programming with C# - Part 04

Table of contents:

## Generic Programming
- Creating Custom Generic Methods
- Constraints on type Parameters
- Enum Constraint
- Creating Custom Generic Structures and Classes
- Generics overview
- Pattern Matching with Generics (7.1)

 ## Creating Custom Generic Methods

 A generic method is a method that is declared with the type parameters and optionally applying constraints.

 Example:
 ```csharp

//Non-Generic Method 
//Problem: As you may see below, swap could only be used by int value type.
//Method overloading has to be done if such method would be used for decimal or so forth.    
public void Swap(ref int a, ref int b)
{
    int tmp;
    tmp = b;
    b = a;
    b = tmp;
}

public void Swap(ref decimal a, ref decimal b)
{
    decimal tmp;
    tmp = b;
    b = a;
    b = tmp;
}

//Generic Method
public void Swap<T>(ref T a, ref T b)
{
    T tmp;
    tmp = b;
    b = a;
    a = tmp;
}
 ```

 ## Constraints on type parameters

 Constraints inform the compiler about the capabilities a type arguments must have. Without any constraint, the type argument could be any type. The compiler can only assume the members of System.Object, which is the ultimate base class for any .Net Type.

 | Generic Constraint | Meaning in Life |
 | -- | -- |
 | where **T**: struct | The type parameter **\<T>** must have a System.ValueType in its chain of inheritance (i.e., **T** must be a structure). | 
 | where **T**: class | The type parameter must be a reference type. This constraint applies also to any class, interface, delegate, or array type. In a nullable context, T must be a non-nullable reference type. |
 | where **T**: class? | The type parameter must be a reference type, either nullable or non-nullable. This constraint applies to any class, interface, delegate, or array type. | 
 | where **T**: notnull | The type parameter must be a non-nullable type. The parameter can be a non-nullable reference type of non-nullable value type. |
 | where **T**: new() | The type parameter **\<T>** must have a default constructor. This is helpful if your generic type must create an instance of the type parameter because you cannot assume you know the format of custom constructors. Note that this constraint must be last listed on a multi-constrained type. | 
 | where **T**: **NameOfBaseClass** | The type parameter must be or derive from the specified base class. In a nullable context, T must be a non-nullable reference type derived from the specified base class. | 
 | where **T**: **NameOfBaseClass?** | The type parameter must be or derive from the specified base class. In a nullable context, T may be either a nullable or non-nullable type derived from the specified base class. | 
 | where **T**: **NameOfInterface** | The type argument must be or implement the specified interface. Multiple interface constraints can be specified. The constraining interface can also be generic. In a nullable context, T must be a non-nullable type that implements the specified interface. |
 | where **T**: **NameOfInterface?** | The type argument must be or implement the specified interface. Multiple interface constraints can be specified. The constraining interface can also be generic. In a nullable context, T may be a nullable reference type, a non-nullable reference type, or a value type. T may not be a nullable value type. |
 | where **T**: U | The type argument supplied for T must be or derive from the argument supplied for U. In a nullable context, if U is a non-nullable reference type, T must be non-nullable reference type. If U is a nullable reference type, T may be either nullable or non-nullable. |

 Example:
 ```csharp
public class Employee 
{
    public Employee(string name, int id) => (Name, ID) = (name, id);
    public string Name { get; set; }
    public int ID { get; set; }
}

public class GenericList<T> where T: Employee
{
    //Implementation
}

public class EmployeeList<T> where T: Employee, IEmployee, System.IComparable<T>, new()
{
    //Implementation
}
 ```

 **Notes:**
 - When applying the **where T: class** constraint, avoid **==** and **!=** operators on the type parameter because these operators will test for reference identity only, not for value equality. This behavior occurs even if these operators are overloaded in a type that is used as an argument. 

 Example:
 ```csharp
public static void OpEqualsTest<T>(T s, T t) where T : class
{
    System.Console.WriteLine(s == t);
}

private static void TestStringEquality()
{
    string s1 = "target";
    System.Text.StringBuilder sb = new System.Text.StringBuilder("target");
    string s2 = sb.ToString();
    OpEqualsTest<string>(s1, s2);
}
 ```
The compiler only knows that T is a reference type at compile time and must use the default operators that are valid for all reference types. If you must test for value equality, the recommended way is to also apply the where T : IEquatable<T> or where T : IComparable<T> constraint and implement the interface in any class that will be used to construct the generic class.

- **Enum Constraint**

You can also specify the System.Enum type as a base class constraint. The CLR always allowed this constraint, but the C# language disallowed it. Generics using System.Enum provide type-safe programming to cache results from using the static methods in System.Enum. The following sample finds all the valid values for an enum type, and then builds a dictionary that maps those values to its string representation.

Example:
```csharp
public static Dictionary<int, string> EnumNamedValues<T>() where T : System.Enum
{
    var result = new Dictionary<int, string>();
    var values = Enum.GetValues(typeof(T));

    foreach (int item in values)
        result.Add(item, Enum.GetName(typeof(T), item)!);
    return result;
}

enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}

var map = EnumNamedValues<Rainbow>();

foreach (var pair in map)
    Console.WriteLine($"{pair.Key}:\t{pair.Value}");
```
 ## Creating Custom Generic Structures and Classes

Generic classes encapsulate operations that are not specific to a particular data type. The most common use for generic classes is with collections like linked lists, hash tables, stacks, queues, trees, and so on. Operations such as adding and removing items from the collection are performed in basically the same way regardless of the type of being stored.

Typically, you create generic classes by starting with an existing concrete class, and changing types into type parameters one at a time until you reach the optimal balance of generalization and usability.

- Which types to generalize into type parameters

    As a rule, the more types you can parameterize, the more flexible and reusable your code becomes. However, too much generalization can create that is difficult for other developers to read and understand.

- What contraints, if any, to apply to the parameters    

    A good rule is to apply the maximum constraints possible that will still let you handle the types you must handle. For example, if you know that your generic class is intended for use only with reference types, apply the class constraint. That will prevent unintended use of your class with value types, and will enable you to use the **as** operator on **T**, check for null values.

- Whether to factor generic behavior into base classes and subclasses

    Because generic classes can serve as a base class, the same design considerations apply here as with non-generic classes. 

- Whether to implement one or more generic interfaces

    For example, if you are designing a class that will be used to create items in a generic-based collection, you may have to implment an interface such as **IComparable\<T>** where **T** is the type of your class.

The rules for type parameters and constraints have several implications for generic class behavior, especially regarding inheritance and member accessibility. Before proceeding, you should understand some terms. For a generic class Node<T>, client code can reference the class either by specifying a type argument - to create a closed constructed type (Node<int>); or by leaving the type parameter unspecified - for example when you specify a generic base class, to create an open constructed type (Node<T>). Generic classes can inherit from concrete, closed constructed, or open constructed base classes:

Example:
```csharp
class BaseNode { }
class BaseNodeGeneric<T> { }

// concrete type
class NodeConcrete<T> : BaseNode { }

//closed constructed type
class NodeClosed<T> : BaseNodeGeneric<int> { }

//open constructed type
class NodeOpen<T> : BaseNodeGeneric<T> { }
```
Non-generic, in other words, concrete, classes can inherit from closed constructed base classes, but not from open constructed classes or from type parameters because there is no way at run time for client code to supply the type argument required to instantiate the base class.

Example:
```csharp
//No error
class Node1 : BaseNodeGeneric<int> { }

//Generates an error
//class Node2 : BaseNodeGeneric<T> {}

//Generates an error
//class Node3 : T {}
```
Generic classes that inherit from open constructed types must supply type arguments for any base class type parameters that are not shared by the inheriting class.

Example:
```csharp
class BaseNodeMultiple<T, U> { }

//No error
class Node4<T> : BaseNodeMultiple<T, int> { }

//No error
class Node5<T, U> : BaseNodeMultiple<T, U> { }

//Generates an error
//class Node6<T> : BaseNodeMultiple<T, U> {}
```

Example:
```csharp
// type parameter T in angle brackets
public class GenericList<T>
{
    // The nested class is also generic on T.
    private class Node
    {
        // T used in non-generic constructor.
        public Node(T t)
        {
            next = null;
            data = t;
        }

        private Node? next;
        public Node? Next
        {
            get { return next; }
            set { next = value; }
        }

        // T as private member data type.
        private T data;

        // T as return type of property.
        public T Data
        {
            get { return data; }
            set { data = value; }
        }
    }

    private Node? head;

    // constructor
    public GenericList()
    {
        head = null;
    }

    // T as method parameter type:
    public void AddHead(T t)
    {
        Node n = new Node(t);
        n.Next = head;
        head = n;
    }

    public IEnumerator<T> GetEnumerator()
    {
        Node? current = head;

        while (current != null)
        {
            yield return current.Data;
            current = current.Next;
        }
    }
}

class TestGenericList
{
    static void Main()
    {
        // int is the type argument
        GenericList<int> list = new GenericList<int>();

        for (int x = 0; x < 10; x++)
        {
            list.AddHead(x);
        }

        foreach (int i in list)
        {
            System.Console.Write(i + " ");
        }
        System.Console.WriteLine("\nDone");
    }
}
```

**Generics overview**

- Use generic types to maximize code reuse, type safety, and performance.
- The most common use of generics is to create collection classes.
- The .NET class library contains several generic collection classes in the System.Collections.Generic namespace. The generic collections should be used whenever possible instead of classes such as ArrayList in the System.Collections namespace.
- You can create your own generic interfaces, classes, methods, events, and delegates.
- Generic classes may be constrained to enable access to methods on particular data types.
- Information on the types that are used in a generic data type may be obtained at run-time by using reflection.

- **Pattern Matching with Generics (7.1)**

```csharp
static void PatternMatching<T>(Point<T> p)
{
    swtich (p)
    {
        case Point<string> pString:
            Console.WriteLine("Point is based on strings"),
            return;
        case Point<int> pInt:
            Console.WriteLine("Point is base on integers".);
            return
    }
}
```