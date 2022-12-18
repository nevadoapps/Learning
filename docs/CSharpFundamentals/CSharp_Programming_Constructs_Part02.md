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
- The ref keyword cannot be used on the first argument on an extesion method when the argument is not a struct, or a generic type not constrained to be a struct.
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

</p>
</details>