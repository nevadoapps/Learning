# Object Oriented Programming with C# - Part 03

Table of contents:

## Collections
- System.Collections Classes
- System.Collections.Generic Classes
- Key Interfaces supported by Classes of System.Collections.Generic
- Working with List\<T>
- Search and sort lists
- Implementing a Collection of Key/Value Pairs
- Using LINQ to Access Collection
- Working with the Stack<T> Class
- Working with the Queue\<T> Class

## Collections

Arrays are most useful for creating and working with a fixed number of strongly typed objects.

Collections provide a more flexible way to work with groups of objects. Unlike arrays, the group of objects you work with can grow or shrink dynamically as the needs of the application chage. 

A collection is a class, so you must declare an instance of the class before you can add elements to that collection.

- **System.Collections Classes**

The classes in the System.Collections namespace do not store elements as specifically typed objects, but as objects of type **Object**.

Whenever, possible, you should use the generic collections in the **System.Collections.Generic** namespace or the **Sytem.Collections.Concurrent** namespace instead of the legacy types in the **System.Collections**.

| Class | Description |
| -- | -- |
| ArrayList | Represents an array of objects whose size is dynamically increased or decreased. |
| Hashtable | Represents a collection of key/value pairs that are organized based on the hash code of the key |
| Queue | Represents a first in, first out (FIFO) collection of objects. |
| Stack | Represents a last in, first our (LIFO) collection of objects. |

Example: (ArrayList)
```csharp
static void Main(string[] args)
{
    Arraylist strArray = new ArrayList();
    strArray.AddRange(new string[] { "First", "Second", "Third" })

    Console.WriteLine($"This collection has {strArray.Count} items");

    strArray.Add("Fourth");

    foreach(string s in strArray)
    {
        Console.WriteLine($"String in Array is : {s}");
    }
}
```
**Notes:**
- Using the **System.Collections** and **System.Collections.Specialized** classes can result in some poorly performing code.
- Most of the nongeneric collection classes are not type-safe because they were developed to operate on **System.Object**, and they could contain anything at all.
- Boxing and Unboxing would be needed

Boxing: can be formally defined as the process of explicitly assigning a value type to a System.Object. When you box a value, the CoreCLR allocated a new object on the heap and copies the value type's value into that instance.

Unboxing: is the process of converting the value held in the object reference back into a corresponding value type on the stack.

Example:
```csharp
static void SimpleBoxUnboxOperation()
{
    int myInt = 25;
    object boxedInt = myInt;

    int unboxedInt = int(boxedInt);
}
```


- **System.Collections.Generic Classes**

You can create a generic collection by using one of the classes in the **System.Collections.Generic** namespace. A generic collection is useful when every item in the collection has the same data type. A generic collection enforces strong typing by allowing only the desired data type to be added.

| Class | Description |
| -- | -- |
| Dictionary<TKey, TValue> | Represents a collection of key/value pairs that are organized based on the key. |
| List\<T> | Represents a list of objects that can be accessed by index. Provides methods to search, sort, and modift lists. |
| Queue\<T> | Represents a first in, first out (FIFO) collection of objects. |
| SortedList\<T> | Represents a collection of key/value pairs that are sorted by key based on the associated IComparer\<T> implementation. |
| Stack\<T> | Represents a last in, first out (LIFO) collection of objects. |

When you use generic collection classes, you rectify all the previous issues, including boxing/unboxing penalties, and a lack of type safety.

- **Key Interfaces supported by Classes of System.Collections.Generic**

| System.Collections.Generic.Interface | Meaning in Life |
| -- | -- |
| ICollection\<T> | Defines general characteristics for all generic collection types. |
| IComparer\<T> | Defines a way to compare to objects |
| IDictionary<TKey, TValue> | Allows a generic collection object to represent its content using key-value pairs. |
| IEnumerable\<T>/IAsyncEnumerable\<T> | Returns the IEnumerator\<T> interface for a given object. IAsyncEnumerator (8.0) |
| IEnumerator\<T> | Eneables foreach-style iteratino over a generic collection. |
| IList\<T> | Provides behavior to add, remove, and index items in a sequential list of objects. |
| ISet\<T> | Provides the bas interface for abstraction of sets. |


**Collection Initialization Syntaxes:**

Example:
```csharp
List<Point> myPoints = new List<Point>() {
    new Point { X = 2, Y = 2},
    new Point { X = 12, Y = 12},
    new Point { X = 22, Y = 22}
};
```


