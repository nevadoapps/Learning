# Core C# Programming Constructs - Part 02
Table of Contents:
1. Understanding C# Arrays
2. C# Array Initialization
3. Defining an Array of Objects
4. Multidimensional Arrays
5. Jagged Arrays
6. The System.Array Base Class
7. Methods
8. Expression-Bodied Members
9. Static Local Functions
10. Method Parameter Modifiers
    - The Default Behavior for Value Types
    - The Default Behavior for Reference Types
    - Pass a Reference Type by reference
    - The Out modifier
    - The Ref modifier
    - The In modifier
    - Defining Optional Parameters
    - Understanding method overloading
11. Understanding the enum Type
12. Understanding the Structure (Struct) type (aka Value Type)
13. Understanding the Value Types and Reference Types
14. Understanding C# Nullable Types
    - Nullable Value Types
    - Nullable Reference Types
    - Opting in for Nullable Reference Types
    - Values for Nullable in Project Files
    - The Null-Coalescing Operator
    - The Null-Coalescing Assigment Operator (8.0)
    - The Conditional Operator
    - The Default Keyword
15. Tuples (Not documented yet!)
16. Records (Not documented yet!)

<details>
<summary>

## Understanding C# Arrays
</summary>
<p>

An array is a set of contiguous data points of the same type (an array of ints, an array of strings, an array of SportsCar, etc.)

Example:
```csharp
class program
{
    static void Main(string[] args)
    {
        Console.WriteLine("**** Arrays *****");

        int[] aInts = new int[3];

        //Iterate array of Index value
        for (int i = 0; i < aInts.Length; i++)
        {
            Console.WriteLine($"Value of index in Array: {aInt[i]}");
        }

        //Iterate array over items
        foreach(int i int aInts)
        {
            Console.WriteLine($"Value of Array item: {i}");
        }

        Console.ReadLine();
    }
}
```

**Notes:**
- An array can be single-dimensional, multidimensional or jagged.
- The number of dimensions and the length of each dimensions are established when the array instance is created.
- The default values of numeric array elements are set to zero, and reference elements are set to null.
- A jagged array is an array of arrays, and therefore its elements are reference types and are initialized to null.
- Arrays are zero indexed.
- Array types are reference types derived from the abstract base type Array. All arrays implement IList, and IEnumerable. You can use the foreach statement to iterate through an array.
</p>
</details>

<details>
<summary>

## C# Array Initialization
</summary>
<p>

```csharp
//Array initialization syntax using the new keyword
string[] stringArray = new string[] { "one", "two", "three" };
bool[] boolArray = new bool[] { true, false, true, false };
int[] intArray = new int[] { 1, 5, 43, 23, 10 };
```

**Notes:**
- With the use of curly-bracket syntax, you do not need to specify the size of the array.
</p>
</details>

<details>
<summary>

## 3. Defining an Array of Objects
</summary>
<p>
System.Object is the ultimate base class to every type (including fundamental data types) in the .Net Core type system. Given this fact, if you were to define an array of System.Object data types, the subitems could be anything at all.

```csharp
Console.WriteLine("=> Array of objects");
object[] myObjects = new object[4];
myobject[0] = 10;
myobject[1] = false;
myobject[2] = "My String";

foreach (object obj in myObjects)
{
    Console.WriteLine($"Type: {obj.GetType(), Value: {obj}");
}
```
</p>
</details>

<details>
<summary>

## 4. Multidimensional Arrays
</summary>
<p>
Arrays can have more than one dimension. 

For example:
```csharp
int[,] intArray = new int[3,5];
int[,,] intArray2 = new int[4,4,3];
```

**Array Initialization**
```csharp
//Two dimensional array

int[,] array2D = new int[,] { {1,2}, {3,4}, {5,6}, {7,8} };
//The same array with dimensions specified.
int[,] array2Dx = new int[4,2] { {1,2}, {3,4}, {5,6}, {7,8} };

//You can also initialize the array without specifying the rank.
int[,] array4 = { {1,2}, {3,4}, {5,6}, {7,8} };
```
</p>
</details>

<details>
<summary>

## 5. Jagged Arrays
</summary>
<p>
A jagged array is an array whose elements are arrays, possibly of different sizes. A jagged array is sometimes called an "array of arrays".

Example:
The following is a declaration of a single-dimensional array that has three elements, each of which is a single-dimensional array of integers.
```csharp
int[][] jArray = new int[3][];
```

