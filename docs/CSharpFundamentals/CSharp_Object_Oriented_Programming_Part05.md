# Object Oriented Programming with C# - Part 05
Table of contents:
- Understanding Indexer Methods
- Understanding Operator Overloading
- Understanding Custom Type Conversions
- Understanding Extension Methods
- Understanding Anonymous Types


## Understanding Indexer Methods

Indexers allow instance of a class or struct to be indexed just like arrays. The indexed value can be set or retrieved without explicitly specifying a type of instance member. Indexers resemble **properties** except that their accessors take parameters.

Example:
```csharp
    internal class Program
    {
        static void Main(string[] args) 
        {
            GenericCollection<Person06> collection = new();

            collection.Add(new Person06() { Id = 1, Name = "HSY01"});
            collection.Add(new Person06() { Id = 2, Name = "HSY02" });
            collection.Add(new Person06() { Id = 3, Name = "HSY03" });
            collection.Add(new Person06() { Id = 4, Name = "HSY05" });
            collection.Add(new Person06() { Id = 5, Name = "HSY05" });

            Console.WriteLine($"Collection Count: {collection.Count()}");

            foreach (var colItem in collection)
            {
                Console.WriteLine($"Id: {colItem.Id}, Name: {colItem.Name}");
            }

            var result = collection.Where(f => f.Name == "HSY05").FirstOrDefault();

            if (result is null)
                Console.WriteLine("Result is null which is expected");
            else
                Console.WriteLine($"Found - > Id: {result.Id}, Name: {result.Name}");

            var value1 = (Person06)collection.Get(0);
            if (collection.Get(0).Equals(value1))
                Console.WriteLine("(Over Get Method) - > Two objects are equal");
            else
                Console.WriteLine("(Over Get Method) - > Two objects are not equal which is expected result");

            var iValue1 = (Person06)collection[0];
            var iValue2 = (Person06)collection[0];
            
            if (iValue1.Equals(iValue2))
                Console.WriteLine("(Over Indexer Method) - > Two objects are equal");
            else
                Console.WriteLine("(Over Indexer Method) - > Two objects are not equal which is expected result");
        }
    }

    internal class Person06
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    internal class GenericCollection<T>: IEquatable<T>, IEnumerable<T> where T: class
    {
        private List<T> list;

        public GenericCollection() 
        { 
            list = new List<T>();
        }

        public T this[int index]
        {
            get { return (T)list[index]; }
            set { list[index] = value; }
        }

        public void Add(T item)
        {
            list.Add(item);
        }

        public void Remove(T item)
        {
            list.Remove(item);
        }

        public T Get(int index)
        {
            return list[index];
        }

        public IEnumerator<T> GetEnumerator()
        {
            return this.list.GetEnumerator();
        }

        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }

        public bool Equals(T? other)
        {
            if (other == null) return false;
            
            var properties = typeof(T).GetProperties();

            foreach ( var property in properties)
            {
                var value1 = property.GetValue(this);
                var value2 = property.GetValue(other);

                if (!Equals(value1, value2)) return false;
            }

            return true;
        }
    }
}
```

