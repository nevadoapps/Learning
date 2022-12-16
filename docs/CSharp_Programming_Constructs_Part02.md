# Core C# Programming Constructs - Part 02
Table of Contents:
1. Understanding C# Arrays
2. C# Array Initialization
3. Defining an Array of Objects
4. Multidimensional Arrays
5. Jagged Arrays
6. The System.Array Base Class
7. Methods

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
</p>
</details>