```csharp
class ArrayTest
{
    static void Main()
    {
        // Declare the array of two elements.
        int[][] arr = new int[2][];

        // Initialize the elements.
        arr[0] = new int[5] { 1, 3, 5, 7, 9 };
        arr[1] = new int[4] { 2, 4, 6, 8 };

        // Display the array elements.
        for (int i = 0; i < arr.Length; i++)
        {
            System.Console.Write("Element({0}): ", i);

            for (int j = 0; j < arr[i].Length; j++)
            {
                System.Console.Write("{0}{1}", arr[i][j], j == (arr[i].Length - 1) ? "" : " ");
            }
            System.Console.WriteLine();
        }
        // Keep the console window open in debug mode.
        System.Console.WriteLine("Press any key to exit.");
        System.Console.ReadKey();
    }
}
/* Output:
    Element(0): 1 3 5 7 9
    Element(1): 2 4 6 8
*/
```
</p>
</details>

<details>
<summary>

## 6. The System.Array Base Class
</summary>
<p>

| Member of Array Class | Meaning of Life |
| -- | -- |
| Clear | The static method sets a range of elements in the array to empty values. (0 for numbers, null for object references, false for Booleans)|
| CopyTo | This method is used to copy elements from the source array into the destination array.|
| Length | This property returns the number of items within the array.|
| Rank | This property returns the number of dimensions of the current array.|
| Revers | This static method reverses the contents of a one-dimensional array.|
| Sort | This static method sorts a one-dimensional array of instrinsic types.

```csharp
string[] strArray = new string[] { "Seha", "Evrim", "Neva" };

//Printing array items
for (int i = 0; i < strArray.Length; i++)
{
    Console.WriteLine($"Array Item Value: {strArray[i]}");
}

//Reversing array items
Array.Reverse(strArray);

for (int i = 0; i < strArray.Length; i++)
{
    Console.WriteLine($"Array Item Value: {strArray[i]}");
}
```
</p>
</details>

<details>
<summary>

## 7. Methods
</summary>
<p>
Methods are defined by an access modifier, return type (or void for no return type), and may or may not take parameters. A method that returns a value to the caller is commonly referred to as a function, while methods that do not return a value are commonly returned to as methods.

Example:
```csharp
class Program 
{
    static void Main(string[] args)
    {

    }

    //Function - Method
    static int AddTwoNumbers(int a, int b)
    {
        return a+b;
    }

    //No Return Value - Method
    static void WriteToConsole(string message)
    { 
        Console.WriteLine($"{Message}");
    }
}
```
</p>
</details>

<details>
<summary>

## 8. Expression-Bodied Members
</summary>
<p>
C# 6 introduced expression-bodied members that shorten the syntax for single-line methods.

Example:
```csharp
static int AddTwoNumbers(int a, int b) => a+b;
```
</p>
</details>

<details>
<summary>

## 9. Static Local Functions (8.0)
</summary>
<p>
Local functions is the ability to create method within method, referred to officially as local functions. A local function is a function declared inside another function.

Example:
```csharp
var result = AddTwoNumbers(5, 10);
Console.WriteLine($"Result: {result}");

static int? AddTwoNumbers(object a, object b)
{
    if (IsInteger(a) && IsInteger(b))
        return (int)a + (int)b;
    else
        return null;

    static bool IsInteger(object a)
    {
        if (a.GetType().ToString() == "System.Int32")
            return true;
        else
            return false;
    }
}
```
</p>
</details>

<details>
<summary>

## 10. Method Parameter Modifiers
</summary>
<p>

| Parameter Modifier | Meaning in life |
| -- | -- |
| (None) | If a value type parameter is not marked with a modifier, it is assummed to be passed by value, meaning the called method receives a copy of the original data. Reference types without a modifier are passed in by reference. | 
| out | Output parameters must be assigned by the method being called and, therefore are passed by reference. If the called method fails to assign output parameters, you are issued a compiler error.|
| ref | The value is initially assigned by the caller and may be optionally modified by the called method (as the data is also passed by reference). No compiler error is generated if the called method failes to assign a ref parameter.|
| in | New in 7.2, the in modifier indicates that a ref parameter is read-only by the called method.|
| params | This parameter modifier allows you to send in a variable number of arguments as a single logical parameter. A method can have only a single params modifier and it must be the final parameter of the method. In reality, you might not need to use the params modifier all to often; however, be aware that numerous methods within the base class libraries do make use of this C# language feature.|
</p>

- ## The Default Behavior for Value Types
The default manner in which a value type parameter is sent into a function is by value. Simply put, if you do not mark the argument with a modifier, a copy of the data is passed into the function.

Example: 
```csharp
class Program 
{
    static void Main(string[] args)
    {
        int x = 9, y = 10;
        Console.WriteLine($"Before method call, x: {x}, y: {y}");
        Console.WriteLine($"Result of the method call: {Add(x, y)}");
        Console.WriteLine($"After method call, x: {x}, y: {y}");
    }

    static int Add(int x, int y)
    {
        int result = x + y;
        x = x + 100;
        y = y + 100;

        Console.WriteLine($"In the method, after reassigning x: {x}, y: {y}");
        return result;
    }
}
```

- ## The Default Behavior of Reference Types