Example - 02:
```csharp
internal class Program
    {
        static void Main(string[] args)
        {
            CustomCollection<string> strings= new CustomCollection<string>();
            
            int i = 0;
            while (i < 1000)
            { 
                Random rnd = new Random();
                var tmpValue = rnd.Next(10000).ToString();
                if (!strings.Contains(tmpValue))
                {
                    strings.Add(tmpValue);
                    i++;
                }
            }

            Console.WriteLine($"String in index 0 : {strings[0]}");

            foreach (var str in strings)
            {
                Console.WriteLine($"String value: {str}");
            }

            Console.WriteLine($"Strings collection count: {strings.Count()}");

            Console.WriteLine($"Strings Unique Count: {strings.Distinct<string>().Count()}");
        }
    }

    public class CustomCollection<T>: ICollection<T>, IEquatable<T>, IEnumerable<T> where T : class
    {
        private List<T> _collection;

        public CustomCollection() 
        { 
            _collection= new List<T>();
        }

        public int Count => ((ICollection<T>)_collection).Count;

        public bool IsReadOnly => ((ICollection<T>)_collection).IsReadOnly;

        public T this[int index]
        {
            get { return (T)_collection[index]; }
            set { _collection[index] = value; }
        }

        public void Add(T item)
        {
            ((ICollection<T>)_collection).Add(item);
        }

        public void Clear()
        {
            ((ICollection<T>)_collection).Clear();
        }

        public bool Contains(T item)
        {
            return ((ICollection<T>)_collection).Contains(item);
        }

        public void CopyTo(T[] array, int arrayIndex)
        {
            ((ICollection<T>)_collection).CopyTo(array, arrayIndex);
        }

        public bool Equals(T? other)
        {
            if (other is null) return false;

            var properties = typeof(T).GetProperties();

            foreach (var property in properties)
            {
                var tvalue = property.GetValue(this);
                var ovalue = property.GetValue(other);

                if (tvalue != ovalue) return false;
            }

            return true;
        }

        public IEnumerator<T> GetEnumerator()
        {
            return _collection.GetEnumerator();
        }

        public bool Remove(T item)
        {
            return ((ICollection<T>)_collection).Remove(item);
        }

        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }
    }
```
- ## Understanding Operator Overloading

A user-defined type can overload a predefined C# operator. That is, a type can provide the custom implementation of an operation in case one or both of the operands are of that type. The Overloadable operators section shows which C# operators can be overloaded.

The following table shows the operators that can be overloaded:
| Operators | Notes |
|-- | -- |
| +x, -x, !x, ~x, ++, --, true, false | The true and false operators must be overloaded together. |
| x + y, x - y, x * y, x / y, x % y,
x & y, x | y, x ^ y,
x << y, x >> y, x >>> y | |
| x == y, x != y, x < y, x > y, x <= y, x >= y | Must be overloaded in pairs as follows: == and !=, < and >, <= and >=. | 

Use the **operator** keyword to declare an operator. An operator declaration must satify the following rules:

- It includes both a public and static modifier.
- A unary operator has one input parameter. A binary operator has two input parameters. In each case, at least one parameter must have T or T? where T is the type that contains the operator declaration.

Example:
```csharp
public readonly struct Fraction
{
    private readonly int num;
    private readonly int den;

    public Fraction(int numerator, int denominator)
    {
        if (denominator == 0)
        {
            throw new ArgumentException("Denominator cannot be zero.", nameof(denominator));
        }
        num = numerator;
        den = denominator;
    }

    public static Fraction operator +(Fraction a) => a;
    public static Fraction operator -(Fraction a) => new Fraction(-a.num, a.den);

    public static Fraction operator +(Fraction a, Fraction b)
        => new Fraction(a.num * b.den + b.num * a.den, a.den * b.den);

    public static Fraction operator -(Fraction a, Fraction b)
        => a + (-b);

    public static Fraction operator *(Fraction a, Fraction b)
        => new Fraction(a.num * b.num, a.den * b.den);

    public static Fraction operator /(Fraction a, Fraction b)
    {
        if (b.num == 0)
        {
            throw new DivideByZeroException();
        }
        return new Fraction(a.num * b.den, a.den * b.num);
    }

    public override string ToString() => $"{num} / {den}";
}

public static class OperatorOverloading
{
    public static void Main()
    {
        var a = new Fraction(5, 4);
        var b = new Fraction(1, 2);
        Console.WriteLine(-a);   // output: -5 / 4
        Console.WriteLine(a + b);  // output: 14 / 8
        Console.WriteLine(a - b);  // output: 6 / 8
        Console.WriteLine(a * b);  // output: 5 / 8
        Console.WriteLine(a / b);  // output: 10 / 4
    }
}
```

Example - 02:
```csharp
public class Point
{
    public int X { get; set; }
    public int Y { get; set;}

    public Point(int xPos, int yPos)
    {
        X = xPos;
        Y = yPos;
    }

    public static Point operator + (Point p1, Point p2)
    {
        return new Point(p1.X + p2.X, p1.Y + p2.Y);
    }

    public static Point operator - (Point p1, Point p2)
        => new Point(p1.X - p2.X, p1.Y - p2.Y);

    public static bool operator == (Point p1, Point p2)
    {
        return p1.Equals(p2);
    }

    public static bool operator != (Point p1, Point p2)
    {
        return !p1.Equals(p2);
    }

    public override bool Equals(object o)
    {
        return o.ToString() == this.ToString();
    }

    public override int GetHashCode()
    {
        return this.ToString().GetHashCode();
    }

    public override string ToString()
    {
        return $"[{this.X}, {this.Y}]";
    }
}
```