- **Working with List\<T>**

    - **Modifying list contents**

    The collection you created uses the **List\<T>** type. This type stores sequences of elements. You specify the type of the elements between the angle brackets.

    One important aspect of this **List\<T>** type is that it can grow or shrink, enabling you to add or remove elements.

    Example:
    ```csharp
    public class Names
    {
        public string Name { get; set; }
    }

    List<Names> names = new(); // List<Names> names = new List<Names>();
    names.Add("Troy");
    names.Add("Jill");
    names.Add("Jasmine");
    names.Remove("Jill");

    foreach(var name in names)
    {
        Console.WriteLine($"Current item in names list is {name}");
    }

    Console.WriteLine($"Number of items in Names list: {names.Count}");
    ```
    - **Search and sort lists**

    To find elements in largar collections, you need to search the list for different items. The **IndexOf** method searches for an item and returns the index of the item. If the item is not in the list, **IndexOf** return **-1.

    Example:
    ```csharp
    var index = names.IndexOf("Jill");

    if (index == -1)
        Console.WriteLine($"Item does not exist in the list");
    else
        Console.WriteLine($"The name {names[index]} is at index {index}");
    ```

    Example:
    ```csharp
    public class Person
    {
        public string Name { get; set; }
        public string Surname { get; set; }
        public override bool Equals(Object? obj)
        {
            if ((obj == null) || ! this.GetType().Equals(obj.GetType()))
                return false;
            else
            {
                Person p = obj as Person;

                return (p.Name == this.Name && p.Surname == this.Surname);
            }
        }

        public bool Equals(Person? person)
        {
            if ((person == null) || !this.GetType().Equals(person.GetType()))
                return false;
            else
            {
                return (person.Name == this.Name && person.Surname == this.Surname);
            }
        }

        public override int GetHashCode()
        {
            return (Name, Surname).GetHashCode();
        }
        public override string ToString()
        {
            return $"{Name} {Surname}";
        }
    }

    static class Program
    {
        static void Main(string[] args)
        {
            List<Person> person = new()
            {
                new Person { Name = "Evrim", Surname = "Yetginer" },
                new Person { Name = "Seha", Surname = "Yetginer" },
                new Person { Name = "Neva", Surname = "Yetginer"}
            };

            Dictionary<string, List<Person>> familyDictionary = new Dictionary<string, List<Person>>()
            {
                { "Yetginer", person }
            };

            if (familyDictionary.TryGetValue("Yetginer", out List<Person> familyList))
            {
                Console.WriteLine($"The number of items in family list: {familyList.Count}");

                foreach(var familyMember in familyList)
                {
                    Console.WriteLine($"Name: {familyMember.Name}, Surname: {familyMember.Surname}");
                }

                //IndexOf
                var index = familyList.IndexOf(new Person { Name = "Neva", Surname = "Yetginer" });

                if (index == -1)
                    Console.WriteLine($"Item is not found by IndexOf");
                else
                    Console.WriteLine($"Item is found by IndexOf");

                //Find - Return T
                if (familyList.Find(x => x.Name.Contains("Neva")) == null)
                    Console.WriteLine("Item is not found by Find Method");
                else
                    Console.WriteLine("Item is found by Find Method");

                //Contains - > True/False

                if (!familyList.Contains(new Person { Name = "Neva", Surname = "Yetginer" }))
                    Console.WriteLine("Item is not found by Contains Method");
                else
                    Console.WriteLine("Item is found by Contains Method");

                //Equals override in Person not working... But for others yes
                if (!familyList.Equals(new Person { Name = "Neva", Surname = "Yetginer" }))
                    Console.WriteLine($"Item is not found by Equals Method");
                else
                    Console.WriteLine($"Item is found by Equals Method");
            }
        }
    }
    ```

- **Implementing a Collection of Key/Value Pairs**

The **Dictionary<TKey, TValue>** generic collection enables you to access elements in a collection by using the key of each element. Each addition to the dictionary consists of a value and its associated key. Retrieving a value by using its key fast because the **Dictionary** class is implemented as a hash table.