The default manner in which reference type parameter is sent into a function by reference for its properties, but value for itself.

When you pass a reference type by value:
- If the method assigns the parameter to refer to a different object, those changes aren't visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes are visible from the caller.

Example:
```csharp
int[] arr = { 1, 4, 5 };
System.Console.WriteLine("Inside Main, before calling the method, the first element is: {0}", arr[0]);

Change(arr);
System.Console.WriteLine("Inside Main, after calling the method, the first element is: {0}", arr[0]);

static void Change(int[] pArray)
{
    pArray[0] = 888;  // This change affects the original element.
    pArray = new int[5] { -3, -1, -2, -3, -4 };   // This change is local.
    System.Console.WriteLine("Inside the method, the first element is: {0}", pArray[0]);
}
/* Output:
    Inside Main, before calling the method, the first element is: 1
    Inside the method, the first element is: -3
    Inside Main, after calling the method, the first element is: 888
*/
```

- ## Pass a Reference Type by reference

When you pass a reference type by reference:

- If the method assigns the parameter to refer to a different object, those changes are visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes are visible from the caller.

Example:
```csharp
int[] arr = { 1, 4, 5 };
System.Console.WriteLine("Inside Main, before calling the method, the first element is: {0}", arr[0]);

Change(ref arr);
System.Console.WriteLine("Inside Main, after calling the method, the first element is: {0}", arr[0]);

static void Change(ref int[] pArray)
{
    // Both of the following changes will affect the original variables:
    pArray[0] = 888;
    pArray = new int[5] { -3, -1, -2, -3, -4 };
    System.Console.WriteLine("Inside the method, the first element is: {0}", pArray[0]);
}
/* Output:
    Inside Main, before calling the method, the first element is: 1
    Inside the method, the first element is: -3
    Inside Main, after calling the method, the first element is: -3
*/
```

- ## The Out modifier

**Notes:**
- You cannot use the out keyword for the following kinds of methods:
  - Async methods which you define by using the async modifier
  - Iterator methods, which include a yield return or yield break statement.
  - The out keyword cannot be used on the first argument of an extension method.

- ## The Ref modifier
Reference parameters are necessary when you want to allow a method to operate on (and usually change the values of) various data points declared in the caller's scope. 

**Notes:**
- Output parameters do not need to be initialized before they are passed to the method. The reason for this is that the method must assign output parameters before exiting.
- Reference parameters must be initialized before they are passed to the method. The reason for this is that you are passing a reference to an existing variable. If you don't assign it to an initial value, that would be the equivalent of operating on an unassigned local variable.
- The ref keyword cannot be used on the first argument on an extension method when the argument is not a struct, or a generic type not constrained to be a struct.
- You cannot use the out keyword for the following kinds of methods:
  - Async methods which you define by using the async modifier
  - Iterator methods, which include a yield return or yield break statement.

Example:
```csharp
public static void SwapStrings(ref string s1, ref string s2)
{
    string tmpStr = s1;
    s1 = s2;
    s2 = tmpStr;
}
```

- ## The In modifier

The in modifier passes a value by reference (for both value and reference types) and prevents the called method from modifying the values. This clearly states a design intent in your code, as well as **potentially reducing memory pressure**. When value types are passed by value, they are copied (internally) by the called method. If the object is large (such as a large struct), the extra overhead of making a copy for local use can be significant.

Example:
```csharp
static int Add(in int x, in int y)
{
    //Error CS8331 Cannot assign to variable 'in int' because it is a readonly variable
    x = 10000;
    y = 20000;
    int ans = x+y;
    return ans;
}
```
**Notes:**
- Async methods, which you define by using the async modifier.
- Iterator methods, which include a yield return or yield break statement.
- The first argument of an extension method cannot have the in modifier unless that argument is a struct.
- The first argument of an extension method where that argument is a generic type (even when that type is constrained to be a struct.)

- ## Defining Optional Parameters
C# allows you to create methods that can take **optional** arguments. This technique allows the caller to invoke a single method while omitting arguments deemed unnecessary, provided the caller is happy with the specified defaults.

Example: 
```csharp
static void AddNumbers(int x, int y, int z = 0)
{
    return x + y + z;
}
```

- ## Understanding method overloading
Simply, when you define a set of identically named methods that differ by the number of (or type) of parameters, the method in question is said to be overloaded.

Example:
```csharp
static class MathOps
{
    public static int Add(int[] intArray)
    {
        int retVal= 0;

        foreach(int i in intArray)
            retVal = retVal + i;

        return retVal;
    }

    public static int Add(int x, int y)
    {
        return x + y;
    }

    public static int Add(int x, int y, int z = 0)
    {
        return x + y + z;
    }

    public static double Add(double x, double y)
    {
        return x + y;
    }
}
```
</details>

<details>
<summary>