- ## Understanding Custom Type Conversions

    - **Type Conversion Tables in .Net**

    Widening conversion occurs when a value of one type is converted to another type that is of equal or greater size.

    A narrowing conversion occurs when a value of one type is converted to a value of another type that is of a smallar size.

    - The following table describes the widening conversions that can be performed without the loss of information.

    | Type | Can be converted without data loss to |
    | -- | -- |
    | Byte | 	UInt16, Int16, UInt32, Int32, UInt64, Int64, Single, Double, Decimal | 
    | SByte | Int16, Int32, Int64, Single, Double, Decimal | 
    | Int16 | 	Int32, Int64, Single, Double, Decimal |
    | UInt16 | UInt32, Int32, UInt64, Int64, Single, Double, Decimal |
    | Char | UInt16, UInt32, Int32, UInt64, Int64, Single, Double, Decimal | 
    | Int32 | Int64, Double, Decimal | 
    | UInt32 | Int64, UInt64, Double, Decimal | 
    | Int64 | Decimal |
    | UInt64 | Decimal |
    | Single | Double |


    - Some widening conversions to Single or Double can cause a loss of precision. The following table describes the widening conversions that sometimes result in a loss of information. 

    | Type | Can be converted to |
    | -- | -- |
    | Int32 | Single |
    | UInt32 | Single |
    | Int64 | Single, Double |
    | UInt64 | Single, Double | 
    | Decimal | Single, Double |

    **Narrowing Conversions**

    A narrowing conversion to Single or Double can cause a loss of information. If the target type cannot properly express the magnitude of the source, the resulting type is set to the constant PositiveInfinity or NegativeInfinity. PositiveInfinity results from dividing a positive number by zero and is also returned when the value of a Single or Double exceeds the value of the MaxValue field. NegativeInfinity results from dividing a negative number by zero and is also returned when the value of a Single or Double falls below the value of the MinValue field. A conversion from a Double to a Single might result in PositiveInfinity or NegativeInfinity.

    A narrowing conversion can also result in a loss of information for other data types. However, an OverflowException is thrown if the value of a type that is being converted falls outside of the range specified by the target type's MaxValue and MinValue fields, and the conversion is checked by the runtime to ensure that the value of the target type does not exceed its MaxValue or MinValue. Conversions that are performed with the System.Convert class are always checked in this manner.

    The following table lists conversions that throw an OverflowException using System.Convert or any checked conversion if the value of the type being converted is outside the defined range of the resulting type.

    | Type | Can be converted to  | 
    | -- | -- |
    | Byte	 | SByte  | 
    | SByte	 | Byte, UInt16, UInt32, UInt64 | 
    | Int16	 | Byte, SByte, UInt16  | 
    | UInt16	 | Byte, SByte, Int16  | 
    | Int32	 | Byte, SByte, Int16, UInt16,UInt32  | 
    | UInt32	 | Byte, SByte, Int16, UInt16, Int32  | 
    | Int64	 | Byte, SByte, Int16, UInt16, Int32,UInt32,UInt64  | 
    | UInt64	 | Byte, SByte, Int16, UInt16, Int32, UInt32, Int64  | 
    | Decimal	 | Byte, SByte, Int16, UInt16, Int32, UInt32, Int64, UInt64  | 
    | Single	 | Byte, SByte, Int16, UInt16, Int32, UInt32, Int64, UInt64  | 
    | Double	 | Byte, SByte, Int16, UInt16, Int32, UInt32, Int64, UInt64  | 

Every value has an associated type, which defines attributes such as the amount of space allocated to the value, the range of possible values it can have, and the members that it makes available. Many values can be expressed as more than one type. For example, the value 4 can be expressed as an integer or a floating-point value. Type conversion creates a value in a new type that is equivalent to the value of an old type, but does not necessarily preserve the identity (or exact value) of the original object.

.NET automatically supports the following conversions:

- Conversion from a derived class to a base class. This means, for example, that an instance of any class or structure can be converted to an Object instance. This conversion does not require a casting or conversion operator.

- Conversion from a base class back to the original derived class. In C#, this conversion requires a casting operator. In Visual Basic, it requires the CType operator if Option Strict is on.

- Conversion from a type that implements an interface to an interface object that represents that interface. This conversion does not require a casting or conversion operator.

- Conversion from an interface object back to the original type that implements that interface. In C#, this conversion requires a casting operator. In Visual Basic, it requires the CType operator if Option Strict is on.

In addition to these automatic conversions, .NET provides several features that support custom type conversion. These include the following:

- The Implicit operator, which defines the available widening conversions between types. For more information, see the Implicit Conversion with the Implicit Operator section.

- The Explicit operator, which defines the available narrowing conversions between types. For more information, see the Explicit Conversion with the Explicit Operator section.

- The IConvertible interface, which defines conversions to each of the base .NET data types. For more information, see The IConvertible Interface section.

- The Convert class, which provides a set of methods that implement the methods in the IConvertible interface. For more information, see The Convert Class section.

- The TypeConverter class, which is a base class that can be extended to support the conversion of a specified type to any other type. For more information, see The TypeConverter Class section.

**Implicit conversion with the implicit operator**

Widening conversions involve the creation of a new value from the value of an existing type that has either a more restrictive range or a more restricted member list than the target type. Widening conversions cannot result in data loss (although they may result in a loss of precision). Because data cannot be lost, compilers can handle the conversion implicitly or transparently, without requiring the use of an explicit conversion method or a casting operator.

Example: 
```csharp
byte byteValue = 16;
short shortValue = -1024;
int intValue = -1034000;
long longValue = 1152921504606846976;
ulong ulongValue = UInt64.MaxValue;

decimal decimalValue;

decimalValue = byteValue;
Console.WriteLine("After assigning a {0} value, the Decimal value is {1}.",
                byteValue.GetType().Name, decimalValue);

decimalValue = shortValue;
Console.WriteLine("After assigning a {0} value, the Decimal value is {1}.",
                shortValue.GetType().Name, decimalValue);

decimalValue = intValue;
Console.WriteLine("After assigning a {0} value, the Decimal value is {1}.",
                intValue.GetType().Name, decimalValue);

decimalValue = longValue;
Console.WriteLine("After assigning a {0} value, the Decimal value is {1}.",
                longValue.GetType().Name, decimalValue);

decimalValue = ulongValue;
Console.WriteLine("After assigning a {0} value, the Decimal value is {1}.",
                  ulongValue.GetType().Name, decimalValue);
// The example displays the following output:
//    After assigning a Byte value, the Decimal value is 16.
//    After assigning a Int16 value, the Decimal value is -1024.
//    After assigning a Int32 value, the Decimal value is -1034000.
//    After assigning a Int64 value, the Decimal value is 1152921504606846976.
//    After assigning a UInt64 value, the Decimal value is 18446744073709551615.
```

**Explicit conversion with the explicit operator**

Narrowing conversions involve the creation of a new value from the value of an existing type that has either a greater range or a larger member list than the target type. Because a narrowing conversion can result in a loss of data, compilers often require that the conversion be made explicit through a call to a conversion method or a casting operator. That is, the conversion must be handled explicitly in developer code.

    **Notes:**

    The major purpose of requiring a conversion method or casting operator for narrowing conversions is to make the developer aware of the possibility of data loss or an OverflowException so that it can be handled in code. However, some compilers can relax this requirement. For example, in Visual Basic, if Option Strict is off (its default setting), the Visual Basic compiler tries to perform narrowing conversions implicitly.

To handle such narrowing conversions, .NET allows types to define an Explicit operator. Individual language compilers can then implement this operator using their own syntax, or a member of the Convert class can be called to perform the conversion. (For more information about the Convert class, see The Convert Class later in this topic.) The following example illustrates the use of language features to handle the explicit conversion of these potentially out-of-range integer values to Int32 values.