Example:
```csharp
private static void IterateThruDictionary()
{
    Dictionary<string, Element> elements = BuildDictionary();

    foreach (KeyValuePair<string, Element> kvp in elements)
    {
        Element theElement = kvp.Value;

        Console.WriteLine("key: " + kvp.Key);
        Console.WriteLine("values: " + theElement.Symbol + " " +
            theElement.Name + " " + theElement.AtomicNumber);
    }
}

private static Dictionary<string, Element> BuildDictionary()
{
    var elements = new Dictionary<string, Element>();

    AddToDictionary(elements, "K", "Potassium", 19);
    AddToDictionary(elements, "Ca", "Calcium", 20);
    AddToDictionary(elements, "Sc", "Scandium", 21);
    AddToDictionary(elements, "Ti", "Titanium", 22);

    return elements;
}

private static void AddToDictionary(Dictionary<string, Element> elements,
    string symbol, string name, int atomicNumber)
{
    Element theElement = new Element();

    theElement.Symbol = symbol;
    theElement.Name = name;
    theElement.AtomicNumber = atomicNumber;

    elements.Add(key: theElement.Symbol, value: theElement);
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```
To instead of use a collection initializer to build the **Dictionary** collection, you can replace the **BuildDictionary** and **AddToDictionary** methods as follow:

```csharp
public static Dictionary<string, Element> BuildDictionary()
{
    return new Dictionary<string, Element> {
        { "K", new Element() { Symbol = "K", Name = "Potassium", AtomicNumber = 19 } },
        { "Ca", new Element() { Symbol = "Ca", Name = "Calcium", AtomicNumber = 20 } },
        { "Sc", new Element() { Symbol = "Sc", Name = "Scandium", AtomicNumber = 21 } },        
        { "Ti", new Element() { Symbol = "Ti", Name = "Titanium", AtomicNumber = 22 } }
    };
}
```

The following example uses the **ContainsKey** method and the **Item[]** of **Dictionary** to quickly find an item by key. The **item** property enables you to access an item in the **elements** collection by using the **elements[symbol]** in C#.

```csharp
private static void FindInDictionary(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    if (elements.ContainsKey(symbol) == false)
    {
        Console.WriteLine(symbol + " not found");
    }
    else
    {
        Element theElement = elements[symbol];
        Console.WriteLine("found: " + theElement.Name);
    }
}
```

The following example instead uses the TryGetValue method quickly find an item by key.

```csharp
private static void FindInDictionary2(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    Element theElement = null;
    if (elements.TryGetValue(symbol, out theElement) == false)
        Console.WriteLine(symbol + " not found");
    else
        Console.WriteLine("found: " + theElement.Name);
}
```
- **Using LINQ to Access a Collection**