## 11. Understanding the enum Type
</summary>
<p>
An enumeration type (or enum type) is a value type defined by a set of named constants of the underlying integral numeric type. To define an enumeration type, use the **enum** keyword and specify the names of enum members. 

**Notes:** By convention, enum types are suffixed with Enum. 
Example:
```csharp
public enum PaymentTypeEnum
{
    PayWithCreditCard, // = 0
    PayWithCash, // = 1
    PayWithMealTicket // = 2
}

//By controlling storage type
public enum PaymentTypeEnum: sbyte
{
    PayWithCreditCard, // = 0
    PayWithCash, // = 1
    PayWithMealTicket // = 2
}

//By controlling storage and enum values
public enum PaymentTypeEnum: sbyte
{
    PayWithCreditCard = 100,
    PayWithCash = 101,
    PayWithMealTicket = 102
}
```
</p>
</details>

<details>
<summary>

## 12. Understanding the Structure (Struct) type (aka Value Type)
</summary>
<p>

A **structure type** (or struct type) is a value type that can encapsulate data and related functionality. You use the struct keyword to define a structure type.

Example:
```csharp
public struct Coords
{
    public double X;
    public double Y,

    public struct Coords(double x, double y)
    {
        X = y;
        Y = y;
    }

    public override string ToString() => $"({X},{Y})";
}
```

Usage:
```csharp
//Usage - 01
Point p1;
p1.X = 100;
p1.Y = 102;
p1.Display();

//Usage - 02
Point p2 = new Point();
p2.Display();

//Erronous Usage
Point p3;
p3.X = 102;
p3.Display(); //Error! Did not assign Y value

struct Point
{
    public int X;
    public int Y;

    public void Increment()
    {
        X++;
        Y++;
    }

    public void Decrement()
    {
        X--;
        Y--;
    }

    public void Display()
    {
        Console.WriteLine($"X = {X}, Y = {Y}");
    }
}
```

**Notes:** You can think of a struct as a lighweight class type, given that structures provide a way to define a type that supports encapsulation but cannot be used to build a family of related types. When you need to build a family of related types through inheritance, you will need to make use of class types.
</p>
</details>

<details>
<summary>

## 13. Understanding the Value Types and Reference Types
</summary>
<p>

Unlike, arrays, string or enumerations, C# structures do not have an identically named representation in the .Net Core library but are implicitly derived from System.ValueType. The role of System.ValueType is to ensure that the derived type (e.g., any structure) is allocated on the **stack**, rather than the garbage-collected **heap**.

Simply put, data collected on the stack can be created and destroyed quickly, as its lifetime is determined by the defining scope. The base class of ValueType is System.Object.

**Heap-allocated** data, on the other hand, is monitoerd by the .Net Core Garbage collector and has a lifetime that is determined by a large number of factors.

- ## Value Types, Reference Types, and the Assignment Operator
    - ## Value Types
When you assign one value type to another, a member-by-member copy of the field data is achieved.

A variable of a value type contains an instance of the type. This differs from a variable of a reference type, which contains a reference to an instance of the type. By default, on **assignment**, passing an argument to a method, and returning a method result, variable values are copied. The case of value-type variables, the corresponding type instances are copied.

**When you pass a value type by value:**
- If the method assigns the parameter to refer to a different object, those changes aren't visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes aren't visible from the caller.

**When you pass a value type by reference:**
- If the method assigns the parameter to refer to a different object, those changes aren't visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes are visible from the caller.

Example:
```csharp
using System;

public struct MutablePoint
{
    public int X;
    public int Y;

    public MutablePoint(int x, int y) => (X, Y) = (x, y);

    public override string ToString() => $"({X}, {Y})";
}

public class Program
{
    public static void Main()
    {
        var p1 = new MutablePoint(1, 2);
        var p2 = p1;
        p2.Y = 200;
        Console.WriteLine($"{nameof(p1)} after {nameof(p2)} is modified: {p1}");
        Console.WriteLine($"{nameof(p2)}: {p2}");

        MutateAndDisplay(p2);
        Console.WriteLine($"{nameof(p2)} after passing to a method: {p2}");
    }

    private static void MutateAndDisplay(MutablePoint p)
    {
        p.X = 100;
        Console.WriteLine($"Point mutated in a method: {p}");
    }
}
// Expected output:
// p1 after p2 is modified: (1, 2)
// p2: (1, 200)
// Point mutated in a method: (100, 200)
// p2 after passing to a method: (1, 200)
```

If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value-type instance is copied. Both the copy and original value-type instance have access to the same reference-type instance. 