Example:
```csharp
public struct ByteWithSignE
{
    private SByte signValue;
    private Byte value;

    private const byte MaxValue = byte.MaxValue;
    private const int MinValue = -1 * byte.MaxValue;

    public static explicit operator ByteWithSignE(int value)
    {
        // Check for overflow.
        if (value > ByteWithSignE.MaxValue || value < ByteWithSignE.MinValue)
            throw new OverflowException(String.Format("'{0}' is out of range of the ByteWithSignE data type.",
                                                      value));

        ByteWithSignE newValue;
        newValue.signValue = (SByte)Math.Sign(value);
        newValue.value = (byte)Math.Abs(value);
        return newValue;
    }

    public static explicit operator ByteWithSignE(uint value)
    {
        if (value > ByteWithSignE.MaxValue)
            throw new OverflowException(String.Format("'{0}' is out of range of the ByteWithSignE data type.",
                                                      value));

        ByteWithSignE newValue;
        newValue.signValue = 1;
        newValue.value = (byte)value;
        return newValue;
    }

    public override string ToString()
    {
        return (signValue * value).ToString();
    }
}

ByteWithSignE value;

try
{
    int intValue = -120;
    value = (ByteWithSignE)intValue;
    Console.WriteLine(value);
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message);
}

try
{
    uint uintValue = 1024;
    value = (ByteWithSignE)uintValue;
    Console.WriteLine(value);
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message);
}
// The example displays the following output:
//       -120
//       '1024' is out of range of the ByteWithSignE data type.
```

- ## Understanding Extension Methods

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are static methods, but they're called as if they were instance methods on the extended type. 

Extension methods are defined as static methods but are called by using instance method syntax. Their first parameter specifies which type the method operates on. The parameter is preceded by the **this** modifier. Extension methods are only in scope when you explicitly import the namespace into your source code with a **using** directive.

Example:
```csharp
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this string str)
        {
            return str.Split(new char[] { ' ', '.', '?' },
                             StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }
}

using ExtensionMethods;

string s = "Hello Extension Methods";
int i = s.WordCount();
//or 
string s = "Hello Extension Methods";
int i = MyExtensions.WordCount(s);
```

- **Common Usage Patterns**

    - **Layer-Specific Functionality**

    When using an Onion Architecture or other layered application design, it's common to have a set of Domain Entities or Data Transfer Objects that can be used to communicate across application boundaries. These objects generally contain no functionality, or only minimal functionality that applies to all layers of the application. Extension methods can be used to add functionality that is specific to each application layer without loading the object down with methods not needed or wanted in other layers.

    Example:
    ```csharp
    public class DomainEntity
    {
        public int Id { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }

    static class DomainEntityExtensions
    {
        static string FullName(this DomainEntity value)
            => $"{value.FirstName} {value.LastName}";
    }
    ```
    - **Extending Predefined Types**

    Rather than creating new objects when reusable functionality needs to be created, we can often extend an existing type, such as a .NET or CLR type. As an example, if we don't use extension methods, we might create an Engine or Query class to do the work of executing a query on a SQL Server that may be called from multiple places in our code. However we can instead extend the System.Data.SqlClient.SqlConnection class using extension methods to perform that query from anywhere we have a connection to a SQL Server. Other examples might be to add common functionality to the System.String class, extend the data processing capabilities of the System.IO.File and System.IO.Stream objects, and System.Exception objects for specific error handling functionality. These types of use-cases are limited only by your imagination and good sense.

    Extending predefined types can be difficult with struct types because they're passed by value to methods. That means any changes to the struct are made to a copy of the struct. Those changes aren't visible once the extension method exits. You can add the ref modifier to the first argument of an extension method. Adding the ref modifier means the first argument is passed by reference. This enables you to write extension methods that change the state of the struct being extended.

    - **Extending Types Implementing Specific Interfaces**

    It is possible to define an extension method that can only extend a class or structure that implements the correct interfaces. 

    Example:
    ```csharp
    static class MyExtensions
    {
        public static void PrintDataAndBeep(this System.Collection.IEnumerable iterator)
        {
            foreach (var itm in iterator)
            {
                Console.WriteLine(itm);
                Console.Beep();
            }
        }
    }

    static void Main(string[] args)
    {
        string[] data = {"Wow", "this", "is", "sort", "of", "annoying" };

        data.PrintDataAndBeep();
    }
    ```