Example:
```csharp
private static void ShowLINQ()
{
    List<Element> elements = BuildList();

    // LINQ Query.
    var subset = from theElement in elements
                 where theElement.AtomicNumber < 22
                 orderby theElement.Name
                 select theElement;

    foreach (Element theElement in subset)
    {
        Console.WriteLine(theElement.Name + " " + theElement.AtomicNumber);
    }

    // Output:
    //  Calcium 20
    //  Potassium 19
    //  Scandium 21
}

private static List<Element> BuildList()
{
    return new List<Element>
    {
        { new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        { new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        { new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        { new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

- **Working with the Stack\<T> Class**

The **Stack\<T>** class represents a collection that maintains items using a last in, first our manner. 

Stacks are useful when you need a temporary storage information; use **System.Collections.Generic.Stack\<T>** if you need to access information in reverse order compared to **Queue\<T>**.

Use the **System.Collections.Concurrent.ConcurrentStack\<T>** and **System.Collections.Concurrent.ConCurrentQueue\<T>** types when you need to access the collection from multiple threads concurrently.

A common use for **System.Collections.Generic.Stack<T>** is to preserve variable states during the calls to other procedures.

Three main operations can be performed on a **System.Collections.Generic.Stack\<T>** and its elements:

- **Push** inserts an element at the top of the **Stack\<T>**
- **Pop** removes an element from the top of the **Stack\<T>**
- **Peek** returns an element that is at the top of the **Stack\<T>** but does not remove it from the **Stack\<T>**.

The capacity of a Stack\<T> is the number of elements the Stack\<T> can hold. As elements are added to a Stack\<T>, the capacity is automatically increased as required by reallocating the internal array.

If **Count** is less than the capacity of the stack, **Push** is an O(1) operation. If the capacity needs to be increased to accomodate the new element, **Push** becomes an O(__n__) operation, where **n** is **Count**. **Pop** is an O(1) operation.

Example:
```csharp
static void UseGenericStack()
{
    Stack<Person> stackOfPeople = new Stack<Person>();

    stackOfPeople.Push(new Person { Name = "Troy", Surname = "Blitzer" });
    stackOfPeople.Push(new Person { Name = "Jill", Surname = "Morison" });
    stackOfPeople.Push(new Person { Name = "Tracy", Surname = "Harrison" });

    Console.WriteLine($"First person is {stackOfPeople.Peek()}");
    Console.WriteLine($"Popped off {stackOfPeople.Pop()}");

    Console.WriteLine($"First person is {stackOfPeople.Peek()}");
    Console.WriteLine($"Popped off {stackOfPeople.Pop()}");

    Console.WriteLine($"First person is {stackOfPeople.Peek()}");
    Console.WriteLine($"Popped off {stackOfPeople.Pop()}");

    try
    {
        Console.WriteLine($"First person is {stackOfPeople.Peek()}");
        Console.WriteLine($"Popped off {stackOfPeople.Pop()}");
    }
    catch (Exception ex)     
    {
        Console.WriteLine($"An error occurred: {ex.Message}");
    }
}
```

- **Working with the Queue\<T>**

Queues are containers that ensure items are accessed in a first in, first our manner. 

| Select Member of Queue\<T> | Meaning of Life |
| -- | -- |
| Dequeue() | Removes and returns the object at the beginning of Queue\<T> | 
| Enqueue() | Add an object to the end of the Queue\<T> |
| Peek() | Returns the object at the beginning of the Queue\<T> without removing it |

The following code example demonstrates several methods of the Queue\<T> generic class. The code example creates a queue of strings with default capacity and uses the Enqueue method to queue five strings. The elements of the queue are enumerated, which does not change the state of the queue. The Dequeue method is used to dequeue the first string. The Peek method is used to look at the next item in the queue, and then the Dequeue method is used to dequeue it.

The ToArray method is used to create an array and copy the queue elements to it, then the array is passed to the Queue\<T> constructor that takes IEnumerable\<T>, creating a copy of the queue. The elements of the copy are displayed.

An array twice the size of the queue is created, and the CopyTo method is used to copy the array elements beginning at the middle of the array. The Queue\<T> constructor is used again to create a second copy of the queue containing three null elements at the beginning.

The Contains method is used to show that the string "four" is in the first copy of the queue, after which the Clear method clears the copy and the Count property shows that the queue is empty.

Example:
```csharp
using System;
using System.Collections.Generic;

class Example
{
    public static void Main()
    {
        Queue<string> numbers = new Queue<string>();
        numbers.Enqueue("one");
        numbers.Enqueue("two");
        numbers.Enqueue("three");
        numbers.Enqueue("four");
        numbers.Enqueue("five");

        // A queue can be enumerated without disturbing its contents.
        foreach( string number in numbers )
        {
            Console.WriteLine(number);
        }

        Console.WriteLine("\nDequeuing '{0}'", numbers.Dequeue());
        Console.WriteLine("Peek at next item to dequeue: {0}",
            numbers.Peek());
        Console.WriteLine("Dequeuing '{0}'", numbers.Dequeue());

        // Create a copy of the queue, using the ToArray method and the
        // constructor that accepts an IEnumerable<T>.
        Queue<string> queueCopy = new Queue<string>(numbers.ToArray());

        Console.WriteLine("\nContents of the first copy:");
        foreach( string number in queueCopy )
        {
            Console.WriteLine(number);
        }

        // Create an array twice the size of the queue and copy the
        // elements of the queue, starting at the middle of the
        // array.
        string[] array2 = new string[numbers.Count * 2];
        numbers.CopyTo(array2, numbers.Count);

        // Create a second queue, using the constructor that accepts an
        // IEnumerable(Of T).
        Queue<string> queueCopy2 = new Queue<string>(array2);

        Console.WriteLine("\nContents of the second copy, with duplicates and nulls:");
        foreach( string number in queueCopy2 )
        {
            Console.WriteLine(number);
        }

        Console.WriteLine("\nqueueCopy.Contains(\"four\") = {0}",
            queueCopy.Contains("four"));

        Console.WriteLine("\nqueueCopy.Clear()");
        queueCopy.Clear();
        Console.WriteLine("\nqueueCopy.Count = {0}", queueCopy.Count);
    }
}

/* This code example produces the following output:

one
two
three
four
five

Dequeuing 'one'
Peek at next item to dequeue: two
Dequeuing 'two'

Contents of the first copy:
three
four
five

Contents of the second copy, with duplicates and nulls:



three
four
five

queueCopy.Contains("four") = True

queueCopy.Clear()

queueCopy.Count = 0
 */
```