Example:
```csharp
using System;
using System.Collections.Generic;

public struct TaggedInteger
{
    public int Number;
    private List<string> tags;

    public TaggedInteger(int n)
    {
        Number = n;
        tags = new List<string>();
    }

    public void AddTag(string tag) => tags.Add(tag);

    public override string ToString() => $"{Number} [{string.Join(", ", tags)}]";
}

public class Program
{
    public static void Main()
    {
        var n1 = new TaggedInteger(0);
        n1.AddTag("A");
        Console.WriteLine(n1);  // output: 0 [A]

        var n2 = n1;
        n2.Number = 7;
        n2.AddTag("B");

        Console.WriteLine(n1);  // output: 0 [A, B]
        Console.WriteLine(n2);  // output: 7 [A, B]
    }
}
```

- ## Passing Reference Types by Value

**When you pass a reference type by value:**
- If the method assigns the parameter to refer to a different object, those changes aren't visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes are visible from the caller.

Example:
```csharp
class Person
{
    public string personName;
    public int personAge;

    public Person(string name, int age)
    {
        personAge = age;
        personName = name;
    }

    public Person() { }

    public string Display()
    {
        return $"Name: {personName}, Age: {personAge}";
    }
}

public class Program
{
    static void SendAPersonByValue(Person p, int age)
    {
        p.personAge = age;
        //Will the caller see the reassigment?
        p = new Person("Nikki", age + 1);
    }

    static void Main(string[] args)
    {
        //Passing ref-types by value
        Console.WriteLine("**** Passing person object by value Example ****");
        Person fred = new Person("Fred", 12);
        Console.WriteLine($"Initial state of Fred Instance of type Person -> {fred.Display()}");

        Console.WriteLine();

        Console.WriteLine($"SendAPersonValue is to be called with Passing Fred by Value and Age to Set 99");
        SendAPersonByValue(fred, 99);
        Console.WriteLine($"SendAPersonValue is called with Passing Fred by Value and Age to set 99, Fred - > {fred.Display()}");

        Console.WriteLine();

        Console.WriteLine("A new instance to be created by using Fred instance of the type");
        var jane = fred;
        Console.WriteLine("Fred object is copied to Jane");

        Console.WriteLine();

        Console.WriteLine($"SendAPersonValue is to be called with Passing Jane by Value and Age to Set 101");
        SendAPersonByValue(jane, 101);
        Console.WriteLine($"SendAPersonValue is called with Passing Jane by Value and Age to set 101, Jane ->  {jane.Display()}");
        Console.WriteLine($"Current state of Fred Instance of type Person - >{fred.Display()}");
    }
}

/*
**** Passing person object by value Example ****
Initial state of Fred Instance of type Person -> Name: Fred, Age: 12

SendAPersonValue is to be called with Passing Fred by Value and Age to Set 99
SendAPersonValue is called with Passing Fred by Value and Age to set 99, Fred - > Name: Fred, Age: 99

A new instance to be created by using Fred instance of the type
Fred object is copied to Jane

SendAPersonValue is to be called with Passing Jane by Value and Age to Set 101
SendAPersonValue is called with Passing Jane by Value and Age to set 101, Jane ->  Name: Fred, Age: 101
Current state of Fred Instance of type Person - >Name: Fred, Age: 101 
*/
```

- ## Passing Reference Types by Reference

**When you pass a reference type by reference:**
- If the method assigns the parameter to refer to a different object, those changes are visible from the caller.
- If the method modifies the state of the object referred to by the parameter, those changes are visible from the caller.

```csharp
class Person
{
    public string personName;
    public int personAge;

    public Person(string name, int age)
    {
        personAge = age;
        personName = name;
    }

    public Person() { }

    public string Display()
    {
        return $"Name: {personName}, Age: {personAge}";
    }
}

public class Program
{
    static void SendAPersonByValue(ref Person p, int age)
    {
        p.personAge = age;
        //Will the caller see the reassigment?
        p = new Person("Nikki", age + 1);
    }

    static void Main(string[] args)
    {
        //Passing ref-types by value
        Console.WriteLine("**** Passing person object by reference Example ****");
        Person fred = new Person("Fred", 12);
        Console.WriteLine($"Initial state of Fred Instance of type Person -> {fred.Display()}");

        Console.WriteLine();

        Console.WriteLine($"SendAPersonValue is to be called with Passing Fred by Value and Age to Set 99");
        SendAPersonByValue(ref fred, 99);
        Console.WriteLine($"SendAPersonValue is called with Passing Fred by Value and Age to set 99, Fred - > {fred.Display()}");

        Console.WriteLine();

        Console.WriteLine("A new instance to be created by using Fred instance of the type");
        var jane = fred;
        Console.WriteLine("Fred object is copied to Jane");

        Console.WriteLine();

        Console.WriteLine($"SendAPersonValue is to be called with Passing Jane by Value and Age to Set 101");
        SendAPersonByValue(ref jane, 101);
        Console.WriteLine($"SendAPersonValue is called with Passing Jane by Value and Age to set 101, Jane ->  {jane.Display()}");
        Console.WriteLine($"Current state of Fred Instance of type Person - >{fred.Display()}");
    }
}

/*
**** Passing person object by reference Example ****
Initial state of Fred Instance of type Person -> Name: Fred, Age: 12

SendAPersonValue is to be called with Passing Fred by Value and Age to Set 99
SendAPersonValue is called with Passing Fred by Value and Age to set 99, Fred - > Name: Nikki, Age: 100

A new instance to be created by using Fred instance of the type
Fred object is copied to Jane

SendAPersonValue is to be called with Passing Jane by Value and Age to Set 101
SendAPersonValue is called with Passing Jane by Value and Age to set 101, Jane ->  Name: Nikki, Age: 102
Current state of Fred Instance of type Person - >Name: Nikki, Age: 101
*/
```

- ## Value Types and Reference Types Comparison

| Intriguing Question | Value Type | Reference Type |
| -- | -- | -- |
| Where are objects are allocated? | Allocated on the stack | Allocated on the managed heap |
| How is a variable represented? | Value type variables are local copies. | Reference type variables are pointing to the memory occupied by the allocated instance. | 
| What is the base type? | Implicitly extends, System.ValueType | Can derive from any other type (except System.ValueType), as long as that type is not "sealed". |
| Can this type function as a base to other types? | No, Value types are always sealed and cannot be inherited from. | Yes, If the type is not sealed, it may function as a base to other types. |
| What is the default parameter-passing behavior? | Variables are passed by value. | For reference types, the reference is copied by value. |
| Can this type override System.Object.Finalize()? | No | Yes, indirectly. |
| Can I define constructors for this type? | Yes, but the default constructor is reserved. | But of course.! |
| When do variables of this type die? | When they fall out of the defining scope. | When the object is garbage collected. |

- ## 14. Understanding C# Nullable Types

- ## Nullable Value Types
As you know, C# data types have a fixed range and are represented as a type in the System namespace. For example, the System.Boolean data type can be assigned a value from the set {true, false}. Value types can never be assigned the value of null, as that is used to establish an empty object reference.

C# supports the concept of **nullable data types**. Simply put, a nullable type can represent all the values of its underlying type, plus the value null. Thus, if you declare a nullable bool, it could be assigned a value from the set {true, false, null}.

In C#, the **?** suffix notation is a shorthand for creating an instance of the generic System.Nullable<T> structure type.

Example:
```csharp
int? nullableInt = 10;
double? nullableDouble = 3.14;
bool? nullableBool = null;
char? nullableChar = 'a';
int[]? arrayOfNullableInts = new Int?[10];
```

- ## Nullable Reference Types

A significant change in C# 8 is the support for nullable reference types. In fact, the change is so significant that .Net FW could not be updated to support this new feature. Hence, the decisions to only support C# 8 in .Net Core 3.0 or later and the decision that support for nullable reference types is an opt in. By default, when you create a new project in .NetCore 3/3.1, reference types work the same way that they did on C# 7.

- ## Opting in for Nullable Reference Types
Support for nullable reference types is controlled by setting a Nullable Context. This can be as big as an entire project (by updating the project file) or as small as a few lines (by using compiler directives). 
- Nullable Annotation Context: This enables/disables the nullable annotation (?) for nullable reference types.
- Nullable Warning Context: This enables/disables the compiler warnings for nullable reference types.

Update the project file to support nullabe reference types by adding the <Nullable> node.

Example:

```xml
    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <Nullable>enable</Nullable>
    </PropertyGroup>
```
- ## Values for Nullable in Project Files
| Value | Meaning in Life |
| -- | -- |
| Enable | Nullable Annotations are enabled an Nullable Warnings are enabled.|
| Warnings | Nullable Annotations are disabled and Nullable Warnings are enabled. |
| Annotations | Nullable Annotations are enabled and Nullable Warnings are disabled. | 
| Disable | Nullable Annotations are disabled and Nullable Warnings are disabled. |

- ## The Null-Coalescing Operator

The null-coalescing operator ?? returns the value of its left-hand operand if it isn't null; otherwise, it evaluates the right-hand operand and returns its result. The ?? operator doesn't evaluate its right-hand operand if the left-hand operand evaluates to non-null.

Example:
```csharp
List<int> numbers = null;
int? a = null;

(numbers ??= new List<int>()).Add(5);
Console.WriteLine(string.Join(" ", numbers));  // output: 5

numbers.Add(a ??= 0);
Console.WriteLine(string.Join(" ", numbers));  // output: 5 0
Console.WriteLine(a);  // output: 0
```

**Notes:** 
- The left-hand operand of the **??** operator must be a variable, a **operator**, or an **indexer** element.
- The type of the left-hand operand of the **??** and **??=** operators can't be a non-nullable value type.

- ## The Null-Coalescing Assigment Operator (8.0)

Building on the null-coalescing operator, C# 8 introduced the null-coalescing operator **??=**. This operator assigns the left hand-side to the right-hand side only if the left-hand side is null.

Example:
```csharp
int? nullableInt = null;
nullableInt ??=12;
nullableInt ??=14;
Console.WriteLine($"Output:{nullableInt}");
```

- ## The Conditional Operator
A null-conditional operator applies a member access, ?., or element access, ?[], operation to its operand only if that operand evaluates to non-null; otherwise, it returns null.

- If a evaluates to null, the result of a?.x or a?[x] is null.
- If a evaluates to non-null, the result of a?.x or a?[x] is the same as the result of a.x or a[x], respectively.

Example:
```csharp
double SumNumbers(List<double[]> setsOfNumbers, int indexOfSetToSum)
{
    return setsOfNumbers?[indexOfSetToSum]?.Sum() ?? double.NaN;
}

var sum1 = SumNumbers(null, 0);
Console.WriteLine(sum1);  // output: NaN

var numberSets = new List<double[]>
{
    new[] { 1.0, 2.0, 3.0 },
    null
};

var sum2 = SumNumbers(numberSets, 0);
Console.WriteLine(sum2);  // output: 6

var sum3 = SumNumbers(numberSets, 1);
Console.WriteLine(sum3);  // output: NaN
```

- ## The Default Keyword

```csharp
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("***** Testing aInt *****");
            int? aInt = default;

            if (aInt is null)
            {
                Console.WriteLine($"aInt is null");
                aInt ??= 22;
                Console.WriteLine($"Current value of aInt is {aInt}");
            }
            else
                Console.WriteLine($"aInt is not null and the value of aInt: {aInt}");

            Console.WriteLine();
            Console.WriteLine("***** Testing bInt *****");

            int? bInt = default(int);

            if (aInt is null)
            {
                Console.WriteLine($"bInt is null");
                bInt ??= 32;
                Console.WriteLine($"Current value of aInt is {bInt}");
            }
            else
                Console.WriteLine($"bInt is not null and the value of bInt: {bInt}");
        }
    }

    /*

    ***** Testing aInt *****
    aInt is null
    Current value of aInt is 22

    ***** Testing bInt *****
    bInt is not null and the value of bInt: 0
    */
}
```
</p>
</details>

<details>
<summary>

## 14. Tuples (Not documented yet!)
</summary>
<p>
</p>
</details>

<details>
<summary>

## 15.Records (Not documented yet!)
</summary>
<p>

Beginning with C# 9, you use the **record** keyword to define a **reference type** that provides built-in functionality for encapsulating data. C# 10 allows the **record class** syntax as a synonym to clarify a reference type, and **record struct** to define a value type with similar functionality. 

The following two examples demonstrate record (or record class) reference types:
```csharp
public record Person(string FirstName, string LastName);

//or 

public record Person
{
    public string FirstName { get; init; } = default!;
    public string LastName { get; init; } = default!;
};
```

While records can be mutable, they're primarily intended for supporting immutable data models. The record type offers the following features:

Concise syntax for creating a reference type with immutable properties
Built-in behavior useful for a data-centric reference type:
- Value equality
- Concise syntax for nondestructive mutation
- Built-in formatting for display
- Support for inheritance hierarchies

**Value equality of Records**


If you don't override or replace equality methods, the type you declare governs how equality is defined:

- For class types, two objects are equal if they refer to the same object in memory.
- For struct types, two objects are equal if they are of the same type and store the same values.
- For record types, including record struct and readonly record struct, two objects are equal if they are of the same type and store the same values.

The definition of equality for a **record struct** is the same as for a struct. The difference is that for a **struct**, the implementation is in ValueType.Equals(Object) and relies on reflection. For records, the implementation is compiler synthesized and uses the declared data members.

Reference equality is required for some data models. For example, Entity Framework Core depends on reference equality to ensure that it uses only one instance of an entity type for what is conceptually one entity. **For this reason, records and record structs aren't appropriate for use as entity types in Entity Framework Core.**

The following example illustrates value equality of record types:

```csharp
public record Person(string FirstName, string LastName, string[] PhoneNumbers);

public static void Main()
{
    var phoneNumbers = new string[2];
    Person person1 = new("Nancy", "Davolio", phoneNumbers);
    Person person2 = new("Nancy", "Davolio", phoneNumbers);
    Console.WriteLine(person1 == person2); // output: True

    person1.PhoneNumbers[0] = "555-1234";
    Console.WriteLine(person1 == person2); // output: True

    Console.WriteLine(ReferenceEquals(person1, person2)); // output: False
}
```

To implement value equality, the compiler synthesizes several methods, including:

- An override of **Object.Equals(Object)**. It is an error if the override is declared explicitly.

This method is used as the basis for the **Object.Equals(Object, Object)** static method when both parameters are non-null.

- A **virtual**, or **sealed**, Equals(R? other) where R is the record type. This method implements **IEquatable\<T>**. This method can be declared explicitly.

- If the record type is derived from a base record type Base, **Equals(Base? other)**. It is an error if the override is declared explicitly. If you provide your own implementation of **Equals(R? other)**, provide an implementation of GetHashCode also.

- An override of **Object.GetHashCode()**. This method can be declared explicitly.

- Overrides of operators == and !=. It is an error if the operators are declared explicitly.

- If the record type is derived from a base record type, **protected override Type EqualityContract { get; };**. This property can be declared explicitly. For more information, see Equality in inheritance hierarchies.

If a record type has a method that matches the signature of a synthesized method allowed to be declared explicitly, the compiler doesn't synthesize that method.

**Built-in formatting for display**

Record types have a compiler-generated ToString method that displays the names and values of public properties and fields. The ToString method returns a string of the following format:

**\<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}**


The string printed for \<value> is the string returned by the ToString() for the type of the property. In the following example, ChildNames is a System.Array, where ToString returns System.String[]:

```csharp
Person { FirstName = Nancy, LastName = Davolio, ChildNames = System.String[] }
```

**Inheritance**
This section only applies to record class types.

A record can inherit from another record. However, a record can't inherit from a class, and a class can't inherit from a record.

**With Expression** 
A with expression produces a copy of its operand with the specified properties and fields modified. you use object initializer syntax to specify what members to modify and their new values:

```csharp
using System;

public class WithExpressionBasicExample
{
    public record NamedPoint(string Name, int X, int Y);

    public static void Main()
    {
        var p1 = new NamedPoint("A", 0, 0);
        Console.WriteLine($"{nameof(p1)}: {p1}");  // output: p1: NamedPoint { Name = A, X = 0, Y = 0 }
        
        var p2 = p1 with { Name = "B", X = 5 };
        Console.WriteLine($"{nameof(p2)}: {p2}");  // output: p2: NamedPoint { Name = B, X = 5, Y = 0 }
        
        var p3 = p1 with 
            { 
                Name = "C", 
                Y = 4 
            };
        Console.WriteLine($"{nameof(p3)}: {p3}");  // output: p3: NamedPoint { Name = C, X = 0, Y = 4 }

        Console.WriteLine($"{nameof(p1)}: {p1}");  // output: p1: NamedPoint { Name = A, X = 0, Y = 0 }

        var apples = new { Item = "Apples", Price = 1.19m };
        Console.WriteLine($"Original: {apples}");  // output: Original: { Item = Apples, Price = 1.19 }
        var saleApples = apples with { Price = 0.79m };
        Console.WriteLine($"Sale: {saleApples}");  // output: Sale: { Item = Apples, Price = 0.79 }
    }
}
```

In C# 9.0, a left-hand operand of a with expression must be of a record type. Beginning with C# 10, a left-hand operand of a with expression can also be of a structure type or an anonymous type.

The result of a with expression has the same run-time type as the expression's operand, as the following example shows:

```csharp
using System;

public class InheritanceExample
{
    public record Point(int X, int Y);
    public record NamedPoint(string Name, int X, int Y) : Point(X, Y);

    public static void Main()
    {
        Point p1 = new NamedPoint("A", 0, 0);
        Point p2 = p1 with { X = 5, Y = 3 };
        Console.WriteLine(p2 is NamedPoint);  // output: True
        Console.WriteLine(p2);  // output: NamedPoint { X = 5, Y = 3, Name = A }
    }
}
```

**with Expression in derived records**

The result of a with expression has the same run-time type as the expression's operand. All properties of the run-time type get copied, but you can only set properties of the compile-time type, as the following example shows:

```csharp
public record Point(int X, int Y)
{
    public int Zbase { get; set; }
};
public record NamedPoint(string Name, int X, int Y) : Point(X, Y)
{
    public int Zderived { get; set; }
};

public static void Main()
{
    Point p1 = new NamedPoint("A", 1, 2) { Zbase = 3, Zderived = 4 };

    Point p2 = p1 with { X = 5, Y = 6, Zbase = 7 }; // Can't set Name or Zderived
    Console.WriteLine(p2 is NamedPoint);  // output: True
    Console.WriteLine(p2);
    // output: NamedPoint { X = 5, Y = 6, Zbase = 7, Name = A, Zderived = 4 }

    Point p3 = (NamedPoint)p1 with { Name = "B", X = 5, Y = 6, Zbase = 7, Zderived = 8 };
    Console.WriteLine(p3);
    // output: NamedPoint { X = 5, Y = 6, Zbase = 7, Name = B, Zderived = 8 }
}
```

**Generic Constraints**
There's no generic constraint that requires a type to be a record. Records satisfy either the class or struct constraint. To make a constraint on a specific hierarchy of record types, put the constraint on the base record as you would a base class. For more information, see Constraints on type parameters.
</p>
</details>